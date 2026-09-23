# VS Code 1.138 重點摘要

> 原文：<https://code.visualstudio.com/updates/v1_138>
> 發布日期：2026 年 9 月 16 日
> 本文件依據「全文翻譯」逐項整理，不含原文以外的內容。

## 一、Agents

### 1. 在本機 Dev Container 中執行 agent 工作階段

- 設定：`chat.agentHost.devContainer.enabled`（僅 Agents 視窗）
- 在本機資料夾的 Dev Container 內執行工作階段，agent 使用專案設定的環境與相依套件，而不是本機環境。
- 啟用後，具備支援的 Dev Container 設定的本機資料夾會自動出現資料夾選單，內含「Use Dev Container」動作；選取即可在該資料夾的 Dev Container 中執行工作階段。
- 機器上必須安裝 Docker。
- 逐步推出中，設定可能尚未預設啟用，可手動開啟試用。

### 2. Agent host 中擴充的 Codex 支援

- 設定：`chat.agentHost.codexAgent.enabled`、`chat.editor.codex.preferAgentHost`
- **選擇訂閱**：可用 GitHub Copilot 訂閱或 ChatGPT 訂閱使用 Codex。兩者都登入時，可在模型選擇器中於 Copilot 支援與 ChatGPT 支援的模型之間切換，不遺失目前對話。
- **跨應用程式接續**：同一個 Codex 工作階段可在 ChatGPT 應用程式與 VS Code 之間移動，不必重新開始對話。
- **與桌面應用程式互動**：若 ChatGPT 應用程式已安裝並設定為可進行電腦操作（computer use），VS Code 中的 Codex harness 可重用該設定與電腦上的應用程式互動；Copilot 與 ChatGPT 訂閱的模型皆適用。
- **使用 VS Code 工具**：Codex 可使用 VS Code 提供的全部工具，包含內建、擴充套件與 MCP 工具；使用 ChatGPT 支援的模型時，還可直接在工作階段中使用圖片生成工具。

### 3. 在工作區中接續 Codex quick chat

- 延伸上一版（1.137）Copilot 的「在工作區中接續 quick chat」流程到 Codex。
- 請 Codex 附加本機資料夾，可選擇直接使用該資料夾或建立獨立 worktree。
- 確認後，同一個 chat 與原生 Codex thread 成為工作區工作階段，保留標題、對話歷史、目前請求、所選模型與權限模式，並以可存取專案檔案的狀態繼續處理請求。
- 限制：只適用於 Interactive 模式下閒置的 Codex quick chat，只支援單一根目錄工作區。
- 取消或無法套用時，原本沒有工作區的 chat 仍可繼續使用。

### 4. 隨需下載 agent SDK

- Agents 視窗中的 Claude 與 Codex agent 依賴一個未隨 VS Code 提供的 SDK，會在第一次需要時下載。
- 先前下載提示只在未登入的帳號設定流程中出現，已登入者要到第一則訊息時才下載。
- 現在只要 SDK 缺失就會顯示下載提示。
- 該 agent 的模型已可用時，通知提供兩個選項：按「Download」，或直接送出訊息在該回合中下載 SDK。通知顯示期間若模型變為可用，文字會就地更新。

### 5. 從 agent 工作階段建立 pull request

- 設定：`chat.agentMerge.enabled`（選用，僅供 Agent Merge 使用）
- 在 Agents 視窗中從 Agent Host 工作階段建立 PR，用單一表單檢視並編輯產生的標題與描述、選擇草稿狀態、設定可用的合併選項。
- 可直接建立 PR，或將請求交給 agent 處理；偏好的選項會被記住供下次使用。
- 「Agent Merge」選項為實驗性，只有啟用上述設定時才可使用。

### 6. 讓已完成的工作階段保持整齊（預覽）

- 設定：`chat.agentSessions.archiveNudge.enabled`、`chat.agentSessions.autoMarkAsDoneMergedSessionsAfterDays`、`chat.agentSessions.autoDeleteArchivedMergedSessionsAfterDays`、`sessions.markAsDoneConfetti`（僅 Agents 視窗）
- 閒置工作階段的所有 PR 都合併後，Agents 視窗可建議將其標記為完成；首次使用有導覽指出「Mark as Done」在工作階段清單中的位置。需啟用 `chat.agentSessions.archiveNudge.enabled` 才會顯示建議。
- 自動清理：PR 合併後自動將閒置工作階段標記為完成，並可選擇在另一段寬限期後刪除；兩個自動清理設定預設關閉。
- 在「Mark as Done」建議中選「Configure Automatic Cleanup」可開啟這兩個設定而不直接啟用。
- 啟用 `sessions.markAsDoneConfetti` 可在標記完成時顯示彩帶動畫，動畫尊重減少動態效果的偏好。

### 7. 查看哪些工作階段需要注意（預覽）

- 設定：`sessions.showApplicationBadge`（僅 Agents 視窗）
- 在 macOS Dock、Linux 啟動器或 Windows 工作列顯示徽章，不必切回 VS Code 就能得知工作階段需要注意。
- 徽章標示的情況：有新結果、要求輸入、PR 檢查需要處理。

### 8. 統一的工作區與儲存庫選擇器（實驗性）

