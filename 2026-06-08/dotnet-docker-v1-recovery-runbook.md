# .NET Docker Image v1 復原作業手冊

> 在原始碼、build commit、打包/編譯機器、CI 設定、Dockerfile 全部遺失，且專案沒有單元測試的情況下，
> 如何安全地從 `docker image v1` 反推出遺失的行為差異、補回目前版控，並打包出可信的 `docker image v2`。

---

## Part 0 — 前提與已知限制（最重要，先讀這段）

這份手冊的所有步驟都建立在以下「硬前提」上。它們是經過多次釐清後確認的事實，不是假設，後續任何決策都不得違反。

### 0.1 唯一可信來源

```text
docker image v1 是目前唯一可信、且接近 production 真實行為的來源。
其餘一切都不假設存在。
```

### 0.2 明確「不存在」的東西

下列全部當作**不可復原**，不要在任何步驟裡依賴它們：

```text
- 建立 v1 的原始 Git commit / branch（多年前 GitLab 升級失敗時遺失）
- GitLab server 端的 git object / reflog / dangling commit（機器當年已清空）
- 當年的編譯機、打包機（已不存在，現在是全新環境）
- 當年的 Dockerfile、build 參數、CI/CD 設定
- 既有的單元測試（專案沒有測試）
```

唯一邏輯上獨立、值得碰運氣問一句的，是「某位開發者多年前本機留下的舊 clone / 個人備份」——但這不是手冊的依賴項，有就賺到，沒有也不影響流程。

### 0.3 目前版控的狀態

```text
- 目前版控的原始碼是「從其他地方找回來的」，來源不明、不完整。
- 與 v1 的差異「大約兩個 commit」是推測，不是事實。
- 真實差異可能是「雙向分歧」：current 可能同時「缺 v1 有的」與「多了 v1 沒有的」。
  → 不要用「current = v1 減兩個 commit」這種單向心態處理。
```

### 0.4 成功的定義（關鍵觀念）

```text
成功 = 「對相同輸入，v2 觀察到的行為與 v1 一致」（行為一致）
失敗的目標 = 「重建出跟 v1 一樣的二進位 / IL」（二進位一致）← 徹底放棄
```

理由：build 機器與環境全新，且 .NET 的 build 本來就**不是 bit-for-bit 可重現**的（即使原始碼與 SDK 都對得上，timestamp、MVID、embedded path 仍會不同）。追求二進位一致只會浪費時間。

### 0.5 v1 image 在整個復原中的兩個角色

```text
角色一：指紋來源（fingerprint）
  → 從 image 讀出 target framework、套件版本、publish 模式，
    讓比對環境盡量逼近當年，降低雜訊。

角色二：行為 oracle（behavioral oracle）
  → v1 實際跑出來的輸入/輸出，是唯一的驗收標準。
```

### 0.6 一個必須記住的語意

```text
v1 是「行為基準」，不一定是「正確答案」。
v1 可能本身就帶著 bug；那兩個遺失 commit 之一也可能正是某個修正。
所以每一筆差異都要單獨判斷：「v1 這個行為是刻意的，還是 bug？」
不要在不知情下把舊 bug 一起「復原」回去。
```

---

## Part 1 — 核心策略

### 1.1 三個原則

```text
1. 不相信目前版控是完整的。
2. 不相信反組譯結果就等於原始碼（它只是差異線索）。
3. 把 docker image v1 當成唯一可信的行為基準（Golden Master）。
```

### 1.2 為什麼是 Characterization / Golden Master 測試

因為沒有單元測試，無法用「程式看起來補對了」來判斷安全。唯一可靠的方式是：把 v1 在固定輸入下的實際輸出記錄下來，當成標準答案，之後 current / v2 必須符合這份答案。這就是 Michael Feathers 的 Characterization Test（行為定性測試）/ Golden Master 手法。

```text
固定 input → 丟給 v1 → 保存 v1 的 response / DB 結果 / 輸出
            → 之後 current / v2 必須產生相同結果
```

---

## Part 2 — 整體流程總覽

