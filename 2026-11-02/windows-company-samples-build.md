# 在公司 Windows 環境建置與驗證 dotnet-gcp-sdk 範例專案

整理日期：2026-11-02。

本文適用於原始碼已放進公司 Windows 電腦、第三方套件統一由公司內部 NuGet／Nexus 提供，並以 PowerShell 與 Rider 建置及驗證範例的情境。第 2 至 8 節完成建置；第 9 節從 WSL、Docker 與映像來源準備開始，完成地端服務初始化、Console 範例、WebAPI 往返及 samples 整合測試。

本文以 `D:\Temp\dotnet-gcp-sdk` 為原始碼根目錄。請依實際位置替換路徑；公司來源網址使用佔位值，套用前必須換成公司提供的實際 NuGet API 網址。

## 1. 建置流程與必要條件

完整順序如下：

1. 準備 .NET 10 SDK 與 .NET 8 SDK。
2. 在 repo 根目錄設定公司 NuGet 來源。
3. 調整 `samples/nuget.config`，繼承公司來源並保留本機 SDK 來源。
4. 還原 SDK 方案，確認公司來源能提供所需版本。
5. 執行打包工具，產生 SDK 套件並自動還原 samples。
6. 在 `samples/` 建置範例方案。
7. 以 Rider 開啟範例方案。
8. 準備 WSL 2、Docker Desktop、公司容器映像來源與本機連接埠。
9. 以 Compose 啟動並初始化 Vault、SQL Server、Kafka 與 RustFS。
10. 驗證 Console 範例與 WebAPI 的實際讀寫、加解密及訊息處理。
11. 明確啟用 samples 的 Testcontainers 整合測試，記錄通過結果。

各階段使用的 SDK 與來源如下：

| 階段 | 執行目錄 | 使用的 .NET SDK | 套件來源 |
| --- | --- | --- | --- |
| 還原、建置與打包 SDK | repo 根目錄 | .NET 10 SDK | 公司 NuGet |
| 還原與建置 samples | `samples/` | .NET 8 SDK | 自製 SDK 從 `artifacts/packages/`；第三方套件從公司 NuGet |

samples 透過 `PackageReference` 使用 SDK 的 NuGet 套件。`artifacts/packages/` 是打包產物，不納入 Git，因此第一次取得原始碼後，需要先打包 SDK。

僅完成建置不需要啟動 Docker、資料庫或 GCP 資源。地端驗證則需要第 9 節列出的服務。本機驗證使用 `OnPremise` profile；不需要 Google Cloud CLI、ADC 或實際 GCP 資源。整合測試中的 PostgreSQL 也在本機容器執行。

## 2. 確認原始碼位置與目錄結構

本文使用以下位置：

```text
D:\Temp\dotnet-gcp-sdk\
├─ global.json
├─ nuget.config                              ← 公司環境需新增或調整
├─ Directory.Packages.props
├─ CompanyName.Com.Dotnet.Cloud.slnx          ← SDK 方案
├─ eng\
│  └─ pack-local.proj
├─ src\
├─ tests\
├─ artifacts\
│  └─ packages\                             ← 打包後產生
└─ samples\
   ├─ global.json
   ├─ nuget.config                          ← 已存在，需調整
   ├─ compose.yaml                          ← 本機服務與初始化腳本
   ├─ Directory.Packages.props
   ├─ CompanyName.Com.Dotnet.Cloud.Samples.sln
   ├─ SecretsSample\
   ├─ KmsSample\
   ├─ SqlSample\
   ├─ PubSubSample\
   ├─ StorageSample\
   └─ WebApiSample\
```

開啟 PowerShell，切換到 repo 根目錄並確認必要檔案：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'

Get-Location
Test-Path .\global.json
Test-Path .\eng\pack-local.proj
Test-Path .\CompanyName.Com.Dotnet.Cloud.slnx
Test-Path .\samples\CompanyName.Com.Dotnet.Cloud.Samples.sln
```

四個 `Test-Path` 都應回傳 `True`。若移動了原始碼，後續指令與 IDE 都要改用新的目錄。

## 3. 確認 .NET SDK

### 3.1 檢查已安裝的版本

```powershell
dotnet --list-sdks
```

輸出中需要同時有：

- .NET 10 SDK：`10.0.xxx`，至少符合根目錄 `global.json` 的 `10.0.100`。
- .NET 8 SDK：`8.0.xxx`，至少符合 `samples/global.json` 的 `8.0.200`。

兩份 `global.json` 都設定 `rollForward: latestFeature`，可使用同一個主、次版本內符合條件的較新 SDK feature band 與修補版本。

這裡需要安裝的是 **SDK**。只安裝 .NET Runtime 無法完成建置。缺少版本時，依公司軟體安裝流程安裝對應 SDK，再重新開啟 PowerShell 與 Rider。

### 3.2 確認不同目錄選到正確 SDK

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'
dotnet --version
```

預期顯示 `10.0.xxx`。

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
dotnet --version
```

預期顯示 `8.0.xxx`。

SDK 的選擇與目前工作目錄有關。因此後續打包指令在根目錄執行，samples 的建置指令在 `samples/` 執行。

## 4. 設定公司 NuGet 來源

### 4.1 取得公司提供的套件 API 網址

請使用公司 NuGet／Nexus 提供的實際套件來源，例如：

| 類型 | 網址格式範例 |
| --- | --- |
| Nexus NuGet v3 | `https://nexus.example.internal/repository/nuget-group/index.json` |
| Nexus NuGet v2 | `https://nexus.example.internal/repository/nuget-group/` |
| 公司提供的 HTTP 端點 | `http://nexus.example.internal:8081/repository/nuget-group/` |

