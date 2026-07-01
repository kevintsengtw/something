# Visual Studio Code 1.127

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 7 月 1 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.127.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.127.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.127.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.127.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.127.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.127.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.127.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.127.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.127.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.127 版本。本次發行帶來可在瀏覽器中建置和測試 Web 應用程式的 Agent、更安全的逐站瀏覽，以及保持繁忙 Agent 工作階段井然有序的新方式。

- [**Agent 瀏覽器工具**](#agent-工具正式可用)：讓 Agent 開啟頁面、擷取截圖並點擊驗證自身工作，現已正式可用。
- [**逐站瀏覽器權限**](#攝影機位置裝置等)：授予頁面存取攝影機、位置、裝置等權限，每個站台單獨提示。
- [**Agent 工作階段**](#使用群組組織工作階段)：將相關工作階段分組，並透過拖放排列繁忙的 Agents 視窗。
- [**聊天輸入橫幅**](#聊天輸入橫幅)：在不離開對話的情況下對失敗的 CI 檢查和傳入的 PR 留言採取行動。
- [**子代理點數**](#子代理點數)：懸停子代理可查看它處理的工作成本。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agents 視窗（Preview）

[Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)是一個專用的輔助視窗，針對跨專案和機器探索、迭代和審閱 Agent 工作階段進行最佳化。本次發行帶來組織工作階段清單和管理繁忙工作階段清單的新方式。

#### 使用群組組織工作階段

當您同時執行多個 Agent 工作階段時，工作階段清單可能快速增長且難以掃視。您現在可以將工作階段清單組織成群組，將相關工作階段放在一起。建立您自己的自訂群組，並摺疊群組標題以整理清單並專注於重要的內容。

每個群組也提供快速操作：您可以直接在群組中啟動新工作階段，或一鍵將其所有工作階段標記為完成。

#### 工作階段清單中的拖放

工作階段清單現在支援拖放以進一步組織您的工作階段：

- 透過向上或向下拖曳來重新排序工作階段
- 拖曳工作階段群組和工作區標題以重新排列清單
- 將工作階段拖曳至群組以將其加入該群組
- 將工作階段放至 **Pinned** 區段以釘選它
- 選取多個工作階段以批次移動

#### 聊天輸入橫幅

當編碼 Agent 工作階段有開放的 Pull Request 時，Agents 視窗會直接在聊天輸入上方顯示橫幅，讓您在工作的地方對失敗的檢查和傳入的回饋採取行動。每個橫幅提供單一操作來修正或查看問題，無需離開您的對話：

- **CI 失敗**：當 Pull Request 上的檢查失敗時，橫幅顯示失敗的檢查數量（例如「2 of 5 checks failed」），並提供快速操作：**Fix Checks** 啟動 Agent 修正，**Reveal Checks** 在 Changes 檢視中開啟失敗的檢查。

- **Pull Request 留言**：當新的審閱留言傳入時，橫幅顯示留言數量並提供操作：**Address Comments** 將它們交給 Agent，**Reveal Comments** 在編輯器中開啟。

#### 引導導覽（實驗性）

如果您不確定 Agent 能為您做什麼，開始使用 Agent 可能令人望而卻步。引導導覽現在在 Agents 視窗中提供，幫助您快速上手。這些引導式導覽醒目標示關鍵功能，並向您展示如何充分利用與 Agent 合作，幫助您從第一天起就發現委派任務和保持高效的最佳方式。

我們正在實驗這些導覽，以找到向新使用者介紹體驗的最有用方式。

#### 審閱 Agent 變更時的編輯器邊界回饋

審閱 Agent 的變更時，將它指向您想變更的確切程式碼應該毫不費力。您現在可以直接從編輯器邊界留下回饋：懸停某一行會在邊界中顯示 **Add Feedback** 圖示，選取它會在該行放置留言，更快速地將 Agent 引導至程式碼中的特定位置。

本次發行也帶來一系列 Agent 回饋體驗的改進，包括回饋輸入、懸停行為和整體視覺一致性的精修。

#### 從工作階段上下文產生更好的 PR 標題和描述

從 Agents 視窗建立 Pull Request 過去會產生通用的標題和描述，通常需要手動編輯。**Create Pull Request** 按鈕現在使用工作階段上下文來產生 PR 標題和描述，產出更準確、更具描述性的 Pull Request，更好地反映工作階段中完成的工作。

#### 多聊天工作階段

多聊天工作階段讓您在單一 agent host Copilot 工作階段中執行多個聊天。本次發行在此基礎上帶來以下改善。

##### 關閉、重新開啟和刪除聊天

從工作階段標題中的 **+ New Chat** 按鈕建立新聊天。當開啟多個聊天後，會出現分頁列並在尾部附有 **+** 以新增更多。使用分頁上的 **X** 關閉聊天會隱藏它而非捨棄它——從 **Conversations** 下拉選單中將其帶回，每個聊天有一個核取方塊來顯示或隱藏。若要永久移除聊天，開啟其分頁右鍵選單並選取 **Delete Chat**。

##### 跨所有聊天的進度和變更

先前，工作階段僅反映活動聊天的活動，難以分辨對等聊天是否仍在工作或變更了什麼。進度和檔案變更現在跨所有聊天彙總：當任何聊天正在工作時工作階段顯示為進行中，每個分頁呈現自己的進度，工作階段標題的 Changes 膠囊反映來自每個對等聊天的合併編輯。

##### 將對話分支為對等聊天

當您在多聊天工作階段中分支對話時，分支現在在同一工作階段中建立新的對等聊天，而非全新的頂層工作階段。分支的聊天繼承到分支點為止的對話，獨立於其兄弟聊天執行，並取得自動產生的標題。單聊天和非 agent host 工作階段保持現有的分支為新工作階段的行為。

#### 工作階段佈局

##### 工作階段標題中一致的膠囊按鈕

工作階段標題下的操作列現在一致地渲染為統一、緊湊的次要按鈕膠囊。**Workspace 膠囊**顯示工作區圖示（依工作區類型為雲端、資料夾或 worktree）和標籤，長名稱會截斷。**Changes 膠囊**（`N files +X -Y`）讀取並開啟工作階段的預設變更集，保持其計數和開啟的多檔差異在 Copilot 和 agent host 供應商之間一致。

![截圖顯示工作階段標題中一致的 Workspace 和 Changes 膠囊按鈕。](https://code.visualstudio.com/assets/updates/1_127/session-header-pills.webp)

##### 切換工作階段時焦點移至聊天輸入

當您在 Agents 視窗中開啟工作階段時，鍵盤焦點現在落在聊天輸入中，準備好讓您立即開始輸入，即使工作階段有編輯器開啟或正在載入 Changes 檢視。使用鍵盤在工作階段清單中醒目標示項目不會移動焦點，直到您實際開啟工作階段。

##### 響應式工作階段側邊列（實驗性）

**設定**：`sessions.layout.autoCollapseSessionsSidebar`

在窄視窗上，同時顯示編輯器、側邊面板和工作階段側邊列幾乎沒有工作空間。啟用後，當視窗較窄且編輯器和側邊面板同時開啟時，Agents 視窗會自動隱藏工作階段側邊列，有空間時再顯示。它會尊重手動關閉，並在同時顯示多個工作階段時暫停此行為。

### 使用 /troubleshoot 診斷 Agent 行為

troubleshoot 技能透過 `/troubleshoot` 命令呼叫，透過分析聊天工作階段紀錄並呈現 Agent 行為的見解來幫助診斷聊天問題。使用它來調查為何自訂指令被忽略或為何回應緩慢。

在本次發行中，您可以使用 `/troubleshoot` 診斷 agent host 工作階段，包括本機和遠端工作階段。在 Agents 視窗中，在聊天輸入中輸入 `/troubleshoot` 後接 `#session`，選取您想診斷的工作階段，並新增問題或您遇到的問題描述。

![截圖顯示 Agents 視窗中 agent host 工作階段的 troubleshoot 結果。](https://code.visualstudio.com/assets/updates/1_127/ahptroubleshoot.jpg)

---

## 成本管理

### 子代理點數

當 Agent 將工作委派給子代理時，可能難以得知委派工作的成本。為了使此更透明，您現在可以懸停聊天回應中的子代理區段，查看該子代理使用的 AI 點數。

![截圖顯示聊天回應中子代理區段的點數使用量懸停。](https://code.visualstudio.com/assets/updates/1_127/subagent_cost_hover.webp)

---

## Chat

### macOS 和 Linux 的終端機命令沙箱

核准每個 Agent 呼叫的終端機命令很快變得乏味。從本次發行開始，我們在 macOS 和 Linux 上推出終端機命令的沙箱：命令在封鎖網路存取和限制檔案系統存取的情況下執行，讓 Agent 以更少的提示工作。

Agent 僅在命令需要升級並在沙箱外執行時才請求核准。若要了解更多，請參閱 [Agent 沙箱](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety#_agent-sandboxing)。

您可以透過 Permissions 下拉選單關閉此功能：

![截圖顯示在 Default Approvals 模式中可用的沙箱切換。](https://code.visualstudio.com/assets/updates/1_127/sandboxing-toggle.webp)

---

## 語言模型

### 棄用內建 Ollama 供應商

模型供應商可以透過擴充功能為 VS Code 聊天體驗貢獻模型。透過使用擴充功能，供應商可以比內建供應商更快地為您提供新模型和功能的支援。

Ollama 現在有[官方 VS Code 擴充功能](https://marketplace.visualstudio.com/publishers/Ollama)，這是在聊天中使用本機 Ollama 模型的建議方式。

因此，內建 Ollama 供應商現已棄用。如果您正在使用內建供應商透過 [Bring Your Own Key（BYOK）](https://code.visualstudio.com/docs/copilot/customization/language-models#_bring-your-own-language-model-key)執行本機模型，請安裝官方擴充功能並移除內建供應商以繼續無中斷地使用您的 Ollama 模型。以下影片展示如何移除已棄用的供應商。

---

## 整合式瀏覽器

### 攝影機、位置、裝置等

整合式瀏覽器現在支援逐站權限。這使頁面能夠使用更多 Web API，包括：

- 地理位置
- 攝影機和麥克風
- 感測器，例如加速計和陀螺儀
- 剪貼簿
- 裝置，例如藍牙、USB、序列和 HID

當頁面請求權限時，VS Code 會如您在傳統瀏覽器中所預期的提示您允許或拒絕請求。

![截圖顯示系統對話框，app.zoom.us 請求麥克風存取，提供 Allow、Block 或 Cancel 選項。](https://code.visualstudio.com/assets/updates/1_127/browser-permission-prompt.webp)

從 **Site Permissions** 瀏覽器選單項目管理目前站台的權限。

### Agent 工具正式可用

**設定**：`workbench.browser.enableChatTools`（此設定由組織層級管理。請聯繫您的管理員以變更。）

瀏覽器工具讓 Agent 可在整合式瀏覽器中開啟頁面、讀取內容和主控台錯誤、擷取截圖，以及選取、輸入和導覽以驗證自身工作，全部無需外部 MCP 伺服器。經過多個 Preview 階段，瀏覽器工具現已正式可用且預設啟用。

衷心感謝所有執行 Preview、提交問題和分享回饋的人。您的測試直接塑造了本次發行中的每工作階段分頁隔離、明確的頁面共享控制和權限模型。

要求 Agent 建置和驗證 Web 應用程式，或按照逐步指南[使用瀏覽器 Agent 工具建置和測試 Web 應用程式](https://code.visualstudio.com/docs/agents/guides/browser-agent-testing-guide)來查看封閉的建置-測試-修正迴圈。完整參考請見[Agent 的瀏覽器工具](https://code.visualstudio.com/docs/debugtest/integrated-browser#_browser-tools-for-agents)。

管理員可以透過企業政策管控瀏覽器工具：使用 `BrowserChatTools` 政策完全停用，或透過 Agent 網路篩選（`ChatAgentNetworkFilter` 加上允許和拒絕網域清單）限制 Agent 工具可達到的網域。請參閱[為組織設定 AI 設定](https://code.visualstudio.com/docs/enterprise/ai-settings#_configure-agent-network-filtering)。

---

## 企業

### 檔案型受管理 Copilot 設定傳遞

管理員現在可以從磁碟上的 JSON 檔案傳遞受管理的 GitHub Copilot 設定，作為[原生 MDM 管道](https://code.visualstudio.com/updates/v1_125#_native-mdm-delivery-for-managed-copilot-settings)和帳戶型企業設定檔的補充。

這為組織提供了一種直接的方式，在未註冊裝置管理解決方案的機器上，或透過現有工具（如設定管理系統或映像管線）佈建檔案比撰寫原生 MDM 負載更簡單的情況下套用政策。

VS Code 從每個作業系統的固定位置讀取 `managed-settings.json` 檔案。此檔案僅在 MDM 或帳戶型企業設定不存在時才會生效。

- **macOS**：`/Library/Application Support/GitHubCopilot/managed-settings.json`
- **Linux**：`/etc/github-copilot/managed-settings.json`
- **Windows**：`%ProgramFiles%\GitHubCopilot\managed-settings.json`

檔案包含一個 JSON 物件，使用管理員透過 GitHub.com 撰寫的相同結構描述，例如：

```json
{
  "permissions": {
    "disableBypassPermissionsMode": "disable"
  },
  "enabledPlugins": {
    "plugin@marketplace": false
  }
}
```

若要了解更多，請參閱 GitHub 關於[企業管理的客戶端設定](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/configure-enterprise-plugin-standards)的文件。

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@aaronpowell (Aaron Powell)](https://github.com/aaronpowell)：處理外掛市集拉取復原 [PR #318270](https://github.com/microsoft/vscode/pull/318270)
- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
  - 允許在設定同步未使用時停用工作區的 AuthenticationProvider 擴充功能 [PR #320415](https://github.com/microsoft/vscode/pull/320415)
  - 修正 /causedByExtension 回應中的命令大小寫 [PR #298925](https://github.com/microsoft/vscode/pull/298925)
- [@rfeltis (Ralph Feltis)](https://github.com/rfeltis)
  - 為通知遙測新增配額檢查點 [PR #322767](https://github.com/microsoft/vscode/pull/322767)
  - 新增聊天配額軌跡提示 [PR #320683](https://github.com/microsoft/vscode/pull/320683)
- [@romalpani (Rohan Malpani)](https://github.com/romalpani)：在 Agents 視窗中為 ask-questions 輪播新增可見外框 [PR #322188](https://github.com/microsoft/vscode/pull/322188)
- [@Sid200026 (Siddharth Singha Roy)](https://github.com/Sid200026)：為 chat.modelChange 遙測新增 chatSessionId [PR #322579](https://github.com/microsoft/vscode/pull/322579)
- [@tamuratak (Takashi Tamura)](https://github.com/tamuratak)：修正：聊天停止時 CancellationToken 未傳播至 `provideLanguageModelChatResponse` [PR #319098](https://github.com/microsoft/vscode/pull/319098)
- [@yavanosta (Dmitry Guketlev)](https://github.com/yavanosta)：修正行內補全的 `handleEndOfLifetime` `supersededBy` 追蹤 [PR #320143](https://github.com/microsoft/vscode/pull/320143)
- [@yulia-vasyura](https://github.com/yulia-vasyura)：將「Apply Update...」命令重新命名為「Apply Update from File...」[PR #322504](https://github.com/microsoft/vscode/pull/322504)

對 `node-pty` 的貢獻：

- [@codebytere-ant (Shelley Vohr)](https://github.com/codebytere-ant)
  - 修正：在 macOS 上的 SetupExitCallback 中關閉 kqueue fd [PR #931](https://github.com/microsoft/node-pty/pull/931)
  - 修正：在 Windows 上將 CreateProcessW 失敗作為 'exit' 呈現而非 uncaughtException [PR #934](https://github.com/microsoft/node-pty/pull/934)
  - 修正：conpty spawn 失敗時關閉管道控制代碼並釋放屬性清單 [PR #935](https://github.com/microsoft/node-pty/pull/935)

### 問題追蹤

問題追蹤的貢獻：

- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@mantasu (Mantas)](https://github.com/mantasu)
- [@davemcom (DaveM.)](https://github.com/davemcom)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| agent host | agent host |
| Agents window | Agents 視窗 |
| banner | 橫幅 |
| browser tools | 瀏覽器工具 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Changes pill | Changes 膠囊 |
| chat input | 聊天輸入 |
| CI (Continuous Integration) | CI（持續整合） |
| credits | 點數 |
| drag and drop | 拖放 |
| enterprise policy | 企業政策 |
| extension | 擴充功能 |
| fork (conversation) | 分支（對話） |
| generally available (GA) | 正式可用 |
| group | 群組 |
| gutter | 邊界 |
| HID | HID |
| Integrated Browser | 整合式瀏覽器 |
| managed settings | 受管理設定 |
| MDM | MDM（裝置管理） |
| MCP | MCP |
| multi-chat session | 多聊天工作階段 |
| node-pty | node-pty |
| Ollama | Ollama |
| onboarding tour | 引導導覽 |
| peer chat | 對等聊天 |
| per-site permission | 逐站權限 |
| pill | 膠囊按鈕 |
| pin | 釘選 |
| Pull Request | Pull Request |
| sandbox | 沙箱 |
| session | 工作階段 |
| subagent | 子代理 |
| terminal | 終端機 |
| token | Token |
| troubleshoot | troubleshoot |
| workspace | 工作區 |
| Workspace pill | Workspace 膠囊 |
| worktree | worktree |

*資料來源：[Visual Studio Code 1.127 發行說明](https://code.visualstudio.com/updates/v1_127)*
