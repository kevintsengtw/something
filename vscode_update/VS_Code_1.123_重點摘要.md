# Visual Studio Code 1.123 版本重點摘要

**版本：** 1.123
**發行日期：** 2026 年 6 月 3 日
**原文：** https://code.visualstudio.com/updates/v1_123

---

本次發行改善了 Agent 和整合式瀏覽器的使用方式。以下為官方列出的四大亮點：

## 一、工作階段同步與 Chronicle

- **設定**：`chat.sessionSync.enabled`（由組織層級管理）
- 聊天工作階段自動同步至 GitHub 帳戶，提供跨機器和工作區的個人化、可搜尋的工作歷史
- 每個工作階段捕獲對話內容、觸及的檔案、儲存庫上下文（repo、分支、時間戳記）以及引用的 Pull Request、Issue 或提交
- 全新 chronicle 命令（`/chronicle`）：以自然語言查詢過去的工作階段、產生站立會議報告、取得個人化生產力建議、依主題/檔案/PR 搜尋編碼歷史

## 二、Agents 視窗（Preview）

- **多個工作階段並排開啟**：可同時在 Agents 視窗中開啟多個工作階段，透過右鍵選單「Open to the Side」、拖放或按住 Alt 點擊開啟
- 同一時間只有一個活動工作階段，Terminal、Files 和 Changes 檢視操作目前活動的工作階段
- 支援**釘選**工作階段檢視（不被替換）和**最大化**單一工作階段檢視

## 三、研究 Agent（Preview）

- 目前僅在 Insiders 的 Copilot CLI（本機）工作階段中提供
- 對主題進行深度研究，從程式碼庫、相關 GitHub 儲存庫和網路蒐集並綜合資訊，產出詳盡且有引用來源的 Markdown 報告
- 針對深度而非速度最佳化，僅有唯讀存取權限
- 透過聊天輸入 `/research` 加上主題來執行

## 四、整合式瀏覽器更新

- **我的最愛頁面**：網址列重新設計，可將頁面加入我的最愛（星號圖示），選取網址列時可查看我的最愛清單和已開啟的分頁
- **更多截圖擷取方式**：
  - **Add Area Screenshot to Chat**：擷取矩形區域截圖附加至聊天
  - **Add Full Page Screenshot to Chat（實驗性）**：擷取整個網頁截圖（含超出視窗範圍的部分），需啟用 `workbench.browser.experimentalUserTools.enabled`

---

## 其他

- **沙箱中重試網路相關命令**（`chat.agent.sandbox.retryWithAllowNetworkRequests`）：本機 Agent 的終端機命令需要存取未設定為允許網域的網域時，自動在沙箱中以不受限的網路存取重試，仍失敗則退回無沙箱執行
- **延遲擴充功能自動更新**：新發佈的擴充功能版本在自動更新前有兩小時延遲，作為額外保護層；不適用於 Microsoft、GitHub 和 OpenAI 等受信任發行者；使用者仍可隨時手動立即更新

---

*資料來源：[Visual Studio Code 1.123 發行說明](https://code.visualstudio.com/updates/v1_123)*
