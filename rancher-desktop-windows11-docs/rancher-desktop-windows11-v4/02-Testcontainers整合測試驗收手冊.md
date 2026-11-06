# 02｜.NET Testcontainers 整合測試驗收手冊（V4）

## 1. 測試前提

Windows 11 原生執行 Visual Studio 2022 / Rider / `dotnet test`；WSL2 + Rancher Desktop（Moby）提供 Linux Docker API。**不需 Ubuntu，也不需手動安裝 Docker Engine。**

驗收包含：Docker API、容器生命週期、動態 Port、Windows TCP 連線、MsSql、Ryuk 清理、兩套 IDE。先完成文件 01 的 Moby 初始化、WSL 發行版搬移至 D 槽、CLI 與 Redis 手動驗證。

## 2. 建立獨立 POC

```powershell
dotnet new xunit -n RancherDesktop.Testcontainers.Poc
cd RancherDesktop.Testcontainers.Poc
dotnet add package Testcontainers --version 4.13.0
dotnet add package Testcontainers.Redis --version 4.13.0
dotnet add package Testcontainers.MsSql --version 4.13.0
dotnet add package Microsoft.Data.SqlClient
```

> 上述 `4.13.0` 是與既有 MsSql 測試版本對齊的範例；部署前確認 NuGet 套件可取得且與目標 .NET 版本相容。不要在團隊環境無驗證地自動升級。

## 3. 連線設定

優先不設額外環境變數，讓 Testcontainers 自動偵測。若偵測失敗，在同一個 PowerShell 工作階段測試：

```powershell
$env:DOCKER_HOST = 'npipe:////./pipe/docker_engine'
Remove-Item Env:DOCKER_CONTEXT -ErrorAction SilentlyContinue
Remove-Item Env:TESTCONTAINERS_HOST_OVERRIDE -ErrorAction SilentlyContinue

docker version
dotnet test
```

`docker version` 成功**不代表** Testcontainers 已通過；兩者需分開驗證。不要為排除故障直接關閉 Ryuk。

## 4. 基本容器生命週期測試

建立 `ContainerLifecycleTests.cs`：

```csharp
using DotNet.Testcontainers.Builders;
using Xunit;

public sealed class ContainerLifecycleTests
{
    [Fact]
    public async Task Should_start_a_linux_container()
    {
        await using var container = new ContainerBuilder()
            .WithImage("alpine:3.20")
            .WithCommand("sleep", "120")
            .Build();

        await container.StartAsync();
        Assert.True(container.State == DotNet.Testcontainers.Containers.TestcontainersStates.Running);
    }
}
```

執行 `dotnet test`。驗證完成後檢查容器是否清理。

## 5. Redis 動態 Port 與 TCP 驗證

建立 `RedisConnectivityTests.cs`：

```csharp
using System.Net.Sockets;
using Testcontainers.Redis;
using Xunit;

public sealed class RedisConnectivityTests
{
    [Fact]
    public async Task Should_connect_to_redis_mapped_port()
    {
        await using var container = new RedisBuilder()
            .WithImage("redis:7-alpine")
            .Build();

        await container.StartAsync();
        var host = container.Hostname;
        var port = container.GetMappedPublicPort(6379);
        using var tcp = new TcpClient();
        await tcp.ConnectAsync(host, port);
        Assert.True(tcp.Connected);
    }
}
```

這是 Windows 端真正能連到 Linux Container 的驗收，不可只測 `docker ps`。

## 6. SQL Server 實際連線

建立 `MsSqlConnectivityTests.cs`：

```csharp
using Microsoft.Data.SqlClient;
using Testcontainers.MsSql;
using Xunit;

public sealed class MsSqlConnectivityTests
{
    [Fact]
    public async Task Should_open_sql_connection()
    {
        await using var container = new MsSqlBuilder().Build();
        await container.StartAsync();
        await using var connection = new SqlConnection(container.GetConnectionString());
        await connection.OpenAsync();
        Assert.Equal(System.Data.ConnectionState.Open, connection.State);
    }
}
```

SQL Server 啟動比 Redis 慢；如有不穩定，記錄映像版本、容器 Log、可用 RAM 與連線錯誤。不要只用 Port 開放取代 SQL 實際 `OpenAsync()`。

## 7. Ryuk 驗收

Ryuk 負責測試資源清理。確認測試結束後建立的測試容器及相關資源按預期移除；如果 Ryuk 啟動失敗，檢查 Docker API、Socket 權限、容器網路與公司防護軟體。**不將 `TESTCONTAINERS_RYUK_DISABLED=true` 當作標準設定。**

## 8. IDE 驗收

1. 在 PowerShell 執行 `dotnet test`，記錄結果。
2. 從相同環境變數的 PowerShell 啟動 Visual Studio，再由 Test Explorer 執行。
3. 同樣驗證 Rider Test Runner；Rider 的 Docker UI 設定與測試程序環境變數是兩回事。
4. 驗證單一、重複、並行測試；並行執行時避免固定宿主 Port。
5. 對 `MsSqlBuilder`、Redis、RabbitMQ、PostgreSQL、MongoDB 等實際使用的模組逐一做 POC。

## 9. 驗收紀錄

| 項目 | 預期 | 結果 |
|---|---|---|
| Windows Docker API | 可建立 Linux 容器 | 待測 |
| Redis 動態 Port | Windows TCP 連線成功 | 待測 |
| SQL Server | `OpenAsync()` 成功 | 待測 |
| Ryuk | 測試資源正常清理 | 待測 |
| Visual Studio | Test Explorer 全部通過 | 待測 |
| Rider | Test Runner 全部通過 | 待測 |
| D 槽資料位置 | 已搬移的 Rancher Desktop WSL BasePath 在 D 槽且重啟後持久化正常 | 待驗證 |

> 這份文件是可執行 POC 的驗收規格，不宣稱已在使用者電腦實測通過。
