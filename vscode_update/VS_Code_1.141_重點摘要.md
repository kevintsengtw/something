# Visual Studio Code 1.141 重點摘要

- 版本：1.141.0（Stable）
- 發行日期：2026 年 10 月 7 日
- 原文連結：https://code.visualstudio.com/updates/v1_141

本版的核心方向：更容易管理 agent session、保護 agent 工作流程、編輯欄狀文字，以及跨多個 GitHub Enterprise 執行個體工作。官方列出的六大亮點為：worktree 儲存空間清理、跨平台沙箱、session 格狀排列、接續外部 session、區塊貼上、多個 GitHub Enterprise 執行個體登入。

---

## 一、Agent 迴圈（Agent loop）

### 1. Copilot harness
- 由 Copilot SDK 驅動，行為與能力與獨立的 GitHub Copilot app、Copilot CLI 一致。
- 在聊天輸入框的 harness 選擇器中選取即可使用；本版對部分使用者可能已是預設選項。
- 在專屬的 agent host 程序中執行，基於 Agent Host Protocol（AHP），因此可從多個 VS Code 視窗連線到同一個 agent session。
- 相關文件：使用 Copilot harness、agent host 架構部落格；意見請提交 issue。

### 2. 回收閒置 worktree 的儲存空間
- 命令：**Chat: Open Worktree Cleanup**。可查看閒置 agent session worktree 佔用的磁碟空間、依閒置時間篩選、檢視各 worktree 大小並勾選清理。
- 清理編輯器可設定「已合併 PR 的 session 自動清理」，以及是否在 session 儲存空間變大時由 VS Code 建議清理。
- 設定 `chat.agentSessions.sessionStorageCleanupSuggestion.enabled` 啟用後，閒置 worktree 數量或磁碟佔用達到門檻時，Agents 視窗會通知。
- 清理會將 session 標記為 done 並刪除 worktree；之後可還原 session 重建 worktree。
- 保護範圍：active、running、needs-input、pinned 狀態的 session 不會被清理。

### 3. 追蹤背景 shell
- Agent 在背景執行命令時可繼續處理其他步驟。
- Copilot harness session 中，聊天輸入框上方出現 **Background Shells** 標籤，列出仍在執行的 shell（Agents 視窗與編輯器視窗皆有）。
- 點選可看每個 shell 的命令與已執行時間；選取某 shell 可觀看輸出串流（若有輸出）。命令完成後 shell 自動從清單移除。

### 4. 對 agent 觸發的訊息有更多控制
- Agent 跨對話委派工作時，可在需求變更時調整任務，不必等另一個 session 結束。
- `send_message` 工具支援（同一 agent host 上的 session 與巢狀 session 之間）：
  - 以修正指令引導（steer）進行中的對話
  - 將後續訊息排入佇列作為獨立一輪
  - 取代佇列中的訊息並保留其位置
  - 在處理開始前取消佇列中的訊息
- 佇列訊息依序執行；一旦開始處理即無法取代或取消。

---

## 二、Agent UX

### 1. 以格狀排列工作階段
- Agents 視窗提供二維 session 格狀版面，可拖曳成水平／垂直分割並調整窗格大小，與編輯器視窗相同。
- 可將單一 session 最大化專注，再回到格狀版面。
- 用途：比較結果、監看長時間任務、跨相關對話工作。

### 2. 精簡版面密度（Compact layout density）
- 設定：`window.density.layout`。
- Compact 密度：較矮的標題列、移除工作台各部分間隙、縮減窗格間距。
- Agents 視窗預設跟隨使用者層級設定（同時影響編輯器視窗）；若只想改 Agents 視窗，於該視窗選 **View > Layout Density**，編輯器視窗密度不變。

### 3. 更多 session 篩選選項
- **Filter Sessions** 可獨立依以下條件篩選：
  - **Environment**：本機、雲端或遠端主機
  - **Harness**：所使用的 agent harness
  - **Created In**：建立來源應用程式（VS Code、Copilot CLI、Copilot app）
- 選單整合排序、分組、**Show Done**、**Created Externally** 控制項。

