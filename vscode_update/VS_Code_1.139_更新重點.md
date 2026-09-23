# VS Code 1.139 更新重點

**版本：** 1.139｜**發行日期：** 2026 年 9 月 23 日
**原文：** https://code.visualstudio.com/updates/v1_139

---

本版三大主軸為遠端 Dev Container 工作階段、工作階段清單改善與編輯體驗：

- **在遠端主機的 Dev Container 中執行 Agent 工作階段**（`chat.agentHost.devContainer.enabled`）：Dev Container 工作階段從本機擴展到 SSH、Tunnel 與 WSL 主機，讓 Agent 直接用專案的工具鏈建置與測試遠端專案；遠端需有 Docker 與受支援的 Dev Container 設定，目前逐步推出中。
- **工作階段清單改善**：改用集中式中繼資料目錄，約 645 個工作階段時首次載入快約 12 倍、重新整理快約 4 倍；另新增 **Compact View** 精簡清單、可隱藏空群組，並支援在清單中直接重新命名工作階段與聊天。
- **編輯體驗**：新增自動換行指示器，以箭頭標示有換行的行；輸入左括號時若已有相符右括號，不再重複插入。

其他更新：

- **聊天呈現方式（Preview）**（`sessions.showChatTabs`）：多聊天的工作階段可切換 **Multiple**（分頁）或 **Single**（僅顯示作用中聊天）。
- **寵物命名比賽（實驗性）**：已於 9 月 17 日截止，新名字即將公布。
- **Proposed API**：`AuthenticationSession` 新增 `expiresAfter`，提供存取權杖的剩餘存留期。
- **修正**：帳戶原則停用 Agent 模式時無法再透過 `code --agents` 規避；修正企業管理 OTel 設定的競爭條件。
- **棄用**：本版無。

**總結**：本版讓 Agent 能在遠端 Dev Container 中工作、大型工作階段清單載入更快也更好管理，並補強日常編輯細節。

---

*資料來源：[Visual Studio Code 1.139 發行說明](https://code.visualstudio.com/updates/v1_139)*
