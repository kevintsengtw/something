# VS Code 1.133 更新重點

**版本：** 1.133｜**發行日期：** 2026 年 8 月 12 日
**原文：** https://code.visualstudio.com/updates/v1_133

---

本版三大主軸為 Claude 工作階段的模型彈性、免 GitHub 登入，以及 HTML 自動重新載入：

- **Claude 工作階段可混用 Anthropic 與 Copilot 模型**：模型選擇器同時列出 **Anthropic** 與 **Copilot** 兩個群組，可在回合之間直接切換供應商，選取的模型套用於下一回合，無需重新設定 agent host。Anthropic 群組向你的 API 金鑰計費，Copilot 群組走 Copilot 訂閱。
- **免 GitHub 登入開啟 Agents 視窗（實驗性）**：啟用 `chat.agentHost.allowSignedOutWhenUsable` 後，Agents 視窗不再被無法關閉的 GitHub 登入提示擋住，GitHub 驗證改為綁在個別 Agent 或模型上。本版僅支援 Claude，Copilot（自有模型金鑰）與 Codex 待未來版本。
- **整合式瀏覽器自動重新載入 HTML**：本機 HTML 檔案在磁碟上變更時自動重新整理，可立即看到 Agent 編輯或自己儲存的結果。可逐分頁切換，預設值由 `workbench.browser.autoReloadOnFileChange` 控制。

其他更新：

- **Agent host**：可從多個 VS Code 視窗連線同一工作階段，依 AHP 在專用程序執行 Agent 工具鏈，Copilot Agent 由 Copilot SDK 驅動。
- **提示黏性捲動**（`chat.stickyScroll.enabled`）：長對話中已捲過的提示會釘選在頂部並顯示其位置，點選可跳回，旁邊的上／下按鈕可逐一瀏覽提示。
- **棄用**：本版無。

**總結**：這是一次針對「使用門檻」的鬆綁——模型供應商可隨時換、不用 GitHub 帳號也能進 Agents 視窗；再加上長對話定位與 HTML 即時預覽兩項體驗改善。

---

*資料來源：[Visual Studio Code 1.133 發行說明](https://code.visualstudio.com/updates/v1_133)*
