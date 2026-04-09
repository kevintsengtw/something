# Visual Studio Code 1.115 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.115
**發行日期：** 2026 年 4 月 8 日
**原文：** https://code.visualstudio.com/updates/v1_115

---

歡迎使用 Visual Studio Code 1.115 版本。本次發行包含多項以 Agent-native 開發體驗為核心的功能強化，並在終端機、整合式瀏覽器與 CLI 等多個領域帶來顯著改善。

以下是本次發行的主要亮點：

- **VS Code Agents 伴隨應用程式**：一個專為 agent-native 開發設計的全新預覽版應用程式，與 VS Code Insiders 一同發行
- **背景終端機 Agent 工具**：Agent 可在背景終端機命令完成或需要輸入時自動收到通知
- **終端機貼上檔案**：支援透過 `Ctrl+V`、拖放、右鍵將圖片等檔案直接貼到終端機
- **Minimap 測試涵蓋率指示器**：在 Minimap 中顯示測試涵蓋率缺口
- **整合式瀏覽器 macOS 捏合縮放**：支援觸控板捏合手勢純視覺放大
- **`code serve-web` CLI 新選項**：新增 `--disable-telemetry`、`--default-folder`、`--default-workspace`

---

## GitHub Copilot

### VS Code Agents 伴隨應用程式（Preview）

Visual Studio Code Agents 是一個全新的預覽版伴隨應用程式，與 VS Code Insiders 一同發行，專為 agent-native 開發打造，讓您可以更有效率地管理與多個 Agent 並行的工作流程。

#### 跨專案平行任務（Parallelize tasks across projects）

您可以在多個儲存庫中平行啟動 Agent 工作階段，每個工作階段都隔離於自己的 Git worktree 中，避免彼此影響。UI 會根據您目前選取的工作階段動態調整，讓您能快速切換上下文，並在人工審查與 Agent 審查之間反覆迭代。

#### 監控與審查（Monitor and review）

您可以追蹤工作階段進度、內嵌（inline）檢視檔案 diff、對 Agent 留下回饋，並直接在 Agents 應用程式中建立 Pull Request，整個流程都無需離開該應用。

#### 自訂項目無縫帶入（Your customizations carry over）

您現有的自訂項目會在 Agents 應用程式中同樣適用，包括自訂指令（custom instructions）、提示檔案（prompt files）、自訂 Agent、MCP 伺服器、Hooks、以及外掛程式（plugins）。此外，主題等其他 VS Code 自訂項目也會一併套用。

### 背景終端機的 Agent 工具（Terminal Tools for Background Agents）

本次發行為 Agent 與背景終端機互動提供了全新的能力。具體而言，新的實驗性設定 `chat.tools.terminal.backgroundNotifications` 啟用後，當背景終端機的命令完成執行或要求使用者輸入時，Agent 會自動被通知。

此機制同樣適用於**前景終端機逾時被移至背景**的情況。收到通知後，Agent 可以採取適當行動，例如檢視輸出內容，或是透過新的 `send_to_terminal` 工具向終端機送出輸入。

`send_to_terminal` 工具讓 Agent 可以在使用者確認下執行命令，而不再因背景程序干擾而靜默失敗。

### Agent 工作階段檔案編輯追蹤

Agent 工作階段現在具備追蹤檔案編輯的能力，可呈現 diff，並提供自動執行期間自訂項目變更的 undo／redo 功能。

### 聊天參考瀏覽器分頁

聊天功能現在可以參考工作階段期間所開啟的瀏覽器分頁，讓 Agent 能將網頁內容納入對話脈絡，提供更完整的上下文。

---

## 整合式瀏覽器（Integrated Browser）

### Agent 瀏覽器分頁行為

Agent 現在被更強烈地抑制重複開啟瀏覽器分頁。當 Agent 嘗試開啟新分頁、而同一主機已有可用分頁時，除非 Agent 明確傳入旗標，否則不會建立新分頁。此改進避免了工作階段中產生大量重複分頁的情況。

此外，當 Agent 呼叫瀏覽器工具時，工具呼叫現在會顯示更具描述性的標籤，並提供連結可直接跳至目標瀏覽器分頁，讓您更容易追蹤 Agent 正在操作的頁面。

### macOS 捏合縮放（Pinch-to-Zoom on Mac）

整合式瀏覽器在 macOS 上新增支援觸控板捏合縮放手勢。您可以使用觸控板的捏合手勢將網頁內容放大最多 3 倍。

與標準瀏覽器縮放不同，捏合縮放是純視覺性放大，**不會** 重新排版網頁版面。這讓您可以在不改變頁面版面的情況下，放大檢視特定區域的細節。

---

## 終端機（Terminal）

### 將檔案貼上終端機（Paste Files into Terminal）

終端機現在支援將檔案（例如圖片）直接貼上，支援下列三種方式：

- `Ctrl+V`（鍵盤貼上）
- 拖放（drag-and-drop）
- 右鍵貼上

過去若要在終端機中使用本地檔案，必須先將檔案存到磁碟上、再手動輸入或複製路徑。此改進消除了這個繁瑣步驟，對於偵錯工作階段、Agent 互動、以及其他需要快速提供檔案給 CLI 工具的場景特別有用。

---

## 編輯器（Editor）

### Minimap 顯示測試涵蓋率指示器（Test Coverage Indicators in Minimap）

