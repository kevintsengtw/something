# Windows 11：Docker Desktop 移除與 Rancher Desktop 遷移前檢查手冊

> 文件版本：1.1｜適用：Windows 11、WSL2、Windows 原生 Visual Studio / Rider、Testcontainers for .NET｜目的：移除舊 Docker Desktop，避免與 Rancher Desktop + Moby 混用。

## 1. 目的與適用範圍

本文件是**獨立的遷移前置手冊**，不取代 Rancher Desktop 安裝手冊。適用於已安裝 Docker Desktop、準備改用 Rancher Desktop + Moby（dockerd）的 Windows 11 開發電腦。僅使用 Linux Containers；不需安裝 Ubuntu，也不應移除 Windows WSL2 平台。

**建議流程：** 確認目前 Runtime → 清點與備份資料 → 退出 Docker Desktop → **以系統管理員 PowerShell 執行官方解除安裝程式** → 檢查 WSL、CLI、Context、環境變數與殘留 → 重新開機 → 安裝 Rancher Desktop → 驗證 Testcontainers。

**公司環境限制：** 一般使用者無法透過 Windows「已安裝的應用程式」解除安裝軟體，但可以開啟具有系統管理員權限的 PowerShell／Windows Terminal。因此本文件**不採用 GUI 解除安裝流程**。管理員權限不代表可以繞過公司端點管理、AppLocker、WDAC 或軟體部署政策；若官方卸載程式遭政策封鎖，應交由 IT 管理員處理。

## 2. 架構與資料隔離

```text
Windows 11
├─ Visual Studio / Rider → .NET Testcontainers → Docker API
├─ Docker CLI / Compose
├─ [舊] Docker Desktop → docker-desktop (WSL2) → 舊 images/volumes
└─ [新] Rancher Desktop → rancher-desktop (WSL2) → Moby → 新 images/volumes
```

Docker Desktop 與 Rancher Desktop 管理的是**不同的容器資料庫**。解除安裝前必須備份需要的資料；Rancher Desktop 不會自動繼承 Docker Desktop 的 images、containers、volumes。`docker ps`、`docker volume ls` 等指令只查詢**當前 Docker Context 指向的 Engine**。

## 3. 解除安裝前清點（PowerShell）

先啟動 Docker Desktop，確認 Docker CLI 指向舊 Engine，再執行：

```powershell
docker context ls
docker context show
docker version
docker ps -a
docker image ls
docker volume ls
docker network ls
docker compose ls
wsl --list --verbose
where.exe docker
Get-ChildItem Env:DOCKER* -ErrorAction SilentlyContinue
Get-ChildItem Env:TESTCONTAINERS* -ErrorAction SilentlyContinue
```

如 `docker context show` 並非 `desktop-linux`，先確認實際連線目標，**不要在不明 Runtime 上備份或刪除**。保存 Compose 檔、`.env`、映像檔來源與版本、具名 Volume 名稱、Bind Mount 路徑、必要的 Registry 憑證設定。

## 4. 備份與資料保護

Docker 官方明確警告：解除安裝 Docker Desktop 會刪除其本機 containers、images、volumes 等資料。應先依資料類型備份。

- **資料庫**：優先使用 SQL Server / PostgreSQL / MongoDB 等資料庫原生備份、匯出與還原程序，並測試可還原性。
- **具名 Volume**：使用受信任的暫存容器或檔案工具匯出，注意檔案擁有者、權限與資料庫一致性；對正在寫入的資料庫，不應僅依賴檔案複製。
- **Image**：有 Registry 的優先重新拉取；本機自建且無來源者可使用 `docker save` 匯出。
- **Bind Mount**：備份 Windows 上的實際資料夾，不要誤認解除安裝會備份它們。
- **組態**：保存 Compose、環境設定與必要 Secrets 的安全備份；不要將密碼寫進團隊文件。

如需完整保留舊環境，請在執行解除安裝前停止遷移並確認備份策略。備份檔應放在不會隨 Docker Desktop 解除安裝而被清除的位置。

## 5. 使用系統管理員 PowerShell 正式解除安裝 Docker Desktop

### 5.1 前置條件

1. 先完成第 3、4 章的資料清點及備份，停止 Visual Studio／Rider 的整合測試。
2. 在 Docker Desktop 系統匣選單執行 **Quit Docker Desktop**。此動作只是停止應用程式，**不是解除安裝**。
3. 開啟 **Windows Terminal／PowerShell（以系統管理員身分執行）**；不要在一般權限的終端機直接執行卸載。
4. 確認目前終端機具有提升的管理員權限：

