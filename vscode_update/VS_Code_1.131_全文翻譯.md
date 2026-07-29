# Visual Studio Code 1.131

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 7 月 29 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.131.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.131.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.131.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.131.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.131.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.131.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.131.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.131.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.131.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.131 版本。本次發行帶來對執行中子代理更多的可見性、跨工作台的內建聽寫功能，以及全新的混合式 Markdown 編輯器。

- [**子代理**](#關於執行中子代理的更多資訊agents-視窗)：無需開啟對話即可查看執行中子代理的模型、經過時間和使用中的工具呼叫。
- [**內建聽寫（實驗性）**](#跨-vs-code-的內建聽寫實驗性)：在聊天、編輯器和終端機中聽寫，無需安裝 Speech 擴充功能。
- [**混合式 Markdown 編輯器（實驗性）**](#混合式-markdown-編輯器實驗性)：在 Agents 視窗中檢視、編輯 Markdown 檔案，並新增 Agent 可據以行動的留言。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

如同我們在過去幾個版本中提到的，我們正圍繞 agent host 重新架構 Agent 工作階段在 VS Code 中的運作方式——agent host 是一個專用程序，根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）執行 Copilot、Claude 和 Codex 等 Agent 工具鏈。因為工作階段存在於自己的程序中，同一工作階段可以同時從多個 VS Code 視窗連線和渲染。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，這意味著其行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host 並漸進式地向使用者推出。若要加入，請啟用 `chat.agentHost.enabled`（此設定可由您的組織管理。請聯繫您的管理員以變更。），然後從工具鏈下拉選單中選取 agent host 工具鏈。以下截圖展示如何在編輯器視窗中選取 agent host 上的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_131/agent-host-harness-dropdown-editor.webp)

您可以在我們的 [VS Code Agent Host 架構文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)中了解更多。如果您有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

### 關於執行中子代理的更多資訊（Agents 視窗）

處理複雜任務時，Agent 可以將任務委派給[子代理](https://code.visualstudio.com/docs/agents/concepts/agents#_subagents)，讓它們在自己的上下文視窗中並行執行。您現在可以快速查看執行中的子代理正在做什麼，而無需開啟其對話。在 Agents 視窗中，主對話會為每個執行中的子代理顯示以下資訊：

- 子代理使用的模型
- 子代理已執行多久
- 子代理正在主動呼叫的工具

選取執行中的子代理可在另一個聊天中開啟其對話，您可以在那裡審閱其完整進度，同時父對話仍保持可用。

---

## Chat

### VS Code 寵物（實驗性）

VS Code 中發現了一隻全新的高度實驗性寵物！在聊天中輸入 `/vscode-pet` 來認識您的新夥伴。

---

## 編輯器體驗

### 跨 VS Code 的內建聽寫（實驗性）

**設定**：`dictation.enabled`（此設定可由您的組織管理。請聯繫您的管理員以變更。）、`dictation.showTranscript`、`dictation.experimental.llmCleanup`

若要在 VS Code 中使用聽寫，您不再需要安裝 VS Code Speech 擴充功能。內建的轉錄服務可在聊天輸入、文字編輯器和整合式終端機中運作，各介面均有適合的即時文字和控制項。單一語音工作階段和麥克風選取在這三個介面之間共用，可防止重疊錄音，並將聽寫的文字保持在預期的位置。

內建聽寫使用私密的離線 Nemotron 模型。該模型在首次使用時下載，並將音訊保留在您的裝置上。

使用 `dictation.showTranscript` 控制在聽寫時是否顯示即時逐字稿。啟用 `dictation.experimental.llmCleanup` 後，Copilot 會在您說話時透過新增格式和移除填充詞來優化您的逐字稿。逐字稿文字會被發送至語言模型進行清理。如果清理不可用，VS Code 會保留原始逐字稿。

#### 平台支援

以下平台支援內建聽寫：

- Windows x64 和 Arm64
- Apple silicon 上的 macOS
- 具有 glibc 2.34 或更新版本的 Linux x64 和 Arm64
- 遠端工作區（因為轉錄在本機 VS Code 客戶端上執行）

以下平台目前不支援內建聽寫：

- VS Code for the Web
- Intel 架構的 Mac、32 位元系統和 Arm32 系統

對更多平台和語言的支援仍在進行中。

### 混合式 Markdown 編輯器（實驗性）

**設定**：`workbench.editor.markdownDefaultEditorInAgentsWindow`

在本次發行中，我們在 Agents 視窗中引入了全新的混合式 Markdown 編輯器。它讓您可以檢視 Markdown 檔案、就地編輯它們，並新增 Agent 可以據以行動的留言。

透過使用 **Reopen Editor With**，您可以在文字編輯器和這個新的 Markdown 編輯器之間切換，在 Agents 視窗和編輯器視窗中皆可。

---

## 協助工具

### 對終端機螢幕閱讀器更新的更多控制

**設定**：`terminal.integrated.accessibleViewPreserveCursorPosition`

螢幕閱讀器使用者可以按照自己的節奏閱讀終端機輸出，同時命令持續產生輸出。將 `terminal.integrated.accessibleViewPreserveCursorPosition` 設為 `always` 可在終端機[可存取檢視](https://code.visualstudio.com/docs/configure/accessibility/accessibility#_accessible-view)中保留游標位置，包括新內容到達時。現有的 `true` 和 `false` 值仍可繼續運作。

終端機即時更新也改用非中斷式的 ARIA 狀態公告，而非強制性警示。輸出仍可供螢幕閱讀器讀取，而不會反覆中斷其他語音。

---

## 終端機

### 控制終端機調整大小尺寸疊加層

**設定**：`terminal.integrated.resizeDimensionsOverlay.enabled`

如果您在調整終端機大小時覺得欄×列的疊加層令人分心，可以使用 `terminal.integrated.resizeDimensionsOverlay.enabled` 設定將其停用。此疊加層預設保持啟用，設定變更會立即套用至開啟的終端機，無需重新啟動。

![截圖顯示終端機調整大小時，在終端機中間出現的目前欄數和列數疊加層。](https://code.visualstudio.com/assets/updates/1_131/terminal-resize-overlay.webp)

---

## 語言

### Python

在推出達到 100% 使用者後，Python Environments 成為 VS Code Stable 和 Insiders 中的預設環境管理體驗。請查看 [Python Environments 推出詳情與追蹤](https://github.com/microsoft/vscode-python-environments/issues/581)。

Python 專案啟動更快，且花在重新整理環境的時間更少。Conda 探索延後至需要時才進行、並行環境掃描被合併，且 Pylance 可以在完整重新整理繼續進行時使用最後已知的直譯器。_[#1600：延遲註冊 Conda 管理員](https://github.com/microsoft/vscode-python-environments/pull/1600)、[#1598：合併具有相同金鑰的並行原生尋找器重新整理](https://github.com/microsoft/vscode-python-environments/pull/1598)、[#1607：在 getEnvironment 逾時時回傳最後已知的環境](https://github.com/microsoft/vscode-python-environments/pull/1607)_

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@accnops (Arthur Cnops)](https://github.com/accnops)：voice：將被動 ptt_start 作為正式的免持旁白修正 [PR #326405](https://github.com/microsoft/vscode/pull/326405)
- [@bwateratmsft (Brandon Waterloo [MSFT])](https://github.com/bwateratmsft)：新增 when 子句上下文金鑰以偵測擴充功能是否已安裝並啟用 [PR #326814](https://github.com/microsoft/vscode/pull/326814)
- [@Kaidesuyoo (Kaidesuyo)](https://github.com/Kaidesuyoo)：修正由現代化 UI 樣式造成的效能退化 [PR #325985](https://github.com/microsoft/vscode/pull/325985)
- [@mirimadahmed (Mir)](https://github.com/mirimadahmed)：修正語音插話退化 [PR #326611](https://github.com/microsoft/vscode/pull/326611)
- [@piyushmadan (Piyush Madan)](https://github.com/piyushmadan)：在系列回退之前以確切的 CAPI id 解析執行子代理模型 [PR #324859](https://github.com/microsoft/vscode/pull/324859)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：abstractTaskService 中的記憶體洩漏 [PR #326934](https://github.com/microsoft/vscode/pull/326934)
  - 修正：abstractRuntimeExtensionsEditor 中的記憶體洩漏 [PR #326890](https://github.com/microsoft/vscode/pull/326890)
  - 修正：terminalProcessManager 中的記憶體洩漏 [PR #326930](https://github.com/microsoft/vscode/pull/326930)
  - 修正：debugModel 中的記憶體洩漏 [PR #327047](https://github.com/microsoft/vscode/pull/327047)
  - 修正：terminalService 中的記憶體洩漏 [PR #327156](https://github.com/microsoft/vscode/pull/327156)
  - 修正：mainThreadTerminalService 中的記憶體洩漏 [PR #327155](https://github.com/microsoft/vscode/pull/327155)
- [@SixFive7 (Jori Huisman)](https://github.com/SixFive7)：為 Windows 工作列跳躍清單遵循 Explorer 的 `minItems` [PR #318117](https://github.com/microsoft/vscode/pull/318117)
- [@soreavis](https://github.com/soreavis)：Git - 為 Open Changes / Open File 解析使用中的筆記本 [PR #326468](https://github.com/microsoft/vscode/pull/326468)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Accessible View | 可存取檢視 |
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| agent host | agent host |
| Agents window | Agents 視窗 |
| ARIA | ARIA |
| Conda | Conda |
| context window | 上下文視窗 |
| Copilot SDK | Copilot SDK |
| dictation | 聽寫 |
| filler words | 填充詞 |
| harness | 工具鏈 |
| hybrid Markdown editor | 混合式 Markdown 編輯器 |
| interpreter | 直譯器 |
| jump list | 跳躍清單 |
| memory leak | 記憶體洩漏 |
| Nemotron | Nemotron |
| overlay | 疊加層 |
| Pylance | Pylance |
| Python Environments | Python Environments |
| screen reader | 螢幕閱讀器 |
| session | 工作階段 |
| speech session | 語音工作階段 |
| subagent | 子代理 |
| terminal | 終端機 |
| token | Token |
| transcript | 逐字稿 |
| transcription | 轉錄 |
| workbench | 工作台 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.131 發行說明](https://code.visualstudio.com/updates/v1_131)*
