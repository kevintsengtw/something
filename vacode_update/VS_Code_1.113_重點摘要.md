# Visual Studio Code 1.113 版本重點摘要

**版本：** 1.113（2026 年 3 月 25 日發行）
**類型：** 每週穩定版（Weekly Stable Release）

---

## 一、聊天自訂項目編輯器（Chat Customizations Editor）

- 全新統一介面，可從單一位置管理所有聊天相關自訂項目
- 透過聊天檢視中的「設定聊天」齒輪圖示，或從命令面板執行「Chat: Open Chat Customizations」開啟
- 編輯器將自訂類型組織成獨立分頁：自訂指令、提示檔案、自訂 Agent、Agent 技能
- 內嵌程式碼編輯器，支援語法醒目提示與驗證
- 可從頭建立自訂項目，或使用 AI 根據專案產生初始內容
- 可直接從編輯器瀏覽 MCP 伺服器和 Agent 外掛程式市集

## 二、可設定的思考力度（Configurable Thinking Effort）

- 支援推理的模型（如 Claude Sonnet 4.6、GPT-5.4）在模型選擇器中新增「Thinking Effort」子選單
- 可直接從 UI 控制模型的推理層級，無需進入 VS Code 設定
- 模型選擇器標籤現在也顯示所選力度等級（例如「GPT-5.3-Codex · Medium」）
- VS Code 會記住每個模型的所選力度等級，跨對話保留
- 非推理模型不會顯示此子選單
- **已棄用設定：** `github.copilot.chat.anthropic.thinking.effort` 和 `github.copilot.chat.responsesApiReasoningEffort` 已棄用，改由模型選擇器直接設定

## 三、巢狀子代理（Nested Subagents）

- 子代理現可呼叫其他子代理，支援複雜的多步驟工作流程
- 新設定 `chat.subagents.allowInvocationsFromSubagents` 啟用此功能
- 先前為防止無限遞迴，子代理被限制不能呼叫其他子代理
- 支援協調者模式：協調者 Agent 將工作委派給專門的工作者 Agent
- 可透過 `disable-model-invocation` 屬性控制 Agent 是否可被其他 Agent 呼叫

## 四、CLI Agent 功能增強（CLI Agent Capabilities）

- Copilot CLI 和 Claude Agent 現支援 MCP 伺服器
- MCP 伺服器直接橋接至 CLI，無需在編輯器與命令列間重新設定工具
- 支援分叉工作階段（Fork Sessions）：建立現有對話的副本，探索不同方向
- 可在 CLI Agent 中檢視除錯日誌（Debug Logs）

## 五、圖片預覽（Images Preview）

- 聊天中的圖片附件可在全功能圖片檢視器中開啟
- 支援縮放與平移：點擊放大、Alt+Click/Ctrl+Click 縮小、滾輪/捏合連續縮放
- 圖片依對話回合分組，方便識別來源
- 可透過底部縮圖條或方向鍵在圖片間導覽
- 檔案總管檢視中的圖片檔案也可透過右鍵選單「在圖片預覽中開啟」
- 相關設定：`imageCarousel.chat.enabled` 和 `imageCarousel.explorerContextMenu.enabled`

## 六、預設主題更新（Default Themes Refresh）

- 全新預設主題：「VS Code Light」和「VS Code Dark」
- 保持先前「Modern」主題的熟悉度和可用性，同時提供更現代的外觀
- 新使用者的 OS 主題同步將預設使用新主題，自動匹配作業系統的亮色/暗色模式

## 七、釘選聊天工作階段（Pinned Chat Sessions）

- 可釘選聊天工作階段，讓重要對話保持可存取，無需捲動歷史記錄

## 八、可點擊的斜線命令（Clickable Slash Commands）

- 斜線命令（如 `/fix` 或 `/explain`）現在可點擊
- 點擊後可在發送前檢視或修改其參數

## 九、整合式瀏覽器改善（Integrated Browser Improvements）

- 可暫時信任無法驗證的自簽憑證，解除使用自簽憑證時的開發阻礙
- 瀏覽器分頁右鍵選單新增關閉同一群組中所有瀏覽器分頁的選項
- 可透過命令面板關閉所有群組的瀏覽器分頁

## 十、終端機改善（Terminal Improvements）

- `run_in_terminal` 工具在命令逾時時，現在會明確標示輸出為已截斷，而非靜默返回部分結果

## 十一、無障礙功能改善（Accessibility）

- 改善隱含內容的螢幕閱讀器標籤

## 十二、重要修正（Notable Fixes）

- 修正連線至 WSL 時「在檔案總管中顯示」（Reveal in File Explorer）的問題
- 圖片輪播檢視器的縮放支援

---

*資料來源：VS Code 1.113 發行說明 (https://code.visualstudio.com/updates/v1_113)*
