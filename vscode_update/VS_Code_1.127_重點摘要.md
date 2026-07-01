# Visual Studio Code 1.127 版本重點摘要

**版本：** 1.127
**發行日期：** 2026 年 7 月 1 日
**原文：** https://code.visualstudio.com/updates/v1_127

---

本次發行帶來可在瀏覽器中建置和測試 Web 應用程式的 Agent、更安全的逐站瀏覽，以及保持繁忙 Agent 工作階段井然有序的新方式。以下為官方列出的五大亮點：

## 一、Agent 瀏覽器工具正式可用

- **設定**：`workbench.browser.enableChatTools`（由組織層級管理）
- 瀏覽器工具讓 Agent 可在整合式瀏覽器中開啟頁面、讀取內容和主控台錯誤、擷取截圖、選取、輸入和導覽以驗證自身工作，無需外部 MCP 伺服器
- 經過多個 Preview 階段後正式可用且預設啟用
- 管理員可透過企業政策控制：使用 `BrowserChatTools` 政策完全停用，或透過 `ChatAgentNetworkFilter` 搭配允許/拒絕網域清單限制 Agent 可達到的網域

## 二、逐站瀏覽器權限

- 整合式瀏覽器支援逐站權限，涵蓋地理位置、攝影機和麥克風、感測器（加速計、陀螺儀）、剪貼簿、裝置（藍牙、USB、序列、HID）
- 頁面請求權限時，VS Code 會如傳統瀏覽器般提示允許或拒絕
- 可從 **Site Permissions** 瀏覽器選單項目管理目前站台的權限

## 三、Agent 工作階段組織

- **群組**：可建立自訂群組將相關工作階段歸類，摺疊群組標題以整理清單，群組提供快速操作（直接在群組中啟動新工作階段、一鍵標記所有工作階段為完成）
- **拖放**：支援重新排序工作階段、拖曳群組和工作區標題、拖曳工作階段至群組或釘選區段、多選工作階段批次移動
- **多聊天工作階段改善**：關閉/重新開啟/刪除聊天、進度與變更跨所有聊天彙總、分支對話建立同工作階段的對等聊天
- **工作階段佈局**：一致的 Workspace 和 Changes 膠囊按鈕、切換工作階段時焦點移至聊天輸入、響應式工作階段側邊列（`sessions.layout.autoCollapseSessionsSidebar`）

## 四、聊天輸入橫幅

- Agent 工作階段有開放的 PR 時，聊天輸入上方顯示橫幅
- **CI 失敗**：顯示失敗的檢查數量，提供「Fix Checks」和「Reveal Checks」快速操作
- **PR 留言**：顯示留言數量，提供「Address Comments」和「Reveal Comments」操作

## 五、子代理點數

- 懸停子代理區段可查看該子代理使用的 AI 點數，使委派工作的成本更透明

---

## 其他

- **macOS 和 Linux 的終端機命令沙箱**：Agent 呼叫的終端機命令在封鎖網路存取和限制檔案系統存取的沙箱中執行，減少核准提示；命令需要升級時才會請求核准
- **使用 /troubleshoot 診斷 Agent 行為**：可在 Agents 視窗中使用 `/troubleshoot` 搭配 `#session` 診斷 agent host 工作階段（含本機和遠端）
- **引導導覽（實驗性）**：Agents 視窗提供引導式導覽幫助新使用者快速上手
- **編輯器邊界回饋**：審閱 Agent 變更時，懸停行號區可顯示「Add Feedback」圖示直接在該行留下留言
- **更好的 PR 標題和描述**：Create Pull Request 按鈕使用工作階段上下文產生更準確的 PR 標題和描述
- **棄用內建 Ollama 供應商**：Ollama 現有[官方 VS Code 擴充功能](https://marketplace.visualstudio.com/publishers/Ollama)，建議改用擴充功能
- **企業：檔案型受管理 Copilot 設定傳遞**：可從磁碟上的 `managed-settings.json` 檔案傳遞受管理設定（macOS/Linux/Windows 各有固定路徑），在 MDM 或帳戶型企業設定不存在時生效

---

*資料來源：[Visual Studio Code 1.127 發行說明](https://code.visualstudio.com/updates/v1_127)*
