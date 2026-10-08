# Visual Studio Code 1.141 更新重點

- 版本：1.141.0（Stable）
- 發行日期：2026 年 10 月 7 日
- 原文連結：https://code.visualstudio.com/updates/v1_141

本版 6 大主軸為：agent session 管理、agent 安全性、外部 session 接續、編輯器體驗、多 GitHub Enterprise 登入、企業原則管理。

## 主軸項目

**1. Worktree 儲存空間清理**（`chat.agentSessions.sessionStorageCleanupSuggestion.enabled`）
以 **Chat: Open Worktree Cleanup** 查看並刪除閒置 agent session 的 worktree，可設定已合併 PR 自動清理；空間過大時 Agents 視窗會主動建議。

**2. 跨平台沙箱**（`chat.agent.sandbox.enabled`）
在 Windows、macOS、Linux 限制 agent 對檔案與網路的存取，降低模型出錯與提示詞注入的影響；可在 **Permissions** 選單以 session 為單位切換；本機啟動的 MCP 與語言伺服器預設納入沙箱。

**3. Session 格狀排列與篩選**
Agents 視窗可將多個 session 拖曳成水平／垂直分割並排比較；**Filter Sessions** 新增依 Environment、Harness、Created In 篩選；巢狀 session 顯示未讀狀態與最後修改時間。

**4. 接續外部 Copilot 與 Codex 對話**
Copilot CLI、Copilot app 的新對話會自動以外部 session 出現；ChatGPT app 或 Codex CLI 的 Codex 聊天可在同一台機器的 VS Code 中接續，歷程完整保留，不需重新載入。

**5. 展開式區塊貼上**（`editor.multiCursorPaste`）
單一游標貼上區塊時，各列自動分配到連續的行（預設 `spread`；設 `full` 則貼完整區塊）。

**6. 登入多個 GitHub Enterprise 執行個體**（`github-enterprise.uris`）
同一個 VS Code 視窗可同時登入 GHE.com 與 GitHub Enterprise Server 帳號；GitHub Copilot、GitHub Pull Requests、GitHub Repositories 擴充功能皆支援。

## 其他更新

- **Copilot harness**：由 Copilot SDK 驅動，在專屬 agent host 程序執行，可從多個 VS Code 視窗連到同一 session，本版可能已是預設。
- **Background Shells**：Copilot harness session 中可追蹤背景執行中的 shell 並觀看輸出串流。
- **`send_message` 工具**：可引導進行中的對話、排隊／取代／取消佇列中的訊息。
- **Compact 版面密度**（`window.density.layout`）：Agents 視窗可獨立設定更緊湊的版面。
- **新 session 控制項、agent 選擇器位置、歡迎訊息自訂**（實驗性）：Session Options 托盤、Agent 選擇器可移入 Add Context 選單、`{name}` 自訂歡迎語句。
- **Add Context 選擇器**：改在觸發按鈕旁開啟。
- **從編輯器下載遠端檔案**：Agents 視窗 **More Actions > Download...**。
- **Dev Container 範例**（實驗性，`chat.agentHost.devContainer.samples.enabled`）：提供 Go、.NET、Node.js、PHP、Python、Rust 範例環境。
- **持續進度顯示**（`chat.experimental.persistentProgress`）：推送給所有使用者，可選三種圖示樣式。
- **Blobby**：`/vscode-pet` 正式命名為 Blobby，`/blobby` 可自訂。
- **MCP Servers（預覽）**：Customizations 編輯器可看到外掛、擴充功能、內建整合提供的 MCP 伺服器。
- **毛玻璃 UI**（`workbench.modernUIFrostedGlass`／`Opacity`）與**分頁樣式**（實驗性，`connected`／`pill`）。
- **企業**：舊版原則執行階段轉譯為統一受管理設定；受管理設定可強制沙箱（`allowBypass: false`，預覽）；遙測可擷取 OS 使用者名稱與主機名稱（預設關閉）；原則檢查期間保留所選 agent。
- **GitHub Pull Requests 擴充功能**：堆疊式 PR（`githubPullRequests.experimental.stacks`）、多 GitHub Enterprise 執行個體支援。
- **棄用**：`github-enterprise.uri`（改用 `github-enterprise.uris`）；舊版 VS Code 沙箱設定與裝置原則（`ChatAgentSandboxEnabled` 改為可覆寫預設值）。
- **社群貢獻**：@SimonSiefke 提交大量記憶體洩漏修正。

**總結**：1.141 以 agent session 的管理（清理、格狀排列、外部接續）與安全（跨平台沙箱、企業受管理設定）為核心，並補上多 GitHub Enterprise 登入與區塊貼上等實用改進。

資料來源：https://code.visualstudio.com/updates/v1_141
