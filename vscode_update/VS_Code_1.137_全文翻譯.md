# Visual Studio Code 1.137

> 原文：<https://code.visualstudio.com/updates/v1_137>
> 發布日期：2026 年 9 月 9 日

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev)、[Instagram](https://www.instagram.com/vscode.ig) 上追蹤我們

* * *

_發布日期：2026 年 9 月 9 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.137.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.137.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.137.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.137.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.137.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.137.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.137.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.137.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.137.0/linux-snap-x64/stable)

* * *

歡迎來到 Visual Studio Code 1.137 版本。這個版本可以協助你自動化重複性的工作、在工作區中接續 quick chat、用語音跟 agent 交談，並且不用離開 Agents 視窗就能處理 GitHub issue 與 pull request。

*   [The Story of VS Code](#the-story-of-vs-code)：觀看 VS Code 紀錄片，了解這個編輯器與它的社群一路走來是如何演進的。

*   [Automations（預覽）](#automations預覽)：排程重複性的 agent 任務，讓它每小時、每天或每週執行，也可以隨時手動執行。

*   [Voice Mode（實驗性）](#voice-mode實驗性)：在 agent 處理你的程式碼時跟它說話，並隨時打斷或改變它的方向。

*   [在工作區中接續 quick chat](#在工作區中接續-quick-chat)：把專案掛到既有的 quick chat 上，不會遺失對話紀錄與目前的請求。

*   [GitHub issue 與 pull request（實驗性）](#開啟-github-issue-與-pr-詳細資料實驗性)：直接在 Agents 視窗中檢視 issue 與 pull request 的詳細資料，就算沒有開啟該儲存庫也可以。

*   [活動：為 VS Code 寵物命名](#幫-vs-code-寵物取名字實驗性)：認識 VS Code 寵物，並參加為牠命名的活動。

> **別忘了參加 2026 年 9 月 10 日的 [GitHub Copilot Day](https://gh.io/githubcopilotday) 直播。** 學習使用 Copilot 最有效的方式，從 agent、模型選擇，到跨 GitHub、Copilot app、Copilot CLI 與 Visual Studio Code 的協作。

Happy Coding！

* * *

VS Code 會逐步推送給所有使用者。在 VS Code 中使用 **Check for Updates**（檢查更新）可以立刻取得最新版本。

想要盡快體驗新功能，可以[**下載每晚更新的 Insiders 版本**](https://code.visualstudio.com/insiders)，最新的更新一準備好就會包含進去。

* * *

## The Story of VS Code

探索 VS Code 背後的故事，從最初的起步，到今天數百萬名開發者使用的平台，以及一路上協助形塑它的社群。

[![以 VS Code 標誌為主視覺的海報，標誌在滿是程式碼的深色背景上發光，標題為「The Story of VS Code」。](https://code.visualstudio.com/assets/updates/1_136/the-story-of-vs-code.png)](https://aka.ms/the-story-of-vs-code)

## Agents

### Automations（預覽）

**設定**：`chat.automations.enabled`

Automations 會依排程執行重複性的 agent 任務，例行工作就不必手動啟動。你可以從範本開始，例如追蹤變更、分類 issue 或找出 bug，也可以自訂自己的 prompt 與排程。

想試用 automations，請啟用 `chat.automations.enabled`，開啟 Agents 視窗，然後在側邊欄選擇 **Automations**。你可以隨時手動執行 automation，或是排程成每小時、每天或每週執行。想了解更多，請參考文件中的 [automations](https://code.visualstudio.com/docs/agents/run/automations) 說明。

Automations 目前是預覽版，正逐步推送給所有使用者。

### 開啟 GitHub issue 與 PR 詳細資料（實驗性）

**設定**：`extensions.experimental.enableAgentsWindowCapability`

當 chat 對話提到 GitHub 上的工作時，Agents 視窗可以直接與 GitHub Pull Requests 擴充套件整合。選取 `github.com` 的 issue 或 pull request 連結，就能直接在 Agents 視窗中開啟它的詳細資料，不必切換到瀏覽器。就算你沒有開啟該儲存庫的工作區，這個功能一樣可以使用。

想試用這個初步整合，請在你預設的 VS Code 設定檔中安裝 [GitHub Pull Requests 擴充套件](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)，並啟用 `extensions.experimental.enableAgentsWindowCapability`。

### 在任何 chat 中附加 GitHub issue 與 pull request

在任何 chat 輸入框的 **Add Context...** 選單中，都可以把 GitHub issue 或 pull request 加入作為 context，包含 Chat 檢視、Chat 編輯器與 Agents 視窗。這讓 agent 可以取得 issue 描述、留言或 pull request 的變更內容，而不必把它們複製到你的 prompt 裡。

![顯示從 Add Context 選單搜尋 GitHub issue 的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_137/github-issue-pull-request-context.webp)

你也可以把 GitHub issue 或 pull request 的 URL 貼到新工作階段的輸入框。URL 會留在你的 prompt 中，同時自動加上 context 附件。

![顯示在 chat 中貼上 GitHub issue 與 pull request URL 並附帶 context 附件的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_137/paste-github-issue-pull-request.webp)

### 幫 VS Code 寵物取名字（實驗性）

實驗性的 VS Code 寵物是一個互動夥伴，會隨著你與 agent 的互動做出反應。在 chat 中輸入 `/vscode-pet` 就能認識牠，然後幫忙選出牠的名字。

![顯示 chat 中 VS Code 寵物介紹的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_137/toggle.webp)

從 2026 年 9 月 10 日到 9 月 17 日，可以[提交 VS Code 寵物的名字](https://forms.cloud.microsoft/r/4iFTRDnvaY)。請參閱[活動條款與細則](https://code.visualstudio.com/docs/agents/reference/chat-pet#_contest-terms-and-conditions)。

### 在工作區中接續 quick chat

你可以在 Agents 視窗中開始一段 quick chat，而不把它跟工作區綁在一起，例如用來討論一般性的問題或想法。如果對話後來變成跟特定專案有關，可以請 Copilot agent 掛上一個本機資料夾，然後在同一段 chat 中繼續。

在你確認工作區、並選擇要直接使用該資料夾或是建立獨立的 worktree 之後，這段 chat 就會變成工作區工作階段。這個工作階段會保留原本的標題、對話紀錄與目前的請求，工作區設定完成後，agent 會自動帶著專案檔案的存取權限繼續進行。

另一種做法是，在新的工作區工作階段中開始這份專案工作。

> **注意**：在工作區工作階段中接續 chat 對話，目前只有 Copilot harness 支援。

### Agent 排入佇列的訊息

讓多個 agent 平行工作，同時不打斷已經在處理請求的 chat。當 agent 使用 `send_message` 這個工作階段管理工具去聯絡一個忙碌中的 chat 時，VS Code 會把訊息排入佇列，等目前這一輪順利完成後再開始處理。

agent 可以為同一個 session 或另一個 session 中的 chat 排入多則訊息。VS Code 會依照送出的先後順序處理佇列中的訊息，讓多個 chat 的工作流程更可預期。

### Agent host

Agent host 讓你可以從多個 VS Code 視窗連到同一個 agent session。它以 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）為基礎，在專屬的處理程序中執行 agent harness。Agent host 的 Copilot agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，讓 agent 的行為與功能跟 Copilot CLI、獨立的 GitHub Copilot app 以及其他 Copilot 產品保持一致。

我們正在積極開發 agent host。想了解更多，請參考 [agent host 文件](https://code.visualstudio.com/docs/agents/concepts/agent-host)以及我們的 [agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)，我們在裡面分享了為什麼要打造 agent host、它在 VS Code 中帶來哪些可能、架構與這個開放協定如何運作，以及你可以自己動手試的工作流程。

如果你有任何回饋或需求，歡迎透過[提交 issue](https://github.com/microsoft/vscode/issues) 讓我們知道。

## Chat

### Voice Mode（實驗性）

**設定**：`agents.voice.enabled`、`agents.voice.showTranscript`、`agents.voice.voice`

Voice Mode 讓你在 agent 處理程式碼的同時，用自然的口語跟它對話。

想試用的話，啟用 `agents.voice.enabled`，然後在 chat 輸入框中選擇 **Voice Mode** 按鈕。

當 agent 正在說話時，你可以直接開口，或使用 push-to-talk 快捷鍵打斷它的回應並繼續對話。

Voice Mode 知道你目前的工作階段狀態，可以回答關於執行中的 session、選用的模型與已附加檔案的問題。你也可以要求 Voice Mode 開一個新的 session。當請求被路由時，Voice Mode 會說明它是把請求送到既有的 session，還是開了一個新的。

你可以用以下幾種方式自訂 Voice Mode：

*   啟用 `agents.voice.showTranscript`，在 Voice Mode 進行中於 chat 輸入框顯示對話逐字稿。使用 Voice Mode 控制項可以顯示或隱藏逐字稿、把麥克風靜音或取消靜音，而不需要結束語音工作階段。
*   從命令面板執行 **Chat: Dictate: Select Microphone**，選擇語音聽寫與 Voice Mode 共同使用的輸入裝置。
*   使用 `agents.voice.voice` 選擇朗讀回應的語音。
*   從命令面板執行 **Voice Mode: Show Introduction**，重新開啟導覽介紹，在裡面可以選擇麥克風並試聽可用的語音。

你也可以在 chat 輸入框的 **Voice Mode** 按鈕上按右鍵，快速存取它的設定、使用說明、導覽介紹、麥克風選擇與逐字稿控制項。想了解更多，請參考[使用 Voice Mode](https://code.visualstudio.com/docs/configure/accessibility/voice#use-voice-mode) 的說明。

管理員可以透過關閉組織的 Copilot 預覽功能來停用 Voice Mode。

## 程式碼編輯

### 智慧型 diff 編輯器版面配置

一般 diff、多檔 diff 以及 Agents 視窗的 **Changes** 編輯器，現在都用相同的方式選擇 diff 版面配置。開啟 **More Actions**（**...**）> **Diff View**，然後選擇 **Inline**、**Side by Side** 或 **Automatic**。

**Automatic** 選項會顯示目前生效的是哪一種版面配置，並隨著編輯器寬度變化自動調整。如果你刻意拖曳 sash 把 inline diff 拉寬，在調整大小的過程中 diff 會維持 inline，內容才不會突然位移。

### 多檔 diff 中的二進位檔案

有變更的二進位檔案（例如圖片）現在會保留在多檔 diff 中，不會被略過。Diff 會在該檔案原本的位置顯示 **Binary file changed** 佔位提示。選擇 **Open Diff** 就能用標準的 diff 體驗檢視這個檔案，例如圖片 diff 或適用的自訂編輯器。

![顯示多檔 diff 中二進位檔案佔位提示與 Open Diff 動作的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_137/binary-files-multi-diff.webp)

編輯器視窗中的多檔 diff 也採用了與 Agents 視窗多檔 diff 相同的視覺設計。

## 程式語言

### Markdown 編輯器中的 GitHub 連結（實驗性）

**設定**：`markdown.experimental.richLinks.enabled`、`chat.experimental.richLinks.enabled`

Markdown 編輯器中的 GitHub issue 與 pull request 連結會顯示目前的標題與狀態，不必先開啟就能了解每個參照的內容。當 issue 或 pull request 的狀態改變時（包含 CI 的更新），連結也會跟著更新。

![顯示 Markdown 編輯器中 GitHub pull request 以豐富連結呈現並帶有即時狀態指示的螢幕截圖。](https://code.visualstudio.com/assets/updates/1_137/markdown-github-rich-links.webp)

想在 chat 中使用同樣的連結呈現方式，把 `chat.experimental.richLinks.enabled` 設為 `true`。

## 擴充套件的貢獻

### GitHub Pull Requests

[GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) 擴充套件又有新的進展，這個擴充套件讓你可以處理、建立與管理 pull request 和 issue。新功能包含：

*   更快開啟 pull request webview
*   VS Code 中所有的 github.com 連結都會用這個擴充套件開啟（可以用 `githubPullRequests.openPullLinks` 停用）

想了解這次發布的全部內容，請查看擴充套件 [0.166.0 版的變更紀錄](https://github.com/microsoft/vscode-pull-request-github/blob/main/CHANGELOG.md#01660)。

## 已棄用的功能與設定

無

## 感謝

對 `vscode` 的貢獻：

*   [@accnops (Arthur Cnops)](https://github.com/accnops)：語音聽寫改用專屬的 MAI 轉錄 [PR #334042](https://github.com/microsoft/vscode/pull/334042)
*   [@arpankanwer (Birarpanjot Singh Kanwer)](https://github.com/arpankanwer)：fix(copilot)：不再自動重試已過期的影像附件，改為顯示可據以處理的錯誤 [PR #334129](https://github.com/microsoft/vscode/pull/334129)
*   [@bstee615 (Benjamin Steenhoek)](https://github.com/bstee615)
    *   在 PatchBased02Unified 中辨識 eagerness 選項 [PR #333606](https://github.com/microsoft/vscode/pull/333606)
    *   新增 PatchBased02UnifiedEagerness prompt 策略 [PR #333302](https://github.com/microsoft/vscode/pull/333302)
*   [@hadley (Hadley Wickham)](https://github.com/hadley)：修正 webview view badge 設為 undefined 時沒有清除的問題 [PR #331019](https://github.com/microsoft/vscode/pull/331019)
*   [@jmymay (Jeremy Majewski)](https://github.com/jmymay)：在 platforms.ts 與 context 中加入 isChromeOS [PR #309097](https://github.com/microsoft/vscode/pull/309097)
*   [@joshspicer](https://github.com/joshspicer)
    *   agentHost：修正 shell 路徑與讀取提示的受管理權限問題 [PR #333249](https://github.com/microsoft/vscode/pull/333249)
    *   受管理設定：更常重用快取的 policy [PR #333697](https://github.com/microsoft/vscode/pull/333697)
    *   改善 mock policy server 的疑難排解 [PR #333930](https://github.com/microsoft/vscode/pull/333930)
    *   以迴歸測試確保受管理設定的穩定性 [PR #334056](https://github.com/microsoft/vscode/pull/334056)
    *   優化 mock policy server 的 UI [PR #334122](https://github.com/microsoft/vscode/pull/334122)
    *   移除舊的 tool-rename-deprecation skill [PR #334279](https://github.com/microsoft/vscode/pull/334279)
    *   從 mock-policy-server 移除範例 [PR #334361](https://github.com/microsoft/vscode/pull/334361)
*   [@kondv](https://github.com/kondv)：mcp：避免因為等效的 URI 定義而停止 server [PR #333443](https://github.com/microsoft/vscode/pull/333443)
*   [@leep-frog](https://github.com/leep-frog)：為 `editor.action.formatDocument.multiple` 加上 args [PR #245743](https://github.com/microsoft/vscode/pull/245743)
*   [@mcumming (Michael Cummings (MSFT))](https://github.com/mcumming)
    *   啟用 Microsoft Entra ID 登入以存取 Private Marketplace [PR #325331](https://github.com/microsoft/vscode/pull/325331)
    *   讓 extensions.gallery.authProvider 可由 policy 控制 [PR #333837](https://github.com/microsoft/vscode/pull/333837)
*   [@mst-mkt (keito)](https://github.com/mst-mkt)：修正箭頭函式後用括號包住選取範圍的問題（#\_225916）[PR #321210](https://github.com/microsoft/vscode/pull/321210)
*   [@piyushmadan (Piyush Madan)](https://github.com/piyushmadan)：向執行中的 subagent 顯示剩餘的回合數 [PR #332704](https://github.com/microsoft/vscode/pull/332704)
*   [@RajeshKumar11](https://github.com/RajeshKumar11)：修正 extensions.allowed schema 把合法的版本陣列判定為不合法的問題 [PR #329744](https://github.com/microsoft/vscode/pull/329744)
*   [@rohit489 (Rohit Agrawal - MSFT)](https://github.com/rohit489)：在 chat 遙測事件中記錄 CAPI 的 X-Copilot-Service-Request-Id [PR #334638](https://github.com/microsoft/vscode/pull/334638)
*   [@RyanEwen (Ryan Ewen)](https://github.com/RyanEwen)：把訊息為空的瀏覽器工具錯誤回報為失敗 [PR #334311](https://github.com/microsoft/vscode/pull/334311)
*   [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
    *   fix：修正 git 分支保護 provider 的記憶體洩漏 [PR #333381](https://github.com/microsoft/vscode/pull/333381)
    *   fix：修正 extension host 虛擬終端機的記憶體洩漏 [PR #333397](https://github.com/microsoft/vscode/pull/333397)
    *   fix：修正無障礙檢視工具列的記憶體洩漏 [PR #333183](https://github.com/microsoft/vscode/pull/333183)
    *   fix：修正測試結果的記憶體洩漏 [PR #333244](https://github.com/microsoft/vscode/pull/333244)
    *   fix：修正 extension host comments 的記憶體洩漏 [PR #334095](https://github.com/microsoft/vscode/pull/334095)
    *   fix：修正 mainThreadNotebook 的記憶體洩漏 [PR #334189](https://github.com/microsoft/vscode/pull/334189)
    *   fix：修正終端機設定檔服務的記憶體洩漏 [PR #334100](https://github.com/microsoft/vscode/pull/334100)
*   [@vscodebot-pr (VS Code PR Bot)](https://github.com/vscodebot-pr)
    *   fix：夾限 ListView 中反轉的可見範圍以避免 RangeError（修正 #333230）[PR #333236](https://github.com/microsoft/vscode/pull/333236)
    *   fix：在 dispose 時取消單一窗格 docked tab 尚未完成的調和作業（修正 #333537）[PR #333541](https://github.com/microsoft/vscode/pull/333541)
    *   fix：在 copilotcli 診斷推送中防範 null 的 diagnostic code（修正 #333772）[PR #333781](https://github.com/microsoft/vscode/pull/333781)
    *   fix：測試環境下的 Voice Mode 導覽預覽略過真實的 AudioContext（vscode-engineering#3742 的建置修正）[PR #333858](https://github.com/microsoft/vscode/pull/333858)
*   [@wibaek (Wibaek Park)](https://github.com/wibaek)：fix：避免在 IME 組字期間重新聚焦搜尋輸入框 [PR #320898](https://github.com/microsoft/vscode/pull/320898)
*   [@YOSHII-Hiroto (吉井　啓人（YOSHII, Hiroto）)](https://github.com/YOSHII-Hiroto)：支援在內建瀏覽器中開啟 MHTML 檔案 [PR #333307](https://github.com/microsoft/vscode/pull/333307)

對 `vscode-pull-request-github` 的貢獻：

*   [@jameswilmiller (James Miller)](https://github.com/jameswilmiller)：在樹狀檢視中永遠顯示 commit SHA [PR #8840](https://github.com/microsoft/vscode-pull-request-github/pull/8840)
*   [@tamird (Tamir Duberstein)](https://github.com/tamird)
    *   要求舊版 GraphQL 查詢必須帶入變數 [PR #8889](https://github.com/microsoft/vscode-pull-request-github/pull/8889)
    *   以較小的分頁重試 review 留言 [PR #8890](https://github.com/microsoft/vscode-pull-request-github/pull/8890)

### Issue 追蹤

對我們 issue 追蹤的貢獻：

*   [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
*   [@RedCMD (RedCMD)](https://github.com/RedCMD)
*   [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
*   [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

* * *

我們非常感謝大家願意在新功能一準備好就試用，記得常回來看看有什麼新東西。

> 如果你想閱讀先前 VS Code 版本的版本說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 的 [Updates](https://code.visualstudio.com/updates) 頁面。
