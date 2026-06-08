# Visual Studio Code 1.118 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.118
**發行日期：** 2026 年 4 月 29 日
**原文：** https://code.visualstudio.com/updates/v1_118

---

歡迎使用 Visual Studio Code 1.118 版本。本次發行擴展了您使用 Copilot Agent 的場景，並讓它們更加高效。以下是本次發行的亮點：

- **[遠端控制](#copilot-cli-工作階段遠端控制實驗性)**：從 GitHub.com 或行動裝置遠端追蹤並控制正在執行的 Copilot CLI 工作階段。
- **[程式碼庫搜尋](#程式碼庫搜尋與上下文)**：在任何工作區中使用語意搜尋找到所需程式碼，並跨 GitHub 儲存庫與組織進行文字搜尋。
- **[Skill 專屬上下文](#skill-專屬上下文實驗性)**：隔離 Skill 執行，讓主要聊天保持專注、回答更精確。
- **[聊天工作階段洞察](#chronicle實驗性)**：將聊天歷程轉化為站立報告、建議與過往工作的回答。
- **[企業控管](#核准帳號組織政策)**：將 AI 功能存取限制在管理員信任的組織內。
- **[Token 效率改善](#token-效率改善)**：以更低的 Token 使用量，從每次請求中獲得更多價值。

Happy Coding!

---

## Agent 體驗

### Visual Studio Code Agents（Insiders）

> **注意**：Visual Studio Code Agents 應用程式目前為預覽版，僅在安裝 VS Code Insiders 時可用。

[Visual Studio Code Agents](https://aka.ms/VSCode/Agents/docs) 應用程式是與 VS Code Insiders 一同發行的伴隨應用程式。它提供一個專注的、agent-native 環境，讓您可以跨儲存庫執行平行工作階段，並反覆處理多步驟編碼任務。我們在 [1.115](https://code.visualstudio.com/updates/v1_115#_visual-studio-code-agents-preview) 中首次推出 Agents 應用程式，並持續根據使用者回饋進行改進。

在本次發行中，您可以直接從 VS Code Insiders 標題列探索 Agents 應用程式，讓您輕鬆進入 Agent 驅動的工作流程。我們也發佈了專屬的 [Agents 文件](https://aka.ms/VSCode/Agents/docs)，協助您入門。

近期功能亮點包括：

- **VS Code 與 Agents 之間的共享狀態**：Agents 應用程式與 VS Code Insiders 共享更多狀態，讓兩者之間的轉換更加順暢。包括認證（Windows 上）、AI 自訂項目、工作區信任（workspace trust）、最近資料夾，以及鍵盤快捷鍵。

- **Claude Agent**：Claude Agent 可在 Agents 應用程式中使用，讓您可以將它與 Copilot CLI 或 Copilot Cloud 等其他 Agent 一起用於編碼任務。

- **Web 用戶端**：在瀏覽器中透過 [insiders.vscode.dev/agents](https://insiders.vscode.dev/agents) 存取 Agents 體驗，將 agent-native 工作流程帶到任何執行 Dev Tunnel 的機器上（透過 `code-insiders tunnel`）。要開始使用，請[下載 VS Code Insiders](https://code.visualstudio.com/insiders) 並執行 `code-insiders tunnel` 來設定 [Dev Tunnel](https://code.visualstudio.com/docs/remote/tunnels)。然後即可從網頁連線。

- **背景瀏覽器**：整合式瀏覽器現跨工作階段持續存在，當您返回工作階段時不再重新整理。這讓您在 Agent 工作時使用整合式瀏覽器預覽變更時，上下文切換更加順暢。

- **變更的版面控制**：當 Agent 進行變更時，您可以將 diff 檢視與 Chat 檢視並排開啟，或在模態視窗中開啟以專注於變更。使用 diff 檢視工具列中的版面控制在不同顯示模式之間切換。

- **動態標題列入口**：只需一鍵即可在 VS Code Insiders 與 Agents 應用程式之間切換。從 VS Code Insiders 標題列選取 **Open in Agents** 跳至 Agents 應用程式，或從 Agents 標題列選取 **Open in VS Code** 返回 Insiders 編輯器。

您的回饋有助於我們塑造 Agents 體驗——請繼續透過[在 GitHub 上提交 Issue](https://github.com/microsoft/vscode/issues) 與我們分享。您也可以瀏覽[現有 Issue](https://github.com/microsoft/vscode/issues?q=state%3Aopen%20label%3A%22agents-app%22) 了解其他人的回報，並在特定主題上提供您的回饋。

### Copilot CLI 工作階段遠端控制（實驗性）

**設定**：`github.copilot.chat.cli.remote.enabled`

過去，要與 Copilot CLI 工作階段互動，您必須待在啟動它的機器旁邊。如果 Agent 在等待核准時暫停，或在您不在位時遇到問題，工作就會停滯直到您回來。有了遠端控制，您可以從任何地方監控並操控正在執行的 Copilot CLI 工作階段，讓您有更大的彈性持續推動工作而不必被綁在機器旁。

[Copilot CLI 的遠端控制](https://code.visualstudio.com/docs/copilot/agents/copilot-cli)讓您從另一台裝置（使用 GitHub.com 或 GitHub 行動裝置 App）檢查進度、回應核准，並操控工作，同時您的 Copilot CLI 工作階段繼續在背景執行。

要試用遠端控制：

1. 啟用 `github.copilot.chat.cli.remote.enabled` 設定。
2. 在 Copilot CLI 聊天中輸入 `/remote on` 即可開始。

您隨時可以執行 `/remote` 檢視遠端控制狀態，或 `/remote off` 停用遠端控制。

### Copilot CLI 的同步工作階段標題

聊天工作階段標題用於不同的聊天介面，如聊天工作階段清單、聊天編輯器分頁與標頭，以及 Copilot CLI 終端機介面，為工作階段提供一致的識別碼。過去，取決於您在哪裡重新命名 [Copilot CLI](https://code.visualstudio.com/docs/copilot/agents/copilot-cli) 工作階段，其他聊天介面可能仍顯示舊標題。

VS Code 採用 Copilot SDK 的 session-title API 作為唯一來源（source of truth），並透過單一標題解析器（title resolver）路由工作階段清單與聊天編輯器標頭，確保顯示的標題在各個介面間保持一致。聊天工作階段清單、聊天編輯器分頁與標頭，以及終端機中的 `copilot --resume` 現在都會在您重新命名工作階段時保持同步，無論重新命名是從哪裡發起的。

在終端機中從 Copilot CLI 執行的重新命名，也會在下次讀取工作階段中繼資料時被 VS Code 接收。

### Git AI 共同作者預設啟用

VS Code 現在預設為聊天與 Agent 工作流程啟用 Git AI 共同作者（co-authoring）。當 Copilot 修改您的檔案時，Copilot 會自動被加入為該次提交的共同作者。

您可以使用 `git.addAICoAuthor` 設定來更改預設行為。

---

## 程式碼庫搜尋與上下文

### 非 GitHub 儲存庫的語意索引向所有使用者推出

當您問 Copilot 像「我們在哪裡處理使用者認證？」這樣的問題時，Agent 必須將您模糊的意圖轉化為確切的相關檔案與符號。純文字搜尋只能比對您輸入的字面文字，因此當您的程式碼庫使用不同術語時，通常會錯過相關程式碼。語意索引讓 Agent 能依「意義」搜尋，找出使用相關術語的檔案，例如 `login`、`signIn`、`verifyCredentials` 或 `OAuth token exchange`，即使 "authentication" 一詞從未出現在程式碼中。這讓 Agent 能為回答與編輯提供更好的基礎。

語意索引現在在所有工作區中可用。先前此功能僅限於使用 GitHub 或 ADO 儲存庫的工作區。

語意索引會自動建置與維護。使用 GitHub 或 ADO 儲存庫的工作區通常可以立即使用語意搜尋，而其他工作區可能需要幾分鐘來建立初始索引。您也可以使用 **Build Codebase semantic index** 命令明確地為目前工作區建立索引。

語意搜尋是 Copilot 用來理解您工作區的眾多工具之一。Copilot 會挑選最適合任務的工具，所以您通常不需要微觀管理它的搜尋方式。請參閱 [How Copilot understands your workspace](https://code.visualstudio.com/docs/copilot/reference/workspace-context) 文件，了解更多關於語意搜尋及 Copilot 使用的其他工具的詳情。

### GitHub 跨儲存庫或組織的文字搜尋

當 Agent 需要在您目前工作區之外的程式碼中查找精確的字串、API 名稱或錯誤訊息時，語意搜尋不一定是最適合的。您需要的是跨已知儲存庫或整個組織的精確比對，而非模糊比對。

為此，Copilot 現在內建了 `githubTextSearch` Agent 工具，它可以在 GitHub 儲存庫或整個 GitHub 組織的程式碼中進行 grep 風格的搜尋。這與既有的 `githubRepo` 工具互補，後者在 GitHub 儲存庫內進行語意搜尋。這兩個工具結合起來，讓 Agent 有更豐富的方式從您目前工作區以外的程式碼庫中學習。

若需更進階的 GitHub 功能，例如搜尋與管理 Issue 或 Pull Request，請考慮使用 [GitHub MCP 伺服器](https://github.com/github/github-mcp-server)。

### Skill 專屬上下文（實驗性）

**設定**：`github.copilot.chat.skillTool.enabled`

當您使用會執行多步驟工具呼叫或引入大量參考資料的 Skill 時，這些輔助內容可能會擠壓您的主要聊天上下文，並降低後續回應的品質。

您現在可以讓 Skill 在專屬的子代理上下文（dedicated subagent context）中執行，將其與主對話隔離，讓您的主要上下文保持專注，且 Skill 的回應維持更高品質。

要讓 Skill 在專屬子代理上下文中執行，請在 `SKILL.md` 的 frontmatter 中設定 `context` 屬性：

```yaml
---
name: my-skill
description: My skill description
context: fork
---
```

此功能為實驗性，需啟用 `github.copilot.chat.skillTool.enabled` 設定。

---

## Token 效率改善

4 月 27 日，GitHub 宣布 Copilot 將於 2026 年 6 月 1 日起改為[以用量計費（usage-based billing）](https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/)。為協助您從方案中獲得最大價值，我們一直在推動多項改善 Token 效率的措施，同時不影響 Agent 的品質。以下描述的大多數改善已經生效。若有可選擇加入的設定，會在相關章節中註明。

### 提示快取效率

在過去幾個迭代中，我們改善了系統提示、工具、對話歷程與摘要之間的快取重用，而不改變 Agent 的行為方式。實際上，這意味著重複的上下文會以更低的 Token 費率計費（例如，Anthropic 模型約低 10 倍），這有助於在較長的多輪次 Agent 工作流程中降低成本。

**策略性快取斷點放置。** 我們審計了快取斷點的設置位置，確保它們被有效使用並放置在穩定的邊界上：系統提示尾端、工具尾端、最近工具輪次尾端，以及對話輪次邊界。因此，一旦 Agent 工作階段開始運行，每次請求中**超過 93%** 的內容可從快取重用，而非被計為新的輸入。

**快取穩定的系統提示與工具清單。** 快取前綴的效果取決於它前面的位元組。我們檢視了系統提示與工具註冊路徑，移除了跨請求的位元組漂移來源。例如，新的 `chat.experimental.symbolTools.cacheStable` 設定以靜態描述註冊 `vscode_renameSymbol` 和 `vscode_listCodeUsages`，取代先前會依載入語言而變動的描述。這樣一來，當語言擴充功能在工作階段中途啟動時，不會再更改請求並重設快取。我們也重新排序了工具清單，讓延遲與非延遲的工具以可預測的方式分組，確保工具陣列的位元組在各輪次間保持一致。

**快取友善的背景壓縮。** 隨著工作階段變長，我們會在背景摘要較舊的輪次，讓 Agent 可以持續工作而不會耗盡上下文。模型在需要時仍可查詢先前輪次的工具結果與詳情。這些背景摘要現在重用與主 Agent 相同的快取上下文，讓長時間的多輪次工作階段顯著更加高效。

**最後兩則訊息斷點策略。** 在長時間的 Agent 工作階段中，較舊的輪次最終會落出可快取的視窗。我們現在將快取斷點錨定在**系統提示、工具清單，以及最近兩則訊息**上。此功能目前透過 `github.copilot.chat.anthropic.cacheBreakpoints.lastTwoMessages` 設定提供。

### 工具搜尋工具（Tool Search Tool）

工具搜尋工具透過將 Agent 的工具集分為兩組來保持請求精簡。一組緊湊的**始終可用**核心約 30 個工具（涵蓋約 88% 的工具呼叫）始終包含在內。其餘工具為**延遲載入**：它們的 schema 不會載入模型的上下文中，直到模型明確請求為止。當 Agent 需要延遲載入的功能時，它會呼叫 `tool_search`，這會執行客戶端的、基於嵌入的語意搜尋，並按需回傳最相關的結果。

結果是每個輪次都有一個穩定的、可快取的前綴，以及顯著更小的每輪次工具足跡，同時 Agent 仍然能存取完整的工具集。

工具搜尋工具已針對 Anthropic 模型（Claude Sonnet 4.5+ 與 Opus 4.5+）預設啟用，我們觀察到最高達 **20% 的 Token 節省**。在本次發行中，我們正透過 Responses API 將其推出至支援的 OpenAI 模型（**GPT-5.4 與 GPT-5.5**），早期 Insiders 結果顯示類似或更佳的節省。要在 GPT 模型上使用工具搜尋工具，請啟用 `github.copilot.chat.responsesApi.toolSearchTool.enabled` 設定。

### 用於搜尋與執行的新工具

本次發行帶來兩個新的專門 Agentic 工具：Search 與 Execution。兩者皆由小型、專門建置的模型驅動，執行成本顯著更低。經過超過一個月的飛行測試（flighting），我們看到了有前景的結果，Token 節省最高達 20%。

#### Agentic 搜尋工具

Agentic 搜尋工具負責程式碼庫探索與上下文檢索。當主 Agent 需要上下文時，它以自然語言描述要找的內容，然後搜尋工具接手。搜尋工具隨後執行獨立的流程，使用 grep、檔案搜尋、語意搜尋與檔案讀取來搜尋您的工作區，然後回傳最相關的結果。

在底層，此工具由一個微調過的小型語言模型驅動，被訓練為在最少輪次內平行執行多次搜尋。這個精簡的範圍保持了低延遲與低成本，同時不犧牲檢索品質。

推出將在接下來的一個月內持續進行，將這些節省帶給所有 Copilot Chat 使用者。

#### Agentic 執行工具

Agentic 執行工具負責處理任何與執行終端機命令相關的事項。當 Agent 需要執行測試或檢查建置時，它會將任務交給執行工具，由執行工具執行命令並回報結果。為保持範圍精簡，執行工具只能執行終端機命令，且每次呼叫上限為 10 次終端機呼叫，因此不會無限循環。

終端機輸出往往冗長且雜亂。任務完成後，執行工具會將輸出過濾為編碼 Agent 實際需要的部分，只傳回相關內容。將這項工作從主模型卸載到更小、更快的模型上，能避免冗長的輸出消耗您的 Token 使用量。

---

## 聊天效能與歷程

### OpenAI 模型支援 WebSocket

對於支援此功能的 OpenAI 模型，聊天請求現在使用 Responses API 上的 [WebSocket 模式](https://developers.openai.com/api/docs/guides/websocket-mode)。VS Code 不再為每個輪次開啟新的 HTTP 請求，而是維持一個持久的 WebSocket 連線，只發送新的輸入項目以及先前的回應 ID。伺服器保留對話狀態，這降低了後續輪次的請求大小與延遲，在有大量來回呼叫的 Agent 工作流程中特別明顯。我們的測量顯示，使用 WebSocket 讓 OpenAI 模型**快 12%**。

WebSocket 模式會在所選模型支援時自動使用，無需任何配置。

### Chronicle（實驗性）

**設定**：`github.copilot.chat.localIndex.enabled`

隨著您更加依賴 Copilot，您的聊天歷程成為一份有價值的記錄：您做了什麼、觸及了哪些檔案、參考了哪些 PR 與 Issue。但這份歷程很難回顧：捲動瀏覽過去的工作階段來回想昨天做了什麼或準備站立會議很慢，而且沒有簡單的方式跨工作階段提問或從自己的使用模式中學習。

Chronicle 透過在本地 SQLite 資料庫中追蹤您的聊天互動來解決這個問題。每次您聊天時，它會記錄工作階段中繼資料（分支、儲存庫、時間戳）、對話輪次、透過工具呼叫觸及的檔案，以及外部參考（PR、Issue、Commit），讓您可以按需搜尋與摘要您的編碼活動。Chronicle 還可以分析您的使用方式，給您個人化的建議，告訴您如何改善提示與工具使用。

Chronicle 提供幾個您可以在聊天中使用的命令，查詢您的工作階段歷程並獲得關於編碼活動的洞察：

- `/chronicle:standup`：從過去 24 小時的編碼工作階段產生站立報告，依功能／分支分組，附摘要、檔案清單與 PR 連結。

- `/chronicle:tips`：分析 7 天的使用紀錄，給予關於提示、工具使用與工作流程的個人化建議。

- `/chronicle [查詢]`：對工作階段歷程進行自由格式的自然語言查詢（例如「昨天我編輯了哪些檔案？」）。

此功能為實驗性，需啟用 `github.copilot.chat.localIndex.enabled` 設定。

---

## 信任與安全

### 核准帳號組織政策

企業現在可以使用 `ChatApprovedAccountOrganizations` 裝置政策，依據核准的 GitHub 組織成員資格來管控聊天與相關 AI 功能的啟用。

此政策協助組織在所有聊天入口點一致地套用 GitHub 帳號層級的政策。聊天功能在以下條件滿足前不會啟用：(1) 使用者已登入具有核准組織成員資格的 GitHub 帳號，且 (2) 帳號層級的政策已完成解析。這種 fail-closed（預設關閉）行為對於在 GitHub.com 上配置帳號層級政策、並需要在顯示聊天功能前強制檢查資格的企業特別有用。

了解更多關於[企業政策](https://code.visualstudio.com/docs/enterprise/policies)。

### 沙箱預設讀取權限

`$HOME` 目錄下的所有路徑不再自動啟用讀取權限。此更新加強了沙箱隔離，並確保命令只存取它們明確需要的檔案。

在沙箱中執行任何命令之前，讀取權限僅根據正在執行的命令添加，`$HOME` 目錄下的所有其他路徑皆被拒絕讀取存取。存取任意路徑會因讀取權限被拒絕而失敗。

預設情況下，工作區資料夾與沙箱暫存資料夾（在執行時管理沙箱配置）在 `$HOME` 目錄下被授予讀取權限。

---

## 無障礙功能

### 從問題輪播聚焦終端機的鍵盤快捷鍵

**設定**：`accessibility.verbosity.chatQuestionCarousel`

當 Copilot 透過由終端機互動觸發的問題輪播（question carousel）提問時，您現在可以按 `⌥T`（Windows、Linux 為 `Alt+T`）快速將焦點返回終端機。先前，唯一的導航方式是選取 **Focus Terminal** 按鈕。

按鈕的 aria 標籤現在也包含快捷鍵提示，讓螢幕閱讀器使用者更容易發現。您可以使用 `accessibility.verbosity.chatQuestionCarousel` 設定來控制導航提示是否出現在 carousel 的 aria 標籤中。

---

## 編輯器體驗

### Webview 中大型本地資源的最佳化載入

我們最佳化了 [Webview](https://code.visualstudio.com/api/extension-guides/webview) 載入本地資源的方式，以提升速度並降低記憶體使用。此變更讓任何使用 Webview 或自訂編輯器的擴充功能受益，同時也改善了 VS Code 內建功能，例如 Notebook 渲染。

VS Code 中的 Webview 使用 Service Worker 從工作區或主機檔案系統載入資源。Service Worker 攔截本地檔案的請求，然後透過 VS Code 的檔案系統呼叫代理。這讓我們不僅能從磁碟載入資源，也能從擴充功能貢獻的虛擬檔案系統載入。

先前，對於檔案系統請求，VS Code 會將整個檔案讀入緩衝區，然後發送給 Webview 的 Service Worker。這對幾個小的 JavaScript 和圖片檔案可行，但當您載入 20 個各為數十到數百 MB 的影片檔案時就不行了。

現在，我們以分塊串流（chunks）的方式將檔案內容串流到 Service Worker。這種方式改善了回應性，也減少了 VS Code 在移交給瀏覽器引擎之前必須累積的資料量。

我們進一步採用 [Transferable Streams](https://github.com/whatwg/streams/blob/main/explainers/transferable-streams.md#transferable-streams-explained) 來最佳化串流。檔案串流在 VS Code 主要渲染器程序中建立，並直接由 Webview Service Worker 中的 `new Response(...)` 消費。這繞過了先前多層的 `postMessage` 呼叫。

---

## 語言

### TypeScript 7.0 Beta 支援

我們持續與 TypeScript 團隊合作，改善 VS Code 對 [TypeScript 7](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0-beta/) 的支援。TypeScript 7 是以原生程式碼完全重寫，提供大幅改善的效能。

TypeScript 7.0 Beta 持續改善語言功能，也包含多項編輯器生活品質改善。我們也讓試用 TS 7.0 以及在它與目前穩定的 TS 6.0 之間來回切換更加容易。

要在 VS Code 中試用 TS 7.0，您只需安裝 [TypeScript Native Preview 擴充功能](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0-beta/#editor-experience)。

---

## 擴充功能貢獻

### Chat Customizations Evaluation 擴充功能

我們新增了一個擴充功能，**Chat Customizations Evaluations**（擴充功能 ID：`ms-vscode.vscode-chat-customizations-evaluations`），用於協助分析並改善您的聊天自訂項目，例如 prompt files、custom agents、instructions 與 skills。在分析自訂項目檔案後，此擴充功能會產生它偵測到的問題診斷，並給予改善建議。

開啟一個 prompt、agent、instructions 或 skill 定義檔，然後選取 **Analyze** 來進行評估。診斷出現後，使用 customization evaluations 修復 skill 來套用建議的變更。

---

## 遠端開發

[Remote Development 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)讓您可以使用 [Dev Container](https://code.visualstudio.com/docs/devcontainers/containers)、透過 SSH 或 [Remote Tunnels](https://code.visualstudio.com/docs/remote/tunnels) 的遠端機器，或 [Windows Subsystem for Linux](https://learn.microsoft.com/windows/wsl)（WSL）作為完整功能的開發環境。

### Dev Container Feature 的 Lockfile 預設啟用

**設定**：`dev.containers.lockfile`

我們預設啟用了 lockfile `devcontainer-lock.json`。Lockfile 在 Feature 首次安裝時記錄 Dev Container Feature 的版本與校驗碼，並將 Feature 釘選至該特定版本與校驗碼，以改善對供應鏈攻擊的抵禦能力。

當有較新版本可用時，編輯器會在 `devcontainer.json` 檔案中的 Dev Container Feature 上顯示 Code Lens。

[Dependabot 支援](https://containers.dev/guide/dependabot)也可用於自動提交 PR 來更新 lockfile。

更多資訊請參閱 Dev Container 規格中的 [Dev Container Feature Lockfile](https://github.com/devcontainers/spec/blob/main/docs/specs/devcontainer-lockfile.md)。

---

## 工程

### 使用 TypeScript 7 加速開發建置

VS Code 的開發監看任務（development watch task）現在使用 TypeScript 7 進行型別檢查。這大幅縮減了建置與完整型別檢查我們程式碼庫的時間。

先前，型別檢查 VS Code 主專案中約 6,000 個檔案需要約 60 秒。TypeScript 7 將全新建置（fresh build）縮短至約 10 秒。從啟動監看建置任務到 VS Code 及所有內建擴充功能建置完成且完全通過型別檢查，現在大約需要 30 秒。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 感謝

Issue 追蹤貢獻者：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@AbhitejJohn (Abhitej John)](https://github.com/AbhitejJohn)：將 skillContentRead 遙測屬性重新命名以使用 'skill' 前綴 [PR #311945](https://github.com/microsoft/vscode/pull/311945)
- [@andrewkchan (Andrew Chan)](https://github.com/andrewkchan)：輔助視窗 — 修正 setTimeout 洩漏 [PR #311824](https://github.com/microsoft/vscode/pull/311824)
- [@austinngan (Austin Ngan)](https://github.com/austinngan)：修正 Markdown 預覽捲動回饋迴路 (#303765) [PR #312237](https://github.com/microsoft/vscode/pull/312237)
- [@fishcharlie (Charlie Fish)](https://github.com/fishcharlie)：Chat：在模型選擇器中為重複的 BYOK 模型顯示提供者實例名稱 [PR #312028](https://github.com/microsoft/vscode/pull/312028)
- [@kevin-m-kent](https://github.com/kevin-m-kent)：為 vscode_renameSymbol 和 vscode_listCodeUsages 新增快取穩定模式（實驗性）[PR #312568](https://github.com/microsoft/vscode/pull/312568)
- [@maruthang (Maruthan G)](https://github.com/maruthang)
  - 修正：處理終端機工具執行中的 heredoc／多行命令 [PR #307960](https://github.com/microsoft/vscode/pull/307960)
  - 修正（chat）：在工具確認中為高度受限的程式碼區塊啟用捲軸 (#283242) [PR #310975](https://github.com/microsoft/vscode/pull/310975)
  - 修正：在 ScopedContextKeyService 中 disposeContext 之前清除父變更監聽器 [PR #307593](https://github.com/microsoft/vscode/pull/307593)
- [@mossgowild (moss)](https://github.com/mossgowild)：終端機工具：偵測最後輸出行的提示 [PR #311765](https://github.com/microsoft/vscode/pull/311765)
- [@ssg (Sedat Kapanoğlu)](https://github.com/ssg)：新增土耳其 DOS（CP 857）編碼支援 [PR #300114](https://github.com/microsoft/vscode/pull/300114)
- [@Tyriar (Daniel Imms)](https://github.com/Tyriar)：將 skip shell 處理移至服務並最佳化 [PR #311892](https://github.com/microsoft/vscode/pull/311892)
- [@winjo](https://github.com/winjo)：修正 AutoRepliesPtyServiceContribution 在程序釋放時的記憶體洩漏 [PR #312150](https://github.com/microsoft/vscode/pull/312150)
- [@xingsy97 (xingsy97)](https://github.com/xingsy97)：contextkey：修正掃描器對大於運算子回傳 '>=' 而非 '>' [PR #307059](https://github.com/microsoft/vscode/pull/307059)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - 修正：為設定指示器懸停使用 setupDelayedHover 以支援 Ctrl+K I [PR #304990](https://github.com/microsoft/vscode/pull/304990)
  - 為所有除錯主控台完成項目顯示詳細欄位 [PR #310379](https://github.com/microsoft/vscode/pull/310379)

`vscode-pull-request-github` 貢獻者：

- [@Will-hxw (Will-hxw)](https://github.com/Will-hxw)：修正（reviewManager）：在 hasBranch 呼叫中使用 pr.base.ref 取代 pr.base.name [PR #8698](https://github.com/microsoft/vscode-pull-request-github/pull/8698)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Remote Control | 遠端控制 |
| Copilot CLI Sessions | Copilot CLI 工作階段 |
| Codebase Search | 程式碼庫搜尋 |
| Semantic Indexing | 語意索引 |
| Semantic Search | 語意搜尋 |
| Text Search | 文字搜尋 |
| githubTextSearch | githubTextSearch 工具 |
| githubRepo | githubRepo 工具 |
| Dedicated Context | 專屬上下文 |
| Subagent Context | 子代理上下文 |
| context: fork | context: fork |
| Skill | Skill（技能） |
| SKILL.md | SKILL.md |
| Frontmatter | Frontmatter |
| Chronicle | Chronicle（歷程記錄） |
| Local SQLite Database | 本地 SQLite 資料庫 |
| Standup Report | 站立報告 |
| Session Metadata | 工作階段中繼資料 |
| /chronicle:standup | /chronicle:standup 命令 |
| /chronicle:tips | /chronicle:tips 命令 |
| Token Efficiency | Token 效率 |
| Usage-based Billing | 以用量計費 |
| Prompt Caching | 提示快取 |
| Cache Breakpoint | 快取斷點 |
| Cache-stable | 快取穩定 |
| Byte Drift | 位元組漂移 |
| Background Compaction | 背景壓縮 |
| Last-two-messages Breakpoint | 最後兩則訊息斷點 |
| Tool Search Tool | 工具搜尋工具 |
| Always-available Core | 始終可用核心 |
| Deferred Tools | 延遲載入工具 |
| Embedding-based Semantic Search | 基於嵌入的語意搜尋 |
| Agentic Search Tool | Agentic 搜尋工具 |
| Agentic Execution Tool | Agentic 執行工具 |
| Fine-tuned Small Language Model | 微調小型語言模型 |
| Flighting | 飛行測試（分階段推出） |
| WebSocket Mode | WebSocket 模式 |
| Responses API | Responses API |
| Persistent Connection | 持久連線 |
| ChatApprovedAccountOrganizations | ChatApprovedAccountOrganizations 政策 |
| Device Policy | 裝置政策 |
| Fail-closed | 預設關閉 |
| Sandbox Isolation | 沙箱隔離 |
| Explicit Path Access | 明確路徑存取 |
| Question Carousel | 問題輪播 |
| Webview | Webview |
| Service Worker | Service Worker |
| Chunked Streaming | 分塊串流 |
| Transferable Streams | Transferable Streams |
| postMessage | postMessage |
| TypeScript 7.0 Beta | TypeScript 7.0 Beta |
| Native Code Rewrite | 原生程式碼重寫 |
| TypeScript Native Preview Extension | TypeScript Native Preview 擴充功能 |
| Chat Customizations Evaluations | Chat Customizations Evaluations 擴充功能 |
| Prompt Files | Prompt Files |
| Custom Agents | 自訂 Agent |
| Instructions | Instructions |
| Dev Container Lockfile | Dev Container 鎖定檔 |
| devcontainer-lock.json | devcontainer-lock.json |
| Feature Version | Feature 版本 |
| Checksum | 校驗碼 |
| Supply Chain Attacks | 供應鏈攻擊 |
| Code Lens | Code Lens |
| Dependabot | Dependabot |
| Watch Task | 監看任務 |
| Fresh Build | 全新建置 |
| Type Checking | 型別檢查 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |
| Git AI Co-authoring | Git AI 共同作者 |
| git.addAICoAuthor | git.addAICoAuthor 設定 |
| Session Title | 工作階段標題 |
| Source of Truth | 唯一來源 |
| Title Resolver | 標題解析器 |
| VS Code Agents App | VS Code Agents 應用程式 |
| Companion App | 伴隨應用程式 |
| Agent-native | Agent 原生 |
| Claude Agent | Claude Agent |
| Dev Tunnel | Dev Tunnel |
| Background Browsers | 背景瀏覽器 |
| Layout Controls | 版面控制 |
| Diff View | Diff 檢視 |
| Modal Window | 模態視窗 |
| Dynamic Title Bar Entry Points | 動態標題列入口 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Remote Development | 遠端開發 |
| WSL | WSL（Windows Subsystem for Linux） |

---

*資料來源：[Visual Studio Code 1.118 發行說明](https://code.visualstudio.com/updates/v1_118)*
