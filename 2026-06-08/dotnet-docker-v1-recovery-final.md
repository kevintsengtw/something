# .NET Docker Image v1 復原作業手冊（整合版）

> 環境：Windows + Docker Desktop + PowerShell 7。所有指令可直接執行。
> 比對一律不使用 git，改用 WinMerge / Beyond Compare / `fc` / `Compare-Object`。
> 資料存取為 Dapper / ADO.NET 原生（非 EF Core），手冊不假設 ORM、不假設 migrations。

---

## 前置需求（環境與工具）

開工前先備齊以下環境與工具，並用本節最後的檢查指令確認。

### 必備環境

| 項目 | 說明 |
|---|---|
| Windows 10 / 11 | 作業系統 |
| Docker Desktop | 需啟動中。預設 Linux container 模式（WSL2 backend）；若 v1 是 Windows container 再切換 |
| PowerShell 7（pwsh） | 本手冊所有指令以此為準（內建 `Get-FileHash`/`Compare-Object`/`ConvertFrom-Json`） |
| .NET SDK | 先裝一個近期版本讓 global tools 可用；recon 後再依 v1 的 target framework 補裝對應 band，並用 `global.json` 釘版本 |
| 磁碟空間 | 足以容納 image 解出的 `/app`、`decompiled-v1`、`decompiled-current` 與 DB 匯出，數 GB 起跳 |

```powershell
winget install Docker.DockerDesktop
winget install Microsoft.PowerShell
winget install Microsoft.DotNet.SDK.8        # band 依 v1 實際版本調整（如 .SDK.9 / .SDK.10）
```

### 必裝工具（dotnet global tools）

```powershell
dotnet tool install --global ilspycmd                          # 反組譯（路線 B/C）
dotnet tool install --global sourcelink                        # 檢查 PDB / SourceLink（路線 A）
dotnet tool install --global Microsoft.DotNet.ApiCompat.Tool   # 選配：先比 API 表面縮小範圍
```

### 比對工具（擇一 GUI；單檔用 Windows 內建）

| 工具 | 取得 | 用途 |
|---|---|---|
| WinMerge | `winget install WinMerge.WinMerge` | 資料夾/檔案比對，有 CLI `WinMergeU.exe` |
| Beyond Compare | 官網（付費） | 資料夾比對 |
| JetBrains Rider / dotPeek | 你已安裝 Rider | Directory Compare / GUI 反組譯 |
| `fc.exe`、`Compare-Object` | Windows / PowerShell 內建 | 單檔比對，免安裝 |

本手冊比對不使用 git。git 只在第 7 步寫 v2 label 時選配出現，且取自新 repo。

### 資料庫工具（依你的引擎擇一安裝）

| DB 引擎 | 安裝 | 提供的指令 |
|---|---|---|
| SQL Server | `dotnet tool install --global Microsoft.SqlPackage`；`Install-Module SqlServer -Scope CurrentUser` | `SqlPackage`、`Invoke-Sqlcmd`（`bcp` 隨 mssql-tools） |
| PostgreSQL | 安裝 PostgreSQL client | `pg_dump`、`psql` |
| MySQL / MariaDB | 安裝 MySQL client | `mysqldump`、`mysql` |

另需一個可丟棄的 DB 執行個體來還原 seed，可用 Docker 直接起一個對應引擎的容器。

### 開工前要先準備好的素材

```text
1. v1 image 本身（在 Docker Desktop 中，或一份可 docker load 的 tar）
2. 目前版控的原始碼（現在的新 GitLab repo）
3. 一份具代表性的 DB seed（SQL 腳本或備份），且可重複還原
4. 幾組真實請求 / 輸入（建議取自 production 歷史 log），當作行為基準的 input
```

### 開工前檢查（一次跑完）

```powershell
$PSVersionTable.PSVersion          # 期望 7.x
docker version                     # daemon 要回應（Docker Desktop 啟動中）
dotnet --info                      # 確認已安裝的 SDK
ilspycmd --version
sourcelink --help | Select-Object -First 1
Get-Command WinMergeU.exe -ErrorAction SilentlyContinue   # 用 WinMerge 才需要
```

---

## 0. 情境與前提

### 0.1 情境（可直接給團隊看）

