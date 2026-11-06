# 02｜.NET Testcontainers 整合測試驗收手冊

更新日期：2026-11-06

## 1. 驗收目標

確認 Windows 11 原生 `dotnet test`、Visual Studio Test Explorer 與 Rider 測試執行器可以透過 Rancher Desktop 的 **Moby / dockerd** 建立 Linux Containers、使用動態映射 Port、連線 Redis/SQL Server，並正常清理資源。重點是 **Testcontainers for .NET**，不是 Testcontainers Java。

本文件使用 .NET 8 xUnit 作為可重現 POC。使用 `Testcontainers.MsSql 4.13.0` 的既有專案可以保持該版本，驗證完成後再考慮升級。NuGet API 可能因版本不同而有差異，以下程式碼應以目標版本實際編譯為驗收準則。

## 2. 驗收前置條件

依第一份文件確認：

```powershell
docker version
docker info
docker compose version
docker run --rm hello-world
```

Rancher Desktop 必須選擇 **moby (dockerd)**，Kubernetes 停用。確認 Docker Desktop 未在執行。

### 2.1 設定當前測試工作階段

```powershell
$env:DOCKER_HOST = 'npipe:////./pipe/docker_engine'
Remove-Item Env:DOCKER_CONTEXT -ErrorAction SilentlyContinue
Remove-Item Env:TESTCONTAINERS_HOST_OVERRIDE -ErrorAction SilentlyContinue
Remove-Item Env:TESTCONTAINERS_DOCKER_SOCKET_OVERRIDE -ErrorAction SilentlyContinue
Remove-Item Env:TESTCONTAINERS_RYUK_DISABLED -ErrorAction SilentlyContinue
```

不建議永久寫入所有系統環境變數；先以獨立 PowerShell 視窗執行 POC。不要停用 Ryuk 來掩蓋錯誤。

## 3. 建立測試專案

```powershell
mkdir RancherDesktopTestcontainersPoc
cd RancherDesktopTestcontainersPoc
dotnet new xunit -n RancherDesktopTestcontainersPoc.Tests -f net8.0
cd RancherDesktopTestcontainersPoc.Tests
dotnet add package Testcontainers --version 4.13.0
dotnet add package Testcontainers.MsSql --version 4.13.0
dotnet add package Microsoft.Data.SqlClient
```

**版本注意**：上述版本針對既有 4.13.0 測試環境；若套件版本無法解析或 NuGet 版本資訊與預期不同，先檢查來源與 `dotnet list package`，不要直接使用未經確認的版本替代。若 `Testcontainers.MsSql` 已透過相依性帶入 `Testcontainers`，顯式參考仍可方便固定版本。

## 4. 測試 A：Docker API 與容器生命週期

建立 `DockerRuntimeTests.cs`：

```csharp
using DotNet.Testcontainers.Builders;
using Xunit;

public sealed class DockerRuntimeTests
{
    [Fact]
    public async Task Should_start_and_stop_linux_container()
    {
        await using var container = new ContainerBuilder()
            .WithImage("alpine:3.20")
            .WithCommand("sleep", "60")
            .Build();

        await container.StartAsync();

        Assert.True(container.State ==
            DotNet.Testcontainers.Containers.TestcontainersStates.Running);
    }
}
```

執行：

```powershell
dotnet test --filter FullyQualifiedName~DockerRuntimeTests --logger 'console;verbosity=detailed'
```

**通過條件**：可拉取 Image、建立/啟動容器、測試通過、Dispose 後容器清理。若 `ContainerBuilder` 命名空間因實際版本不同而變動，依 NuGet 官方 API 調整；勿把編譯錯誤誤判為 Docker 連線錯誤。

## 5. 測試 B：Redis 動態 Port Mapping

建立 `RedisPortTests.cs`：

```csharp
using System.Net.Sockets;
using DotNet.Testcontainers.Builders;
using Xunit;

public sealed class RedisPortTests
{
    [Fact]
    public async Task Windows_should_reach_redis_mapped_port()
    {
        await using var container = new ContainerBuilder()
            .WithImage("redis:7-alpine")
            .WithPortBinding(6379, true)
            .WithWaitStrategy(Wait.ForUnixContainer()
                .UntilInternalTcpPortIsAvailable(6379))
            .Build();

        await container.StartAsync();

        var host = container.Hostname;
        var port = container.GetMappedPublicPort(6379);
        using var client = new TcpClient();
        await client.ConnectAsync(host, port);

        Assert.True(client.Connected);
    }
}
```

```powershell
dotnet test --filter FullyQualifiedName~RedisPortTests --logger 'console;verbosity=detailed'
```

**通過條件**：Port 是 Docker 動態配置，不需要預留固定的 6379；Windows 能連線到 `container.Hostname` 和 `GetMappedPublicPort(6379)` 回傳的端點。若 Testcontainers 回傳非預期 Hostname，先檢查環境變數、Rancher Desktop Port Forwarding 與 Windows 防火牆。

## 6. 測試 C：SQL Server 實際連線

建立 `MsSqlConnectionTests.cs`：

