# Visual Studio Code 1.116 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.116
**發行日期：** 2026 年 4 月 15 日
**原文：** https://code.visualstudio.com/updates/v1_116

---

歡迎使用 Visual Studio Code 1.116 版本。本次發行持續讓聊天與 Agent 的使用更加強大與高效。以下是一些新功能亮點：

- **[Agent Debug Logs](#偵錯先前的-agent-工作階段)**：檢視先前 Agent 工作階段的紀錄，以理解與偵錯 Agent 互動。
- **[Copilot CLI 思考力度](#在-copilot-cli-中配置思考力度)**：在 Copilot CLI 中配置模型思考力度，以平衡回應品質與延遲。
- **[終端機 Agent 工具](#前景終端機支援-agent-工具)**：從 Agent 工作階段與任何終端機工作階段互動。
- **[GitHub Copilot 內建](#github-copilot-現為內建)**：無需安裝 GitHub Copilot Chat 擴充功能即可開始使用 AI。

Happy Coding!

---

## Agent 體驗

### 偵錯先前的 Agent 工作階段

**設定**：`github.copilot.chat.agentDebugLog.fileLogging.enabled`

Agent Debug Log 面板顯示聊天工作階段期間 Agent 互動的時序事件紀錄，對於理解當您送出提示後發生什麼事、以及偵錯聊天自訂項目非常有用。

您現在可以檢視目前工作階段以及先前工作階段的紀錄，紀錄會持久化儲存於本地磁碟。這讓您即使在工作階段結束後，仍可回顧與偵錯過去的 Agent 互動。

啟用 Agent Debug Logs 面板的設定現已合併至疑難排解設定 `github.copilot.chat.agentDebugLog.fileLogging.enabled`。

了解更多關於 [Agent Debug Logs 面板](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view#_agent-debug-log-panel)的文件說明。

### 在 Copilot CLI 中配置思考力度

與本地 Agent 工作階段類似，您現在可以透過語言模型選擇器，在 Copilot CLI 工作階段中配置推理模型的思考力度。思考力度控制模型對每次請求投入多少推理，可根據您的需求協助平衡回應品質與延遲。

在選擇器中選擇推理模型，然後選取箭頭以顯示可用的力度等級。可用的力度等級可能因模型而異。非推理模型不會顯示子選單。

了解更多關於[思考力度與推理](https://code.visualstudio.com/docs/copilot/concepts/language-models#_thinking-and-reasoning)的文件說明。

### 自訂項目歡迎頁面

Chat Customizations 對話框（可透過 **Chat: Open Customizations** 命令或 Chat 檢視中的齒輪圖示開啟）現在有一個歡迎頁面，提供您所有 Agent 自訂項目的概覽。

首次建立自訂項目可能令人卻步，因此您現在可以在歡迎頁面上使用 **Customize Your Agent** 輸入框，以自然語言描述讓 VS Code 起草 agents、skills 與 instructions 等自訂項目。

了解更多關於自訂 Agent 的 [Agent 自訂項目文件](https://code.visualstudio.com/docs/copilot/customization/overview)。

### 工具確認輪播（實驗性）

**設定**：`chat.tools.confirmationCarousel.enabled`

為了讓核准或拒絕多個工具呼叫更有效率，聊天現在會為工具確認顯示輪播控制項。輪播提供一種緊湊且可導航的方式，讓您依序審查並核准多個工具呼叫，無需在對話中捲動。

此功能為實驗性，由 `chat.tools.confirmationCarousel.enabled` 設定控制。在 VS Code Insiders 中預設啟用，正逐步推出至 Stable 版本。

### Visual Studio Code Agents（Insiders）

> **注意**：Visual Studio Code Agents 應用程式目前為預覽版，僅在安裝 VS Code Insiders 時可用。

在上一個版本中，我們分享了 **Visual Studio Code Agents** 應用程式，一個與 VS Code Insiders 一同發行的全新預覽版伴隨應用程式，專為 agent-native 開發打造。

自 1.115 推出以來，我們持續根據回饋迭代功能與修正，以提供卓越的 agent-first 體驗。

一些最新更新包括：

- **推理等級選擇**：如上所述，您現在可以在 Copilot CLI 工作階段中為推理模型配置思考力度。
- **Plan 模式處理**：對於涉及規劃的 CLI 工作階段，Plan 模式會自動啟動。
- **Files 分頁預設顯示在 Changes 中**：**Files** 分頁現在預設顯示在 Changes 面板中。
- **工作階段回應、主題與渲染改善**：回應處理、視覺一致性與渲染效能的一系列改進。
- **應用程式名稱**：我們已將應用程式重新命名為 **Visual Studio Code Agents - Insiders**。

我們在 VS Code 歡迎頁面新增了一個 `Try out the new Agents app` 的入口。

您也仍可透過與 1.115 相同的方式開啟應用程式：

- 從作業系統的開始功能表或應用程式資料夾啟動 **Visual Studio Code Agents - Insiders**。
- 從命令面板執行 **Chat: Open Agents Application**。

### 前景終端機支援 Agent 工具

`send_to_terminal` 和 `get_terminal_output` Agent 工具現在也可與前景終端機搭配使用，不再僅限於由 Agent 建立的背景終端機。這意味著 Agent 可以讀取終端機面板中任何可見終端機的輸出，並向其送出輸入，例如執行中的 REPL 或互動式腳本。

### 終端機輸入改善

本次發行包含 Agent 工作階段中終端機輸入體驗的多項改善：

- **偵測終端機輸入**：移除了基於 LLM 的提示輸入偵測（prompt-for-input detection）。先前，每個終端機輸出區塊都會觸發額外的 LLM 呼叫來分類終端機是否在等待輸入，這增加了延遲並使用額外的 Token。Agent 現在透過 `send_to_terminal` 直接處理終端機輸入，並在需要時使用問題輪播將控制權交給您。

- **進度訊息**：當 Agent 向終端機送出回答時，進度訊息現在會顯示正在回答哪個問題，例如：`Sending "my-project" to terminal (replying to: What is your project name?)`。

- **Focus Terminal**：當 Agent 需要終端機輸入（例如提示輸入密碼或 `npm init` 等互動式安裝程式）時，問題輪播現在包含 **Focus Terminal** 按鈕。選取它可聚焦至相關終端機並直接輸入您的回應。如果您在輪播開啟時開始在終端機中輸入，它會自動關閉並通知 Agent 您正在直接處理輸入。

### 背景終端機通知預設啟用

**設定**：`chat.tools.terminal.backgroundNotifications`

背景終端機通知現在預設啟用。當 Agent 在背景終端機中執行命令時，它會在命令完成、逾時或需要輸入時自動收到通知。這讓 Agent 能更快速且準確地回應，無需輪詢終端機輸出。

---

## Chat UX

本次發行包含聊天的幾項 UX 改善：

- **Diff 在頂層顯示**：程式碼 Diff 現在直接在聊天對話中渲染，讓您無需切換到獨立的 diff 檢視即可審查建議的變更。

- **渲染效能**：聊天回應現在應該渲染得更快，改善包括減少版面抖動（layout thrashing）和串流期間更有效率的增量更新。也修正了工具呼叫更新的快速連發導致擴充功能主機短暫卡頓的問題。

- **聊天送出效能**：修正了聊天訊息送出被載入聊天自訂項目所阻塞的問題。即使提示仍在載入中，訊息現在會立即在聊天對話中視覺呈現。

- **子代理進度**：子代理進度的展開檢視現在在視覺上更加鮮明，讓您更容易追蹤子代理正在執行的進度。

---

## 無障礙功能

### Agents 應用程式無障礙功能

Agents 應用程式（可在 VS Code Insiders 中使用）現在為鍵盤與螢幕閱讀器使用者提供全面的無障礙支援。

- **無障礙說明對話框**：在聊天輸入框聚焦時按 `Alt+F1`（macOS 為 `Option+F1`）可開啟無障礙說明對話框。它提供 Agents 應用程式的概覽、列出可用檢視，並顯示在它們之間導航的快捷鍵。

- **鍵盤導航命令**：新的快捷鍵讓您可以快速聚焦 Agents 應用程式中的關鍵檢視：
  - `Focus Changes View`
  - `Focus Chat Customizations View`
  - `Focus Files Explorer View`

  這些快捷鍵的範圍限定在 Agents 視窗中，不會覆蓋其在標準 VS Code 中的對應項。

- **詳細度設定**：`accessibility.verbosity.sessionsChat` 設定控制聊天輸入框是否朗讀關於開啟無障礙說明的 ARIA 提示。停用它可隱藏該朗讀。

- **ARIA 標籤與地標**：輔助列（auxiliary bar）現在標記為帶有描述性標籤的 complementary 地標，工作區選擇器按鈕具有有意義的 ARIA 標籤，工作階段清單項目包含建立時間的上下文。

### 螢幕閱讀器的鍵盤快捷鍵搜尋結果說明

在 Keyboard Shortcuts 編輯器中搜尋時，螢幕閱讀器現在會朗讀導航至搜尋結果的說明。NVDA 及其他螢幕閱讀器會朗讀「Use Ctrl+Down Arrow to access the searched shortcut details」，讓您可以快速導航至結果表格。您可以使用 `accessibility.verbosity.keyboardShortcuts` 設定停用此朗讀。

---

## 整合式瀏覽器

[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)現在透過兩個新入口更容易存取：

- **View** 選單，位於 **View** > **Browser**
- 鍵盤快捷鍵

這些動作會在沒有分頁開啟時開啟整合式瀏覽器，或讓您快速查看並跳至現有分頁。

這些新入口是先前既有入口的補充：

- **Browser: Open Integrated Browser** 命令
- 點擊 localhost 網站連結（`workbench.browser.openLocalhostLinks`）
- 標題列圖示（`workbench.browser.showInTitleBar`）
- 要求 Agent 開啟或與瀏覽器互動（`workbench.browser.enableChatTools`）

---

## 語言

### JS/TS Chat Features 擴充功能（Preview）

**設定**：`jsts-chat-features.skills.enabled`

全新的內建 JS/TS Chat Features 擴充功能增強了 Copilot 處理 TypeScript 與 JavaScript 的能力。在這個首次發行中，此擴充功能貢獻了設定現代 TypeScript 專案的 skills。我們計畫在未來的版本中增強並擴展其功能。

要試用這些 skills，請啟用 `jsts-chat-features.skills.enabled` 設定。

---

## 工程

### GitHub Copilot 現為內建

GitHub Copilot Chat 現在是 VS Code 的內建擴充功能。新使用者不再需要安裝任何擴充功能即可開始使用 Copilot 功能，如聊天、行內建議與 Agents。Copilot 作為標準 VS Code 安裝的一部分，開箱即用。

此變更是我們持續努力將 VS Code 打造為開源 AI 程式碼編輯器的一環。透過將 Copilot 作為內建擴充功能發行，我們降低了新使用者的門檻，並確保 AI 驅動的功能從首次啟動起就無縫整合。

既有使用者不受此變更影響。如果您已安裝 Copilot 擴充功能，它會繼續如往常般運作。

如先前一樣，如果您不希望使用 AI 功能，可以使用 `chat.disableAIFeatures` 設定來停用。

---

## 企業

### 過濾 Agent 網路存取的群組政策

管理員現在可以使用群組政策來控制 Agent 工具可以存取哪些網路網域。當 `chat.agent.networkFilter` 設定透過政策啟用時，Agent 工具（如 fetch 工具與整合式瀏覽器）的網路存取會根據允許與拒絕的網域清單進行限制。

- `chat.agent.allowedNetworkDomains`：指定 Agent 工具可以存取的網域。支援萬用字元，例如 `*.example.com`。
- `chat.agent.deniedNetworkDomains`：指定哪些網域被封鎖。拒絕的網域優先於允許的網域。

當網路過濾啟用且兩個清單皆為空時，所有網域都會被封鎖。當 `chat.agent.sandbox.enabled` 也啟用時，網路網域規則會額外適用於終端機沙箱。

這些政策使用 `ChatAgentNetworkFilter`、`ChatAgentAllowedNetworkDomains` 與 `ChatAgentDeniedNetworkDomains` 鍵配置。了解更多關於[企業政策](https://code.visualstudio.com/docs/enterprise/policies)的文件說明。

---

## 擴充功能貢獻

### GitHub Pull Requests

[GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) 擴充功能持續進展，讓您可以處理、建立與管理 Pull Request 和 Issue。新功能包括：

- 新增建立 Pull Request 的聊天工具。
- Worktree 也可以從「Delete Local Branches and Remotes」命令中刪除。

請查閱此擴充功能 [0.136.0 版本的 changelog](https://github.com/microsoft/vscode-pull-request-github/blob/main/CHANGELOG.md#01360) 了解完整內容。

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

- [@AndreasArvidsson (Andreas Arvidsson)](https://github.com/AndreasArvidsson)：修正 TextmateSnippet clone 方法以正確指派 _children [PR #295555](https://github.com/microsoft/vscode/pull/295555)
- [@gryan11 (Gabriel Ryan)](https://github.com/gryan11)：修正：在測試 mock 類別中新增缺少的 override 修飾詞 [PR #308558](https://github.com/microsoft/vscode/pull/308558)
- [@maruthang (Maruthan G)](https://github.com/maruthang)
  - 修正：在聊天串流期間保留程式碼區塊工具列的可見性 [PR #307978](https://github.com/microsoft/vscode/pull/307978)
  - 修正：從行內測試輸出訊息中移除 ANSI 跳脫碼 [PR #308161](https://github.com/microsoft/vscode/pull/308161)
  - 修正：在首次啟動時解析 Markdown 檔案的預設檢視 [PR #308739](https://github.com/microsoft/vscode/pull/308739)
- [@romalpani (Rohan Malpani)](https://github.com/romalpani)：功能：以尋找小工具和標頭動作增強工作階段檢視 [PR #307679](https://github.com/microsoft/vscode/pull/307679)
- [@winstliu (Winston Liu)](https://github.com/winstliu)：修正 --prof-startup 始終無法分析渲染器／擴充功能主機 [PR #307849](https://github.com/microsoft/vscode/pull/307849)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - 為失敗的測試新增捲軸指示器 [PR #307996](https://github.com/microsoft/vscode/pull/307996)
  - 修正：檢查 failureInVisibleDocument peek 的訊息位置可見性 [PR #308697](https://github.com/microsoft/vscode/pull/308697)
  - 在 gutter 中 Alt+click 顯示中斷點小工具 [PR #308687](https://github.com/microsoft/vscode/pull/308687)
  - 修正：在除錯主控台中從文字選取排除來源註解 [PR #308925](https://github.com/microsoft/vscode/pull/308925)
- [@zackbach (Zack Eisbach)](https://github.com/zackbach)：在 `tokenTypes` 中新增 `regex` 支援 [PR #304885](https://github.com/microsoft/vscode/pull/304885)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Agent Debug Logs | Agent 偵錯紀錄 |
| Agent Debug Log Panel | Agent Debug Log 面板 |
| Chronological Event Log | 時序事件紀錄 |
| Persisted Locally on Disk | 持久化儲存於本地磁碟 |
| Thinking Effort | 思考力度 |
| Reasoning Models | 推理模型 |
| Language Model Picker | 語言模型選擇器 |
| Effort Levels | 力度等級 |
| Customizations Welcome Page | 自訂項目歡迎頁面 |
| Chat Customizations | 聊天自訂項目 |
| Customize Your Agent | 自訂您的 Agent |
| Natural Language Description | 自然語言描述 |
| Tool Confirmation Carousel | 工具確認輪播 |
| Compact and Navigable | 緊湊且可導航 |
| VS Code Agents App | VS Code Agents 應用程式 |
| Companion App | 伴隨應用程式 |
| Agent-native Development | Agent 原生開發 |
| Agent-first Experience | Agent 優先體驗 |
| Reasoning Level Selection | 推理等級選擇 |
| Plan Mode | Plan 模式 |
| Files Tab | Files 分頁 |
| Changes Panel | Changes 面板 |
| Foreground Terminals | 前景終端機 |
| Background Terminals | 背景終端機 |
| send_to_terminal | send_to_terminal 工具 |
| get_terminal_output | get_terminal_output 工具 |
| REPL | REPL（互動式直譯器） |
| LLM-based Prompt-for-input Detection | 基於 LLM 的提示輸入偵測 |
| Question Carousel | 問題輪播 |
| Focus Terminal Button | Focus Terminal 按鈕 |
| Background Terminal Notifications | 背景終端機通知 |
| Diffs in the Top Level | Diff 在頂層顯示 |
| Layout Thrashing | 版面抖動 |
| Incremental Updates | 增量更新 |
| Subagent Progress | 子代理進度 |
| Accessibility Help Dialog | 無障礙說明對話框 |
| Keyboard Navigation Commands | 鍵盤導航命令 |
| Focus Changes View | Focus Changes View |
| Focus Chat Customizations View | Focus Chat Customizations View |
| Focus Files Explorer View | Focus Files Explorer View |
| ARIA Labels and Landmarks | ARIA 標籤與地標 |
| Complementary Landmark | Complementary 地標 |
| Verbosity Setting | 詳細度設定 |
| Integrated Browser | 整合式瀏覽器 |
| JS/TS Chat Features Extension | JS/TS Chat Features 擴充功能 |
| Skills | Skills（技能） |
| Built-in Extension | 內建擴充功能 |
| Inline Suggestions | 行內建議 |
| Open Source AI Code Editor | 開源 AI 程式碼編輯器 |
| chat.disableAIFeatures | chat.disableAIFeatures 設定 |
| Group Policy | 群組政策 |
| Network Filter | 網路過濾 |
| Allowed / Denied Network Domains | 允許／拒絕的網路網域 |
| Wildcards | 萬用字元 |
| Terminal Sandbox | 終端機沙箱 |
| ChatAgentNetworkFilter | ChatAgentNetworkFilter 政策 |
| GitHub Pull Requests Extension | GitHub Pull Requests 擴充功能 |
| Worktree | Worktree |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |

---

*資料來源：[Visual Studio Code 1.116 發行說明](https://code.visualstudio.com/updates/v1_116)*
