# Visual Studio Code 1.111 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.111
**發行日期：** 2026 年 3 月 9 日
**原文：** https://code.visualstudio.com/updates/v1_111

---

歡迎使用 Visual Studio Code 1.111 版本，**這是我們首個每週穩定版發行**！本次發行透過以下功能進一步強化 Agent 體驗：

- **[Agent 權限](#autopilot-與-agent-權限)**：為每個工作階段調整 Agent 的自主等級。
- **[Autopilot（Preview）](#autopilotpreview)**：讓 Agent 自主反覆迭代直到完成任務。
- **[Agent 範圍的掛鉤（Preview）](#agent-範圍的掛鉤preview)**：為 Agent 附加前處理和後處理邏輯，不影響其他聊天互動。
- **[Agent 疑難排解](#偵錯事件快照)**：使用偵錯事件快照疑難排解 Agent 行為和自訂項目。

Happy Coding!

---

## Autopilot 與 Agent 權限

**設定**：`chat.autopilot.enabled`

聊天檢視中全新的權限選擇器讓您控制 Agent 的自主程度。權限等級僅適用於目前工作階段。您可以在工作階段期間隨時從權限選擇器中選取不同的等級來變更它。

您可以從以下權限等級中選擇：

| 權限等級 | 說明 |
|---------|------|
| Default Approvals | 使用您已設定的核准設定。需要核准的工具會在執行前顯示確認對話方塊。 |
| Bypass Approvals | 自動核准所有工具呼叫，不顯示確認對話方塊，並在錯誤時自動重試。 |
| Autopilot（Preview） | 自動核准所有工具呼叫、在錯誤時自動重試、自動回應問題，Agent 持續自主工作直到任務完成。 |

### Autopilot（Preview）

Autopilot 在 Insiders 中預設啟用。您可以透過啟用 `chat.autopilot.enabled` 在 Stable 版中啟用它。

在幕後，Agent 持續保持控制並反覆迭代，直到它透過呼叫 `task_complete` 工具表示完成。

> **注意**：**Bypass Approvals** 和 **Autopilot** 會略過手動核准提示，並忽略您已設定的核准設定，包括具潛在破壞性的操作，例如檔案編輯、終端機命令和外部工具呼叫。首次啟用任一等級時，會顯示警告對話方塊要求您確認。只有在您了解安全影響的情況下才使用這些等級。

更多資訊請參閱文件中的 [Autopilot 和 Agent 權限](https://code.visualstudio.com/docs/copilot/agents/agent-tools#permission-levels)。

---

## Agent 範圍的掛鉤（Preview）

**設定**：`chat.useCustomAgentHooks`

自訂 Agent 的前置資料現在支援 Agent 範圍的掛鉤，僅在您選取特定 Agent 或透過 `runSubagent` 呼叫時才會執行。這讓您可以為特定 Agent 附加前處理和後處理邏輯，不影響其他聊天互動。

若要建立 Agent 範圍的掛鉤，請在您的 `.agent.md` 檔案的 YAML 前置資料的 `hooks` 區段中定義它。

若要試用此功能，請啟用 `chat.useCustomAgentHooks` 設定。更多資訊請參閱文件中的 [Agent 範圍的掛鉤](https://code.visualstudio.com/docs/copilot/customization/hooks#_agentscoped-hooks)。

---

## 偵錯事件快照

為幫助您了解和疑難排解 Agent 行為，您現在可以透過 `#debugEventsSnapshot` 將 Agent 偵錯事件的快照作為上下文附加至聊天。使用它來詢問 Agent 有關已載入的自訂項目、Token 消耗，或疑難排解 Agent 行為。

您也可以選取 Agent Debug 面板右上角的閃光聊天圖示，將偵錯事件快照作為附件新增至聊天編輯器。選取附件會開啟 Agent Debug 面板日誌，並篩選至快照拍攝時的時間戳記。

更多資訊請參閱文件中的[偵錯聊天互動](https://code.visualstudio.com/docs/copilot/chat/chat-debug-view)。

---

## 聊天提示改善

聊天體驗快速演進，我們希望確保您了解新功能和改善。我們重新設計了聊天提示體驗，在您的聊天旅程中的適當時機更好地呈現相關提示。

聊天提示現在引導您經歷結構化的入門旅程。基礎提示（例如使用 Plan Agent 和建立自訂 Agent）會先顯示。在您完成或關閉基礎提示後，生活品質提示（例如實驗性設定或生成 Mermaid 圖表）會以隨機順序顯示。

額外的聊天提示改善包括：

- 提示僅在單一聊天工作階段可見時顯示，例如在歡迎檢視或聊天檢視中。如果開啟了多個聊天編輯器，提示會隱藏以減少雜亂。
- 提示包含鍵盤快捷鍵，幫助您發現相關的按鍵綁定。
- 提示在您對它們採取行動或在目前工作階段中關閉後會隱藏。
- 我們新增了 `/init` 和 `/fork` 斜線命令的提示。`/init` 提示幫助您發現初始化專案組態的命令，`/fork` 提示介紹手動對話分叉，讓您可以分支對話以探索不同的方法。

---

## AI CLI 設定檔群組（實驗性）

**設定**：`terminal.integrated.experimental.aiProfileGrouping`

AI CLI 終端機設定檔（例如 GitHub Copilot CLI）現在顯示在終端機設定檔下拉選單頂部的專用群組中，以提升可發現性。若要啟用此功能，請開啟 `terminal.integrated.experimental.aiProfileGrouping` 設定。

---

### 擴充功能 package.json 檔案中本地化字串的基本 IntelliSense

VS Code 支援[在擴充功能的 `package.json` 中本地化字串](https://github.com/microsoft/vscode-l10n?tab=readme-ov-file#packagenlsjson)。在本次迭代中，我們新增了幾個基本的 IntelliSense 功能，讓處理這些本地化字串更加容易。

- `Go to Definition`：跳至或窺視 `package.nls.json` 檔案中本地化字串的定義。
- `Find all References`：查看本地化字串在 `package.json` 或 `package.nls.json` 檔案中被參考的所有位置。

---

## 工程

隨著轉向**每週穩定版發行**，我們持續改善工程流程，以更快的步調交付高品質功能。

### 測試計畫項目建立

我們新增了一鍵體驗，可從功能請求 Issue 建立測試計畫項目。這減少了為新功能設定結構化測試計畫所需的手動步驟。

### 驗證步驟生成

由於測試計畫項目是隨機指派給工程師的，清晰的驗證步驟對於高效和有效的測試至關重要。我們新增了一個按鈕，可在相關的 Issue 上生成驗證步驟。這有助於確保 Issue 在關閉前具有清晰、結構化的步驟來驗證修正和功能。

### PR 媒體自動附加至關聯的 Issue

當您合併一個描述中包含圖片或 GIF 的 Pull Request 時，媒體內容現在會自動作為留言發佈到關聯的 Issue 上。這透過讓修正或功能的視覺展示直接在 Issue 上可見，精簡了驗證流程。

### Chat Showcase 流水線

一個新的自動化流水線處理被標記為 `chat-showcase` 的 Issue。當識別到一個 Showcase Issue 時，會自動建立對應的聊天提示 Issue，讓新增功能提示更加容易。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 重要修正

（無）

---

## 感謝

`vscode` 程式碼貢獻者：

- [@cathaysia (cathaysia)](https://github.com/cathaysia)：修正（json.schemaDownload.trustedDomains）：避免總是更新 json.sch… [PR #298423](https://github.com/microsoft/vscode/pull/298423)
- [@eliericha (Elie Richa)](https://github.com/eliericha)
  - 在 Shell 環境中包含除錯擴充功能主機環境 (#_241078) [PR #298276](https://github.com/microsoft/vscode/pull/298276)
  - 在遠端終端機 Shell 環境中包含遠端除錯擴充功能主機環境 [PR #299007](https://github.com/microsoft/vscode/pull/299007)
- [@jaidhyani (Jai Dhyani)](https://github.com/jaidhyani)：編輯器：為 cursorMove 命令新增 'foldedLine' 單位 [PR #296106](https://github.com/microsoft/vscode/pull/296106)
- [@neruthes (Neruthes 0x5200DF38)](https://github.com/neruthes)：修正編輯器標點符號寬度 [PR #297741](https://github.com/microsoft/vscode/pull/297741)
- [@RajeshKumar11](https://github.com/RajeshKumar11)：MCP Gateway：避免在啟動時阻塞列表呼叫 [PR #298040](https://github.com/microsoft/vscode/pull/298040)
- [@Rohan5commit (Rohan Santhosh)](https://github.com/Rohan5commit)：docs：修正提案 API 註解中的重複用語 [PR #298522](https://github.com/microsoft/vscode/pull/298522)
- [@sanchirico (John Sanchirico)](https://github.com/sanchirico)：修正聊天終端機在串流期間的閃爍 [PR #298598](https://github.com/microsoft/vscode/pull/298598)

`vscode-copilot-chat` 程式碼貢獻者：

- [@24anisha (Anisha Agarwal)](https://github.com/24anisha)：豁免搜尋子代理工具結果不寫入磁碟 [PR #4219](https://github.com/microsoft/vscode-copilot-chat/pull/4219)
- [@arieluchka (Ariel Agranovich)](https://github.com/arieluchka)：docs：不正確的 Jaeger 埠號文件 <------ 簡單修正 [PR #4251](https://github.com/microsoft/vscode-copilot-chat/pull/4251)
- [@bharatvansh (Ayush Singh)](https://github.com/bharatvansh)：避免將子代理 Token 使用量報告至上下文視窗小工具 [PR #3515](https://github.com/microsoft/vscode-copilot-chat/pull/3515)

`language-server-protocol` 程式碼貢獻者：

- [@dietrichm (Dietrich Moerman)](https://github.com/dietrichm)：修正 Neovim LSP 文件的連結 [PR #2236](https://github.com/microsoft/language-server-protocol/pull/2236)
- [@MariaSolOs (Maria Solano)](https://github.com/MariaSolOs)：更新 metamodel [PR #2234](https://github.com/microsoft/language-server-protocol/pull/2234)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Weekly Stable Release | 每週穩定版發行 |
| Agent Permissions | Agent 權限 |
| Permissions Picker | 權限選擇器 |
| Default Approvals | 預設核准 |
| Bypass Approvals | 略過核准 |
| Autopilot | Autopilot（自動駕駛） |
| Autonomy Level | 自主等級 |
| Confirmation Dialog | 確認對話方塊 |
| Auto-approve | 自動核准 |
| Auto-retry | 自動重試 |
| task_complete | task_complete 工具 |
| Destructive Actions | 破壞性操作 |
| Agent-scoped Hooks | Agent 範圍的掛鉤 |
| YAML Frontmatter | YAML 前置資料 |
| .agent.md | .agent.md 檔案 |
| runSubagent | runSubagent |
| Pre-processing / Post-processing | 前處理／後處理 |
| Debug Events Snapshot | 偵錯事件快照 |
| #debugEventsSnapshot | #debugEventsSnapshot |
| Agent Debug Panel | Agent Debug 面板 |
| Token Consumption | Token 消耗 |
| Chat Tips | 聊天提示 |
| Onboarding Journey | 入門旅程 |
| Foundational Tips | 基礎提示 |
| Quality-of-life Tips | 生活品質提示 |
| /init | /init 斜線命令 |
| /fork | /fork 斜線命令 |
| Conversation Forking | 對話分叉 |
| AI CLI Profile Group | AI CLI 設定檔群組 |
| Terminal Profile Dropdown | 終端機設定檔下拉選單 |
| IntelliSense | IntelliSense |
| Localized Strings | 本地化字串 |
| Go to Definition | Go to Definition |
| Find all References | Find all References |
| package.nls.json | package.nls.json |
| Test Plan Item | 測試計畫項目 |
| Verification Steps | 驗證步驟 |
| PR Media Attachment | PR 媒體附件 |
| Chat Showcase Pipeline | Chat Showcase 流水線 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |

---

*資料來源：[Visual Studio Code 1.111 發行說明](https://code.visualstudio.com/updates/v1_111)*