```text
Phase 0  取得並保全 v1 image
Phase 1  image 解剖（reconnaissance）→ 決定走哪條路【全流程的真正起點】
Phase 2  依解剖結果分流：
           路線 A：原始碼直接救回（最理想）
           路線 B：反組譯差異比對
           路線 C：純黑箱行為復原（最困難）
Phase 3  建立行為基準（Golden Master / Characterization Tests）
Phase 4  整理差異清單、以正常 C# 人工補回目前版控
Phase 5  驗證 current 行為與 v1 一致
Phase 6  打包 v2 → 健康檢查 → 行為再驗證 → 保留 v1 rollback
Phase 7  事後補上 traceability，避免重蹈覆轍
```

**重點：在 Phase 1 完成之前，Phase 2 的三條路線都只是「分支」，不要預先投入。**

---

## Part 3 — 詳細操作步驟

### Phase 0：取得並保全 v1 image

目前連 image 都還沒拿到，所以第一件事是把它弄到手並備份，避免任何覆蓋或刪除。

```bash
# 1. 確認 image 存在、記錄身分
docker images | grep your-image
docker inspect your-image:v1 --format '{{.Id}}'
docker inspect your-image:v1 --format '{{index .RepoDigests 0}}'   # 若是從 registry 拉的

# 2. 匯出成 tar 永久備份，並記錄 checksum
docker save your-image:v1 -o your-image-v1.tar
sha256sum your-image-v1.tar | tee your-image-v1.tar.sha256

# 3. 還原方式（備查）
# docker load -i your-image-v1.tar
```

> 把 tar 與 sha256 放到安全位置（NAS / 物件儲存）。從現在起，v1 是唯一接近 production 真實行為的東西。

---

### Phase 1：image 解剖（reconnaissance）— 全流程的真正起點

目的：在打開 image 之前，我們完全不知道裡面是什麼（framework-dependent？self-contained？single-file？NativeAOT？有沒有 PDB？是不是 multi-stage？有沒有混淆？）。**解剖結果直接決定走哪條路。** 這一步大約十幾分鐘。

#### 1.1 image 層級資訊

```bash
# OS / 架構：windows 代表可能是 .NET Framework Windows container，layout 與工具不同
docker inspect your-image:v1 --format '{{.Os}}/{{.Architecture}}'

# Entrypoint / Cmd / WorkingDir / Env / Labels（決定 app 在哪、怎麼啟動）
docker inspect your-image:v1 --format '{{json .Config}}' | jq

# 還原 image 疊法；若看到 COPY 整包 source、或 base 是 dotnet/sdk → 原始碼可能就在 layer 裡
docker history --no-trunc your-image:v1
```

#### 1.2 把檔案撈出來（路徑依 `.Config.WorkingDir`，不一定是 /app）

```bash
docker create --name v1tmp your-image:v1
docker cp v1tmp:/app ./v1-app          # ← 依實際 WorkingDir 調整
docker rm v1tmp

ls -la ./v1-app
find ./v1-app -maxdepth 3 -type f | sort
```

#### 1.3 逐項判斷（這些答案組合成「走哪條路」）

```bash
# A. 有沒有原始碼被誤包進來（最理想）
find ./v1-app /src /build -name '*.cs' 2>/dev/null | head
#   → 有 .cs / .csproj → 可能直接救回原始碼（路線 A）

# B. publish 模式判斷
ls ./v1-app | grep -iE 'libcoreclr|coreclr|System.Private.CoreLib'
#   → 有：self-contained（你自己的 dll 仍是正常 IL，可反組譯）
#   → 沒有，但有大量 Microsoft.*/System.* dll + 用 `dotnet App.dll` 啟動：framework-dependent（最易比對）

#   檔案極少、只有一個很大的 native 執行檔：single-file（要先拆 bundle）
#   只有一個 native 執行檔、完全沒有你的 .dll、也沒有 coreclr：NativeAOT（無 IL，反組譯死路）

# C. 有沒有 PDB（可能含 SourceLink / 內嵌原始碼）
find ./v1-app -name '*.pdb'

# D. target framework / runtime 版本（決定能否逼近環境重建）
cat ./v1-app/*.runtimeconfig.json

# E. 套件確切版本（重建時用來逼近當年環境，降低反組譯雜訊）
cat ./v1-app/*.deps.json | jq '.libraries | keys'

# F. commit 線索（即使 commit 已遺失，至少能確認 image 身分）
docker inspect your-image:v1 --format '{{json .Config.Labels}}'
strings ./v1-app/Your.Api.dll | grep -iE 'commit|revision|repository|git'

# G. 是否被混淆：用 ILSpy 開一個 dll，看 type/member 名稱是否為 a/b/c 或亂碼
#    混淆會讓反組譯 diff 不可靠 → 偏向走純黑箱
```

