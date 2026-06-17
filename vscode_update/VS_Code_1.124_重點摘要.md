# Visual Studio Code 1.124 版本重點摘要

**版本：** 1.124
**發行日期：** 2026 年 6 月 10 日
**原文：** https://code.visualstudio.com/updates/v1_124

---

本次發行讓跨 Agent 工作階段的工作更快速，並賦予 Agent 更多自主權來完成您的任務。以下為官方列出的四大亮點：

## 一、Autopilot 預設啟用

- **設定**：`chat.permissions.default`、`chat.tools.global.autoApprove`（由組織層級管理）
- Autopilot 現在在 VS Code 中預設啟用，Agent 可自主行動無需每個操作都獲得使用者核准
- 組織仍可透過 `chat.tools.global.autoApprove` 控制 Autopilot 的可見性和使用
- **Advanced Autopilot**（`chat.autopilot.advanced.enabled`）：使用小型公用模型讀取聊天紀錄判斷任務是否完成，目標顯示在聊天上方的工具提示中，最多迴圈三次後停止

## 二、背景傳送新工作階段

- 在新工作階段檢視中按 Alt+Enter（或按住 Alt 選取 **Send**），可在背景傳送請求
- 檢視立即重設並保留狀態（已選模型和上下文），僅清除查詢文字，可持續排隊請求
- 每個啟動的工作階段在提交後出現在工作階段清單中

## 三、工作階段導覽

- **工作階段選擇器**（Ctrl+R / Cmd+R）：開啟 Quick Pick，依「recently opened」和「other sessions」分組列出工作階段，支援標題和資料夾搜尋，可在側邊或背景開啟
- **前進/後退**：Ctrl+Tab 後退、Ctrl+Shift+Tab 前進，依最近造訪順序
- **上一個/下一個工作階段**：Alt+Up / Alt+Down 依顯示順序步進
- **依位置聚焦**：Ctrl+1 至 Ctrl+9 聚焦網格中第 N 個可見工作階段
- **重新載入時還原工作階段**：可見工作階段網格、每工作階段佈局、工作階段清單狀態全部自動還原
- **關閉所有工作階段**：Ctrl+K Ctrl+W 一次關閉所有工作階段

## 四、瀏覽器歷史記錄

- **設定**：`workbench.browser.maxHistoryEntries`
- 整合式瀏覽器保留造訪頁面的歷史記錄，歷史項目在網址列輸入時作為建議顯示
- 可透過 ⌘H（Windows/Linux Ctrl+H）在瀏覽器分頁中管理歷史記錄

---

## 其他

- **Changes 檢視中的單檔差異（Preview）**（`sessions.changes.openSingleFileDiff`）：啟用後在 Changes 檢視中選取檔案時開啟單檔差異編輯器而非多檔差異
- **側邊列展開/收合箭頭**：編輯器標題列新增箭頭切換，可收合次要側邊列以加寬編輯器
- **改善的工具列自訂性**：整合式瀏覽器工具列所有溢出選單操作皆可透過右鍵選單設為持續顯示
- **更快的 Agent 文字輸入**：`typeInPage` 工具新增 `submit` 參數，一次工具呼叫即可輸入文字並按 Enter
- **簡易檔案對話框建立資料夾**：從簡易檔案對話框開啟資料夾時，可直接輸入名稱建立新資料夾
- **企業管理的 Copilot 外掛政策（實驗性）**：VS Code 讀取與 Copilot CLI 相同的企業設定檔，管理員可集中控制聊天外掛和外掛市集（`chat.plugins.enabledPlugins`、`chat.plugins.extraMarketplaces`、`chat.plugins.strictMarketplaces`）

---

*資料來源：[Visual Studio Code 1.124 發行說明](https://code.visualstudio.com/updates/v1_124)*
