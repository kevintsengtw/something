# VS Code 1.136 更新重點

**版本：** 1.136｜**發行日期：** 2026 年 9 月 2 日
**原文：** https://code.visualstudio.com/updates/v1_136

---

本版四大主軸為 Agent Merge、多根工作區、聊天背景與聊天工作階段階層：

- **Agent Merge（Preview）**（`chat.agentMerge.enabled`）：請 Agent 處理 PR 的審閱回饋、修正失敗檢查與合併衝突並重跑工作流程，反覆進行直到 PR 可合併。目前僅能從 Agents 視窗啟用——執行 **Enable Agent Merge for Active Session** 或點標題列的 **Agent Merge** 按鈕。
- **編輯器視窗多根工作區（實驗性）**（`chat.agentHost.copilotAgent.multiRootEnabled`、`chat.agentHost.claudeAgent.multiRootEnabled`）：編輯器視窗 Chat 檢視的 Copilot 與 Claude 工作階段支援多根工作區，範圍目前僅限編輯器視窗。Agent hooks 仍限單一資料夾，多個資料夾都有時會提示你選主要資料夾。
- **聊天背景（實驗性）**：可用感知佈景主題的 **Codicons** 圖樣或自己的圖片裝飾 Agents 視窗。**Chat: Set Background...** 選擇來源（最近五張存在本機）、**Chat: Change Background Layout...** 從 11 種版面配置（Repeat／Stretch／Center 與各邊角，可即時預覽）、**Chat: Clear Background** 還原。深淺色主題各存一份背景並隨主題切換，高對比主題完全停用。聊天內容自帶填色維持可讀性。
- **聊天工作階段階層**：聊天以子項目形式掛在父工作階段下，每列顯示自己的標題、狀態與待處理核准，可展開／摺疊並直接開啟、重新命名、移動、刪除。Agent 委派出的聊天會自動取得有意義的標題，接收端請求帶有 **Sent by another session**／**Sent from another chat** 來源連結可一鍵回溯。

其他更新：

- **The Story of VS Code 紀錄片**：9 月 4 日太平洋時間上午 8:00 全球首映。
- **重新設計的新工作階段輸入**：提示、模型、工作區與其他控制項整合在同一版面。
- **改善的工作區解析**：可用專案名稱指定工作區（例如「run this in the vscode workspace」），同名時會列出候選而非默默選一個；支援遠端工作區 URI，並為多根工作區保留專案 URI 與所有工作目錄。
- **可讀的工作階段階層連結**：不再於階層連結顯示內部工作階段識別碼，改用穩定的供應商與工作階段標籤。
- **Agent 工作階段通知**（`chat.notifyWindowOnConfirmation`、`chat.notifyWindowOnResponseReceived`）：工作階段需要輸入或完成時通知，預設僅在視窗未聚焦時出現，點擊可直接跳到對應視窗與工作階段。
- **Agent host**：另發布[架構部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)說明設計動機、開放協定與可試用的工作流程。
- **聽寫資料企業控制**：管理員可要求裝置端轉錄並停用語言模型清理，避免聽寫資料送往雲端或 Copilot 模型。
- **協助工具**：Agents 視窗標題列新增 **Screen Reader Optimized** 徽章，點選可關閉該模式。
- **版面密度（實驗性）**（`workbench.experimental.modernUI`、`window.density.layout`）：新增 **Compact** 密度，移除面板間距並縮減內距。
- **自動換行改善**：換行計算納入色彩裝飾器、Inlay 提示、行內進度指示器與中斷點預留位置的視覺寬度，內容不再被推出可視區域。
- **整合式瀏覽器拼字建議**：可編輯欄位右鍵取得更正建議，持續性資料儲存的工作階段還可 **Add to Dictionary**。
- **終端機延遲改善**：擴充功能執行命令時不再有不必要的等待，JavaScript 偵錯工具使用者不會再遇到五秒延遲。
- **棄用**：本版無。

**總結**：本版把 Agent 的守備範圍往「收尾」推進——Agent Merge 直接把 PR 帶到可合併狀態；同時在多根工作區、工作階段階層與通知上補齊了大型專案與多工作階段場景的管理能力，另有聊天背景這類個人化調劑。

---

*資料來源：[Visual Studio Code 1.136 發行說明](https://code.visualstudio.com/updates/v1_136)*
