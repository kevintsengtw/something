# VS Code 1.137 更新重點

> 原文：<https://code.visualstudio.com/updates/v1_137>
> 發布日期：2026 年 9 月 9 日

## 依類別整理

### Agent 相關

| 項目 | 狀態 | 說明 |
| --- | --- | --- |
| Automations | 預覽 | 排程或手動執行重複性 agent 任務，內建追蹤變更、分類 issue、找 bug 等範本，支援每小時／每天／每週 |
| 開啟 GitHub issue 與 PR 詳細資料 | 實驗性 | 在 Agents 視窗中直接開啟 `github.com` 的 issue／PR 詳細資料，沒有開啟該儲存庫的工作區也能用 |
| 在任何 chat 中附加 issue 與 PR | 一般可用 | Chat 檢視、Chat 編輯器與 Agents 視窗的 **Add Context...** 都支援；貼上 URL 會自動附加 context |
| 在工作區中接續 quick chat | 一般可用（限 Copilot harness） | 未綁定工作區的 quick chat 可掛上本機資料夾或 worktree，轉成工作區工作階段並保留標題、對話紀錄與目前請求 |
| Agent 排入佇列的訊息 | 一般可用 | 透過 `send_message` 送給忙碌中的 chat 的訊息會排入佇列，依送出順序在目前回合成功結束後處理 |
| Agent host | 開發中 | 以 Agent Host Protocol（AHP）為基礎，在專屬處理程序執行 agent harness，可從多個 VS Code 視窗連到同一個 agent session；Copilot agent 由 Copilot SDK 驅動 |
| VS Code 寵物 | 實驗性 | 在 chat 輸入 `/vscode-pet` 認識這個會隨著 agent 互動反應的夥伴 |

### Chat

*   **Voice Mode（實驗性）**：與 agent 進行口語對話，可在 agent 說話時直接開口或用 push-to-talk 打斷。
*   Voice Mode 具備 session 感知能力：能回答執行中的 session、選用模型與已附加檔案的問題，可要求開新 session，並會說明請求被路由到哪裡。
*   可調整項目：顯示逐字稿、選擇麥克風（**Chat: Dictate: Select Microphone**）、選擇朗讀語音、重新開啟導覽介紹（**Voice Mode: Show Introduction**）；在 **Voice Mode** 按鈕上按右鍵可快速存取這些設定。
*   管理員可透過關閉組織的 Copilot 預覽功能停用 Voice Mode。

### 編輯器與 UI

*   **智慧型 diff 編輯器版面配置**：一般 diff、多檔 diff、Agents 視窗的 **Changes** 編輯器統一由 **More Actions（...）> Diff View** 選擇 **Inline**／**Side by Side**／**Automatic**；**Automatic** 會顯示目前生效的版面配置並隨寬度調整，手動拖曳 sash 加寬時 inline 會維持不變。
*   **多檔 diff 中的二進位檔案**：圖片等二進位檔案不再被略過，會以 **Binary file changed** 佔位提示留在原本位置，選 **Open Diff** 可用圖片 diff 或自訂編輯器檢視。
*   編輯器視窗的多檔 diff 與 Agents 視窗採用相同的視覺設計。

### 程式語言

*   **Markdown 編輯器中的 GitHub 連結（實驗性）**：issue 與 pull request 連結會顯示目前的標題與狀態，狀態改變（含 CI 更新）時連結也會更新；把 `chat.experimental.richLinks.enabled` 設為 `true` 可在 chat 中套用相同呈現方式。

### 擴充套件

*   **GitHub Pull Requests**：pull request webview 開啟速度更快；VS Code 中所有 github.com 連結都改由此擴充套件開啟，可用 `githubPullRequests.openPullLinks` 停用。完整內容見 0.166.0 版變更紀錄。

### 錯誤修復與社群貢獻

*   記憶體洩漏修正共七項（git 分支保護 provider、extension host 虛擬終端機、無障礙檢視工具列、測試結果、extension host comments、mainThreadNotebook、終端機設定檔服務），皆由 @SimonSiefke 提交。
*   其他修正包含：ListView 反轉可見範圍造成的 RangeError、dispose 時未完成的 docked tab 調和作業、copilotcli 診斷推送中的 null diagnostic code、IME 組字期間搜尋輸入框被重新聚焦、箭頭函式後用括號包住選取範圍、`extensions.allowed` schema 誤判合法版本陣列、webview view badge 設為 undefined 未清除、過期影像附件的自動重試行為。
*   其他強化：語音聽寫改用專屬 MAI 轉錄、Microsoft Entra ID 登入存取 Private Marketplace、`extensions.gallery.authProvider` 可由 policy 控制、mcp 避免因等效 URI 定義而停止 server、`editor.action.formatDocument.multiple` 支援 args、內建瀏覽器支援開啟 MHTML、向執行中的 subagent 顯示剩餘回合數、chat 遙測記錄 CAPI 的 X-Copilot-Service-Request-Id。

### 已棄用

本次版本沒有棄用任何功能或設定。

## 本次版本新增或提及的設定一覽

| 設定 | 用途 |
| --- | --- |
| `chat.automations.enabled` | 啟用 Automations |
| `extensions.experimental.enableAgentsWindowCapability` | 在 Agents 視窗中開啟 GitHub issue／PR 詳細資料 |
| `agents.voice.enabled` | 啟用 Voice Mode |
| `agents.voice.showTranscript` | 在 chat 輸入框顯示 Voice Mode 逐字稿 |
| `agents.voice.voice` | 選擇朗讀回應的語音 |
| `markdown.experimental.richLinks.enabled` | Markdown 編輯器中的 GitHub 豐富連結 |
| `chat.experimental.richLinks.enabled` | 在 chat 中使用相同的豐富連結呈現 |
| `githubPullRequests.openPullLinks` | 停用「所有 github.com 連結由 GitHub Pull Requests 擴充套件開啟」 |

## 值得關注的預覽與實驗性功能

*   **預覽**：Automations（逐步推送中）。
*   **實驗性**：Voice Mode、Agents 視窗開啟 GitHub issue／PR 詳細資料、Markdown 與 chat 的 GitHub 豐富連結、VS Code 寵物。
*   **開發中**：Agent host（歡迎透過 GitHub issue 回饋）。

## 時程相關

*   2026 年 9 月 9 日：1.137 發布。
*   2026 年 9 月 10 日：GitHub Copilot Day 直播。
*   2026 年 9 月 10 日至 9 月 17 日：VS Code 寵物命名活動徵件期間。

## 使用前提

*   在 Agents 視窗開啟 GitHub issue／PR 詳細資料，需要在**預設的 VS Code 設定檔**中安裝 GitHub Pull Requests 擴充套件。
*   在工作區工作階段中接續 chat 對話，目前**只有 Copilot harness 支援**。
*   Automations 與 VS Code 一樣採逐步推送，啟用設定後不一定立即出現。