```csharp
using System.Data;
using Microsoft.Data.SqlClient;
using Testcontainers.MsSql;
using Xunit;

public sealed class MsSqlConnectionTests
{
    [Fact]
    public async Task Sql_server_should_accept_connections()
    {
        await using var container = new MsSqlBuilder().Build();
        await container.StartAsync();

        await using var connection =
            new SqlConnection(container.GetConnectionString());

        await connection.OpenAsync();
        Assert.Equal(ConnectionState.Open, connection.State);
    }
}
```

```powershell
dotnet test --filter FullyQualifiedName~MsSqlConnectionTests --logger 'console;verbosity=detailed'
```

**通過條件**：`MsSqlBuilder` 啟動 SQL Server Linux Container；Windows 的 `SqlConnection.OpenAsync()` 成功。第一次下載 SQL Server Image 可能較慢。若連線失敗，先看 `docker logs` 與 Testcontainers 診斷，不要僅憑容器 Running 判定資料庫已就緒。

### 6.1 SQL Server 啟動與重試

- `StartAsync()` 成功代表 Testcontainers 指定的啟動等待策略已完成，不等於所有應用層操作一定成功。
- 若專案要驗證資料庫真正可用，使用 `OpenAsync()` 或 `SELECT 1` 作為就緒條件。
- 需要重試時使用**有截止時間的非同步重試**，只捕捉可重試的連線例外；保留最後一次錯誤，並將逾時原因輸出至測試記錄。
- 不建議對所有例外無限重試，也不建議將 SQLCMD 命令作為唯一判斷依據。

## 7. Ryuk 資源清理驗證

Testcontainers 預設使用 Ryuk 清理測試建立的資源。先確認未設定 `TESTCONTAINERS_RYUK_DISABLED=true`：

```powershell
Get-ChildItem Env:TESTCONTAINERS* | Format-Table -AutoSize
docker ps -a
```

測試執行期間可在另一個 PowerShell 觀察：

```powershell
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}'
```

測試正常結束後，確認測試容器已清理。若測試遭強制終止，Ryuk 可能稍後清理，需觀察而非立即判斷失敗。**不要在共享 Docker Host 上執行 `docker system prune -a --volumes` 作為常規測試清理**。

## 8. Visual Studio 2022 驗收

在**設定好環境變數的 PowerShell** 啟動 Visual Studio；依實際 Edition 修改路徑：

```powershell
$env:DOCKER_HOST = 'npipe:////./pipe/docker_engine'
Remove-Item Env:TESTCONTAINERS_HOST_OVERRIDE -ErrorAction SilentlyContinue
& 'C:\Program Files\Microsoft Visual Studio\2022\Professional\Common7\IDE\devenv.exe'
```

於 Test Explorer 依序執行三組測試，確認每組均通過。若 Visual Studio 已在執行，請完全關閉所有 `devenv.exe` 再從新環境啟動，避免沿用舊程序環境變數。確認測試專案的 Target Framework 與已安裝 .NET SDK 一致。

## 9. Rider 驗收

同樣在已設定環境變數的 PowerShell 啟動 Rider（路徑依安裝方式調整）：

```powershell
$env:DOCKER_HOST = 'npipe:////./pipe/docker_engine'
Remove-Item Env:TESTCONTAINERS_HOST_OVERRIDE -ErrorAction SilentlyContinue
& 'C:\Program Files\JetBrains\JetBrains Rider\bin\rider64.exe'
```

於 Unit Tests 執行三組測試。Rider 的 Docker Tool Window 連線設定與 xUnit 測試程序的 Docker Endpoint 是兩回事；兩者都應各自驗證。

## 10. 驗收紀錄表

| ID | 測試項目 | 驗收標準 | 結果 |
|---|---|---|---|
| TC-01 | `docker version` | Client/Server 可用 | 待驗證 |
| TC-02 | Alpine 容器 | 建立、啟動、清理成功 | 待驗證 |
| TC-03 | Redis 動態 Port | Windows TCP 連線成功 | 待驗證 |
| TC-04 | SQL Server | `OpenAsync()` 成功 | 待驗證 |
| TC-05 | Ryuk | 正常清理測試資源 | 待驗證 |
| TC-06 | Visual Studio | Test Explorer 全部通過 | 待驗證 |
| TC-07 | Rider | Unit Tests 全部通過 | 待驗證 |
| TC-08 | 重複測試 | 連續 3 次通過、無殘留 | 待驗證 |

## 11. 版本相容性與重要聲明

Rancher Desktop 官方文件明確描述 **Windows Native + Named Pipe** 的 Testcontainers 設定，但官方示例以 Java 為主。Testcontainers for .NET 官方亦說明其他 Docker Runtime 不一定包含在主要測試矩陣中。因此本文件是**建置與驗收規範**，不是已在你的電腦上實際跑過的成功報告。

## 官方參考

- https://docs.rancherdesktop.io/1.24/how-to-guides/using-testcontainers/
- https://dotnet.testcontainers.org/custom_configuration/
- https://dotnet.testcontainers.org/api/create_docker_container/
- https://dotnet.testcontainers.org/