#### 1.4 解剖結果 → 路線決策表

| 觀察到的狀況 | 走哪條路 | 說明 |
|---|---|---|
| image layer 裡有完整 `*.cs` / `*.csproj`，或 PDB 內嵌原始碼 | **路線 A** | 直接救回原始碼，可能根本不必反組譯 |
| 正常 IL dll（framework-dependent 或 self-contained），未混淆，framework 版本可逼近 | **路線 B** | 反組譯 v1 與 current，比對差異 |
| single-file | **路線 B'** | 先拆出 bundle 內的 managed assembly，再走 B |
| NativeAOT（無 IL）／重度混淆／framework 版本落差過大使 IL 比對失去意義 | **路線 C** | 反組譯不可信，只能把 v1 當純行為 oracle |

> 不論走哪條路，**Phase 3 的行為基準（Golden Master）都要做**，它是所有路線共同的安全網。

---

### Phase 2：依解剖結果分流

#### 路線 A — 原始碼直接救回（最理想）

**情況一：原始碼在 image layer 裡**

```bash
# 從 v1-app 或 /src 把原始碼複製出來，與目前版控做標準資料夾 / git 比對
git diff --no-index ./current-repo ./recovered-src > recovered-vs-current.diff
```

**情況二：PDB 內嵌原始碼（需先確認）**

```bash
dotnet tool install --global sourcelink
sourcelink print-documents ./v1-app/Your.Api.pdb     # 列出 PDB 內的文件
sourcelink print-json      ./v1-app/Your.Api.pdb     # 看 SourceLink 對應與 repo URL/commit
```

判讀：
- 若輸出只是「URL + commit hash」→ 原始碼**沒有**內嵌（SourceLink 只指回已遺失的 repo），只能拿到身分資訊。
- 若 build 當年有開 `EmbedAllSources` → 原始碼文字**真的**內嵌在 PDB，可用支援讀取內嵌來源的工具（如新版 ILSpy 可顯示 embedded documents）取出。具體工具待看到 PDB 後確認。

救回後仍需走 Phase 3～5：救回的不一定就是 build v1 那一版，還是要用行為驗證。

---

#### 路線 B — 反組譯差異比對

**B-1. 先逼近當年環境（這步是降低雜訊的關鍵，比 publish flag 重要得多）**

反組譯比對最大的雜訊來源是**編譯器版本不同**（async state machine、lambda 快取、switch lowering 等 codegen 會隨 SDK 版本變動，產生大量假差異）。

```bash
# 從 runtimeconfig.json 讀出 target framework / runtime 版本
cat ./v1-app/*.runtimeconfig.json

# 用 global.json 把本機 SDK 釘到最接近的版本
#   { "sdk": { "version": "8.0.xxx", "rollForward": "disable" } }

# 從 deps.json 對齊套件版本（盡量逼近，不強求完全相同）
cat ./v1-app/*.deps.json | jq '.libraries | keys'
```

**B-2. 反組譯 v1 的自家 dll（不要碰第三方 System.*/Microsoft.* dll）**

```bash
dotnet tool install --global ilspycmd
mkdir -p decompiled-v1
ilspycmd -p -o ./decompiled-v1/Api            ./v1-app/Your.Api.dll
ilspycmd -p -o ./decompiled-v1/Application    ./v1-app/Your.Application.dll
ilspycmd -p -o ./decompiled-v1/Domain         ./v1-app/Your.Domain.dll
ilspycmd -p -o ./decompiled-v1/Infrastructure ./v1-app/Your.Infrastructure.dll
```

