# Visual Studio Code 1.123

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 6 月 3 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.123.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.123.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.123.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.123.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.123.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.123.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.123.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.123.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.123.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.123 版本。本次發行改善了您與 Agent 和整合式瀏覽器的協作方式。

- [**工作階段同步**](#工作階段同步與-chronicle)：自動跨機器同步您的聊天工作階段並搜尋您的編碼歷史。
- [**Agents 視窗**](#agents-視窗preview)：並排開啟多個 Agent 工作階段，以比較或平行審閱工作。
- [**研究 Agent**](#研究-agentpreview)：對主題進行深度研究，取得詳盡且有引用來源的 Markdown 報告。
- [**整合式瀏覽器更新**](#整合式瀏覽器)：將頁面加入我的最愛以快速存取，以及更多擷取瀏覽器截圖的選項。

> 務必加入 2026 年 6 月 3 日的 [VS Code Live at Build 2026](https://aka.ms/VSCode/Livestage)！

Happy Coding!

---

## Agents

### 工作階段同步與 Chronicle

**設定**：`chat.sessionSync.enabled`（此設定由組織層級管理。請聯繫您的管理員以變更。）

您的聊天工作階段現在會自動同步至您的 GitHub 帳戶，為您提供跨機器和工作區的個人化、可搜尋的工作歷史。

每個工作階段會捕獲對話內容、您觸及的檔案、儲存庫上下文（repo、分支、時間戳記），以及過程中引用的任何 Pull Request、Issue 或提交。

透過聊天中的全新 chronicle 命令（`/chronicle`），您可以善用該歷史：

- 以自然語言查詢過去的工作階段
- 產生站立會議報告
- 取得個人化生產力建議
- 依主題、檔案或 PR 搜尋您的編碼歷史

若要啟用工作階段同步，請開啟 `chat.sessionSync.enabled`（此設定由組織層級管理。請聯繫您的管理員以變更。）。您可以在 VS Code 狀態列的 Copilot 狀態儀表板中查看工作階段同步的狀態。

![Copilot 狀態儀表板中工作階段同步狀態訊息的截圖。](https://code.visualstudio.com/assets/updates/1_123/session-sync-status.webp)

如需更多詳細資訊，請參閱[工作階段同步](https://code.visualstudio.com/docs/agents/sessions/session-sync)和 [Chronicle](https://code.visualstudio.com/docs/agents/sessions/session-insights) 文件。

### 在沙箱中重試網路相關命令

**設定**：`chat.agent.sandbox.retryWithAllowNetworkRequests`

當由*本機* Agent 執行的終端機命令需要存取未設定為允許網域的網域時，該命令會自動在沙箱中以不受限的網路存取重試。之後如果仍然失敗，則退回無沙箱執行。這使得像 `git fetch` 這樣的網路相關操作得以完成，同時保持檔案系統保護。

### Agents 視窗（Preview）

[Agents 視窗](https://aka.ms/VSCode/Agents/docs)是一個專用的輔助視窗，針對跨專案和機器探索、迭代和審閱 Agent 工作階段進行最佳化。本次發行中，我們專注於讓您能夠並排處理多個工作階段。

#### 多個開啟的工作階段

您現在可以在 Agents 視窗中同時開啟多個工作階段。除了活動工作階段外，可透過以下方式在旁邊開啟另一個工作階段：

- 在工作階段清單中的工作階段右鍵選單中選擇 **Open to the Side**。
- 將工作階段從工作階段清單拖放至工作階段檢視區域。
- 按住 Alt 並在工作階段清單中選取工作階段。

即使多個工作階段可以同時顯示，但任何時候只有一個是活動工作階段。**Terminal**、**Files** 和 **Changes** 檢視都操作目前活動的工作階段，因此切換活動工作階段會更新這些檢視以反映其狀態。

預設情況下，在工作階段清單中選取工作階段會以選取的工作階段取代活動工作階段檢視。若要防止工作階段檢視被取代，請使用檢視右上方的釘選操作來釘選它。釘選的工作階段檢視永遠不會被取代——選取另一個工作階段會在未釘選的檢視中開啟。如果每個工作階段檢視都已釘選，選取的工作階段會在側邊開啟。

使用工作階段檢視右上方的最大化操作來將其擴展至所有已開啟的工作階段檢視，讓您在不關閉其他工作階段的情況下聚焦查看單一工作階段。

如需更多詳細資訊，請參閱 [Agents 視窗文件](https://aka.ms/VSCode/Agents/docs)。

### 研究 Agent（Preview）

> **注意**：研究 Agent 目前為 Preview，僅在 Insiders 的 Copilot CLI（本機）工作階段中提供。

當您需要理解不熟悉的程式碼、比較不同方法，或了解某個函式庫或 API 的運作方式時，快速的聊天回答不一定足夠。研究 Agent 對主題進行深度研究，從您的程式碼庫、相關 GitHub 儲存庫和網路蒐集並綜合資訊，產出詳盡且有引用來源的 Markdown 報告。

研究 Agent 針對深度而非速度進行最佳化，且僅有唯讀存取權限，因此它會調查和報告而非變更您的程式碼。若要執行它，請在 Copilot CLI（本機）工作階段的聊天輸入中輸入 `/research` 加上您的主題。

如需更多詳細資訊，請參閱[使用研究 Agent 進行深度研究](https://code.visualstudio.com/docs/agents/agent-types/copilot-cli#run-deep-research-with-the-research-agent)。

---

## 整合式瀏覽器

### 我的最愛頁面

我們將整合式瀏覽器中的網址列重新設計為更多功能的體驗，您不僅可以輸入 URL，還可以將頁面加入我的最愛，並輕鬆存取您的最愛和已開啟的分頁。

若要將頁面加入我的最愛，請選取瀏覽器網址列中的星號圖示。

![整合式瀏覽器的截圖，醒目標示瀏覽器網址列中標記為「Add to Favorites」的星號按鈕。](https://code.visualstudio.com/assets/updates/1_123/browser-favorite-button.webp)

當您選取網址列時，可以看到您的我的最愛頁面清單和已開啟的分頁。

![瀏覽器網址列周圍的彈出視窗截圖，顯示我的最愛和已開啟的分頁。](https://code.visualstudio.com/assets/updates/1_123/browser-url-bar.webp)

### 更多擷取截圖的方式

**設定**：`workbench.browser.experimentalUserTools.enabled`

上一個版本引入了 **Add Screenshot to Chat**，讓您可以將當前瀏覽器視窗的截圖作為上下文附加至聊天。這對 UI 相關任務特別有用，例如偵錯版面配置問題。

本次發行中，我們新增了兩個相關功能：

- **Add Area Screenshot to Chat**：擷取您選取的矩形區域截圖，並將該截圖作為聊天上下文新增。
- **Add Full Page Screenshot to Chat（實驗性）**：擷取整個網頁的截圖，即使超出目前視窗顯示的範圍，並將該截圖作為聊天上下文新增。此實驗性功能需要啟用 `workbench.browser.experimentalUserTools.enabled` 設定。

![整合式瀏覽器的 Add to Chat 選單截圖，顯示「Add Area Screenshot to Chat」和「Add Full Page Screenshot to Chat (Experimental)」選項，擷取的截圖已作為上下文附加在聊天檢視中。](https://code.visualstudio.com/assets/updates/1_123/browser-screenshot-area-fullpage.webp)

---

## 編輯器體驗

### 延遲擴充功能自動更新

VS Code 現在在自動將擴充功能更新至新發佈的版本前會套用兩小時的延遲。當啟用自動更新時，新版本會在發佈兩小時後自動更新，為有問題或可能遭入侵的發行版本新增額外的保護層。

這不會妨礙您的工作，因為您仍可以隨時使用 **Update** 按鈕立即更新任何擴充功能。在等待更新期間，擴充功能的詳細資料檢視會說明為何尚未更新以及自動更新將於何時發生。

> **注意**：此延遲不適用於來自 Microsoft、GitHub 和 OpenAI 等受信任發行者的擴充功能。這些擴充功能仍會立即更新。

![截圖顯示擴充功能的延遲資訊和更新排程。](https://code.visualstudio.com/assets/updates/1_123/extension-delayed-autoupdate.webp)

---

## 感謝

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

對 `vscode` 的貢獻：

- [@aaronpowell (Aaron Powell)](https://github.com/aaronpowell)：為外掛市集新增 marketplace ref 支援 [PR #317901](https://github.com/microsoft/vscode/pull/317901)
- [@goingforstudying-ctrl](https://github.com/goingforstudying-ctrl)：修正：為 browser-emulation-toolbar-label 新增 white-space: nowrap [PR #318935](https://github.com/microsoft/vscode/pull/318935)
- [@guomaggie](https://github.com/guomaggie)：[搜尋子代理] 處理上下文視窗限制超出錯誤 [PR #316529](https://github.com/microsoft/vscode/pull/316529)
- [@maruthang (Maruthan G)](https://github.com/maruthang)：修正：合併 URI 旗標以防止 Windows 上的 Electron 引數過濾 [PR #308150](https://github.com/microsoft/vscode/pull/308150)
- [@oded-ist (Oded S)](https://github.com/oded-ist)：修正 read_cell_output 錯誤地回報所有輸出過大 [PR #318148](https://github.com/microsoft/vscode/pull/318148)
- [@PenguinDOOM (Penguin)](https://github.com/PenguinDOOM)：修正 BYOK 無效的有狀態標記重試 [PR #317292](https://github.com/microsoft/vscode/pull/317292)
- [@rebornix (Peng Lyu)](https://github.com/rebornix)：新增行動裝置多差異檢視 [PR #318081](https://github.com/microsoft/vscode/pull/318081)
- [@SimonSiefke (Simon Siefke)](https://github.com/SimonSiefke)
  - 修正：擴充功能操作的記憶體洩漏 [PR #315054](https://github.com/microsoft/vscode/pull/315054)
  - 修正：ipc.electron.ts 中的記憶體洩漏 [PR #317846](https://github.com/microsoft/vscode/pull/317846)
  - 修正：搜尋結果中的記憶體洩漏 [PR #282309](https://github.com/microsoft/vscode/pull/282309)
- [@SLdragon (rentu)](https://github.com/SLdragon)：功能：為 nes/inline completion 供應商新增 languageDiagnosticsService 選項 [PR #317678](https://github.com/microsoft/vscode/pull/317678)
- [@Tyriar (Daniel Imms)](https://github.com/Tyriar)：修正：移除 Shell 整合測試中 Promise.race 內的 await [PR #319068](https://github.com/microsoft/vscode/pull/319068)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agents window | Agents 視窗 |
| air-gapped | 隔離環境 |
| auto-update | 自動更新 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| chronicle | chronicle |
| context window | 上下文視窗 |
| extension | 擴充功能 |
| favorite | 我的最愛 |
| Integrated Browser | 整合式瀏覽器 |
| pin / unpin | 釘選 / 取消釘選 |
| publisher | 發行者 |
| research agent | 研究 Agent |
| sandbox | 沙箱 |
| session | 工作階段 |
| session sync | 工作階段同步 |
| standup report | 站立會議報告 |
| terminal | 終端機 |
| token | Token |
| trusted publisher | 受信任發行者 |
| viewport | 視窗 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.123 發行說明](https://code.visualstudio.com/updates/v1_123)*
