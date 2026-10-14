# Visual Studio Code 1.140

- 發行日期：2026 年 9 月 30 日（Stable）
- 原文連結：<https://code.visualstudio.com/updates/v1_140>

## 1.140.0 下載

- Windows：[x64](https://update.code.visualstudio.com/1.140.0/win32-x64-user/stable)｜[Arm64](https://update.code.visualstudio.com/1.140.0/win32-arm64-user/stable)
- macOS：[Universal](https://update.code.visualstudio.com/1.140.0/darwin-universal-dmg/stable)｜[Intel](https://update.code.visualstudio.com/1.140.0/darwin-x64-dmg/stable)｜[Apple silicon](https://update.code.visualstudio.com/1.140.0/darwin-arm64-dmg/stable)
- Linux：[.deb](https://update.code.visualstudio.com/1.140.0/linux-deb-x64/stable)｜[.rpm](https://update.code.visualstudio.com/1.140.0/linux-rpm-x64/stable)｜[.tar.gz](https://update.code.visualstudio.com/1.140.0/linux-x64/stable)｜[Arm 說明](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions)｜[Snap](https://update.code.visualstudio.com/1.140.0/linux-snap-x64/stable)

已經安裝了嗎？請在 VS Code 中使用 **Check for Updates**。想搶先體驗即將推出的功能，請使用 [Insiders 組建](https://code.visualstudio.com/insiders)。

## [版本亮點](https://code.visualstudio.com/updates/v1_140#_release-highlights)

本版擴充了代理程式（agent）工作流程、改善 worktree 的重複使用，並新增企業 AI 控管。

- [Copilot harness](https://code.visualstudio.com/updates/v1_140#_copilot-harness)：使用 Copilot harness，在各項 Copilot 產品之間獲得一致的代理程式行為。
- [多資料夾工作階段（實驗性）](https://code.visualstudio.com/updates/v1_140#_multi-folder-sessions-experimental)：在單一代理程式工作階段中，跨多個資料夾處理工作。
- [遠端委派（實驗性）](https://code.visualstudio.com/updates/v1_140#_delegate-tasks-to-remote-agent-hosts-experimental)：將工作委派給已連線的遠端 agent host。
- [HydraFusion（研究預覽）](https://code.visualstudio.com/updates/v1_140#_hydrafusion-model-orchestration-research-preview)：讓多個模型為程式設計工作撰寫草稿、提出評論並修改，不需要你自行協調。
- [共用 worktree 資料夾（實驗性）](https://code.visualstudio.com/updates/v1_140#_reuse-ignored-folders-across-worktrees-experimental)：在 worktree 之間重複使用被忽略的資料夾，避免重複安裝相依套件與產生重複的建置產物。
- [企業控管](https://code.visualstudio.com/updates/v1_140#_enterprise)：確保組織中的每個人都遵循 AI 的版本需求與預設值。

---

## [代理程式（Agents）](https://code.visualstudio.com/updates/v1_140#_agents)

### [Copilot harness](https://code.visualstudio.com/updates/v1_140#_copilot-harness)

Copilot harness 為 VS Code 加入令人振奮的全新代理程式功能，同時讓你維持熟悉的工作方式。它由 Copilot SDK 驅動，因此其行為與功能和其他 Copilot 產品一致，包括獨立的 GitHub Copilot 應用程式與 Copilot CLI。

要開始使用 Copilot harness，請在聊天輸入框的 harness 選擇器中選取它。在本版中，它可能已經是你的預設選項。你可以照平常的方式繼續工作。

![畫面截圖：harness 選擇器中已選取 Copilot harness。](https://code.visualstudio.com/assets/updates/1_140/Harness-Copilot.webp)

這個 harness 在以 Agent Host Protocol（AHP）為基礎的專屬 [agent host](https://code.visualstudio.com/docs/agents/concepts/agent-host) 處理程序中執行，因此你可以從多個 VS Code 視窗連線到同一個代理程式工作階段。

了解如何[使用 Copilot harness](https://code.visualstudio.com/docs/agents/run/agent-harnesses#use-the-copilot-harness)，或在 [agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)中探索其架構與工作流程。如果你有回饋或需求，[請提交 issue](https://github.com/microsoft/vscode/issues)。

### [HydraFusion 模型協調（研究預覽）](https://code.visualstudio.com/updates/v1_140#_hydrafusion-model-orchestration-research-preview)

[HydraFusion](https://github.blog/ai-and-ml/github-copilot/project-hydrafusion-frontier-quality-via-multi-model-orchestration/) 是一套自適應的模型協調系統，會為每個程式設計工作選擇模型與工作流程。它可以用單一模型解決工作、升級到更強的模型，或讓另一個模型評論並修改結果。這個做法的目標是在兼顧速度與成本的同時提升結果品質，而且不需要你自行協調各個模型。

已啟用預覽功能的合格使用者，可以在模型選擇器中選取 HydraFusion。

### [多資料夾工作階段（實驗性）](https://code.visualstudio.com/updates/v1_140#_multi-folder-sessions-experimental)

多資料夾工作階段讓你在單一工作階段中，協調跨儲存庫或跨獨立 worktree 的相關工作。以往在[多聊天工作階段](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions#_run-multiple-chats-in-a-session)中，每個聊天都共用同一個資料夾與 checkout。在本版中，每個聊天都可以使用自己的資料夾或 worktree，各聊天之間的變更不會互相滲漏。

每個聊天會將其資料夾用於自己的終端機、工作（tasks）、變更、提取要求（pull request）與 Agent merge 狀態。使用相同資料夾的聊天會共用這些狀態。在工作階段清單中，將滑鼠停留在某個工作階段上，可以看到它所有資料夾的摘要；停留在巢狀聊天上，則只會看到該聊天所屬資料夾的詳細資訊。

![畫面截圖：工作階段的停留提示，顯示主聊天的工作區、worktree、分支與提取要求，以及列出兩個儲存庫及其提取要求的 Session summary。](https://code.visualstudio.com/assets/updates/1_140/multi-folder-session-hover.webp)

範例使用情境包括：

- **跨儲存庫實作功能**：請主聊天在某個儲存庫中工作，並為第二個儲存庫建立一個對等聊天（peer chat）。
- **在不同 worktree 中比較不同做法**：請主聊天建立多個對等聊天，每個都使用同一個儲存庫的全新 worktree。每種做法都會有自己的分支、變更與提取要求。

多資料夾工作階段為實驗性功能，預設停用。這些設定不會顯示在設定編輯器中。請在使用者範圍的 `settings.json` 檔案中，將你所用 harness 對應的設定設為 `true`，然後建立工作階段：

- Copilot：`chat.agentHost.copilotAgent.multiRootEnabled`
- Claude：`chat.agentHost.claudeAgent.multiRootEnabled`
- Codex：`chat.agentHost.codexAgent.multiRootEnabled`

新的工作階段會套用設定變更，不需要重新啟動 Agent Host。目前沒有用來新增資料夾或選擇對等聊天所用資料夾的 UI，因此請要求主聊天建立對等聊天，並描述它應該使用的儲存庫或 worktree。

### [封存工作階段中的聊天](https://code.visualstudio.com/updates/v1_140#_archive-chats-in-a-session)

當某個聊天完成工作時，讓工作階段的其餘部分繼續保持運作。由於工作階段可以包含多個聊天，選取 **Mark as Done** 只會封存已完成的那個對等聊天，而不是整個工作階段。若要還原它（標題與對話紀錄完整保留），請在工作階段清單中選取 **Done** 篩選條件。

### [自動清理 Dev Container](https://code.visualstudio.com/updates/v1_140#_automatic-dev-container-cleanup)

**設定**：`chat.agentHost.devContainer.enabled`、`chat.agentHost.devContainer.idleTimeout`（僅限 Agents 視窗）

當所有使用某個 Dev Container 的工作階段都閒置五分鐘後，VS Code 會停止該容器。當你繼續工作時，它會自動重新啟動。將最後一個工作階段標記為完成或將其刪除，就會移除該容器。

### [更快的 Dev Container 啟動速度](https://code.visualstudio.com/updates/v1_140#_faster-dev-container-startup)

**設定**：`chat.agentHost.devContainer.enabled`（僅限 Agents 視窗）

Dev Container 工作階段讓代理程式能在隔離環境中使用你專案的工具與相依套件。全新的容器可以重複使用已快取的 VS Code Server 與 CLI 下載內容，藉此縮短設定時間，並減少跨專案的重複下載。

### [為委派的工作選擇工作階段關係與工作區](https://code.visualstudio.com/updates/v1_140#_choose-the-session-relationship-and-workspace-for-delegated-work)

當你要求代理程式委派工作時，要做兩個獨立的決定。首先，決定這項工作是否屬於目前工作階段的計畫或交付成果。接著，只有在工作需要儲存庫檔案時才指定工作區，並且只有在檔案變更需要隔離時才要求 worktree。相關的工作仍然可以使用不同的工作區。

- **為相關工作建立對等聊天**：要求代理程式在目前的工作階段中建立對等聊天。若要重複使用目前的工作區與 checkout，請在提示中說明。由於對等聊天共用 checkout，這個選項適合用於研究、規劃，或不需要隔離檔案變更的相關工作。

  ```
  Create a peer chat in this session that reuses the current workspace and checkout to review the authentication flow. Do not modify files.
  ```

  （提示中譯：在這個工作階段中建立一個對等聊天，重複使用目前的工作區與 checkout 來審查驗證流程。不要修改檔案。）

- **為無關的工作建立獨立工作階段**：當另一項交付成果不需要儲存庫檔案時，要求代理程式建立一個沒有工作區的獨立工作階段。新的工作階段不會繼承來源資料夾。如果該工作確實需要儲存庫，請指名專案或資料夾，並在其變更需要隔離時明確要求 worktree。

  ```
  Create an independent session with no workspace to research licensing options for a separate project.
  ```

  （提示中譯：建立一個沒有工作區的獨立工作階段，為另一個專案研究授權選項。）

進一步了解[協調聊天與工作階段](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions#_orchestrate-sessions-from-agent-host-sessions)。

### [在遠端 agent host 上建立無工作區的聊天](https://code.visualstudio.com/updates/v1_140#_create-workspace-less-chats-on-remote-agent-hosts)

**設定**：`sessions.chat.unifiedWorkspacePicker.enabled`（僅限 Agents 視窗）

[Agent Host](https://code.visualstudio.com/docs/agents/run/remote-agent-sessions) 讓你在遠端主機上執行並追蹤代理程式工作階段。本版新增在遠端主機上執行無工作區聊天的支援。

從統一工作區選擇器中選取已連線的主機，不需選擇工作區。**Chat** 項目會標示所選的主機，讓你在開始前確認工作階段的執行位置。

![畫面截圖：遠端 agent host 上的無工作區聊天，Chat 項目中標示了所選的主機。](https://code.visualstudio.com/assets/updates/1_140/workspace-less-chat-remote-host.webp)

遠端資料夾現在也會與本機資料夾一起顯示在主選擇器中。這讓常用的遠端工作區更容易選取，不必再開啟 **Remote** 子選單。

### [將工作委派給遠端 agent host（實驗性）](https://code.visualstudio.com/updates/v1_140#_delegate-tasks-to-remote-agent-hosts-experimental)

**設定**：`chat.remoteAgentHostsEnabled`、`chat.remoteSessions.tools.enabled`（僅限 Agents 視窗）

你的代理程式可以從 Agents 視窗將工作委派給已連線的[遠端 agent host](https://code.visualstudio.com/docs/agents/run/remote-agent-sessions)，不需要你為每項工作在選擇器中選取主機。

新的內建工具讓你的代理程式可以：

- 使用 `list_agent_hosts` 探索主機、模型、資源容量與工作階段負載。
- 使用 `create_remote_session` 啟動工作階段。可以指定主機，或讓自動配置依作業系統（Windows、Linux 或 macOS）、最低記憶體容量、邏輯 CPU 數量，以及選用的模型進行比對。在符合條件的主機中，配置會優先選擇執行中工作階段與待建立工作階段最少的主機。
- 使用 `get_remote_session` 檢查遠端工作階段的狀態與最新回應。
- 使用 `send_remote_message` 傳送後續工作，或將結果與問題回報給發起的聊天。

這些工具預設為關閉。請同時啟用此設定與 `chat.remoteAgentHostsEnabled`，並連線到你想使用的主機。在 agent host 上執行的聊天中，提出以下要求：

```
Start a remote session on a connected Linux host with at least 16 GiB of memory and eight logical CPUs. Check the installed Node.js and Python versions and report back to this chat.
```

（提示中譯：在一台已連線、至少有 16 GiB 記憶體與八個邏輯 CPU 的 Linux 主機上啟動遠端工作階段。檢查已安裝的 Node.js 與 Python 版本，並回報給這個聊天。）

除非你指定，否則工作階段沒有工作區。若要處理儲存庫工作，請使用目標主機上既有的受信任資料夾，可以直接使用，也可以在新的 Git worktree 中使用。這些工具不會 clone 或複製發起端的工作區，一般的核准流程也仍然適用。

請讓負責協調的 Agents 視窗保持開啟並維持連線，訊息才能傳遞。遠端代理程式會透過 `send_remote_message` 回報；它們的最終回答不會自動轉送。

### [個人化新工作階段歡迎語（實驗性）](https://code.visualstudio.com/updates/v1_140#_personalize-new-session-welcome-phrases-experimental)

**設定**：`sessions.chat.experimental.welcomePhrases`、`sessions.chat.experimental.welcomeName`、`accessibility.verbosity.newSessionWelcome`（僅限 Agents 視窗）

在 Agents 視窗中建立新工作階段時，可以顯示選用的歡迎標題，為 Agents 視窗增添一些個性。標題會顯示五種語句之一，例如 **What are we building?** 或 **Let's ship something**，並可附加你 GitHub 個人檔案中的名字。

![畫面截圖：Agents 視窗的新工作階段撰寫區，顯示個人化歡迎語 Let's cook, Megan。](https://code.visualstudio.com/assets/updates/1_140/custom-welcome-phrases.webp)

歡迎語預設為關閉。啟用後，你可以從標題、其內容功能表或命令選擇區使用 **Set Welcome Name** 選擇其他名字。這個名字會在各裝置之間同步。清除名字後，若有可用的 GitHub 名字就會使用它；若你的個人檔案中沒有名字，則會顯示通用語句。

當螢幕助讀程式最佳化模式啟用時，VS Code 會在撰寫區出現時朗讀一次標題。如果你不想聽到這段朗讀，請停用 `accessibility.verbosity.newSessionWelcome`。

### [改良的工作階段撰寫區控制項（實驗性）](https://code.visualstudio.com/updates/v1_140#_improved-session-composer-controls-experimental)

**設定**：`sessions.chat.experimental.newSessionComposerLayout`、`sessions.chat.unifiedWorkspacePicker.enabled`（僅限 Agents 視窗）

實驗性的工作階段撰寫區版面，減少在 Agents 視窗各個區域之間來回移動的需要。在新的工作階段中，工作區、分支、worktree 與 harness 控制項會集中在聊天輸入框上方。

![畫面截圖：實驗性的工作階段撰寫區版面，工作區、分支、worktree 與 harness 控制項集中在聊天輸入框上方。](https://code.visualstudio.com/assets/updates/1_140/composer-layout.webp)

同時啟用兩項設定即可使用此版面。

統一工作區選擇器也提供可直接移至其控制項的命令。使用 Ctrl+K Ctrl+F 聚焦工作區選擇器，或使用 Ctrl+K Ctrl+H 聚焦 harness 選擇器。停留提示會顯示你設定的按鍵繫結，每個選擇器的內容功能表也都提供 **Configure Keybinding** 動作。這些鍵盤操作改進只需要統一工作區選擇器，不需要實驗性的撰寫區版面。

從這個里程碑開始，當你建立新的 worktree 工作階段時，分支選擇器會預設為目前分支的上游分支，例如 `origin/main`。在建立 worktree 之前，VS Code 會自動嘗試擷取（fetch）所選的遠端分支，讓工作階段從可取得的最新遠端狀態開始。

選擇器也會針對每個工作區只載入一次分支，並在本機進行篩選，大幅降低搜尋延遲，在大型儲存庫中尤其明顯。

![畫面截圖：新工作階段頁面，已選取 New Worktree，預設分支為 origin/main。](https://code.visualstudio.com/assets/updates/1_140/new-session-branch-picker.webp)

### [優先查看最近完成的工作階段](https://code.visualstudio.com/updates/v1_140#_see-recently-completed-sessions-first)

Agents 視窗中的 **Done** 區段，現在會依你將工作階段標記為完成的時間排序，最近完成的工作階段列在最前面。在 **Created** 與 **Updated** 排序之間切換，以及重新載入 VS Code 之後，這個順序都會維持不變。

### [更可靠的代理程式協調（實驗性）](https://code.visualstudio.com/updates/v1_140#_more-reliable-agent-orchestration-experimental)

**設定**：`chat.agentHost.agentOrchestrationLimits`

需要大量協調的代理程式工作流程，比較不會在工作完成前就停止。本版提高了整個處理程序範圍內，對代理程式建立的工作階段、聊天、工作階段間訊息，以及遞迴建立工作階段的上限。

例如，代理程式可以將 CI 失敗分類、把每項修正委派給不同的工作階段，並協調這些工作階段，而不會在工作完成前就用盡訊息上限。達到上限時會阻擋新的協調動作，但不會中斷已經在執行中的工作。

此設定預設為 `on`，變更會在不重新啟動 Agent Host 的情況下套用。將它設為 `off` 可移除協調上限。確認要求與輸入驗證仍然適用。

### [透過 URL 在 Agents 視窗中開啟新工作階段](https://code.visualstudio.com/updates/v1_140#_open-a-new-session-in-the-agents-window-from-a-url)

從其他工具或網頁開啟 Agents 視窗，並備妥工作區與可編輯的提示供你檢閱。請使用帶有 `agents/new` 路徑，以及選用的 `prompt` 與 `workspace` 查詢參數的 URL。

例如，以下 URL 會開啟一個沒有工作區、且已輸入提示的新工作階段：

```
vscode://agents/new?prompt=Explain%20how%20this%20project%20runs%20its%20tests.%20Do%20not%20run%20anything%20yet.
```

（URL 中的提示中譯：說明這個專案如何執行它的測試。先不要執行任何東西。）

VS Code 會將解碼後的提示放入 **New Session** 撰寫區，但不會送出。若要選取工作區，請加上 `workspace` 參數，並填入經過 URL 編碼的資料夾 URI。如果 URL 省略了 `workspace`，草稿會使用 **No Workspace**。已經有內容的撰寫區會維持不變，因此開啟連結不會覆寫進行中的工作。

進一步了解[使用 URL 開啟 VS Code](https://code.visualstudio.com/docs/configure/command-line#_opening-vs-code-with-urls)。

> **注意：** VS Code Insiders 請使用 `vscode-insiders://` 取代 `vscode://`。

---

## [聊天（Chat）](https://code.visualstudio.com/updates/v1_140#_chat)

### [代理程式回應的持續進度顯示（實驗性）](https://code.visualstudio.com/updates/v1_140#_persistent-progress-for-agent-responses-experimental)

**設定**：`chat.experimental.persistentProgress`、`chat.experimental.persistentProgressVerbosity`

有了持續進度顯示，長時間執行的代理程式回應更容易追蹤。啟用後，聊天會在底部持續顯示進度指示器，直到回應結束為止。

啟用持續進度也會改善進行中回應的呈現方式與可見度。推理內容會出現在獨立的可摺疊預覽中，工具呼叫在執行期間也會保持可見。在 **Compact** 詳細程度下，當回應文字繼續輸出或回應結束時，已完成的工具群組會摺疊成可展開的摘要。選取 **Verbose** 則會保留完整的工具呼叫細節。

使用 `chat.experimental.persistentProgress` 可以關閉指示器，或選擇彩色或單色的 Draw 動畫。此設定在 VS Code Insiders 中預設為 **Draw**，並正逐步推出到 Stable 版。

### [終端機輸出重排](https://code.visualstudio.com/updates/v1_140#_terminal-output-reflow)

**設定**：`chat.tools.terminal.outputReflow`

自 [VS Code 1.132](https://code.visualstudio.com/updates/v1_132#_terminal-output-reflow) 起，聊天中的終端機輸出會重排，以符合輸出預覽的寬度。

新的 `chat.tools.terminal.outputReflow` 設定讓你控制這個行為。如果你偏好讓輸出保持固定寬度，請停用此設定，並以水平捲動的方式閱讀長行。重排預設仍為啟用。

### [在編輯器視窗中使用多個聊天](https://code.visualstudio.com/updates/v1_140#_use-multiple-chats-in-the-editor-window)

一個工作階段可以包含多個聊天。在此之前，主聊天及其巢狀的對等聊天只會出現在 Agents 視窗中。現在編輯器視窗也會顯示它們：**Sessions** 檢視會列出主聊天，其對等聊天則巢狀列在下方，位置就在 **Chat** 檢視旁邊。選取任一列，即可在 **Chat** 檢視中開啟該聊天。

![畫面截圖：編輯器視窗中，Sessions 檢視列出一個工作階段及其巢狀對等聊天，旁邊的 Chat 檢視顯示所選的聊天。](https://code.visualstudio.com/assets/updates/1_140/editor-window-multi-chats.webp)

---

## [MCP](https://code.visualstudio.com/updates/v1_140#_mcp)

### [將 MCP 伺服器加入可攜式設定檔](https://code.visualstudio.com/updates/v1_140#_add-mcp-servers-to-portable-configuration-files)

你可以從 **MCP: Add Server** 流程將 MCP 伺服器加入可攜式設定檔，這樣就能在相容的 Copilot 工具之間使用相同的伺服器設定，不必手動編輯 JSON。

對於全域伺服器，選擇 **Copilot Global** 可將伺服器儲存到 `$COPILOT_HOME/mcp-config.json`；若未設定 `COPILOT_HOME`，則儲存到 `~/.copilot/mcp-config.json`。

![畫面截圖：Copilot Global 與已淘汰的使用者設定，作為全域 MCP 伺服器的儲存位置。](https://code.visualstudio.com/assets/updates/1_140/mcp-global-configuration.webp)

對於工作區伺服器，選擇 `.mcp.json` 可將伺服器儲存在工作區根目錄。

![畫面截圖：工作區根目錄的 .mcp.json 與已淘汰的 .vscode/mcp.json，作為工作區 MCP 伺服器的儲存位置。](https://code.visualstudio.com/assets/updates/1_140/mcp-workspace-configuration.webp)

---

## [協助工具（Accessibility）](https://code.visualstudio.com/updates/v1_140#_accessibility)

### [聽見彩帶出現](https://code.visualstudio.com/updates/v1_140#_hear-when-confetti-appears)

**設定**：`accessibility.signals.confetti`

新的 **Confetti** 協助工具訊號，讓你在看不到動畫時，也能獲得同樣的慶祝回饋。當聊天中按讚觸發的彩帶出現，以及你在 Agents 視窗中成功將工作階段標記為完成時，都會播放這段歡樂的音效。

---

## [原始檔控制（Source Control）](https://code.visualstudio.com/updates/v1_140#_source-control)

### [在 worktree 之間重複使用被忽略的資料夾（實驗性）](https://code.visualstudio.com/updates/v1_140#_reuse-ignored-folders-across-worktrees-experimental)

**設定**：`git.worktreeSymlinkFolders`

避免在每個 [Git worktree](https://code.visualstudio.com/docs/sourcecontrol/branches-worktrees#_understanding-worktrees) 中重新安裝相依套件，或複製大型建置產物。請以 `.gitignore` 風格的模式，為被忽略的資料夾（例如 `node_modules`）設定 `git.worktreeSymlinkFolders`。當 VS Code 建立新的 worktree 時（包括為代理程式工作階段建立時），會為目前 checkout 中符合模式的資料夾建立符號連結。

---

## [程式碼編輯（Code Editing）](https://code.visualstudio.com/updates/v1_140#_code-editing)

### [控制選取文字比對大小寫敏感度的新選項](https://code.visualstudio.com/updates/v1_140#_new-option-to-control-selection-text-matching-case-sensitivity)

**設定**：`editor.selectedTextMatchMode`

我們新增了 `editor.selectedTextMatchMode` 設定，用來控制選取範圍比對的大小寫敏感度。

預設值 `findOptions` 表示選取比對的敏感度與尋找小工具連動，與先前的行為相同。另外兩個值 `caseSensitive` 與 `caseInsensitive`，分別以區分大小寫與不區分大小寫的方式比對選取範圍。

---

## [終端機（Terminal）](https://code.visualstudio.com/updates/v1_140#_terminal)

### [macOS 上更清晰的終端機文字（實驗性）](https://code.visualstudio.com/updates/v1_140#_sharper-terminal-text-on-macos-experimental)

**設定**：`terminal.integrated.fontRendering`

在 macOS 高 DPI 顯示器上，終端機文字看起來可能比其他終端機中的相同字型更柔和（較不銳利）。將 `terminal.integrated.fontRendering` 設為 `grayscale`，可讓終端機文字看起來更清晰。

---

## [企業（Enterprise）](https://code.visualstudio.com/updates/v1_140#_enterprise)

### [說明最低版本需求](https://code.visualstudio.com/updates/v1_140#_explain-minimum-version-requirements)

管理員可以要求開發人員在使用 AI 功能前先更新 VS Code，例如為了採用較新的沙箱防護。VS Code 現在不再顯示 **Update Required** 對話方塊，而是在 AI 功能無法使用的地方說明這項需求：

- **Chat**：顯示所需版本與已安裝版本，並提供更新動作。
- **編輯器視窗**：即使 Chat 已關閉，也會顯示橫幅。其他編輯器功能仍可使用。
- **Agents 視窗**：顯示阻擋式覆蓋層，並提供 **Open Editor Window** 動作。

當有可用的更新時，VS Code 會提供對應的 **Check for Updates**、**Download Update**、**Install Update** 或 **Restart to Update** 動作。如果你的組織透過原則停用了內建更新，VS Code 則會請你聯絡管理員。符合需求後，這些通知就會清除。VS Code 強制執行此需求的方式沒有改變。

![畫面截圖：編輯器視窗中的橫幅與 Chat 通知，說明組織要求使用較新版本才能使用 AI 功能。](https://code.visualstudio.com/assets/updates/1_140/minversion-chat-banner.webp)

### [設定預設的 Auto 層級](https://code.visualstudio.com/updates/v1_140#_set-a-default-auto-tier)

VS Code 中的 Auto 模型可以在不同層級下運作，分別針對效率、平衡或智慧進行最佳化。管理員可以讓 Auto 模型的預設行為與組織的優先事項一致，同時不妨礙開發人員選擇其他層級。

管理員可以透過 `autoTier` 受管理設定，設定 Auto 模型的預設層級。可接受的值為 `efficiency`、`balance` 與 `intelligence`。

此層級會套用到同一台電腦上 Local harness 與 Copilot agent host 中的新聊天。受管理的層級會在模型選擇器中顯示為 **Default**。

受管理的層級是起點，而不是限制。開發人員仍然可以選擇不同的層級。當受管理的層級變更或移除時，VS Code 會保留明確做出的選擇與已還原的選擇。

![畫面截圖：Auto 模型的 Optimize for 功能表，已選取 Intelligence 並標示為 Default。](https://code.visualstudio.com/assets/updates/1_140/managed-auto-tier-default.webp)

### [在 OpenTelemetry 中擷取使用者身分](https://code.visualstudio.com/updates/v1_140#_capture-user-identity-in-opentelemetry)

**設定**：`github.copilot.chat.otel.captureIdentity`

組織可以將 [Copilot OpenTelemetry](https://code.visualstudio.com/docs/enterprise/ai-settings#_configure-telemetry-export-with-opentelemetry) 資料歸屬到個別開發人員。啟用身分擷取後，Local 聊天工作階段會加入以下屬性：

- 代理程式呼叫 span 上的 `user.name`，包括子代理程式（subagent）與內嵌聊天。
- 作為資源屬性的 `process.user.name` 與 `host.name`。

身分擷取預設為關閉，且與內容擷取彼此獨立。管理員透過 `telemetry.capture.identity` 受管理設定（`CopilotOtelCaptureIdentity` 原則）控制身分擷取。受管理的值優先於 `COPILOT_OTEL_CAPTURE_IDENTITY` 與使用者設定。當原則拒絕擷取時，VS Code 會在不需重新載入的情況下，從後續匯出的資料中移除身分資訊，包括明確設定為資源屬性的身分屬性。

本版也變更了受管理遙測設定的解析方式：

- VS Code 只會套用優先順序最高的傳遞管道中的 `telemetry` 區塊，順序依序為原生 MDM、伺服器、檔案。所選區塊中省略的欄位，不再由優先順序較低的管道補上。
- 受管理的 `telemetry.resourceAttributes` 現在優先於 `OTEL_RESOURCE_ATTRIBUTES`。

> **注意**：身分擷取目前僅適用於 Local harness。Agent host 的支援在 [#337413](https://github.com/microsoft/vscode/issues/337413) 中追蹤。

---

## [社群媒體](https://code.visualstudio.com/updates/v1_140#_social-media)

VS Code 現在也在 Instagram 上了！追蹤 [@vscode.ig](https://www.instagram.com/vscode.ig)，取得 VS Code 的更新、新功能、使用技巧等資訊。

---

## [提議的 API（Proposed APIs）](https://code.visualstudio.com/updates/v1_140#_proposed-apis)

### [驗證工作階段上的授權伺服器](https://code.visualstudio.com/updates/v1_140#_authorization-server-on-authentication-sessions)

`authIssuers` 提議讓擴充功能可以指名它要進行驗證的 OAuth 授權伺服器，這是 VS Code 在 [1.101 版本說明](https://code.visualstudio.com/updates/v1_101#_authentication-providers-supported-authorization-servers-for-mcp)中為 MCP 引入的功能。本版新增了反方向的資訊，讓工作階段可以告訴你是哪一個授權伺服器簽發了它。

```typescript
export interface AuthenticationSession {
  /**
   * The authorization server that issued this session, when provided by the authentication provider.
   * This identifies the OAuth server, not a REST API endpoint or resource audience.
   */
  readonly authorizationServer?: Uri;
}
```

（註解中譯：當驗證提供者有提供時，此為簽發這個工作階段的授權伺服器。它識別的是 OAuth 伺服器，而不是 REST API 端點或資源對象（audience）。）

如果你的擴充功能會與某個服務的多個部署溝通，例如公開的 GitHub 與 GitHub Enterprise 主機，你可以將每組認證與其簽發時的目的地配對，而不是讀取一個使用者可以獨立變更的額外設定。這可以避免有效的權杖被送到錯誤的主機。

請注意兩件事。第一，這個值識別的是 OAuth 簽發者，而不是 API 端點，因此請將它對應到你自己的端點。第二，內建的 GitHub 提供者對公開 GitHub 工作階段與企業工作階段都會填入此值，所以光是有這個值存在，並不代表該工作階段就是企業工作階段。

歡迎試用，並在 [API 提議 issue](https://github.com/microsoft/vscode/issues/248775) 中告訴我們你的想法。若要了解如何針對提議的 API 進行開發，請參閱[使用提議的 API](https://code.visualstudio.com/api/advanced-topics/using-proposed-api)。

---

## [已淘汰的功能與設定](https://code.visualstudio.com/updates/v1_140#_deprecated-features-and-settings)

無

---

## [感謝](https://code.visualstudio.com/updates/v1_140#_thank-you)

對 `vscode` 的貢獻：

- [@na2co3-ftw (na2co3)](https://github.com/na2co3-ftw)：Modern UI：修正已連接編輯器索引標籤中多餘的索引標籤動作淡出效果 [PR #336871](https://github.com/microsoft/vscode/pull/336871)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：終端機重播中的記憶體流失 [PR #338252](https://github.com/microsoft/vscode/pull/338252)
  - 修正：語意權杖中的記憶體流失 [PR #336024](https://github.com/microsoft/vscode/pull/336024)
  - 修正：搜尋編輯器中的記憶體流失 [PR #331014](https://github.com/microsoft/vscode/pull/331014)
  - 終端機：在處置（disposal）時取消不必要的遙測逾時 [PR #333963](https://github.com/microsoft/vscode/pull/333963)
  - 修正：動畫影格視窗處置中的記憶體流失 [PR #338263](https://github.com/microsoft/vscode/pull/338263)
  - 修正：聊天參與者處置中的記憶體流失 [PR #338230](https://github.com/microsoft/vscode/pull/338230)
  - 修正：自訂文件處置中的記憶體流失 [PR #338250](https://github.com/microsoft/vscode/pull/338250)
  - 修正：對應編輯提供者處置中的記憶體流失 [PR #338245](https://github.com/microsoft/vscode/pull/338245)
  - 修正：擴充主機遙測中的記憶體流失 [PR #334096](https://github.com/microsoft/vscode/pull/334096)
  - 修正：輔助視窗字型量測中的記憶體流失 [PR #338255](https://github.com/microsoft/vscode/pull/338255)
  - 修正：SCM 成品命令中的記憶體流失 [PR #338251](https://github.com/microsoft/vscode/pull/338251)
  - 修正：像素比例視窗處置中的記憶體流失 [PR #338257](https://github.com/microsoft/vscode/pull/338257)
  - 修正：整合式瀏覽器元素控制代碼中的記憶體流失 [PR #338247](https://github.com/microsoft/vscode/pull/338247)
  - 修正：終端機水平捲軸中的記憶體流失 [PR #338241](https://github.com/microsoft/vscode/pull/338241)
  - 修正：擴充主機樹狀檢視處置中的記憶體流失 [PR #338235](https://github.com/microsoft/vscode/pull/338235)
  - 修正：複合拖放註冊中的記憶體流失 [PR #338215](https://github.com/microsoft/vscode/pull/338215)
  - 修正：整合式瀏覽器檢查器工作階段中的記憶體流失 [PR #338228](https://github.com/microsoft/vscode/pull/338228)
  - 修正：chatInputPart 中的記憶體流失 [PR #327157](https://github.com/microsoft/vscode/pull/327157)
  - 修正：終端機 shell 執行串流中的記憶體流失 [PR #338243](https://github.com/microsoft/vscode/pull/338243)
  - 修正：筆記本儲存格狀態列命令中的記憶體流失 [PR #336028](https://github.com/microsoft/vscode/pull/336028)
  - 修正：issueReporterModel 中的記憶體流失 [PR #335098](https://github.com/microsoft/vscode/pull/335098)
  - 修正：圖片輪播編輯器中的記憶體流失 [PR #333981](https://github.com/microsoft/vscode/pull/333981)
  - 修正：筆記本序列化程式處置中的記憶體流失 [PR #338212](https://github.com/microsoft/vscode/pull/338212)
  - 修正：MainThreadShare 中的記憶體流失 [PR #334110](https://github.com/microsoft/vscode/pull/334110)
  - 修正：多重差異編輯器索引標籤中的記憶體流失 [PR #333186](https://github.com/microsoft/vscode/pull/333186)
  - 修正：測試觀察器處置中的記憶體流失 [PR #338213](https://github.com/microsoft/vscode/pull/338213)
  - 修正：modalEditorPart 中的記憶體流失 [PR #326885](https://github.com/microsoft/vscode/pull/326885)
  - 修正：註解討論串處置中的記憶體流失 [PR #338216](https://github.com/microsoft/vscode/pull/338216)
  - 修正：行資料事件附加元件中的記憶體流失 [PR #332139](https://github.com/microsoft/vscode/pull/332139)

### [問題追蹤](https://code.visualstudio.com/updates/v1_140#_issue-tracking)

對問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們非常感謝大家在新功能一推出就搶先試用，請經常回來看看有哪些新內容。

> 如果你想閱讀先前 VS Code 版本的版本說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。
