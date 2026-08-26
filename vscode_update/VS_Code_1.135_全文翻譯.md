# Visual Studio Code 1.135

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev)、[Instagram](https://www.instagram.com/vscode.ig) 上追蹤我們

---

_發行日期：2026 年 8 月 26 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.135.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.135.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.135.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.135.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.135.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.135.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.135.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.135.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.135.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.135 版本。本次發行協助您跨應用程式延續 Agent 工作階段、對 Agent 的工作取得第二意見，並在更精簡的 Agents 視窗中了解聊天用量。

- [**外部 Agent 工作階段**](#在-vs-code-中延續外部-agent-工作階段)：在 VS Code 中延續來自其他應用程式的近期 Copilot 或 Claude Agent 工作階段。
- [**Rubber Duck（實驗性）**](#rubber-duck實驗性)：從互補模型取得第二意見，以揭露被遺漏的細節和邊界情況。
- [**Agents 視窗 UX 改善**](#單一窗格側邊佈局現為預設)：使用精簡的側邊佈局、簡化的工作階段控制項，以及更容易找到的工作階段資訊。
- [**詳細的聊天用量**](#檢視詳細的聊天回合用量)：檢視每個聊天回合中依模型細分的輸入、快取輸入和輸出 Token。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

Agent host 讓您可以從多個 VS Code 視窗連線至同一個 Agent 工作階段。它根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）在專用程序中執行 Agent 工具鏈。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，使 Agent 的行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host。以下截圖顯示在編輯器視窗中為 agent host 選取的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_135/agent-host-harness-dropdown-editor.webp)

從我們的 [VS Code Agent Host 文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)和下方的影片中了解更多。如果您有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

在我們的 [Agent Host 介紹 YouTube 影片](https://www.youtube.com/watch?v=k91ejc3G1YM)中觀看實際運作。

### 在 VS Code 中延續外部 Agent 工作階段

**設定**：`chat.agentSessions.showExternal`

VS Code 中的 Sessions 清單現在可以顯示您在其他應用程式中建立的近期 Copilot 或 Claude Agent 工作階段。預設情況下，VS Code 會顯示最多兩個最近更新的外部工作階段。從 Sessions 清單中選取工作階段以檢視該對話，並以您的 Copilot 訂閱在 VS Code 中繼續進行。

當您開啟外部工作階段時，聊天頂部的橫幅可讓您設定在 Sessions 清單中顯示多少個外部工作階段。您也可以使用 Sessions 清單篩選器中的 **External** 子選單來選擇要顯示哪些外部工作階段。您隨時可以使用 `chat.agentSessions.showExternal` 設定變更此偏好。

### Rubber Duck（實驗性）

Rubber Duck 是一項實驗性功能，讓您可以從互補模型取得對 Agent 工作的第二意見。它有助於揭露被遺漏的細節或邊界情況。您可以在 Copilot agent host 工作階段中呼叫 `/rubber-duck` 命令來使用 Rubber Duck。

了解更多關於 [GitHub Copilot CLI 中的 Rubber Duck](https://github.blog/ai-and-ml/github-copilot/github-copilot-cli-combines-model-families-for-a-second-opinion/)。

### 單一窗格側邊佈局現為預設

**設定**：`sessions.layout.singlePaneDetailPanel`

在上一個版本中，我們引入了單一窗格佈局。在此佈局中，工作階段詳細資料和編輯器位於單一側邊窗格中，並與聊天共用分頁列。單一窗格佈局現在在桌面版的 Agents 視窗中預設啟用。

本次發行也改善了此佈局：

- 差異在空間允許時使用並排檢視，並在側邊窗格變得太窄時切換為行內檢視。使用編輯器標題選單中的 **Always Show Inline Diff** 可讓差異在任何寬度下都保持行內。
- 操作列較不擁擠。針對差異、檢視模式、程式碼審閱和附件的編輯器專屬操作移至編輯器標題區域，而共用標頭則專注於顯示或隱藏 Details。

![截圖顯示單一窗格佈局中 Changes 編輯器簡化後的操作列。](https://code.visualstudio.com/assets/updates/1_135/agents-single-pane-action-bar.webp)

若要使用傳統佈局，請停用 `sessions.layout.singlePaneDetailPanel` 並重新載入視窗。

### 簡化的工作階段控制項與資訊

工作階段標頭較不擁擠，因此您可以更容易識別使用中的工作階段並專注於對話。

工作階段標題具有醒目的位置，而標題旁的溢位選單則彙整了建立聊天和釘選工作階段的操作。搜尋仍然在 Sessions 清單中可用。當工作階段包含多個聊天時，聊天分頁會取代單一聊天標頭。

![截圖顯示在簡化的 Agents 工作階段標頭中彙整的工作階段和聊天操作。](https://code.visualstudio.com/assets/updates/1_135/agents-simplified-session-header.webp)

工作階段資訊移至聊天輸入的正上方，在您工作時更容易找到。膠囊可以顯示變更、Pull Request、Issue、Agent 正在互動的瀏覽器，以及來自工作階段的產出物。例如，**Changes** 膠囊顯示即時的檔案和差異計數，並可開啟工作階段變更的完整集合。

![截圖顯示聊天輸入上方的變更、Pull Request 和產出物膠囊，並展開了產出物清單。](https://code.visualstudio.com/assets/updates/1_135/agents-session-artifacts-pill.webp)

在個別膠囊上按右鍵可開啟內容選單並選擇要顯示哪些膠囊類型。只要工作階段有變更，**Changes** 膠囊就會保持可見。

![截圖顯示用於選擇要顯示哪些工作階段膠囊類型的內容選單。](https://code.visualstudio.com/assets/updates/1_135/agents-session-pill-visibility.webp)

---

## Chat

### 聊天的編輯器風格黏性捲動（實驗性）

**設定**：`chat.stickyScroll.enabled`、`chat.experimental.stickyScroll.enabled`

聊天黏性捲動讓您在捲動瀏覽和審閱長回應時保持目前提示可見。我們進一步調整了其行為，使其與編輯器中的黏性捲動更加一致。同時啟用這兩個設定即可試用重新設計的體驗。

### 檢視詳細的聊天回合用量

為了讓您更了解自己的聊天用量，我們重新設計了聊天回應頁尾。當您懸停在其上時，頁尾會顯示該聊天回合中使用的輸入、快取輸入和輸出 Token 的依模型細分資料。

![截圖顯示聊天回合中每個模型的詳細 Token 用量。](https://code.visualstudio.com/assets/updates/1_135/chat_turn_info.webp)

### 本機 Agent 工具鏈的沙箱

我們先前已將沙箱推出給 50% 的使用者，以在更大規模和實際使用情境中進行驗證。雖然我們並未發現特定的阻斷性問題，但在我們希望將重心保留在更高優先順序投資（特別是在 agent host 和 Copilot 工具鏈領域）的此刻，繼續推出可能需要更多的支援與後續工作。因此，我們目前已將預設推出比例調回 0%。

本機 Agent 工具鏈的沙箱仍可透過 UI 作為選擇加入的功能使用，因此想試用的使用者仍可啟用它。

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@a-stewart (Anthony Stewart)](https://github.com/a-stewart)：將殘留的 respectAutoSaveConfig 變數重新命名為 isRefactoring [PR #160703](https://github.com/microsoft/vscode/pull/160703)
- [@bstee615 (Benjamin Steenhoek)](https://github.com/bstee615)：為 diffpatch 提示新增積極度選項 [PR #327544](https://github.com/microsoft/vscode/pull/327544)
- [@cipheraxat (Akshat Anand)](https://github.com/cipheraxat)：修正：在 32px 標頭中將現代化 UI 面板標題分頁置中 [PR #331612](https://github.com/microsoft/vscode/pull/331612)
- [@danyalahmed1995 (Danyal Ahmed)](https://github.com/danyalahmed1995)：修正不區分大小寫的彙總 basename glob 比對 [PR #316387](https://github.com/microsoft/vscode/pull/316387)
- [@dzsquared (Drew Skwiers-Koballa)](https://github.com/dzsquared)：在工具選擇器中將 mcp 伺服器金鑰作為描述加入 [PR #325003](https://github.com/microsoft/vscode/pull/325003)
- [@guimmd2 (Guilherme Menezes Magalhães)](https://github.com/guimmd2)：修正 npm 12+ 中損壞的 package.json 懸停中繼資料 [PR #327951](https://github.com/microsoft/vscode/pull/327951)
- [@jachinsamuel (Jachin Samuel)](https://github.com/jachinsamuel)：docs：移除失效的 Gitter 徽章（該服務已於 2023 年 6 月關閉）[PR #322702](https://github.com/microsoft/vscode/pull/322702)
- [@jadefr (Jade Ferreira Vieira)](https://github.com/jadefr)：修正 html-language-features esbuild 指令碼中的檔案 URL 轉路徑 [PR #328557](https://github.com/microsoft/vscode/pull/328557)
- [@jainampatel27 (Jainam Patel)](https://github.com/jainampatel27)：修正 selectionHighlightMaxLength 描述中的錯字 [PR #296427](https://github.com/microsoft/vscode/pull/296427)
- [@juliagongms (Julia Gong)](https://github.com/juliagongms)：nes：新增最佳化的 PatchBased02 提示策略 [PR #332018](https://github.com/microsoft/vscode/pull/332018)
- [@n-gist (n-gist)](https://github.com/n-gist)：修正 languages.getDiagnostics() problem-matchers 診斷重複的問題 [PR #290278](https://github.com/microsoft/vscode/pull/290278)
- [@rfeltis (Ralph Feltis)](https://github.com/rfeltis)
  - 移除 Agents 視窗啟動 A/A 實驗觸發器 [PR #331559](https://github.com/microsoft/vscode/pull/331559)
  - 還原聊天配額軌跡提示 [PR #331401](https://github.com/microsoft/vscode/pull/331401)
- [@RyanEwen (Ryan Ewen)](https://github.com/RyanEwen)：不要將失敗的工具呼叫呈現為成功 [PR #330707](https://github.com/microsoft/vscode/pull/330707)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：markersTable 中的記憶體洩漏 [PR #327885](https://github.com/microsoft/vscode/pull/327885)
  - 修正：mainThreadDocumentsAndEditors 中的記憶體洩漏 [PR #331170](https://github.com/microsoft/vscode/pull/331170)
  - 修正：設定預覽指示器中的記憶體洩漏 [PR #331990](https://github.com/microsoft/vscode/pull/331990)
- [@srikanthananthula63053 (srikanthananthula)](https://github.com/srikanthananthula63053)
  - 修正：延遲初始化聊天附件上下文時使用全新的服務存取器（修正 #329610）[PR #331416](https://github.com/microsoft/vscode/pull/331416)
  - 修正所有項目未勾選時的區段核取方塊狀態 [PR #331419](https://github.com/microsoft/vscode/pull/331419)
- [@TheRealAlexxx (alexxx)](https://github.com/TheRealAlexxx)：修正設定 UI 中 editor.selectionHighlightMaxLength 描述的錯字 [PR #332162](https://github.com/microsoft/vscode/pull/332162)
- [@zainnadeem786 (Zain Nadeem)](https://github.com/zainnadeem786)
  - 在 setUrisTrust() 中等待工作區信任轉換完成 [PR #328626](https://github.com/microsoft/vscode/pull/328626)
  - 修正 runInTerminal 環境值的 PowerShell 引號處理 [PR #331753](https://github.com/microsoft/vscode/pull/331753)

對 `vscode-windows-process-tree` 的貢獻：

- [@danfiedler-msft (Dan Fiedler)](https://github.com/danfiedler-msft)：將 GitHub Actions 釘選至完整長度的提交 SHA [PR #91](https://github.com/microsoft/vscode-windows-process-tree/pull/91)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@xguarch (Xavier Guarch)](https://github.com/xguarch)
- [@homeworld614 (homeworld614)](https://github.com/homeworld614)
- [@johnnydecimal (Johnny Noble)](https://github.com/johnnydecimal)
- [@wenma531 (noreply)](https://github.com/wenma531)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| action bar | 操作列 |
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| agent host | agent host |
| Agents window | Agents 視窗 |
| artifact | 產出物 |
| cached input | 快取輸入 |
| Copilot SDK | Copilot SDK |
| context menu | 內容選單 |
| diff | 差異 |
| edge case | 邊界情況 |
| external session | 外部工作階段 |
| harness | 工具鏈 |
| inline view | 行內檢視 |
| memory leak | 記憶體洩漏 |
| overflow menu | 溢位選單 |
| pill | 膠囊 |
| Pull Request | Pull Request |
| Rubber Duck | Rubber Duck |
| sandboxing | 沙箱 |
| session | 工作階段 |
| session header | 工作階段標頭 |
| side-by-side view | 並排檢視 |
| single-pane layout | 單一窗格佈局 |
| sticky scroll | 黏性捲動 |
| subscription | 訂閱 |
| tab bar | 分頁列 |
| terminal | 終端機 |
| token | Token |
| turn | 回合 |
| Workspace Trust | 工作區信任 |

*資料來源：[Visual Studio Code 1.135 發行說明](https://code.visualstudio.com/updates/v1_135)*