```powershell
$identity = [Security.Principal.WindowsIdentity]::GetCurrent()
$principal = [Security.Principal.WindowsPrincipal]::new($identity)
$principal.IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
```

預期輸出為 `True`。若為 `False`，請重新開啟提升權限的終端機。

### 5.2 確認官方卸載程式位置

Docker Desktop Windows 預設安裝路徑通常包含官方安裝／卸載程式：

```powershell
$installer = Join-Path $env:ProgramFiles 'Docker\Docker\Docker Desktop Installer.exe'
Test-Path -LiteralPath $installer
```

若為 `False`，先查詢可能的安裝位置，不要自行假設安裝路徑：

```powershell
Get-ChildItem 'C:\Program Files\Docker' -Filter 'Docker Desktop Installer.exe' -Recurse -ErrorAction SilentlyContinue |
    Select-Object FullName
```

如果公司採用自訂安裝路徑，請改用實際路徑。若找不到官方卸載程式，**不要直接刪除 `C:\Program Files\Docker`**，請由 IT 透過原本的軟體部署工具修復或解除安裝。

### 5.3 執行官方命令列解除安裝

確認路徑後，在**提升權限**的 PowerShell 執行：

```powershell
$installer = Join-Path $env:ProgramFiles 'Docker\Docker\Docker Desktop Installer.exe'
if (-not (Test-Path -LiteralPath $installer)) {
    throw "找不到 Docker Desktop 官方卸載程式：$installer"
}

$process = Start-Process -FilePath $installer `
    -ArgumentList 'uninstall' `
    -Wait -PassThru

"Docker Desktop uninstaller exit code: $($process.ExitCode)"
```

此方式呼叫 Docker Desktop 官方安裝程式的 `uninstall` 動作，**不依賴 Windows 設定中的解除安裝按鈕**。卸載程式可能仍會顯示視窗或要求互動；不要將此命令視為保證完全靜默的企業部署指令。

若退出碼非 `0`，或卸載視窗回報失敗，**先停止後續清理**，記錄錯誤並交由 IT 查明；不要以刪除服務、WSL 發行版或安裝目錄代替正式解除安裝。

### 5.4 驗證解除安裝結果

```powershell
Get-Process -Name 'Docker Desktop' -ErrorAction SilentlyContinue
Get-Service -Name 'com.docker.service' -ErrorAction SilentlyContinue
Test-Path -LiteralPath (Join-Path $env:ProgramFiles 'Docker\Docker\Docker Desktop.exe')
wsl --list --verbose
```

以上是**輔助檢查**，不能只靠單一項目判定卸載成功。服務、檔案或 WSL 發行版可能有殘留；若存在，依第 6～8 章確認是否可安全清理。完成正式解除安裝與必要檢查後，再重新啟動 Windows。

### 5.5 公司環境常見阻礙

- **無法執行卸載程式／遭組織原則封鎖：** 即使 PowerShell 已提升權限，仍可能受 AppLocker、WDAC、EDR 或集中軟體管理政策限制；請交由 IT 處理，不要嘗試繞過。
- **找不到 `Docker Desktop Installer.exe`：** 不要使用 `Remove-Item` 強行刪除程式資料夾；應由 IT 查詢原安裝來源與可用的卸載方式。
- **卸載程式顯示正在使用：** 先停止測試與 Docker Desktop，必要時由 IT 確認相關背景程序。
- **卸載後 Docker CLI 仍可執行：** 可能是另一份 CLI、PATH 或 Context 殘留，不代表 Docker Desktop 仍安裝。

官方依據：Docker Desktop Windows 解除安裝文件：
https://docs.docker.com/desktop/uninstall/

## 6. 檢查 WSL2 發行版（不可誤刪）

```powershell
wsl --list --verbose
```

可能看到 `docker-desktop`、舊版的 `docker-desktop-data`、`rancher-desktop` 或其他使用者自建發行版。**只在確認 Docker Desktop 已卸載、資料已備份且發行版確實屬於舊 Docker Desktop 時**，才可執行：

```powershell
wsl --unregister docker-desktop
# 僅在確實存在且已確認可刪除時：
wsl --unregister docker-desktop-data
```

`wsl --unregister` 是永久刪除；**不可對 `rancher-desktop`、`rancher-desktop-data` 或仍需使用的 Ubuntu 執行**。不同 Docker Desktop 版本的 WSL 儲存架構不同，`docker-desktop-data` 不一定存在。**保留 WSL2 平台**，不需執行 `wsl --uninstall`。

## 7. 檢查 Docker CLI、Context 與環境變數