- 有一個正在（準）production 使用的 .NET 專案 `docker image v1`。
- build 出 v1 的原始碼、commit、編譯/打包機、Dockerfile、CI 設定都已遺失。
- 舊 GitLab 已消失，現在的 GitLab 是全新建立的，與 v1 沒有任何版控歷史關聯。
- 目前版控的原始碼是從別處找回來的，不完整。與 v1 推測差「約兩個 commit」，但差異可能是雙向的。
- 專案使用資料庫，資料存取以 Dapper / ADO.NET 為主，沒有單元測試。
- 目標：安全地推回 v1 的行為差異、補回目前版控，再打包出可信的 `docker image v2`。

### 0.2 硬前提（所有步驟不得違反）

1. 唯一可信來源是 `docker image v1`，其餘一律視為不存在。
2. 因新舊 GitLab 無關聯，沒有可用的 commit 層級操作（無 diff / cherry-pick / bisect / 還原）。能做的只有兩種：
   - artifact 層級比對：反組譯出的檔案、設定檔、內嵌 SQL 字串。
   - 行為層級比對：v1 實際跑出來的 API 回應、DB 結果、輸出。
3. 驗收看的是行為一致，不是二進位一致。build 環境全新，且 .NET build 不可 bit-reproducible，追求二進位相同沒有意義。
4. v1 是行為基準，不保證是正確答案。每筆差異都要判斷它是刻意的還是 bug，不要把舊 bug 一起補回去。

### 0.3 需你自行替換的佔位

image 名稱 `your-image:v1`、容器工作目錄、對外 port、各專案 dll 名稱（`Your.Api.dll` 等）、DB 連線與 seed。

---

## 1. 保全 v1 image（先做）

```powershell
docker images | Select-String 'your-image'
docker inspect your-image:v1 --format '{{.Id}}'

docker save your-image:v1 -o your-image-v1.tar
Get-FileHash .\your-image-v1.tar -Algorithm SHA256 | Tee-Object -FilePath .\your-image-v1.tar.sha256
# 還原：docker load -i .\your-image-v1.tar
```

---

## 2. 解剖 image（決定走哪條路）

在打開前我們不知道裡面是什麼，這一步的輸出決定第 4 步走哪條路。

```powershell
# OS / 架構（windows 代表 Windows container，路徑改成 C:\app）
docker inspect your-image:v1 --format '{{.Os}}/{{.Architecture}}'

# 工作目錄 / 啟動方式 / 環境變數 / Labels
$cfg = (docker inspect your-image:v1 | ConvertFrom-Json)[0].Config
$cfg | Format-List WorkingDir, Entrypoint, Cmd, Env, Labels

# image 疊法；若看到 COPY 整包 source 或 base 是 dotnet/sdk，原始碼可能就在 layer 裡
docker history --no-trunc your-image:v1
```

把檔案撈到本機（路徑依 WorkingDir）：

```powershell
docker create --name v1tmp your-image:v1
docker cp v1tmp:/app .\v1-app          # Windows container 改 v1tmp:C:\app
docker rm v1tmp
Get-ChildItem .\v1-app -Recurse -File | Select-Object -ExpandProperty FullName
```

逐項判斷：

```powershell
# A. 原始碼有沒有被一起包進去（最省事的情況）
Get-ChildItem .\v1-app -Recurse -Include *.cs, *.csproj -ErrorAction SilentlyContinue

# B. publish 模式
Get-ChildItem .\v1-app -Filter *.dll | Where-Object { $_.Name -match 'coreclr|System.Private.CoreLib' }
#   有 coreclr/System.Private.CoreLib：self-contained（自家 dll 仍是正常 IL，可反組譯）
#   檔案很少、只有一個大 native exe：single-file（要先拆 bundle）
#   只有一個 native exe、沒有你的 .dll、也沒 coreclr：NativeAOT（無 IL，反組譯走不通）
#   大量 Microsoft.*/System.* dll + 用 dotnet App.dll 啟動：framework-dependent（最好比對）

# C. PDB
Get-ChildItem .\v1-app -Recurse -Filter *.pdb

# D. target framework / runtime 版本
Get-Content .\v1-app\*.runtimeconfig.json -Raw

# E. 套件版本（順便確認有沒有 Dapper / Microsoft.Data.SqlClient / System.Data.* 等資料存取套件）
((Get-Content .\v1-app\*.deps.json -Raw) | ConvertFrom-Json).libraries.PSObject.Properties.Name

# F. commit 線索（即使遺失也只是確認身分，與新 GitLab 無關）
$cfg.Labels
(Get-Item .\v1-app\Your.Api.dll).VersionInfo | Format-List ProductVersion, FileVersion

# G. 有沒有被混淆：用 ILSpy / dotPeek 開一個 dll 看 type 名稱是不是 a/b/c 或亂碼
```

