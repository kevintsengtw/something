# Visual Studio Code 1.134

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 8 月 19 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.134.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.134.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.134.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.134.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.134.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.134.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.134.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.134.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.134.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.134 版本。本次發行協助您跨視窗工作、將相關聊天並排整理，並更快速地瀏覽長對話。

- [**並排聊天**](#工作階段中聊天的格線佈局)：將相關聊天和子代理聊天排列成群組，以便更容易比較。
- [**提示時間軸**](#提示時間軸)：快速在提示之間導覽並審閱其檔案變更。
- [**在聊天中尋找**](#在聊天中尋找)：輕鬆在完整對話中搜尋文字。
- [**預覽 HTML 檔案**](#預設在整合式瀏覽器中開啟-html-檔案)：透過將整合式瀏覽器設為其預設編輯器，直接在 VS Code 中預覽本機 HTML 檔案。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

Agent host 讓您可以從多個 VS Code 視窗連線至同一個 Agent 工作階段。它根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）在專用程序中執行 Agent 工具鏈。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，使 Agent 的行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host。以下截圖顯示在編輯器視窗中為 agent host 選取的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_134/agent-host-harness-dropdown-editor.webp)

您可以在我們的 [VS Code Agent Host 文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)中了解更多。如果您有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

### 工作階段中聊天的格線佈局

透過將聊天排列成水平或垂直群組，讓相關對話保持可見。將聊天或子代理聊天拖曳至群組中，以比較結果或並排監控工作。當您返回工作階段或重新載入視窗時，VS Code 會還原聊天群組佈局和焦點。

建立側邊聊天可在目前聊天旁邊開啟新的對話。

將子代理聊天拖放至群組中，即可與目前聊天並排檢視。

您也可以在 **Chats** 選擇器中以 Alt+選取聊天，將其開啟至側邊。

### 側邊窗格佈局改善

**設定**：`sessions.layout.singlePaneDetailPanel` 和 `workbench.editor.showTabs`

單一窗格佈局將工作階段詳細資料和編輯器保持在聊天旁的共用分頁列中。本次發行讓此佈局更容易控制：

- 佈局遵循 `workbench.editor.showTabs` 設定。多個分頁保持可見，而 `single` 和 `none` 值則使用緊湊的單一標題標頭。

  ![截圖顯示 Agents 視窗中緊湊的單一標題標頭。](https://code.visualstudio.com/assets/updates/1_134/agents-window-single-tab.webp)

- 文字檔案編輯器使用與 Changes 編輯器相同的標頭結構，標頭中含有檔案階層連結。

- 側邊窗格在您切換工作階段時保持其大小和可見性，避免非預期的佈局變動。

啟用 `sessions.layout.singlePaneDetailPanel` 並重新載入視窗以使用此佈局。

### 提示時間軸

**設定**：`sessions.chatTimeline.display`

長時間的 Agent 工作階段可能讓您難以找到較早的提示，並辨識哪些提示變更了檔案。

Agents 視窗在逐字稿邊界中包含一條時間軸。每個圓點代表您的一個提示，醒目標示的圓點標記您目前的位置。懸停在時間軸上可檢視您的提示，然後選取其中一個以跳至該處。對於變更了檔案的提示，清單會顯示新增和移除的行數，並讓您直接開啟這些變更以供審閱。

使用 `sessions.chatTimeline.display` 設定將時間軸顯示在捲軸旁（`ruler`）或隱藏它（`off`）。

---

## Chat

### 在聊天中尋找

先前要從長對話中較早的部分尋找資訊，必須捲動瀏覽整份逐字稿。現在可使用 ⌘F（Windows、Linux 為 Ctrl+F）在 Chat 檢視、聊天編輯器和 Agents 視窗中搜尋對話。

搜尋涵蓋整個對話，即使是目前未渲染在畫面上的內容。當您在符合項目之間移動時，VS Code 會將每個符合項目捲動至檢視中，並在摺疊的工作摘要包含該符合項目時將其展開。您也可以進行大小寫比對、全字比對，或使用規則運算式。

---

## 編輯器體驗

### 從分頁關閉其他編輯器

不使用分頁的右鍵選單即可只保留一個編輯器開啟。按住 Alt 可將每個編輯器分頁上的關閉操作變更為 **Close Other Editors**，然後在您想保留的分頁上選取該操作。

### 預設在整合式瀏覽器中開啟 HTML 檔案

**設定**：`workbench.editorAssociations`

如果您經常預覽本機 HTML 檔案而非編輯它們，請將整合式瀏覽器設為其預設編輯器。您可以使用 `workbench.editorAssociations` 設定或從編輯器標頭來設定此行為。

整合式瀏覽器提供與獨立瀏覽器分頁相同的功能，同時保持與 HTML 檔案的關聯。為了保留此關聯，連結和其他導覽會在新分頁中開啟。

---

## 感謝

對 `vscode` 的貢獻：

- [@a1exmozz](https://github.com/a1exmozz)：agentHost：向 CTS 發出使用者訊息遙測 [PR #329961](https://github.com/microsoft/vscode/pull/329961)
- [@abmahdy (Ahmed Mahdy)](https://github.com/abmahdy)：為終端機完成通知保留指令 [PR #330570](https://github.com/microsoft/vscode/pull/330570)
- [@benelog (Sanghyuk Jung)](https://github.com/benelog)：修正 Copilot 提示文字中重複的字詞 [PR #328961](https://github.com/microsoft/vscode/pull/328961)
- [@cipheraxat (Akshat Anand)](https://github.com/cipheraxat)：現代化 UI 分頁：保留關閉按鈕欄位以免覆蓋檔案名稱（修正 #329605）[PR #330754](https://github.com/microsoft/vscode/pull/330754)
- [@jadefr (Jade Ferreira Vieira)](https://github.com/jadefr)：功能／Alt 點擊關閉其他分頁 [PR #328975](https://github.com/microsoft/vscode/pull/328975)
- [@martincheck (Martin Check)](https://github.com/martincheck)：chat：避免在 read_file 中分割代理對 [PR #331005](https://github.com/microsoft/vscode/pull/331005)
- [@marvinroger (Marvin ROGER)](https://github.com/marvinroger)：修正因未定義的 `document.queryCommandSupported` 造成的當機 [PR #330298](https://github.com/microsoft/vscode/pull/330298)
- [@mirimadahmed (Mir)](https://github.com/mirimadahmed)：voice：遵循 send_to_chat 上的 new_session 旗標 [PR #330859](https://github.com/microsoft/vscode/pull/330859)
- [@Shaurav-Vora (Shaurav Vora)](https://github.com/Shaurav-Vora)：共同撰寫：為聊天窗格和編輯器實作 Ctrl+F 尋找小工具支援 [PR #330340](https://github.com/microsoft/vscode/pull/330340)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：擴充功能檢視中的記憶體洩漏 [PR #330210](https://github.com/microsoft/vscode/pull/330210)
  - 修正：原始檔控制檢視中的記憶體洩漏 [PR #330241](https://github.com/microsoft/vscode/pull/330241)
  - 修正：程式碼操作中的記憶體洩漏 [PR #330142](https://github.com/microsoft/vscode/pull/330142)
  - 修正：搜尋檢視中的記憶體洩漏 [PR #330240](https://github.com/microsoft/vscode/pull/330240)
  - 修正：聊天小工具中的記憶體洩漏 [PR #326876](https://github.com/microsoft/vscode/pull/326876)
  - 修正：參考檢視中的記憶體洩漏 [PR #330191](https://github.com/microsoft/vscode/pull/330191)
  - 修正：搜尋結果資料夾比對中的記憶體洩漏 [PR #331012](https://github.com/microsoft/vscode/pull/331012)
- [@yzxcj797](https://github.com/yzxcj797)：docs：修正 copilot 擴充功能 README 中失效的 nes-video.gif 連結 [PR #330992](https://github.com/microsoft/vscode/pull/330992)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們感謝大家在新功能準備好後儘早試用。請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| agent host | agent host |
| Agents window | Agents 視窗 |
| breadcrumbs | 階層連結 |
| chat group | 聊天群組 |
| Copilot SDK | Copilot SDK |
| editor association | 編輯器關聯 |
| extension | 擴充功能 |
| find widget | 尋找小工具 |
| grid layout | 格線佈局 |
| gutter | 邊界 |
| harness | 工具鏈 |
| header | 標頭 |
| Integrated Browser | 整合式瀏覽器 |
| match case | 大小寫比對 |
| match whole words | 全字比對 |
| memory leak | 記憶體洩漏 |
| prompt | 提示 |
| prompt timeline | 提示時間軸 |
| regular expression | 規則運算式 |
| ruler | 捲軸旁（尺規） |
| session | 工作階段 |
| side chat | 側邊聊天 |
| side pane | 側邊窗格 |
| single-pane layout | 單一窗格佈局 |
| subagent | 子代理 |
| surrogate pair | 代理對 |
| tab bar | 分頁列 |
| terminal | 終端機 |
| transcript | 逐字稿 |
| work summary | 工作摘要 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.134 發行說明](https://code.visualstudio.com/updates/v1_134)*
