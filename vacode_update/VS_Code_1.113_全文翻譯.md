# Visual Studio Code 1.113 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.113
**發行日期：** 2026 年 3 月 25 日
**原文：** https://code.visualstudio.com/updates/v1_113

---

歡迎使用 Visual Studio Code 1.113 版本。本次發行包含了 Agent 與開發人員體驗方面的多項改善。

以下是本次發行的主要亮點：

- **聊天自訂項目**：從單一的統一介面管理所有聊天相關的自訂項目
- **可設定的思考力度**：直接從 UI 控制模型的推理層級
- **巢狀子代理**：允許子代理呼叫其他子代理，以處理複雜的多步驟工作流程
- **CLI Agent 功能**：在 CLI Agent 中使用 MCP 伺服器、分叉工作階段及檢視除錯日誌
- **圖片預覽**：使用全功能圖片檢視器預覽聊天中的圖片
- **預設主題更新**：透過更新的預設亮色和暗色主題，享受煥然一新的外觀

---

## GitHub Copilot

### 聊天自訂項目（Chat Customizations）

聊天自訂項目編輯器提供了一個集中化的 UI，讓您在一個地方建立和管理所有聊天自訂項目。

若要開啟編輯器，請選取聊天檢視中的「設定聊天」（齒輪圖示），或從命令面板（⇧⌘P，Windows/Linux 為 Ctrl+Shift+P）執行「Chat: Open Chat Customizations」。

編輯器將自訂類型組織成獨立的分頁，例如自訂指令（Custom Instructions）、提示檔案（Prompt Files）、自訂 Agent（Custom Agents）和 Agent 技能（Agent Skills）。它還提供了一個內嵌的程式碼編輯器，支援語法醒目提示和驗證。

您可以從頭建立新的自訂項目，或使用 AI 根據您的專案產生初始內容。若要新增 MCP 伺服器和 Agent 外掛程式，可以直接從編輯器瀏覽對應的市集。

### 可設定的思考力度（Configurable Thinking Effort）

支援推理的模型（例如 Claude Sonnet 4.6 和 GPT-5.4）現在會在模型選擇器中直接顯示一個「Thinking Effort」（思考力度）子選單，讓您可以控制模型對每個請求施加多少推理，而無需導覽至 VS Code 設定。

在選擇器中選擇一個推理模型，然後選取箭頭以顯示可用的力度等級。VS Code 會記住每個模型的所選力度等級，並跨對話保留。

模型選擇器標籤現在也會顯示所選的力度等級，例如「GPT-5.3-Codex · Medium」，讓您更容易看到每個模型目前啟用的力度等級。

非推理模型不會顯示此子選單。

> **已棄用：** `github.copilot.chat.anthropic.thinking.effort` 和 `github.copilot.chat.responsesApiReasoningEffort` 設定已被棄用。使用者現在應透過語言模型選擇器直接設定思考力度。

### 巢狀子代理（Nested Subagents）

子代理現在可以呼叫其他子代理，為複雜的多步驟工作流程啟用更精密的協調模式。

先前，子代理被限制不能呼叫其他子代理，以防止無限遞迴。透過新設定 `chat.subagents.allowInvocationsFromSubagents`，您可以在需要時啟用此功能。

#### 協調者模式（Orchestration Patterns）

子代理支援協調者模式，其中一個協調者 Agent 將工作委派給專門的工作者 Agent。這種方法幫助您建構精密的工作流程，同時讓每個 Agent 專注於其最擅長的工作。

例如，一個協調者 Agent 管理整體任務並將子任務委派給專門的子代理。每個工作者 Agent 可以擁有量身定制的工具組合。例如，規劃和審查 Agent 只需要唯讀存取權限，而實作者需要編輯功能。

#### 控制子代理呼叫行為

您可以透過 Agent 屬性控制子代理的呼叫行為。`disable-model-invocation` 屬性可防止 Agent 被其他 Agent 作為子代理呼叫（預設為 false），應在 Agent 僅應由使用者明確觸發時設為 true。

### CLI Agent 功能增強（CLI Agent Capabilities）

CLI Agent 功能已增強，可在 CLI Agent 中使用 MCP 伺服器、分叉工作階段和檢視除錯日誌。

#### MCP 伺服器支援

