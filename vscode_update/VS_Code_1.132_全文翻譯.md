# Visual Studio Code 1.132

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 8 月 5 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.132.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.132.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.132.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.132.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.132.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.132.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.132.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.132.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.132.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.132 版本。本次發行帶來整合式瀏覽器中的元素層級回饋、多語言聽寫、側邊聊天，以及混合式 Markdown 編輯器中的 Markdown 差異。

- [**整合式瀏覽器中的留言**](#整合式瀏覽器中的留言)：透過對特定網頁元素留言，給予 Agent 精確的回饋。
- [**多語言聽寫**](#聽寫引導與自訂)：使用遵循您語言偏好或自動偵測語言的裝置端模型，以多種語言聽寫。
- [**使用 `/btw` 的側邊聊天**](#使用-btw-的側邊聊天)：在不中斷目前 Agent 回合的情況下提出情境問題。
- [**混合式 Markdown 編輯器中的 Markdown 差異（實驗性）**](#混合式-markdown-編輯器中的-markdown-差異實驗性)：在已渲染的 Markdown 中審閱變更，同時繼續編輯修改後的文件。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

Agent host 讓您可以從多個 VS Code 視窗連線至同一個 Agent 工作階段。它根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）在專用程序中執行 Copilot、Claude 和 Codex 等 Agent 工具鏈。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，使其行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host 並漸進式地向使用者推出。以下截圖展示如何在編輯器視窗中選取 agent host 上的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_132/agent-host-harness-dropdown-editor.webp)

您可以在我們的 [VS Code Agent Host 文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)中了解更多。如果您有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

### Agents 視窗

[Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)為您提供專用空間來啟動和監控多個 Agent 工作階段。

#### 從聊天輸入追蹤工作階段活動

當 Agent 跨檔案、子代理、預覽和瀏覽器工作時，您可以使用聊天輸入上方的即時狀態膠囊來追蹤其進度並快速返回相關工作。這些膠囊會隨著您正在檢視的工作階段的活動而更新：

- **Changes** 彙總變更的數量。選取它可在即時更新的多檔差異中檢視目前回合的變更。
- **Previews** 提供 Agent 建立或編輯的檔案的 Markdown 預覽存取。
- **Subagents** 讓您透過在獨立聊天中開啟子代理的工作，追蹤 Agent 在子代理中正在做什麼。
- **Browsers** 讓您在 Agent 與整合式瀏覽器互動時跟隨其進行。

#### 切換編輯器類型

**設定**：`breadcrumbs.showEditorType`

當有多種編輯器類型可用時，Agents 視窗中的編輯器類型下拉選單會顯示目前的編輯器，並讓您在可用的一般編輯器和差異編輯器之間切換，而不需使用 **Reopen Editor With** 命令。您也可以從下拉選單變更預設編輯器。此下拉選單使用階層連結列的右側，該列在 Agents 視窗中同時提供檔案導覽。

若要在編輯器視窗中顯示編輯器類型下拉選單，請啟用 `breadcrumbs.showEditorType`。

### 使用 /btw 的側邊聊天

當您想在不中斷目前回合的情況下提出問題時，可以在聊天輸入中輸入 `/btw` 來開啟側邊聊天。側邊聊天共用您主要聊天的上下文和提示快取，因此您可以針對目前回合提出問題。同樣地，您可以選取聊天回應中的文字，針對該回應提出情境問題。

您也可以透過在其他聊天中引用聊天來共用上下文，方式是將聊天分頁拖曳至輸入框，或輸入 `#chat:` 然後挑選要包含的聊天標題。

---

## Chat

### 終端機輸出重新流動

Chat 中展開的終端機輸出會隨著檢視調整大小而重新流動至可用寬度。先前，輸出使用固定寬度，導致行過早換行，並在較寬的檢視中留下未使用的空間。

此改善適用於來自 Local Agent 工具鏈和在 [agent host](#agent-host) 上執行的 Copilot 工具鏈的終端機輸出。

---

## 編輯器體驗

### 聽寫引導與自訂

**設定**：`agents.voice.language`

內建聽寫可在聊天輸入、編輯器和終端機中將語音轉換為文字。當您首次使用聽寫時，引導體驗會協助您在開始前確認選取的麥克風。它會顯示即時的麥克風波形、在有多支麥克風可用時提供裝置選擇器，並連結至聽寫設定與自訂項目。您可以從命令面板使用 **Voice Mode: Show Introduction** 重新開啟此體驗。

![截圖顯示聽寫介紹，包含麥克風選取和即時音訊波形。](https://code.visualstudio.com/assets/updates/1_132/dictation-onboarding.webp)

聽寫現在使用多語言的 Nemotron 3.5 作為其預設的裝置端模型。該模型將音訊保留在您的裝置上，並遵循 `agents.voice.language`。使用自動語言選取時，聽寫在支援的情況下使用您的系統或瀏覽器地區設定，否則讓模型偵測語言。

若要讓最終逐字稿適應專案術語或格式偏好，請執行 **Voice: Configure Dictation Instructions**。VS Code 會合併來自 `~/.copilot/dictation.md` 和受信任工作區中 `.github/dictation.md` 的指令。這些指令補充內建的清理規則，這些規則會保留您語音的含意，並在適當時偏好使用數字符號。

如果網路限制導致裝置端模型無法下載，錯誤通知會提供從磁碟匯入官方 Foundry Local 模型套件的操作。在正常的首次使用下載期間，麥克風按鈕會顯示進度，而非在輸入框中放置下載文字。

### 混合式 Markdown 編輯器中的 Markdown 差異（實驗性）

在上一個版本中，我們[引入了混合式 Markdown 編輯器](https://code.visualstudio.com/updates/v1_131#_hybrid-markdown-editor-experimental)，它結合了已渲染的 Markdown、就地編輯和 Agent 可據以行動的留言。在本次發行中，Markdown 差異可以在混合式 Markdown 編輯器中開啟。修改後的文件仍可編輯，同時邊界指示器會醒目顯示新增、變更和刪除的內容。您可以使用編輯器類型下拉選單在文字差異和帶有差異註解的 Markdown 編輯器之間切換。

### 整合式瀏覽器中的留言

為了對網頁提供回饋，對特定元素留言通常很有用。在本次發行中，整合式瀏覽器新增了選取網頁元素並以 Agent 回饋為其加上註解的支援。您可以使用 `workbench.action.browser.addElementCommentToChat` 鍵盤快速鍵來觸發此模式。

在此模式下，您可以選取多個元素並為每個元素加上留言，然後再發送至聊天。

---

## 終端機

### 終端機命令的更好聽寫

終端機聽寫套用感知 Shell 的清理，讓說出的命令保留 Shell 語法。例如，說出「git commit dash m hello world」會產生 `git commit -m "Hello World"`，而非逐字插入這些字詞。

選取使用中的麥克風按鈕以停止錄音。

---

## 提議的 API

### 依編輯器模式設定自訂編輯器優先順序

提議的 `customEditors.priority` 功能讓擴充功能可以為文字編輯器和差異編輯器選擇不同的優先順序。例如，擴充功能可以對一般檔案預設使用其自訂編輯器，同時保持內建編輯器作為差異的預設值，反之亦然。

現有的單一優先順序值仍可繼續運作，且差異編輯器預設使用 `explicit`。擴充功能作者可以了解更多關於[設定自訂編輯器優先順序](https://github.com/microsoft/vscode/issues/292379)的資訊。我們計劃在下一個版本中將此功能推向穩定。

---

## 已棄用的功能和設定

### 本次發行的新棄用項目

- Agent host 政策

`ChatAgentHostEnabled` 政策已被移除，因此管理員無法再透過政策集中停用 agent host。開發者可以繼續使用 `chat.agentHost.enabled` 來選擇 Agent 是否在獨立的 agent host 程序中執行。

### 即將到來的棄用項目

沒有即將到來的棄用項目。

---

## 感謝

對 `vscode` 的貢獻：

- [@accnops (Arthur Cnops)](https://github.com/accnops)
  - Voice：以語音回答問題輪播 [PR #327899](https://github.com/microsoft/vscode/pull/327899)
  - Voice：將並行的問題表單排入佇列而非交換它們 [PR #328205](https://github.com/microsoft/vscode/pull/328205)
- [@alexander-zw (Alexander Wu)](https://github.com/alexander-zw)：[trivial] 在 cursorEvents.ts 的 docstring 中新增範例來源 [PR #241250](https://github.com/microsoft/vscode/pull/241250)
- [@AndyBodnar (Andy )](https://github.com/AndyBodnar)：customEditor：修正檔案刪除後的過時快取 [PR #287966](https://github.com/microsoft/vscode/pull/287966)
- [@AntonioLujanoLuna (Antonio Lujano Luna)](https://github.com/AntonioLujanoLuna)：保留傳入的 Anthropic PDF 文件 [PR #325833](https://github.com/microsoft/vscode/pull/325833)
- [@Bestra (Chris Westra)](https://github.com/Bestra)：記錄組織資源快取建立失敗 [PR #326958](https://github.com/microsoft/vscode/pull/326958)
- [@bstee615 (Benjamin Steenhoek)](https://github.com/bstee615)：將預設 NES 積極度設為 medium [PR #327049](https://github.com/microsoft/vscode/pull/327049)
- [@denizguney (Deniz Güney Yıldırım)](https://github.com/denizguney)：feat：新增 getAccessibilityStatus [PR #328018](https://github.com/microsoft/vscode/pull/328018)
- [@dsavy4 (Dmitry Savy)](https://github.com/dsavy4)
  - 修正 stableStringify 將共用參考視為循環的問題 [PR #327398](https://github.com/microsoft/vscode/pull/327398)
  - 修正 BidirectionalMap 在金鑰更新時留下過時反向項目的問題 [PR #327403](https://github.com/microsoft/vscode/pull/327403)
- [@EmrecanKaracayir (Emrecan Karaçayır)](https://github.com/EmrecanKaracayir)：修正終端機建議符號圖示遺失的 CSS 對應 [PR #293158](https://github.com/microsoft/vscode/pull/293158)
- [@jdanbrown (Dan Brown)](https://github.com/jdanbrown)：終端機分頁標題：顯示「~」而非「$HOME」[PR #275378](https://github.com/microsoft/vscode/pull/275378)
- [@KevinWang-wpq](https://github.com/KevinWang-wpq)：在解密失敗後移除過時的密鑰 [PR #324014](https://github.com/microsoft/vscode/pull/324014)
- [@mirimadahmed (Mir)](https://github.com/mirimadahmed)
  - Voice：在免持播放前預熱擷取 [PR #328225](https://github.com/microsoft/vscode/pull/328225)
  - 讓編碼 Agent 具備語音感知以提供更好的語音體驗 [PR #328217](https://github.com/microsoft/vscode/pull/328217)
- [@Moli13337 (Moli)](https://github.com/Moli13337)：docs：修正 CONTRIBUTING.md 檔案中不正確的目錄路徑 [PR #325810](https://github.com/microsoft/vscode/pull/325810)
- [@Mr-Nilarnab (MR NILARNAB GITHUB)](https://github.com/Mr-Nilarnab)：docs：修正 README 中的小錯字和文法 [PR #325006](https://github.com/microsoft/vscode/pull/325006)
- [@Muszic (Sangeet)](https://github.com/Muszic)：修正：更正 takeWhile 和 takeFromEndWhile 中的 ArrayQueue 邊界 [PR #301119](https://github.com/microsoft/vscode/pull/301119)
- [@ohah (ohah)](https://github.com/ohah)：修正：file-found Badge 垂直對齊 [PR #273098](https://github.com/microsoft/vscode/pull/273098)
- [@peterdanwan (Peter Wan)](https://github.com/peterdanwan)：修正：簽章說明的使用中多載未更新 [PR #320980](https://github.com/microsoft/vscode/pull/320980)
- [@praneethhere (Praneeth Kodumagulla)](https://github.com/praneethhere)：為 Copilot CLI 狀態支援 COPILOT_HOME [PR #314917](https://github.com/microsoft/vscode/pull/314917)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)：修正：Restrict continue comment 中的 Bug [PR #322668](https://github.com/microsoft/vscode/pull/322668)
- [@rushil-b-patel (Rushil Patel (rusp))](https://github.com/rushil-b-patel)：feat：為 Markdown 預覽中的程式碼區塊新增複製按鈕 [PR #323609](https://github.com/microsoft/vscode/pull/323609)
- [@samir-nimbly](https://github.com/samir-nimbly)：修正登出 GitHub 時聊天圖片附件被靜默丟棄的問題 [PR #323856](https://github.com/microsoft/vscode/pull/323856)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：titleBarPart 中的記憶體洩漏 [PR #327552](https://github.com/microsoft/vscode/pull/327552)
  - 修正：筆記本檢視模型中的記憶體洩漏 [PR #328208](https://github.com/microsoft/vscode/pull/328208)
  - 修正：decorationAddon._decorations 中的記憶體洩漏 [PR #326933](https://github.com/microsoft/vscode/pull/326933)
  - 修正：chatServiceImpl 中的記憶體洩漏 [PR #327128](https://github.com/microsoft/vscode/pull/327128)
  - 修正：settings-tree 中的記憶體洩漏 [PR #327909](https://github.com/microsoft/vscode/pull/327909)
  - 修正：historyService 中的記憶體洩漏 [PR #327518](https://github.com/microsoft/vscode/pull/327518)
- [@sricursion (Sriraj)](https://github.com/sricursion)：在自訂項目索引中跳脫動態值 [PR #327475](https://github.com/microsoft/vscode/pull/327475)
- [@thernstig (Tobias Hernstig)](https://github.com/thernstig)：修正轉發埠狀態列項目以切換埠檢視 [PR #320090](https://github.com/microsoft/vscode/pull/320090)
- [@Vector341](https://github.com/Vector341)：[html] 修正 JavaScript 區塊註解上的驗證錯誤（針對 #171153）[PR #240932](https://github.com/microsoft/vscode/pull/240932)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - 修正(javascript)：將 JSDoc 接續模式與 TypeScript 語言設定同步 [PR #308433](https://github.com/microsoft/vscode/pull/308433)
  - [json] 為語言設定新增 #region 摺疊標記 [PR #318515](https://github.com/microsoft/vscode/pull/318515)
  - 為 npm 補全和懸停新增 package.json catalog 支援 [PR #307989](https://github.com/microsoft/vscode/pull/307989)
  - 在 Toggle Search Details 工具提示中顯示按鍵繫結 [PR #311859](https://github.com/microsoft/vscode/pull/311859)
  - 修正(server)：在 serve-web 中傳播 --enable-proposed-api [PR #310207](https://github.com/microsoft/vscode/pull/310207)
  - 修正：當選取範圍為空時移除所有手動摺疊範圍 [PR #304793](https://github.com/microsoft/vscode/pull/304793)
  - 修正：在終端機結束代碼 hack 中偵測被中斷的命令 [PR #307256](https://github.com/microsoft/vscode/pull/307256)
  - 修正：將模組指令碼內容包裝在區塊範圍中以防止錯誤的重新宣告錯誤 [PR #308027](https://github.com/microsoft/vscode/pull/308027)
- [@zmr-233](https://github.com/zmr-233)：避免使用會觸發整個工作台樣式失效的 :has() 選擇器 [PR #327052](https://github.com/microsoft/vscode/pull/327052)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@MominRaza (Momin Ahmad)](https://github.com/MominRaza)
- [@palinkasnorbert (Norbert Palinkas)](https://github.com/palinkasnorbert)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@ganlvtech (Ganlv)](https://github.com/ganlvtech)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| agent host | agent host |
| Agents window | Agents 視窗 |
| annotate | 註解 |
| breadcrumb bar | 階層連結列 |
| Command Palette | 命令面板 |
| Copilot SDK | Copilot SDK |
| custom editor | 自訂編輯器 |
| dictation | 聽寫 |
| diff | 差異 |
| editor type dropdown | 編輯器類型下拉選單 |
| extension | 擴充功能 |
| Foundry Local | Foundry Local |
| gutter indicator | 邊界指示器 |
| harness | 工具鏈 |
| hybrid Markdown editor | 混合式 Markdown 編輯器 |
| Integrated Browser | 整合式瀏覽器 |
| locale | 地區設定 |
| memory leak | 記憶體洩漏 |
| multi-file diff | 多檔差異 |
| Nemotron | Nemotron |
| numerals | 數字符號 |
| on-device model | 裝置端模型 |
| onboarding | 引導 |
| pill | 膠囊 |
| prompt cache | 提示快取 |
| Proposed API | 提議的 API |
| reflow | 重新流動 |
| session | 工作階段 |
| shell-aware cleanup | 感知 Shell 的清理 |
| side chat | 側邊聊天 |
| speech-to-text | 語音轉文字 |
| subagent | 子代理 |
| terminal | 終端機 |
| transcript | 逐字稿 |
| trusted workspace | 受信任工作區 |
| turn | 回合 |
| Voice Mode | 語音模式 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.132 發行說明](https://code.visualstudio.com/updates/v1_132)*
