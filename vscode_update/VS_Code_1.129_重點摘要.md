# Visual Studio Code 1.129 版本重點摘要

**版本：** 1.129
**發行日期：** 2026 年 7 月 15 日
**原文：** https://code.visualstudio.com/updates/v1_129

---

本次發行帶來專用的 agent host、Agents 視窗中的全新編輯器面板、使用 `!` 執行命令，以及現代化 UI 的預覽。以下為官方列出的四大亮點：

## 一、Agent Host

- 重新架構 Agent 工作階段的運作方式，圍繞 agent host 建構——一個專用程序，根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）執行 Copilot、Claude 和 Codex 等 Agent 工具鏈
- 因為工作階段存在於獨立程序中，同一工作階段可同時從多個 VS Code 視窗連線和渲染
- Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式及其他 Copilot 產品一致
- 啟用方式：設定 `chat.agentHost.enabled`，然後從工具鏈下拉選單選取 agent host 工具鏈

## 二、Agents 視窗中的新編輯器面板（實驗性）

- 重新設計的編輯器面板，將編輯器和詳細資料區域合併為一個固定窗格，共用分頁列
- 可在固定編輯器中直接開啟檔案和差異、透過 **New Tab** 操作新增分頁
- **Changes** 檢視改善：切換行內和並排檢視、一次展開或摺疊所有檔案、更緊湊的差異呈現
- 每個工作階段還原側邊窗格寬度、開啟的編輯器、活動編輯器和每檔摺疊狀態
- 啟用方式：設定 `sessions.layout.singlePaneDetailPanel` 並重新載入視窗

## 三、使用 `!` 前綴執行命令

- 在聊天訊息前加上 `!` 即可將其內容作為終端機命令執行
- 可在編輯器和 Agents 視窗中的 agent host 工作階段使用

## 四、現代化 UI 預覽（實驗性）

- **設定**：`workbench.experimental.modernUI`
- 可預覽更新外觀的 VS Code 工作台 UI
- 在 Insiders 組建中預設啟用

---

## 其他

- **工作階段管理工具**：在 agent host 上執行的 Agent 可列舉、建立、觀察和操作其他工作階段及聊天，包括讀取其他工作階段的對話、建立新工作階段/聊天、向工作階段發送訊息（需確認）
- **Agents 視窗改善**：新工作階段選擇器記住上次的 Agent 模式和核准選擇；以 **New Worktree** 核取方塊取代下拉選單，簡化 worktree 隔離選擇
- **BYOK 模型搭配 Copilot Agent 工具鏈**：在 Agents 視窗中選取 agent host 上的 Copilot 工具鏈時可使用 BYOK 模型
- **提示檔案遷移至技能（實驗性）**：啟用 `chat.customizations.promptMigration.enabled` 後，可將 `*.prompt.md` 檔案遷移為技能，以跨工具鏈相容
- **從編輯器工具列重新開啟編輯器**：當檔案或差異支援多個編輯器時，可從編輯器工具列的 **Reopen Editor With** 子選單直接切換
- **GitHub Enterprise 支援 agent host 中的 Copilot**：透過 GitHub Enterprise 實例提供 Copilot 存取的開發者現在可以登入並使用 Copilot
- **提議的 API：自訂編輯器的差異和合併編輯器設定**：自訂編輯器現在預設退出差異和合併編輯器，並提供 `customEditorPriority` API 為檔案、差異和合併編輯器分別設定優先順序

---

*資料來源：[Visual Studio Code 1.129 發行說明](https://code.visualstudio.com/updates/v1_129)*