---

## 3. 路線決策表

| 解剖結果 | 路線 | 做法 |
|---|---|---|
| layer 內有 `*.cs`/`*.csproj`，或 PDB 內嵌原始碼 | A | 直接救回原始碼，可能不必反組譯 |
| 正常 IL dll、未混淆、framework 版本可逼近 | B | 反組譯 v1 與 current 後比對 |
| single-file | B' | 先拆 bundle 取出 managed assembly，再走 B |
| NativeAOT／重度混淆／framework 落差太大 | C | 反組譯不可信，v1 只當行為基準（第 5 步），案例需更廣 |

不論走哪條路，第 5 步的行為基準都要建（含 DB），它是共同的安全網。

---

## 4. 找差異

### 路線 A：原始碼救回

```powershell
# 原始碼在 layer / 已撈到 .\v1-app（或容器內 /src）
# 用 GUI 比對目前 repo 與救回的原始碼：WinMerge / Beyond Compare / Rider Directory Compare
WinMergeU.exe /r /u .\current-repo .\v1-app    # 若已安裝 WinMerge

# PDB 內嵌原始碼（先確認）
dotnet tool install --global sourcelink
sourcelink print-documents .\v1-app\Your.Api.pdb
sourcelink print-json      .\v1-app\Your.Api.pdb
#   只有 URL + commit：原始碼沒內嵌；build 當年開過 EmbedAllSources：可取出內嵌來源
```

救回後仍要走第 5 到 6 步驗證，救回的不一定就是 build v1 的那一版。

### 路線 B：反組譯比對

```powershell
# B-1. 逼近當年環境（降雜訊關鍵：編譯器/SDK 版本不同會產生大量假差異）
#   依 runtimeconfig.json 的版本，在 repo 根目錄放 global.json 釘住 SDK：
#   { "sdk": { "version": "8.0.xxx", "rollForward": "disable" } }

# B-2. 反組譯 v1 的自家 dll（不要碰 System.*/Microsoft.*）
dotnet tool install --global ilspycmd
New-Item -ItemType Directory -Force .\decompiled-v1 | Out-Null
ilspycmd -p -o .\decompiled-v1\Api            .\v1-app\Your.Api.dll
ilspycmd -p -o .\decompiled-v1\Application    .\v1-app\Your.Application.dll
ilspycmd -p -o .\decompiled-v1\Domain         .\v1-app\Your.Domain.dll
ilspycmd -p -o .\decompiled-v1\Infrastructure .\v1-app\Your.Infrastructure.dll

# B-3. 用目前版控 build 一份，再用同一套反組譯器反組譯
dotnet restore
dotnet publish -c Release -o .\publish-current
New-Item -ItemType Directory -Force .\decompiled-current | Out-Null
ilspycmd -p -o .\decompiled-current\Api            .\publish-current\Your.Api.dll
ilspycmd -p -o .\decompiled-current\Application    .\publish-current\Your.Application.dll
ilspycmd -p -o .\decompiled-current\Domain         .\publish-current\Your.Domain.dll
ilspycmd -p -o .\decompiled-current\Infrastructure .\publish-current\Your.Infrastructure.dll

# B-4. 比對（用 GUI，不用 git）
WinMergeU.exe /r /u .\decompiled-current .\decompiled-v1
#   也可用 Beyond Compare / Rider Directory Compare / dotPeek
```

Dapper / ADO 的 SQL 多半是 inline 字串，反組譯後字串會原樣保留，所以 SQL 邏輯差異在 diff 裡很好認，這是相對 EF（查詢在 runtime 由 LINQ 翻譯）比較佔便宜的地方。比對時特別看 SQL 字串：`WHERE` 條件、`JOIN`、欄位增減、狀態過濾、`ORDER BY`、參數化方式。

### 設定檔比對（差異不一定在 C#）

```powershell
# 比對 v1 的設定檔與目前 repo（聚焦 appsettings*.json / *.config / *.xml）
fc.exe .\v1-app\appsettings.json .\current-repo\src\Your.Api\appsettings.json
#   重點：ConnectionStrings、FeatureFlags、Timeout、Batch size、endpoint、某流程啟用/停用
```

### 差異清單（先整理再動手，不要看到 diff 就全補）

| 差異位置 | v1 行為 | current 行為 | 風險 | 刻意 or bug | 驗證方式 | 是否補回 |
|---|---|---|---|---|---|---|
| (待填) | | | | | | |

