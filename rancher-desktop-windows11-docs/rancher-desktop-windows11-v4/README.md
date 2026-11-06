# Windows 11：Rancher Desktop + .NET Testcontainers（V4）

> 修訂日期：2026-11-06。**本版取代 V3；不安裝 Ubuntu，新增 Rancher Desktop Moby 初始化後搬移 WSL 發行版至 D 槽的完整程序。**

## 先讀：架構與授權

- **只安裝 WSL2 平台，不必安裝 Ubuntu。** Rancher Desktop 在 Windows 使用自己的 WSL 發行版執行 Moby/dockerd。
- **Rancher Desktop 是 Apache-2.0 開源軟體，允許公司商業使用，沒有 Docker Desktop 的大型企業付費門檻。** 仍須遵守第三方元件、映像檔授權及公司 IT 政策。
- **Docker Desktop 是另一項產品。** Docker Desktop 的企業商業使用限制不可直接套用到 Rancher Desktop。
- **D 槽安裝要求：** Rancher Desktop 初始化 Moby 後，退出應用程式並停止 WSL，使用 `wsl --manage <distro> --move <location>` 將實際存在的 Rancher Desktop WSL 發行版搬到 `D:\docker\wsl\`。依版本可能有 `rancher-desktop` 與 `rancher-desktop-data`，或只有前者；**不能假設後者一定存在**。操作前先確認 WSL 支援此命令並備份重要資料。
- **搬移後必須驗證：** WSL 登錄 BasePath、D 槽 VHDX、Docker Image 重啟持久化、Testcontainers for .NET。Factory Reset 後須重新檢查。

## 架構

```text
Windows 11
├─ Visual Studio 2022 / Rider / dotnet test (Windows 原生)
│  └─ xUnit + Testcontainers for .NET
│     ├─ Docker API → Windows Named Pipe → Rancher Desktop
│     └─ TCP → localhost:動態映射埠 → Linux 容器
├─ Rancher Desktop (Windows GUI、docker CLI、Compose)
└─ WSL2 平台（不需 Ubuntu）
   └─ Rancher Desktop 管理的 Linux WSL 環境
      ├─ Moby / dockerd / containerd
      ├─ Linux Containers（SQL Server、Redis 等）
      └─ Images / Volumes / 持久化資料（搬移後應位於 D 槽並驗證）
```

## 文件導覽

1. [01-環境安裝與設定手冊.md](01-環境安裝與設定手冊.md)：架構、授權、WSL2、Rancher Desktop、Moby WSL 搬移至 D 槽與 GUI。
2. [02-Testcontainers整合測試驗收手冊.md](02-Testcontainers整合測試驗收手冊.md)：Windows 原生 xUnit、Redis、MsSql、Ryuk、IDE 驗收。
3. [03-團隊部署標準與故障排除.md](03-團隊部署標準與故障排除.md)：授權審核、版本、備份、故障排除、驗收。

## 官方依據

- Rancher Desktop 授權：https://github.com/rancher-sandbox/rancher-desktop/blob/main/LICENSE
- Rancher Desktop 架構：https://docs.rancherdesktop.io/references/architecture/
- Rancher Desktop Windows 安裝：https://docs.rancherdesktop.io/getting-started/installation/
- Rancher Desktop FAQ：https://docs.rancherdesktop.io/faq/
- Docker Desktop 訂閱條款：https://www.docker.com/legal/docker-subscription-service-agreement/
- Testcontainers for .NET：https://dotnet.testcontainers.org/

> 參考資料以 2026-11-06 查詢為準；部署時仍應確認當時版本及公司內部審核要求。

- Rancher Desktop WSL 搬移討論：https://github.com/rancher-sandbox/rancher-desktop/discussions/6296
- Microsoft WSL 指令：https://learn.microsoft.com/en-us/windows/wsl/basic-commands
