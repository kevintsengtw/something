# Visual Studio Code 1.120

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 5 月 13 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.120.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.120.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.120.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.120.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.120.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.120.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.120.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.120.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.120.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.120 版本。本次發行將 Agents 視窗帶入 Stable，改善 BYOK 模型的可見性與控制，並新增 Markdown 生活品質改善及 Agent 安全功能。以下是本次發行的亮點：

- [**Agents 視窗進入 Stable**](#agents-視窗)：使用全新的 Agents 視窗，以 Agent 為優先的方式跨所有專案工作。
- [**BYOK 改善**](#語言模型)：追蹤和最佳化 Token 使用量，並為 BYOK 模型設定思考力度。
- [**Markdown 改善**](#語言)：使用 Markdown 差異預覽來審閱 Markdown 內容而非語法。
- [**命令風險評估**](#終端機命令風險評估實驗性)：在終端機命令執行前評估其風險。
- [**Token 最佳化**](#終端機工具輸出壓縮preview)：透過壓縮大型終端機輸出來減少上下文視窗使用量。

Happy Coding!

---

## Agents

### 使用 Agents 視窗跨專案協調任務（Preview）

雖然 VS Code 已被數百萬開發者用於 Agent 編碼，但其編輯器佈局主要針對單一任務、單一工作區的工作流程進行最佳化。為了讓使用者（以及我們自己！）能夠跨多個專案與多個 Agent 合作，我們建立了一種新型視窗：**Agents**。

全新的 Agents 視窗是您熟悉的編輯器的輔助視窗：專為 Agent 驅動開發打造，提供專用空間來探索、迭代和審閱跨多個專案的任務，並在它們之間無縫切換。由於 VS Code 是為開發者的選擇和靈活性而建造的，Agents 視窗讓您能選擇 Agent 工具鏈、在遠端機器上執行 Agent，並按照您想要的方式設定環境——色彩主題、鍵盤繫結和擴充功能皆包含在內。

Agents 視窗在過去幾個版本中已作為 VS Code Insiders 的一部分提供，而在本次發行中，它現在作為 Preview 在 VS Code Stable 中提供。

您可以透過多種方式開啟 Agents 視窗，包括 VS Code 標題列中的「Open in Agents」按鈕。若要了解更多關於其運作方式和可執行的操作，請參閱 [Agents 視窗文件](https://aka.ms/VSCode/Agents/docs)。

#### 有什麼新功能？

如果您已經在 Insiders 中使用 Agents 視窗，感謝您！我們持續根據您的回饋進行改進，本週推出了以下改善：

- **偏好設定跨新工作階段保留**：您在 Agent 工具鏈和隔離模式等下拉選單中的最近選擇，會在建立新工作階段時保留。
- **更輕鬆地捨棄變更**：您可以直接從 Changes 面板捨棄編輯。
- **在新工作階段中同步上游變更**：Files 面板上的同步按鈕讓您在 Agent 開始工作前查看來自基底分支的上游變更並拉取。
- **更確定性的變更互動**：Changes 面板中的操作可以更快完成，因為它們現在是確定性的。
- **已完成的工作階段預設顯示所有變更**：當您開啟一個標記為完成的工作階段時，會自動獲得 Agent 完整編輯的一覽檢視。
- **在最近的工作階段之間導覽**：使用標題列左上方的箭頭按鈕，無需離開視窗即可在最近的工作階段之間跳轉。
- **針對視窗覆寫設定**：Agents 視窗現在共用您所有的 VS Code 設定，當您希望在 Agents 視窗中有不同行為時，可以僅針對 Agents 視窗覆寫特定設定。

您的回饋持續對塑造 Agents 有很大幫助。請在 [GitHub 上提交問題](https://github.com/microsoft/vscode/issues)或瀏覽[現有問題](https://github.com/microsoft/vscode/issues?q=state%3Aopen%20label%3A%22agents-window%22)。

#### 擴展性

僅提供靜態內容的擴充功能（例如主題、語法、語言和鍵盤繫結）會在 Agents 視窗中自動啟動。我們也測試了 Marketplace 排名前 100 的擴充功能，其中一些在安裝於預設 VS Code 設定檔時也會啟動。

對於其他擴充功能，您可以透過 `extensions.supportAgentsWindow` 設定以 ID 方式加入。您以這種方式啟用的任何擴充功能都需要安裝在預設的 VS Code 設定檔中。

```json
"extensions.supportAgentsWindow": {
    "myextension.id": true
}
```

雖然我們仍在開發更完整的擴充功能支援，但我們希望與擴充功能作者合作，了解在 Agents 視窗中啟用擴充功能能解鎖哪些功能，以及各種擴充功能在此環境中應如何運作。無論您想構思利用跨專案執行 Agent 的新場景，還是分享您現有擴充功能在 Agents 視窗中的表現回饋，我們都樂於透過 [GitHub 問題](https://github.com/microsoft/vscode/issues?q=state%3Aopen%20label%3A%22agents-window%22)與您合作。

### 自動探索 Copilot CLI 外掛

以 GitHub Copilot CLI 安裝的 [Agent 外掛](https://code.visualstudio.com/docs/copilot/customization/agent-plugins)會被 VS Code 自動偵測，因此單次 `copilot plugin install` 即可同時涵蓋兩個介面。先前，您必須在 VS Code 中另外安裝相同的外掛，或將其路徑加入 `chat.plugins.paths`。

---

## 語言模型

透過 Bring Your Own Key（BYOK），您可以使用來自 Anthropic、OpenAI 和其他供應商的自有 API 金鑰，以利用您自己的計費或模型代管選項。若要了解更多，請參閱 [BYOK 文件](https://code.visualstudio.com/docs/copilot/customization/language-models#_bring-your-own-language-model-key)。

### 檢視 BYOK 模型 Token 使用量

管理模型的上下文視窗是獲得良好結果和控制成本的關鍵。模型可能會遺失對話中的重要細節，而 Token 使用量可能增加成本。本次發行為 BYOK 模型帶來更好的 Token 使用量可見性，讓您可以留意上下文視窗。

先前，當您使用透過自有 API 金鑰（Anthropic、OpenAI 或其他）引入的模型進行聊天時，控制項一律顯示 0% 和零 Token 計數，因為 Token 統計僅對內建模型有效。

聊天檢視中的上下文視窗控制項現在為 BYOK 模型顯示準確的 Token 使用量和已用百分比。

![聊天檢視中上下文視窗控制項的截圖，顯示 BYOK 模型的準確 Token 使用量和已用百分比。](https://code.visualstudio.com/assets/updates/1_120/context-window-byok-token-usage.webp)

### 設定 BYOK 推理模型的思考力度

具備推理能力的語言模型允許您設定其「思考力度」（thinking effort），這是一種在回應品質與速度/成本之間取捨的方式。您可以在[思考力度](https://code.visualstudio.com/docs/copilot/concepts/language-models#_thinking-effort)文件中了解更多。

在本次發行中，您現在可以直接從聊天檢視的模型選擇器設定 BYOK 推理模型的思考力度。選定的力度會在每次請求時轉發給模型，讓您在延遲和成本與回答品質之間取捨。

![語言模型編輯器的截圖，顯示 BYOK 模型的思考力度設定選項。](https://code.visualstudio.com/assets/updates/1_120/byok-configure-thinking-effort.webp)

> 適用於：透過 OpenAI 相容端點（OpenAI、xAI (Grok)、OpenRouter 和自訂 OpenAI / Azure OpenAI 部署）提供的 Bring-Your-Own-Key（BYOK）推理模型。Anthropic 模型先前已支援此功能；控制項現在在各供應商之間保持一致。

### 依供應商分組的模型選擇器

聊天檢視中的模型選擇器現在依供應商分組模型，當您有來自多個來源的模型存取權時，更容易找到想要的模型。您也可以按名稱搜尋模型。

最近使用的模型現在在模型名稱旁顯示灰色的供應商名稱，讓您可以快速區分來自不同供應商的同名模型。

> **提示**
>
> 您可以在聊天輸入中輸入 `/models` 來快速存取模型。

---

## Chat

### 終端機工具輸出壓縮（Preview）

**設定**：`chat.tools.compressOutput.enabled`

來自 `git diff`、`ls -l` 和 `npm install` 等命令的長終端機輸出可能佔用模型上下文視窗的大部分空間，為您的程式碼和 Agent 的推理留下更少的空間。

當您啟用 `chat.tools.compressOutput.enabled` 設定時，VS Code 會在將這些命令的輸出傳送給模型之前進行後處理。diff 中未變更的大區塊會被摺疊，lockfile 和 snapshot 差異會被丟棄，`ls -l` 會精簡為項目名稱，`npm install` 的進度條、棄用警告和稽核摘要會被去除。

壓縮輸出前會附加簡短的橫幅，讓模型可以看到哪些篩選器被觸發，以及在需要原始文字時如何停用壓縮。

### 終端機命令風險評估（實驗性）

**設定**：`chat.tools.riskAssessment.enabled`

為了幫助您快速決定某個命令是否值得仔細查看，終端機命令確認現在包含一個帶有 AI 生成說明的風險徽章，說明該命令的作用。

每個徽章顯示三個等級之一，並附上針對該特定命令量身打造的一句摘要：

- **Safe**（綠色）：讀取檔案或列印輸出，不進行任何變更。
- **Caution**（橙色）：修改工作區、安裝套件或透過網路傳送資料。
- **Review carefully**（紅色）：執行可能難以或無法復原的操作，例如強制推送至遠端或刪除工作區外的檔案。

![截圖顯示聊天中的終端機命令確認，帶有 AI 生成的風險徽章和命令下方的說明。](https://code.visualstudio.com/assets/updates/1_120/terminal-risk-assessment.webp)

### Claude 和 Copilot CLI 的計畫模式控制

**設定**：`chat.planWidget.inlineEditor.enabled`

當您在 Claude Agent 或 Copilot CLI 中使用計畫模式時，VS Code 會顯示行內計畫控制項，讓您在 Agent 開始執行前審閱和塑造計畫。本次發行對該流程帶來了多項改善：

- **行內編輯計畫**：編輯計畫現在在控制項內的行內編輯器中進行，而不是開啟單獨的編輯器分頁，讓您可以在不失去上下文的情況下迭代計畫。
- **更清楚的回饋模式**：當您對計畫提供回饋時，控制項會顯示更清楚的指示，表明您處於回饋模式，並顯示您迄今已新增的回饋。
- **停用行內編輯器**：透過設定 `chat.planWidget.inlineEditor.enabled` 來退出行內編輯體驗，改為在一般編輯器分頁中編輯。

---

## 語言

### Markdown 差異預覽（Preview）

當您從**原始碼控制**檢視開啟 Markdown 檔案時，可以使用 VS Code 的渲染 Markdown 預覽來查看差異，而非原始碼。

![原始碼控制差異在並排 Markdown 預覽中渲染的截圖。](https://code.visualstudio.com/assets/updates/1_120/md-diff-overview.webp)

這使得發現有意義的變更（例如更新的標題、新區段、修改的圖片或重新組織的清單）變得更加容易，無需逐行心算解析 Markdown 語法。

差異 Markdown 預覽支援並排差異檢視和行內檢視兩種模式。

![原始碼控制差異在行內 Markdown 預覽中渲染的截圖。](https://code.visualstudio.com/assets/updates/1_120/md-diff-inline.webp)

若要試用，從原始碼控制（或任何其他差異編輯器）開啟 Markdown 差異，使用**以其他方式重新開啟編輯器...**（Reopen Editor With...）切換至 Markdown 預覽差異檢視。您也可以透過 `workbench.diffEditorAssociations` 設定，讓差異預設以 Markdown 預覽開啟：

```json
"workbench.diffEditorAssociations": {
  "*.md": "vscode.markdown.preview.editor"
}
```

此功能仍為 Preview，因此您可能會遇到問題。我們認為它對於審閱來自 Agent 或 Pull Request 的文件變更特別有用。

### Markdown 預覽預設值變更

VS Code 的內建 Markdown 預覽已經存在了一段時間，其中一些原始功能已不再像過去那麼必要。在本次迭代中，我們決定預設停用其中兩項功能：

- `markdown.preview.doubleClickToSwitchToEditor`：在預覽中雙擊會切換回原始碼編輯器。使用者經常覺得困惑，因為他們想用雙擊來進行選取。我們現在有「以其他方式重新開啟」等功能，大致取代了此功能。

- `markdown.preview.markEditorSelection`：標記編輯器中目前選取的行。我們認為對現代工作流程來說較不實用。

如果您偏好先前的行為，可以重新啟用這些設定。

### Markdown 路徑補全和驗證的 HTML id 支援

我們的內建 [Markdown 路徑補全](https://code.visualstudio.com/docs/languages/markdown#_path-completions)和[連結驗證](https://code.visualstudio.com/docs/languages/markdown#_link-validation)現在可識別 Markdown 檔案中 HTML 元素的 `id` 屬性。

```html
<div id="install-guide">...</div>

See the [installation steps](#_install-guide) for details.
```

這些 ID 的連結現在會在補全中建議：

![截圖顯示 Markdown 連結中的 ID 屬性補全。](https://code.visualstudio.com/assets/updates/1_120/md-id-completion.webp)

它們也用於[連結驗證](https://code.visualstudio.com/docs/languages/markdown#_link-validation)：

![截圖顯示含有未知 HTML id 的 Markdown 連結的驗證錯誤。](https://code.visualstudio.com/assets/updates/1_120/md-id-validation.webp)

### Markdown 表格的智慧選取

Markdown 表格現在支援基本的[智慧選取](https://code.visualstudio.com/docs/editing/codebasics#_shrinkexpand-selection)。使用**展開選取**（⌃⇧⌘→（Windows、Linux Shift+Alt+Right））將選取範圍從儲存格擴展到所在列，再到整個表格；使用**縮小選取**（⌃⇧⌘←（Windows、Linux Shift+Alt+Left））向下回退。

---

## Proposed API

### Custom editor diffs

全新的 `customEditorDiffs` proposed API 讓自訂編輯器可以使用專屬的差異 UI 來渲染差異。這是驅動新 [Markdown 差異預覽](#markdown-差異預覽preview)的底層技術，它為文字差異對底層原始碼無用的情況開啟了更好的比較體驗。

自訂編輯器供應商可以透過在 `CustomReadonlyEditorProvider` 或 `CustomTextEditorProvider` 上實作以下一種或兩種方法來選擇加入：

- `resolveCustomEditorInlineDiff(documents, webviewPanel, token)`：在單一 webview 中渲染差異，擴充功能可存取原始文件和修改後的文件。

- `resolveCustomEditorSideBySideDiff(documents, webviewPanels, token)`：使用兩個 webview 渲染差異，每邊一個，VS Code 負責協調佈局和捲動同步。

結合 [`diffEditorPriority`](#自訂編輯器的差異和合併分開設定優先順序)，擴充功能現在可以完全控制其自訂編輯器是否處理差異以及如何呈現差異。請參閱 [issue #138525](https://github.com/microsoft/vscode/issues/138525) 追蹤進度並提供回饋。

### 自訂編輯器的差異和合併分開設定優先順序

自訂編輯器擴充功能現在可以為編輯、差異比較和合併檔案類型設定不同的預設優先順序。`customEditors` 貢獻接受兩個新的可選欄位 `diffEditorPriority` 和 `mergeEditorPriority`，與現有的 `priority` 並列。

```json
"contributes": {
  "customEditors": [
    {
      "viewType": "myExtension.editor",
      "displayName": "My Custom Editor",
      "selector": [
        { "filenamePattern": "*.custom" }
      ],
      "priority": "default",
      "diffEditorPriority": "option",
      "mergeEditorPriority": "option"
    }
  ]
}
```

以上貢獻使得開啟 `*.custom` 檔案時使用自訂編輯器，但從原始碼控制開啟差異時使用一般文字差異檢視。

此 API 仍為 proposed。請試用並在 [issue #292379](https://github.com/microsoft/vscode/issues/292379) 中分享回饋。

### Document diff

全新的 `documentDiff` proposed API 透過 `workspace.getTextDiff(original, modified, options?)` 將 VS Code 的內建差異演算法暴露給擴充功能。它回傳行級變更的串流非同步可迭代物件，加上包含摘要資訊（identical、may-be-incomplete 和選用的移動偵測）的 `complete` Promise。每個變更上包含內部字元級範圍。

這對自訂差異編輯器（參見 [Custom editor diffs](#custom-editor-diffs)）特別有用，使其可以渲染與內建編輯器完全相同的差異，而無需自帶演算法。

```typescript
const diff = vscode.workspace.getTextDiff(originalDoc, modifiedDoc, {
  ignoreTrimWhitespace: true,
  computeMoves: false
});

for await (const change of diff.changes) {
  // change.originalRange, change.modifiedRange, change.innerChanges
}

const { identical, mayBeIncomplete, moves } = await diff.complete;
```

在 [issue #315174](https://github.com/microsoft/vscode/issues/315174) 中追蹤進度並提供回饋。

---

## 擴充功能的貢獻

### GitHub Pull Requests

[GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) 擴充功能持續有更多進展，該擴充功能讓您能夠處理、建立和管理 Pull Request 和 Issue。新功能包括：

- 透過複製/貼上和上傳按鈕將圖片上傳至 Pull Request 留言。
- 以 worktree 方式簽出 Pull Request 時，資料夾名稱更具描述性。
- `"githubIssues.issueBranchTitle"` 現在支援 `${issueType}` 範本變數。

查閱擴充功能 [0.144.0 版本的變更日誌](https://github.com/microsoft/vscode-pull-request-github/blob/main/CHANGELOG.md#01440)以了解此版本的所有內容。

---

## 已棄用的功能和設定

### 本次發行的新棄用項目

（本次無新增棄用項目。）

### 即將棄用的項目

（本次無即將棄用的項目。）

---

## 重要修正

- [microsoft/vscode #314545](https://github.com/microsoft/vscode/issues/314545) 在整合式瀏覽器的 localhost 目標中包含 All-Interfaces 連結

---

## 感謝

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

對 `vscode` 的貢獻：

- [@damonxue (DamonXue)](https://github.com/damonxue)：右鍵點擊非活動編輯器分頁時「Add File to Chat」無作用 [PR #315197](https://github.com/microsoft/vscode/pull/315197)
- [@davidwengier (David Wengier)](https://github.com/davidwengier)：更新 Razor 儲存庫的 repository 和 path [PR #313011](https://github.com/microsoft/vscode/pull/313011)
- [@Dmitriusan](https://github.com/Dmitriusan)：修正子檔案中的 gitignore 否定未覆寫父/全域規則的問題 [PR #300613](https://github.com/microsoft/vscode/pull/300613)
- [@EhabY (Ehab Younes)](https://github.com/EhabY)：透過 keepalive 逾時偵測中斷的連線 [PR #310131](https://github.com/microsoft/vscode/pull/310131)
- [@JeffreyCA](https://github.com/JeffreyCA)
  - 更新 Azure Developer CLI (azd) 的 Fig spec [PR #308613](https://github.com/microsoft/vscode/pull/308613)
  - 整合式終端機 — 修正過期的 OSC 8 連結懸停工具提示問題 [PR #309539](https://github.com/microsoft/vscode/pull/309539)
- [@kevin-m-kent](https://github.com/kevin-m-kent)
  - 在 response.* 事件和子代理迴圈中發出 parentRequestId [PR #314309](https://github.com/microsoft/vscode/pull/314309)
  - 為聊天請求新增 X-Interaction-Type 標頭和 requestKind 遙測屬性 [PR #312262](https://github.com/microsoft/vscode/pull/312262)
  - 發佈穩定版的 symbol 工具描述 [PR #315686](https://github.com/microsoft/vscode/pull/315686)
- [@Larsjep (Lars Jeppesen)](https://github.com/Larsjep)：修正 https://github.com/microsoft/vscode/issues/291188 [PR #314713](https://github.com/microsoft/vscode/pull/314713)
- [@n-gist (n-gist)](https://github.com/n-gist)：保證 TreeDataProvider.getChildren() 的回傳值不被 vscode 修改 [PR #306955](https://github.com/microsoft/vscode/pull/306955)
- [@Pengkun-ZHU (pzhu)](https://github.com/Pengkun-ZHU)：自訂延遲時間功能 [PR #298934](https://github.com/microsoft/vscode/pull/298934)
- [@pranavvaid-ac](https://github.com/pranavvaid-ac)
  - 在延遲錨點解析後更新聊天行內參考 [PR #314281](https://github.com/microsoft/vscode/pull/314281)
  - 使用 tree-sitter 備援改善連結符號錨點 [PR #314864](https://github.com/microsoft/vscode/pull/314864)
- [@ruryu (ruryu)](https://github.com/ruryu)：修正（agentHost）：await dbClose 以解決不穩定的工作階段資料庫測試 [PR #313810](https://github.com/microsoft/vscode/pull/313810)
- [@ShehabSherif0 (Shehab Sherif)](https://github.com/ShehabSherif0)：修正範圍偵測中不正確的 inspect 屬性使用 [PR #301472](https://github.com/microsoft/vscode/pull/301472)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)：修正 utilityProcessWorkerMainService 中的記憶體洩漏 [PR #294005](https://github.com/microsoft/vscode/pull/294005)
- [@Tyriar (Daniel Imms)](https://github.com/Tyriar)
  - 將模糊選項放入 interface [PR #313953](https://github.com/microsoft/vscode/pull/313953)
  - 移除未使用的 export const [PR #315244](https://github.com/microsoft/vscode/pull/315244)
- [@yemohyleyemohyle](https://github.com/yemohyleyemohyle)：回應成功 GDPR 封鎖 [PR #315128](https://github.com/microsoft/vscode/pull/315128)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)：在終端機 quickpick 篩選比對中去除 codicons [PR #313197](https://github.com/microsoft/vscode/pull/313197)

對 `vscode-pull-request-github` 的貢獻：

- [@MaxDNG (Maxime Guitet)](https://github.com/MaxDNG)：修正：重新指定上移目錄子項的父級以確保正確的核取方塊重新整理 [PR #8679](https://github.com/microsoft/vscode-pull-request-github/pull/8679)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agents window | Agents 視窗 |
| agent harness | Agent 工具鏈 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Chat | 聊天 |
| context window | 上下文視窗 |
| custom editor | 自訂編輯器 |
| diff | 差異 |
| extension | 擴充功能 |
| inline | 行內 |
| isolation mode | 隔離模式 |
| lockfile | lockfile |
| Markdown preview | Markdown 預覽 |
| model picker | 模型選擇器 |
| plan mode | 計畫模式 |
| plugin | 外掛 |
| Proposed API | Proposed API |
| risk assessment | 風險評估 |
| session | 工作階段 |
| smart select | 智慧選取 |
| Source Control | 原始碼控制 |
| subagent | 子代理 |
| terminal | 終端機 |
| thinking effort | 思考力度 |
| token | Token |
| workspace | 工作區 |
| worktree | worktree |

*資料來源：[Visual Studio Code 1.120 發行說明](https://code.visualstudio.com/updates/v1_120)*
