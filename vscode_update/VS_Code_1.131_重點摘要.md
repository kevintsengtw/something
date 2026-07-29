# Visual Studio Code 1.131 版本重點摘要

**版本：** 1.131
**發行日期：** 2026 年 7 月 29 日
**原文：** https://code.visualstudio.com/updates/v1_131

---

本次發行帶來對執行中子代理更多的可見性、跨工作台的內建聽寫功能，以及全新的混合式 Markdown 編輯器。以下為官方列出的三大亮點：

## 一、子代理

- 無需開啟子代理的對話，即可查看執行中子代理的模型、經過時間和使用中的工具呼叫
- 在 Agents 視窗中，主對話會為每個執行中的子代理顯示：
  - 子代理使用的模型
  - 子代理已執行多久
  - 子代理正在主動呼叫的工具
- 選取執行中的子代理可在另一個聊天中開啟其對話，在父對話仍保持可用的情況下審閱其完整進度

## 二、跨 VS Code 的內建聽寫（實驗性）

- **設定**：`dictation.enabled`、`dictation.showTranscript`、`dictation.experimental.llmCleanup`
- 使用 VS Code 中的聽寫功能不再需要安裝 VS Code Speech 擴充功能
- 內建轉錄服務可在聊天輸入、文字編輯器和整合式終端機中運作，各介面有適合的即時文字和控制項
- 三個介面共用單一語音工作階段和麥克風選取，避免重疊錄音
- 使用私密的離線 Nemotron 模型，首次使用時下載模型，音訊保留在您的裝置上
- `dictation.experimental.llmCleanup` 啟用後，Copilot 會在您說話時即時優化逐字稿（新增格式、移除填充詞）
- **支援平台**：Windows x64 和 Arm64、Apple silicon 上的 macOS、glibc 2.34 以上的 Linux x64 和 Arm64、遠端工作區
- **不支援平台**：VS Code for the Web、Intel 架構的 Mac、32 位元系統和 Arm32 系統

## 三、混合式 Markdown 編輯器（實驗性）

- **設定**：`workbench.editor.markdownDefaultEditorInAgentsWindow`
- Agents 視窗中的全新混合式 Markdown 編輯器，可檢視 Markdown 檔案、就地編輯，並新增 Agent 可據以行動的留言
- 可透過 **Reopen Editor With** 在文字編輯器和新的 Markdown 編輯器之間切換（Agents 視窗和編輯器視窗皆可）

---

## 其他

- **Agent host**：持續開發與漸進推出，依 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）執行 Copilot、Claude、Codex 等工具鏈，同一工作階段可從多個 VS Code 視窗連線與渲染；以 `chat.agentHost.enabled` 啟用
- **VS Code 寵物（實驗性）**：在聊天中輸入 `/vscode-pet` 認識您的新夥伴
- **終端機螢幕閱讀器更新控制**：`terminal.integrated.accessibleViewPreserveCursorPosition` 設為 `always` 可在新內容到達時保留可存取檢視中的游標位置；終端機即時更新改用非中斷式 ARIA 狀態公告而非強制警示
- **終端機調整大小尺寸疊加層控制**：可透過 `terminal.integrated.resizeDimensionsOverlay.enabled` 停用調整終端機大小時顯示的欄×列疊加層，預設仍為啟用，變更立即套用無需重啟
- **Python**：Python Environments 推出已達 100% 使用者，成為 Stable 和 Insiders 的預設環境管理體驗；專案啟動更快，Conda 探索延後至需要時、合併並行環境掃描、Pylance 可在完整重新整理期間使用最後已知的直譯器

---

*資料來源：[Visual Studio Code 1.131 發行說明](https://code.visualstudio.com/updates/v1_131)*