```powershell
where.exe docker
Get-Command docker -All -ErrorAction SilentlyContinue
Get-ChildItem Env:DOCKER* -ErrorAction SilentlyContinue
Get-ChildItem Env:TESTCONTAINERS* -ErrorAction SilentlyContinue
```

卸載後 `docker` 指令不存在是正常的；Rancher Desktop 安裝後會提供 CLI。若仍找到 `C:\Program Files\Docker\Docker\resources\bin` 等舊路徑，請檢查 Windows 使用者與系統 PATH，移除**確定已失效**的舊項目，不要刪除其他工具共用路徑。

若舊 CLI 尚可執行：

```powershell
docker context ls
```

不要把 `desktop-linux` 誤當成 Rancher Desktop Context；只在確認不再使用後，才移除舊 Context。不要直接刪除整個 `%USERPROFILE%\.docker`，其中可能包含 Registry 登入資訊及其他 Context。

檢查目前 PowerShell 工作階段的變數，若確認是舊設定，可清除：

```powershell
Remove-Item Env:DOCKER_HOST -ErrorAction SilentlyContinue
Remove-Item Env:DOCKER_CONTEXT -ErrorAction SilentlyContinue
Remove-Item Env:TESTCONTAINERS_HOST_OVERRIDE -ErrorAction SilentlyContinue
```

**這不會清除永久使用者／系統環境變數**。若永久設定存在，請在 Windows「環境變數」設定中確認來源後再修改，並重新開啟 IDE / Terminal。

## 8. 檢查殘留檔案（僅檢查，不批次刪除）

Docker 官方列出的可能殘留位置包括：

```text
C:\ProgramData\Docker
C:\ProgramData\DockerDesktop
C:\Program Files\Docker
%LOCALAPPDATA%\Docker
%APPDATA%\Docker
%APPDATA%\Docker Desktop
%USERPROFILE%\.docker
```

先確認資料用途、備份與權限；**不要執行一鍵 `Remove-Item -Recurse` 清理以上全部路徑**。`%USERPROFILE%\.docker` 尤其可能需要保留。解除安裝完成後建議重新開機。

## 9. 安裝 Rancher Desktop 與遷移後驗收

接續獨立的 Rancher Desktop V4 安裝手冊：安裝 WSL2（不安裝 Ubuntu）→ 安裝 Rancher Desktop → 選擇 **Moby (dockerd)** → 關閉 Kubernetes → 初始化 → 視版本確認 WSL 發行版 → 完全關閉 Rancher Desktop → 使用支援的 WSL 搬移方法將其資料移至 `D:\docker\wsl\` → 重啟。

驗證：

```powershell
wsl --list --verbose
where.exe docker
docker context ls
docker version
docker info
docker compose version
docker run --rm hello-world
```

**Windows 原生 Testcontainers 驗收不可省略：** 驗證 Docker API、Ryuk、Redis 動態 Port Mapping、`Testcontainers.MsSql` 的實際 SQL 連線，以及 Visual Studio Test Explorer / Rider 執行。`docker ps` 成功不代表 Testcontainers 已通過驗收。

## 10. 驗收紀錄模板

| 項目 | 結果 | 備註 |
|---|---|---|
| 舊 Docker Engine 資料已清點及備份 | ☐ | |
| Docker Desktop 已解除安裝 | ☐ | |
| 舊 `docker-desktop*` 發行版已確認／必要時移除 | ☐ | |
| WSL2 平台仍可正常使用 | ☐ | |
| 舊 PATH / Docker Context / 環境變數已檢查 | ☐ | |
| Rancher Desktop + Moby 正常啟動 | ☐ | |
| Rancher Desktop WSL 資料位置已驗證 | ☐ | |
| Docker CLI / Compose 正常 | ☐ | |
| .NET Testcontainers + Ryuk + SQL Server 正常 | ☐ | |
| Visual Studio / Rider 測試執行正常 | ☐ | |

## 11. 官方參考資料

- Docker Desktop 卸載：https://docs.docker.com/desktop/uninstall/
- Rancher Desktop 安裝：https://docs.rancherdesktop.io/getting-started/installation/
- Rancher Desktop 官網：https://rancherdesktop.io/
- Microsoft WSL 指令：https://learn.microsoft.com/windows/wsl/basic-commands
- Testcontainers for .NET 設定：https://dotnet.testcontainers.org/custom_configuration/

> **安全提醒：** 本手冊以保護既有資料為優先。凡涉及 `wsl --unregister`、Volume 刪除、殘留資料夾刪除，均須先確認對象與備份，且不要在仍有工作負載執行時操作。
