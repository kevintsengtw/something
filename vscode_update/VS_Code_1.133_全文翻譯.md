# Visual Studio Code 1.133

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 8 月 12 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.133.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.133.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.133.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.133.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.133.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.133.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.133.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.133.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.133.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.133 版本。本次發行讓您在 Claude 工作階段中有更多彈性、讓長聊天更容易追蹤，並在您工作時重新整理本機 HTML 預覽。

- [**變更 Claude 工作階段的模型供應商**](#在-claude-工作階段中混用-anthropic-和-copilot-模型)：在回合之間切換供應商，無需重新設定 agent host。
- [**無需 GitHub 登入的 Agents 視窗**](#無需-github-登入即可開啟-agents-視窗實驗性)：當 GitHub 登入不可用時，使用您現有的 API 金鑰搭配 Claude。
- [**自動重新載入 HTML 檔案**](#在整合式瀏覽器中自動重新載入-html-檔案)：立即預覽 HTML 變更，無需手動重新整理。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

Agent host 讓您可以從多個 VS Code 視窗連線至同一個 Agent 工作階段。它根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）在專用程序中執行 Agent 工具鏈。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，使其行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host。以下截圖展示如何在編輯器視窗中選取 agent host 上的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_133/agent-host-harness-dropdown-editor.webp)

您可以在我們的 [VS Code Agent Host 文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)中了解更多。如果您有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

### 在 Claude 工作階段中混用 Anthropic 和 Copilot 模型

先前，Claude 工作階段完全透過您的 GitHub Copilot 訂閱或 Claude 的現有設定（例如 API 金鑰）執行。切換供應商需要重新設定 agent host。

現在，模型選擇器會顯示這兩個群組，讓您可以在回合之間切換供應商。您選取的模型會用於下一個回合。**Anthropic** 下的模型會向您的 API 金鑰計費，而 **Copilot** 下的模型則使用您的 Copilot 訂閱。

![截圖顯示 Claude 工作階段中的模型選擇器，模型分別歸類在 Anthropic 標題和 Copilot 標題下。](https://code.visualstudio.com/assets/updates/1_133/agent-host-claude-model-picker.webp)

### 無需 GitHub 登入即可開啟 Agents 視窗（實驗性）

**設定**：`chat.agentHost.allowSignedOutWhenUsable`

[Agents 視窗](https://code.visualstudio.com/docs/agents/run/agents-window)過去開啟時會出現一個您無法關閉的 GitHub 登入提示。這阻擋了所有機器無法連線至 github.com 的人，以及所有不與 GitHub 互動的人。已經用 API 金鑰設定好 Claude、不需要 GitHub 登入的使用者也必須經過這個額外步驟。

啟用此設定即可在未登入 GitHub 的情況下開啟 Agents 視窗。GitHub 驗證接著會關聯至個別的 Agent 或模型，而非 Agents 視窗。在本次發行中，此行為僅支援 Claude。對搭配您自有模型金鑰的 Copilot 以及 Codex 的支援計劃於未來版本推出。

![截圖顯示在登出 GitHub 的情況下開啟的 Agents 視窗，帶有一則寫著「We've discovered your existing Claude configuration」的通知和一個 Sign in to GitHub 按鈕。](https://code.visualstudio.com/assets/updates/1_133/agent-host-signed-out-claude.webp)

---

## Chat

### 提示的黏性捲動

**設定**：`chat.stickyScroll.enabled`

當您捲動瀏覽長對話時，可能會搞不清楚某個回應屬於哪個提示。您已捲動經過的提示現在會釘選在聊天頂部，類似於編輯器中的黏性捲動。

釘選的提示會顯示其在對話中的位置。選取它可跳回該提示，或使用它旁邊的上一個和下一個按鈕來逐一瀏覽您的提示。

---

## 編輯器體驗

### 在整合式瀏覽器中自動重新載入 HTML 檔案

**設定**：`workbench.browser.autoReloadOnFileChange`

當本機 HTML 檔案在[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)中開啟時，該檔案在磁碟上變更時現在會自動重新整理。

這有助於您立即看到 Agent 的編輯或您自己儲存的變更。您可以為個別的瀏覽器分頁切換自動重新載入，並使用 `workbench.browser.autoReloadOnFileChange` 設定來設定預設值。

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@accnops (Arthur Cnops)](https://github.com/accnops)
  - voice：說明語音連線失敗的原因 [PR #329453](https://github.com/microsoft/vscode/pull/329453)
  - 將語音模式的語音重新命名為核准的名稱 [PR #329576](https://github.com/microsoft/vscode/pull/329576)
- [@benelog (Sanghyuk Jung)](https://github.com/benelog)：修正 notebook.cellToolbarLocation 設定描述中重複的字詞 [PR #328957](https://github.com/microsoft/vscode/pull/328957)
- [@Bosphoramus (Tony)](https://github.com/Bosphoramus)：修正：在標題列隱藏時為現代化 UI 浮動面板新增頂部間距 [PR #328688](https://github.com/microsoft/vscode/pull/328688)
- [@bstee615 (Benjamin Steenhoek)](https://github.com/bstee615)：為 diffpatch 提示新增選用的已拒絕編輯記憶 [PR #327367](https://github.com/microsoft/vscode/pull/327367)
- [@karthikkatu (Karthikeyan M)](https://github.com/karthikkatu)：當預覽的本機檔案變更時自動重新載入整合式瀏覽器 [PR #324618](https://github.com/microsoft/vscode/pull/324618)
- [@lfraleigh (Lori Fraleigh)](https://github.com/lfraleigh)：將遺漏的 Azure SDK for Go 模組新增至 GoModulesToLookFor [PR #322786](https://github.com/microsoft/vscode/pull/322786)
- [@mateusabelli (Mateus Abelli)](https://github.com/mateusabelli)：更新 Copilot 擴充功能連結 [PR #329229](https://github.com/microsoft/vscode/pull/329229)
- [@rfeltis (Ralph Feltis)](https://github.com/rfeltis)：新增 Agents 視窗啟動 A/A 實驗觸發器 [PR #328454](https://github.com/microsoft/vscode/pull/328454)
- [@ShehabSherif0 (Shehab Sherif)](https://github.com/ShehabSherif0)：修正時間軸 pageSize 計算中的運算子優先順序 [PR #303258](https://github.com/microsoft/vscode/pull/303258)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)：修正：PerfModelContentProvider 中的記憶體洩漏 [PR #328581](https://github.com/microsoft/vscode/pull/328581)
- [@SVOG23 (Suraj Vaghela)](https://github.com/SVOG23)：docs：修正 Claude 聊天工作階段 AGENTS.md 中過時的相對路徑 [PR #327612](https://github.com/microsoft/vscode/pull/327612)
- [@vscodebot-pr (VS Code PR Bot)](https://github.com/vscodebot-pr)：修正：防範測試裝飾中的過時行號（修正 #328988）[PR #328994](https://github.com/microsoft/vscode/pull/328994)
- [@Xaena53 (Bedirhan ÇETİN)](https://github.com/Xaena53)：重構：共用尋找輸入的切換導覽 [PR #329128](https://github.com/microsoft/vscode/pull/329128)

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
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| agent host | agent host |
| Agents window | Agents 視窗 |
| API key | API 金鑰 |
| auto-reload | 自動重新載入 |
| Copilot SDK | Copilot SDK |
| extension | 擴充功能 |
| harness | 工具鏈 |
| Integrated Browser | 整合式瀏覽器 |
| memory leak | 記憶體洩漏 |
| model picker | 模型選擇器 |
| model provider | 模型供應商 |
| prompt | 提示 |
| session | 工作階段 |
| sign-in | 登入 |
| sticky scroll | 黏性捲動 |
| subscription | 訂閱 |
| terminal | 終端機 |
| turn | 回合 |
| Voice Mode | 語音模式 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.133 發行說明](https://code.visualstudio.com/updates/v1_133)*
