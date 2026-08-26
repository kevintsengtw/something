# VS Code 1.135 更新重點

**版本：** 1.135｜**發行日期：** 2026 年 8 月 26 日
**原文：** https://code.visualstudio.com/updates/v1_135

---

本版四大主軸為外部工作階段接續、第二意見、Agents 視窗 UX 精簡與用量透明度：

- **在 VS Code 延續外部 Agent 工作階段**（`chat.agentSessions.showExternal`）：Sessions 清單可列出你在其他應用程式建立的近期 Copilot 或 Claude 工作階段，預設顯示最近更新的兩個。選取即可檢視對話並以 Copilot 訂閱在 VS Code 續接；可用聊天頂部橫幅或 Sessions 清單篩選器的 **External** 子選單調整顯示數量與範圍。
- **Rubber Duck（實驗性）**：在 Copilot agent host 工作階段輸入 `/rubber-duck`，由互補模型對 Agent 的工作給出第二意見，用來揭露遺漏的細節與邊界情況。
- **Agents 視窗 UX 改善**：單一窗格側邊佈局在桌面版成為預設（`sessions.layout.singlePaneDetailPanel`），差異依空間自動在並排／行內之間切換（可用 **Always Show Inline Diff** 固定行內），編輯器專屬操作移至編輯器標題區使操作列更清爽。工作階段標頭簡化——標題更醒目、次要操作收進溢位選單、多聊天時改用聊天分頁；工作階段資訊改放在聊天輸入正上方，以膠囊呈現變更、PR、Issue、瀏覽器與產出物，可右鍵選擇顯示哪些類型（**Changes** 恆常顯示）。
- **詳細聊天回合用量**：重新設計回應頁尾，懸停即可看到該回合依模型細分的輸入、快取輸入與輸出 Token。

其他更新：

- **Agent host**：可從多個視窗連線同一工作階段，依 AHP 在專用程序執行工具鏈，Copilot Agent 由 Copilot SDK 驅動；官方另提供介紹影片。
- **聊天黏性捲動（實驗性）**：行為調整為更貼近編輯器的黏性捲動，需同時開啟 `chat.stickyScroll.enabled` 與 `chat.experimental.stickyScroll.enabled`。
- **本機 Agent 工具鏈沙箱**：先前推出至 50% 使用者，雖未發現阻斷性問題，但為將重心保留給 agent host 與 Copilot 工具鏈等更高優先項目，預設推出比例暫調回 0%，仍可從 UI 自行選擇啟用。
- **棄用**：本版無。

**總結**：本版讓 Agent 工作可以跨應用程式接力、多一道模型交叉檢查，同時把 Agents 視窗的版面與資訊層次整理得更清爽，並讓 Token 花在哪裡一目了然。

---

*資料來源：[Visual Studio Code 1.135 發行說明](https://code.visualstudio.com/updates/v1_135)*