本次發行為 Copilot CLI（實驗性）和 Claude Agent 新增了 MCP 伺服器支援。使用 CLI Agent 的開發人員將發現 MCP 伺服器直接橋接，因此不需要在編輯器與命令列之間重新設定工具。

#### 分叉工作階段（Fork Sessions）

從 VS Code 1.113 開始，您現在可以在 Copilot CLI（實驗性）和 Claude Agent 中分叉工作階段。分叉工作階段可讓您在對話歷史的任何時間點建立現有工作階段的副本，當您想要探索不同的思路或嘗試不同的提示，而不失去原始工作階段的上下文時，這非常有用。

#### 除錯日誌（Debug Logs）

Agent 除錯日誌面板是理解您發送提示後發生什麼事的主要工具，顯示聊天工作階段中 Agent 互動的時間順序事件日誌。CLI Agent 現在也可以存取此功能。

### 圖片預覽（Images Preview）

當您在聊天中使用圖片時，無論是將截圖附加到請求中，還是 Agent 透過工具呼叫產生圖片，您現在都可以選取任何圖片附件，在全功能圖片檢視器體驗中開啟它。

#### 導覽（Navigation）

使用箭頭按鈕、鍵盤方向鍵或底部的縮圖條，瀏覽目前聊天工作階段中的所有圖片。

#### 分組（Sections）

圖片依對話回合分組，讓您可以看到哪些圖片來自特定的請求或回應。

#### 縮放與平移（Zoom & Pan）

點擊以放大，使用 Option+Click（Mac）或 Ctrl+Click（Windows/Linux）縮小，或滾動/捏合以連續縮放。

#### 檔案總管檢視存取

圖片檢視器現在也可從檔案總管檢視的右鍵選單中存取圖片檔案。當您選取「在圖片預覽中開啟」（Open in Images Preview）時，檢視器會開啟並顯示目前資料夾中的所有圖片。

#### 設定

這兩個功能預設都已啟用。若要獨立設定它們，請使用 `imageCarousel.chat.enabled` 和 `imageCarousel.explorerContextMenu.enabled`。

### 釘選聊天工作階段（Pinned Chat Sessions）

聊天工作階段現在可以釘選，讓重要的對話保持可存取，無需捲動歷史記錄。釘選功能讓您可以快速回到經常參考的對話，而不會在持續累積的工作階段列表中遺失它們。

### 可點擊的斜線命令（Clickable Slash Commands）

斜線命令（例如 `/fix` 或 `/explain`）現在可以點擊，讓您可以在發送前檢視或修改其參數。這提供了更好的控制，讓您在提交請求之前確認命令的意圖和範圍。

### run_in_terminal 工具改善

`run_in_terminal` 工具現在在命令逾時時，會明確標示輸出為已截斷（truncated），而不是靜默地返回部分結果。這讓使用者和 Agent 都能清楚知道命令輸出是不完整的，從而做出適當的後續處理。

---

## 整合式瀏覽器（Integrated Browser）

### 暫時信任自簽憑證

您現在可以選擇暫時信任無法驗證的憑證，以解除在使用自簽憑證的情境中開發時的阻礙。這對於使用自簽憑證的本地開發伺服器（localhost HTTPS）特別有用，讓您無需額外設定即可繼續開發工作。

### 瀏覽器分頁管理

瀏覽器分頁的右鍵選單現在新增了關閉同一群組中所有瀏覽器分頁的選項，類似於現有的「全部關閉」項目。所有群組的瀏覽器分頁也可透過命令面板關閉。

此外，新增了一個「快速開啟」（Quick Open）命令，可篩選已開啟的分頁，以便快速存取或關閉群組中的所有分頁。

---

## 工作區（Workbench）

### 預設主題更新（Default Themes Refresh）

VS Code 現在隨附全新的預設主題：「VS Code Light」和「VS Code Dark」。這些主題旨在提供煥然一新的現代外觀，同時維持先前預設「Modern」主題的熟悉度和可用性。

OS 主題同步將為新使用者預設使用新主題，讓 VS Code 自動匹配您作業系統的亮色/暗色模式與新主題。

對於既有使用者，先前的主題偏好設定將繼續生效。您可以隨時透過設定手動切換至新的預設主題。

---

## 終端機（Terminal）

