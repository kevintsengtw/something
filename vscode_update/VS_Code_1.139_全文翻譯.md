# Visual Studio Code 1.139 全文翻譯

> 來源：[Visual Studio Code 1.139](https://code.visualstudio.com/updates/v1_139)

發布日期：2026 年 9 月 23 日｜Stable

## 1.139.0 下載

- Windows：[x64](https://update.code.visualstudio.com/1.139.0/win32-x64-user/stable)｜[Arm64](https://update.code.visualstudio.com/1.139.0/win32-arm64-user/stable)
- macOS：[Universal](https://update.code.visualstudio.com/1.139.0/darwin-universal-dmg/stable)｜[Intel](https://update.code.visualstudio.com/1.139.0/darwin-x64-dmg/stable)｜[Apple silicon](https://update.code.visualstudio.com/1.139.0/darwin-arm64-dmg/stable)
- Linux：[.deb](https://update.code.visualstudio.com/1.139.0/linux-deb-x64/stable)｜[.rpm](https://update.code.visualstudio.com/1.139.0/linux-rpm-x64/stable)｜[.tar.gz](https://update.code.visualstudio.com/1.139.0/linux-x64/stable)｜[Arm 說明](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions)｜[Snap](https://update.code.visualstudio.com/1.139.0/linux-snap-x64/stable)

已經安裝了嗎？請在 VS Code 中使用 **Check for Updates**（檢查更新）。若想搶先體驗即將推出的功能，請使用 [Insiders 組建](https://code.visualstudio.com/insiders)。

---

## [版本亮點](https://code.visualstudio.com/updates/v1_139#_release-highlights)

此版本讓大型 agent 工作階段（session）清單的速度更快、將 Dev Container 支援擴展至遠端專案，並改善日常編輯體驗。

- [遠端 Dev Container 工作階段](https://code.visualstudio.com/updates/v1_139#_run-agent-sessions-in-dev-containers-on-remote-hosts)：在 SSH、Tunnel 與 WSL 主機上，於專案的 Dev Container 內執行 agent。
- [工作階段清單改善](https://code.visualstudio.com/updates/v1_139#_faster-session-list-loading)：更快載入大型工作階段清單、在畫面上容納更多工作階段，以及就地重新命名工作階段。
- [編輯器體驗](https://code.visualstudio.com/updates/v1_139#_editor-experience)：一眼辨識自動換行的行，並在輸入時避免產生重複的右括號。

---

## [Agents](https://code.visualstudio.com/updates/v1_139#_agents)

[Agent host](https://code.visualstudio.com/docs/agents/concepts/agent-host) 會在以 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）為基礎的專用處理程序中執行 agent harness，因此你可以從多個 VS Code 視窗連線到同一個工作階段。可在 [agent host 部落格文章](https://code.visualstudio.com/blogs/2026/08/26/agent-host-architecture)中深入了解其架構與工作流程。

### [在遠端主機上的 Dev Container 中執行 agent 工作階段](https://code.visualstudio.com/updates/v1_139#_run-agent-sessions-in-dev-containers-on-remote-hosts)

**設定**：`chat.agentHost.devContainer.enabled`（在 VS Code 中開啟／在 VS Code Insiders 中開啟）（僅限 Agents 視窗）

讓 agent 使用正確的工具與相依套件來建置與測試你的遠端專案，而不必在你的筆電或遠端主機上重複設定工具鏈。此版本將 Dev Container 工作階段從本機資料夾擴展到 SSH、Tunnel 與 WSL 主機上的專案。

要開始使用，請啟用 `chat.agentHost.devContainer.enabled`（在 VS Code 中開啟／在 VS Code Insiders 中開啟），並在 Agents 視窗的資料夾選單中選取 **Use Dev Container**。遠端資料夾必須具備受支援的 Dev Container 設定，且遠端主機上必須有可用的 Docker。

> **注意**：Dev Container 工作階段正在逐步推出，因此此設定可能尚未預設為你啟用。你可以手動啟用此設定，立即試用這項功能。

### [更快的工作階段清單載入](https://code.visualstudio.com/updates/v1_139#_faster-session-list-loading)

VS Code 載入與重新整理大型 agent 工作階段清單的速度更快了。Agent host 會將輕量的工作階段與聊天中繼資料保存在一個集中式目錄（catalog）中，而不是在每次建立清單時都開啟每一個對話資料庫。完整的對話內容仍然隔離存放在各個工作階段與聊天資料庫中。

這項改善會隨著工作階段數量增加而更加明顯，因為先前的做法所需的工作量與你的工作階段數量成正比。以下是在一台開發機上、約 645 個工作階段的測量結果：

| 操作 | 之前 | 之後 | 改善幅度 |
| --- | --- | --- | --- |
| 啟動後第一次列出工作階段 | 1.3 秒 | 0.1 秒 | 約快 12 倍 |
| 重新整理工作階段清單 | 0.6 秒 | 0.15 秒 | 約快 4 倍 |

如果你的工作階段不多，差異會比較小。在此版本之前建立的工作階段，會在背景自動移轉。

### [精簡工作階段清單](https://code.visualstudio.com/updates/v1_139#_compact-sessions-list)

在 Agents 視窗的工作階段清單檢視中啟用 **Compact View**，即可在工作階段清單中容納更多工作階段。

精簡列在靜止狀態下只顯示工作階段標題，當你將滑鼠停留在該列上或將焦點移到該列時，才會顯示工作區的詳細資訊。當工作階段需要輸入或核准時，該列會展開，讓這些請求保持可見。

進度也會顯示在負責該項工作的聊天所在的列上。當你摺疊某個工作階段時，父列會彙總其隱藏聊天的進度。

### [篩選空的工作階段群組](https://code.visualstudio.com/updates/v1_139#_filter-empty-session-groups)

從 **Filter Sessions** 中停用 **Empty Groups**，即可隱藏空的自訂群組以及空的 Chats 區段。此偏好設定會儲存在你的設定檔（profile）中，並會隨其他工作階段清單篩選條件一起重設。

### [就地重新命名工作階段與聊天](https://code.visualstudio.com/updates/v1_139#_rename-sessions-and-chats-in-place)

直接在工作階段清單中重新命名工作階段或巢狀聊天。按兩下其標題、使用 **Rename** 內容功能表動作，或將焦點移到該列後，針對工作階段按 F2、針對巢狀聊天按 F2。內嵌驗證可防止空白標題，取消則會還原為先前的標題。

### [選擇聊天在工作階段中的呈現方式（Preview）](https://code.visualstudio.com/updates/v1_139#_choose-how-chats-appear-in-a-session-preview)

**設定**：`sessions.showChatTabs`（在 VS Code 中開啟／在 VS Code Insiders 中開啟）（僅限 Agents 視窗）

一個 agent 工作階段可以包含[多個聊天](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions#_run-multiple-chats-in-a-session)，每個聊天代表不同的對話或情境（context）。當工作階段包含多個聊天時，可從工作階段標頭選單中選擇最符合你工作流程的呈現方式：

- **Multiple**：將每個聊天顯示在各自的索引標籤上。
- **Single**：只顯示作用中的聊天，並隱藏索引標籤列。

切換呈現方式會保留你已開啟的聊天、作用中的聊天與對話狀態。在 Single 模式下，你明確開啟到側邊的聊天，仍會維持為具有各自標頭動作的獨立窗格。

---

## [Chat](https://code.visualstudio.com/updates/v1_139#_chat)

### [寵物命名比賽更新（Experimental）](https://code.visualstudio.com/updates/v1_139#_pet-naming-contest-update-experimental)

感謝每一位為 VS Code 寵物提交名字的人。命名比賽已於 2026 年 9 月 17 日截止，我們正在審閱符合資格的參賽名稱。我們很快就會公布得獎者以及寵物的新名字。

在等待的同時，可以在聊天中輸入 `/vscode-pet` 來認識你的夥伴，並[探索它所有的互動與反應](https://code.visualstudio.com/docs/agents/reference/chat-pet)。

---

## [編輯器體驗](https://code.visualstudio.com/updates/v1_139#_editor-experience)

### [自動換行指示器](https://code.visualstudio.com/updates/v1_139#_word-wrap-indicators)

顯示自動換行指示器，讓自動換行的行更容易辨識。編輯器右側自動換行欄位處的箭頭，表示該行有自動換行。

![編輯器中顯示自動換行指示器的螢幕擷取畫面。](https://code.visualstudio.com/assets/updates/1_139/word_wrap_indicators.webp)

### [改善括號自動關閉行為](https://code.visualstudio.com/updates/v1_139#_improved-bracket-auto-closing-behavior)

當你輸入左括號時，VS Code 會避免插入重複的右括號。如果已存在相符的右括號，VS Code 會使用它；否則，VS Code 會插入一個右括號。

---

## [提議的 API（Proposed APIs）](https://code.visualstudio.com/updates/v1_139#_proposed-apis)

### [驗證工作階段上的存取權杖存留期](https://code.visualstudio.com/updates/v1_139#_access-token-lifetime-on-authentication-sessions)

`AuthenticationSession` 會公開存取權杖（access token），但不提供該權杖有效期限多長的資訊。若擴充功能將認證傳遞給具有自己重新整理回呼（refresh callback）的 SDK，就無法區分永不過期的權杖與即將過期的權杖。因此，擴充功能不是不必要地重新整理認證，就是在權杖過期時讓長時間執行的作業失敗。

`authSessionExpiration` 提議為 `AuthenticationSession` 新增選用的 `expiresAfter` 屬性：

```typescript
export interface AuthenticationSession {
  /**
   * The access token's remaining lifetime, in milliseconds, when the authentication
   * provider returns the session.
   */
  readonly expiresAfter?: number;
}
```

（程式碼註解中譯：驗證提供者傳回工作階段時，存取權杖的剩餘存留期，以毫秒為單位。）

此值是傳回工作階段當下的剩餘存留期，而不是絕對的過期時間戳記。擴充功能主機（extension host）可能與用戶端在不同的機器上執行，而兩者的時鐘可能不一致。傳回快取工作階段的驗證提供者，每次都會重新計算此值；當權杖的過期時間未知時，則將其保留為 `undefined`。內建的 Microsoft 帳戶提供者會提供此值。

歡迎試用，並在 [API 提議 issue](https://github.com/microsoft/vscode/issues/335184) 中告訴我們你的想法。若要了解如何針對提議的 API 進行開發，請參閱[使用提議的 API](https://code.visualstudio.com/api/advanced-topics/using-proposed-api)。

---

## [已淘汰的功能與設定](https://code.visualstudio.com/updates/v1_139#_deprecated-features-and-settings)

無

---

## [值得注意的修正](https://code.visualstudio.com/updates/v1_139#_notable-fixes)

- 對於所屬組織透過帳戶原則停用 Agent 模式的使用者，確保 Welcome 中的邀請開啟入口會被隱藏，並確保啟動已停用之 Agents 視窗的其他方式（例如 `code --agents`）無法規避此控制。_[#336968：修正 Agents 視窗中的帳戶原則強制執行](https://github.com/microsoft/vscode/pull/336968)_
- 對於使用企業管理之 [OpenTelemetry（OTel）設定](https://code.visualstudio.com/docs/enterprise/ai-settings#configure-telemetry-export-with-opentelemetry)的使用者，修正 Local（即非 Agent Host Harness）中 OTel 設定的競爭條件（race condition），以確保 OTel 不會被丟棄。_[#336701：修正 Copilot 擴充功能中企業管理 OTel 的競爭條件](https://github.com/microsoft/vscode/pull/336701)_

---

## [感謝](https://code.visualstudio.com/updates/v1_139#_thank-you)

對 `vscode` 的貢獻：

- [@AnupamKumar-1 (Anupam Kumar)](https://github.com/AnupamKumar-1)：fix(chat)：在編輯 #file 參考前方的文字時保留該參考 [PR #333965](https://github.com/microsoft/vscode/pull/333965)
- [@baywet (Vincent Biret)](https://github.com/baywet)：feat：為 JSON 擴充功能新增預設的 OpenAPI 檔案比對 [PR #336273](https://github.com/microsoft/vscode/pull/336273)
- [@brandonh-msft (Brandon H)](https://github.com/brandonh-msft)：修正遠端聊天外掛程式路徑 [PR #326916](https://github.com/microsoft/vscode/pull/326916)
- [@brignano (anthony)](https://github.com/brignano)：github-authentication：針對 EMU 帳戶略過 education.github.com 檢查 [PR #336608](https://github.com/microsoft/vscode/pull/336608)
- [@Chirag-Bhardwaj (Chirag Bhardwaj)](https://github.com/Chirag-Bhardwaj)：修正清除自訂終端機標題的問題 [PR #336599](https://github.com/microsoft/vscode/pull/336599)
- [@dobbydobap (varshitha)](https://github.com/dobbydobap)：在 Configure Snippets 中優先顯示作用中編輯器的語言 [PR #324369](https://github.com/microsoft/vscode/pull/324369)
- [@emxs1 (Emma)](https://github.com/emxs1)：新增更多有趣的工作中訊息 [PR #335600](https://github.com/microsoft/vscode/pull/335600)
- [@jlelong (Jerome Lelong)](https://github.com/jlelong)：LaTeX：更新語言設定 [PR #332303](https://github.com/microsoft/vscode/pull/332303)
- [@joltcoke (Florian Schirmer)](https://github.com/joltcoke)：在 WebKit 剪貼簿因應措施中保護 navigator.clipboard [PR #334878](https://github.com/microsoft/vscode/pull/334878)
- [@joshspicer](https://github.com/joshspicer)：修正 mock-policy-server 檔案命令中的權限位元 [PR #334382](https://github.com/microsoft/vscode/pull/334382)
- [@Muszic (Sangeet)](https://github.com/Muszic)：為終端機建議新增上游 Cargo 補全規格 [PR #305309](https://github.com/microsoft/vscode/pull/305309)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - fix：文件拖放編輯中的記憶體流失 [PR #336025](https://github.com/microsoft/vscode/pull/336025)
  - fix：對話方塊主要服務中的記憶體流失 [PR #336019](https://github.com/microsoft/vscode/pull/336019)
  - fix：筆記本附件診斷中的記憶體流失 [PR #333383](https://github.com/microsoft/vscode/pull/333383)
  - fix：簽章說明中的記憶體流失 [PR #336020](https://github.com/microsoft/vscode/pull/336020)
  - fix：擴充功能主機階層中的記憶體流失 [PR #336023](https://github.com/microsoft/vscode/pull/336023)
  - fix：issueReporterOverlay 中的記憶體流失 [PR #335102](https://github.com/microsoft/vscode/pull/335102)
  - fix：工作區符號中的記憶體流失 [PR #336029](https://github.com/microsoft/vscode/pull/336029)
  - fix：擴充功能主機語音中的記憶體流失 [PR #336021](https://github.com/microsoft/vscode/pull/336021)
  - fix：通知動作檢視項目中的記憶體流失 [PR #333340](https://github.com/microsoft/vscode/pull/333340)
- [@yoavbls (Yoav Balasiano)](https://github.com/yoavbls)：允許在 span 樣式中使用 display:inline-block [PR #180498](https://github.com/microsoft/vscode/pull/180498)

### [問題追蹤](https://code.visualstudio.com/updates/v1_139#_issue-tracking)

對問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們非常感謝大家在新功能一準備好就立即試用，所以請經常回來這裡看看有哪些新內容。

> 如果你想閱讀先前 VS Code 版本的版本說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。
