# 整合測試技術轉移指南（WebApplicationFactory + Testcontainers + Respawner + xUnit）

> 這份文件給要接手 / 採用這套整合測試的開發人員。重點在**觀念、流程、與「為什麼這樣設計」**，
> 而不是逐行程式碼。完整程式碼請看
> [multi-database-sqlserver-integration-test.md](multi-database-sqlserver-integration-test.md)。

---

## 目錄

- [1. 這套整合測試在測什麼？](#1-這套整合測試在測什麼)
- [2. 四項核心技術各自的角色](#2-四項核心技術各自的角色)
- [3. 各類別的職責](#3-各類別的職責)
- [4. 執行流程順序](#4-執行流程順序最重要)
- [5. 為什麼用 ICollectionFixture？](#5-為什麼用-icollectionfixture不是-iclassfixture也不是建構式)
- [6. 為什麼建構式注入 IntegrationTestFixture？](#6-為什麼測試類別建構式要注入-integrationtestfixture)
- [7. request/response 與 ILogger/Serilog 如何接到 ITestOutputHelper](#7-request--response-與-sut-的-ilogger--serilog-如何接到-itestoutputhelper)
- [8. 時間控制：讓 SUT 的「當下時間」可控](#8-時間控制讓-sut-的當下時間可控)
- [9. Respawner 如何做到測試間隔離](#9-respawner-如何做到測試間隔離)
- [10. 怎麼寫一個新的整合測試](#10-怎麼寫一個新的整合測試onboarding-步驟)
- [11. 常見問題與注意事項](#11-常見問題與注意事項)
- [附錄：一眼看懂的關係圖](#附錄一眼看懂的關係圖)

---

## 1. 這套整合測試在測什麼？

不是 mock 一切的單元測試，而是**把真正的 WebApi 跑起來、打真的 HTTP 請求、連真的資料庫與快取**，
驗證「從 HTTP 進來 → 過 Controller / Service / Repository → 讀寫真資料庫 → 回應」整條路是對的。

- **被測對象（SUT, System Under Test）**：`Day23.WebApi`（整個 ASP.NET Core 應用）。
- **真依賴**：SQL Server、Redis、Kafka 都用 **Testcontainers** 開真的 Docker 容器，不是 in-memory 假貨。
- **隔離**：每個測試之間用 **Respawner** 把資料清乾淨，測試互不干擾。
- **可控時間**：用 `FakeTimeProvider` 把 SUT 的「當下時間」變成測試可設定（見第 8 節）。

一句話：**用最接近正式環境的方式，驗證整個 WebApi 的行為。**

---

## 2. 四項核心技術各自的角色

| 技術 | 角色 | 在本專案 |
|------|------|----------|
| **WebApplicationFactory** | 在**記憶體內**啟動整個 WebApi（TestServer），提供 `HttpClient` 直接打它；可覆寫設定與服務註冊 | `TestWebApplicationFactory` |
| **Testcontainers** | 用程式**開真的 Docker 容器**（SQL Server / Redis / Kafka），測試結束自動清掉 | `DatabaseManager` 內的 `MsSqlContainer`、`RedisFixture` 內的 `RedisContainer`、`KafkaFixture` 內的 `KafkaContainer` |
| **Respawner** | 每個測試後把資料庫**資料清空**（只刪資料、不動 schema），達成測試間隔離 | `DatabaseManager.CleanDatabaseAsync()` |
| **xUnit** | 測試框架；提供 fixture 生命週期（`IAsyncLifetime`）、資源共享（`ICollectionFixture`）、測試輸出（`ITestOutputHelper`） | `IntegrationTestFixture`、`IntegrationTestBase`、`IntegrationTestCollection` |

這四者的合作關係：

```text
xUnit（框架 / 生命週期 / 共享）
  └─ 開啟 Testcontainers 容器（真 SQL Server / Redis / Kafka）
        └─ 把容器連線字串注入 WebApplicationFactory（啟動真 WebApi）
              └─ 測試用 HttpClient 打 WebApi → 讀寫容器裡的真資料庫
                    └─ 每個測試後用 Respawner 清資料
```

以下為 Mermaid 版：

```mermaid
flowchart TD
    xUnit["xUnit（框架 / 生命週期 / 共享）"]
    TC["Testcontainers 容器<br/>真 SQL Server、Redis、Kafka"]
    WAF["WebApplicationFactory<br/>啟動真 WebApi（SUT）"]
    Client["測試 HttpClient"]
    Respawner["Respawner"]

    xUnit -->|開啟| TC
    TC -->|注入連線字串| WAF
    Client -->|打 HTTP 請求| WAF
    WAF -->|讀寫| TC
    Respawner -->|每個測試後清資料| TC
```

---

## 3. 各類別的職責

| 類別 | 職責 | 生命週期 |
|------|------|----------|
| `IntegrationTestCollection` | 定義一個 **xUnit Collection**，宣告要共享 `IntegrationTestFixture` | 靜態定義 |
| `IntegrationTestFixture` | **組合根**：建立/啟動/釋放所有容器與 factory；設 `PreserveExecutionContext`；提供 `ResetStateAsync` | **整個 Collection 一次** |
| `DatabaseManager` | 自建並持有一台 SQL Server 容器；跑 schema/seed；提供連線字串與 DB 操作；Respawner 重置 | 隨 fixture |
| `DatabaseManagerRegistry` | 管理多台資料庫（多個 `DatabaseManager`），統一 Init/Reset/Dispose | 隨 fixture |
| `RedisFixture` | 自建並持有 Redis 容器；提供連線字串 | 隨 fixture |
| `KafkaFixture` | 自建並持有 Kafka 容器；提供 bootstrap servers 與 consumer 驗證 helper（SUT 尚未使用，先備好） | 隨 fixture |
| `TestWebApplicationFactory` | 接收注入的容器 fixture，把連線字串套進 WebApi 設定、接管 log；產生 `HttpClient`；持有 `FakeTimeProvider` | 隨 fixture |
| `IntegrationTestBase` | 測試基底：提供 `HttpClient` / DB 存取 / 時間控制 / log 輸出；每個測試後重置狀態 | **每個測試一次** |
| `TestOutputSink` / `TestOutputSerilogSink` / `XUnitLogger` / `RequestCaptureHandler` | 把 SUT 內部 log 與 request/response 轉發到測試輸出（見第 7 節） | — |

> **關鍵區分：兩個層級的生命週期**
>
> - **Fixture 層（Collection scope）**：容器啟動 / 關閉，**整個測試集合只做一次**（貴的資源共享）。
> - **Base 層（per-test scope）**：每個測試方法前後各跑一次（本專案用來「測試後重置資料」）。

---

## 4. 執行流程順序（最重要）

```text
① Collection 開始（整個測試集合一次）
   xUnit 建立 IntegrationTestFixture → 呼叫 fixture.InitializeAsync()
     ├─ Databases.InitializeAllAsync()
     │    每個 DatabaseManager：啟動 SQL Server 容器 → 跑 Schema/Seed 腳本 → 建立 Respawner
     ├─ Redis.InitializeAsync()：啟動 Redis 容器
     ├─ Kafka.InitializeAsync()：啟動 Kafka 容器
     ├─ new TestWebApplicationFactory(Databases, Redis, Kafka)
     │    （把各容器連線字串記著，稍後注入 WebApi 設定）
     └─ Factory.Server.PreserveExecutionContext = true
          （讓測試設的 AsyncLocal，例如 TimeProviderAccessor 的時間，能流進 SUT 請求管線）

② 每一個測試方法（Collection 內循序執行）
   a. xUnit 建立測試類別 → 建構式注入 (IntegrationTestFixture, ITestOutputHelper)
        → base 建構式：
             - HttpClient = fixture.Factory.CreateDefaultClient(new RequestCaptureHandler())
               （第一次 CreateClient 時，WebApplicationFactory 才真正建起 TestServer / 啟動 WebApi）
             - DatabaseManager / AuditDatabase = fixture.Databases[DatabaseNames.X]
             - Logger = XUnitLogger(這個測試的 ITestOutputHelper)
   b. xUnit 呼叫 base.InitializeAsync()（per-test；本專案不用再做事，表已在 fixture 建好）
   c. 測試方法本體：
        - Arrange：用 TestHelpers / ExecuteAsync 塞測試資料；需要控時就 UseLocalTime(...)
        - Act：HttpClient 打 WebApi（SUT 內部 log 在此累積進 OutputSink 佇列）
        - 輸出：LogResponseBodyAsync() 印出 Request / SUT Logs / Response
        - Assert：驗證回應與資料庫狀態
   d. xUnit 呼叫 base.DisposeAsync()（per-test）
        → Fixture.ResetStateAsync()：Respawner 清空所有資料庫
        → FlurlClient.Dispose() + OutputSink.DrainAll()（清掉殘留 log）

③ Collection 結束（整個測試集合一次）
   xUnit 呼叫 fixture.DisposeAsync() → 釋放 factory 與所有容器
```

以下為 Mermaid 版（時序圖）：

```mermaid
sequenceDiagram
    autonumber
    participant xUnit
    participant Fixture as IntegrationTestFixture
    participant Registry as DatabaseManagerRegistry
    participant Factory as TestWebApplicationFactory
    participant Test as 測試（IntegrationTestBase）
    participant SUT as WebApi（SUT）

    note over xUnit,SUT: ① Collection 開始（整個集合一次）
    xUnit->>Fixture: InitializeAsync()
    Fixture->>Registry: InitializeAllAsync()
    Registry->>Registry: 啟動容器 + 跑 Schema/Seed + 建 Respawner
    Fixture->>Factory: new（注入 Databases、Redis、Kafka）
    Fixture->>Factory: Server.PreserveExecutionContext = true

    note over xUnit,SUT: ② 每個測試（Collection 內循序）
    xUnit->>Test: 建構式（fixture, ITestOutputHelper）
    Test->>Factory: CreateDefaultClient() → 啟動 SUT
    xUnit->>Test: InitializeAsync()（per-test）
    Test->>SUT: HttpClient 打 API（Act）
    SUT-->>Test: 回應（SUT log 進 OutputSink 佇列）
    Test->>Test: LogResponseBodyAsync()（印 Request / Logs / Response）
    xUnit->>Test: DisposeAsync()
    Test->>Fixture: ResetStateAsync()（Respawner 清資料）

    note over xUnit,SUT: ③ Collection 結束（整個集合一次）
    xUnit->>Fixture: DisposeAsync() → 釋放 factory 與所有容器
```

> **同一個 Collection 內的測試是「循序」執行**（xUnit 預設不在 collection 內平行跑）。
> 這一點很重要——第 7 節的「SUT log 佇列」正是靠這個保證才能正確對應到每次呼叫。

---

## 5. 為什麼用 `ICollectionFixture`？（不是 IClassFixture、也不是建構式）

xUnit 有三種「共享範圍」，差別在**貴資源會被建立幾次**：

| 方式 | 共享範圍 | 容器會啟動幾次 |
|------|----------|----------------|
| 建構式 / `Dispose`（預設） | **每個測試方法** | 每個測試都重開一次 → 極慢 ❌ |
| `IClassFixture<T>` | **單一測試類別內** | 每個測試類別各開一次 → 有幾個類別就開幾次 ❌ |
| **`ICollectionFixture<T>`** | **同一 Collection 的所有測試類別** | **整個集合只開一次** ✅ |

啟動一台 SQL Server 容器要好幾秒（甚至更久），如果每個測試 / 每個類別都重開，測試會慢到不可用。
`ICollectionFixture` 讓「開容器」這種昂貴動作**整個測試集合只做一次**，所有測試類別共享，是效能上的關鍵。

實作上：

- `IntegrationTestCollection` 用 `[CollectionDefinition("Integration Tests")]` + `ICollectionFixture<IntegrationTestFixture>` 宣告共享。
- `IntegrationTestBase` 掛 `[Collection("Integration Tests")]`，所以**所有繼承它的測試類別都屬於同一個 collection、共享同一組容器**。

---

## 6. 為什麼測試類別建構式要注入 `IntegrationTestFixture`？

這是 **xUnit 的相依注入機制**：

1. 當一個測試類別屬於某 collection、而該 collection 用 `ICollectionFixture<IntegrationTestFixture>` 宣告了共享物件，
   xUnit 就會**建立一個 `IntegrationTestFixture`（整個 collection 唯一）**，並在建立每個測試類別時，把它**注入建構式**。
2. `ITestOutputHelper` 則是 xUnit 內建、**每個測試各一份**的輸出管道，也由 xUnit 注入建構式。

所以測試類別的建構式簽章長這樣：

```csharp
public XxxTests(IntegrationTestFixture fixture, ITestOutputHelper output)
    : base(fixture, output) { }
```

- `fixture`：**共享的**（容器、factory 都在裡面）。
- `output`：**這個測試專屬的**輸出。

`IntegrationTestBase` 接住這兩者：從 `fixture` 拿共享資源（`Factory`、`Databases`），用 `output` 建立
只屬於這次測試的 `Logger`。**「共享的東西從 fixture 拿、每測試獨立的東西從 output 拿」——這是整個注入設計的核心。**

---

## 7. request / response 與 SUT 的 ILogger / Serilog 如何接到 `ITestOutputHelper`？

這是最容易卡住、也最有價值的部分。分成兩條線：**測試端的 HTTP request/response** 與 **SUT 內部的 log**。

### 7-1. `ITestOutputHelper` 是什麼、為什麼要它

xUnit 不建議在測試裡用 `Console.WriteLine`；要顯示在測試結果裡，要透過 `ITestOutputHelper.WriteLine`。
它由 xUnit **每個測試注入一份**。我們用 `XUnitLogger<T>`（一個 `ILogger` 實作）把它包成標準 `ILogger`，
測試裡就能用 `Logger.LogInformation(...)` 輸出。

### 7-2. HTTP Request body：`RequestCaptureHandler`

問題：`HttpClient` 送出請求後，通常會 **dispose 掉 `request.Content`**，等你拿到回應想印請求內容時已經讀不到。

解法：掛一個 `DelegatingHandler`（`RequestCaptureHandler`），在**送出前**先把 body 緩衝成字串、存進
`request.Options`；測試拿到回應後，從 `response.RequestMessage.Options` 取回來印。

```text
HttpClient（掛 RequestCaptureHandler）
  → 送出前：緩衝 request body 存進 Options
  → 拿到 response 後：從 response.RequestMessage.Options 取回 body 輸出
```

### 7-3. SUT 內部 log（Serilog）：為什麼要「佇列緩衝 + 主動 drain」

這裡是重點。被測 WebApi 用 **Serilog** 記 log。我們想把 SUT 內部（Controller/Service/Middleware）的 log
也印到**當前測試**的 `ITestOutputHelper`，方便測試失敗時診斷。但有個難題：

> **難題**：SUT 在 TestServer 裡處理請求的執行環境，**預設不會延續**呼叫端測試方法的 `AsyncLocal` 脈絡，
> 所以 SUT 內部程式碼**拿不到「當前測試的 `ITestOutputHelper`」**，沒辦法直接寫進去。

解法：**中央佇列緩衝 + 測試主動取出**。

```text
①（fixture 啟動時）TestWebApplicationFactory 在 SUT 的 Serilog 上掛一個自訂 sink：
     TestOutputSerilogSink(OutputSink)

② SUT 內部每寫一筆 log
     → Serilog 觸發 TestOutputSerilogSink.Emit()
     → 格式化後 Enqueue 進 OutputSink（一個 ConcurrentQueue 佇列，掛在 factory 上、集合共享）

③ 測試在 Act 之後呼叫 LogResponseBodyAsync()
     → OutputSink.DrainAll() 取出目前佇列裡累積的所有 log
     → 用 Logger（XUnitLogger → 這次測試的 ITestOutputHelper）逐行印出

④ 測試結束 DisposeAsync 再 DrainAll 一次，清掉殘留，避免污染下一個測試
```

以下為 Mermaid 版（時序圖）：

```mermaid
sequenceDiagram
    autonumber
    participant SUT as SUT 內部（Serilog）
    participant Sink as TestOutputSerilogSink
    participant Queue as OutputSink（佇列）
    participant Test as 測試（Act 之後）
    participant Helper as ITestOutputHelper

    SUT->>Sink: 每寫一筆 log → Emit()
    Sink->>Queue: 格式化後 Enqueue
    note over Test: Act 之後
    Test->>Queue: DrainAll() 取出累積的 log
    Queue-->>Test: 這次呼叫的 log 行
    Test->>Helper: 逐行 WriteLine（透過 XUnitLogger）
```

**為什麼這樣可行？** 因為**同一 collection 的測試是循序執行的**（第 4 節）——在 Act 之後、下一個測試開始前
去 drain 佇列，取出的內容就「正好是這次呼叫產生的 log」，不會跟別的測試交錯。

> 補充：`TestOutputSerilogSink` 的攔截點下在 **Serilog 的 sink 層**（不是 `Microsoft.Extensions.Logging` 的
> `ILoggerProvider`），因為 SUT 改用 Serilog 後，訊息不再流經 MEL 的 provider，必須在 Serilog 這一層接。

### 7-4. 三段式輸出

`LogResponseBodyAsync()` 把一次呼叫整理成三段印出，診斷很直覺：

```text
──────────────────────────────
▶ HTTP Request: POST /products      ← RequestCaptureHandler 緩衝的 request（body 美化縮排）
──────────────────────────────
■ SUT Logs:                          ← OutputSink drain 出的 SUT 內部 log
  [SUT] [Information] [ProductService] ...
──────────────────────────────
◀ HTTP Response: 201                 ← 回應狀態碼與 body（JSON 美化縮排）
{ ... }
```

---

## 8. 時間控制：讓 SUT 的「當下時間」可控

要對「時間戳」做確定性斷言（例如 `CreatedAt` 是不是 2030-06-15），SUT 讀到的「現在」就必須可控。
本專案用 **`FakeTimeProvider`**（`Microsoft.Extensions.Time.Testing`）當假時鐘。

**但關鍵是：SUT 取時間有兩種寫法，控制方式完全不同。搞錯就會「設了時間卻拿到電腦系統時間」。**

### 8-1. 兩種取時間的方式 vs 兩種控制方式

| SUT 怎麼取時間 | 範例 | 控制方式 | 需要 `PreserveExecutionContext`？ |
|----------------|------|----------|-----------------------------------|
| **建構子注入的 `TimeProvider`** | `ProductService`、`HealthController` 用 `_timeProvider.GetUtcNow()` | factory 在 `ConfigureServices` 把 DI 的 `TimeProvider` 換成 `FakeTimeProvider`；用 base 的 `SetTime`/`AdvanceTime`/`ResetTime` 控制（作用在同一個實例） | **不需要** |
| **AsyncLocal 靜態存取器** | `TimeController` 用 `TimeProviderAccessor.Now` | 用 base 的 `UseLocalTime(...)` 在測試方法內設，並**在 fixture 開 `PreserveExecutionContext = true`** | **一定要** |

- **注入版**：因為 SUT 拿的是 DI 容器裡那顆 `TimeProvider`，factory 換成假的、你在測試操作同一顆，自然生效。
- **AsyncLocal 版**：`TimeProviderAccessor` 讀的是 `AsyncLocal<TimeProvider>`，值綁在執行脈絡上——這正是第 7-3 節那個坑：**AsyncLocal 預設不會流進 TestServer 的請求管線**。差別在，log 那邊用「佇列」繞過；時間這邊我們**反而要讓脈絡流進去**，所以打開 `PreserveExecutionContext`。

### 8-2. AsyncLocal 版：為什麼沒開 `PreserveExecutionContext` 就拿到系統時間

```text
測試方法內：UseLocalTime(2030) → TimeProviderAccessor 的 AsyncLocal 設成 fake(2030)
        │
        │ await HttpClient.Get(...)
        ▼
TestServer 請求管線
  ├─ PreserveExecutionContext = false（預設）→ AsyncLocal 沒流進 → SUT 讀到系統時間 ❌
  └─ PreserveExecutionContext = true          → AsyncLocal 流進   → SUT 讀到 2030 ✅
        ▼
SUT：TimeProviderAccessor.Now → fake.GetLocalNow() → 2030
```

以下為 Mermaid 版：

```mermaid
flowchart TD
    Set["測試方法內：UseLocalTime(2030)<br/>設 TimeProviderAccessor 的 AsyncLocal"]
    Send["await HttpClient.Get(...)"]
    Server{"TestServer<br/>PreserveExecutionContext?"}
    No["false（預設）<br/>AsyncLocal 不流進 → SUT 拿系統時間 ❌"]
    Yes["true<br/>AsyncLocal 流進 → SUT 拿 2030 ✅"]
    SUT["SUT：TimeProviderAccessor.Now"]

    Set --> Send --> Server
    Server -->|false| No
    Server -->|true| Yes --> SUT
```

### 8-3. 怎麼做（AsyncLocal 版，實測可行）

**一次性（`IntegrationTestFixture.InitializeAsync()`，建完 factory 後）：**
```csharp
Factory = new TestWebApplicationFactory(Databases, Redis, Kafka);
Factory.Server.PreserveExecutionContext = true;   // ← 沒這行，AsyncLocal 到不了 SUT
```

**base 的 helper（`IntegrationTestBase`）：**
```csharp
private static readonly TimeZoneInfo TaipeiZone = TimeZoneInfo.FindSystemTimeZoneById("Asia/Taipei");

protected IDisposable UseLocalTime(DateTime localWallClock)
{
    var utc = TimeZoneInfo.ConvertTimeToUtc(
        DateTime.SpecifyKind(localWallClock, DateTimeKind.Unspecified), TaipeiZone);

    // 用「建構子」設初始時間，不是 SetUtcNow：SetUtcNow 不能往回設，且預設起始為 2000-01-01
    var fake = new FakeTimeProvider(new DateTimeOffset(utc, TimeSpan.Zero));
    fake.SetLocalTimeZone(TaipeiZone);
    return TimeProviderAccessor.CreateTestScope(fake);   // using 離開時自動還原
}
```

**測試裡（時間設在測試方法內、Act 之前）：**
```csharp
[Fact]
public async Task Xxx()
{
    using var _ = UseLocalTime(new DateTime(2030, 6, 15, 9, 0, 0));   // 台灣本地牆上時間
    var response = await HttpClient.GetAsync("/time");                // SUT 讀到 2030-06-15 09:00
    // Assert ...
}
```

### 8-4. 四個地雷（踩過就白試半天）

| 地雷 | 正確做法 |
|------|----------|
| 沒開 `PreserveExecutionContext`（AsyncLocal 版） | fixture 建完 factory 後設 `Factory.Server.PreserveExecutionContext = true` |
| 時間設在 fixture / `InitializeAsync` | AsyncLocal 流不進測試方法 → **一定設在測試方法內** |
| 用 `SetUtcNow` 設 2000 之前的時間 | `SetUtcNow` 不能往回設 → 改用**建構子** `new FakeTimeProvider(utc)` |
| 只 `SetUtcNow` 沒設時區（讀 `.Now`/本地時間） | `SetLocalTimeZone(...)`；且 `SetUtcNow` 傳 offset=0 的 UTC（`GetLocalNow` 以 Ticks 計算） |

### 8-5. 設了還是拿到系統時間？兩點探針定位

（尤其把這套搬到別的專案卻失效時）在一支測試裡跑一次，看斷在哪：

```csharp
using var _ = /* 你設定時間的方式 */;

var testSide = TimeProviderAccessor.Now;                 // 探針 A：測試端自己讀
var sutSide  = await HttpClient.GetStringAsync("/time"); // 探針 B：SUT 回報它看到的時間
```

| 探針 A | 探針 B | 問題點 |
|:---:|:---:|---|
| ❌ 系統時間 | — | **設定沒生效**：accessor / `CreateTestScope` 有問題，或時間設在測試方法外 |
| ✅ 設定時間 | ❌ 系統時間 | **流動沒生效**：`PreserveExecutionContext` 沒設 / 設在錯的 server |
| ✅ 設定時間 | ✅ 但原端點仍系統時間 | **那條程式路徑沒走 accessor**（某處用了 `DateTime.Now`） |

> 本專案已用 `TimeController`（讀 `TimeProviderAccessor.Now`）+ `TimeControlTests` 實測這一整套，全數通過。

---

## 9. Respawner 如何做到測試間隔離

- **建表只做一次**：容器啟動時（fixture 層）跑 schema 腳本建表，並 `Respawner.CreateAsync` 掃描 schema、
  算出「要清哪些表、什麼順序清」的計畫。
- **每個測試後清資料**：`DisposeAsync → ResetStateAsync → Respawner.ResetAsync`，依外鍵順序**刪掉所有資料列**。
- **只刪資料、不動 schema**：表還在（空的），所以下一個測試不用重建表，快又乾淨。

> 注意：Respawner 是「刪光資料」，**不會保留靜態 seed**。若需要跨測試都存在的種子資料，要用
> `RespawnerOptions.TablesToIgnore` 排除、或每次 reset 後重跑 seed（本專案的 Seed 目錄目前留空，
> 測試改用 `TestHelpers` 在每個測試裡動態塞資料）。

---

## 10. 怎麼寫一個新的整合測試（onboarding 步驟）

**A. 寫一個新的測試類別**：繼承 `IntegrationTestBase`，建構式照抄注入。

```csharp
public class OrdersControllerTests : IntegrationTestBase
{
    public OrdersControllerTests(IntegrationTestFixture fixture, ITestOutputHelper output)
        : base(fixture, output) { }

    [Fact]
    public async Task 建立訂單_應回傳201()
    {
        // Arrange：需要前置資料就用 TestHelpers / DatabaseManager.ExecuteAsync 塞；需要控時就 UseLocalTime(...)
        // Act
        var response = await HttpClient.PostAsJsonAsync("/orders", request);
        await LogResponseBodyAsync(response);   // 印出 Request / SUT Logs / Response

        // Assert：驗回應
        response.Should().Be201Created();
        // 需要時直接查資料庫驗狀態
        var count = await DatabaseManager.QuerySingleAsync<int>("SELECT COUNT(*) FROM orders");
        count.Should().Be(1);
    }
}
```

你**不用**管容器啟動、連線字串、清資料、`PreserveExecutionContext`——base 與 fixture 都處理好了。

**B. 需要新的資料表**：把建表 `.sql` 放到 `TestData/Sql/Schema/{DatabaseName}/`，容器啟動時會自動執行。

**C. 需要第三個資料庫**：在 `DatabaseNames` 加一個常數並納入 `All`，其餘（registry / factory / fixture）
都不用改（見完整程式碼文件第 5 節）。

**D. 需要控制時間**：`using var _ = UseLocalTime(new DateTime(...));` 放在 Act 之前（見第 8 節）。

---

## 11. 常見問題與注意事項

- **一定要有 Docker**：Testcontainers 靠 Docker 開容器；CI/開發機都要能連 Docker daemon。
- **容器啟動較慢**：SQL Server 映像大、啟動要時間；靠 `ICollectionFixture` 整個集合只開一次來攤平成本。
- **測試在 collection 內循序執行**：不要假設同集合測試會平行；「SUT log 佇列」也依賴這個前提。
- **不要在測試裡用 `Console.WriteLine`**：改用 `Logger`（會進 `ITestOutputHelper`）。
- **設定覆寫時機**：`TestWebApplicationFactory` 在 `ConfigureAppConfiguration` 清掉原設定、注入容器連線字串；
  SUT 的連線工廠要在**解析時（resolution）**才讀設定，才能拿到覆寫後的值。
- **每個測試預設是乾淨的資料庫**：靠 `ResetStateAsync`（Respawner）；不要依賴上一個測試留下的資料。
- **時間控制分兩種**：注入版換 DI 即可；AsyncLocal 版一定要 `PreserveExecutionContext = true` 且時間設在測試方法內（見第 8 節）。

---

## 附錄：一眼看懂的關係圖

```text
[Collection("Integration Tests")]
        │  綁定
        ▼
ICollectionFixture<IntegrationTestFixture>   ← 整個集合共享一份（xUnit 注入）
        │  持有
        ├─ DatabaseManagerRegistry ──> N × DatabaseManager ──> N × MsSqlContainer（Testcontainers）
        │                                     └─ Respawner（清資料）
        ├─ RedisFixture ──> RedisContainer（Testcontainers）
        ├─ KafkaFixture ──> KafkaContainer（Testcontainers；SUT 尚未使用，先備好）
        └─ TestWebApplicationFactory（WebApplicationFactory）
                 ├─ 注入容器連線字串 → 啟動真 WebApi（SUT）
                 ├─ Server.PreserveExecutionContext = true（讓 AsyncLocal 流進 SUT）
                 ├─ CreateDefaultClient(RequestCaptureHandler) → HttpClient
                 ├─ FakeTimeProvider（可控時間）
                 └─ Serilog sink → TestOutputSink（佇列）

每個測試類別（繼承 IntegrationTestBase）
        建構式注入 (IntegrationTestFixture 共享, ITestOutputHelper 專屬)
                 ├─ 從 fixture 拿 HttpClient / Databases
                 ├─ 用 output 建 XUnitLogger（Logger）
                 ├─ UseLocalTime(...) 控制 SUT 的當下時間
                 └─ DisposeAsync → Fixture.ResetStateAsync()（Respawner 清資料）
```

以下為 Mermaid 版：

```mermaid
flowchart TD
    Collection["Collection: Integration Tests"]
    Fixture["IntegrationTestFixture<br/>ICollectionFixture（集合共享）"]
    Registry["DatabaseManagerRegistry"]
    DM["DatabaseManager × N"]
    MsSql["MsSqlContainer × N<br/>（Testcontainers）"]
    Respawner["Respawner（清資料）"]
    Redis["RedisFixture → RedisContainer"]
    Kafka["KafkaFixture → KafkaContainer<br/>（SUT 尚未使用，先備好）"]
    Factory["TestWebApplicationFactory<br/>（WebApplicationFactory）<br/>PreserveExecutionContext=true<br/>FakeTimeProvider"]
    Queue["TestOutputSink（佇列）"]
    SUT["真 WebApi（SUT）"]
    Base["測試類別<br/>（繼承 IntegrationTestBase）"]
    Helper["ITestOutputHelper<br/>（每測試專屬）"]

    Collection -->|綁定| Fixture
    Fixture --> Registry
    Registry --> DM
    DM --> MsSql
    DM --> Respawner
    Fixture --> Redis
    Fixture --> Kafka
    Fixture --> Factory
    Factory -->|注入連線字串 + 時間| SUT
    Factory -->|Serilog sink| Queue
    Base -->|建構式注入 共享| Fixture
    Base -->|建構式注入 專屬| Helper
    Base -->|HttpClient / UseLocalTime| SUT
```

> 完整可編譯程式碼：[multi-database-sqlserver-integration-test.md](multi-database-sqlserver-integration-test.md)