### 4. 追蹤巢狀 session 的活動
- Agents 視窗中每個巢狀 session 顯示未讀狀態與最後修改時間。
- 狀態逐對話獨立追蹤：開啟巢狀 session 只標記該對話為已讀；巢狀未讀不會讓主 session 顯示未讀；最後修改時間只反映該對話。
- 已讀狀態與時間在重新載入／重啟後保留。
- 在主 session 右鍵 **Mark as Read** 可清除整個 session 樹的未讀狀態，不影響無關 session。

### 5. 簡化的新 session 控制項（實驗性）
- 設定：`sessions.chat.experimental.newSessionComposerLayout`、`sessions.chat.unifiedWorkspacePicker.enabled`（僅限 Agents 視窗）。
- 工作區、儲存庫、harness 控制項移入聊天輸入框上方可收合的 **Session Options** 托盤。
- 入門提示改顯示於輸入框下方的圓角通知，不再隔開控制項與提示詞。
- 收合托盤不改變選擇；會記住選擇、視窗變窄時調整標籤；螢幕閱讀器最佳化模式下保持展開；自訂聊天背景時托盤使用不透明表面。

### 6. 改善 context 選擇器的定位
- **Add Context** 選擇器在觸發按鈕旁開啟，再按一次關閉；編輯器與 Agents 視窗皆適用。

### 7. 將 agent 選擇器移入 context 控制項（實驗性）
- 設定：`sessions.chat.experimental.agentsPickerInAttachContextMenu`（僅限 Agents 視窗）。
- 啟用後 **Agent** 選擇器一開始隱藏、移入 **Add Context** 選單；選取 agent 後回到聊天輸入框。

### 8. 自訂新 session 的歡迎訊息（實驗性）
- 設定：`sessions.chat.experimental.welcomePhrases`、`sessions.chat.experimental.welcomeMessages`（僅限 Agents 視窗）。
- 可在預設語句外新增自訂語句，或取代預設；`{name}` 預留位置可放在語句任何位置。
- 標題旁 **Customize Welcome Message** 可改名稱或開啟設定；標題右鍵 **Hide Welcome Message** 可關閉。

---

## 三、Agent 環境（Agent environments）

### 1. 不需重新載入即可接續本機的外部 Copilot 工作階段
- Copilot CLI 與 GitHub Copilot app 的新對話，在送出第一個請求時即自動被納入為外部 session，即使 Agents 視窗已開啟。
- 可直接從 Agents 視窗開啟並接續；以 **Created In** 與 **Created Externally** 篩選器控制顯示。

### 2. Codex 聊天在應用程式之間交接
- 可接手 ChatGPT app 或 Codex CLI 中開始的 Codex 聊天，在同一台機器的 VS Code Agents 視窗繼續，歷程完整保留；也可切回原應用程式繼續。
- 在模型選擇器中選擇 Copilot 模型即可使用 GitHub Copilot 訂閱（例如 ChatGPT 達使用上限後改用 Copilot）。
- 前提：啟用 Codex 並顯示 **Created Externally** session；ChatGPT／Codex CLI 中建立或更新的聊天會在幾秒內出現，不需重新載入。
- 限制：同一時間只有一個應用程式能對該聊天傳送訊息。若另一個應用程式仍開著，VS Code 顯示 **This chat is open in another app** 橫幅；需完全結束 ChatGPT 或退出 Codex CLI session 後按 **Retry**。草稿與附件保留，**Retry** 只檢查存取權、不送出訊息。

### 3. 從編輯器下載遠端檔案
- Agents 視窗中，編輯器的 **More Actions** 選單 → **Download...**，可將開啟中的遠端檔案存到本機，不必到 Files 面板尋找。

### 4. Dev Container 範例（實驗性）
- 設定：`chat.agentHost.devContainer.samples.enabled`（僅限 Agents 視窗）。
- 提供 Go、.NET、Node.js、PHP、Python、Rust 的 Dev Container 範例，不需先 clone 儲存庫或安裝語言工具即可試用 agent。
- 步驟：安裝並啟動 Docker → 啟用設定 → 在桌面版 Agents 視窗開新 session → 工作區選擇器選 **Dev Container Sample** → 送出提示詞。
- 另需啟用 `chat.agentHost.devContainer.enabled` 與 `chat.remoteAgentHostsEnabled`。

---

## 四、Agent 安全性（Agent security）