**B-3. 用目前版控 build 一份 publish output，再用同一套反組譯器反組譯**

```bash
dotnet restore
dotnet publish -c Release -o ./publish-current     # 盡量對齊 v1 的 runtime / 參數

mkdir -p decompiled-current
ilspycmd -p -o ./decompiled-current/Api            ./publish-current/Your.Api.dll
ilspycmd -p -o ./decompiled-current/Application    ./publish-current/Your.Application.dll
ilspycmd -p -o ./decompiled-current/Domain         ./publish-current/Your.Domain.dll
ilspycmd -p -o ./decompiled-current/Infrastructure ./publish-current/Your.Infrastructure.dll
```

**B-4. 先用 API surface 篩出「真的變動的 type/method」，再深入比對（避免被淹沒）**

```bash
dotnet tool install --global Microsoft.DotNet.ApiCompat.Tool
# 先比 public API 表面差異，鎖定變動範圍
```

**B-5. 對反組譯結果做資料夾比對**

```bash
git diff --no-index ./decompiled-current ./decompiled-v1 > v1-vs-current-decompiled.diff
```
人工判讀建議用 GUI：Beyond Compare / WinMerge / Rider Directory Compare / Meld / VS Code。

**B-6. 另外比對設定檔（差異不一定在 C#）**

```text
appsettings*.json、serilog.json、nlog.config、web.config、*.xml、*.config
重點：ConnectionStrings、FeatureFlags、Timeout、Batch size、API endpoint、
      log 設定、某流程的啟用/停用
```

---

#### 路線 C — 純黑箱行為復原（最困難）

當反組譯不可信（NativeAOT 無 IL、重度混淆、或 framework 落差過大）時：

```text
- 放棄從程式碼層找差異。
- v1 僅作為行為 oracle：用大量真實/設計輸入跑 v1，記錄輸出。
- 在目前版控上實作，反覆調整，直到 current 對相同輸入的輸出與 v1 一致。
- 這條路完全依賴 Phase 3 的行為基準品質，測試案例要更廣。
```

---

### Phase 3：建立行為基準（Golden Master / Characterization Tests）

所有路線共用的安全網。把 v1 當標準答案。

#### 3.1 善用「跑了多年」的資產：真實輸入

專案在 production 跑了很久，不必憑空編 input。從歷史資料挑真實案例，覆蓋面遠勝手刻：

```text
- API access log / request log → 重放真實流量
- DB 既有資料 → 真實的資料情境與邊角案例
- 批次的歷史輸入/輸出檔
```

#### 3.2 三層保護網

**第一層：API 黑箱比對**

同時啟動 v1 與 current 兩個 container，用同一份 request 打兩邊：

```bash
curl -s -X POST http://localhost:5001/api/xxx -H "Content-Type: application/json" -d @request.json > response-v1.json
curl -s -X POST http://localhost:5002/api/xxx -H "Content-Type: application/json" -d @request.json > response-current.json

# 正規化後比對（排序 key）
jq -S . response-v1.json      > response-v1.sorted.json
jq -S . response-current.json > response-current.sorted.json
diff -u response-v1.sorted.json response-current.sorted.json
```

> **務必處理非決定性欄位**：`DateTime.Now`、GUID、DB identity、排序、decimal/float 格式、culture。比對前要對這些 volatile 欄位做 masking，否則測試會 flaky。

**第二層：DB 結果比對**

```text
1. restore 同一份 seed database → 執行 v1 → dump DB 結果
2. restore 同一份 seed database → 執行 current → dump DB 結果
3. 比對兩份 dump（新增/更新/刪除/狀態轉換/金額計算/錯誤訊息）
```
> 注意 schema drift：若遺失的 commit 含 EF migration，v1 期望的 schema 可能與 current 不同，要先確認用哪套 schema。

**第三層：批次 / worker 輸出比對**

```bash
docker run --rm -v ./input:/app/input -v ./output-v1:/app/output      --env-file .env.v1      your-image:v1
docker run --rm -v ./input:/app/input -v ./output-current:/app/output --env-file .env.current your-image:current
diff -ruN output-v1 output-current     # 也要比 log 與 exit code
```

