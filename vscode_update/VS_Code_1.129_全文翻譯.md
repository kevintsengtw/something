# Visual Studio Code 1.129

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 7 月 15 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.129.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.129.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.129.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.129.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.129.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.129.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.129.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.129.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.129.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.129 版本。本次發行帶來專用的 agent host、Agents 視窗中的全新編輯器面板、使用 `!` 執行命令，以及現代化 UI 的預覽。

- [**Agent host**](#agent-host)：在專用程序中執行 Agent 工作階段，並從多個視窗連線至它們。
- [**Agents 視窗中的新編輯器面板（實驗性）**](#agents-視窗中的新編輯器面板實驗性)：在固定編輯器中審閱 Agent 產生的檔案和差異。
- [**使用 `!` 前綴執行命令**](#使用--前綴執行命令)：直接從聊天提示執行終端機命令。
- [**現代化 UI 預覽（實驗性）**](#現代化-ui-預覽實驗性)：搶先體驗更新的 VS Code 工作台外觀。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

我們正圍繞 agent host 重新架構 Agent 工作階段在 VS Code 中的運作方式——agent host 是一個專用程序，根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）執行 Copilot、Claude 和 Codex 等 Agent 工具鏈。因為工作階段存在於自己的程序中，同一工作階段可以同時從多個 VS Code 視窗連線和渲染。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，這意味著其行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

我們正積極開發 agent host 並開始向編輯器視窗和 [Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)的使用者推出。若要加入，請啟用 `chat.agentHost.enabled`（此設定由組織層級管理。請聯繫您的管理員以變更。），然後從工具鏈下拉選單中選取 agent host 工具鏈。以下截圖展示如何在編輯器視窗中選取 agent host 上的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_129/agent-host-harness-dropdown-editor.webp)

隨著我們持續投資 agent host，本次發行說明中的一些新功能可能僅在 Agent 於其上執行時才可用。這些功能會連結回本區段，並在相關時註明任何額外的啟用設定（例如，`chat.agents.claude.preferAgentHost` 以在 agent host 上啟用 Claude Agent）。

### Agents 視窗中的新編輯器面板（實驗性）

[Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)在對話旁邊顯示 Agent 產生的檔案和變更的詳細資料區域。本次發行引入重新設計的**編輯器面板**，將編輯器和詳細資料區域合併為一個固定窗格並共用分頁列，讓審閱 Agent 的工作感覺像是在主編輯器中工作，而非在不同面板之間切換。

透過新的編輯器面板，您可以：

- 在固定編輯器中直接開啟檔案和差異，就在您的聊天旁邊，並透過與聊天分頁列相配的 **New Tab** 操作新增分頁。
- 在 **Changes** 檢視中審閱變更，享受改善的差異體驗：切換行內和並排檢視、一次展開或摺疊所有檔案，以及在更緊湊的差異呈現中讀取變更，讓更多變更內容顯示在畫面上。下一步操作（如 **Create Pull Request**）可直接從編輯器分頁標題取得，且如切換差異檢視等編輯器按鍵繫結的運作方式與主 VS Code 視窗中相同。
- 從上次離開的地方繼續。每個工作階段在工作階段切換和視窗重新載入時還原其側邊窗格寬度、開啟的編輯器、活動編輯器和每檔摺疊狀態。

這是一個實驗性、需手動加入的佈局。若要試用，請啟用 `sessions.layout.singlePaneDetailPanel` 並重新載入視窗，因為此設定僅在啟動時讀取一次。

### Agent Host 工作階段的工作階段管理工具

在 [agent host](#agent-host) 上執行的 Agent（Copilot、Claude 和 Codex）現在可以存取一套工作階段管理工具，讓 Agent 可以列舉、建立、觀察和操作其他工作階段及聊天，而您無需離開目前的對話。

透過這些工具，Agent 可以：

- 列出您的工作階段及其狀態、工作區和變更，以便找到正確的工作階段來操作。除非明確請求，否則已封存的工作階段會被排除。
- 讀取另一個工作階段的近期對話，以了解其正在做什麼。
- 建立新的工作階段或在現有工作階段中建立新聊天，以便交接子任務，而非在單一對話中堆積不相關的工作。
- 向它建立的工作階段或聊天發送訊息，以啟動或引導該工作。

每當工具建立或指向工作階段時，VS Code 都會渲染一個 **Open Session** 膠囊，讓您可以直接跳轉至該工作階段。向另一個工作階段發送訊息始終會先要求您的確認。Agent 無法向自己的聊天發送訊息，且連續發送有上限，因此單一請求不會擴散為無限數量的工作階段。

### Agents 視窗改善

本次發行包含對 [Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)新工作階段流程的幾項小改善：

- **記住新工作階段預設值**：新工作階段選擇器記住您上次的 Agent 模式和核准選擇，並在您下次建立工作階段時使用它們作為預設值，讓您不必為每個任務重新選取相同的選項。
- **Worktree 核取方塊**：新工作階段設定不再使用下拉選單在資料夾和 worktree 隔離之間選擇，而是顯示一個 **New Worktree** 核取方塊。勾選它以使用 Git worktree 隔離執行工作階段，這會將 Agent 的變更保持在單獨的資料夾中，直到您準備好審閱和合併；不勾選則使用資料夾隔離。

---

## Chat

### 使用 `!` 前綴執行命令

您現在可以在聊天訊息前加上 `!`，將其內容作為終端機命令執行。這適用於編輯器和 Agents 視窗中的 [agent host](#agent-host) 工作階段。

![截圖顯示使用 ! 前綴從聊天執行終端機命令。](https://code.visualstudio.com/assets/updates/1_129/bang-commands.webp)

### BYOK 模型搭配 Copilot Agent 工具鏈

您現在可以在 Agents 視窗中選取在 [agent host](#agent-host) 上執行的 **Copilot** 工具鏈時，使用 [Bring Your Own Key（BYOK）模型](https://code.visualstudio.com/docs/agent-customization/language-models#_bring-your-own-language-model-key)。

### 遷移提示檔案至技能（實驗性）

提示檔案（`*.prompt.md`）用於描述自訂斜線命令。它們僅在 Local Agent 工具鏈中受支援，而其他工具鏈以技能表達斜線命令。為了跨工具鏈相容，我們建議將所有提示檔案遷移至技能。

啟用 `chat.customizations.promptMigration.enabled` 後，如果您選取在 [agent host](#agent-host) 上執行的工具鏈且您有可遷移的提示檔案，您現在會在 AI Customizations 總覽中看到「Migrate Prompts」項目。

遷移介面讓您可以：

- 檢視來自工作區（`.github/prompts/`）和使用者資料位置的提示檔案。
- 將選取的檔案遷移至技能並開啟新建立的技能。

![遷移提示檔案](https://code.visualstudio.com/assets/updates/1_129/migrate-prompt-files.webp)

---

## 編輯器體驗

### 從編輯器工具列重新開啟編輯器

當檔案或差異支援多個編輯器時，您可以直接從編輯器工具列切換編輯器。開啟 **...** 選單並從 **Reopen Editor With** 子選單中選取編輯器。這讓替代編輯器更容易被發現，而無需使用命令面板。

### 現代化 UI 預覽（實驗性）

**設定**：`workbench.experimental.modernUI`

您現在可以預覽現代化的 VS Code UI，更新編輯器工作台的外觀和感受。這目前是一個實驗性功能，您可以透過 `workbench.experimental.modernUI` 設定來啟用，在 Insiders 組建中預設啟用。

---

## 驗證

### Agent Host 中的 Copilot 支援 GitHub Enterprise

透過 GitHub Enterprise（GHE）實例提供 GitHub Copilot 存取的開發者可以登入並在 VS Code 中使用 Copilot。先前，agent host 的 Copilot 驗證僅支援 github.com，因此由 GHE 支撐的 Copilot 訂閱無法完成登入：OAuth 流程和 Copilot Token 請求都指向 github.com。

在本次發行中，VS Code 可以針對 GitHub Enterprise 實例驗證 Copilot。當您登入 Copilot 時，選擇您的 GitHub Enterprise 實例，VS Code 會執行登入流程並從該主機而非 github.com 請求 Copilot 存取 Token。這在編輯器視窗和 [Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)中都可運作，且同時支援 Copilot 和 Claude Agent。

因為此支援是 [agent host](#agent-host) 的一部分，請確保以 `chat.agentHost.enabled`（此設定由組織層級管理。請聯繫您的管理員以變更。）啟用 agent host。

---

## 提議的 API

### 為自訂編輯器設定差異和合併編輯器

自訂編輯器現在預設退出差異和合併編輯器。因此，檔案可以繼續在自訂編輯器中開啟，而其差異和合併在內建文字編輯器中開啟。如果您先前在開啟差異或合併編輯器時看到自訂編輯器，可能會注意到此變更。

若要在另一個編輯器中開啟差異，請使用編輯器工具列中的 [**Reopen Editor With** 子選單](#從編輯器工具列重新開啟編輯器)。若要始終對配對的差異使用特定編輯器，請設定 `workbench.diffEditorAssociations` 設定。

提議的 `customEditorPriority` API 為檔案、差異和合併編輯器提供單獨的優先順序：

```json
"priority": {
  "textEditor": "default",
  "diffEditor": "option",
  "mergeEditor": "never"
}
```

新的 `never` 優先順序可防止該編輯器類型被自動選取，同時保持該編輯器可供明確選取。

如果文字差異編輯器無法顯示二進位內容，VS Code 仍會退回到相容的自訂差異編輯器。

---

## 感謝

對 `vscode` 的貢獻：

- [@accnops (Arthur Cnops)](https://github.com/accnops)：chat/voice：在問題輪播上落地語音回答（修正 Skipped）[PR #323161](https://github.com/microsoft/vscode/pull/323161)
- [@cipheraxat (Akshat Anand)](https://github.com/cipheraxat)：修正現代化 UI 編輯器分頁完整標籤上的裝飾色彩 [PR #325291](https://github.com/microsoft/vscode/pull/325291)
- [@danielrobbins (Daniel Robbins)](https://github.com/danielrobbins)：修正設定正確 Chat 模型相關的 Bug（修正 Issue #323765）[PR #323767](https://github.com/microsoft/vscode/pull/323767)
- [@dobbydobap (varshitha)](https://github.com/dobbydobap)
  - 修正第二次 Rerun Last Task 對 reevaluateOnRerun 任務無法啟動的問題 [PR #324571](https://github.com/microsoft/vscode/pull/324571)
  - 解除拖曳至未釘選列開頭的已釘選分頁的黏附狀態 [PR #324734](https://github.com/microsoft/vscode/pull/324734)
- [@JeffreyCA](https://github.com/JeffreyCA)：為 Azure Developer CLI (azd) 更新 Fig 規範 [PR #321221](https://github.com/microsoft/vscode/pull/321221)
- [@Kaidesuyoo (Kaidesuyo)](https://github.com/Kaidesuyoo)：修正持續性工作台 UI 效能退化 [PR #324986](https://github.com/microsoft/vscode/pull/324986)
- [@myselfsiddharth (Siddharth Mehta)](https://github.com/myselfsiddharth)：debug：在例外小工具中右對齊工具列操作 [PR #325077](https://github.com/microsoft/vscode/pull/325077)
- [@theanarkh (theanarkh)](https://github.com/theanarkh)
  - workbench：修正 ObjectSettingCheckboxWidget 記憶體洩漏 [PR #323670](https://github.com/microsoft/vscode/pull/323670)
  - 修正：確保在 ipc emitter 新增 listener 時註冊 handler [PR #323663](https://github.com/microsoft/vscode/pull/323663)
- [@yavanosta (Dmitry Guketlev)](https://github.com/yavanosta)：在 growUntilVariableBoundaries 中使用 startColumn [PR #324523](https://github.com/microsoft/vscode/pull/324523)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)
- [@dobbydobap (varshitha)](https://github.com/dobbydobap)
- [@hogiSp (hogiSp)](https://github.com/hogiSp)

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
| archived session | 已封存的工作階段 |
| bang command (`!`) | `!` 前綴命令 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Command Palette | 命令面板 |
| Copilot SDK | Copilot SDK |
| custom editor | 自訂編輯器 |
| diff | 差異 |
| editor panel | 編輯器面板 |
| extension | 擴充功能 |
| folder isolation | 資料夾隔離 |
| GitHub Enterprise (GHE) | GitHub Enterprise（GHE） |
| harness | 工具鏈 |
| inline view | 行內檢視 |
| Integrated Browser | 整合式瀏覽器 |
| keybinding | 按鍵繫結 |
| managed settings | 受管理設定 |
| modern UI | 現代化 UI |
| OAuth | OAuth |
| pill | 膠囊 |
| prompt file | 提示檔案 |
| Proposed API | 提議的 API |
| session | 工作階段 |
| session management tools | 工作階段管理工具 |
| side-by-side view | 並排檢視 |
| skill | 技能 |
| slash command | 斜線命令 |
| subagent | 子代理 |
| tab strip | 分頁列 |
| terminal | 終端機 |
| token | Token |
| workspace | 工作區 |
| worktree | worktree |
| worktree isolation | worktree 隔離 |

*資料來源：[Visual Studio Code 1.129 發行說明](https://code.visualstudio.com/updates/v1_129)*