### Copilot agent host 的沙箱功能
- 設定：`chat.agent.sandbox.enabled`。
- 限制受支援的 agent 操作對檔案與網路資源的存取，降低模型出錯、提示詞注入、不受信任相依套件、本機啟動工具伺服器的影響。
- 依操作與平台使用作業系統保護或 agent host 程序內檢查；是額外一層保護，不取代端點安全，也不是獨立安全邊界。
- 支援 Windows、macOS、Linux；Windows 需安裝所需的作業系統更新（見「檢查平台可用性」文件）。
- 可全域啟用設定，或在 **Permissions** 選單用 **Sandboxing for terminal** 切換於 session 層級控制。
- 可透過對應設定進一步自訂檔案系統權限與網路存取。
- 本機 session 於本機強制執行；遠端 session 於執行工具與命令的遠端主機強制執行；檔案系統權限指執行 agent 那台機器上的路徑。
- 沙箱啟用時，本機啟動的 MCP 與語言伺服器預設納入沙箱。

---

## 五、聊天體驗（Chat experience）

### 1. 聊天中的持續進度顯示
- 設定：`chat.experimental.persistentProgress`、`chat.experimental.persistentProgressVerbosity`。
- 正逐步推送給所有使用者；讓處理中的回應保持可見，並可控制進度細節量。
- `chat.experimental.persistentProgress` 可從三種圖示樣式中選擇（含單色圖示或無圖示）。

### 2. 認識 Blobby
- `/vscode-pet` 正式命名為 Blobby；在編輯器或 Agents 視窗任何聊天輸入 `/vscode-pet` 即可認識。
- 輸入 `/blobby` 可自訂；探索聊天功能並解鎖成就後會開放更多自訂選項。

---

## 六、MCP

### 在 Customizations 編輯器中查看探索到的 MCP 伺服器（預覽）
- Agent Customizations 編輯器的 **MCP Servers** 區段，會將外掛程式、擴充功能、內建整合提供的 MCP 伺服器與已設定的伺服器並列顯示。
- 工作區 `mcp.json` 以外探索到的伺服器可直接在 UI 看到，不必再用 **MCP: List Servers**。
- 操作：命令面板執行 **Chat: Open Customizations** → 選 **MCP Servers**。

---

## 七、編輯器體驗（Editor experience）

### 1. 毛玻璃覆疊層與選單
- 設定：`workbench.modernUIFrostedGlass`、`workbench.modernUIFrostedGlassOpacity`。
- 桌面版 Agents 視窗的選單、命令面板與其他彈出視窗使用模糊毛玻璃背景；編輯器視窗需啟用 `workbench.experimental.modernUI` 才有。
- `workbench.modernUIFrostedGlassOpacity`：值越低越透明，越高越實心。
- 遵循 VS Code 與作業系統的「降低透明度」偏好；高對比主題或不支援時改用實心背景。
- 若有顯示或效能問題，可停用 `workbench.modernUIFrostedGlass`。

### 2. 選擇你的分頁樣式（實驗性）
- 設定：`workbench.experimental.modernUIEditorTabStyle`（需啟用 `workbench.experimental.modernUI`）。
- `connected`：作用中分頁在視覺上與內容連接；`pill`：分頁顯示為獨立圓角膠囊。

### 3. 展開式區塊貼上
- 設定：`editor.multiCursorPaste`。
- 在單一游標貼上已複製區塊時，各列會分配到連續的目的行；`spread`（預設）啟用此行為，`full` 則貼上完整區塊。

---

## 八、驗證（Authentication）

### 登入多個 GitHub Enterprise 執行個體
- 設定：`github-enterprise.uris`（清單）。
- 背景：許多組織以 GHE.com 帳號用 Copilot、程式碼放在 GHES；舊的 `github-enterprise.uri` 只能存一個執行個體，無法同時登入兩者。
- 範例：
  ```json
  "github-enterprise.uris": [
    "https://octocat.ghe.com",
    "https://github.contoso.com"
  ]
  ```