#### 3.3 測試專案結構

```text
tests/
└── YourProject.CharacterizationTests/
    ├── TestCases/
    │   ├── case-001/
    │   │   ├── request.json
    │   │   ├── expected-response.v1.json
    │   │   ├── db-seed.sql
    │   │   └── expected-db-after.sql
    │   └── case-002/ ...
    └── ApiCharacterizationTests.cs
```

```bash
dotnet new xunit -n YourProject.CharacterizationTests
dotnet add YourProject.CharacterizationTests package AwesomeAssertions
```

```csharp
public sealed class ApiCharacterizationTests
{
    private readonly HttpClient _client;

    public ApiCharacterizationTests()
    {
        var baseUrl = Environment.GetEnvironmentVariable("APP_BASE_URL") ?? "http://localhost:5001";
        _client = new HttpClient { BaseAddress = new Uri(baseUrl) };
    }

    [Fact]
    public async Task Case001_ShouldMatchV1Response()
    {
        var requestJson  = await File.ReadAllTextAsync("TestCases/case-001/request.json");
        var expectedJson = await File.ReadAllTextAsync("TestCases/case-001/expected-response.v1.json");

        using var content = new StringContent(requestJson, Encoding.UTF8, "application/json");
        var response = await _client.PostAsync("/api/xxx", content);
        var actualJson = await response.Content.ReadAsStringAsync();

        response.StatusCode.Should().Be(HttpStatusCode.OK);
        Normalize(actualJson).Should().Be(Normalize(expectedJson));
    }

    // 正規化 + 遮罩 volatile 欄位（依實際回傳調整）
    private static string Normalize(string json)
    {
        var node = JsonNode.Parse(json)!;
        MaskVolatileFields(node);                       // 例如把 timestamp / id / traceId 設為固定值
        return node.ToJsonString(new JsonSerializerOptions { WriteIndented = true });
    }

    private static void MaskVolatileFields(JsonNode node) { /* 遞迴遮罩 createdAt、id、guid 等 */ }
}
```

#### 3.4 測試案例優先順序（不追求覆蓋率，先挑高風險）

```text
1. 已知 v1 與 current 有差異的功能
2. production 最重要的 API / batch flow
3. 金額計算
4. 狀態轉換
5. DB 寫入與更新
6. 查詢條件
7. 日期 / 時間 / 交易日判斷
8. 錯誤處理分支
9. 外部 API 回傳處理（需用 WireMock 之類做 record/replay 或 stub 以保證 deterministic）
10. Feature flag 影響的流程
```
時間有限就先做 5～10 個最重要案例。

---

### Phase 4：整理差異清單並人工補回

**不要把反組譯碼直接貼回 repo。** 反組譯碼常有變數名不自然、async/LINQ 還原不佳、nullable 不完整、record/pattern matching 被還原成低階寫法、source generator 產物混入等問題。

差異清單範例：

| 差異位置 | v1 行為 | current 行為 | 風險 | 這是刻意還是 bug | 驗證方式 | 是否補回 |
|---|---|---|---|---|---|---|
| `OrderService.Calculate()` | 多一個條件判斷 | 無 | 高 | 刻意 | API case-001 | 是 |
| `Repository.Query()` | 排除某狀態 | 未排除 | 高 | 刻意 | DB 比對 | 是 |
| `appsettings` | flag = true | false | 中 | 待確認 | config diff | 待確認 |

補回流程：

```text
1. 從差異清單確認要補的邏輯
2. 回到目前 repo，用正常、可維護的 C# 寫法實作（不是貼反組譯碼）
3. 為該變更補上對應的 characterization test
4. 跑測試，確認 current 行為與 v1 一致
```

---

### Phase 5：驗證 current 行為與 v1 一致

```bash
dotnet clean
dotnet restore
dotnet build
dotnet test          # characterization tests 必須全綠
```

逐案確認：current 對相同輸入的 API response、DB 結果、批次輸出，皆與 v1 的 Golden Master 一致。

---

### Phase 6：打包 v2 並上線

