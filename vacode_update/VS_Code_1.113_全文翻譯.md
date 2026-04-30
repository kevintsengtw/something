# Visual Studio Code 1.113 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.113
**發行日期：** 2026 年 3 月 25 日
**原文：** https://code.visualstudio.com/updates/v1_113

---

歡迎使用 Visual Studio Code 1.113 版本。本次發行包含跨 **Agent 與開發者體驗**的多項改善。

- **[Chat Customizations](#chat-customizations-編輯器preview)**：從單一的統一介面管理所有聊天相關自訂項目。
- **[可配置的思考力度](#模型選擇器中可配置的思考力度)**：直接從 UI 控制模型的推理等級。
- **[巢狀子代理](#巢狀子代理)**：允許子代理呼叫其他子代理，實現複雜的多步驟工作流程。
- **[CLI Agent 能力](#agent-體驗)**：在 CLI Agent 中使用 MCP 伺服器、分叉工作階段，以及檢視偵錯日誌。
- **[圖片預覽](#聊天附件的圖片預覽)**：使用全功能的圖片檢視器預覽聊天中的圖片。
- **[預設主題更新](#全新預設主題)**：享受更新後的預設淺色和深色主題帶來的全新外觀。

Happy Coding!

---

VS Code 正在逐步向所有使用者推出。在 VS Code 中使用 **Check for Updates** 可立即取得最新版本。

若要盡快試用新功能，請[**下載每夜建置的 Insiders 版本**](https://code.visualstudio.com/insiders)，其中包含最新的更新。

---

## Agent 體驗

跨本機、CLI 和 Claude Agent 使用相同的工具和工作流程，以更少的摩擦組合多步驟自動化。

### Copilot CLI 和 Claude Agent 的 MCP 支援

先前，您在 VS Code 中設定的 MCP 伺服器僅供在編輯器中執行的本機 Agent 使用。本次發行新增了 Copilot CLI 和 Claude Agent 對 MCP 伺服器的支援。

您在 VS Code 中註冊的 MCP 伺服器會橋接至 Copilot CLI 和 Claude Agent。這適用於使用者定義的伺服器，以及透過工作區中的 `mcp.json` 檔案定義的伺服器。

更多資訊請參閱[在 VS Code 中使用 MCP 伺服器](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)。

### Copilot CLI 和 Claude Agent 的分叉工作階段

**設定**：`github.copilot.chat.cli.forkSessions.enabled`

分叉工作階段讓您可以在對話歷史的任何時間點建立現有工作階段的副本。這在您想探索不同的思路或嘗試不同的提示，而不丟失原始工作階段上下文時非常有用。

在本次發行中，您現在也可以在 Copilot CLI（實驗性）和 Claude Agent 中分叉工作階段。若要為 Copilot CLI 啟用分叉，請啟用 `github.copilot.chat.cli.forkSessions.enabled` 設定。

更多資訊請參閱文件中的[分叉聊天工作階段](https://code.visualstudio.com/docs/copilot/chat/chat-sessions#_fork-a-chat-session)。

### Copilot CLI 和 Claude CLI 工作階段的 Agent 偵錯日誌（Preview）

Agent Debug Log 面板是了解您送出提示後發生什麼事的主要工具。它顯示聊天工作階段期間 Agent 互動的時序事件紀錄。您現在可以將 Agent Debug Log 面板用於 Copilot CLI 和 Claude Agent 工作階段。對本機 Agent 工作階段的支援已在先前版本中提供。

更多資訊請參閱文件中的 [Agent Debug Log 面板](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view#_agent-debug-log-panel)。

### Claude 工作階段列表改用 SDK API

VS Code 現在採用 Claude Agent SDK 的官方 API 來列出工作階段及其訊息。先前，我們依賴解析磁碟上的 Claude JSONL 檔案，若 Claude 變更其結構，可能會有不同步的風險。如果您之前遇到 Claude Agent 未顯示所有工作階段或訊息的問題，現在應該已經解決。

### 巢狀子代理

**設定**：`chat.subagents.allowInvocationsFromSubagents`

子代理現在可以呼叫其他子代理，支援更複雜的多步驟工作流程。先前，子代理被限制不能呼叫其他子代理，以防止無限遞迴。透過新的 `chat.subagents.allowInvocationsFromSubagents` 設定，您可以在需要時啟用此能力。

更多資訊請參閱文件中的[使用子代理](https://code.visualstudio.com/docs/copilot/agents/subagents)。

### 管理外掛程式市集

我們新增了一個命令 **Chat: Manage Plugin Marketplaces**，可列出所有已設定的外掛程式市集。對於每個市集，您可以瀏覽外掛程式、開啟其本機目錄，以及移除它們。

更多資訊請參閱文件中的[使用 Agent 外掛程式](https://code.visualstudio.com/docs/copilot/customization/agent-plugins)。

### 外掛程式安裝的 URL 處理程式

您可以透過 URL 處理程式觸發 VS Code 外掛程式安裝。若要安裝市集，可以觸發以下格式的連結：

```
vscode://chat-plugin/add-marketplace?ref=<source>
```

其中「source」是 GitHub 的 `repo/owner` 或 Base64 編碼的 Git URI。若要安裝擴充功能，可以使用以下格式：

```
vscode://chat-plugin/install?source=<source>
```

若要針對 VS Code Insiders，請將 URL 中的 `vscode` 替換為 `vscode-insiders`。

---

## 聊天體驗

根據您的專案從單一編輯器調整 AI，控制模型在回應前的推理程度，以及在不離開聊天的情況下審查視覺上下文。

### Chat Customizations 編輯器（Preview）

Chat Customizations 編輯器提供一個集中式 UI，可在同一個地方建立和管理您所有的聊天自訂項目。編輯器將自訂類型組織到不同的分頁中，例如自訂指令、提示檔案、自訂 Agent 和 Agent 技能。它也提供內嵌的程式碼編輯器，附帶語法醒目顯示和驗證。

您可以從頭建立新的自訂項目，或使用 AI 根據您的專案生成初始內容。若要新增 MCP 伺服器和 Agent 外掛程式，可直接從編輯器瀏覽對應的市集。

若要開啟編輯器，請在 Chat 檢視中選取 **Configure Chat（齒輪圖示）**，或從命令面板（⇧⌘P（Windows、Linux 為 Ctrl+Shift+P））執行 **Chat: Open Chat Customizations**。

更多資訊請參閱文件中的 [Chat Customizations 編輯器](https://code.visualstudio.com/docs/copilot/customization/overview#_chat-customizations-editor)。

### 模型選擇器中可配置的思考力度

支援推理的模型，例如 Claude Sonnet 4.6 和 GPT-5.4，現在在模型選擇器中直接顯示 **Thinking Effort** 子選單。您可以用它來控制模型對每次請求投入多少推理，而無需前往 VS Code 設定。VS Code 會為每個模型跨對話保留所選的力度等級。

在選擇器中選取推理模型，然後選取箭頭以顯示可用的力度等級。可用的力度等級可能因模型而異。非推理模型不會顯示子選單。

模型選擇器標籤現在也會顯示所選的力度等級，例如「GPT-5.3-Codex · Medium」，讓您更容易看到每個模型目前啟用的力度等級。

更多資訊請參閱文件中的[思考力度與推理](https://code.visualstudio.com/docs/copilot/concepts/language-models#_thinking-and-reasoning)。

> **注意：** `github.copilot.chat.anthropic.thinking.effort` 和 `github.copilot.chat.responsesApiReasoningEffort` 設定已被棄用。推理力度現在直接透過模型選擇器配置。

### 聊天附件的圖片預覽

**設定**：`imageCarousel.chat.enabled`、`imageCarousel.explorerContextMenu.enabled`

當您在聊天中使用圖片時，無論是附加螢幕截圖到請求中，還是 Agent 透過工具呼叫產生圖片，您現在都可以選取任何圖片附件，在全功能的圖片檢視器中開啟它。

檢視器以模態覆蓋層的方式開啟，支援：

- **導航**：使用箭頭按鈕、鍵盤方向鍵或底部的縮圖列瀏覽目前聊天工作階段的所有圖片。
- **分段**：圖片依對話輪次分組，讓您可以看到哪些圖片來自特定的請求或回應。
- **縮放與平移**：按一下可放大，使用 Option+按一下（Mac）或 Ctrl+按一下（Windows/Linux）可縮小，或捲動/捏合可連續縮放。在高縮放倍率下，捲動可在圖片中平移。

圖片檢視器現在也可從檔案總管檢視的右鍵選單中用於圖片檔案。當您選取 **Open in Images Preview** 時，檢視器會開啟並顯示目前資料夾中的所有圖片。

這兩項功能預設為啟用。若要獨立配置它們，請使用 `imageCarousel.chat.enabled` 和 `imageCarousel.explorerContextMenu.enabled`。

---

## 編輯器體驗

在整合式瀏覽器中更有信心地開發和測試 Web 應用程式，並享受編輯器煥然一新的預設外觀。

### 在整合式瀏覽器中使用自簽憑證

當您開發依賴安全 HTTPS 連線的 Web 應用程式時，在測試期間通常需要使用自簽憑證。

在正常情況下，此類憑證不應被信任。先前，任何出示不受信任憑證的網站在整合式瀏覽器中都會直接載入失敗，沒有任何略過的選項。

現在，類似於大多數瀏覽器，您可以選擇暫時信任無法驗證的憑證，以解除這些場景中的開發阻礙。

當您選擇繼續時，使用該憑證對目前主機的連線將在一週內被允許。URL 列會顯示連線不安全，並提供隨時撤銷信任的選項。

更多資訊請參閱文件中的[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)。

### 改善的瀏覽器分頁管理

**設定**：`workbench.browser.showInTitleBar`

管理開啟的分頁本來就可能很困難。隨著我們鼓勵更多使用整合式瀏覽器分頁，我們也新增了更多控制項來輕鬆管理它們。

- **Quick Open Browser Tab**

  此命令會開啟一個 Quick Pick，顯示所有開啟的瀏覽器分頁，並允許快速篩選、聚焦和關閉它們。

  此命令也可以在瀏覽器目前獲得焦點時使用 ⇧⌘A（Windows、Linux 為 Ctrl+Shift+A）鍵盤快捷鍵觸發，或透過 VS Code 標題列中的新快捷按鈕觸發（在瀏覽器分頁開啟時可見）。

  此按鈕的可見性可透過 `workbench.browser.showInTitleBar` 設定配置。

- **Close All Browser Tabs**

  瀏覽器分頁右鍵選單現在有一個選項可關閉同一群組中的所有瀏覽器分頁，類似於既有的「Close All」項目。也可透過命令面板關閉所有群組中的瀏覽器分頁。

### 全新預設主題

VS Code 現在附帶全新的預設主題：「VS Code Light」和「VS Code Dark」。這些主題旨在提供清新、現代的外觀，同時維持先前預設「Modern」主題的熟悉度和可用性。此外，作業系統主題同步對新使用者將預設使用新主題，讓 VS Code 自動以新主題匹配作業系統的淺色/深色模式。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 感謝

### Issue 追蹤

Issue 追蹤貢獻者：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@jcansdale (Jamie Cansdale)](https://github.com/jcansdale)：對多行執行的終端機文字使用括號貼上 [PR #302526](https://github.com/microsoft/vscode/pull/302526)
- [@jeevaratnamputla](https://github.com/jeevaratnamputla)：將 child_process.exec 替換為 execFile 以防止潛在的命令注入 [PR #291825](https://github.com/microsoft/vscode/pull/291825)
- [@kbhujbal (Kunal Bhujbal)](https://github.com/kbhujbal)：修正程式碼品質問題：錯誤記錄和 JSDoc 拼字錯誤 [PR #297893](https://github.com/microsoft/vscode/pull/297893)
- [@sathvikc (Sathvik C)](https://github.com/sathvikc)：修正：防止可重入的 renderGettingStartedTipIfNeeded 產生重複的提示節點 [PR #302317](https://github.com/microsoft/vscode/pull/302317)
- [@ShehabSherif0 (Shehab Sherif)](https://github.com/ShehabSherif0)：修正 sanitizeId 正則表達式中缺少的全域旗標 [PR #303603](https://github.com/microsoft/vscode/pull/303603)
- [@xingsy97 (xingsy97)](https://github.com/xingsy97)：Git — 最佳化 worktree 忽略路徑的計算 [PR #303955](https://github.com/microsoft/vscode/pull/303955)

`vscode-copilot-chat` 程式碼貢獻者：

- [@24anisha (Anisha Agarwal)](https://github.com/24anisha)
  - 搜尋子代理 — 解析相對與絕對路徑 [PR #4429](https://github.com/microsoft/vscode-copilot-chat/pull/4429)
  - 系統提示更新以處理搜尋子代理 [PR #4500](https://github.com/microsoft/vscode-copilot-chat/pull/4500)
- [@etvorun (ET)](https://github.com/etvorun)：修正：NES debounce 和語言上下文擷取未尊重取消 Token [PR #4384](https://github.com/microsoft/vscode-copilot-chat/pull/4384)

`vscode-python-environments` 程式碼貢獻者：

- [@00zayn](https://github.com/00zayn)：修正 ${workspaceFolder} 範圍的全域 defaultInterpreterPath 產生的虛假未解析直譯器警告 [PR #1334](https://github.com/microsoft/vscode-python-environments/pull/1334)
- [@StellaHuang95 (Stella Huang)](https://github.com/StellaHuang95)：為管理器註冊失敗新增遙測 [PR #1365](https://github.com/microsoft/vscode-python-environments/pull/1365)

`vscode-windows-process-tree` 程式碼貢獻者：

- [@ZA139](https://github.com/ZA139)：功能：新增 getAllProcesses API 以擷取所有系統程序 [PR #84](https://github.com/microsoft/vscode-windows-process-tree/pull/84)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Agent Experience | Agent 體驗 |
| MCP Servers | MCP 伺服器 |
| Copilot CLI | Copilot CLI |
| Claude Agent | Claude Agent |
| Bridged | 橋接 |
| mcp.json | mcp.json 檔案 |
| Forking Sessions | 分叉工作階段 |
| Agent Debug Log | Agent 偵錯日誌 |
| Chronological Event Log | 時序事件紀錄 |
| Claude Agent SDK | Claude Agent SDK |
| JSONL Files | JSONL 檔案 |
| Nested Subagents | 巢狀子代理 |
| Infinite Recursion | 無限遞迴 |
| Plugin Marketplaces | 外掛程式市集 |
| URL Handlers | URL 處理程式 |
| Chat Customizations Editor | Chat Customizations 編輯器 |
| Custom Instructions | 自訂指令 |
| Prompt Files | 提示檔案 |
| Custom Agents | 自訂 Agent |
| Agent Skills | Agent 技能 |
| Syntax Highlighting | 語法醒目顯示 |
| Validation | 驗證 |
| Command Palette | 命令面板 |
| Thinking Effort | 思考力度 |
| Model Picker | 模型選擇器 |
| Reasoning | 推理 |
| Effort Level | 力度等級 |
| Image Carousel | 影像輪播 |
| Modal Overlay | 模態覆蓋層 |
| Zoom & Pan | 縮放與平移 |
| Thumbnail Strip | 縮圖列 |
| Conversation Turn | 對話輪次 |
| Explorer Context Menu | 檔案總管右鍵選單 |
| Self-signed Certificate | 自簽憑證 |
| Integrated Browser | 整合式瀏覽器 |
| Untrusted Certificate | 不受信任的憑證 |
| Revoke Trust | 撤銷信任 |
| Quick Open Browser Tab | Quick Open Browser Tab 命令 |
| Quick Pick | Quick Pick |
| Close All Browser Tabs | 關閉所有瀏覽器分頁 |
| Title Bar | 標題列 |
| Default Themes | 預設主題 |
| VS Code Light / VS Code Dark | VS Code Light / VS Code Dark 主題 |
| OS Theme Syncing | 作業系統主題同步 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |
| Bracketed Paste | 括號貼上 |
| Command Injection | 命令注入 |
| Worktree | Worktree（Git 工作樹） |

---

*資料來源：[Visual Studio Code 1.113 發行說明](https://code.visualstudio.com/updates/v1_113)*