---

## 5. 建立行為基準（Golden Master）

把 v1 當標準答案：固定輸入，記錄 v1 的輸出，之後 current/v2 必須符合。建議從 production 歷史 log 或真實資料挑案例，覆蓋面比較好。

### 5.1 API 層

```powershell
# 啟動 v1（port 依 image 實際 EXPOSE / ASPNETCORE_URLS；net8+ 容器內預設 8080）
docker run -d --name v1-run -p 5001:8080 your-image:v1

curl.exe -s -X POST "http://localhost:5001/api/xxx" `
  -H "Content-Type: application/json" --data "@request.json" -o response-v1.json
# 或：Invoke-RestMethod -Method Post -Uri http://localhost:5001/api/xxx `
#       -ContentType 'application/json' -InFile .\request.json | ConvertTo-Json -Depth 50 | Out-File response-v1.json

# 對 current / v2 打同一份請求
docker run -d --name cur-run -p 5002:8080 your-image:current
curl.exe -s -X POST "http://localhost:5002/api/xxx" `
  -H "Content-Type: application/json" --data "@request.json" -o response-current.json

# 比對（不用 git）
fc.exe .\response-v1.json .\response-current.json
# 或：Compare-Object (Get-Content .\response-v1.json) (Get-Content .\response-current.json)
```

時間、GUID、流水號這類非決定性欄位會造成假差異，比對前先把它們遮罩成固定值再比。

### 5.2 DB 層（Dapper / ADO，必做）

資料存取會直接寫 DB，光比 API response 不夠，要比 DB 副作用。流程與引擎無關：

```text
1. 還原一份已知 seed 到 DB（用你既有的 SQL 腳本 / 備份還原）
2. 跑 v1（連到這份 seed），匯出執行後的 DB 結果
3. 重新還原同一份 seed
4. 跑 current（連到這份 seed），匯出執行後的 DB 結果
5. 比對兩份匯出（新增/更新/刪除/狀態轉換/金額計算/錯誤紀錄）
```

匯出工具依 DB 引擎，手冊不假設引擎：

| DB 引擎 | 匯出/比對可用工具 |
|---|---|
| SQL Server | `SqlPackage`（匯出 .bacpac / .dacpac）、`bcp`、或 `Invoke-Sqlcmd` 把關鍵表 SELECT 成 CSV |
| PostgreSQL | `pg_dump`、`psql \copy` 出 CSV |
| MySQL / MariaDB | `mysqldump`、`SELECT ... INTO OUTFILE` |

```powershell
# 範例：把關鍵表匯成 CSV 後用 fc / Compare-Object 比對
# Invoke-Sqlcmd -ServerInstance . -Database AppDb -Query "SELECT * FROM Orders ORDER BY Id" | Export-Csv .\db-v1-orders.csv -NoTypeInformation
# ... 對 current 同樣匯出 db-current-orders.csv ...
fc.exe .\db-v1-orders.csv .\db-current-orders.csv
```

注意事項：

- v1 與 current 必須對相同 seed 的相同起點執行，建議用兩份獨立複本或每次重置。
- 沒有 EF migrations，schema 由 SQL 腳本或工具管理。若那兩個遺失 commit 含 schema 變更，v1 期望的 schema 可能與 current 不同，先確認兩邊 schema 一致再比資料。
- DB 比對也要遮罩非決定性欄位（自動編號、建立時間）。

### 5.3 批次 / worker 輸出比對（若有 console / worker / 批次才做）

```powershell
docker run --rm -v ${PWD}\input:/app/input -v ${PWD}\output-v1:/app/output      --env-file .\.env.v1      your-image:v1
docker run --rm -v ${PWD}\input:/app/input -v ${PWD}\output-current:/app/output --env-file .\.env.current your-image:current
WinMergeU.exe /r /u .\output-current .\output-v1     # 也比 log 與 exit code
```

### 5.4 案例優先順序（先挑高風險，不追求覆蓋率）

```text
1. 已知 v1 與 current 有差異的功能
2. production 最重要的 API / 批次流程
3. 金額計算 / 狀態轉換 / 查詢條件（Dapper SQL 差異常落在這）
4. 日期、時間、交易日判斷
5. DB 寫入與更新結果
6. 錯誤處理分支
7. 外部 API 回傳處理（需 stub 或 record/replay 以保證 deterministic）
8. Feature flag 影響的流程
```