- 驗證提供者保存清單中每個執行個體的帳號，以主機標示（如 `monalisa - octocat.ghe.com`）；設定多個時登入會詢問用哪一個；項目順序不決定預設。
- 檢查／變更擴充功能使用的帳號：Accounts 選單 **Manage Extension Account Preferences...** 或命令面板同名命令。
- 支援多執行個體的擴充功能：
  - **GitHub Copilot**：搭配 GHE.com 執行個體；**Continue with GHE.com** 登入時可輸入執行個體名稱（如 `octocat`）或完整 URL，VS Code 會加入清單而不移除既有項目，並提議修正無效 URL。
  - **GitHub Pull Requests**：開啟未設定的企業執行個體儲存庫時提議加入主機；一次只用一個 GitHub Enterprise 帳號，其他執行個體的儲存庫需切換帳號後才可用；同時使用多個執行個體的需求追蹤於 microsoft/vscode-pull-request-github#9004。
  - **GitHub Repositories**：可並排開啟多個 GHE.com 執行個體的儲存庫；不支援 GHES。
- 遷移：已使用 `github-enterprise.uri` 者維持登入，首次啟動本版時 VS Code 會搬移儲存的認證資訊。`github-enterprise.uri` 已棄用；兩者都設定時以 `github-enterprise.uris` 優先；空清單 `[]` 會關閉 GitHub Enterprise 登入。

---

## 九、企業（Enterprise）

### 1. 將舊版 VS Code 原則遷移至 Agent Host 受管理設定
- Copilot harness 正逐步成為已啟用 VS Code 實驗功能之企業使用者的預設。
- 過渡期間 VS Code 會在可能時於執行階段將受支援的舊版原則轉譯為統一受管理設定；僅適用同機器 session，不變更原則設定。
- 企業仍應在適用時遷移至統一受管理設定，以跨 VS Code、Copilot app、Copilot CLI 一致管理原則。

### 2. 使用受管理設定強制要求沙箱（預覽）
- 統一受管理設定：
  ```json
  { "sandbox": { "enabled": true, "allowBypass": false } }
  ```
- 舊版 VS Code 沙箱設定與裝置原則已棄用；`ChatAgentSandboxEnabled` 在 Agent Host 中只提供可覆寫的預設值，不再是強制要求；本機行為不變。

### 3. 將 Agent Host session 歸屬到使用者與機器
- 在 Agent Host 遙測中加入作業系統使用者名稱與主機名稱；於統一受管理設定中加入：
  ```json
  { "telemetry": { "enabled": true, "capture": { "identity": true } } }
  ```
- 擷取屬性示例：`user.name`（已登入 GitHub 使用者名稱，隨附 Copilot runtime 支援時出現在執行階段 span）、`process.user.name`（OS 使用者名稱）、`host.name`（主機名稱）；後兩者為資源屬性。
- 身分擷取預設關閉；受管理值覆寫個人偏好；與內容擷取獨立；VS Code 主機端控制不管轄 runtime 的直接匯出。

### 4. 在原則檢查期間保留你選取的 agent
- 企業以 `forceRemoteSettingsRefresh` 要求新的原則檢查時，聊天保留所選 agent，不再切到 **Ask** 或 **Edit**。
- 例行背景重新整理在載入期間維持最後接受的原則；啟動、帳號變更、重新整理失敗時仍阻擋送出直到檢查成功；新接受的限制持續套用。

---

## 十、對擴充功能的貢獻

### GitHub Pull Requests
- `githubPullRequests.experimental.stacks` 設為 `true` 可建立、檢視與合併堆疊式 pull request。
- 支援設定多個 GitHub Enterprise 執行個體並在其間選擇。
- 詳見 0.168.0 版變更記錄。

---

## 十一、已棄用的功能與設定

- **`github-enterprise.uri`**：改由 `github-enterprise.uris` 取代；`uris` 未設定時仍會讀取 `uri`。
- **舊版 VS Code 沙箱設定與裝置原則**：已棄用；`ChatAgentSandboxEnabled` 在 Agent Host 中只提供可覆寫的預設值；本機行為不變。

---

## 十二、致謝

- 社群貢獻者包含 @dzamoshchin、@jimmylewis、@na2co3-ftw、@rohit489、@shoemoney、@SimonSiefke、@soreavis、@yuezhang030。
- 其中 @SimonSiefke 貢獻了大量記憶體洩漏修正（終端機、語意權杖、搜尋編輯器、chat、notebook、整合式瀏覽器、擴充功能主機等多處）。
- Issue 追蹤貢獻者：@gjsjohnmurray、@RedCMD、@IllusionMH、@albertosantini。

---

資料來源：https://code.visualstudio.com/updates/v1_141
