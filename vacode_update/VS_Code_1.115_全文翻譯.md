# Visual Studio Code 1.115 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.115
**發行日期：** 2026 年 4 月 8 日
**原文：** https://code.visualstudio.com/updates/v1_115

---

歡迎使用 Visual Studio Code 1.115 版本。本次發行以全新的 VS Code Agents 伴隨應用程式的推出，讓您的 agent-native 開發體驗更上層樓！

- **[VS Code Agents 應用程式](#visual-studio-code-agentspreview)**：一個專為 agent-native 開發最佳化的全新伴隨應用程式，與 VS Code Insiders 並行執行。
- **[整合式瀏覽器](#整合式瀏覽器)**：多項改善讓 Agent 與整合式瀏覽器的協作更加無縫。
- **[終端機工具](#終端機工具改善)**：Agent 與背景終端機互動的新能力。

Happy Coding!

---

## Visual Studio Code Agents（Preview）

**Visual Studio Code Agents** 是一個全新的預覽版伴隨應用程式，與 VS Code Insiders 一同發行，專為 agent-native 開發打造。

- **跨專案平行任務** — 可在多個儲存庫中平行啟動 Agent 工作階段（各自隔離在獨立的 worktree 中），快速切換上下文（UI 會根據您的選取調整），並在人工與 Agent 審查之間反覆迭代。

- **監控與審查** — 追蹤工作階段進度、內嵌檢視 diff、對 Agent 留下回饋，並可直接在應用程式中建立 Pull Request，整個流程都無需離開該應用。

- **自訂項目同步** — Custom instructions、prompt files、custom agents、MCP servers、hooks 與 plugins 皆可在 Agents 應用程式中使用，以及您的其他 VS Code 自訂項目，例如主題。

- **無需額外安裝** — 此應用程式隨 VS Code Insiders 一同發行。從作業系統的開始功能表或應用程式資料夾啟動，或從命令面板執行 **Chat: Open Agents Application**。

Agents 應用程式是一個快速演進的預覽版。它目前僅在 VS Code Insiders 中可用，我們期待在 GitHub Issues 中收到您的回饋。

---

## 整合式瀏覽器

在本次發行中，我們持續強化整合式瀏覽器的體驗及其為 Agent 提供的能力。

### 瀏覽器 Agent 工具改善

**設定**：`workbench.browser.enableChatTools`

#### 更好的工具標籤

當 Agent 呼叫瀏覽器工具時，工具呼叫現在具有更具描述性的標籤，以及一個可直接跳至目標瀏覽器分頁的連結。

#### 長時間運行腳本支援

`Run Playwright Code` 工具改善了對長時間運行腳本的支援。執行超過五秒（預設值）的腳本現在會回傳延遲結果（deferred result），供 Agent 輪詢。

#### 減少重複分頁

Agent 現在被更強烈地抑制重複開啟瀏覽器分頁。當 Agent 嘗試開啟新分頁，而同一主機已有可用分頁開啟時，除非 Agent 明確傳入旗標，否則不會建立新分頁。

### 整合式瀏覽器中的捏合縮放（macOS）

[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)現在在 macOS 上支援捏合縮放。使用觸控板的捏合手勢可將網頁內容放大最多 3 倍。

與標準瀏覽器縮放（`⌘=`（Windows、Linux 為 `Ctrl+=`）/ `⌘-`（Windows、Linux 為 `Ctrl+-`））不同，捏合縮放是純視覺性放大，不會重新排版頁面版面。

---

## 終端機工具改善

本次發行改善了 Agent 在背景執行終端機命令的體驗。

### 向背景終端機送出輸入

先前，背景終端機是唯讀的，只有 `get_terminal_output` 可用。這在前景終端機逾時並移至背景時特別受限，因為 Agent 無法再與之互動。

有了新的 `send_to_terminal` 工具，Agent 可以繼續與背景終端機互動。例如，如果 SSH 工作階段在等待密碼提示時逾時，Agent 仍可送出所需的輸入以完成連線。

### 背景終端機通知（實驗性）

**設定**：`chat.tools.terminal.backgroundNotifications`

先前，當終端機命令在背景執行時，Agent 必須手動呼叫 `get_terminal_output` 來檢查其狀態。沒有方法可以知道命令何時完成或需要輸入。

有了新的實驗性 `chat.tools.terminal.backgroundNotifications` 設定，Agent 在背景終端機命令完成或需要使用者輸入時會自動收到通知。這也適用於前景終端機逾時並被移至背景的情況。Agent 隨後可以採取適當行動，例如檢視輸出或透過 `send_to_terminal` 工具提供輸入。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 重要修正

- [vscode#304257](https://github.com/microsoft/vscode/issues/304257) — 整合式 pwsh 的終端機重啟可能導致游標跑到錯誤位置
- [vscode#304679](https://github.com/microsoft/vscode/issues/304679) — Caps Lock 鍵在 VS Code 終端機中的 Claude Code 插入原始跳脫序列 `\[57358u`

---

## 感謝

Issue 追蹤貢獻者：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@andysharman](https://github.com/andysharman)：功能：新增預設新工作階段模式的 A/B 測試 [PR #306532](https://github.com/microsoft/vscode/pull/306532)
- [@chetanr-25](https://github.com/chetanr-25)：改善動態樣式表規則的型別安全 [PR #288651](https://github.com/microsoft/vscode/pull/288651)
- [@danplischke (Dan Plischke)](https://github.com/danplischke)：為 serve-web CLI 新增 default-folder、default-workspace 與 disable-telemetry [PR #299512](https://github.com/microsoft/vscode/pull/299512)
- [@mossgowild (moss)](https://github.com/mossgowild)：修正：防止 _extractImagesFromOutput 中的災難性正則回溯 [PR #307447](https://github.com/microsoft/vscode/pull/307447)
- [@xingsy97 (xingsy97)](https://github.com/xingsy97)：comments：修正在註解面板中回收樹狀項目時的記憶體洩漏 [PR #304666](https://github.com/microsoft/vscode/pull/304666)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - 修正：將視窗標題中的編輯器服務範圍限定至自身的編輯器群組容器 [PR #306226](https://github.com/microsoft/vscode/pull/306226)
  - 修正：重新開啟中斷點小工具時保留「Wait for Breakpoint」選取 [PR #306564](https://github.com/microsoft/vscode/pull/306564)
  - 修正：在尋找輸入的方向鍵導航中包含額外的切換項 [PR #306559](https://github.com/microsoft/vscode/pull/306559)
  - 功能：在 Minimap 中顯示涵蓋率指示器 [PR #307250](https://github.com/microsoft/vscode/pull/307250)
  - 修正：改善測試涵蓋率篩選快速選取的可讀性 [PR #306562](https://github.com/microsoft/vscode/pull/306562)
  - 修正：在測試總管中將無法識別的 @-前綴文字視為一般篩選 [PR #307555](https://github.com/microsoft/vscode/pull/307555)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Visual Studio Code Agents | Visual Studio Code Agents 應用程式 |
| Companion App | 伴隨應用程式 |
| Agent-native Development | Agent 原生開發 |
| Preview | 預覽版 |
| VS Code Insiders | VS Code Insiders |
| Parallelize Tasks | 平行任務 |
| Worktree | Worktree（Git 工作樹） |
| Monitor and Review | 監控與審查 |
| Inline Diffs | 內嵌 diff |
| Pull Request | Pull Request |
| Custom Instructions | 自訂指令（Custom Instructions） |
| Prompt Files | 提示檔案（Prompt Files） |
| Custom Agents | 自訂 Agent（Custom Agents） |
| MCP Servers | MCP 伺服器 |
| Hooks | Hooks |
| Plugins | 外掛程式（Plugins） |
| Command Palette | 命令面板 |
| Integrated Browser | 整合式瀏覽器 |
| Browser Agent Tools | 瀏覽器 Agent 工具 |
| Tool Labels | 工具標籤 |
| Tool Calls | 工具呼叫 |
| Run Playwright Code | Run Playwright Code 工具 |
| Deferred Result | 延遲結果 |
| Polling | 輪詢 |
| Duplicate Tabs | 重複分頁 |
| Explicit Flag | 明確旗標 |
| Pinch-to-zoom | 捏合縮放 |
| Visual Magnification | 視覺性放大 |
| Reflow | 重新排版 |
| Terminal Tools | 終端機工具 |
| Background Terminals | 背景終端機 |
| Foreground Terminals | 前景終端機 |
| send_to_terminal | send_to_terminal 工具 |
| get_terminal_output | get_terminal_output 工具 |
| Read-only | 唯讀 |
| Background Terminal Notifications | 背景終端機通知 |
| chat.tools.terminal.backgroundNotifications | chat.tools.terminal.backgroundNotifications 設定 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |
| Notable Fixes | 重要修正 |
| Integrated pwsh | 整合式 pwsh |
| Escape Sequence | 跳脫序列 |
| Minimap | Minimap（縮圖） |
| Coverage Indicators | 涵蓋率指示器 |
| Catastrophic Regex Backtracking | 災難性正則回溯 |

---

*資料來源：[Visual Studio Code 1.115 發行說明](https://code.visualstudio.com/updates/v1_115)*
