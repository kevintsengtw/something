# VS Code 1.140 更新重點

- 版本：1.140.0（Stable）
- 發行日期：2026 年 9 月 30 日
- 原文連結：<https://code.visualstudio.com/updates/v1_140>

本版 6 大主軸為 Copilot harness、多資料夾工作階段、遠端委派、HydraFusion、共用 worktree 資料夾與企業控管。

1. **Copilot harness**：由 Copilot SDK 驅動，行為與 GitHub Copilot 應用程式、Copilot CLI 一致；在專屬的 agent host 處理程序中執行，可從多個視窗連線到同一工作階段。
2. **多資料夾工作階段（實驗性）**（`chat.agentHost.<copilotAgent|claudeAgent|codexAgent>.multiRootEnabled`）：同一工作階段中的每個聊天可使用自己的資料夾或 worktree，變更互不滲漏。
3. **遠端委派（實驗性）**（`chat.remoteAgentHostsEnabled`、`chat.remoteSessions.tools.enabled`）：代理程式透過新的內建工具探索遠端主機、自動配置並啟動工作階段，再回報結果。
4. **HydraFusion（研究預覽）**：自適應模型協調，可用單一模型、升級更強模型，或讓另一模型評論並修改結果。
5. **共用 worktree 資料夾（實驗性）**（`git.worktreeSymlinkFolders`）：以符號連結重複使用 `node_modules` 等被忽略的資料夾，免去重複安裝。
6. **企業控管**（`autoTier`、`github.copilot.chat.otel.captureIdentity`）：在 AI 功能處說明最低版本需求、設定 Auto 模型預設層級，並可在 OpenTelemetry 中擷取使用者身分。

## 其他更新

- Agents 視窗：可單獨封存工作階段中的聊天；Dev Container 閒置自動停止且啟動更快；遠端 agent host 支援無工作區聊天；撰寫區控制項集中、分支預設為上游分支；Done 依完成時間排序；提高協調上限；可用 `vscode://agents/new` URL 開啟新工作階段；可加入個人化歡迎語。
- 聊天：代理程式回應的持續進度顯示（實驗性）；新增 `chat.tools.terminal.outputReflow` 控制終端機輸出重排；編輯器視窗的 Sessions 檢視可使用多個聊天。
- MCP：**MCP: Add Server** 可將伺服器存入可攜式設定檔（`~/.copilot/mcp-config.json`、工作區 `.mcp.json`）。
- 協助工具：新增 Confetti 音效訊號。
- 程式碼編輯：`editor.selectedTextMatchMode` 控制選取比對的大小寫敏感度。
- 終端機：macOS 可將 `terminal.integrated.fontRendering` 設為 `grayscale` 讓文字更清晰（實驗性）。
- 提議 API：`AuthenticationSession` 新增 `authorizationServer`，標示簽發工作階段的授權伺服器。
- 社群：VS Code 開設 Instagram（@vscode.ig）。
- 已淘汰的功能與設定：無。

**總結**：1.140 以 Copilot harness 與多資料夾、遠端委派等代理程式協調能力為核心，並補上 worktree 共用與企業端的版本、模型層級及遙測控管。

資料來源：<https://code.visualstudio.com/updates/v1_140>
