# Visual Studio Code 1.124 — 重點摘要

_發布日期：2026 年 6 月 10 日_

本次 1.124 版本聚焦於兩大方向：**讓你在多個 agent（代理）工作階段之間切換得更快**，以及**賦予 agent 更高的自主權來完成你的任務**。

## 四大重點

- **Autopilot（自動駕駛）預設啟用**：Autopilot 現已預設開啟，並變得更聰明，能更準確判斷任務是否真正完成。透過一個小型的工具模型（utility model）閱讀對話記錄來決定任務是否已完成，最多迭代三次。
- **新工作階段的背景送出（Background send）**：可快速在背景送出請求，同時繼續撰寫下一個工作階段。按 Alt+Enter（或按住 Alt 並選擇「Send」），畫面會立即重設並保留狀態（如所選模型與內容），只清除查詢文字。
- **工作階段導覽（Session navigation）**：透過鍵盤搜尋、跳轉與逐步切換 agent 工作階段。包含可搜尋的選擇器（Ctrl+R）、前後導覽（Ctrl+Tab／Ctrl+Shift+Tab）、上一個／下一個工作階段、以及依位置聚焦（Ctrl+1 至 Ctrl+9）。
- **瀏覽器歷史記錄（Browser history）**：可重新瀏覽並搜尋你在整合式瀏覽器中開啟過的頁面，歷史項目會在 URL 列輸入時以建議形式出現。

## 各區段更新一覽

### Agents 視窗（預覽）

- **新工作階段的背景送出**：在背景送出請求，畫面立即重設並保留狀態，只清除查詢文字，可持續排入請求。
- **在工作階段之間導覽**：工作階段選擇器（Ctrl+R）、往回／往前（Ctrl+Tab／Ctrl+Shift+Tab）、上一個／下一個工作階段（Alt+Up／Alt+Down 等）、依位置聚焦（Ctrl+1 至 Ctrl+9）。
- **重新載入時還原工作階段**：自動還原先前可見的工作階段格線、各工作階段版面配置與工作階段清單狀態。
- **關閉所有工作階段**：新的 **Close All Sessions** 命令，快速鍵 Ctrl+K Ctrl+W。
- **Changes 檢視中的單檔差異比對（預覽）**：啟用 `sessions.changes.openSingleFileDiff` 可在選取檔案時一律開啟聚焦的單檔差異編輯器。
- **以側邊欄箭號加寬編輯器**：編輯器標題列新增箭號切換按鈕，可摺疊輔助列以加寬編輯器。

### Autopilot（預覽）

- **Autopilot 預設啟用**：以 `chat.permissions.default` 設定預設權限層級；組織可透過 `chat.tools.global.autoApprove` 控制（由組織層級管理）。
- **進階 Autopilot**：由小型工具模型閱讀對話記錄判斷任務是否完成，目標顯示於聊天上方工具提示，最多迴圈三次。設定 `chat.autopilot.advanced.enabled` 為 `true` 啟用。

### 編輯器體驗

- **從簡易檔案對話框開啟資料夾時建立資料夾**：可直接輸入名稱並按 Enter／OK 建立新資料夾。
- **整合式瀏覽器 — 歷史記錄**：保留已造訪頁面歷史，URL 列輸入時顯示建議，以 ⌘H／Ctrl+H 管理，數量上限由 `workbench.browser.maxHistoryEntries` 調整。
- **整合式瀏覽器 — 改進的工具列可自訂性**：溢位選單中的所有動作皆可右鍵設為持續顯示。
- **整合式瀏覽器 — 更快的 agentic 文字輸入**：`typeInPage` 工具新增 `submit` 參數，可在單一工具呼叫中輸入文字並按 Enter。

### 企業（Enterprise）

- **企業管理的 Copilot 外掛政策（實驗性）**：VS Code 與 Copilot CLI 共用同一政策設定檔。管理員可集中控制可用的聊天外掛與外掛市集，三項政策設定為 `chat.plugins.enabledPlugins`、`chat.plugins.extraMarketplaces`、`chat.plugins.strictMarketplaces`。

### 已棄用的功能與設定

- 無
