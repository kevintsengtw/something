# Visual Studio Code 1.119 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.119
**發行日期：** 2026 年 5 月 6 日
**原文：** https://code.visualstudio.com/updates/v1_119

---

歡迎使用 Visual Studio Code 1.119 版本。本次發行聚焦於更流暢的 Agent 互動、增強的可觀測性，以及更高效的信任與安全控制。

- **[與 Agent 共享瀏覽器分頁](#agent-體驗)**：讓 Agent 發現並請求整合式瀏覽器的存取權限。
- **[最佳化 Token 使用量](#最佳化-token-使用量以管理-todo-清單實驗性)**：使用輕量模型管理 Agent 的 Todo 清單。
- **[OpenTelemetry 追蹤](#opentelemetry-追蹤-agent-工作階段)**：透過 OpenTelemetry 監控 Agent 工作階段。
- **[信任與開發者效率](#信任與安全)**：減少網路存取或暫存資料夾寫入請求的中斷。
- **[Markdown 預覽](#切換目前編輯器至-markdown-預覽)**：快速在 Markdown 原始碼和預覽之間切換。

---

## Agent 體驗

### 與 Agent 共享瀏覽器分頁

當 Agent 能夠存取即時瀏覽器時，它們可以即時驗證變更並更快地迭代。對於 Web 開發，Agent 可以編輯程式碼、重新載入頁面，並在單一回合中確認修正。對於設計工作流程，Agent 可以將渲染輸出與您的意圖進行比較，並即時調整佈局或樣式。開始使用：[在 VS Code 中搭配 Agent 使用整合式瀏覽器](https://code.visualstudio.com/docs/copilot/guides/browser-agent-testing-guide)。

Agent 不會自動取得[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)的存取權限。您需要明確地將瀏覽器頁面共享給 Agent，它才能與之互動。這有助於保護敏感資料的隱私。

在本次發行中，我們新增了與 Agent 共享瀏覽器的新方式：

#### 將瀏覽器分頁附加為上下文

瀏覽器分頁現在可以透過典型的進入點明確附加至聊天，例如建議的上下文、上下文選擇器和拖放動作。

當瀏覽器分頁被附加時，它會進入共享狀態，Agent 可以讀取並與頁面互動。使用瀏覽器中的共享按鈕即可在完成後停止共享。

Agent 現在會知道您有多少瀏覽器分頁已開啟但未共享。當它們需要與頁面互動時，可以請求共享已開啟的分頁，您可以在提示中核准或拒絕該請求。

當 Agent 嘗試在與現有未共享分頁相同的網域上開啟新分頁時，會出現提示詢問您是否要重用既有分頁。此做法旨在鼓勵分頁重用並減少雜亂。

### Visual Studio Code Agents（Insiders）

> **注意**：Visual Studio Code Agents 目前為預覽版，僅在 VS Code Insiders 中提供。

[Visual Studio Code Agents](https://aka.ms/VSCode/Agents/docs) 是隨 VS Code Insiders 一同發佈的配套體驗。它提供一個專注的、Agent 原生的環境，讓您可以跨儲存庫執行並行工作階段，並迭代多步驟的編碼任務。我們在 [1.115](https://code.visualstudio.com/updates/v1_115#_visual-studio-code-agents-preview) 首次引入 VS Code Agents，並持續根據使用者回饋進行改進。

本次發行的更新包括：

- **重新設計的新工作階段 Repo 選擇器**：開始新工作階段時，您現在可以輕鬆切換本機資料夾、儲存庫或遠端選項。
- **子工作階段改進**：建立和管理子工作階段持續改進，包括子工作階段分頁和生命週期處理的修正。
- **Web 和行動裝置改善**：我們持續改進在 [1.118](https://code.visualstudio.com/updates/v1_118#_web-client) 中引入的 [Agents Web 用戶端](https://insiders.vscode.dev/agents)，使瀏覽器體驗與桌面體驗保持一致。這包括行動裝置體驗的改進，讓您可以從行動裝置的瀏覽器建立和管理工作階段及其變更。
- **環境管理與持續性**：我們持續投資 VS Code 與 Agents 之間的連接，並改進環境的管理方式。這將在未來版本中持續完善。
- **進度 UX**：當 Agent 正在處理任務時，其進度現在更加可見，包括旋轉的進度訊息和聊天輸入框的邊框動畫。
- **開發者樂趣**：我們正在迭代 UX 機會以激發開發者樂趣，包括新工作階段頁面上的趣味彩蛋。啟用 sessions.developerJoy.enabled 看看您能否發現它！

您的回饋有助於我們塑造 Agents 體驗，請繼續透過[在 vscode GitHub 儲存庫中提交 Issue](https://github.com/microsoft/vscode/issues) 與我們分享。您也可以瀏覽[既有的 Issue](https://github.com/microsoft/vscode/issues?q=state%3Aopen%20label%3A%22agents-app%22) 查看其他人的報告，並對特定主題提供回饋。

### OpenTelemetry 追蹤 Agent 工作階段

**設定**：github.copilot.chat.otel.enabled、github.copilot.chat.otel.otlpEndpoint

隨著 Agent 工作階段變得更長且更自主，了解 Agent 做了什麼、每個步驟花了多長時間，以及 Token 消耗在哪裡，對於最佳化成本和偵錯意外行為至關重要。[OpenTelemetry](https://opentelemetry.io/) 是業界標準的可觀測性框架。

Copilot Chat Agent 工作階段（包括本機 Agent、Copilot CLI 背景 Agent 和 Claude Agent）現在會發出 OpenTelemetry 追蹤、指標和事件，遵循 [GenAI 語義慣例](https://github.com/open-telemetry/semantic-conventions/blob/main/docs/gen-ai/)，因此您可以在任何 OTLP 相容的後端（例如 [Aspire Dashboard](https://aspire.dev/dashboard/standalone/)）中監控 Agent 行為、延遲和 Token 使用。

每個使用者請求會產生一個 `invoke_agent` 根 span（例如 `invoke_agent claude`），包含巢狀的 `chat`、`execute_tool` 和 `execute_hook` 子 span。子 Agent 呼叫會自動掛載為呼叫 Agent 的 `execute_tool` span 的子項，讓您在單一連結追蹤中完整了解 Agent 的工作。Span 會報告模型和 Token 使用量，包括快取讀取和快取建立的明細。

若要試用，請啟用 github.copilot.chat.otel.enabled 並將 github.copilot.chat.otel.otlpEndpoint 指向您的收集器。

更多資訊請參閱 VS Code 文件中的[使用 OpenTelemetry 監控 Agent 使用](https://code.visualstudio.com/docs/copilot/guides/monitoring-agents)。

---

## 聊天體驗

### 顯示 Copilot CLI 和 Claude Agent 回應的模型詳情

**設定**：github.copilot.chat.agent.modelDetails.enabled

了解哪個模型處理了回應，以及如何計入您的使用量，有助於您掌控成本和品質。

聊天檢視中的 [Copilot CLI](https://code.visualstudio.com/docs/copilot/agents/copilot-cli) 和 [Claude Agent](https://code.visualstudio.com/docs/copilot/agents/third-party-agents) 回應現在會在每個回應上顯示模型及其計費倍率。徽章會在每個回應完成時即時顯示，無需重新載入視窗，並在您於工作階段中切換模型時更新。

當您在 Copilot CLI 中使用 **Auto** 模型選擇時，徽章會顯示實際使用的模型（例如 `Claude Sonnet 4.6`）而非 `auto`。解析後的模型在從歷史記錄重建工作階段時也會保留。

此行為預設啟用。若要關閉徽章，請停用 github.copilot.chat.agent.modelDetails.enabled 設定並重新載入視窗。

### 最佳化 Token 使用量以管理 Todo 清單（實驗性）

**設定**：github.copilot.chat.agent.backgroundTodoAgent.enabled

Todo 清單透過為 Agent 提供已完成事項和待辦事項的明確記錄，幫助 Agent 在複雜的多步驟任務中保持正軌。然而，主模型每次更新 Todo 清單的工具呼叫都會消耗 Token，而這些成本在長時間的工作階段中會累積。

透過將 Todo 清單管理卸載至輕量背景 Agent，主模型可以專注於實際任務，而較小的模型則保持進度追蹤同步。這在不犧牲保持 Agent 專注的引導下，減少了整體 Token 使用量。

啟用此設定時，背景 Agent 會監視主 Agent 的活動並更新 Todo 清單以反映已完成和進行中的工作。主 Agent 將不會擁有 todo 工具，從而節省對話的 Token 成本。

> **注意**：如果 todo 工具被手動新增至聊天請求（例如使用 `#todo`），或自訂 Agent 在其工具清單中指定了它，背景 Agent 將被覆寫且不會執行。

此功能預設停用。若要試用，請啟用 github.copilot.chat.agent.backgroundTodoAgent.enabled 設定。

### 用量計費更新

GitHub Copilot 自 6 月 1 日起過渡至[用量計費](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-individuals)。為此，本次發行包含對聊天狀態儀表板、聊天輸入通知和模型選擇器的內部變更，以支援顯示計費和額度資訊。這些 UI 更新尚未對使用者可見，將在用量計費推出時生效。

---

## 信任與安全

### 允許 Agent 沙箱中的網路存取

**設定**：chat.agent.sandbox.enabled

Agent 沙箱透過限制 Agent 工具可以存取的內容來保護您的系統，但嚴格的網路封鎖會在 Agent 需要安裝套件、呼叫 API 或執行開發伺服器時造成阻礙。

chat.agent.sandbox.enabled 設定現在新增了 `allowNetwork` 模式，該模式保留檔案系統限制但移除網路網域封鎖，因此您可以獲得沙箱保護而不會因為網路存取而不斷被中斷。

```json
"chat.agent.sandbox.enabled": "allowNetwork"
```

當沙箱允許網路存取時，chat.agent.allowedNetworkDomains 和 chat.agent.deniedNetworkDomains 設定會被忽略。

更多資訊請參閱 VS Code 文件中的 [Agent 沙箱](https://code.visualstudio.com/docs/copilot/concepts/trust-and-safety#_agent-sandboxing)。

### 自動核准對暫存資料夾的寫入（工作階段允許的命令）

**設定**：chat.tools.terminal.blockDetectedFileWrites

頻繁的例行檔案寫入核准提示會減慢 Agent 工作流程。當 chat.tools.terminal.blockDetectedFileWrites 設定為其預設值 `outsideWorkspace` 時，寫入工作區外的終端機命令需要核准，即使您已選擇 **Allow All Commands in Session**。

對作業系統暫存資料夾（macOS 和 Linux 上的 `/tmp`，Windows 上的 `%TEMP%`）的寫入，在 **Allow All Commands in Session** 啟用時現在免除此檢查。

這意味著常見的 Agent 工作流程（在暫存資料夾中暫存臨時檔案）不再中斷工作階段，而對工作區外其他位置的寫入仍需確認。

---

## 語言

### 切換目前編輯器至 Markdown 預覽

我們讓切換目前編輯器至 Markdown 預覽（以及切換回來）變得更容易。VS Code [很早就有此功能](https://code.visualstudio.com/updates/v1_63#_markdown-preview-custom-editor)，但它經常被忽略。這些新的按鈕和命令使其更容易被發現。

在 Markdown 檔案中，選擇工具列中的按鈕或執行 **Markdown: Switch to Preview View** 命令。

開啟預覽後，您可以選擇 **Switch to Editor View** 按鈕或命令切換回原始碼檢視。

### 重新組織的 Markdown 設定

為了幫助您發現和管理 [VS Code 內建 Markdown 支援](https://code.visualstudio.com/docs/languages/markdown)的設定，我們在設定編輯器中的 **Extensions** > **Markdown Language Features** 下建立了幾個基本分組。

所有設定 ID 保持不變，但現在所有與[內建 Markdown 預覽](https://code.visualstudio.com/docs/languages/markdown#_markdown-preview)相關的設定都列在 **Preview** 子區段下。

---

## 工程

### 完成 Webview 遷移至 CSS 錨點定位

VS Code 的 Webview 現在使用[錨點定位](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Anchor_positioning)在工作台中進行視覺定位。這改善了效能並使重新佈局更具回應性，特別是在有許多活躍 Webview 的情況下。這也讓我們修正了一些棘手的長期 bug，例如在 Web 上移動工作台時 Webview 位置偏移的問題。

以下是切換到錨點定位之前，單一 Webview 的典型重新佈局呼叫：

在此之前，Webview 的定位使用 JS 完成，會呼叫 `getBoundingClientRect`。此呼叫相對較慢，因為它會觸發瀏覽器樣式重新計算和重新佈局。

透過改用錨點定位，瀏覽器現在根據 CSS 為我們處理 Webview 的定位。

### 型別檢查現在使用 TypeScript 7 以加快開發迭代

上一個版本[我們將 VS Code 的主要監看任務改為使用 TypeScript 7](https://code.visualstudio.com/updates/v1_118#_faster-development-builds-with-typescript-7)。在本次版本中，我們完成了遷移，所有內建擴充功能和核心程式碼皆使用 TypeScript 7。

透過將 Copilot 擴充功能遷移至使用 TypeScript 7，我們將型別檢查時間從 22 秒縮短至 4 秒。這些顯著的加速使開發者和 Agent 都能在 VS Code 程式碼庫中更快速地迭代。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 chat.editMode.hidden 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 感謝

問題追蹤貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@64johnlee (john lee)](https://github.com/64johnlee)：fix: enable text selection in elicitation dialog markdown content [PR #313730](https://github.com/microsoft/vscode/pull/313730)
- [@aanil677](https://github.com/aanil677)：Fix minor grammatical issues in README [PR #312480](https://github.com/microsoft/vscode/pull/312480)
- [@AshtonYoon (Ashton Yoon)](https://github.com/AshtonYoon)：markdown: fix scroll sync regressions introduced in #287050 [PR #307763](https://github.com/microsoft/vscode/pull/307763)
- [@iideprived (Herbert Smith)](https://github.com/iideprived)：debug: default triggered breakpoint picker to first breakpoint [PR #313453](https://github.com/microsoft/vscode/pull/313453)
- [@Jah-yee (RoomWithOutRoof)](https://github.com/Jah-yee)：fix: resolve NoChangeError tool name interpolation and typo [PR #309709](https://github.com/microsoft/vscode/pull/309709)
- [@maruthang (Maruthan G)](https://github.com/maruthang)：webview: respect default localResourceRoots for custom editors [PR #312492](https://github.com/microsoft/vscode/pull/312492)
- [@OrenMe (Oren Me)](https://github.com/OrenMe)：Add structured preview for markdown customizations [PR #312545](https://github.com/microsoft/vscode/pull/312545)
- [@shaypet](https://github.com/shaypet)：Add compareBranch to TitleAndDescriptionProvider for enhanced PR context [PR #312326](https://github.com/microsoft/vscode/pull/312326)
- [@xAndreiLi (Andrei Li)](https://github.com/xAndreiLi)：feat(plugins): allow component paths within repository boundary [PR #308776](https://github.com/microsoft/vscode/pull/308776)
- [@yemohyleyemohyle](https://github.com/yemohyleyemohyle)
  - Yemohyle/add to telemetry [PR #311837](https://github.com/microsoft/vscode/pull/311837)
  - Yemohyle/add to ext telemetrey [PR #313159](https://github.com/microsoft/vscode/pull/313159)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - Add 'hint' and 'info' search keywords to editor.hover.enabled [PR #313491](https://github.com/microsoft/vscode/pull/313491)
  - Add 'pane' search keyword to editor group settings [PR #313490](https://github.com/microsoft/vscode/pull/313490)

`vscode-pull-request-github` 程式碼貢獻者：

- [@mohamedamara1 (Mohamed Amara)](https://github.com/mohamedamara1)：Display linked issue(s) from the PR Overview #5824 [PR #6835](https://github.com/microsoft/vscode-pull-request-github/pull/6835)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Sharing browser tabs with agents | 與 Agent 共享瀏覽器分頁 |
| Integrated browser | 整合式瀏覽器 |
| Sharing state | 共享狀態 |
| Context picker | 上下文選擇器 |
| Drag-and-drop | 拖放 |
| Tab reuse | 分頁重用 |
| Visual Studio Code Agents | Visual Studio Code Agents |
| Sub-session | 子工作階段 |
| Repo picker | Repo 選擇器 |
| OpenTelemetry tracing | OpenTelemetry 追蹤 |
| OTLP (OpenTelemetry Protocol) | OTLP |
| GenAI semantic conventions | GenAI 語義慣例 |
| Root span | 根 span |
| Child span | 子 span |
| Token usage | Token 使用量 |
| Cache read / Cache creation | 快取讀取 / 快取建立 |
| Model details | 模型詳情 |
| Multiplier | 計費倍率 |
| Auto model selection | Auto 模型選擇 |
| Background todo agent | 背景 Todo Agent |
| Usage-based billing | 用量計費 |
| Agent sandboxing | Agent 沙箱 |
| allowNetwork mode | allowNetwork 模式 |
| Allow All Commands in Session | 工作階段允許所有命令 |
| Temp folder | 暫存資料夾 |
| Markdown preview | Markdown 預覽 |
| Switch to Preview View | 切換至預覽檢視 |
| Switch to Editor View | 切換至編輯器檢視 |
| CSS anchor positioning | CSS 錨點定位 |
| Relayout | 重新佈局 |
| TypeScript 7 | TypeScript 7 |
| Typechecking | 型別檢查 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |

---

*資料來源：[Visual Studio Code 1.119 發行說明](https://code.visualstudio.com/updates/v1_119)*