### run_in_terminal 輸出截斷標示

`run_in_terminal` 工具現在在命令逾時時，會明確標示輸出為已截斷，而不是靜默地返回部分結果。此改善確保使用者和 Agent 都能了解輸出的完整性狀態，從而避免基於不完整資訊做出錯誤判斷。

---

## 無障礙功能（Accessibility）

### 改善隱含內容的螢幕閱讀器標籤

本次發行改善了隱含內容（implicit content）的螢幕閱讀器標籤。當 UI 元素包含隱含的語義或狀態時，螢幕閱讀器現在能更準確地傳達這些資訊給使用者，提升視障使用者的操作體驗。

---

## 重要修正（Notable Fixes）

- **WSL「在檔案總管中顯示」修正**：修正了連線至 WSL（Windows Subsystem for Linux）時，「在檔案總管中顯示」（Reveal in File Explorer）功能無法正常運作的問題。使用者現在可以在 WSL 遠端工作階段中正確地在 Windows 檔案總管中顯示檔案。

- **圖片輪播縮放支援**：圖片輪播檢視器現在支援縮放功能，讓使用者可以更詳細地檢視圖片內容。

---

## 新設定與已棄用設定摘要

### 新設定

| 設定 | 說明 |
|------|------|
| `chat.subagents.allowInvocationsFromSubagents` | 啟用子代理呼叫其他子代理的功能 |
| `imageCarousel.chat.enabled` | 啟用或停用聊天中的圖片預覽功能 |
| `imageCarousel.explorerContextMenu.enabled` | 啟用或停用檔案總管右鍵選單中的圖片預覽選項 |

### 已棄用設定

| 設定 | 替代方案 |
|------|---------|
| `github.copilot.chat.anthropic.thinking.effort` | 改用模型選擇器中的 Thinking Effort 子選單 |
| `github.copilot.chat.responsesApiReasoningEffort` | 改用模型選擇器中的 Thinking Effort 子選單 |

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Chat Customizations | 聊天自訂項目 |
| Chat Customizations Editor | 聊天自訂項目編輯器 |
| Custom Instructions | 自訂指令 |
| Prompt Files | 提示檔案 |
| Custom Agents | 自訂 Agent |
| Agent Skills | Agent 技能 |
| Configurable Thinking Effort | 可設定的思考力度 |
| Reasoning | 推理 |
| Model Picker | 模型選擇器 |
| Effort Level | 力度等級 |
| Nested Subagents | 巢狀子代理 |
| Orchestration Patterns | 協調者模式 |
| Coordinator Agent | 協調者 Agent |
| Worker Agent | 工作者 Agent |
| Disable Model Invocation | 停用模型呼叫 |
| CLI Agent Capabilities | CLI Agent 功能 |
| Fork Sessions | 分叉工作階段 |
| Debug Logs | 除錯日誌 |
| MCP Servers | MCP 伺服器 |
| Images Preview | 圖片預覽 |
| Image Carousel | 圖片輪播 |
| Zoom & Pan | 縮放與平移 |
| Thumbnail Strip | 縮圖條 |
| Pinned Chat Sessions | 釘選聊天工作階段 |
| Clickable Slash Commands | 可點擊的斜線命令 |
| run_in_terminal | run_in_terminal 工具 |
| Truncated Output | 已截斷的輸出 |
| Integrated Browser | 整合式瀏覽器 |
| Self-Signed Certificate | 自簽憑證 |
| Temporary Trust | 暫時信任 |
| Quick Open | 快速開啟 |
| Default Themes Refresh | 預設主題更新 |
| VS Code Light | VS Code Light（亮色主題） |
| VS Code Dark | VS Code Dark（暗色主題） |
| OS Theme Syncing | 作業系統主題同步 |
| Screen Reader Labels | 螢幕閱讀器標籤 |
| Implicit Content | 隱含內容 |
| Reveal in File Explorer | 在檔案總管中顯示 |
| WSL | Windows Subsystem for Linux |
| Command Palette | 命令面板 |
| Syntax Highlighting | 語法醒目提示 |
| Validation | 驗證 |
| Marketplace | 市集 |
| Deprecated | 已棄用 |

---

*資料來源：VS Code 1.113 發行說明 (https://code.visualstudio.com/updates/v1_113)*
