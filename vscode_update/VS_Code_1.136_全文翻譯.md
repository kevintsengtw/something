# Visual Studio Code 1.136

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev)、[Instagram](https://www.instagram.com/vscode.ig) 上追蹤我們

---

_發行日期：2026 年 9 月 2 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.136.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.136.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.136.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.136.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.136.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.136.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.136.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.136.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.136.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.136 版本。本次發行協助您以 Agent 完成 Pull Request，並跨複雜的工作區和相關聊天管理 Agent 工作。

- [**Agent Merge（Preview）**](#agent-mergepreview)：解決審閱回饋、失敗的檢查和合併衝突，直到您的 Pull Request 可以合併為止。
- [**多根工作區（實驗性）**](#編輯器視窗中的多根工作區實驗性)：在多根工作區的所有資料夾中使用 Copilot 和 Claude Agent 工作階段。
- [**聊天背景（實驗性）**](#agents-視窗中的聊天背景實驗性)：使用內建圖樣或您自己的圖片來個人化 Agents 視窗。
- [**聊天工作階段**](#瀏覽相關的聊天和工作階段)：在工作階段階層中組織相關聊天，並快速看出哪些需要您的關注。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## The Story of VS Code：全球首映

探索 VS Code 背後的故事，從它早期的起源，到今日數百萬開發者使用的平台，以及一路上協助形塑它的社群。

**[首映：9 月 4 日太平洋時間上午 8:00。與我們同在！](https://aka.ms/the-story-of-vs-code)**

[![圖形海報，VS Code 標誌在充滿程式碼的深色背景上發光，標題為「The Story of VS Code」。](https://code.visualstudio.com/assets/updates/1_136/the-story-of-vs-code.png)](https://aka.ms/vscode-trailer)

---

## Agents

### 編輯器視窗中的多根工作區（實驗性）

**設定**：`chat.agentHost.copilotAgent.multiRootEnabled`、`chat.agentHost.claudeAgent.multiRootEnabled`

編輯器視窗 Chat 檢視中的 Copilot 和 Claude Agent 工作階段支援[多根工作區](https://code.visualstudio.com/docs/editing/workspaces/multi-root-workspaces)。

此功能目前的範圍僅限於編輯器視窗。

[Agent hooks](https://code.visualstudio.com/docs/agent-customization/hooks) 仍限定於單一工作區資料夾。如果在多個資料夾中偵測到 hooks，VS Code 會提示您選取要從哪個主要資料夾載入它們。

### 重新設計的新工作階段輸入

在重新設計的新工作階段輸入中，以更少的設定開始委派工作。更新後的體驗將提示、模型選取、工作區選取以及其他工作階段控制項整合在同一個版面中。

![截圖顯示 Agents 視窗中重新設計的新工作階段輸入，含上下文、權限、worktree 和分支控制項。](https://code.visualstudio.com/assets/updates/1_136/agents-new-session-input.webp)

### 改善的工作區解析

除了絕對路徑和工作區 URI 之外，Agent 現在也可以透過專案名稱來解析工作區。工作階段工具也會為多根工作區保留專案 URI 和所有工作目錄。

因此，您可以提出像是「run this in the vscode workspace」這樣的請求，而不需提供完整路徑。如果多個工作區有相同名稱，Agent 會回報可能的相符項目，而不是默默地選擇其中一個。遠端工作區 URI 同樣受到支援。

### 瀏覽相關的聊天和工作階段

讓相關的 Agent 工作保持井然有序，並從 Agents 視窗的工作階段清單在工作階段和聊天之間移動。聊天以其父工作階段的子項目形式呈現，因此您可以了解哪些聊天屬於同一組，而不需管理一堆不相關的工作階段。

每個聊天列顯示自己的標題、狀態和待處理的核准，因此您可以看出哪個聊天需要您的輸入。您可以展開或摺疊此階層，並直接從樹狀結構開啟、重新命名、移動或刪除個別的聊天。

當 Agent 將獨立的工作委派給多個聊天時，每個建立的聊天都會取得有意義的標題，並出現在此階層中。

新的工作階段或聊天會被放置在靠近其來源的位置，且接收的請求會包含來源連結，例如 **Sent by another session** 或 **Sent from another chat**。選取該連結可返回發起它的確切工作階段或聊天。

![截圖顯示帶有 Sent from another chat 來源連結的委派請求。](https://code.visualstudio.com/assets/updates/1_136/session-chat-source.webp)

### 工作階段檔案的可讀階層連結

先前在內部工作階段狀態目錄中建立的檔案，會在編輯器階層連結中顯示內部的工作階段識別碼。階層連結現在使用穩定的供應商和工作階段標籤，讓您更容易辨識檔案位置，而不會暴露實作細節。

![截圖顯示 Agent 建立的檔案在階層連結中呈現可讀的工作階段標籤。](https://code.visualstudio.com/assets/updates/1_136/session-breadcrumbs.webp)

### Agents 視窗中的聊天背景（實驗性）

**設定**：`chat.agentSessions.preferredDarkBackgroundImage`、`chat.agentSessions.preferredLightBackgroundImage`、`chat.agentSessions.backgroundImageLayout`

使用裝飾性的聊天背景來個人化 Agents 視窗：可以是感知佈景主題的內建 VS Code 圖示圖樣，或是您自己的圖片。

執行 **Chat: Set Background...** 以在 **Codicons** 圖樣和您機器上的圖片之間選擇。您最近使用的五張圖片會列在 **Recently Used** 之下，且該清單僅保存在這台機器上。

![截圖顯示 Agents 視窗中新工作階段輸入後方的 Codicons 圖樣。](https://code.visualstudio.com/assets/updates/1_136/agents-chat-background-codicons.webp)

當您使用自己的圖片時，執行 **Chat: Change Background Layout...** 來配置它。共有 11 種版面可用：**Repeat**、**Stretch**、**Center**，以及各個邊緣和角落。在清單中移動時會就地預覽每種版面，因此您可以在確定之前先判斷結果。**Chat: Clear Background** 可回到純色表面。

深色和淺色佈景主題各自保有不同的背景。每個深色佈景主題共用 `chat.agentSessions.preferredDarkBackgroundImage`，每個淺色佈景主題共用 `chat.agentSessions.preferredLightBackgroundImage`，因此在深色和淺色佈景主題之間切換時，背景也會隨之替換。高對比佈景主題會完全抑制背景，且這三個命令在其中不可用。

聊天內容帶有自己的填色，以便在背後任何內容之上都保持可讀。您的請求保持完全不透明，Agent 回應在兩側的邊距中漸隱，而 Markdown 表格和終端機輸出等寬幅內容則保持完整的背襯。

![截圖顯示 Agent 回應和 Markdown 表格在 Codicons 背景上仍保持可讀。](https://code.visualstudio.com/assets/updates/1_136/agents-chat-background-readability.webp)

深入了解[個人化聊天](https://code.visualstudio.com/docs/chat/chat-overview#_personalize-chat)。

### Agent 工作階段通知

**設定**：`chat.notifyWindowOnConfirmation`、`chat.notifyWindowOnResponseReceived`

當 Agent 工作階段需要您的輸入或完成其工作時，VS Code 可以通知您。當您跨多個工作階段、工作區或 VS Code 視窗委派工作時，這特別有用。

預設情況下，通知僅在 VS Code 視窗未取得焦點時出現。您可以分別為需要輸入的工作階段和已收到回應的工作階段設定通知。通知包含返回相關工作階段的直接連結，因此選取通知會聚焦至正確的視窗並開啟需要關注的工作階段。

### Agent Merge（Preview）

**設定**：`chat.agentMerge.enabled`

Agent Merge 協助您將 Pull Request 推過終點線。它會請 Agent 處理審閱回饋、修正失敗的檢查和合併衝突，並重新執行工作流程。Agent Merge 會重複此流程，直到 Pull Request 可以合併為止。

若要試用 Agent Merge，請啟用 `chat.agentMerge.enabled`。您目前只能從 Agents 視窗為工作階段啟用 Agent Merge。執行 **Enable Agent Merge for Active Session** 或選取標題列中的 **Agent Merge** 按鈕。深入了解[使用 Agent Merge](https://code.visualstudio.com/docs/agents/run/agents-window#_finish-a-pull-request-with-agent-merge)。

### Agent Host

Agent host 讓您可以從多個 VS Code 視窗連線至同一個 Agent 工作階段。它根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）在專用程序中執行 Agent 工具鏈。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，使 Agent 的行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host。以下截圖顯示在編輯器視窗中為 agent host 選取的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_136/agent-host-harness-dropdown-editor.webp)

在 [agent host 文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)和我們新的 [agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)中了解更多，我們在文中分享了為何建置 agent host、它在 VS Code 中帶來什麼、其架構與開放協定如何運作，以及您可以自行嘗試的工作流程。

如果您有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

---

## Chat

### 聽寫資料的企業控制

管理員可以透過企業政策管理聽寫模型和語言模型逐字稿清理。

這些新的控制項讓您可以要求使用裝置端轉錄並停用語言模型清理，因此聽寫仍然可用，同時防止聽寫資料被送往雲端轉錄或 Copilot 模型。深入了解[聽寫資料的企業控制](https://code.visualstudio.com/docs/enterprise/ai-settings#control-dictation-data)。

---

## 協助工具

### Agents 視窗中的 Screen Reader Optimized 徽章

Agents 視窗標題列中的 **Screen Reader Optimized** 徽章讓使用中的協助工具模式更容易識別。當啟用 Screen Reader Optimized 模式時會出現此徽章。選取該徽章可停用此模式。

![Agents 視窗標題列中 Screen Reader Optimized 徽章的截圖。](https://code.visualstudio.com/assets/updates/1_136/agents-screen-reader-optimized-badge.webp)

---

## 編輯器體驗

### 編輯器視窗的版面密度（實驗性）

**設定**：`workbench.experimental.modernUI`、`window.density.layout`

使用 **Compact** 版面密度在編輯器視窗中容納更多內容。當啟用 `workbench.experimental.modernUI` 時，您可以在兩種版面密度之間選擇：

- **Default** 版面密度與目前的編輯器視窗版面相同。
- **Compact** 版面密度移除面板之間的間距，並縮減面板內部的間距。

此設定可在設定選單的 **Layout Density** 區段中取得，也可以使用 `window.density.layout` 設定。

---

## 程式碼編輯

### 自動換行改善

插入的文字不再將換行後的行推出編輯器可視區域。自動換行會將色彩裝飾器、Inlay 提示間距、行內進度指示器和中斷點預留位置的視覺寬度納入考量。請看以下的前後對照截圖。在上方的圖片中，您可以看到 rgba 被輕微裁切；而在第二張圖片中則沒有。

![截圖顯示換行後的 CSS 行在編輯器可視區域邊緣被裁切。](https://code.visualstudio.com/assets/updates/1_136/word-wrapping-before.jpg)

![截圖顯示換行後的 CSS 行完整容納在編輯器可視區域內。](https://code.visualstudio.com/assets/updates/1_136/word-wrapping-after.jpg)

---

## 整合式瀏覽器

### 拼字檢查建議

在可編輯欄位中對拼錯的字按右鍵，即可選取建議的更正。在使用持續性資料儲存的工作階段中，您也可以選取 **Add to Dictionary**。

![截圖顯示整合式瀏覽器內容選單中的拼字建議和 Add to Dictionary。](https://code.visualstudio.com/assets/updates/1_136/spell-check.webp)

---

## 終端機

### 減少執行命令時的延遲

在特定時序條件下 Shell 整合就緒時，由擴充功能執行的終端機命令不再產生不必要的延遲。遇到此情況的 JavaScript 偵錯工具使用者，在啟動程式時將不會再經歷五秒的延遲。

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@abmahdy (Ahmed Mahdy)](https://github.com/abmahdy)：限制提示檔案探索讀取的並行數 [PR #331855](https://github.com/microsoft/vscode/pull/331855)
- [@DanTup (Danny Tuppeny)](https://github.com/DanTup)：註明位置相同的 InlayHints 會依序顯示 [PR #175525](https://github.com/microsoft/vscode/pull/175525)
- [@davidbitton (David B. Bitton)](https://github.com/davidbitton)：為整合式瀏覽器內容選單新增拼字建議 [PR #333043](https://github.com/microsoft/vscode/pull/333043)
- [@JeffreyCA](https://github.com/JeffreyCA)：為 Azure Developer CLI (azd) 更新 Fig 規範 [PR #331727](https://github.com/microsoft/vscode/pull/331727)
- [@juliagongms (Julia Gong)](https://github.com/juliagongms)：nes：在預設供應商路徑上套用 supportsUnifiedCompletions [PR #332802](https://github.com/microsoft/vscode/pull/332802)
- [@koubaki](https://github.com/koubaki)：更新 inlineChatIntent.ts 中的錯誤訊息 [PR #329157](https://github.com/microsoft/vscode/pull/329157)
- [@ktsoator (ktsoator)](https://github.com/ktsoator)：Hover：為長內容還原捲動 [PR #331439](https://github.com/microsoft/vscode/pull/331439)
- [@na2co3-ftw (na2co3)](https://github.com/na2co3-ftw)：現代化 UI：修正分頁操作淡出的 CSS 特異性 [PR #332103](https://github.com/microsoft/vscode/pull/332103)
- [@preitinger (Peter Reitinger)](https://github.com/preitinger)：更新 snippet.md [PR #231790](https://github.com/microsoft/vscode/pull/231790)
- [@remcohaszing (Remco Haszing)](https://github.com/remcohaszing)
  - 正規化行尾游標移動操作 [PR #296712](https://github.com/microsoft/vscode/pull/296712)
  - 修正換行時超出邊界的文字選取 [PR #262910](https://github.com/microsoft/vscode/pull/262910)
  - 四捨五入自訂行高 [PR #298421](https://github.com/microsoft/vscode/pull/298421)
  - 在 Monaco 編輯器中公開 score 函式 [PR #322959](https://github.com/microsoft/vscode/pull/322959)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：explorer viewer 中的記憶體洩漏 [PR #332332](https://github.com/microsoft/vscode/pull/332332)
  - 修正：LSP 終端機補全中的記憶體洩漏 [PR #332173](https://github.com/microsoft/vscode/pull/332173)
- [@TheNotary](https://github.com/TheNotary)：更新不支援的技能屬性的錯誤訊息 [PR #328318](https://github.com/microsoft/vscode/pull/328318)
- [@tisilent (xiejialong)](https://github.com/tisilent)：更新標籤和描述。[PR #219949](https://github.com/microsoft/vscode/pull/219949)
- [@unsupportedpastels (Mark S.)](https://github.com/unsupportedpastels)：在自訂 Agent 檔案中支援推理力度 [PR #329263](https://github.com/microsoft/vscode/pull/329263)
- [@weidehai (io)](https://github.com/weidehai)：錯誤更正指令 [PR #249715](https://github.com/microsoft/vscode/pull/249715)

對 `vscode-emmet-helper` 的貢獻：

- [@danfiedler-msft (Dan Fiedler)](https://github.com/danfiedler-msft)：將 GitHub Actions 釘選至完整長度的提交 SHA [PR #108](https://github.com/microsoft/vscode-emmet-helper/pull/108)

對 `vscode-livepreview` 的貢獻：

- [@danfiedler-msft (Dan Fiedler)](https://github.com/danfiedler-msft)：將 GitHub Actions 釘選至完整長度的提交 SHA [PR #854](https://github.com/microsoft/vscode-livepreview/pull/854)

對 `docfind` 的貢獻：

- [@danfiedler-msft (Dan Fiedler)](https://github.com/danfiedler-msft)：將 GitHub Actions 釘選至完整長度的提交 SHA [PR #62](https://github.com/microsoft/docfind/pull/62)

對 `node-pty` 的貢獻：

- [@danfiedler-msft (Dan Fiedler)](https://github.com/danfiedler-msft)：將 GitHub Actions 釘選至完整長度的提交 SHA [PR #958](https://github.com/microsoft/node-pty/pull/958)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@saroasid-web (Saswwo)](https://github.com/saroasid-web)
- [@zotabee (zotabee)](https://github.com/zotabee)
- [@lppedd (Edoardo Luppi)](https://github.com/lppedd)
- [@sandstrom (sandstrom)](https://github.com/sandstrom)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| Agent Merge | Agent Merge |
| agent hooks | Agent hooks |
| agent host | agent host |
| Agents window | Agents 視窗 |
| badge | 徽章 |
| breadcrumbs | 階層連結 |
| breakpoint | 中斷點 |
| chat background | 聊天背景 |
| Codicons | Codicons |
| color decorator | 色彩裝飾器 |
| Copilot SDK | Copilot SDK |
| dictation | 聽寫 |
| enterprise policy | 企業政策 |
| extension | 擴充功能 |
| harness | 工具鏈 |
| high contrast theme | 高對比佈景主題 |
| hierarchy | 階層 |
| inlay hint | Inlay 提示 |
| Integrated Browser | 整合式瀏覽器 |
| layout density | 版面密度 |
| memory leak | 記憶體洩漏 |
| merge conflict | 合併衝突 |
| multi-root workspace | 多根工作區 |
| on-device transcription | 裝置端轉錄 |
| Pull Request | Pull Request |
| reasoning effort | 推理力度 |
| Screen Reader Optimized | Screen Reader Optimized（螢幕閱讀器最佳化） |
| session | 工作階段 |
| shell integration | Shell 整合 |
| spell check | 拼字檢查 |
| terminal | 終端機 |
| theme-aware | 感知佈景主題 |
| transcript | 逐字稿 |
| viewport | 可視區域 |
| word wrapping | 自動換行 |
| workspace | 工作區 |
| worktree | worktree |

*資料來源：[Visual Studio Code 1.136 發行說明](https://code.visualstudio.com/updates/v1_136)*
