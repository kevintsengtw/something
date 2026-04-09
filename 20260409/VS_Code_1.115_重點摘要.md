# Visual Studio Code 1.115 版本重點摘要

**版本：** 1.115（2026 年 4 月 8 日發行）
**類型：** 每週穩定版（Weekly Stable Release）
**主題：** 強化 agent-native 開發體驗、背景終端機互動與整合式瀏覽器改善

---

## 一、VS Code Agents 伴隨應用程式（Companion App，預覽版）

- 全新的 **VS Code Agents** 預覽版伴隨應用程式，與 VS Code Insiders 一同發行
- 專為 agent-native 開發設計，讓您可以在多個專案與儲存庫中平行執行 Agent 工作階段
- **跨專案平行任務**：可同時在多個儲存庫啟動 Agent 工作階段，每個皆隔離在獨立的 Git worktree 中，並可快速切換上下文
- **監控與審查**：追蹤工作階段進度、內嵌檢視 diff、對 Agent 留下回饋，並可直接在應用程式中建立 Pull Request，無需離開該應用
- **自訂項目同步**：自訂指令（custom instructions）、提示檔案（prompt files）、自訂 Agent、MCP 伺服器、Hooks、外掛程式，以及主題等 VS Code 自訂項目全部可帶入 Agents 應用程式

## 二、背景終端機的 Agent 工具（Terminal Tools for Background Agents）

- 新增實驗性設定 `chat.tools.terminal.backgroundNotifications`
- 當背景終端機的命令完成或需要輸入時，Agent 會自動收到通知（包含結束代碼與輸出內容）
- 對於前景終端機逾時並被移至背景的情況同樣適用
- Agent 可透過新的 `send_to_terminal` 工具檢視輸出或輸入內容，並在需要使用者確認下執行命令，不再因背景程序干擾而靜默失敗

## 三、將檔案貼至整合式終端機（Paste Files into Terminal）

- 終端機現在支援將檔案（例如圖片）直接貼上
- 支援三種方式：`Ctrl+V`、拖放（drag-and-drop）、以及右鍵貼上
- 消除過去必須先把檔案存到硬碟、再手動輸入路徑的繁瑣流程，特別適合偵錯工作階段

## 四、Minimap 顯示測試涵蓋率指示器（Test Coverage in Minimap）

- 編輯器的 Minimap（縮圖）現可顯示測試涵蓋率指示器
- 讓開發者可以更容易地掌握檔案中哪些區域缺少測試涵蓋

## 五、整合式瀏覽器 — macOS 捏合縮放（Pinch-to-Zoom on Mac）

- 整合式瀏覽器在 macOS 上新增支援觸控板捏合縮放手勢（pinch-to-zoom）
- 最多可放大至 3x
- 與標準瀏覽器縮放不同，此為純視覺性放大，**不會** 重新排版網頁內容
- 適合在不改變版面配置的情況下檢視細節

## 六、Agent 整合式瀏覽器行為改善

- Agent 現在被強烈抑制重複開啟相同瀏覽器分頁：若目標主機已有分頁開啟，除非 Agent 明確傳入旗標，否則不會再建立新分頁
- Agent 呼叫瀏覽器工具時，工具呼叫顯示更具描述性的標籤，並提供直接連結至目標瀏覽器分頁
- 聊天功能現可參考工作階段期間所開啟的瀏覽器分頁

## 七、Agent 工作階段檔案編輯追蹤（File Edits with Diff / Undo / Redo）

- Agent 工作階段現在可以追蹤檔案編輯，並顯示 diff
- 支援在自動執行過程中對自訂項目進行 復原／重做（undo/redo）

## 八、`code serve-web` CLI 新選項

- `code serve-web` CLI 命令新增以下選項：
  - `--disable-telemetry`：停用遙測
  - `--default-folder`：指定預設資料夾
  - `--default-workspace`：指定預設工作區
- 對本地伺服器工作流程與受控環境部署有直接幫助

## 九、無障礙功能改善（Accessibility）

- 無障礙檢視（Accessible View）現可**動態串流**聊天回應，在回應生成時即時顯示，不需關閉並重新開啟檢視即可追蹤 AI 回應
- 新增 `${activeEditorLanguageId}` 變數可用於 `window.title` 設定，顯示目前作用中編輯器的語言識別碼，對 Talon 等需要判斷目前程式語言的無障礙工具特別有用

## 十、Git 與重要修正（Notable Fixes）

- Git 最佳化 worktree 忽略路徑計算（ignored-path computation）
- 修正背景 Agent 工作階段在自動化工作流程中讓使用者混淆的行為
- 還原 macOS 整合式瀏覽器的捏合縮放功能

---

*資料來源：VS Code 1.115 發行說明 (https://code.visualstudio.com/updates/v1_115)*