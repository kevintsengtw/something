# Visual Studio Code 1.135 版本重點摘要

**版本：** 1.135
**發行日期：** 2026 年 8 月 26 日
**原文：** https://code.visualstudio.com/updates/v1_135

---

本次發行協助您跨應用程式延續 Agent 工作階段、對 Agent 的工作取得第二意見，並在更精簡的 Agents 視窗中了解聊天用量。以下為官方列出的四大亮點：

## 一、外部 Agent 工作階段

- **設定**：`chat.agentSessions.showExternal`
- VS Code 中的 Sessions 清單現在可顯示您在其他應用程式中建立的近期 Copilot 或 Claude Agent 工作階段
- 預設顯示最多兩個最近更新的外部工作階段
- 從 Sessions 清單選取工作階段即可檢視對話，並以您的 Copilot 訂閱在 VS Code 中繼續進行
- 開啟外部工作階段時，聊天頂部的橫幅可讓您設定要在 Sessions 清單中顯示多少個外部工作階段；也可使用 Sessions 清單篩選器中的 **External** 子選單選擇要顯示哪些外部工作階段

## 二、Rubber Duck（實驗性）

- 實驗性功能，可從互補模型取得對 Agent 工作的第二意見
- 有助於揭露被遺漏的細節或邊界情況
- 在 Copilot agent host 工作階段中呼叫 `/rubber-duck` 命令即可使用

## 三、Agents 視窗 UX 改善

**單一窗格側邊佈局現為預設**（`sessions.layout.singlePaneDetailPanel`）

- 上一版引入的單一窗格佈局（工作階段詳細資料與編輯器位於單一側邊窗格，並與聊天共用分頁列），現在於桌面版 Agents 視窗預設啟用
- 差異在空間允許時使用並排檢視，側邊窗格過窄時切換為行內檢視；可用編輯器標題選單的 **Always Show Inline Diff** 讓差異在任何寬度都保持行內
- 操作列較不擁擠：差異、檢視模式、程式碼審閱和附件等編輯器專屬操作移至編輯器標題區域，共用標頭則專注於顯示或隱藏 Details
- 若要使用傳統佈局，停用該設定並重新載入視窗

**簡化的工作階段控制項與資訊**

- 工作階段標頭較不擁擠，工作階段標題位置更醒目，標題旁的溢位選單彙整建立聊天和釘選工作階段的操作；搜尋仍在 Sessions 清單中可用；當工作階段包含多個聊天時，聊天分頁會取代單一聊天標頭
- 工作階段資訊移至聊天輸入正上方，膠囊可顯示變更、Pull Request、Issue、Agent 正在互動的瀏覽器，以及工作階段的產出物；例如 **Changes** 膠囊顯示即時的檔案與差異計數，並可開啟完整的工作階段變更集
- 對個別膠囊按右鍵可開啟內容選單，選擇要顯示哪些膠囊類型；只要工作階段有變更，**Changes** 膠囊就會保持可見

## 四、檢視詳細的聊天回合用量

- 重新設計聊天回應頁尾，懸停時會顯示該回合中依模型細分的輸入、快取輸入和輸出 Token 用量

---

## 其他

- **Agent host**：讓您可從多個 VS Code 視窗連線至同一 Agent 工作階段，依 [AHP](https://microsoft.github.io/agent-host-protocol/) 在專用程序中執行 Agent 工具鏈；Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，行為與 Copilot CLI、獨立版 GitHub Copilot 應用程式一致；另有 [Agent Host 介紹 YouTube 影片](https://www.youtube.com/watch?v=k91ejc3G1YM)
- **編輯器風格的聊天黏性捲動（實驗性）**（`chat.stickyScroll.enabled`、`chat.experimental.stickyScroll.enabled`）：讓目前的提示在捲動審閱長回應時保持可見，行為進一步調整為與編輯器中的黏性捲動更一致；需同時啟用兩個設定才能試用重新設計的體驗
- **本機 Agent 工具鏈的沙箱**：先前已向 50% 使用者推出以在更大規模和實際使用情境中驗證。雖未發現特定的阻斷性問題，但繼續推出可能需要更多支援與後續工作，而目前團隊希望將重心保留在更高優先順序的投資上（特別是 agent host 和 Copilot 工具鏈領域），因此預設推出比例暫時調回 0%。本機 Agent 工具鏈的沙箱仍可透過 UI 以選擇加入的方式使用
- **已棄用的功能和設定**：無

---

*資料來源：[Visual Studio Code 1.135 發行說明](https://code.visualstudio.com/updates/v1_135)*
