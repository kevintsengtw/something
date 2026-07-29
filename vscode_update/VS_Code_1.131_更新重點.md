# VS Code 1.131 更新重點

**版本：** 1.131｜**發行日期：** 2026 年 7 月 29 日
**原文：** https://code.visualstudio.com/updates/v1_131

---

本版三大主軸為子代理可見性、內建聽寫與混合式 Markdown 編輯器：

- **子代理資訊（Agents 視窗）**：主對話直接顯示每個執行中子代理的模型、已執行時間與正在呼叫的工具，無需開啟對話。點選可在另一個聊天中查看完整進度，父對話仍保持可用。
- **內建聽寫（實驗性）**：不再需要 Speech 擴充功能，聊天輸入、文字編輯器、整合式終端機皆可用，三者共用單一語音工作階段與麥克風。採用離線的 Nemotron 模型，音訊留在裝置上；`dictation.experimental.llmCleanup` 可讓 Copilot 即時加上格式並移除填充詞。支援 Windows x64/Arm64、Apple silicon Mac、glibc 2.34+ 的 Linux 與遠端工作區；不支援 Web 版、Intel Mac、32 位元與 Arm32。
- **混合式 Markdown 編輯器（實驗性）**：Agents 視窗中可檢視、就地編輯 Markdown，並新增 Agent 可據以行動的留言，透過 **Reopen Editor With** 與文字編輯器切換（`workbench.editor.markdownDefaultEditorInAgentsWindow`）。

其他更新：

- **Agent host**：持續漸進推出，依 AHP 執行 Copilot／Claude／Codex，同一工作階段可從多個視窗連線（`chat.agentHost.enabled`）。
- **協助工具**：`terminal.integrated.accessibleViewPreserveCursorPosition` 設為 `always` 可在新輸出到達時保留游標位置；即時更新改用非中斷式 ARIA 狀態公告。
- **終端機**：可用 `terminal.integrated.resizeDimensionsOverlay.enabled` 關閉調整大小時的欄×列疊加層。
- **Python**：Python Environments 推出達 100%，成為預設環境管理體驗；延後 Conda 探索、合併並行掃描、Pylance 可沿用最後已知直譯器，啟動更快。
- **VS Code 寵物（實驗性）**：聊天中輸入 `/vscode-pet`。

**總結**：本版重心在讓 Agent 工作更透明（子代理狀態一目了然）、輸入方式更多元（內建離線聽寫），並補上 Markdown 在 Agents 視窗中的編輯與協作缺口。

---

*資料來源：[Visual Studio Code 1.131 發行說明](https://code.visualstudio.com/updates/v1_131)*
