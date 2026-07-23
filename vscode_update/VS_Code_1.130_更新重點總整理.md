# VS Code 1.130 更新重點

**版本：** 1.130｜**發行日期：** 2026 年 7 月 22 日
**原文：** https://code.visualstudio.com/updates/v1_130

---

本版圍繞 **agent host** 架構持續打磨，重點如下：

- **agent host**：專用程序（依 AHP 執行 Copilot／Claude／Codex），同一工作階段可從多個視窗連線。以 `chat.agentHost.enabled` 啟用。
- **Agents 視窗審閱改善（Preview）**：檔案層級增刪統計、緊湊多檔差異檢視、單行緊湊快速聊天，以及 worktree 隔離擴展到所有工具鏈（Claude、Codex 也能並行）。
- **輔助工具核准**（`chat.assistedPermissions.enabled`）：由模型評估工具呼叫風險，減少核准中斷。
- **可見性與終端機**：聊天時間戳記（`chat.verbose`）；Business／Enterprise 未設預算也能看當期彙總點數用量；終端機辨識 Git 助記前綴（`i/`、`w/`、`1/`、`2/`）使 diff 檔案連結可點擊。
- **工程**：改用 TypeScript 7 正式版編譯。

**總結**：無全新大功能，主要讓 agent host 更成熟——多視窗協作、多檔審閱、並行 worktree 與核准流程更省事的一次整合更新。

---

*資料來源：[Visual Studio Code 1.130 發行說明](https://code.visualstudio.com/updates/v1_130)*