```bash
dotnet publish -c Release
docker build \
  --build-arg GIT_COMMIT=$(git rev-parse HEAD) \
  --build-arg GIT_BRANCH=$(git rev-parse --abbrev-ref HEAD) \
  -t your-image:v2 .
```

上線前檢查清單：

```text
[ ] v2 container 可正常啟動、health check 通過
[ ] v2 對相同 request 的 response 與 v1 一致（跑 Phase 3 同一批案例）
[ ] v2 對相同 seed data 的 DB 結果與 v1 一致
[ ] v2 必要設定與 v1 一致
[ ] log / error behavior 無重大差異
[ ] 保留 v1 image 作為 rollback（Phase 0 的 tar）
```

> 提醒：v2 幾乎一定會用較新的 base image（CVE、EOL），所以 v2 與 v1 在 runtime/base 層本來就不同——這就是為什麼驗收標準訂在「行為一致」而非「二進位一致」。

---

### Phase 7：事後 traceability（避免重蹈覆轍）

這次問題的本質是「image 存在，但 source / commit 對應關係遺失」。之後務必補上可追溯性。

**Docker image 加上 commit label**

```dockerfile
ARG GIT_COMMIT
ARG GIT_BRANCH
LABEL org.opencontainers.image.revision=$GIT_COMMIT
LABEL org.opencontainers.image.source=$GIT_BRANCH
```

```bash
docker inspect your-image:v2 --format '{{json .Config.Labels}}'
```

**.csproj 寫入來源 metadata（SourceLink + 內嵌來源）**

```xml
<PropertyGroup>
  <ContinuousIntegrationBuild>true</ContinuousIntegrationBuild>
  <PublishRepositoryUrl>true</PublishRepositoryUrl>
  <EmbedUntrackedSources>true</EmbedUntrackedSources>
  <!-- 視需要：<EmbedAllSources>true</EmbedAllSources> 讓 PDB 內嵌完整原始碼 -->
  <DebugType>portable</DebugType>
</PropertyGroup>
```
（GitLab 需搭配對應 SourceLink 套件設定。）

**CI/CD 保留 build artifact**

```text
publish output、test report、build log、source commit hash、
Dockerfile、image digest、SBOM
至少要能回答：這個 image 是哪個 commit、哪份 Dockerfile、哪些 build args、哪些 NuGet 版本 build 的。
```

---

## Part 4 — 風險與盲點（必須誠實面對）

```text
1. multi-stage build 的編譯階段不可見
   docker history 只看得到最終 image 的 layer。若當年是 multi-stage，
   編譯階段（含原始碼、SDK、build 參數）在最終 image 裡已被丟棄，看不到。
   build-time-only 的步驟（build 時 codegen、條件編譯、改檔）= 無法逆推。
   對策：承認它存在、標成風險，靠行為比對把關——若它影響輸出，Golden Master 會抓到。

2. 雙向分歧
   current 與 v1 可能各自有對方沒有的東西，不是單向「少兩個 commit」。

3. 非決定性
   未遮罩 volatile 欄位的行為測試會 flaky，浪費時間。

4. v1 可能本身有 bug
   逐筆判斷「刻意 vs bug」，別把舊 bug 一起復原。

5. NativeAOT / 重度混淆
   反組譯不可行或不可信時，只能走純黑箱（路線 C），測試案例需更廣。

6. 二進位不可重現
   即使原始碼與 SDK 都對，也無法 build 出與 v1 相同的二進位——
   所以驗收只看行為。
```

---

## Part 5 — 最終心法

```text
這不是單純的「重新 build image」問題，而是：
  source recovery + behavior recovery + regression protection 三者合一。

核心原則：
  v1 image          = 唯一可信行為來源（兼指紋來源 + 行為 oracle）
  目前版控           = 恢復基底（不完整、不可信為完整）
  反組譯結果         = 差異線索（不是原始碼，不直接貼回）
  Characterization   = 安全網（所有路線共用）

安全的順序：
  先解剖 image 決定路線 → 用反組譯/原始碼找差異 → 用 v1 行為確認差異
  → 用測試固定行為 → 人工補回目前版控 → 驗證 → 出 v2 → 保留 rollback → 補 traceability
```
