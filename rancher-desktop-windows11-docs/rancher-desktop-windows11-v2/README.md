# Windows 11 Rancher Desktop + .NET Testcontainers 文件集

更新日期：2026-11-06

本文件集用於 **Windows 11 原生 Visual Studio 2022 / Rider + .NET 8/9 + xUnit + Testcontainers for .NET**，只執行 Linux Containers，不安裝 Docker Desktop。

## 文件導覽

1. [01-環境安裝與設定手冊.md](01-環境安裝與設定手冊.md)：WSL2、Rancher Desktop、Moby、Docker CLI、Compose、GUI。
2. [02-Testcontainers整合測試驗收手冊.md](02-Testcontainers整合測試驗收手冊.md)：xUnit、Docker API、Redis、SQL Server、Ryuk、IDE 驗收。
3. [03-團隊部署標準與故障排除.md](03-團隊部署標準與故障排除.md)：標準化、環境檢查、資源、升級、排錯。

## 已確定架構

```text
Windows 11
  ├─ Visual Studio 2022 / Rider / dotnet test
  ├─ Testcontainers for .NET → Docker API via Windows Named Pipe
  ├─ Docker CLI + Docker Compose
  └─ Rancher Desktop GUI
       └─ WSL2 / Moby (dockerd)
            ├─ SQL Server
            ├─ PostgreSQL
            ├─ Redis
            ├─ RabbitMQ
            ├─ MongoDB
            └─ Kafka
```

## D 槽安裝要求（2026-11-06 修訂）

- Ubuntu 24.04 WSL2 指定安裝於 `D:\docker\wsl\ubuntu-24.04`，優先使用新版 WSL `wsl --install --location`；不支援時採用 `wsl --import`。
- **Ubuntu 與 Rancher Desktop 的 WSL 發行版是不同的**。Ubuntu 安裝於 D 槽不代表 Docker Images、Volumes 也已移至 D 槽。
- Rancher Desktop 自有資料磁碟是否能遷移至 D 槽，須依安裝版本確認官方支援方式，並實際驗收磁碟路徑；不得直接手動搬動其 VHDX。
- 詳細命令、驗證方式及既有 Ubuntu 遷移警告請見文件 01 第 3 節。

## 核心原則

- Rancher Desktop 使用 **moby (dockerd)**，不要選 containerd。
- 停用 Kubernetes（本方案不需要）。
- Windows 原生 Testcontainers 使用 `DOCKER_HOST=npipe:////./pipe/docker_engine`；不要設定 `TESTCONTAINERS_HOST_OVERRIDE`。
- 不額外安裝 Ubuntu Docker Engine、OpenSSH、Docker API Proxy 或 Docker Desktop。
- 先 POC 再推廣：Rancher Desktop 並非 Testcontainers for .NET 官方主要 CI 驗證的 Windows Runtime，須驗證實際套件版本。

## 官方參考

- https://docs.rancherdesktop.io/getting-started/installation/
- https://docs.rancherdesktop.io/1.24/how-to-guides/using-testcontainers/
- https://dotnet.testcontainers.org/custom_configuration/
- https://dotnet.testcontainers.org/
- https://learn.microsoft.com/windows/wsl/install
