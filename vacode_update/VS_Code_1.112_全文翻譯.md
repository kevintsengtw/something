# Visual Studio Code 1.112 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.112
**發行日期：** 2026 年 3 月 18 日
**原文：** https://code.visualstudio.com/updates/v1_112

---

歡迎使用 Visual Studio Code 1.112 版本。本次發行包含跨 **Agent 與開發者體驗**的多項改善。

- **[整合式瀏覽器除錯](#使用整合式瀏覽器除錯-web-應用程式)**：在不離開 VS Code 的情況下端對端除錯 Web 應用程式。
- **[Copilot CLI 權限](#copilot-cli-的權限等級)**：賦予 Copilot CLI 工作階段更多自主權，讓它們以更少的中斷完成任務。
- **[MCP 伺服器沙箱化](#沙箱化本地執行的-mcp-伺服器)**：在沙箱中執行本地 MCP 伺服器，限制它們在您機器上的存取範圍。
- **[Agent 圖片支援](#agent-的圖片與二進位檔案支援)**：直接在 Agent 對話中使用螢幕截圖、圖表和二進位檔案。
- **[Monorepo 自訂項目](#父儲存庫中的自訂項目探索)**：在 Monorepo 的所有套件間共享 Agent 指令和技能。

Happy Coding!

---

VS Code 正在逐步向所有使用者推出。在 VS Code 中使用 **Check for Updates** 可立即取得最新版本。

若要盡快試用新功能，請[**下載每夜建置的 Insiders 版本**](https://code.visualstudio.com/insiders)，其中包含最新的更新。

---

## Agent 體驗

賦予 Agent 更多自主權、更豐富的上下文和更簡便的診斷工具，讓它們以更少的人工介入處理複雜任務。

### Copilot CLI 的訊息引導與排隊

對於本機 Agent 工作階段，您可以在前一個請求執行期間發送訊息，以引導 Agent 朝向不同的回應，或排隊後續訊息。本次發行為 Copilot CLI 工作階段新增了訊息引導與排隊的支援。

更多資訊請參閱文件中的[訊息引導與排隊](https://code.visualstudio.com/docs/copilot/chat/chat-sessions#_send-messages-while-a-request-is-running)。

### 委派至 Copilot CLI 前預覽變更

當您的工作區有未提交的變更，而您嘗試將任務委派給 Copilot CLI 時，您可以選擇複製、移動或忽略 Copilot CLI 為工作階段建立的 Worktree 中的那些變更。然而，您之前必須檢查原始碼控制檢視才能看到那些變更是什麼。

在本次發行中，Chat 檢視現在會顯示待處理的變更清單，讓您更容易看到哪些變更可以遷移至委派給 Copilot CLI 時建立的 Worktree。

### Copilot CLI 終端機輸出中的可點擊檔案連結

**設定**：`github.copilot.chat.cli.terminalLinks.enabled`

終端機的檔案連結偵測現在可識別 Copilot CLI 產生的、參照 `~/.copilot/session-state/` 目錄下檔案的路徑。先前，這些路徑無法正確解析，因為內建的連結偵測器不知道 Copilot CLI 的 session-state 目錄結構。

連結偵測器現在可處理絕對路徑和相對路徑：絕對路徑和波浪號前綴的路徑直接開啟，而相對路徑則根據活動的 session-state 目錄解析，並以工作區資料夾作為後備。

此功能預設為啟用，可透過 `github.copilot.chat.cli.terminalLinks.enabled` 設定切換。

### Copilot CLI 的權限等級

**設定**：`chat.autopilot.enabled`

您可以在聊天中為本機 Agent 工作階段配置權限，賦予 Agent 更多行動自主權並減少核准請求的數量。本次發行也為 Copilot CLI 工作階段新增了此能力。

對於 Copilot CLI 工作階段，您可以在以下權限等級中選擇：

- `Default Permissions`：使用您已設定的核准設定。需要核准的工具會在執行前顯示確認對話方塊。
- `Bypass Approvals`：自動核准所有工具呼叫，不顯示確認對話方塊，並在錯誤時自動重試。
- `Autopilot`：（在 Insiders 中預設啟用）自動核准所有工具呼叫、自動回應問題，並持續自主工作直到任務完成。透過 `chat.autopilot.enabled` 設定啟用 Autopilot。

更多資訊請參閱文件中的 [Autopilot 和 Agent 權限](https://code.visualstudio.com/docs/copilot/agents/agent-tools#_permission-levels)。

### 使用 /troubleshoot 疑難排解 Agent 行為（Preview）

**設定**：`github.copilot.chat.agentDebugLog.enabled`、`github.copilot.chat.agentDebugLog.fileLogging.enabled`

VS Code 中有多種 Agent 自訂選項可用。如果您的聊天 Agent 行為不如預期，可能很難理解原因。例如，當指令、技能或 Agent 未正確套用時，或回應出乎意料地緩慢時。

為此，我們推出了一個新的 `/troubleshoot` 技能，可直接在對話中分析 Agent 偵錯日誌，並提供有關 Agent 行為的洞察。在聊天輸入中輸入 `/troubleshoot`，後面接著您正在遇到的問題描述。

此技能讀取從聊天工作階段匯出的 JSONL 偵錯日誌檔案，可幫助您了解工具或子代理被使用或跳過的原因、指令或技能未載入的原因、導致回應時間緩慢的因素，以及是否發生了網路連線問題。

若要在聊天中使用 `/troubleshoot` 技能，請啟用以下設定並重新載入 VS Code：

- `github.copilot.chat.agentDebugLog.enabled`：啟用 Agent 偵錯日誌
- `github.copilot.chat.agentDebugLog.fileLogging.enabled`：將偵錯日誌寫入磁碟上的 JSONL 檔案

更多資訊請參閱 VS Code 中的[疑難排解 Agent 行為](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view)。

### 匯出和匯入 Agent 偵錯日誌（Preview）

**設定**：`github.copilot.chat.agentDebugLog.enabled`

VS Code 中的 Agent Debug Logs 面板為您提供工作階段中 Agent 行為的詳細檢視，包括工具使用、子代理決策等。先前，面板中僅有活動工作階段的偵錯資訊。

您現在可以匯出和匯入 Agent 工作階段的偵錯日誌，讓您可以與他人分享或離線分析。這對於疑難排解和分享有關 Agent 行為的洞察特別有用。

更多資訊請參閱 [Agent Debug Logs](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view) 文件。

> **注意**：匯入大於 50 MB 的檔案會顯示包含實際檔案大小的警告對話方塊。如果遇到此警告，請考慮修剪檔案或匯出較短的工作階段。

### Agent 的圖片與二進位檔案支援

**設定**：`chat.imageCarousel.enabled`、`imageCarousel.explorerContextMenu.enabled`

Agent 現在可以從磁碟原生讀取圖片檔案和二進位檔案，讓您可以將 Agent 用於更廣泛的任務，例如分析螢幕截圖、讀取二進位檔案中的資料等。二進位檔案以 hexdump 格式呈現給 Agent。

當 Agent 或工具產生圖片輸出時，例如來自整合式瀏覽器的螢幕截圖，這些圖片現在可在聊天回應中選取，並可在專用的圖片輪播檢視中開啟。透過 `chat.imageCarousel.enabled` 設定（實驗性）啟用此功能。

當啟用 `imageCarousel.explorerContextMenu.enabled`（實驗性）時，您可以在檔案總管檢視中對圖片檔案或資料夾按右鍵，選取 **Open Images in Carousel** 以在輪播檢視中瀏覽圖片。

> **注意**：圖片輪播目前為實驗性功能。

### 自動符號參考

當您複製一個符號，例如類別名稱、函式或方法名稱，並將其貼入聊天時，VS Code 現在會自動將其貼為符號參考 `#sym:Name`。這讓 Agent 自動取得有關該符號的上下文，使其能更快速、更有效率地完成任務。

如果您想在不轉換為符號參考的情況下貼上符號，可以使用 **Paste as Text** 命令，透過 Ctrl+Shift+V（macOS 上為 Cmd+Shift+V）。

---

## Agent 擴充性

透過共享的自訂項目跨專案擴展您的 Agent 設定，並透過對 MCP 伺服器和外掛程式的更嚴格控制保持安全。

### 父儲存庫中的自訂項目探索

**設定**：`chat.useCustomizationsInParentRepositories`

在 Monorepo 設定中，您通常會在 VS Code 中開啟一個套件或子資料夾，而非儲存庫根目錄。先前，聊天自訂項目僅從目前工作區資料夾中探索。透過新的 `chat.useCustomizationsInParentRepositories` 設定，VS Code 也可以從父資料夾向上探索至儲存庫根目錄的自訂檔案。

這種改善的探索機制讓您更容易在 Monorepo 中跨套件共享儲存庫範圍的指引和工具，而無需將完整儲存庫作為工作區開啟。

當探索功能啟用時，它適用於所有聊天自訂類型，包括始終啟用的指令（如 `copilot-instructions.md`、`AGENTS.md` 和 `CLAUDE.md`），以及指令檔案、提示檔案、自訂 Agent、技能和掛鉤。

父儲存庫探索僅在以下情況適用：

- 您開啟的工作區資料夾本身不是 Git 儲存庫
- 父資料夾包含 `.git` 資料夾
- 父儲存庫已透過[工作區信任](https://code.visualstudio.com/docs/editing/workspaces/workspace-trust)被信任

更多資訊請參閱文件中的 [Agent 自訂項目](https://code.visualstudio.com/docs/copilot/customization/overview)。

### 沙箱化本地執行的 MCP 伺服器

在本地執行 MCP 伺服器可能會帶來安全風險，因為它們具有與執行 VS Code 的使用者相同的權限，這使它們能夠存取其功能不需要的檔案或網路資源。

為降低此風險，您現在可以在 macOS 和 Linux 上以沙箱環境執行本地設定的 stdio MCP 伺服器。沙箱伺服器具備受限的檔案系統和網路存取。

若要啟用沙箱化，請在您的 `mcp.json` 檔案中為伺服器設定 `"sandboxEnabled": true`。當沙箱伺服器需要存取額外的資料夾或網域時，VS Code 會提示您授予該權限，並更新該 `mcp.json` 檔案的沙箱組態。在同一個 `mcp.json` 檔案中定義的所有伺服器共享該沙箱組態。

> **注意**：本地執行的 MCP 伺服器沙箱化目前不支援 Windows。遠端情境（如 WSL 和 SSH）仍可正常運作。

### 改善的 MCP 引出表單 UI

當 MCP 伺服器需要額外資訊來完成請求時，它可以觸發引出表單（elicitation form）來向使用者收集該資訊。這些引出表單現在使用與 Ask Questions 工具相同的 UI，在向 MCP 伺服器提供額外資訊時提供更一致且使用者友善的體驗。

### 啟用或停用外掛程式與 MCP 伺服器

先前，外掛程式和 MCP 伺服器只能透過安裝或解除安裝來停用或啟用。本次發行新增了在不解除安裝的情況下啟用或停用外掛程式和 MCP 伺服器的能力。

外掛程式和 MCP 伺服器現在可以全域和按工作區啟用和停用。您可以透過開啟 MCP 或外掛程式頁面，或在擴充功能檢視或 **Chat: Open Customizations** 檢視中對其項目按右鍵來執行此操作。

### 外掛程式自動更新

**設定**：`extensions.autoUpdate`

外掛程式現在可以根據 `extensions.autoUpdate` 設定自動更新。來自 `npm` 和 `pypi` 的外掛程式需要核准才能更新，因為更新這些外掛程式可能會導致新的程式碼在您的機器上執行。

---

## 開發者體驗

在不離開 VS Code 的情況下建置和除錯 Web 應用程式，使用更強大的整合式瀏覽器和精簡的編輯器工作流程。

### 整合式瀏覽器

#### 使用整合式瀏覽器除錯 Web 應用程式

整合式瀏覽器讓您可以直接在 VS Code 內開啟 Web 應用程式，現在您也可以使用整合式瀏覽器啟動除錯工作階段。這讓您可以與 Web 應用程式互動、設定中斷點、逐步執行程式碼和檢視變數，全程無需離開 VS Code。

我們新增了一個 `editor-browser` 除錯類型，可透過 Launch 和 Attach 組態對整合式瀏覽器分頁進行除錯。

既有 `msedge` 和 `chrome` 除錯組態中的大多數選項都受支援，這讓遷移通常只需修改 `launch.json` 中既有組態的 type 即可。

更多資訊請參閱文件中的整合式瀏覽器及除錯設定：[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)。

#### 整合式瀏覽器 UX 改善

- **右鍵選單**

  在瀏覽器頁面中按右鍵現在會顯示常用選項，例如複製/貼上、在新分頁中開啟和檢查元素。

- **獨立縮放層級**

  整合式瀏覽器現在擁有自己的縮放層級，獨立於 VS Code 視窗的縮放。當瀏覽器獲得焦點時，使用 **Zoom In**（⌘=（Windows、Linux 為 Ctrl+=））、**Zoom Out**（⌘-（Windows、Linux 為 Ctrl+-））和 **Reset Zoom**（⌘Numpad0（Windows、Linux 為 Ctrl+Numpad0））快捷鍵，或使用 URL 列選單中的動作。縮放層級會按網站記憶，就像在一般瀏覽器中一樣。

  使用 `workbench.browser.pageZoom` 設定來配置預設縮放層級。當設為「Match Window」或未設定時，瀏覽器會匹配 VS Code 視窗的縮放。

### 搜尋後自動關閉尋找對話方塊

新的 `editor.find.closeOnResult` 設定讓您在找到匹配項後自動關閉尋找控制項，並將焦點移回編輯器。

此設定預設為停用，保留尋找對話方塊在搜尋後保持開啟的既有行為。

---

## 終端機

### 改善的終端機 IME 合成

使用輸入法編輯器（IME）在接近終端機右緣處輸入時，合成預覽文字先前可能會溢出終端機邊界外。合成檢視現在被限制在游標與終端機右緣之間的可用空間內。當您輸入新字元時，較舊的字元會逐漸隱藏，讓預覽文字保持在終端機視窗範圍內。這與 Ghostty 等其他現代終端機的行為一致。

> **注意**：在 Windows 上，請啟用 `terminal.integrated.windowsUseConptyDll` 以獲得最佳的 IME 合成體驗。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 重要修正

- [xtermjs/xterm.js #5737](https://github.com/xtermjs/xterm.js/pull/5737) — 修正在較新版 fish + kitty 鍵盤協定中 ^C 無法結束的問題
- [microsoft/vscode-python #25849](https://github.com/microsoft/vscode-python/pull/25849) — 防止兩個擴充功能造成的雙重/三重啟動

---

## 感謝

### Issue 追蹤

Issue 追蹤貢獻者：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@12LuA (Luca)](https://github.com/12LuA)：修正：authIssuers 提案中的註解拼字錯誤 [PR #300899](https://github.com/microsoft/vscode/pull/300899)
- [@DrSergei (Sergei Druzhkov)](https://github.com/DrSergei)：修正設定回應後的變數更新 [PR #299473](https://github.com/microsoft/vscode/pull/299473)
- [@eliericha (Elie Richa)](https://github.com/eliericha)：讓基於環境的變數解析器包含啟動組態的環境 [PR #299752](https://github.com/microsoft/vscode/pull/299752)
- [@jcansdale (Jamie Cansdale)](https://github.com/jcansdale)：修正：在 macOS 上分塊多行 PTY 寫入以避免 1024 位元組緩衝區損壞 [PR #298993](https://github.com/microsoft/vscode/pull/298993)
- [@jeanp413 (Jean Pierre)](https://github.com/jeanp413)：在有遠端授權時支援 Web Worker 擴充功能主機上的終端機建立 [PR #300897](https://github.com/microsoft/vscode/pull/300897)
- [@JeffreyCA](https://github.com/JeffreyCA)：更新 Azure Developer CLI (azd) 的 Fig 規格 [PR #299892](https://github.com/microsoft/vscode/pull/299892)
- [@lammmab (Liam)](https://github.com/lammmab)：當 AI 功能停用時隱藏「Ask for Edits」介面元素 [PR #300563](https://github.com/microsoft/vscode/pull/300563)
- [@murataslan1 (Murat Aslan)](https://github.com/murataslan1)：修正：在參數提示小工具中換行過長的文件字串 [PR #292258](https://github.com/microsoft/vscode/pull/292258)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)：修正：MainThreadWorkspace 中的記憶體洩漏 [PR #283450](https://github.com/microsoft/vscode/pull/283450)
- [@tamuratak (Takashi Tamura)](https://github.com/tamuratak)：markdown-language-features：透過改善的 URI 解析和選取增強文件連結處理 [PR #296821](https://github.com/microsoft/vscode/pull/296821)
- [@xingsy97 (xingsy97)](https://github.com/xingsy97)：為 AI Agent 工作流程豐富終端機工具結果中繼資料 [PR #300034](https://github.com/microsoft/vscode/pull/300034)

`vscode-copilot-chat` 程式碼貢獻者：

- [@24anisha (Anisha Agarwal)](https://github.com/24anisha)：為搜尋子代理遙測新增 conversation_id [PR #4326](https://github.com/microsoft/vscode-copilot-chat/pull/4326)
- [@aashna (Aashna Garg)](https://github.com/aashna)：為路由決策 API 新增 sticky_threshold 和 sticky_override [PR #4359](https://github.com/microsoft/vscode-copilot-chat/pull/4359)
- [@dennyac (Denny Abraham Cheriyan)](https://github.com/dennyac)：為事件新增已解析的模型 [PR #4210](https://github.com/microsoft/vscode-copilot-chat/pull/4210)
- [@IanMatthewHuff (Ian Huff)](https://github.com/IanMatthewHuff)：更多儲存庫資訊遙測檢查以支援 Windows 儲存庫效能問題 [PR #4339](https://github.com/microsoft/vscode-copilot-chat/pull/4339)

`vscode-docs` 程式碼貢獻者：

- [@karlhorky (Karl Horky)](https://github.com/karlhorky)：重新措詞「Secondary Side Bar」文件以反映預設可見狀態 [PR #9540](https://github.com/microsoft/vscode-docs/pull/9540)
- [@mariaghiondea (Maria Ghiondea)](https://github.com/mariaghiondea)：更新發佈擴充功能文件以反映軟刪除變更 [PR #9544](https://github.com/microsoft/vscode-docs/pull/9544)
- [@putku45](https://github.com/putku45)
  - 修正重構文件中的文法 [PR #9525](https://github.com/microsoft/vscode-docs/pull/9525)
  - 修正 2017 年 10 月發行說明中的拼字錯誤 [PR #9524](https://github.com/microsoft/vscode-docs/pull/9524)
  - 修正 JSON completions 區段中的文法 [PR #9526](https://github.com/microsoft/vscode-docs/pull/9526)

`node-pty` 程式碼貢獻者：

- [@ritschwumm](https://github.com/ritschwumm)：修正文件註解中的拼字錯誤 [PR #897](https://github.com/microsoft/node-pty/pull/897)

`python-environment-tools` 程式碼貢獻者：

- [@lingyaochu (Xin Zhao)](https://github.com/lingyaochu)：僅為二進位目標嵌入 Windows 資源 [PR #374](https://github.com/microsoft/python-environment-tools/pull/374)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Agent Experience | Agent 體驗 |
| Message Steering and Queueing | 訊息引導與排隊 |
| Delegate to Copilot CLI | 委派至 Copilot CLI |
| Pending Changes | 待處理的變更 |
| Worktree | Worktree（Git 工作樹） |
| File Link Detection | 檔案連結偵測 |
| Session-state Directory | Session-state 目錄 |
| Permissions Levels | 權限等級 |
| Default Permissions | 預設權限 |
| Bypass Approvals | 略過核准 |
| Autopilot | Autopilot（自動駕駛） |
| Approval Requests | 核准請求 |
| /troubleshoot | /troubleshoot 技能 |
| Agent Debug Logs | Agent 偵錯日誌 |
| JSONL Debug Log Files | JSONL 偵錯日誌檔案 |
| Export and Import | 匯出和匯入 |
| Image and Binary File Support | 圖片與二進位檔案支援 |
| Hexdump | Hexdump 格式 |
| Image Carousel | 圖片輪播 |
| Automatic Symbol References | 自動符號參考 |
| Symbol Reference | 符號參考 |
| Paste as Text | 以純文字貼上 |
| Agent Extensibility | Agent 擴充性 |
| Customizations Discovery | 自訂項目探索 |
| Parent Repositories | 父儲存庫 |
| Monorepo | Monorepo |
| Always-on Instructions | 始終啟用的指令 |
| Workspace Trust | 工作區信任 |
| Sandbox | 沙箱 |
| MCP Server Sandboxing | MCP 伺服器沙箱化 |
| stdio MCP Servers | stdio MCP 伺服器 |
| Sandbox Configuration | 沙箱組態 |
| MCP Elicitation | MCP 引出表單 |
| Enable or Disable | 啟用或停用 |
| Plugins | 外掛程式 |
| Automatic Plugin Updates | 外掛程式自動更新 |
| Integrated Browser | 整合式瀏覽器 |
| Debug Type | 除錯類型 |
| Launch and Attach | Launch 和 Attach 組態 |
| Context Menu | 右鍵選單 |
| Independent Zoom Level | 獨立縮放層級 |
| Auto-close Find Dialog | 搜尋後自動關閉尋找對話方塊 |
| IME Composition | IME 合成 |
| Input Method Editor | 輸入法編輯器 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |
| Notable Fixes | 重要修正 |

---

*資料來源：[Visual Studio Code 1.112 發行說明](https://code.visualstudio.com/updates/v1_112)*
