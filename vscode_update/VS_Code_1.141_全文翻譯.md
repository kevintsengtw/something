# Visual Studio Code 1.141 全文翻譯

- 版本：1.141.0（Stable）
- 發行日期：2026 年 10 月 7 日
- 原文連結：https://code.visualstudio.com/updates/v1_141

---

## 1.141.0 下載

- Windows：[x64](https://update.code.visualstudio.com/1.141.0/win32-x64-user/stable)、[Arm64](https://update.code.visualstudio.com/1.141.0/win32-arm64-user/stable)
- macOS：[Universal](https://update.code.visualstudio.com/1.141.0/darwin-universal-dmg/stable)、[Intel](https://update.code.visualstudio.com/1.141.0/darwin-x64-dmg/stable)、[Apple silicon](https://update.code.visualstudio.com/1.141.0/darwin-arm64-dmg/stable)
- Linux：[.deb](https://update.code.visualstudio.com/1.141.0/linux-deb-x64/stable)、[.rpm](https://update.code.visualstudio.com/1.141.0/linux-rpm-x64/stable)、[.tar.gz](https://update.code.visualstudio.com/1.141.0/linux-x64/stable)、[Arm 安裝說明](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions)、[Snap](https://update.code.visualstudio.com/1.141.0/linux-snap-x64/stable)

已經安裝了？請在 VS Code 中使用 **Check for Updates**（檢查更新）。若想搶先體驗即將推出的功能，請使用 [Insiders 版本](https://code.visualstudio.com/insiders)。

---

## 版本亮點（Release highlights）

本版讓你更容易管理 agent 工作階段（session）、保護 agent 工作流程、編輯欄狀（columnar）文字，以及跨多個 GitHub Enterprise 執行個體工作。

- [清理 worktree 儲存空間](#回收閒置-worktree-的儲存空間)：隨選或自動回收閒置 agent session worktree 所佔用的磁碟空間。
- [跨平台沙箱（sandboxing）](#copilot-agent-host-的沙箱功能)：透過終端機沙箱，在 Windows、macOS 與 Linux 上限制 agent 對檔案與網路資源的存取。
- [排列工作階段](#以格狀排列工作階段)：以格狀（grid）版面並排比較與監看多個 agent session。
- [接續外部工作階段](#不需重新載入即可接續本機的外部-copilot-工作階段)：在 VS Code 中接續本機的外部 Copilot 與 Codex 對話，而不會遺失脈絡。
- [區塊貼上](#展開式區塊貼上)：從單一游標將區塊的各列貼到連續的多行。
- [多個 GitHub Enterprise 執行個體](#登入多個-github-enterprise-執行個體)：在同一個 VS Code 視窗中同時登入 GHE.com 與 GitHub Enterprise Server 帳號。

---

## Agent 迴圈（Agent loop）

### Copilot harness

Copilot harness 為 VS Code 加入令人興奮的全新 agent 功能。它由 Copilot SDK 驅動，因此其行為與能力與其他 Copilot 產品一致，包括獨立的 GitHub Copilot app 與 Copilot CLI。

若要開始使用 Copilot harness，請在聊天輸入框的 harness 選擇器中選取它。在本版中，它可能已經是你的預設選項。你可以照平常的方式繼續工作。

![顯示 harness 選擇器中已選取 Copilot harness 的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_141/Harness-Copilot.webp)

此 harness 會在一個專屬的 [agent host](https://code.visualstudio.com/docs/agents/concepts/agent-host) 程序中執行，該程序以 Agent Host Protocol（AHP）為基礎，因此你可以從多個 VS Code 視窗連線到同一個 agent session。

了解如何[使用 Copilot harness](https://code.visualstudio.com/docs/agents/run/agent-harnesses#use-the-copilot-harness)，或在 [agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)中探索其架構與工作流程。若有任何意見回饋或需求，[請提交 issue](https://github.com/microsoft/vscode/issues)。

### 回收閒置 worktree 的儲存空間

執行 **Chat: Open Worktree Cleanup**，即可查看閒置的 agent session worktree 使用了多少磁碟空間，並移除你不再需要的 worktree。你可以依閒置時間長短篩選 session、檢視每個 worktree 的大小，並選取要清理的 worktree。

清理編輯器也可讓你設定「針對已合併 pull request 的 session 自動清理」，並控制當 session 儲存空間變大時，VS Code 是否要建議清理。

![顯示 worktree 清理編輯器的螢幕截圖，含清理設定與閒置的 session worktree。](https://code.visualstudio.com/assets/updates/1_141/worktree-cleanup-editor.webp)

當 `chat.agentSessions.sessionStorageCleanupSuggestion.enabled` 啟用時，一旦閒置 worktree 累積到一定數量，或佔用的磁碟空間大到值得清理，Agents 視窗就會通知你。

![顯示 Agents 視窗中 worktree 清理建議的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_141/worktree-cleanup.webp)

清理動作會將所選的 session 標記為已完成（done），並刪除其 worktree。你之後可以還原該 session，以重新建立其 worktree。處於作用中（active）、執行中（running）、需要輸入（needs-input）與已釘選（pinned）狀態的 session 會受到保護，不會被清理。

### 追蹤背景 shell

當 agent 在背景執行命令時，它會在命令執行期間繼續處理其他步驟。在 [Copilot harness](#copilot-harness) 的 session 中，聊天輸入框上方的 **Background Shells** 標籤（pill）會列出仍在執行中的 shell，在 Agents 視窗與編輯器視窗中皆可看到。

選取該標籤可查看每個 shell 的命令以及它已執行了多久；選取某個 shell 則可在命令執行時觀看其輸出串流（若有輸出可用）。當 shell 的命令完成後，它就會從清單中移除。

![顯示 Background Shells 標籤的螢幕截圖，其詳細資訊中正串流顯示一個執行中 shell 的輸出。](https://code.visualstudio.com/assets/updates/1_141/background-shell-streaming.webp)

### 對 agent 觸發的訊息有更多控制

當 agent 跨對話委派工作時，它可以在需求變更時調整委派的任務，而不必等待另一個 session 完成。`send_message` 工具支援在同一個 agent host 上的 session 與巢狀（nested）session 之間執行以下動作：

- 以修正指令引導（steer）進行中的對話。
- 將後續訊息排入佇列，作為獨立的一輪（turn）。
- 取代佇列中的訊息，同時保留其在佇列中的位置。
- 在處理開始前取消佇列中的訊息。

佇列中的訊息會依序執行。訊息一旦開始處理，就無法再被取代或取消。

---

## Agent UX

### 以格狀排列工作階段

比較結果、監看長時間執行的任務，或跨多個相關對話工作，而不必反覆切換 session。如同在編輯器視窗中一樣，Agents 視窗的二維 session 格狀版面可讓你將 session 拖曳成水平或垂直分割，並調整各窗格的大小。

當你需要專注時，可將某個 session 最大化；之後再回到格狀版面，繼續跨對話工作。

### 精簡版面密度（Compact layout density）

**設定**：`window.density.layout`

使用 Compact 版面密度，可在 Agents 視窗中容納更多內容。Compact 密度使用較矮的標題列、移除工作台（workbench）各部分之間的間隙，並縮減窗格間距。

預設情況下，Agents 視窗會遵循使用者層級的 `window.density.layout` 設定，而該設定也同樣套用於編輯器視窗。若要只為 Agents 視窗設定不同的密度，請開啟 Agents 視窗並選擇 **View > Layout Density**。編輯器視窗的密度維持不變。

### 更多 session 篩選選項

讓你的 session 清單聚焦在你在意的工作上。開啟 **Filter Sessions**，可分別依下列條件獨立篩選：

- **Environment**（環境）：session 的執行位置，例如本機、雲端或遠端主機。
- **Harness**：session 使用的 agent harness。
- **Created In**（建立來源）：建立該 session 的應用程式，例如 VS Code、Copilot CLI 或 Copilot app。

此選單同時整合了排序、分組、**Show Done** 與 **Created Externally** 等控制項。

![顯示 Filter Sessions 選單的螢幕截圖，含環境、應用程式、harness 與外部 session 篩選器。](https://code.visualstudio.com/assets/updates/1_141/sessions-filter-menu.webp)

### 追蹤巢狀 session 的活動

[巢狀 session](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions#_run-multiple-chats-in-a-session) 可讓你將相關工作拆分到不同的對話中。在 Agents 視窗中，每個巢狀 session 都會顯示其未讀狀態與最後修改時間，幫助你找到最近的活動以及需要你關注的對話，而不必逐一開啟。

![顯示 Agents 視窗中巢狀 session 樹狀結構的螢幕截圖，其中一個巢狀 session 被突顯，顯示其藍色未讀指示、儲存庫與最後修改時間。](https://code.visualstudio.com/assets/updates/1_141/nested-session-details.webp)

狀態會針對每個對話獨立追蹤：

- 開啟某個巢狀 session 只會將該對話標記為已讀。未讀的巢狀 session 不會讓主 session 顯示為未讀。
- 最後修改時間反映的是該對話中的活動，而非主 session 或其他巢狀 session 的活動。

已讀狀態與最後修改時間在重新載入視窗或重新啟動 VS Code 後仍會保留。

若要清除整個 session 樹的未讀狀態，請在主 session 上按右鍵並選取 **Mark as Read**。無關的 session 不會受到影響。

### 簡化的新 session 控制項（實驗性）

**設定**：`sessions.chat.experimental.newSessionComposerLayout`、`sessions.chat.unifiedWorkspacePicker.enabled`（僅限 Agents 視窗）

實驗性的新 session 編輯器（composer）讓你的提示詞保持在中心位置，同時讓 session 選項更容易檢視。工作區、儲存庫與 harness 的控制項現在會顯示在聊天輸入框上方一個可收合的 **Session Options** 托盤（tray）中。

入門提示現在會顯示在聊天輸入框下方的圓角通知中，因此不會再把 session 控制項與你的提示詞隔開。

![顯示新 session 聊天輸入框下方入門提示的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_141/new-session-composer-tip-below-input.webp)

收合托盤可減少視覺干擾，而不會改變你的選擇。此編輯器會記住你的選擇、在視窗變窄時調整標籤文字，並在螢幕閱讀器最佳化模式啟用時保持選項展開。當你使用自訂聊天背景時，托盤會使用不透明的表面，以確保其控制項可讀。

### 改善 context 選擇器的定位

**Add Context** 選擇器現在會在觸發它的按鈕旁邊開啟，並在你再次選取該按鈕時關閉。這讓選擇器在編輯器與 Agents 視窗中都能在視覺上與聊天輸入框保持連結。

![顯示 Add Context 選擇器固定在聊天輸入框按鈕旁邊的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_141/add-context-picker-anchored.webp)

### 將 agent 選擇器移入 context 控制項（實驗性）

**設定**：`sessions.chat.experimental.agentsPickerInAttachContextMenu`（僅限 Agents 視窗）

如果你希望 **Agent** 選擇器一開始從聊天輸入框中隱藏，以減少控制項的數量，可以設定此項目，將它移入 **Add Context** 選單。當你選取了某個 agent 之後，選擇器就會回到聊天輸入框中。

### 自訂新 session 的歡迎訊息（實驗性）

**設定**：`sessions.chat.experimental.welcomePhrases`、`sessions.chat.experimental.welcomeMessages`（僅限 Agents 視窗）

新 session 的歡迎訊息為 Agents 視窗增添個性。在本版中，你可以在預設語句之外加入自己的語句，或取代預設語句。使用 `{name}` 預留位置，即可將你的歡迎名稱放在語句中的任何位置。

選取標題旁的 **Customize Welcome Message** 可變更你的名稱或開啟語句設定。你也可以在標題上按右鍵並選取 **Hide Welcome Message** 來關閉歡迎語句。

---

## Agent 環境（Agent environments）

### 不需重新載入即可接續本機的外部 Copilot 工作階段

來自 Copilot CLI 與 GitHub Copilot app 的新對話，現在會在你送出第一個請求時就自動以外部 session 的形式被納入，即使 Agents 視窗已經開啟也一樣。你可以直接從 Agents 視窗開啟並接續這些對話。

使用 **Created In** 與 **Created Externally** 篩選器，可選擇哪些對話要顯示在 session 清單中。

### Codex 聊天在應用程式之間交接

你可以接手在 ChatGPT app 或 Codex CLI 中開始的 Codex 聊天，並在同一台機器上於 VS Code 的 Agents 視窗中繼續，且對話歷程完整保留。之後你也可以切回 ChatGPT app 或 Codex CLI，繼續同一段聊天。

若要在 VS Code 中使用你的 GitHub Copilot 訂閱，請在模型選擇器中選擇 Copilot 模型。例如，在達到 ChatGPT 使用上限後，你可以改用 Copilot 繼續工作。

在[啟用 Codex](https://code.visualstudio.com/docs/agents/run/agent-harnesses#codex) 並讓 **Created Externally** 的 session 可見的情況下，於 ChatGPT 或 Codex CLI 中建立或更新的聊天會在幾秒內出現，不需要重新載入 VS Code。

同一時間只有一個應用程式可以對該聊天傳送訊息。如果 ChatGPT 或 Codex CLI 仍開啟著該聊天，VS Code 中會顯示 **This chat is open in another app** 橫幅，說明為何無法傳送。請完全結束 ChatGPT 或退出 Codex CLI 的 session，然後選取 **Retry**。你的草稿與附件會保留在原處，而 **Retry** 只會檢查存取權，不會送出你的訊息。

### 從編輯器下載遠端檔案

將開啟中的遠端檔案儲存到本機，而不必在 Files 面板中尋找它。在 Agents 視窗中，開啟編輯器的 **More Actions** 選單並選取 **Download...**，即可將檔案下載到本機位置。

### Dev Container 範例（實驗性）

**設定**：`chat.agentHost.devContainer.samples.enabled`（僅限 Agents 視窗）

不必先複製（clone）儲存庫或在本機安裝其語言工具，就能搭配立即可用的開發環境試用 agent。Agents 視窗現在提供 Go、.NET、Node.js、PHP、Python 與 Rust 的 Dev Container 範例。

![顯示 Agents 視窗中 Dev Container Sample 子選單的螢幕截圖，含 Go、.NET、Node.js、PHP、Python 與 Rust 的範例。](https://code.visualstudio.com/assets/updates/1_141/dev-container-samples.webp)

請先[安裝並啟動 Docker](https://code.visualstudio.com/docs/devcontainers/containers#installation)，再啟用此設定。在桌面版 Agents 視窗中開啟新 session，從工作區選擇器中選取 **Dev Container Sample**，然後送出提示詞。

這同時需要啟用 `chat.agentHost.devContainer.enabled` 與 `chat.remoteAgentHostsEnabled`。

---

## Agent 安全性（Agent security）

### Copilot agent host 的沙箱功能

**設定**：`chat.agent.sandbox.enabled`

沙箱（sandboxing）讓你在 Copilot 處理任務時，對它能存取的內容有更多控制。它會限制受支援的 agent 操作如何存取檔案與網路資源，並有助於降低模型出錯、提示詞注入（prompt injection）、不受信任的相依套件，以及在本機啟動的工具伺服器所帶來的影響。

視操作與平台而定，這些限制會使用作業系統的保護機制，或 agent host 程序內的檢查。沙箱增加了一層保護，但不能取代端點安全防護，也不構成獨立的安全邊界。

沙箱可在 **Windows、macOS 與 Linux** 上使用。在 Windows 上，必須先安裝所需的作業系統更新。詳情請參閱[檢查平台可用性](https://code.visualstudio.com/docs/agents/run/agent-sandboxing#_check-platform-availability)。

若要為所有 session 設定沙箱，請啟用 `chat.agent.sandbox.enabled` 設定。你也可以在 **Permissions** 選單中使用 **Sandboxing for terminal** 切換開關，於 session 層級控制沙箱。

![顯示 Permissions 選單中 Sandboxing for terminal 切換開關的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_141/sandbox-toggle.webp)

你可以進一步透過 VS Code 中各自對應的設定，自訂檔案系統權限與網路存取。

沙箱同時適用於本機 session 與已連線的遠端 session。對本機 session 而言，限制會在你的機器上強制執行；對遠端 session 而言，限制會在 Copilot 執行工具與命令的遠端主機上強制執行。檔案系統權限指的是執行 agent 的那台機器上的路徑。

當沙箱啟用時，在本機啟動的 MCP 與語言伺服器預設都會被納入沙箱。

---

## 聊天體驗（Chat experience）

### 聊天中的持續進度顯示

**設定**：`chat.experimental.persistentProgress`、`chat.experimental.persistentProgressVerbosity`

持續進度顯示（persistent progress）正逐步推送給所有使用者。它會讓處理中的回應保持可見，並讓你控制要顯示多少進度細節。使用 `chat.experimental.persistentProgress` 可從三種圖示樣式中選擇，包括單色圖示或不顯示圖示。

### 認識 Blobby

`/vscode-pet` 現在有名字了：Blobby。在編輯器或 Agents 視窗的任何聊天中輸入 `/vscode-pet`，即可認識 Blobby。

輸入 `/blobby` 可自訂 Blobby。隨著你探索聊天功能並解鎖成就，會有更多自訂選項可用。

![顯示 VS Code Pet 分別以 Insiders 與 Stable 配色睡覺的動畫。](https://code.visualstudio.com/assets/updates/1_141/sleep-wake-pr330399.png)

進一步了解 [Blobby 與聊天寵物](https://code.visualstudio.com/docs/agents/reference/chat-pet)。

---

## MCP

### 在 Customizations 編輯器中查看探索到的 MCP 伺服器（預覽）

在 Agent Customizations 編輯器的 **MCP Servers** 區段中，可以在你已設定的伺服器旁邊，找到由外掛程式（plugin）、擴充功能與內建整合所提供的 MCP 伺服器。在工作區 `mcp.json` 之外探索到的伺服器會顯示在 UI 中，因此你不必再使用 **MCP: List Servers** 來尋找它們。

從命令面板執行 **Chat: Open Customizations**，並選取 **MCP Servers**，即可檢視可用的整合。進一步了解[新增與管理 MCP 伺服器](https://code.visualstudio.com/docs/agent-customization/mcp-servers)。

---

## 編輯器體驗（Editor experience）

### 毛玻璃（Frosted glass）覆疊層與選單

**設定**：`workbench.modernUIFrostedGlass`、`workbench.modernUIFrostedGlassOpacity`

在桌面版 Agents 視窗中，選單、命令面板與其他彈出視窗現在使用柔和模糊的毛玻璃背景。當透過 `workbench.experimental.modernUI` 啟用現代化 UI 時，毛玻璃效果也可在編輯器視窗中使用。

使用 `workbench.modernUIFrostedGlassOpacity` 可控制模糊內容透出的程度。較低的值會讓背景更透明，較高的值則讓背景更實心。

毛玻璃效果會遵循 VS Code 與作業系統中的「降低透明度」偏好設定，並在使用高對比佈景主題或不支援此效果時改用實心背景。若你發現顯示或效能問題，請停用 `workbench.modernUIFrostedGlass`。

### 選擇你的分頁樣式（實驗性）

**設定**：`workbench.experimental.modernUIEditorTabStyle`

當現代化 UI 啟用時（`workbench.experimental.modernUI`），你可以在兩種分頁樣式之間選擇：

- `connected`：在視覺上將作用中的分頁與其內容連接起來。
- `pill`：將分頁顯示為個別的圓角膠囊（pill）。

### 展開式區塊貼上

**設定**：`editor.multiCursorPaste`

在單一游標處貼上已複製的區塊，可將其各列分配到連續的目的行。當 `editor.multiCursorPaste` 設為 `spread` 時，此行為預設啟用。將它設為 `full`，則會改為貼上完整的區塊。

---

## 驗證（Authentication）

### 登入多個 GitHub Enterprise 執行個體

**設定**：`github-enterprise.uris`

許多組織透過 GHE.com 帳號使用 GitHub Copilot，但將程式碼保存在 GitHub Enterprise Server（GHES）上。原本的 `github-enterprise.uri` 設定只能保存單一執行個體，因此你無法同時登入兩者。例如，你無法在同一個 VS Code 視窗中，一邊以 GHE.com 帳號使用 Copilot，一邊在 GHES 上審查 pull request。

新的 `github-enterprise.uris` 設定可接受一份 GHE.com 與 GHES 執行個體的清單：

```json
"github-enterprise.uris": [
  "https://octocat.ghe.com",
  "https://github.contoso.com"
]
```

GitHub Enterprise 驗證提供者會保存清單中每個執行個體的帳號。每個帳號都會以其主機標示，例如 `monalisa - octocat.ghe.com`，讓你能在 Accounts 選單中區分它們。當設定了多個執行個體時，登入時會詢問要使用哪一個執行個體。項目的順序並不會決定預設的執行個體。

若要檢查或變更某個擴充功能使用的帳號，請在 Accounts 選單中選取 **Manage Extension Account Preferences...**，或從命令面板執行 **Accounts: Manage Extension Account Preferences...**。

以下擴充功能支援多個執行個體：

- **GitHub Copilot** 可搭配 GHE.com 執行個體使用。當你透過 **Continue with GHE.com** 登入時，可以輸入執行個體名稱（例如 `octocat`）或其完整 URL。VS Code 會將該執行個體加入 `github-enterprise.uris`，而不會移除你的其他執行個體；若既有的項目不是有效的 URL，它會提議修正該項目。
- **GitHub Pull Requests** 會在你開啟來自尚未設定的企業執行個體的儲存庫時，提議將該主機加入 `github-enterprise.uris`。它一次只使用一個 GitHub Enterprise 帳號，因此你其他執行個體上的儲存庫需在切換帳號後才可使用。同時使用多個企業執行個體的需求正在 [microsoft/vscode-pull-request-github#9004](https://github.com/microsoft/vscode-pull-request-github/issues/9004) 追蹤中。
- **GitHub Repositories** 可並排開啟來自多個 GHE.com 執行個體的儲存庫。它不支援 GHES 儲存庫。

如果你已經在使用 `github-enterprise.uri`，你會維持登入狀態。首次啟動本版時，VS Code 會將你儲存的認證資訊搬移為可搭配新設定使用。`github-enterprise.uri` 已被棄用。當兩個設定都有設定時，以 `github-enterprise.uris` 為優先；而空清單（`[]`）會關閉 GitHub Enterprise 登入。

---

## 企業（Enterprise）

### 將舊版 VS Code 原則遷移至 Agent Host 受管理設定

由 Copilot SDK 驅動、在 VS Code Agent Host 中執行的 Copilot harness，正逐步成為已啟用 VS Code 實驗功能之企業使用者的預設選項。為了在這段過渡期間保留既有的企業控制措施，VS Code 會在可能的情況下，於執行階段將受支援的舊版原則轉譯為統一的受管理設定（unified managed settings）。此轉譯適用於同一台機器上的 session，且不會變更你的原則設定。

企業仍應在適用的情況下遷移至[統一的受管理設定](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/use-managed-settings/get-started)，以便跨 VS Code、GitHub Copilot app 與 Copilot CLI 進行一致的原則管理。

### 使用受管理設定強制要求沙箱（預覽）

若要在 Copilot Agent Host 中強制要求沙箱並防止繞過，請使用以下[統一的受管理設定](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/use-managed-settings/get-started)：

```json
{
  "sandbox": {
    "enabled": true,
    "allowBypass": false
  }
}
```

舊版 VS Code 沙箱設定與裝置原則已被棄用。`ChatAgentSandboxEnabled` 在 Agent Host 中現在只提供一個可被覆寫的預設值，而非強制性的要求。本機行為維持不變。

### 將 Agent Host session 歸屬到使用者與機器

管理員可透過在 VS Code 的 Agent Host 遙測（telemetry）中加入作業系統使用者名稱與主機名稱，更容易地將 Agent Host session 歸屬到使用者與機器。請將以下範例合併到你的[統一受管理設定](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/use-managed-settings/get-started)中，與你既有的遙測匯出器設定並列：

```json
{
  "telemetry": {
    "enabled": true,
    "capture": {
      "identity": true
    }
  }
}
```

例如，擷取到的身分屬性可能包含（示意值）：

```json
{
  "user.name": "octokit",
  "process.user.name": "devuser",
  "host.name": "dev-workstation"
}
```

作業系統使用者名稱（`process.user.name`）與主機名稱（`host.name`）屬於資源屬性。已登入的 GitHub 使用者名稱（`user.name`）會在隨附的 Copilot runtime 支援時，出現在執行階段的 span 上。

身分擷取預設為關閉，且受管理的值會覆寫個人偏好設定。它與內容擷取（content capture）彼此獨立。VS Code 主機端的控制並不管轄 runtime 的直接匯出。

### 在原則檢查期間保留你選取的 agent

當你的企業透過 [`forceRemoteSettingsRefresh`](https://github.com/github/docs/blob/main/content/copilot/reference/copilot-cli-reference/cli-config-dir-reference.md#supported-keys) 要求進行新的原則檢查時，聊天會在原則檢查期間保留你選取的 agent，而不是切換為 **Ask** 或 **Edit**。例行的背景重新整理也會在載入期間維持最後一次接受的原則有效。啟動、帳號變更與重新整理失敗時，仍會持續阻擋送出，直到原則檢查成功為止。新接受的限制仍會持續套用。

---

## 對擴充功能的貢獻（Contributions to extensions）

### GitHub Pull Requests

[GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) 擴充功能有了更多進展，它讓你可以處理、建立與管理 pull request 與 issue。新功能包括：

- 將 `githubPullRequests.experimental.stacks` 設為 `true`，即可建立、檢視與合併堆疊式（stacked）pull request。
- 支援設定多個 GitHub Enterprise 執行個體並在其間選擇。

請查看此擴充功能 [0.168.0 版的變更記錄](https://github.com/microsoft/vscode-pull-request-github/blob/main/CHANGELOG.md#01680)，以了解該版本的所有內容。

---

## 已棄用的功能與設定（Deprecated features and settings）

### GitHub Enterprise URI 設定

`github-enterprise.uri` 設定已被棄用，改由可接受多個 GHE.com 與 GitHub Enterprise Server 執行個體的 `github-enterprise.uris` 取代。當 `github-enterprise.uris` 未設定時，VS Code 仍會讀取 `github-enterprise.uri`。

### 舊版 VS Code 沙箱設定與裝置原則

舊版 VS Code 沙箱設定與裝置原則已被棄用。`ChatAgentSandboxEnabled` 在 Agent Host 中現在只提供一個可被覆寫的預設值，而非強制性的要求。本機行為維持不變。

---

## 致謝（Thank you）

對 `vscode` 的貢獻：

- [@dzamoshchin (Daniel Zamoshchin)](https://github.com/dzamoshchin)：在虛擬工具群組被啟用時將其標記為已使用 [PR #338812](https://github.com/microsoft/vscode/pull/338812)
- [@jimmylewis (Jimmy Lewis)](https://github.com/jimmylewis)：extensions：讓更新檢查維持在輪詢節奏上 [PR #339365](https://github.com/microsoft/vscode/pull/339365)
- [@na2co3-ftw (na2co3)](https://github.com/na2co3-ftw)：Modern UI：修正 connected 編輯器分頁中多餘的分頁動作淡出效果 [PR #336871](https://github.com/microsoft/vscode/pull/336871)
- [@rohit489 (Rohit Agrawal - MSFT)](https://github.com/rohit489)：在 GitHub 遙測事件中記錄 CAPI X-GitHub-Copilot-Request-Te [PR #339087](https://github.com/microsoft/vscode/pull/339087)
- [@shoemoney (Jeremy Schoemaker)](https://github.com/shoemoney)：fix(markdown)：在預覽連結中保留非行號的片段（fragment） [PR #332661](https://github.com/microsoft/vscode/pull/332661)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - fix：終端機重播（replay）中的記憶體洩漏 [PR #338252](https://github.com/microsoft/vscode/pull/338252)
  - fix：語意權杖（semantic tokens）中的記憶體洩漏 [PR #336024](https://github.com/microsoft/vscode/pull/336024)
  - fix：搜尋編輯器中的記憶體洩漏 [PR #331014](https://github.com/microsoft/vscode/pull/331014)
  - terminal：在釋放（disposal）時取消不必要的遙測逾時 [PR #333963](https://github.com/microsoft/vscode/pull/333963)
  - fix：動畫影格視窗釋放中的記憶體洩漏 [PR #338263](https://github.com/microsoft/vscode/pull/338263)
  - fix：聊天參與者（chat participant）釋放中的記憶體洩漏 [PR #338230](https://github.com/microsoft/vscode/pull/338230)
  - fix：自訂文件釋放中的記憶體洩漏 [PR #338250](https://github.com/microsoft/vscode/pull/338250)
  - fix：mapped edit provider 釋放中的記憶體洩漏 [PR #338245](https://github.com/microsoft/vscode/pull/338245)
  - fix：擴充功能主機遙測中的記憶體洩漏 [PR #334096](https://github.com/microsoft/vscode/pull/334096)
  - fix：輔助視窗字型測量中的記憶體洩漏 [PR #338255](https://github.com/microsoft/vscode/pull/338255)
  - fix：SCM artifact 命令中的記憶體洩漏 [PR #338251](https://github.com/microsoft/vscode/pull/338251)
  - fix：pixel ratio 視窗釋放中的記憶體洩漏 [PR #338257](https://github.com/microsoft/vscode/pull/338257)
  - fix：整合式瀏覽器元素控制代碼中的記憶體洩漏 [PR #338247](https://github.com/microsoft/vscode/pull/338247)
  - fix：終端機水平捲軸中的記憶體洩漏 [PR #338241](https://github.com/microsoft/vscode/pull/338241)
  - fix：擴充功能主機樹狀檢視釋放中的記憶體洩漏 [PR #338235](https://github.com/microsoft/vscode/pull/338235)
  - fix：composite 拖放註冊中的記憶體洩漏 [PR #338215](https://github.com/microsoft/vscode/pull/338215)
  - fix：整合式瀏覽器 inspector session 中的記憶體洩漏 [PR #338228](https://github.com/microsoft/vscode/pull/338228)
  - fix：chatInputPart 中的記憶體洩漏 [PR #327157](https://github.com/microsoft/vscode/pull/327157)
  - fix：終端機 shell 執行串流中的記憶體洩漏 [PR #338243](https://github.com/microsoft/vscode/pull/338243)
  - fix：notebook 儲存格狀態列命令中的記憶體洩漏 [PR #336028](https://github.com/microsoft/vscode/pull/336028)
  - fix：issueReporterModel 中的記憶體洩漏 [PR #335098](https://github.com/microsoft/vscode/pull/335098)
  - fix：圖片輪播編輯器中的記憶體洩漏 [PR #333981](https://github.com/microsoft/vscode/pull/333981)
  - fix：notebook serializer 釋放中的記憶體洩漏 [PR #338212](https://github.com/microsoft/vscode/pull/338212)
  - fix：MainThreadShare 中的記憶體洩漏 [PR #334110](https://github.com/microsoft/vscode/pull/334110)
  - fix：multi diff 編輯器分頁中的記憶體洩漏 [PR #333186](https://github.com/microsoft/vscode/pull/333186)
  - fix：test observer 釋放中的記憶體洩漏 [PR #338213](https://github.com/microsoft/vscode/pull/338213)
  - fix：modalEditorPart 中的記憶體洩漏 [PR #326885](https://github.com/microsoft/vscode/pull/326885)
  - fix：註解串（comment thread）釋放中的記憶體洩漏 [PR #338216](https://github.com/microsoft/vscode/pull/338216)
  - fix：line data event addon 中的記憶體洩漏 [PR #332139](https://github.com/microsoft/vscode/pull/332139)
  - fix：notebook kernel 來源選單中的記憶體洩漏 [PR #338218](https://github.com/microsoft/vscode/pull/338218)
- [@soreavis](https://github.com/soreavis)：JSON - 在 json.schemas 的 fileMatch 模式中支援 `${workspaceFolder}` [PR #326706](https://github.com/microsoft/vscode/pull/326706)
- [@yuezhang030](https://github.com/yuezhang030)：新增 vscModelF 提示詞 [PR #338809](https://github.com/microsoft/vscode/pull/338809)

### Issue 追蹤

對 issue 追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們非常感謝大家在新功能準備好時就立即試用，請經常回來這裡看看有什麼新內容。

> 若想閱讀先前 VS Code 版本的版本說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 的 [Updates](https://code.visualstudio.com/updates) 頁面。

---

資料來源：https://code.visualstudio.com/updates/v1_141