### 5.5 日後常態化（選配）

要把基準固定成回歸測試，再包成簡單的測試專案，用 `HttpClient` 打已啟動的 container，DB 結果用 `Compare-Object`/`fc` 比 CSV。現階段存檔比檔已足以保護，不必一開始就建大型測試專案。

---

## 6. 把差異補回目前版控

```text
- 反組譯/救回的內容只拿來找差異，不要把反組譯碼直接貼回 repo
  （反組譯碼常有變數名不自然、async/LINQ 還原不佳、record/pattern 被降級等問題）。
- 回到目前 repo，用正常可維護的 C# 重寫該邏輯（含修正 Dapper SQL 字串）。
- 每補一處，就用第 5 步基準確認 current 對相同輸入的 API + DB 結果與 v1 一致。
- 逐筆判斷 v1 此行為是刻意還是 bug。
```

---

## 7. 打包 v2、驗證、保留 rollback

```powershell
dotnet clean
dotnet restore
dotnet build

# 打包；commit label 取自現在的新 repo（選配，純為日後可追溯，與 v1 無關）
docker build `
  --build-arg GIT_COMMIT=$(git rev-parse HEAD) `
  --build-arg GIT_BRANCH=$(git rev-parse --abbrev-ref HEAD) `
  -t your-image:v2 .
# 不想用 git 也可手動帶入或從 CI 變數取得 GIT_COMMIT / GIT_BRANCH
```

上線前檢查：

```text
[ ] v2 container 可啟動、health check 通過
[ ] v2 對相同請求的 API response 與 v1 一致
[ ] v2 對相同 seed 的 DB 結果與 v1 一致
[ ] v2 必要設定（含 ConnectionStrings 以外的 FeatureFlag/Timeout 等）與 v1 一致
[ ] log / error 行為無重大差異
[ ] 留著 v1 image（第 1 步的 tar）當 rollback
```

v2 多半會用較新的 base image，與 v1 在 runtime/base 層本來就不同，所以驗收只看行為是否一致。

---

## 8. 事後 traceability（選配）

避免重蹈覆轍，之後在 image 與 assembly 留下來源資訊。

Dockerfile 加 label：

```dockerfile
ARG GIT_COMMIT
ARG GIT_BRANCH
LABEL org.opencontainers.image.revision=$GIT_COMMIT
LABEL org.opencontainers.image.source=$GIT_BRANCH
```

`.csproj` 寫入來源 metadata：

```xml
<PropertyGroup>
  <ContinuousIntegrationBuild>true</ContinuousIntegrationBuild>
  <PublishRepositoryUrl>true</PublishRepositoryUrl>
  <EmbedUntrackedSources>true</EmbedUntrackedSources>
  <DebugType>portable</DebugType>
  <!-- 視需要：<EmbedAllSources>true</EmbedAllSources> 讓 PDB 內嵌完整原始碼 -->
</PropertyGroup>
```

CI/CD 保留：publish output、build log、source commit、Dockerfile、image digest、SBOM。目標是以後能回答「這個 image 是哪個 commit、哪份 Dockerfile、哪些 build args、哪些 NuGet 版本 build 的」。

---

## 已知盲點

```text
1. multi-stage 的編譯階段不可見：docker history 只看得到最終 image 的 layer，
   build-time-only 的步驟（build 時 codegen、條件編譯）無法逆推，靠第 5 步行為比對把關。
2. 雙向分歧：current 與 v1 可能各有對方沒有的東西，不是單向「少兩個 commit」。
3. v1 可能本身有 bug：逐筆判斷要不要復原。
4. NativeAOT / 重度混淆：反組譯走不通時只能走純黑箱（路線 C）。
5. schema 來源不明：沒有 EF migrations，需自行確認 v1 與 current 的 DB schema 是否一致。
6. 二進位不可重現：驗收只看行為，不看二進位/IL 是否相同。
```

---

## 一頁速覽

```text
保全 v1 → 解剖 image 決定路線(A/B/C) → 找差異(反組譯比對，特別看 Dapper SQL 字串 + 設定檔)
→ 建行為基準(API + DB，遮罩非決定性欄位) → 用正常 C# 補回 → 逐筆驗證行為一致
→ 出 v2 → 行為再驗證 → 保留 v1 rollback → 補 traceability

原則：v1 是唯一可信的行為基準；新舊 GitLab 無關聯，只能做 artifact 與行為層級比對；
      驗收看行為一致，不看二進位一致。
```
