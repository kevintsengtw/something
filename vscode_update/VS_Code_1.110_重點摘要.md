# Visual Studio Code 1.110 版本重點摘要

**版本：** 1.110（2026 年 2 月）
**發行日期：** 2026 年 3 月 4 日
**原文：** https://code.visualstudio.com/updates/v1_110

---

**安全更新**：GitHub Copilot Chat 擴充功能有安全更新。更新 1.110.1 解決了核心及 GitHub Copilot Chat 擴充功能中的安全問題。

本次發行**讓 Agent 在處理更長時間、更複雜的任務時更加實用**，提供更多控制與可見性、全新的 Agent 擴展方式，以及更智慧的工作階段管理。以下為官方列出的九大亮點：

## 一、Agent 外掛程式（實驗性）

- 全新的 Agent 外掛程式系統，可安裝包含技能、命令、Agent、MCP 伺服器和掛鉤的預打包套件
- 在擴充功能檢視中輸入 `@agentPlugins` 或執行 **Chat: Plugins** 搜尋安裝
- 預設從 `copilot-plugins` 和 `awesome-copilot` 儲存庫取得，可透過 `chat.plugins.marketplaces` 新增更多來源

## 二、Agent 瀏覽器工具（實驗性）

- Agent 可自主使用整合式瀏覽器與您的應用程式互動並驗證變更
- 提供頁面導航、內容讀取、螢幕截圖、使用者互動模擬、自訂 Playwright 程式碼執行等工具
- 預設在私有的記憶體中工作階段運行，可明確共享頁面授權
- 設定：`workbench.browser.enableChatTools`

## 三、工作階段記憶

- Plan Agent 建立的計畫持久化至工作階段記憶，跨對話輪次保持可用
- 進行調整時基於既有計畫修改而非重建
- 較舊的對話歷史被壓縮後，計畫仍可在記憶中存取

## 四、上下文壓縮

- VS Code 在上下文視窗達上限時自動壓縮對話歷史
- 新增手動壓縮：`/compact` 斜線命令，適用於本機、背景和 Claude Agent 工作階段
- 可在 `/compact` 後加入自訂指引（如 `/compact focus on the database schema decisions`）

## 五、分叉聊天工作階段

- `/fork` 命令建立繼承完整對話歷史的獨立工作階段
- 也可從任何聊天請求懸停選取 **Fork Conversation** 從特定檢查點分叉
- 分叉後的工作階段完全獨立

## 六、Agent Debug 面板（Preview）

- 即時查看聊天事件：自訂項目事件、系統提示、工具呼叫等
- 檢視每個工作階段載入了哪些提示檔案、技能、掛鉤等自訂項目
- 包含圖表檢視，顯示事件的視覺層次結構
- 取代舊的 Diagnostics 聊天動作

## 七、聊天無障礙功能

- 可切換思考內容在無障礙檢視中的顯示（⌥T）
- 問題輪播完全支援螢幕閱讀器（位置朗讀、Alt+N/Alt+P 導覽）
- 聊天問題和確認的通知訊號
- ⇧⌘T 在 TODO 清單和聊天輸入間切換焦點
- 無障礙檢視中記憶游標位置、尋找/篩選的無障礙說明

## 八、從聊天建立 Agent 自訂項目

- 新增 `/create-prompt`、`/create-instruction`、`/create-skill`、`/create-agent`、`/create-hook` 斜線命令
- 可從進行中的對話提取模式（如將除錯流程擷取為可重用技能）
- 也支援自然語言觸發

## 九、Kitty 圖形協定

- 整合式終端機支援 Kitty 圖形協定，可直接渲染高保真圖片
- 支援 PNG、24 位元 RGB、32 位元 RGBA 格式
- 設定：`terminal.integrated.enableImages`、`terminal.integrated.gpuAcceleration`

---

## 其他重要更新

- **背景 Agent 改善**：上下文壓縮、斜線命令、工作階段重新命名
- **Claude Agent 改善**：引導與排隊、工作階段重新命名、上下文視窗渲染與壓縮、新增斜線命令、getDiagnostics 工具、效能改善
- **自動核准斜線命令**：`/autoApprove`（`/yolo`）啟用全域自動核准
- **Edit Mode 與 Ask Mode 變更**：Edit Mode 正式棄用（預設隱藏），Ask Mode 改由自訂 Agent 定義驅動
- **askQuestions 工具**：移入 VS Code 核心，改善可靠性
- **防止聊天期間自動暫停**
- **usages 和 rename 工具**：Agent 可使用既有 LSP 能力精確導航和重構程式碼
- **Explore 子代理**：Plan Agent 將程式碼庫研究委派給專用的唯讀 Explore 子代理
- **內嵌聊天與聊天工作階段整合**：Agent 工作階段已修改檔案時，內嵌聊天排入該工作階段
- **重新設計的模型選擇器**：分為 Auto、精選/最近使用、其他模型等區段
- **情境式提示（實驗性）**：聊天檢視中顯示功能探索提示
- **自訂思考短語**：可自訂載入文字（`chat.agent.thinking.phrases`）
- **可摺疊的終端機工具呼叫**
- **OS 通知改善**：可設為即使視窗獲得焦點時也顯示
- **內嵌聊天懸停模式**、**內嵌聊天介面元素**
- **模態編輯器（實驗性）**：設定、鍵盤快捷鍵等以浮動視窗開啟
- **通知位置可配置**（右上、右下、左下）
- **設定編輯器清理**
- **長距離 NES**：在檔案任何位置預測和建議編輯
- **NES 積極度**：在 Copilot 狀態列調整建議頻率
- **Git AI 共同作者**：自動附加 `Co-authored-by:` 標記（`git.addAICoAuthor`）
- **JavaScript 除錯器**：自訂屬性替換、模擬焦點視窗和事件監聽器中斷點
- **Ghostty 外部終端機支援**（macOS/Linux）
- **外部終端機工作區資料夾選擇**
- **終端機沙箱化改善**
- **統一的 JavaScript/TypeScript 設定**：移至 `js/ts.*` 前綴
- **Python Environments 擴充功能**：向所有使用者推出
- **GitHub Pull Requests 擴充功能改善**
- **Webview 和自訂編輯器支援 ThemeIcon**
- **可攜式模式偵測 API 定案**
- **工程**：TypeScript-Go 用於 VS Code 開發、擴充功能改用 esbuild 打包

---

*資料來源：[Visual Studio Code 1.110 發行說明](https://code.visualstudio.com/updates/v1_110)*
