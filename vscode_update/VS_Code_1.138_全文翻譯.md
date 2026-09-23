# VS Code 1.138 全文翻譯

> 原文：<https://code.visualstudio.com/updates/v1_138>
> 發布日期：2026 年 9 月 16 日

---

[🎬 觀看《The Story of VS Code》！](https://aka.ms/the-story-of-vs-code?source=vsc-website-banner)

[部落格：透過 Agent Host 啟用持續且可攜的 agent 工作階段。](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture?source=vsc-website-banner)

[9 月 10 日加入 GitHub Copilot Day 直播！](https://gh.io/githubcopilotday?source=vsc-website-banner)

關閉此更新公告

# Visual Studio Code 1.138

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev)、[Instagram](https://www.instagram.com/vscode.ig) 追蹤我們

* * *

_發布日期：2026 年 9 月 16 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.138.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.138.0/win32-arm64-user/stable) \| Mac：[Universal](https://update.code.visualstudio.com/1.138.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.138.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.138.0/darwin-arm64-dmg/stable) \| Linux：[deb](https://update.code.visualstudio.com/1.138.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.138.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.138.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.138.0/linux-snap-x64/stable)

* * *

歡迎使用 Visual Studio Code 1.138 版。本版本協助 agent 在你專案的開發環境中工作、給 Codex 工作階段更多彈性，並讓已完成的工作階段保持整齊。

- [Dev Container 中的 agent 工作階段](https://code.visualstudio.com/updates/v1_138#_run-agent-sessions-in-local-dev-containers)：在本機 Dev Container 中，使用你專案的工具與相依套件執行 agent。

- [擴充的 Codex harness](https://code.visualstudio.com/updates/v1_138#_expanded-codex-support-in-the-agent-host)：跨應用程式接續 Codex 工作階段、在 Copilot 與 ChatGPT 訂閱之間選擇，並使用 VS Code 工具。

- [工作階段清理（預覽）](https://code.visualstudio.com/updates/v1_138#_keep-completed-sessions-organized-preview)：自動將已合併的工作階段標記為完成，並可選擇在寬限期後刪除。


_這份版本說明是使用 GitHub Copilot 產生的，可能含有不精確之處。_

Happy Coding！

* * *

VS Code 正逐步推送給所有使用者。在 VS Code 中使用 **Check for Updates** 可立即取得最新版本。

若想儘早嘗試新功能，請[**下載每夜建置的 Insiders 版本**](https://code.visualstudio.com/insiders)，其中包含最新的更新內容。

* * *

## [Agents](https://code.visualstudio.com/updates/v1_138\#_agents)

[Agent host](https://code.visualstudio.com/docs/agents/concepts/agent-host) 以 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）為基礎，在專用的程序中執行 agent harness，因此你可以從多個 VS Code 視窗連線到同一個工作階段。想進一步了解其架構與工作流程，請參閱 [agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)。

### [在本機 Dev Container 中執行 agent 工作階段](https://code.visualstudio.com/updates/v1_138\#_run-agent-sessions-in-local-dev-containers)

**設定**：
chat.agentHost.devContainer.enabled

在 VS Code 開啟／在 VS Code Insiders 開啟（僅限 Agents 視窗）

在本機資料夾的 Dev Container 內執行工作階段，讓 agent 的工作與專案的工具鏈保持一致。Agent 會使用專案所設定的環境與相依套件，而不是你本機上的環境。

啟用此設定後，具備支援的 Dev Container 設定的本機資料夾會自動顯示資料夾選單，其中包含 **Use Dev Container** 動作。選取此動作即可在該資料夾的 Dev Container 中執行 agent 工作階段。你的機器上必須安裝 Docker。

![螢幕截圖：工作區選擇器中某個資料夾顯示 Use Dev Container 動作。](https://code.visualstudio.com/assets/updates/1_138/dev-container-workspace-picker.webp)

> **注意**：本機 Dev Container 工作階段正在逐步推出，因此這個設定對你來說可能尚未預設啟用。你可以手動啟用設定，立即試用此功能。

### [Agent host 中擴充的 Codex 支援](https://code.visualstudio.com/updates/v1_138\#_expanded-codex-support-in-the-agent-host)

**設定**：
chat.agentHost.codexAgent.enabled
在 VS Code 開啟／在 VS Code Insiders 開啟，
chat.editor.codex.preferAgentHost

在 VS Code 開啟／在 VS Code Insiders 開啟

本版本擴充了 agent host 中的 Codex 支援，讓你在變更工作地點與方式時，仍能維持同一個程式撰寫工作階段。

- **選擇你的訂閱**：可以使用 GitHub Copilot 訂閱或 ChatGPT 訂閱來使用 Codex。若兩者都已登入，可以在模型選擇器中於 Copilot 支援的模型與 ChatGPT 支援的模型之間切換，而不會遺失目前的對話。
- **跨應用程式接續**：在 ChatGPT 應用程式與 VS Code 之間移動同一個 Codex 工作階段，而不必重新開始對話。
- **從 VS Code 與桌面應用程式互動**：若 ChatGPT 應用程式已安裝並設定為可進行電腦操作（computer use），VS Code 中的 Codex harness 可以重用該設定，與你電腦上的應用程式互動。這對來自 Copilot 訂閱或 ChatGPT 訂閱的模型都適用。
- **使用 VS Code 工具**：Codex 可以使用 [VS Code 提供的全部工具](https://code.visualstudio.com/docs/agents/concepts/tools)，包含內建工具、擴充套件工具與 MCP 工具。使用 ChatGPT 支援的模型時，Codex 還可以直接在工作階段中使用其圖片生成工具。

![螢幕截圖：Codex 工作階段從 ChatGPT 應用程式交接到 VS Code 並生成圖片。](https://code.visualstudio.com/assets/updates/1_138/codex-session-handoff-and-image-generation.webp)

### [在工作區中接續 Codex quick chat](https://code.visualstudio.com/updates/v1_138\#_continue-codex-quick-chats-in-a-workspace)

上一版我們為 Copilot 工作階段推出了[在工作區中接續 quick chat](https://code.visualstudio.com/updates/v1_137#_continue-quick-chats-in-a-workspace)。本版本將同樣的流程延伸到 Codex，因此開始專案專屬的工作時，不再需要放棄沒有工作區的 Codex quick chat。請 Codex 附加一個本機資料夾，然後選擇直接使用該資料夾或建立一個獨立的 worktree。

確認變更後，同一個 chat 與原生 Codex thread 會成為工作區工作階段。工作階段會保留標題、對話歷史、目前的請求、所選模型與權限模式。Codex 接著會以可存取專案檔案的狀態繼續處理請求。

工作區轉換適用於 Interactive 模式下閒置的 Codex quick chat，並支援單一根目錄工作區目標。若變更被取消或無法套用，原本沒有工作區的 chat 仍可繼續使用。

### [隨需下載 agent SDK](https://code.visualstudio.com/updates/v1_138\#_on-demand-agent-sdk-downloads)

Agents 視窗中的 Claude 與 Codex agent 依賴一個未隨 VS Code 提供的 SDK，會在你第一次需要時下載。先前，下載提示只會在未登入的帳號設定流程中出現，因此若你已登入，SDK 會在第一則訊息時才下載。

現在只要 SDK 缺失，就會顯示下載提示，讓你清楚知道需要下載才能繼續。

![螢幕截圖：Agents 視窗中 chat 輸入框上方顯示「Download the Claude Agent」通知，附有 Download 動作，且 harness 選擇器中已選取 Claude agent。](https://code.visualstudio.com/assets/updates/1_138/agent-sdk-download-offer.jpg)

當該 agent 的模型已可使用時，通知會提供兩個選項：選取 **Download**，或直接傳送訊息，將 SDK 下載作為該回合的一部分。若通知顯示期間模型變為可用，文字內容會就地更新。

### [從 agent 工作階段建立 pull request](https://code.visualstudio.com/updates/v1_138\#_create-pull-requests-from-agent-sessions)

**設定**：
chat.agentMerge.enabled

在 VS Code 開啟／在 VS Code Insiders 開啟（選用，僅供 Agent Merge 使用）

在 Agents 視窗中，從 [Agent Host 工作階段](https://code.visualstudio.com/docs/agents/concepts/agent-host)建立 pull request，使用單一表單即可檢視並編輯產生的標題與描述、選擇草稿狀態，並設定可用的合併選項。可以直接建立 pull request，或將請求交給你的 agent 處理，你偏好的選項會被記住供下次使用。

> **注意**：**Agent Merge** 選項為實驗性功能，只有在啟用上述設定時才可使用。

![螢幕截圖：Create PR 表單，含可編輯的標題與描述、草稿與合併選項，以及用來建立 pull request 的分割按鈕。](https://code.visualstudio.com/assets/updates/1_138/create-pull-request-form.webp)

### [讓已完成的工作階段保持整齊（預覽）](https://code.visualstudio.com/updates/v1_138\#_keep-completed-sessions-organized-preview)

**設定**：
chat.agentSessions.archiveNudge.enabled

在 VS Code 開啟／在 VS Code Insiders 開啟，
chat.agentSessions.autoMarkAsDoneMergedSessionsAfterDays

在 VS Code 開啟／在 VS Code Insiders 開啟，
chat.agentSessions.autoDeleteArchivedMergedSessionsAfterDays

在 VS Code 開啟／在 VS Code Insiders 開啟，
sessions.markAsDoneConfetti

在 VS Code 開啟／在 VS Code Insiders 開啟（僅限 Agents 視窗）

把已完成的工作移開視線，同時保留對話供日後查閱。當某個閒置工作階段的所有 pull request 都已合併，Agents 視窗可以建議將該工作階段標記為完成。首次使用時的導覽會告訴你在工作階段清單中哪裡可以找到 **Mark as Done**。

啟用
chat.agentSessions.archiveNudge.enabled

在 VS Code 開啟／在 VS Code Insiders 開啟 即可顯示這些建議。

![螢幕截圖：pull request 合併後，建議將工作階段標記為完成。](https://code.visualstudio.com/assets/updates/1_138/mark-session-as-done.webp)

若要自動清理，可在 pull request 合併後將閒置工作階段標記為完成，並可選擇在另一段寬限期後刪除。這兩個自動清理設定預設都是關閉的。

當某個工作階段的所有 pull request 都已合併時，在 **Mark as Done** 建議中選取 **Configure Automatic Cleanup**，可以開啟這兩個設定而不直接啟用它們。

啟用
sessions.markAsDoneConfetti

在 VS Code 開啟／在 VS Code Insiders 開啟 即可在你將工作階段標記為完成時顯示彩帶動畫。此動畫會尊重你的減少動態效果偏好。

### [查看哪些工作階段需要注意（預覽）](https://code.visualstudio.com/updates/v1_138\#_see-which-sessions-need-attention-preview)

**設定**：
sessions.showApplicationBadge

在 VS Code 開啟／在 VS Code Insiders 開啟（僅限 Agents 視窗）

不必切換回 VS Code，就能透過 macOS Dock、Linux 啟動器或 Windows 工作列上的徽章，得知 agent 工作階段需要你的注意。徽章會標示有新結果、要求輸入，或 pull request 檢查需要處理的工作階段。

啟用上述預覽設定即可顯示徽章。

![螢幕截圖：Windows 工作列上的應用程式圖示顯示徽章，表示有一個工作階段需要注意。](https://code.visualstudio.com/assets/updates/1_138/sessions-application-badge.webp)

### [統一的工作區與儲存庫選擇器（實驗性）](https://code.visualstudio.com/updates/v1_138\#_unified-workspace-and-repository-picker-experimental)

**設定**：
sessions.chat.unifiedWorkspacePicker.enabled

在 VS Code 開啟／在 VS Code Insiders 開啟（僅限 Agents 視窗）

從單一可搜尋清單中開始 agent 工作，清單包含本機資料夾、GitHub 儲存庫、Cloud 儲存庫與遠端目標。遠端連線動作仍可從 **Remote** 項目使用。

![螢幕截圖：統一的工作區選擇器，含本機資料夾、儲存庫與遠端選項。](https://code.visualstudio.com/assets/updates/1_138/unified-workspace-picker.webp)

選取 **Work in Repository** 會採用 cloud 優先的工作流程。尚未存在於本機的 GitHub 儲存庫會立即以 Cloud harness 選取，不會出現 clone 提示。若你之後選擇本機 harness，VS Code 會提示你 clone 該儲存庫；若你取消，則保留 cloud 選擇。

### [依色彩主題自訂 chat 背景（實驗性）](https://code.visualstudio.com/updates/v1_138\#_customize-chat-backgrounds-by-color-theme-experimental)

**設定**：
chat.agentSessions.preferredDarkBackgroundImageLayout

在 VS Code 開啟／在 VS Code Insiders 開啟，
chat.agentSessions.preferredLightBackgroundImageLayout

在 VS Code 開啟／在 VS Code Insiders 開啟（僅限 Agents 視窗）

Agents 視窗可以在你的工作階段後方顯示裝飾性的 chat 背景，可以是內建 VS Code 圖示組成的圖樣，也可以是你自己的圖片，並且可以為深色與淺色主題分別選擇。

![螢幕截圖：Agents 視窗使用 Codicons 圖樣作為 chat 背景，agent 回應仍清楚可讀。](https://code.visualstudio.com/assets/updates/1_138/agents-chat-background-codicons.webp)

先前，圖片是依主題類型分別設定的，但版面配置不是，因此若你把深色背景設為靠右對齊、再把淺色背景設為靠左對齊，兩者最後都會變成靠左。現在版面配置會隨圖片一起依主題類型儲存，在深色與淺色主題之間切換時，會還原該主題的位置。這兩個新設定取代了 `chat.agentSessions.backgroundImageLayout`。

清除背景的方式也移動了。**Chat: Set Background...** 現在以 **No Background** 作為第一個選項，它會清除你目前使用的色彩主題的背景，而不影響另一個主題。

回應與請求的區塊是不透明的，因此較高對比的背景不會降低 agent 回應的可讀性。

![螢幕截圖：Agents 視窗使用自訂照片作為 chat 背景，agent 回應以不透明的氣泡呈現在上方，仍清楚可讀。](https://code.visualstudio.com/assets/updates/1_138/agents-chat-background-image.jpg)

## [Chat](https://code.visualstudio.com/updates/v1_138\#_chat)

### [改善 Voice Mode 的工作階段感知（實驗性）](https://code.visualstudio.com/updates/v1_138\#_improved-voice-mode-session-awareness-experimental)

不必離開 Voice Mode，就能瀏覽並監控平行進行的 agent 工作。Voice Mode 可以找出最近的 agent 工作階段、依標籤在它們之間切換，並回報每個工作階段的狀態。

## [棄用的功能與設定](https://code.visualstudio.com/updates/v1_138\#_deprecated-features-and-settings)

### [本版本新增的棄用項目](https://code.visualstudio.com/updates/v1_138\#_new-deprecations-in-this-release)

- `chat.agentSessions.backgroundImageLayout` 由
chat.agentSessions.preferredDarkBackgroundImageLayout

在 VS Code 開啟／在 VS Code Insiders 開啟 與
chat.agentSessions.preferredLightBackgroundImageLayout

在 VS Code 開啟／在 VS Code Insiders 開啟 取代，讓 Agents 視窗的 chat 背景版面配置可以為深色與淺色主題分別設定。
- **Chat: Clear Background** 已移除。請在 **Chat: Set Background...** 中選擇 **No Background**，以清除目前色彩主題的背景。

## [感謝](https://code.visualstudio.com/updates/v1_138\#_thank-you)

對 `vscode` 的貢獻：

- [@denizguney (Deniz Güney Yıldırım)](https://github.com/denizguney)：更新 files.exclude 設定的自動完成測試 [PR #332809](https://github.com/microsoft/vscode/pull/332809)
- [@DhineshPonnarasan (Dhinesh Ponnarasan)](https://github.com/DhineshPonnarasan)：修正已完成的進度通知無法關閉的問題 [PR #315184](https://github.com/microsoft/vscode/pull/315184)
- [@jacobjove (Jacob T. Jove)](https://github.com/jacobjove)：修正未被消化的緩衝 pty host 服務事件造成主程序 OOM 的問題 [PR #323980](https://github.com/microsoft/vscode/pull/323980)
- [@RyanEwen (Ryan Ewen)](https://github.com/RyanEwen)：讓 Codex MCP 工具進度不進入工具結果 [PR #334053](https://github.com/microsoft/vscode/pull/334053)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)：fix：markers 表格的記憶體洩漏 [PR #333241](https://github.com/microsoft/vscode/pull/333241)
- [@vladstudio (Vlad Gerasimov)](https://github.com/vladstudio)：修正 terminal-editor 的 shift-drop 在已分離的執行個體上觸發的問題 [PR #318756](https://github.com/microsoft/vscode/pull/318756)
- [@vscodebot-pr (VS Code PR Bot)](https://github.com/vscodebot-pr)：fix：在原生 policy smoke fixture 中解析 win32 的 product.json 路徑（vscode-engineering#3813 的建置修正）[PR #335152](https://github.com/microsoft/vscode/pull/335152)
- [@yutotnh (yutotnh)](https://github.com/yutotnh)：強制 commit 訊息輸出使用 UTF-8 [PR #331087](https://github.com/microsoft/vscode/pull/331087)
- [@zhichli (Zhichao Li)](https://github.com/zhichli)：docs：讓 OTel 指引與 Agent Host 架構一致 [PR #335175](https://github.com/microsoft/vscode/pull/335175)

對 `vscode-chat-customizations-evaluation` 的貢獻：

- [@JakLuminth (Jacob Searcy)](https://github.com/JakLuminth)：修正使用 log 輸出頻道時語言用戶端的啟動問題 [PR #286](https://github.com/microsoft/vscode-chat-customizations-evaluation/pull/286)

### [Issue tracking](https://code.visualstudio.com/updates/v1_138\#_issue-tracking)

對 issue tracking 的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

* * *

我們非常感謝大家在新功能一準備好時就立即試用，請常回來這裡看看有什麼新內容。

> 若想閱讀先前 VS Code 版本的版本說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 的 [Updates](https://code.visualstudio.com/updates) 頁面。

[回到頂端](https://code.visualstudio.com/updates/v1_138# "回到頂端")

## 說明與支援

### 仍需要協助？

- [向社群提問](https://stackoverflow.com/questions/tagged/vscode)
- [提出功能需求](https://github.com/microsoft/vscode/issues/new/choose)
- [回報問題](https://www.github.com/Microsoft/vscode/issues)

### 協助我們改進

編輯此頁面

- [RSS Feed](https://code.visualstudio.com/feed.xml)
- [提問](https://stackoverflow.com/questions/tagged/vscode)
- [追蹤 @code](https://go.microsoft.com/fwlink/?LinkID=533687)
- [提出功能需求](https://github.com/microsoft/vscode/issues/new/choose)
- [回報問題](https://www.github.com/microsoft/vscode/issues)
- [觀看影片](https://www.youtube.com/channel/UCs5Y5_7XK8HLDX0SLNwkd3w)