`nexus.example.internal` 與 `nuget-group` 都是範例，應替換成公司的主機與倉庫名稱。v2／v3 請依公司來源的實際設定選用；已確認可正常查詢套件的端點可以沿用。Nexus v3 的來源應包含 `/index.json`。[Nexus 官方設定說明](https://help.sonatype.com/en/configure-nuget-with-nexus.html)

若網址包含 `/#browse/`，那是 Nexus 的網頁瀏覽路徑。請從倉庫設定複製 Repository URL，或參考公司其他可正常還原套件的專案設定。公司來源可以完全位於內網。

### 4.2 在 repo 根目錄新增或調整 nuget.config

檔案位置：

```text
D:\Temp\dotnet-gcp-sdk\nuget.config
```

以下提供 HTTPS、NuGet v3 的完整範本。**儲存前先替換 `value` 中的公司來源網址。**

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="company-nuget"
         value="https://nexus.example.internal/repository/nuget-group/index.json"
         protocolVersion="3" />
  </packageSources>

  <packageSourceMapping>
    <clear />
    <packageSource key="company-nuget">
      <package pattern="*" />
    </packageSource>
  </packageSourceMapping>
</configuration>
```

設定的作用：

- `packageSources` 的 `<clear />` 清除這個 repo 繼承到的其他來源，讓 SDK 的第三方套件由公司來源還原。
- `packageSourceMapping` 的 `<clear />` 清除繼承的來源對應規則，再將套件對應到公司來源。
- `company-nuget` 是本文使用的來源名稱。若公司已在使用者設定中為某個來源名稱配置認證，請沿用該名稱，並同步修改上面兩個 `key`。

這些設定適用於此 repo，不會修改整台電腦的 NuGet 設定。認證沿用公司既有的設定方式；帳密與 Token 不寫進 repo 內的設定範本。

NuGet 會合併不同層級的設定，`<clear />` 則清除該節點已繼承的內容。[NuGet 設定繼承規則](https://learn.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior)

### 4.3 公司使用 v2 或 HTTP 時的設定

若公司提供的是 **HTTPS、NuGet v2**，請將上一份範本的 `<add ... />` 替換為：

```xml
<add key="company-nuget"
     value="https://nexus.example.internal/repository/nuget-group/"
     protocolVersion="2" />
```

若公司提供並核准使用的是 **HTTP、NuGet v2**，則替換為：

```xml
<add key="company-nuget"
     value="http://nexus.example.internal:8081/repository/nuget-group/"
     protocolVersion="2"
     allowInsecureConnections="true" />
```

`allowInsecureConnections` 是 HTTP 來源的設定，與解決 XML／DTD 解析錯誤不同。若公司提供 HTTP 的 v3 端點，使用公司提供的 `/index.json` 網址，將 `protocolVersion` 設為 `3`，並保留 HTTP 所需的屬性。[NuGet 來源屬性說明](https://learn.microsoft.com/en-us/nuget/reference/nuget-config-file#packagesources)

### 4.4 修改 samples/nuget.config

檔案位置：

```text
D:\Temp\dotnet-gcp-sdk\samples\nuget.config
```

將內容改為：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="sdk-local" value="../artifacts/packages" />
  </packageSources>

  <packageSourceMapping>
    <packageSource key="sdk-local">
      <package pattern="CompanyName.Com.Dotnet.Cloud.*" />
    </packageSource>
  </packageSourceMapping>
</configuration>
```

這份設定繼承根目錄的公司來源與 `*` 對應規則，再加入本機 SDK 來源。此處保留繼承，因此不加 `<clear />`。

原始的 `samples/nuget.config` 有自己的 `<clear />` 與 nuget.org 來源；公司環境需將整份檔案替換為上面的內容，避免繼續套用原本的來源與對應規則。

套件還原的分工如下：

| 套件 | 還原來源 |
| --- | --- |
| `CompanyName.Com.Dotnet.Cloud.*` | `sdk-local`，即 repo 的 `artifacts/packages/` |
| Google、Microsoft、xUnit、Dapper 等第三方套件 | 根目錄設定的公司 NuGet |

`CompanyName.Com.Dotnet.Cloud.*` 的前綴規則比公司來源的 `*` 更明確，因此 SDK 套件會對應到本機來源。第三方套件及其傳遞相依則由公司來源提供。[Package Source Mapping 規則](https://learn.microsoft.com/en-us/nuget/consume-packages/package-source-mapping)

`../artifacts/packages` 是相對於 `samples/nuget.config` 的路徑。整份 repo 搬移後，這個相對路徑仍可使用。

### 4.5 確認兩個範圍的來源

在根目錄執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'
dotnet nuget list source
```

預期只列出已啟用的公司來源，網址為實際套件 API 網址。

再到 samples 執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
dotnet nuget list source
```

預期列出公司來源與 `sdk-local`。本機來源應解析為：

```text
D:\Temp\dotnet-gcp-sdk\artifacts\packages
```

首次打包前，這個資料夾可能尚未存在。下一階段會由打包工具產生。

`list source` 只確認有效設定，實際連線與版本可用性要由 `restore` 驗證。此處直接在各目錄執行，不加 `--configfile`；明確指定該選項時，NuGet 只使用指定檔案的設定，無法用來檢查本文的父子設定合併結果。

## 5. 還原 SDK 方案，確認套件版本可用

在根目錄執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'

dotnet restore .\CompanyName.Com.Dotnet.Cloud.slnx --no-http-cache --force-evaluate
```

這一步單獨執行還原，方便先找出公司來源、認證或版本問題。`--no-http-cache` 排除本機 HTTP 快取，`--force-evaluate` 重新評估相依關係；兩者都不會清除 Nexus 伺服器的快取。[dotnet restore 選項](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-restore)

本文根目錄使用 .NET 10 的 `--no-http-cache`。在 `samples/` 使用 .NET 8 排查還原問題時，對應選項為 `--no-cache`。

執行完立即查看結束代碼：

```powershell
$LASTEXITCODE
```

預期為 `0`。有錯誤時先依第 11 節處理，成功後再打包。

### 本次曾遇到的版本問題

本次排錯時出現：

```text
NU1102: Unable to find package Google.Cloud.PubSub.V1 with version (>= 3.38.0)
Nearest version: 3.37.0
```

代表公司來源能查到套件名稱，但當時無法提供專案要求的版本。對應設定在根目錄 `Directory.Packages.props`：

```xml
<PackageVersion Include="Google.Cloud.PubSub.V1" Version="3.38.0" />
```

官方確實已發布 [Google.Cloud.PubSub.V1 3.38.0](https://www.nuget.org/packages/Google.Cloud.PubSub.V1/3.38.0)。若重查後仍出現同一個錯誤，可提供以下資訊給公司 Nexus 管理員：

> 專案需要 Google.Cloud.PubSub.V1 3.38.0，目前公司 NuGet 來源查詢回報最接近版本為 3.37.0。請確認代理來源、版本清單快取及套件審核狀態，讓此版本與其相依套件可由公司來源還原。

若公司限定只能使用舊版本，需要另行調整 SDK 的相依版本並驗證建置與測試相容性。降版位置是根目錄 `Directory.Packages.props`；samples 的同名檔案管理範例的套件版本，無法修正 SDK 方案還原時的這個錯誤。

## 6. 打包 SDK 並自動還原 samples

在根目錄執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'

dotnet msbuild .\eng\pack-local.proj
```

打包工具會依序：

1. 以根目錄的 .NET 10 SDK 執行 SDK 方案的 Release 打包。
2. 為同一批 SDK 套件設定相同的 `local.<UTC 時間>` 版本後綴。
3. 將套件輸出到 `artifacts/packages/`。
4. 切換到 `samples/`，使用 .NET 8 SDK 重新還原範例方案。
5. 重新評估範例的浮動版本，讓範例取用本機打包的版本。

第 5 節是方便排錯的前置檢查；打包工具本身也會執行必要的還原。工具成功結束後，通常不必再手動還原 samples。

確認結束代碼：

```powershell
$LASTEXITCODE
```

預期為 `0`。再確認套件產物：

```powershell
Get-ChildItem .\artifacts\packages\CompanyName.Com.Dotnet.Cloud.*.nupkg |
    Select-Object Name
```

應能找到同一批版本的六個 SDK 套件：

- `CompanyName.Com.Dotnet.Cloud.Core`
- `CompanyName.Com.Dotnet.Cloud.Secrets`
- `CompanyName.Com.Dotnet.Cloud.Kms`
- `CompanyName.Com.Dotnet.Cloud.Sql`
- `CompanyName.Com.Dotnet.Cloud.PubSub`
- `CompanyName.Com.Dotnet.Cloud.Storage`

檔名版本例如 `0.1.0-local.20261102043000`，時間會隨打包改變。資料夾若保留多次打包產物，檔案數會超過六個。

本機套件版本需符合 `samples/Directory.Packages.props` 的 `0.1.0-local.*` 規則，因此使用 repo 提供的工具統一打包。單純執行 `dotnet build` 產生的 DLL，不會取代這一步需要的 NuGet 套件。

## 7. 建置 samples 方案

進入 `samples/` 後，先確認 SDK，再建置：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'

dotnet --version
```

預期為 `8.0.xxx`。確認後執行：

```powershell
dotnet build .\CompanyName.Com.Dotnet.Cloud.Samples.sln
```

確認結束代碼：

```powershell
$LASTEXITCODE
```

預期為 `0`，且沒有建置錯誤。範例設定了 `TreatWarningsAsErrors`，警告也會影響建置結果。

**完成這一步，才算完成範例方案的命令列建置。** SDK 方案還原與 `pack-local.proj` 成功，分別代表前面的階段完成。

## 8. 在 Rider 開啟範例方案

1. 在 Rider 開啟新的方案位置：

   ```text
   D:\Temp\dotnet-gcp-sdk\samples\CompanyName.Com.Dotnet.Cloud.Samples.sln
   ```

2. 等待方案載入與 NuGet 還原完成。
3. 執行 Build Solution，確認 IDE 建置成功。
4. 要執行範例時，再選擇對應的啟動專案與 launch profile。

repo 根目錄的 `CompanyName.Com.Dotnet.Cloud.slnx` 是 SDK 開發方案；開發與執行範例時，使用 `samples/` 下的 `.sln`。

若命令列已成功，但 Rider 仍顯示舊錯誤，依序處理：

1. 確認開啟的是搬移後的新方案路徑。
2. 在 Rider 重新還原 NuGet 套件，並重新載入方案。
3. 確認 IDE 使用已安裝、符合 `samples/global.json` 的 .NET 8 SDK。
4. 仍保留舊狀態時，再使用 File → Invalidate Caches，重新開啟方案。

## 9. 地端環境準備與完整驗證

本節在公司 Windows 電腦上，以 Docker Desktop 建立本機 Vault、SQL Server、Kafka 與 RustFS，應用程式則在 Windows 執行。所有服務使用專供範例的容器與初始化資源。

| 順序 | 要完成的工作 | 通過條件 |
| --- | --- | --- |
| 9.1 | Windows、WSL 與 Docker | Docker Server 可連線，`OSType` 為 `linux` |
| 9.2 | 公司映像來源 | 四個服務映像可取得，整合測試的 PostgreSQL 與 Ryuk 也可取得 |
| 9.3 | 連接埠與應用程式設定 | 主機連接埠可用，profile 與端點對應正確 |
| 9.4 至 9.5 | 啟動服務及確認初始化 | 四個服務為 healthy，範例資源已建立 |
| 9.6 | Console 範例 | 各項往返驗證完成，結束代碼為 0 |
| 9.7 | WebAPI | 客戶建立／查詢、訊息處理與附件往返完成 |
| 9.8 | 非預設連接埠 | 有衝突時，同時調整 Compose 與應用程式設定 |
| 9.9 至 9.10 | 整合測試及 IDE | 明確啟用後，容器測試實際通過 |
| 9.11 | 停止與重設 | 明確選擇保留容器或清除本次驗證資料 |

### 9.1 準備 Windows、WSL 2 與 Docker Desktop

#### 9.1.1 安裝前確認

| 項目 | 準備內容 |
| --- | --- |
| 作業系統與 CPU | 以 Windows x64 電腦為本文路徑，Windows 版本須在公司核准的 Docker Desktop 支援範圍內；SQL Server 映像是 `linux/amd64` |
| 虛擬化 | 在工作管理員「效能 → CPU」確認虛擬化已啟用；未啟用時，由公司設備管理流程調整 BIOS／UEFI |
| WSL | WSL 2，WSL 套件版本至少 2.1.5 |
| 容器引擎 | 公司核准的 Docker Desktop，使用 WSL 2 backend 與 Linux containers |
| CLI | Windows PowerShell 5.1 或 PowerShell 7、Docker CLI、支援 `up --wait` 的 Docker Compose plugin |
| .NET | 第 3 節的 .NET 10 SDK 與 .NET 8 SDK；程式仍在 Windows 執行 |
| 映像取得 | Docker Hub／Microsoft Container Registry 的公司核准代理，或公司內部 Docker registry／核准離線映像檔 |

Docker Desktop 的 Windows 支援條件與 WSL 最低版本以[官方安裝文件](https://docs.docker.com/desktop/setup/install/windows-install/)為準。

容量規劃可先以實體記憶體 16 GB 以上、WSL 可用記憶體約 8 GB、4 個可用 CPU 核心，以及容器儲存磁碟至少 30 GB 空間估算；若要同時保留 Compose 服務並執行多組整合測試，建議準備更多記憶體。這是本指南的工作負載規劃建議，實際用量依同時啟動的測試而異。

#### 9.1.2 檢查或安裝 WSL

先在 PowerShell 檢查：

```powershell
wsl --version
wsl --status
wsl --list --verbose
```

未安裝 WSL 時，可由公司 IT 或具對應權限的人員執行以下安裝命令；已有可用 WSL 的電腦直接略過安裝：

```powershell
wsl --install --no-distribution
```

依安裝結果重新開機。需要更新或設定新發行版的預設版本時，再執行：

```powershell
wsl --update
wsl --set-default-version 2
```

公司封鎖線上下載時，請使用公司提供的 WSL 安裝／更新套件。指令用途見 [Microsoft WSL 命令說明](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)。

本指南在 Windows PowerShell 執行 Docker 與 dotnet，Docker Desktop 不要求另外安裝 Ubuntu；若已有 Ubuntu，也不需要把這份 repo 搬進 Ubuntu。啟動 Docker Desktop 後，WSL 清單中的 Docker 發行版應使用版本 2。[Docker WSL 2 backend 說明](https://docs.docker.com/desktop/features/wsl/)

#### 9.1.3 安裝與設定 Docker Desktop

1. 由公司的軟體中心或核准安裝套件安裝 Docker Desktop。
2. 從 Windows 開始功能表啟動 Docker Desktop，完成公司要求的初始設定。
3. 在 Settings → General 確認使用 WSL 2 based engine；部分版本在符合條件時預設啟用，不另顯示選項。
4. 確認目前是 Linux container 模式。若選單提供 Switch to Linux containers，先切換至該模式。
5. 依公司網路設定 Docker Desktop 的代理、registry 認證與信任憑證，見第 9.2 節。
6. 等待 Engine 進入可用狀態，再開啟新的 PowerShell。

接著檢查：

```powershell
docker version
docker info --format '{{.OSType}}'
docker context ls
docker compose version
docker compose up --help | Select-String -- '--wait'
```

預期結果：`docker version` 同時列出 Client 與 Server；`OSType` 顯示 `linux`；Compose 能辨識 `--wait`。`desktop-linux` 是 Docker Desktop 常見的 context 名稱，但仍應以電腦實際設定為準。

若 `docker version` 只有 Client，或顯示無法連線，先處理 Engine 啟動與公司權限設定，暫停後續容器操作。

另外確認目前 PowerShell 是否有其他 Docker 專案留下的連線設定：

```powershell
Get-ChildItem Env:DOCKER_HOST,Env:DOCKER_CONTEXT,Env:DOCKER_TLS_VERIFY,Env:DOCKER_CERT_PATH -ErrorAction SilentlyContinue |
    Select-Object Name
```

本文使用本機 Docker Desktop，通常不需要額外設定這些變數。發現設定時，先核對用途；確認是舊專案留下的設定，才移除本工作階段中對應的變數。Testcontainers 也會讀取自己的設定，見第 9.9 節。

#### 9.1.4 容器記憶體不足時

WSL 2 模式的資源限制可透過 `%USERPROFILE%\.wslconfig` 管理。若公司已有設定，合併所需項目；以下僅為有足夠實體資源時的範例：

```ini
[wsl2]
memory=8GB
processors=4
```

儲存後，要先結束受影響的 WSL 工作與容器，再由使用者安排 `wsl --shutdown`，之後重新啟動 Docker Desktop；該命令會停止整個 WSL 環境。設定格式見 [Microsoft WSL 全域設定](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#wslconfig)。

### 9.2 準備公司容器映像來源

`nuget.config` 只管理 .NET 套件；Docker 映像由 Docker 引擎與 registry 設定管理。NuGet 還原成功後，仍須確認以下映像可取得。

#### 9.2.1 本指南需要的映像

下表版本取自目前 repo 的 `samples/compose.yaml` 與兩個 samples 整合測試專案的 `ContainerImages.cs`：

| 用途 | 映像 | 使用時機 |
| --- | --- | --- |
| Vault | `hashicorp/vault:2.1.2` | KmsSample、WebApiSample 與整合測試 |
| SQL Server | `mcr.microsoft.com/mssql/server:2022-CU27-ubuntu-22.04` | SqlSample、WebApiSample 與整合測試 |
| Kafka | `apache/kafka:4.1.2` | PubSubSample、WebApiSample 與整合測試 |
| RustFS | `rustfs/rustfs:1.0.3` | StorageSample、WebApiSample 與整合測試 |
| PostgreSQL | `postgres:18.6` | samples 的 PostgreSQL repository 整合測試 |
| Resource Reaper | `testcontainers/ryuk:0.14.0` | Testcontainers 清理其建立的測試資源 |

目前 Testcontainers 4.16.0 的預設 Ryuk 參照另外包含 digest：`sha256:7c1a8a9a47c780ed0f983770a662f80deb115d95cce3e2daa3d12115b8cd28f0`。準備內部鏡像時，也要提供相同內容的清理工具映像。[Testcontainers 設定說明](https://dotnet.testcontainers.org/custom_configuration/)

本指南執行的是 samples 的驗證；不需要先建置 SDK 完整測試套件另用的 MinIO 社群版映像。

#### 9.2.2 公司允許由核准代理取得原始映像

在 Docker Desktop 的代理設定頁面，套用公司提供的系統代理／PAC 或手動代理。新版 Desktop 可能分成 Desktop 與 Containers proxy；映像下載須使用公司要求的那一組代理。憑證與登入依公司提供的方式設定。[Docker Desktop 代理設定](https://docs.docker.com/desktop/settings-and-maintenance/settings/#proxies)

設定完成後，在 `samples/` 先下載四個服務映像：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'

docker compose pull vault sqlserver kafka rustfs
if ($LASTEXITCODE -ne 0) {
    throw '服務映像下載失敗，請先確認公司 Docker registry／代理／認證設定。'
}
```

若預計執行整合測試，再確認 PostgreSQL 與預設 Ryuk 映像：

```powershell
docker pull postgres:18.6
docker pull testcontainers/ryuk:0.14.0@sha256:7c1a8a9a47c780ed0f983770a662f80deb115d95cce3e2daa3d12115b8cd28f0
```

這些是既有映像，不需要在 Windows 上從 Dockerfile 建置 Vault、SQL Server、Kafka 或 RustFS。

#### 9.2.3 公司只提供內部 Docker registry

先取得公司核准的四個完整映像名稱與版本，依需要登入公司 registry：

```powershell
docker login registry.example.internal
```

`registry.example.internal` 是佔位主機，套用前替換。登入時依公司方式互動輸入憑證。

在 `samples/` 新增 `compose.company.yaml`；下列範例假設公司保留原有映像內容，並以 `dockerhub/`、`mcr/` 區分來源：

```yaml
services:
  vault:
    image: registry.example.internal/dockerhub/hashicorp/vault:2.1.2
  sqlserver:
    image: registry.example.internal/mcr/mssql/server:2022-CU27-ubuntu-22.04
  kafka:
    image: registry.example.internal/dockerhub/apache/kafka:4.1.2
  rustfs:
    image: registry.example.internal/dockerhub/rustfs/rustfs:1.0.3
```

請將四個 `image` 改為公司實際提供的名稱。這個覆寫檔保留原始 Compose 的初始化與健康檢查。

採用此方式時，本指南後續每個 `docker compose` 指令都要帶相同的兩份檔案。例如：

```powershell
docker compose -f .\compose.yaml -f .\compose.company.yaml config --images
docker compose -f .\compose.yaml -f .\compose.company.yaml pull vault sqlserver kafka rustfs
docker compose -f .\compose.yaml -f .\compose.company.yaml up -d --wait vault sqlserver kafka rustfs
docker compose -f .\compose.yaml -f .\compose.company.yaml ps
```

查 log、停止與清除也使用相同的 `-f` 參數。以下操作範例使用原始 Compose 的簡短寫法，內部 registry 使用者依此規則替換。

Compose 覆寫檔不會套用到 Testcontainers。測試映像的公司來源另依第 9.9 節設定。

#### 9.2.4 公司使用離線映像檔

可由公司提供已核准、包含所需標籤的 Docker image archive，再於驗證電腦匯入：

```powershell
docker load --input 'D:\Installers\dotnet-gcp-sdk-images.tar'
```

匯入後，映像名稱必須符合有效 Compose 設定與 Testcontainers 的實際參照。用下列命令檢查個別映像；若名稱不同，依公司映像命名使用覆寫檔或測試設定：

```powershell
docker compose config --images
docker image inspect hashicorp/vault:2.1.2 --format '{{.Id}}'
```

Ryuk 的預設 digest 參照也須可解析；若離線檔只保留核准的 tag，依第 9.9 節明確指定已載入的 Ryuk 名稱。

### 9.3 確認連接埠、設定檔與啟動 profile

#### 9.3.1 本機服務連接埠

| 服務 | Windows 主機位址 | 容器內連接埠 | Compose 的主機 port 變數 |
| --- | --- | --- | --- |
| Vault | `http://127.0.0.1:18200` | `8200` | `CLOUD_SDK_SAMPLES_VAULT_PORT` |
| SQL Server | `127.0.0.1,11433` | `1433` | `CLOUD_SDK_SAMPLES_SQLSERVER_PORT` |
| Kafka | `127.0.0.1:19092` | HOST listener `9094` | `CLOUD_SDK_SAMPLES_KAFKA_PORT` |
| RustFS S3 API | `http://127.0.0.1:19000` | `9000` | `CLOUD_SDK_SAMPLES_RUSTFS_PORT` |
| WebApiSample | `http://127.0.0.1:5180` | 應用程式在 Windows 執行 | 以 WebAPI 的 `--urls` 調整 |

應用程式連線的是 Windows 主機 port。Kafka 容器內 CLI 使用的 `localhost:9092` 是另一個 listener，僅用於容器內初始化與檢查。

啟動前，在 Windows PowerShell 查詢占用與系統保留範圍：

```powershell
Get-NetTCPConnection -State Listen -LocalPort 18200,11433,19092,19000,5180 -ErrorAction SilentlyContinue |
    Select-Object LocalAddress,LocalPort,OwningProcess

netsh interface ipv4 show excludedportrange protocol=tcp
```

已有本次 Compose 服務時，看到其監聽是正常的；首次啟動且被其他程式占用，或落在系統保留範圍時，依第 9.8 節換 port。

#### 9.3.2 各設定檔的分工

| 檔案／來源 | 用途 | 本機驗證需做的事 |
| --- | --- | --- |
| `appsettings.json` | 共用設定與 log 等級 | 保留 repo 的預設值 |
| `appsettings.Development.json` | 預設 `Cloud:Platform=OnPremise`、本機位址與資源對應 | 端點維持預設時可直接使用；有換 port 或服務時調整對應鍵 |
| `Properties/launchSettings.json` 的 `OnPremise` | `Development` 環境與範例 cipher 參照 | 以 `dotnet run --launch-profile OnPremise` 選用 |
| `appsettings.Staging.json` 與 `OnPremise-Staging` | 地端部署設定形態的示範 | 需要驗證 Staging 設定時，再選該 profile |
| `Gcp` profile | GCP 模式的設定 | 本節本機驗證選用 `OnPremise` |

Kms、Sql、Storage 與 WebAPI 範例的 cipher 參照在設定載入時解析。預設的 prototype Cipher 是 UTF-8／Base64 示範編碼器，沒有提供實際的加密保護；只用於這份範例的 Development／Staging 驗證。`Production` 要接上公司真正的 `ICipher`，並滿足各服務的正式環境檢查。

從預設 repo 設定開始時，Compose 自動建立的範例帳號與 launch profile 已配對，不需要另把真實公司帳密填進這些檔案。

以下是預設設定應對得上的值。各 Console 範例的檔案都在 `samples/<範例名稱>/`；WebAPI 的檔案在 `samples/WebApiSample/WebApiSample.Rs/`：

| 範例 | `appsettings.Development.json` 中的地端設定 | `OnPremise` profile 提供的憑證 |
| --- | --- | --- |
| KmsSample | `Cloud:Kms:OnPremise:Address=http://127.0.0.1:18200`；`Cloud:Kms:Keys:sample:OnPremise=kms-sample-key` | `Cloud__Kms__OnPremise__Token`，對應 Compose 的 `kms-sample-app-token` |
| SqlSample | `Cloud:Sql:OnPremise:Server=127.0.0.1,11433`；`Database=SqlSample`；`TrustServerCertificate=true` | `Cloud__Sql__OnPremise__Username`、`Password`，對應 `sql_sample_app` |
| PubSubSample | `Cloud:PubSub:OnPremise:BootstrapServers=127.0.0.1:19092`；Publish 與 `Subscriptions:orders` 的 Topic 均為 `pubsub-sample-topic`，GroupId 為 `pubsub-sample.orders` | 此範例的 Kafka 連線未設定認證 |
| StorageSample | `Cloud:Storage:OnPremise:Endpoint=http://127.0.0.1:19000`；`Cloud:Storage:Buckets:documents:OnPremise=storage-sample` | `Cloud__Storage__OnPremise__AccessKey`、`SecretKey`，對應 `storage-sample-app` |
| WebApiSample.Rs | SQL Database 為 `WebApiSample`；Kms 邏輯 key `customer-pii` 對應 `webapi-sample-customer-key`；Kafka 使用 `webapi-sample-customer-events` 與 `webapi-sample.customer-events`；Storage 邏輯 bucket `customer-documents` 對應 `webapi-sample-documents`，四個服務端點與上表相同 | SQL app 使用者 `webapi_sample_app`、Vault app token `webapi-sample-app-token`、Storage app 使用者 `webapi-sample-app` 的 cipher 參照 |

表內 SQL 的 `Database`、`TrustServerCertificate` 均位於 `Cloud:Sql:OnPremise` 之下；Kafka 的 Topic／GroupId 位於對應的 Publish／Subscription 的 OnPremise 區段。若自行調整範例帳密，需要同步更新 Compose 的服務端初始化與 profile 的參照；只改其中一端會造成登入或權限失敗。

例如可以在 `samples/` 檢查 Kms 的非秘密設定與 profile 名稱：

```powershell
$kmsSettings = Get-Content .\KmsSample\appsettings.Development.json -Raw | ConvertFrom-Json
$kmsSettings.Cloud.Platform
$kmsSettings.Cloud.Kms.OnPremise.Address
$kmsSettings.Cloud.Kms.Keys.sample.OnPremise

$kmsProfiles = Get-Content .\KmsSample\Properties\launchSettings.json -Raw | ConvertFrom-Json
$kmsProfiles.profiles.PSObject.Properties.Name
```

依序應看到 `OnPremise`、Vault 的預設位址、`kms-sample-key`，以及包含 `OnPremise`、`Gcp`、`OnPremise-Staging` 的 profile 清單。實際憑證參照保持由 profile 載入。

有效設定大致依「應用程式命令列參數 → 環境變數 → 環境 JSON → 共用 JSON」決定。啟動前若懷疑舊環境變數覆寫設定，可只列出名稱：

```powershell
Get-ChildItem Env: |
    Where-Object { $_.Name -like 'Cloud__*' -or $_.Name -like 'CLOUD_SDK_SAMPLES_*' } |
    Select-Object Name
```

核對不屬於本次驗證的變數後，才移除對應的本工作階段設定。切換平台時，其他設定來源中的 `sm://` 或 `cipher://` 參照也會接受 Secrets 檢查。

### 9.4 啟動 Compose 並自動初始化服務

所有 Compose 操作都在 `samples/` 執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'

Get-ChildItem Env:COMPOSE_PROJECT_NAME,Env:COMPOSE_FILE -ErrorAction SilentlyContinue
```

本文預期這兩個變數未設定，讓 Compose 使用 repo 的 `compose.yaml` 與 project 名稱 `dotnet-gcp-sdk-samples`。若有設定，先確認是否屬於其他專案，再於本次 PowerShell 工作階段處理。

確認可以移除目前工作階段的覆寫後，可執行；這不修改使用者或系統層級的環境變數：

```powershell
Remove-Item Env:COMPOSE_PROJECT_NAME,Env:COMPOSE_FILE -ErrorAction SilentlyContinue
```

確認有效檔案與四個 service：

```powershell
docker compose config --quiet
docker compose config --services
docker compose config --images
$composeConfig = docker compose config --format json | ConvertFrom-Json
$composeConfig.name
```

`config --quiet` 應成功，service 清單應包含 `vault`、`sqlserver`、`kafka`、`rustfs`，project 名稱應為 `dotnet-gcp-sdk-samples`。名稱或檔案不符時，先核對環境變數及本機 `.env` 等覆寫，再進行啟動或清除。確認後啟動全部地端服務：

```powershell
docker compose up -d --wait vault sqlserver kafka rustfs
if ($LASTEXITCODE -ne 0) {
    throw '地端服務尚未全部就緒，請查看 compose ps 與服務 log。'
}

docker compose ps
```

預期四個服務都為 running／healthy。初始化耗時依電腦及映像是否已下載而異，應以命令結果與 healthy 狀態判斷。

只驗證一個範例時，也可依需求單獨啟動：

```powershell
docker compose up -d --wait vault
docker compose up -d --wait sqlserver
docker compose up -d --wait kafka
docker compose up -d --wait rustfs
```

對應 Kms、Sql、PubSub、Storage 範例；SecretsSample 不需要服務，WebApiSample 需要四個服務。

以下資源已由 Compose 初始化腳本建立，無須先在 Windows 安裝獨立的 Vault、SQL Server、Kafka 或 RustFS：

| 服務 | Console 範例的資源 | WebApiSample 的資源 |
| --- | --- | --- |
| Vault | transit 引擎、`kms-sample-key`、`kms-sample` policy、`kms-sample-app-token` | `webapi-sample-customer-key`、`webapi-sample` policy、`webapi-sample-app-token` |
| SQL Server | `SqlSample` 資料庫、`sql_sample_app` login/user、`dbo.Notes`，授予 SELECT／INSERT／DELETE | `WebApiSample` 資料庫、`webapi_sample_app` login/user；Customers、CustomerProcessingRecords、FailedMessages 的 SELECT／INSERT，以及 CustomerDocuments 的 SELECT／INSERT／DELETE |
| Kafka | `pubsub-sample-topic`、group `pubsub-sample.orders` 的初始 offset | `webapi-sample-customer-events`、group `webapi-sample.customer-events` 的初始 offset |
| RustFS | `storage-sample` bucket、`storage-sample-app` app 使用者與該 bucket 的 policy | `webapi-sample-documents` bucket、`webapi-sample-app` 使用者與該 bucket 的 policy |

Kafka 關閉自動建立 topic；SQL Server 使用 Developer 版與 Compose 內的 `ACCEPT_EULA=Y`，作為開發測試用途。這些初始化與健康檢查的完整定義在 [samples/compose.yaml](../../samples/compose.yaml)。

### 9.5 確認服務與資源真的可用

若前一步失敗，先看狀態及 log：

```powershell
docker compose ps -a
docker compose logs --tail 100 vault sqlserver kafka rustfs
```

再逐項檢查；以下使用 repo 的預設 port 與公開的範例帳密。

**Vault：** HTTP 回應應有 `initialized=true`、`sealed=false`，兩把 transit key 均可讀取：

```powershell
Invoke-RestMethod -Uri 'http://127.0.0.1:18200/v1/sys/health'
docker compose exec -T vault vault read transit/keys/kms-sample-key
docker compose exec -T vault vault read transit/keys/webapi-sample-customer-key
```

**SQL Server：** 以 app 使用者查詢各自的資料表結構。以下密碼是 repo 內固定的本機示範值：

```powershell
docker compose exec -T sqlserver /opt/mssql-tools18/bin/sqlcmd -C -S 127.0.0.1 -U sql_sample_app -P SqlSampleApp2026 -d SqlSample -b -Q 'SELECT TOP (0) NoteId FROM dbo.Notes;'

docker compose exec -T sqlserver /opt/mssql-tools18/bin/sqlcmd -C -S 127.0.0.1 -U webapi_sample_app -P WebApiSampleApp2026 -d WebApiSample -b -Q 'SELECT TOP (0) DocumentId FROM dbo.CustomerDocuments;'
```

兩個命令應成功，結束代碼為 0；資料表可以是空的。

**Kafka：** topic 清單應有兩個範例 topic：

```powershell
docker compose exec -T kafka /opt/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

預期包含 `pubsub-sample-topic` 與 `webapi-sample-customer-events`。

**RustFS：** readiness 端點應回傳 HTTP 200：

```powershell
$rustFsHealth = Invoke-WebRequest -UseBasicParsing -Uri 'http://127.0.0.1:19000/health/ready'
$rustFsHealth.StatusCode
```

RustFS 的 Compose healthy 標記會等兩組 bucket、policy 與 app 使用者初始化完成才建立。主機只開放 S3 API，不開放 RustFS Console；app 權限的實際讀寫由下一節與附件往返確認。

### 9.6 依序驗證五個 Console 範例

先確認已完成第 7 節的 samples Debug 建置。以下使用 `--no-build` 執行同一批產物；有改過程式或 profile 時，先重新建置。每個範例都在其專案目錄執行，讓 `appsettings*.json` 的 content root 正確。

#### 9.6.1 SecretsSample：設定參照替換

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples\SecretsSample'
dotnet run --no-build --launch-profile OnPremise
if ($LASTEXITCODE -ne 0) { throw 'SecretsSample 驗證失敗。' }
```

預期：平台為 `OnPremise`，環境為 `Development`；API key 與連線字串密碼顯示「已替換」及長度，程式正常結束。此步驟不會測試 Vault 或 SQL Server 連線。

#### 9.6.2 KmsSample：Vault 加解密

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples\KmsSample'
dotnet run --no-build --launch-profile OnPremise
if ($LASTEXITCODE -ne 0) { throw 'KmsSample 驗證失敗。' }
```

預期：三種加解密往返一致、65536 bytes 可加密；65537 bytes 的超限錯誤與損毀密文的 `CloudException` 是範例刻意驗證的情境，會被攔截後正常結束。

#### 9.6.3 SqlSample：SQL Server 新增、查詢與刪除

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples\SqlSample'
dotnet run --no-build --launch-profile OnPremise
if ($LASTEXITCODE -ne 0) { throw 'SqlSample 驗證失敗。' }
```

預期：新增完成、查詢往返一致、刪除 1 筆、刪除後找不到。最後查詢 `sample_missing_table` 所顯示的 `SqlException` 是刻意示範的提供者例外；程式應正常結束。

#### 9.6.4 PubSubSample：Kafka 發送、接收與重試

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples\PubSubSample'
dotnet run --no-build --launch-profile OnPremise
if ($LASTEXITCODE -ne 0) { throw 'PubSubSample 驗證失敗。' }
```

預期：正常訊息可處理、暫時失敗訊息重試後成功、不合法訊息重試用盡後交給 `SampleFailedMessageSink`；最後顯示「三個結果都已完成」與 `Health check：Healthy`。此範例刻意輸出部分 Warning／Error；驗收時要同時確認結果與結束代碼。

#### 9.6.5 StorageSample：物件上傳、下載與刪除

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples\StorageSample'
dotnet run --no-build --launch-profile OnPremise
if ($LASTEXITCODE -ne 0) { throw 'StorageSample 驗證失敗。' }
```

預期：文字與二進位物件上傳成功，下載的 SHA-256 與上傳內容一致，不存在物件回傳 null，刪除與重複刪除完成。

要另外驗證 Staging 設定形態時，在同一個範例專案資料夾改用：

```powershell
dotnet run --no-build --launch-profile OnPremise-Staging
```

預期往返結果相同，環境名稱為 `Staging`。預設 Staging 設定仍指向這組本機服務，沒有自動改成公司的正式服務。

### 9.7 驗證 WebApiSample 的完整服務往返

這一步使用 SQL Server、Vault、Kafka 與 RustFS。僅看到程式開始監聽或 `/health` 回傳 200，還不足以確認所有服務已完成實際操作。

#### 9.7.1 在第一個 PowerShell 啟動 WebAPI

確認四個 Compose 服務 healthy 後執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples\WebApiSample\WebApiSample.Rs'
dotnet run --no-build --launch-profile OnPremise
```

預期看到 `Platform=OnPremise`、`Development`，以及 `Now listening on: http://127.0.0.1:5180`。保持此視窗執行，另開第二個 PowerShell 做 API 驗證。

#### 9.7.2 在第二個 PowerShell 建立及查詢客戶

以下各區塊在**同一個第二視窗**依序執行，會沿用 `$baseUrl`、函式與這次建立的識別碼。資料均為虛構的範例資料。

```powershell
$baseUrl = 'http://127.0.0.1:5180'

function Invoke-SamplePost {
    param(
        [string]$Path,
        [hashtable]$Payload,
        [string]$ExpectedCode = '000000'
    )

    $json = $Payload | ConvertTo-Json -Depth 10
    $bodyBytes = [System.Text.Encoding]::UTF8.GetBytes($json)
    $response = Invoke-RestMethod -Method Post -Uri "$baseUrl$Path" -ContentType 'application/json; charset=utf-8' -Body $bodyBytes
    if ($response.code -ne $ExpectedCode) {
        throw "API $Path 回應 code=$($response.code)，預期為 $ExpectedCode。"
    }
    return $response
}

$health = Invoke-WebRequest -UseBasicParsing -Uri "$baseUrl/health"
if ($health.StatusCode -ne 200 -or $health.Content.Trim() -ne 'Healthy') {
    throw 'WebAPI health 尚未為 Healthy。'
}

$create = Invoke-SamplePost -Path '/api/customers/create' -Payload @{
    name = 'Windows 地端驗證客戶'
    phone = '0900-000-000'
    email = 'customer@example.com'
    nationalId = 'Z100000001'
}
$customerId = $create.data.customerId
if (-not $customerId) { throw '建立客戶沒有取得 customerId。' }

$query = Invoke-SamplePost -Path '/api/customers/query' -Payload @{ customerId = $customerId }
if ($query.data.nationalId -ne 'Z100000001') {
    throw '客戶資料的 Kms 加解密往返不一致。'
}
Write-Host '客戶建立、SQL 寫入與查詢、Vault 加解密：通過。'
```

業務 API 使用回應信封，成功代碼為字串 `000000`；錯誤也可能使用 HTTP 200，因此函式同時檢查 `code`。`/health` 主要用來觀察 Pub/Sub 是否卡住；資料庫與 Vault 的實際可用性由建立、查詢結果確認。

#### 9.7.3 確認背景訊息確實寫入處理紀錄

建立客戶會發送事件，背景 handler 接收後寫入處理紀錄。以最多 30 次、每次間隔 1 秒輪詢：

```powershell
$processed = $false
for ($attempt = 0; $attempt -lt 30; $attempt++) {
    $records = Invoke-SamplePost -Path '/api/customers/processing-records/query' -Payload @{ customerId = $customerId }
    if ($null -ne $records.data -and @($records.data).Count -ge 1) {
        $processed = $true
        break
    }
    Start-Sleep -Seconds 1
}
if (-not $processed) {
    throw '等待處理紀錄逾時，請檢查 Kafka、topic、group 與 WebAPI handler 的 log。'
}
Write-Host 'Kafka 發送、接收與處理紀錄寫入：通過。'
```

有處理紀錄才代表這次事件完成了背景處理。若逾時，可先查第一個視窗的 log，再查 Compose 的 Kafka 與 SQL Server 狀態。

#### 9.7.4 上傳、下載比對與刪除附件

以下使用 Windows 的 `curl.exe` 傳送 multipart，避免 Windows PowerShell 5.1 的 `curl` 別名問題。若公司沒有提供 `curl.exe`，可改在 Rider 開啟 `WebApiSample.Rs.http`，依「建立客戶 → 上傳 → 下載 → 刪除」的順序驗證。

```powershell
Get-Command curl.exe -ErrorAction Stop | Out-Null

$uploadFile = 'D:\Temp\dotnet-gcp-sdk\samples\WebApiSample\WebApiSample.Rs\SampleFiles\sample.pdf'
if (-not (Test-Path -LiteralPath $uploadFile)) { throw '找不到範例 PDF。' }

$uploadText = & curl.exe --silent --show-error --fail --noproxy '*' --request POST "$baseUrl/api/customers/documents/upload" --form "customerId=$customerId" --form "file=@$uploadFile;type=application/pdf"
if ($LASTEXITCODE -ne 0) { throw '附件上傳 HTTP 請求失敗。' }
$upload = ($uploadText -join [Environment]::NewLine) | ConvertFrom-Json
if ($upload.code -ne '000000' -or -not $upload.data.documentId) {
    throw "附件上傳失敗，code=$($upload.code)。"
}
$documentId = $upload.data.documentId

$verifyDirectory = Join-Path $env:TEMP ('dotnet-gcp-sdk-verify-' + [Guid]::NewGuid().ToString('N'))
New-Item -ItemType Directory -Path $verifyDirectory -ErrorAction Stop | Out-Null
$downloadFile = Join-Path $verifyDirectory 'downloaded-sample.pdf'
$documentPayload = @{ customerId = $customerId; documentId = $documentId }
$downloadJson = $documentPayload | ConvertTo-Json
$downloadBytes = [System.Text.Encoding]::UTF8.GetBytes($downloadJson)

$download = Invoke-WebRequest -UseBasicParsing -Method Post -Uri "$baseUrl/api/customers/documents/download" -ContentType 'application/json; charset=utf-8' -Body $downloadBytes -OutFile $downloadFile -PassThru
if (($download.Headers['Content-Type'] -join ',') -notlike 'application/pdf*') {
    throw '下載回應不是 PDF，請檢查是否為錯誤信封。'
}
$originalHash = (Get-FileHash -LiteralPath $uploadFile -Algorithm SHA256).Hash
$downloadHash = (Get-FileHash -LiteralPath $downloadFile -Algorithm SHA256).Hash
if ($originalHash -ne $downloadHash) { throw '附件下載內容的 SHA-256 不一致。' }

$null = Invoke-SamplePost -Path '/api/customers/documents/delete' -Payload $documentPayload
$null = Invoke-SamplePost -Path '/api/customers/documents/download' -Payload $documentPayload -ExpectedCode '100003'
Write-Host 'RustFS 附件上傳、下載 SHA-256 比對、刪除與刪除後查無附件：通過。'
Write-Host "本次下載驗證檔案：$downloadFile"
```

此步驟也會驗證 WebAPI 對 `CustomerDocuments` 資料表的寫入與刪除權限。上傳使用 repo 提供的 PDF，符合範例的副檔名、檔案簽章與大小限制。

#### 9.7.5 驗證輸入檢查

```powershell
$null = Invoke-SamplePost -Path '/api/customers/create' -Payload @{
    name = 'Windows 地端驗證客戶'
    phone = '0900-000-000'
    email = 'customer@example.com'
    nationalId = 'invalid'
} -ExpectedCode '100001'
Write-Host '輸入格式驗證：通過。'
```

全部完成後，在第一個 PowerShell 按 Ctrl+C 停止 WebAPI。本次建立的客戶與處理紀錄保留在範例資料庫中；附件已由驗證步驟刪除。重設整組資料的方式見第 9.11 節。

### 9.8 port 被占用或 Windows 保留時如何調整

主機 port 的修改需要同時反映在 Compose 與應用程式。以下示範四個服務都換 port；只需改某一個時，保留其他預設值即可。

若這個 Compose project 已啟動過，先停止相關範例；確認本次範例資料可以清除後，使用第 9.11 節的 `down -v` 重設。這會清除整個 `dotnet-gcp-sdk-samples` 的服務資料。repo 的映像有匿名 volume，改 port 直接重建容器可能沿用舊 volume，因此先清除再啟動。

在負責 Compose 的 PowerShell 設定主機 port：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
$env:CLOUD_SDK_SAMPLES_VAULT_PORT = '18201'
$env:CLOUD_SDK_SAMPLES_SQLSERVER_PORT = '11434'
$env:CLOUD_SDK_SAMPLES_KAFKA_PORT = '19093'
$env:CLOUD_SDK_SAMPLES_RUSTFS_PORT = '19001'

docker compose config --quiet
docker compose up -d --wait vault sqlserver kafka rustfs
```

在執行 Console／WebAPI 的 PowerShell，設定對應的應用程式端點：

```powershell
$env:Cloud__Kms__OnPremise__Address = 'http://127.0.0.1:18201'
$env:Cloud__Sql__OnPremise__Server = '127.0.0.1,11434'
$env:Cloud__PubSub__OnPremise__BootstrapServers = '127.0.0.1:19093'
$env:Cloud__Storage__OnPremise__Endpoint = 'http://127.0.0.1:19001'
```

再依第 9.6、9.7 節執行。新開的 PowerShell 視窗不會繼承另一個已開啟視窗中的設定；Rider 也要使用相同端點或在啟動設定中提供。

Kafka 的 `CLOUD_SDK_SAMPLES_KAFKA_PORT` 會同時調整 port mapping 與 advertised listener；只改 port mapping 會讓 client 在讀取 broker metadata 後連到舊位址。

若只有 WebAPI 的 5180 被占用，可在其專案目錄執行：

```powershell
dotnet run --no-build --launch-profile OnPremise -- --urls http://127.0.0.1:5181
```

並將 API 驗證視窗的 `$baseUrl` 改為 `http://127.0.0.1:5181`。

要恢復服務的預設 port，同樣先停止應用程式、確認可清除資料並重設 Compose，再移除本工作階段的 port 覆寫，重新啟動：

```powershell
Remove-Item Env:CLOUD_SDK_SAMPLES_VAULT_PORT,Env:CLOUD_SDK_SAMPLES_SQLSERVER_PORT,Env:CLOUD_SDK_SAMPLES_KAFKA_PORT,Env:CLOUD_SDK_SAMPLES_RUSTFS_PORT -ErrorAction SilentlyContinue
Remove-Item Env:Cloud__Kms__OnPremise__Address,Env:Cloud__Sql__OnPremise__Server,Env:Cloud__PubSub__OnPremise__BootstrapServers,Env:Cloud__Storage__OnPremise__Endpoint -ErrorAction SilentlyContinue
```

### 9.9 執行 samples 的 Testcontainers 整合測試

#### 9.9.1 確認測試範圍與容器來源

| 驗證方式 | 服務的建立方式 | 連接埠與資料 |
| --- | --- | --- |
| 手動執行 Console／WebAPI | 使用本節的 Compose 服務 | 固定主機 port，資料留在該組範例容器 |
| samples 整合測試 | Testcontainers 自行建立 SQL Server、PostgreSQL、Kafka、Vault、RustFS | 動態主機 port，測試程式自行初始化獨立資料並清理 |

整合測試不讀取 `compose.company.yaml`，也不直接使用 Compose 的固定連接埠或既有資料。可以停止手動執行的應用程式與 Compose 服務來釋放資源，但不必為了執行測試預先啟動 Compose。

PostgreSQL 在這裡用來驗證 PostgreSQL repository，並不連線實際 Cloud SQL，也不需要 ADC。這份指南的測試對象是 `samples/CompanyName.Com.Dotnet.Cloud.Samples.sln`；SDK 的 `tests/` 方案另有自己的環境與映像要求。

#### 9.9.2 公司映像來源與 Ryuk

公司允許經核准代理取得原始映像時，沿用第 9.2.2 節的設定。若公司提供保留原有路徑的 Docker Hub 鏡像，可在執行測試的 PowerShell 指定前綴：

```powershell
$env:TESTCONTAINERS_HUB_IMAGE_NAME_PREFIX = 'registry.example.internal/dockerhub/'
```

請換成公司實際前綴，並確認 PostgreSQL 等映像的路徑規則相符。這個選項只處理 Docker Hub 映像，`mcr.microsoft.com` 的 SQL Server 不會因此改名。[Testcontainers 映像名稱設定](https://dotnet.testcontainers.org/custom_configuration/#automatically-modify-docker-hub-image-names)

SQL Server 可由公司核准的代理以原始名稱供應；若公司提供的是內部 registry 名稱，可先拉取相同內容並建立測試所需的本機 tag。例如：

```powershell
docker pull registry.example.internal/mcr/mssql/server:2022-CU27-ubuntu-22.04
if ($LASTEXITCODE -ne 0) { throw '公司 SQL Server 映像取得失敗。' }
docker tag registry.example.internal/mcr/mssql/server:2022-CU27-ubuntu-22.04 mcr.microsoft.com/mssql/server:2022-CU27-ubuntu-22.04
docker image inspect mcr.microsoft.com/mssql/server:2022-CU27-ubuntu-22.04 --format '{{.Id}}'
```

上述內部路徑均為範例，需使用公司核准、與指定版本內容一致的映像。Testcontainers 目前的預設缺少映像才下載策略可使用已存在的本機映像；實際嘗試下載的名稱仍以測試 log 為準。

Ryuk 也要能啟動。公司使用不同名稱或離線只載入核准的 tag 時，可明確指定：

```powershell
$env:TESTCONTAINERS_RYUK_CONTAINER_IMAGE = 'registry.example.internal/dockerhub/testcontainers/ryuk:0.14.0'
```

若載入的是保留原始 tag 的離線檔，則指定 `testcontainers/ryuk:0.14.0`，並先用 `docker image inspect` 確認該名稱存在。選擇的映像內容應與第 9.2.1 節的版本一致。

另可檢查 `%USERPROFILE%\.testcontainers.properties` 是否有舊的 `docker.host`、`docker.context` 或映像設定。本機 Docker Desktop 的標準路徑先使用預設探索；不要把其他開發環境的 Docker socket 或遠端主機設定直接帶入本次驗證。

#### 9.9.3 先建置，再啟用測試

先在 `samples/` 建置要測試的 Debug 產物：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
dotnet build .\CompanyName.Com.Dotnet.Cloud.Samples.sln
if ($LASTEXITCODE -ne 0) { throw 'samples 建置失敗，先處理建置錯誤。' }
```

預設的測試模式不啟用容器測試。確認本工作階段沒有設為 `true` 的 `CLOUD_SDK_INTEGRATION_TESTS`，再執行：

```powershell
dotnet test .\CompanyName.Com.Dotnet.Cloud.Samples.sln --no-build
```

不需要 Docker 的測試應通過，容器整合測試顯示 Skipped／略過。這個結果只能證明預設模式，尚未完成地端容器整合驗證。

接著確認 Docker Server 可用，再使用 repo 的 runsettings 明確啟用：

```powershell
docker version
dotnet test .\CompanyName.Com.Dotnet.Cloud.Samples.sln --no-build --settings .\WebApiSample\integration.runsettings --logger 'console;verbosity=normal'
if ($LASTEXITCODE -ne 0) { throw 'samples 整合測試失敗，請查看失敗案例與容器 log。' }
```

`integration.runsettings` 將 `CLOUD_SDK_INTEGRATION_TESTS` 設為 `true`，同時啟用 WebApiSample 與 SqlSample 的容器測試。成功條件是容器測試實際執行並通過；不要用所有容器案例都被略過的輸出作為驗證通過紀錄。測試數量會隨原始碼版本改變。

記憶體不足時，可依測試專案分開執行，並保留相同的啟用設定：

```powershell
dotnet test .\SqlSample.IntegrationTests\SqlSample.IntegrationTests.csproj --no-build --settings .\WebApiSample\integration.runsettings

dotnet test .\WebApiSample\WebApiSample.IntegrationTests\WebApiSample.IntegrationTests.csproj --no-build --settings .\WebApiSample\integration.runsettings
```

兩個命令各自確認結束代碼。測試失敗時需要更多診斷，可對失敗的專案加上 `--logger 'console;verbosity=detailed'`。啟用後 Docker 不可用應顯示失敗，不會自動以略過視為通過。

### 9.10 在 Rider 執行地端範例與整合測試

1. 開啟第 8 節的 samples 方案，先完成 Build Solution。
2. 手動範例所需的 Compose 服務保持 healthy。
3. 在啟動組態選擇要執行的專案與 `OnPremise` launch profile；WebAPI 的啟動專案是 `WebApiSample.Rs`。
4. 若更換了端點，將與 PowerShell 相同的有效設定提供給該啟動組態。IDE 不會自動取得另一個已開啟 PowerShell 視窗中新設定的環境變數。
5. 要執行整合測試時，在 Rider 的測試設定選用 `samples/WebApiSample/integration.runsettings`，重新探索測試，再執行。
6. 公司 registry 的 Testcontainers 設定也要在 IDE 測試處理序中生效；可依公司做法配置 IDE 環境或使用者的 `.testcontainers.properties`。

驗證尚未完成前，優先保留命令列的建置、啟用整合測試結果與 API 往返紀錄，方便區分 IDE 設定與服務本身的問題。

### 9.11 停止、保留與重設驗證環境

先在範例／WebAPI 視窗按 Ctrl+C。接著在 `samples/` 選擇需要的操作。

**暫停服務、保留容器：**

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
docker compose stop vault sqlserver kafka rustfs
```

再次使用時，執行相同的 `up -d --wait ...`。SQL Server、Kafka 與 RustFS 的既有容器／volume 資料通常仍在，但 Vault 是 dev mode，重啟會重新產生 key；先前 SQL 資料中的 Vault 密文將無法以新 key 解密。

**重設整組本機範例：** 確認資料可清除後執行：

```powershell
docker compose down -v
```

它移除 project `dotnet-gcp-sdk-samples` 的容器、network 與其掛載的 volume；所有範例的本機資料都會被清除，映像仍保留。採用 `compose.company.yaml` 時，命令改為：

```powershell
docker compose -f .\compose.yaml -f .\compose.company.yaml down -v
```

範例環境不保存正式資料；需要從頭重做整套驗證時，重設後再啟動，可避免 Vault 新 key 與舊資料的密文不一致。清除範圍限於這個 Compose project，不使用全域 prune。

Testcontainers 正常結束時會清理自己建立的測試資源，Compose 的 `down -v` 不負責清除另一組 Testcontainers 容器。第 9.7 節產生的本機下載檔放在顯示的暫存目錄，確認完成後可自行刪除該次目錄。

## 10. 後續修改或搬移原始碼

### 10.1 修改 src/ 後

samples 使用 SDK 套件，因此修改 `src/` 後需要重新打包：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'
dotnet msbuild .\eng\pack-local.proj
```

成功後再建置範例：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
dotnet build .\CompanyName.Com.Dotnet.Cloud.Samples.sln
```

若只修改 samples 的應用程式程式碼，通常直接建置 samples 即可。

### 10.2 搬移整份 repo 後

1. 更新 PowerShell 的工作目錄與 Rider 的方案路徑。
2. 確認根目錄與 samples 的兩份 `nuget.config` 都在新位置。
3. 在新根目錄重新執行 SDK 還原與打包。
4. 在新的 `samples/` 建置方案。

若搬移時一起帶入舊的 `obj/`，而錯誤仍指向舊目錄，可在新位置強制重建還原資訊：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk'
dotnet restore .\CompanyName.Com.Dotnet.Cloud.slnx --force --force-evaluate --no-http-cache
```

還原成功後重新打包。若範例仍有舊路徑，再於 `samples/` 執行：

```powershell
Set-Location 'D:\Temp\dotnet-gcp-sdk\samples'
dotnet restore .\CompanyName.Com.Dotnet.Cloud.Samples.sln --force --force-evaluate --no-cache
```

## 11. 常見錯誤與處理方式

| 錯誤或現象 | 判讀 | 處理方式 |
| --- | --- | --- |
| `NU1801`／`NU1301`，指出 `artifacts/packages` 不存在 | 本機 SDK 套件來源尚未產生，或路徑不正確 | 核對 `samples/nuget.config` 的相對路徑，在根目錄執行打包工具 |
| `NU1101`，找不到 `CompanyName.Com.Dotnet.Cloud.*` | 尚未打包、版本規則不符，或 SDK 對應到錯誤來源 | 確認 `sdk-local`、對應規則，以及 `0.1.0-local.*` 套件產物 |
| `NU1101`，找不到第三方套件 | 公司來源無法提供套件，或來源對應規則不完整 | 核對根目錄公司來源與 `*` 規則，再確認公司來源的套件供應 |
| `NU1301`，伴隨 XML／`DTD is prohibited` | 可能將 Nexus 瀏覽頁設為來源，或收到登入／錯誤網頁 | 改用公司提供的套件 API 網址，確認認證及服務回應；`/#browse/` 是網頁路徑 |
| `NU1102`，顯示要求版本與 `Nearest version` | 來源有套件名稱，但無法提供要求範圍內的版本 | 先排除本機 HTTP 快取，再請公司管理員確認代理、快取或審核狀態 |
| `MSB3073`，打包命令回傳結束代碼 1 | 外層工具回報子命令失敗 | 往上找第一個具體的 NuGet 或建置錯誤，先處理該原因 |
| SDK 找不到、選到不符合預期的版本 | SDK 未安裝、工作目錄錯誤，或 IDE／PATH 使用其他 dotnet | 檢查兩份 `global.json`、`dotnet --list-sdks`、`dotnet --version`；必要時用 `where.exe dotnet` 查安裝位置 |
| 命令列成功、Rider 仍失敗 | IDE 可能仍保留舊路徑、SDK 或還原狀態 | 依第 8 節重新載入、還原與核對 SDK |
| Docker 只有 Client、無法連 Server | Engine 尚未啟動、模式不符或 Docker 連線設定指向其他環境 | 依第 9.1 節確認 Docker Desktop、Linux containers 與有效連線設定 |
| `docker compose pull` 失敗、無權限或找不到映像 | 容器來源與 NuGet 是不同設定，或公司鏡像名稱／版本不符 | 依第 9.2 節確認 Docker 代理、registry 登入及指定映像 |
| port already allocated／存取 socket 被禁止 | 主機 port 被占用或位於 Windows 保留範圍 | 依第 9.3、9.8 節核對占用，成對調整 Compose 與應用程式端點 |
| `up --wait` 失敗、服務 unhealthy | 服務或初始化腳本尚未完成，可能為記憶體、映像或資源建立問題 | 先看 `compose ps -a` 與該服務的 `logs --tail 100`，再依第 9.5 節確認資源 |
| WebAPI `/health` 正常，建立客戶卻失敗 | health 主要觀察 Pub/Sub，沒有完成 SQL／Vault 的實際往返 | 依第 9.7 節檢查回應信封、SQL app 權限、Vault key/token 與應用程式 log |
| Kafka 能連 bootstrap，後續 broker 連線失敗 | broker metadata 的 advertised 位址無法從 Windows 連線 | 同時核對 bootstrap 與 advertised listener；換 port 使用 repo 提供的 Compose 變數 |
| 重啟 Vault 後舊客戶資料無法解密 | dev mode 重新建立 key，舊密文仍留在 SQL Server | 確認本機測試資料可清除後，依第 9.11 節重設整組範例 |
| 整合測試全部 Skipped／略過 | 尚未明確啟用容器測試，或 IDE 尚未重新探索 | 依第 9.9 節帶入 runsettings，確認容器案例實際執行 |
| Testcontainers 啟動 Ryuk／映像失敗 | 測試的映像來源未設定、清理工具映像不可取得，或 Docker 端點不符 | 檢查測試 log 的實際映像名稱、第 9.9 節設定與本機 Docker 連線 |

`NU1101`、`NU1102` 與 `NU1301` 的詳細定義可參考 [Microsoft NuGet 錯誤說明](https://learn.microsoft.com/en-us/nuget/reference/errors-and-warnings)。

遇到錯誤時，先記錄第一個具體錯誤、套件名稱、要求版本、來源回報版本，以及執行時的目錄。多個專案可能因同一個相依套件失敗，錯誤數量不一定代表不同問題。

## 12. 設定完成後可直接複製的建置流程

執行前，先完成兩份 `nuget.config` 的公司來源設定，並將下方 `$repoPath` 改為實際位置。此流程會在任一個 .NET 階段失敗時停止，適用於 Windows PowerShell 5.1 與 PowerShell 7。它完成建置；地端服務、API 往返與整合測試另依第 9 節逐項驗證。

```powershell
$repoPath = 'D:\Temp\dotnet-gcp-sdk'

Set-Location -LiteralPath $repoPath -ErrorAction Stop

# 根目錄應使用 .NET 10 SDK。
$sdkVersion = dotnet --version
if ($LASTEXITCODE -ne 0 -or $sdkVersion -notmatch '^10\.0\.') {
    throw '根目錄未選到 .NET 10 SDK，請先確認安裝版本與 global.json。'
}
Write-Host "SDK 打包使用：$sdkVersion"

# 先確認 SDK 方案的所有相依套件可從公司來源還原。
dotnet restore .\CompanyName.Com.Dotnet.Cloud.slnx --no-http-cache --force-evaluate
if ($LASTEXITCODE -ne 0) {
    throw 'SDK 方案還原失敗，請先處理 NuGet 來源或套件版本錯誤。'
}

# 產生本機 SDK 套件，並自動還原 samples。
dotnet msbuild .\eng\pack-local.proj
if ($LASTEXITCODE -ne 0) {
    throw 'SDK 打包或 samples 還原失敗，請查看前面的第一個具體錯誤。'
}

Set-Location -LiteralPath (Join-Path $repoPath 'samples') -ErrorAction Stop

# samples 應使用 .NET 8 SDK。
$sdkVersion = dotnet --version
if ($LASTEXITCODE -ne 0 -or $sdkVersion -notmatch '^8\.0\.') {
    throw 'samples 未選到 .NET 8 SDK，請先確認安裝版本與 samples/global.json。'
}
Write-Host "範例建置使用：$sdkVersion"

dotnet build .\CompanyName.Com.Dotnet.Cloud.Samples.sln
if ($LASTEXITCODE -ne 0) {
    throw 'samples 建置失敗，請查看前面的建置錯誤。'
}

Write-Host 'SDK 還原、打包與 samples 建置皆已完成。'
```

## 13. 完成條件

### 13.1 建置完成

依序確認以下結果：

- [ ] 根目錄的 `dotnet --version` 為 `10.0.xxx`。
- [ ] samples 的 `dotnet --version` 為 `8.0.xxx`。
- [ ] 根目錄的有效套件來源為公司 NuGet。
- [ ] samples 的有效套件來源包含公司 NuGet 與 `sdk-local`。
- [ ] SDK 方案 `dotnet restore` 的結束代碼為 `0`。
- [ ] `dotnet msbuild .\eng\pack-local.proj` 的結束代碼為 `0`。
- [ ] `artifacts/packages/` 中有同一批版本的六個 SDK 套件。
- [ ] samples 方案 `dotnet build` 的結束代碼為 `0`。
- [ ] 若使用 Rider，開啟新位置的 samples 方案並確認 IDE 建置成功。

### 13.2 地端手動驗證完成

- [ ] Windows 主機具備可用的 WSL 2 與 Linux Docker Engine，`docker version` 同時列出 Client 與 Server。
- [ ] 公司來源可以提供所需的容器映像；Compose 有效設定的映像與版本已確認。
- [ ] 四個服務的主機 port 與應用程式有效設定一致。
- [ ] Vault、SQL Server、Kafka、RustFS 為 healthy，且初始化資源檢查完成。
- [ ] 五個 Console 範例的預期結果完成，結束代碼均為 0；刻意示範的失敗情境有依說明判讀。
- [ ] WebAPI 建立、查詢客戶成功，Kms 解密內容與本次虛構輸入一致。
- [ ] 本次客戶有背景訊息處理紀錄，完成 Kafka 接收與資料庫寫入驗證。
- [ ] 附件可上傳、下載 SHA-256 相同、刪除後回應 `100003`。
- [ ] 不合法輸入回應預期的 `100001`。

### 13.3 自動整合驗證完成

- [ ] Testcontainers 所需的 SQL Server、PostgreSQL、Kafka、Vault、RustFS 與 Ryuk 映像可取得。
- [ ] samples 建置成功後，使用 `WebApiSample/integration.runsettings` 明確啟用測試。
- [ ] 容器整合測試實際執行並通過，結束代碼為 0；保留有執行案例的測試輸出。
- [ ] 確認手動 Compose 與 Testcontainers 的資料、連接埠及清理方式不同。
- [ ] 依需要停止或重設本機範例，並記錄 Vault dev mode 重啟後舊密文失效的限制。

本次對話已回報 SDK 還原與打包完成；這份文件的地端設定依 repo 的 Compose、profile、測試程式與官方工具文件核對。公司 Windows 實機的服務啟動、API 往返與整合測試結果，仍需依以上清單執行及記錄。

相關 repo 文件：[使用說明](../usage/README.md)、[Windows 開發機設定](windows.md)、[本機打包工具](../../eng/pack-local.proj)。