編輯器的 Minimap（縮圖）現在可以顯示測試涵蓋率指示器。當您執行帶有涵蓋率的測試後，Minimap 會以視覺化方式呈現哪些程式碼行已被涵蓋、哪些尚未涵蓋，讓開發者能快速掃視並找出缺少測試涵蓋的區域。

此功能對於大型檔案特別有幫助，您無須逐行捲動即可了解檔案的整體涵蓋率狀況。

---

## CLI（`code serve-web`）

### 新增 CLI 選項

`code serve-web` CLI 命令新增了下列選項，強化本地伺服器與受控環境部署的彈性：

| 選項 | 說明 |
|------|------|
| `--disable-telemetry` | 停用遙測資料收集 |
| `--default-folder` | 指定伺服器啟動時開啟的預設資料夾 |
| `--default-workspace` | 指定伺服器啟動時開啟的預設工作區 |

這些選項對企業部署與自訂化工作流程非常重要，讓管理員能更精確地控制 `code serve-web` 的啟動行為，並符合隱私與合規需求。

---

## 無障礙功能（Accessibility）

### 無障礙檢視動態串流聊天回應

無障礙檢視（Accessible View）現在會動態串流聊天回應，在 AI 生成內容的過程中即時顯示。先前您必須關閉並重新開啟無障礙檢視才能看到最新內容，現在可以留在無障礙檢視中即時追蹤 AI 回應，大幅改善跟讀 AI 輸出的體驗。

### `${activeEditorLanguageId}` 變數

`window.title` 設定新增了 `${activeEditorLanguageId}` 變數，可用於在視窗標題中顯示目前作用中編輯器的語言識別碼。

此變數對於 Talon 等仰賴目前程式語言的無障礙工具特別有用，這類工具能透過視窗標題判斷目前的語言並套用對應的語音命令集。

---

## Git 與原始碼控制（Source Control）

### Worktree 忽略路徑計算最佳化

Git 整合最佳化了 worktree 忽略路徑（ignored-path）的計算。在使用 Git worktree 功能時，VS Code 處理 `.gitignore` 的效率有所提升，特別是在大型儲存庫或含多個 worktree 的場景下能明顯感受到改善。

---

## 重要修正（Notable Fixes）

- 修正背景 Agent 工作階段行為，先前會在自動化工作流程中讓使用者感到困惑
- 還原 macOS 整合式瀏覽器的捏合縮放功能（此前版本曾短暫失效）
- Git worktree 忽略路徑計算效能最佳化

---

## 新設定摘要

| 設定 | 說明 |
|------|------|
| `chat.tools.terminal.backgroundNotifications`（實驗性） | 啟用後，背景終端機命令完成或需要輸入時會自動通知 Agent |

---

## 新 CLI 選項摘要（`code serve-web`）

| 選項 | 說明 |
|------|------|
| `--disable-telemetry` | 停用 serve-web 遙測 |
| `--default-folder` | 指定預設開啟資料夾 |
| `--default-workspace` | 指定預設開啟工作區 |

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| VS Code Agents | VS Code Agents 伴隨應用程式 |
| Companion App | 伴隨應用程式 |
| Agent-native Development | Agent 原生開發 |
| Parallelize tasks | 平行任務 |
| Worktree | Git 工作樹（worktree） |
| Worktree Isolation | Worktree 隔離 |
| Monitor and Review | 監控與審查 |
| Inline Diffs | 內嵌 diff |
| Pull Request | Pull Request（PR） |
| Custom Instructions | 自訂指令 |
| Prompt Files | 提示檔案 |
| Custom Agents | 自訂 Agent |
| MCP Servers | MCP 伺服器 |
| Hooks | Hooks |
| Plugins | 外掛程式 |
| Terminal Tools | 終端機工具 |
| Background Terminals | 背景終端機 |
| Foreground Terminals | 前景終端機 |
| chat.tools.terminal.backgroundNotifications | chat.tools.terminal.backgroundNotifications 設定 |
| send_to_terminal | send_to_terminal 工具 |
| Exit Code | 結束代碼 |
| Paste Files into Terminal | 終端機貼上檔案 |
| Drag-and-drop | 拖放 |
| Right-click Paste | 右鍵貼上 |
| Minimap | 縮圖（Minimap） |
| Test Coverage Indicators | 測試涵蓋率指示器 |
| Pinch-to-Zoom | 捏合縮放 |
| Integrated Browser | 整合式瀏覽器 |
| Visual Magnification | 視覺放大 |
| Reflow | 重新排版 |
| Browser Tabs | 瀏覽器分頁 |
| Browser Tool | 瀏覽器工具 |
| File Edits with Diff | 檔案編輯 diff |
| Undo/Redo | 復原／重做 |
| code serve-web | code serve-web CLI |
| --disable-telemetry | --disable-telemetry 選項 |
| --default-folder | --default-folder 選項 |
| --default-workspace | --default-workspace 選項 |
| Accessible View | 無障礙檢視 |
| Dynamic Streaming | 動態串流 |
| ${activeEditorLanguageId} | ${activeEditorLanguageId} 變數 |
| window.title | window.title 設定 |
| Language Identifier | 語言識別碼 |
| Talon | Talon（語音控制工具） |
| Git Worktree | Git 工作樹 |
| Ignored-path Computation | 忽略路徑計算 |
| VS Code Insiders | VS Code Insiders（預覽版） |

---

*資料來源：VS Code 1.115 發行說明 (https://code.visualstudio.com/updates/v1_115)*