# Visual Studio Code 1.136 版本重點摘要

**版本：** 1.136
**發行日期：** 2026 年 9 月 2 日
**原文：** https://code.visualstudio.com/updates/v1_136

---

本次發行協助您以 Agent 完成 Pull Request，並跨複雜工作區和相關聊天管理 Agent 工作。以下為官方列出的四大亮點：

## 一、Agent Merge（Preview）

- **設定**：`chat.agentMerge.enabled`
- 協助您將 Pull Request 推過終點線：請 Agent 處理審閱回饋、修正失敗的檢查和合併衝突，並重新執行工作流程
- Agent Merge 會重複此流程，直到 Pull Request 可以合併為止
- 目前只能從 Agents 視窗為工作階段啟用：執行 **Enable Agent Merge for Active Session** 或選取標題列中的 **Agent Merge** 按鈕

## 二、編輯器視窗中的多根工作區（實驗性）

- **設定**：`chat.agentHost.copilotAgent.multiRootEnabled`、`chat.agentHost.claudeAgent.multiRootEnabled`
- 編輯器視窗 Chat 檢視中的 Copilot 和 Claude Agent 工作階段支援[多根工作區](https://code.visualstudio.com/docs/editing/workspaces/multi-root-workspaces)
- 此功能目前範圍僅限編輯器視窗
- [Agent hooks](https://code.visualstudio.com/docs/agent-customization/hooks) 仍限於單一工作區資料夾；若在多個資料夾中偵測到 hooks，VS Code 會提示您選取要從哪個主要資料夾載入

## 三、Agents 視窗中的聊天背景（實驗性）

- **設定**：`chat.agentSessions.preferredDarkBackgroundImage`、`chat.agentSessions.preferredLightBackgroundImage`、`chat.agentSessions.backgroundImageLayout`
- 可用感知佈景主題的內建 VS Code 圖示圖樣，或您自己的圖片來個人化 Agents 視窗
- 執行 **Chat: Set Background...** 在 **Codicons** 圖樣和本機圖片之間選擇，最近使用的五張圖片會列在 **Recently Used**（僅保存於本機）
- 使用自己的圖片時，執行 **Chat: Change Background Layout...** 進行配置，共有 11 種版面：**Repeat**、**Stretch**、**Center**，以及各個邊緣和角落；瀏覽清單時會就地預覽。**Chat: Clear Background** 可回到純色表面
- 深色與淺色佈景主題各自保留背景，切換主題時背景一併切換；高對比佈景主題完全抑制背景，且該三個命令在其中不可用
- 聊天內容自帶填色以維持可讀性：您的請求保持完全不透明，Agent 回應在兩側邊距漸隱，Markdown 表格和終端機輸出等寬內容則保持完整背襯

## 四、聊天工作階段（瀏覽相關聊天與工作階段）

- 聊天在 Agents 視窗的工作階段清單中呈現為其父工作階段的子項目，讓您理解哪些聊天屬於同一組
- 每個聊天列顯示自己的標題、狀態和待處理核准，可看出哪個聊天需要您的輸入
- 可展開或摺疊階層，並直接從樹狀結構開啟、重新命名、移動或刪除個別聊天
- 當 Agent 將獨立工作委派給多個聊天時，每個建立的聊天都會取得有意義的標題並出現在此階層中
- 新工作階段或聊天會放置在其來源附近，接收的請求包含來源連結（如 **Sent by another session** 或 **Sent from another chat**），選取即可返回發起它的確切工作階段或聊天

---

## 其他

- **The Story of VS Code 全球首映**：從早期起源到今日數百萬開發者使用的平台，以及一路上形塑它的社群。首映時間：9 月 4 日太平洋時間上午 8:00
- **重新設計的新工作階段輸入**：將提示、模型選取、工作區選取和其他工作階段控制項整合於一個版面中，減少設定步驟
- **改善的工作區解析**：Agent 除了絕對路徑和工作區 URI 外，也能以專案名稱解析工作區；工作階段工具亦保留多根工作區的專案 URI 和所有工作目錄。可直接說「run this in the vscode workspace」而不需提供完整路徑；若多個工作區同名，Agent 會回報可能的相符項目而非默默選一個。也支援遠端工作區 URI
- **工作階段檔案的可讀階層連結**：內部工作階段狀態目錄中建立的檔案，階層連結改用穩定的供應商與工作階段標籤，不再顯示內部工作階段識別碼
- **Agent 工作階段通知**（`chat.notifyWindowOnConfirmation`、`chat.notifyWindowOnResponseReceived`）：當 Agent 工作階段需要您的輸入或完成工作時通知您。預設僅在 VS Code 視窗未取得焦點時顯示，可分別為「需要輸入」和「已收到回應」設定；通知含直接連結，選取後會聚焦正確視窗並開啟該工作階段
- **Agent host**：讓您可從多個 VS Code 視窗連線至同一 Agent 工作階段，依 [AHP](https://microsoft.github.io/agent-host-protocol/) 在專用程序中執行 Agent 工具鏈；Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動。另發布[新的 agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)
- **聽寫資料的企業控制**：管理員可透過企業政策管理聽寫模型與語言模型逐字稿清理，可要求裝置端轉錄並停用語言模型清理，讓聽寫維持可用的同時避免資料送往雲端轉錄或 Copilot 模型
- **協助工具：Agents 視窗中的 Screen Reader Optimized 徽章**：啟用螢幕閱讀器最佳化模式時，Agents 視窗標題列會顯示徽章，選取徽章可停用該模式
- **編輯器視窗的版面密度（實驗性）**（`workbench.experimental.modernUI`、`window.density.layout`）：啟用現代化 UI 後可選擇 **Default**（同現行版面）或 **Compact**（移除面板間距並縮減面板內間距）
- **程式碼編輯：自動換行改善**：插入的文字不再將換行後的行推出編輯器可視區域，自動換行會將色彩裝飾器、Inlay 提示間距、行內進度指示器和中斷點預留位置的視覺寬度納入計算
- **整合式瀏覽器：拼字檢查建議**：在可編輯欄位中對拼錯的字按右鍵可選取建議的更正；在使用持續性資料儲存的工作階段中，還可選取 **Add to Dictionary**
- **終端機：減少執行命令的延遲**：擴充功能執行的終端機命令在特定時序條件下 Shell 整合就緒時，不再產生不必要的延遲；使用 JavaScript 偵錯工具而遇到此情況的使用者，啟動程式時不會再有五秒延遲
- **已棄用的功能和設定**：無

---

*資料來源：[Visual Studio Code 1.136 發行說明](https://code.visualstudio.com/updates/v1_136)*