- 設定：`sessions.chat.unifiedWorkspacePicker.enabled`（僅 Agents 視窗）
- 從單一可搜尋清單開始 agent 工作，清單包含本機資料夾、GitHub 儲存庫、Cloud 儲存庫與遠端目標；遠端連線動作仍可從「Remote」項目使用。
- 「Work in Repository」採 cloud 優先流程：尚未在本機的 GitHub 儲存庫會立即以 Cloud harness 選取，不出現 clone 提示。
- 之後若改選本機 harness，VS Code 會提示 clone；取消則保留 cloud 選擇。

### 9. 依色彩主題自訂 chat 背景（實驗性）

- 設定：`chat.agentSessions.preferredDarkBackgroundImageLayout`、`chat.agentSessions.preferredLightBackgroundImageLayout`（僅 Agents 視窗）
- Agents 視窗可在工作階段後方顯示裝飾性背景：內建 VS Code 圖示（Codicons）圖樣或自訂圖片，深色與淺色主題分別選擇。
- 先前圖片依主題類型儲存但版面配置不是，導致深色靠右、淺色靠左最後兩者都靠左；現在版面配置隨圖片一起依主題類型儲存，切換主題時還原該主題的位置。
- 兩個新設定取代 `chat.agentSessions.backgroundImageLayout`。
- 「Chat: Set Background...」現以「No Background」作為第一個選項，只清除目前色彩主題的背景，不影響另一個主題。
- 回應與請求區塊不透明，高對比背景不影響 agent 回應的可讀性。

## 二、Chat

### 改善 Voice Mode 的工作階段感知（實驗性）

- 不必離開 Voice Mode 就能瀏覽並監控平行的 agent 工作。
- Voice Mode 可找出最近的 agent 工作階段、依標籤切換，並回報各工作階段的狀態。

## 三、棄用的功能與設定

| 項目 | 處置 | 替代方案 |
| --- | --- | --- |
| `chat.agentSessions.backgroundImageLayout` | 棄用 | `chat.agentSessions.preferredDarkBackgroundImageLayout`、`chat.agentSessions.preferredLightBackgroundImageLayout`，讓深色與淺色主題分別設定版面配置 |
| 「Chat: Clear Background」命令 | 移除 | 「Chat: Set Background...」中的「No Background」，清除目前色彩主題的背景 |

## 四、設定一覽

| 設定 | 用途 | 狀態／範圍 |
| --- | --- | --- |
| `chat.agentHost.devContainer.enabled` | 在本機 Dev Container 中執行 agent 工作階段 | 逐步推出，僅 Agents 視窗 |
| `chat.agentHost.codexAgent.enabled` | 在 agent host 中啟用 Codex agent | |
| `chat.editor.codex.preferAgentHost` | Codex 優先使用 agent host | |
| `chat.agentMerge.enabled` | 啟用實驗性的 Agent Merge 選項 | 實驗性，選用 |
| `chat.agentSessions.archiveNudge.enabled` | 顯示「Mark as Done」建議 | 預覽，僅 Agents 視窗 |
| `chat.agentSessions.autoMarkAsDoneMergedSessionsAfterDays` | PR 合併後幾天自動標記完成 | 預覽，預設關閉 |
| `chat.agentSessions.autoDeleteArchivedMergedSessionsAfterDays` | 標記完成後幾天自動刪除 | 預覽，預設關閉 |
| `sessions.markAsDoneConfetti` | 標記完成時顯示彩帶動畫 | 預覽，僅 Agents 視窗 |
| `sessions.showApplicationBadge` | 在 Dock／啟動器／工作列顯示徽章 | 預覽，僅 Agents 視窗 |
| `sessions.chat.unifiedWorkspacePicker.enabled` | 統一的工作區與儲存庫選擇器 | 實驗性，僅 Agents 視窗 |
| `chat.agentSessions.preferredDarkBackgroundImageLayout` | 深色主題的 chat 背景版面配置 | 實驗性，僅 Agents 視窗 |
| `chat.agentSessions.preferredLightBackgroundImageLayout` | 淺色主題的 chat 背景版面配置 | 實驗性，僅 Agents 視窗 |

## 五、社群貢獻

- `vscode`（9 位）：files.exclude 自動完成測試更新、已完成進度通知關閉修正、pty host 事件造成主程序 OOM 修正、Codex MCP 工具進度不進入工具結果、markers 表格記憶體洩漏修正、terminal-editor shift-drop 在已分離執行個體觸發的修正、win32 product.json 路徑建置修正、commit 訊息強制 UTF-8 輸出、OTel 文件與 Agent Host 架構對齊。
- `vscode-chat-customizations-evaluation`（1 位）：修正使用 log 輸出頻道時語言用戶端的啟動問題。
- Issue tracking（4 位）：John Murray、RedCMD、Andrii Dieiev、Alberto Santini。

## 六、時程與使用前提

**時程**

- 2026 年 8 月 26 日：Agent Host 架構部落格文章。
- 2026 年 9 月 10 日：GitHub Copilot Day 直播。
- 2026 年 9 月 16 日：VS Code 1.138 發布。

**使用前提**

- Dev Container 工作階段需本機安裝 Docker，資料夾要有支援的 Dev Container 設定。
- Codex 與桌面應用程式互動需 ChatGPT 應用程式已安裝並設定 computer use。
- Codex 圖片生成工具只在 ChatGPT 支援的模型下可用。
- Codex quick chat 轉工作區僅限 Interactive 模式、閒置狀態、單一根目錄工作區。
- Claude 與 Codex agent 首次使用需下載 SDK。
- 自動清理的兩個設定預設關閉。
- 多數新功能僅在 Agents 視窗中提供。
