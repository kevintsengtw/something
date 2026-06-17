# Visual Studio Code 1.124

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 6 月 10 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.124.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.124.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.124.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.124.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.124.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.124.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.124.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.124.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.124.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.124 版本。本次發行讓跨 Agent 工作階段的工作更快速，並賦予 Agent 更多自主權來完成您的任務。

- [**Autopilot**](#autopilot-預設啟用)：Autopilot 預設啟用，現在更聰明地判斷任務何時真正完成。
- [**背景工作階段**](#新工作階段的背景傳送)：快速在背景傳送請求並繼續撰寫下一個工作階段。
- [**工作階段導覽**](#工作階段間導覽)：透過鍵盤搜尋、跳轉和步進 Agent 工作階段。
- [**瀏覽器歷史記錄**](#歷史記錄)：重新造訪和搜尋您已在整合式瀏覽器中開啟過的頁面。

Happy Coding!

---

## Agents 視窗（Preview）

[Agents 視窗](https://aka.ms/VSCode/Agents/docs)是一個專用的輔助視窗，針對跨專案和機器探索、迭代和審閱 Agent 工作階段進行最佳化。本次發行專注於加速在工作階段之間的移動，以及跨重新載入時保留您的上下文。

### 新工作階段的背景傳送

先前，啟動新工作階段意味著需要等待它載入完畢才能撰寫下一個。您現在可以改為在背景傳送請求：在新工作階段檢視中按 Alt+Enter（或按住 Alt 並選取 **Send**）。

檢視會立即重設並保留其狀態，例如已選的模型和上下文，僅清除查詢文字，讓您可以繼續排隊請求。每個啟動的工作階段在提交後出現在工作階段清單中。

使用 Enter 傳送提示仍會像以前一樣導覽進入新工作階段。

### 工作階段間導覽

當您跨多個 Agent 工作階段工作時，能夠快速找到並切換工作階段非常重要。本次發行新增了多種鍵盤驅動的方式來瀏覽您的工作階段，從可搜尋的選擇器到前進/後退導覽和依位置直接跳轉。

- **工作階段選擇器**：按 Ctrl+R（macOS 上為 Cmd+R）開啟 Quick Pick，以兩個群組列出您的工作階段：**recently opened** 和 **other sessions**，並預選活動工作階段。可搜尋工作階段標題和資料夾，然後：

  - 按 Enter 開啟選取的工作階段。
  - 按 Cmd/Ctrl+Enter 在側邊開啟。
  - 按右箭頭在背景開啟，同時保持選擇器開啟。

  ![截圖顯示工作階段選擇器 Quick Pick，工作階段分為 recently opened 和 other sessions 兩組。](https://code.visualstudio.com/assets/updates/1_124/agents-recent-sessions.webp)

- **前進和後退**：按 Ctrl+Tab 後退，按 Ctrl+Shift+Tab 前進，依最近造訪順序瀏覽您已造訪的工作階段。

- **上一個和下一個工作階段**：**Go to Previous Session** 和 **Go to Next Session** 命令依顯示順序步進可見的工作階段清單，遵循分組、篩選和摺疊的區段，並在清單邊緣停止。使用 Alt+Up / Alt+Down（或 Ctrl+PageUp / Ctrl+PageDown；macOS 上為 Cmd+Alt+Left / Cmd+Alt+Right）。

- **依位置聚焦工作階段**：按 Ctrl+1 至 Ctrl+9（macOS 上為 Cmd+1 至 Cmd+9）聚焦網格中從左到右第 N 個可見的工作階段。

### 重新載入時還原工作階段

重新載入或重新開啟 Agents 視窗不再遺失您的佈局。先前可見的工作階段及其狀態會自動還原，讓您回到離開時的位置。這包括：

- 可見的工作階段網格：哪些工作階段已開啟、它們的左右順序、活動工作階段，以及任何置頂或釘選的工作階段。
- 每工作階段的佈局，包括輔助列可見性、活動檢視容器，以及每個工作階段中開啟的編輯器。
- 工作階段清單狀態，包括每個工作階段的釘選和已讀狀態。

### 關閉所有工作階段

類似於編輯器的 **Close All...** 命令，您現在可以使用全新的 **Close All Sessions** 命令一次關閉所有工作階段。當您開啟了許多工作階段且想快速切換至新工作階段時，無需逐一關閉每個工作階段，這特別有用。

使用鍵盤快捷鍵 Ctrl+K Ctrl+W（macOS 上為 Cmd+K Cmd+W），在工作階段取得焦點時執行，或從命令選擇區存取命令。

### Changes 檢視中的單檔差異（Preview）

**設定**：`sessions.changes.openSingleFileDiff`

預設情況下，在 **Changes** 檢視中選取檔案會開啟多檔差異編輯器。若要僅開啟該檔案的變更，您必須按住 Alt 並選取檔案（**Open Changes**）。

若要在 Changes 檢視中選取檔案時始終開啟聚焦的單檔差異編輯器，請啟用 `sessions.changes.openSingleFileDiff`。這讓您可以一次專注於一個變更，不會被多檔差異中的其他檔案分散注意力。啟用此設定後，多餘的 **Open Changes** 行內操作會被隱藏。

### 使用側邊列箭頭加寬編輯器

當您在 Agents 視窗內開啟檔案時，編輯器標題列的分頁右側會出現一個箭頭切換按鈕。選取它可收合次要側邊列（輔助列）以加寬編輯器，再次選取可恢復側邊列。箭頭方向反映目前的側邊列可見性。

---

## Autopilot（Preview）

Autopilot 是聊天[權限等級](https://code.visualstudio.com/docs/agents/approvals#_permission-levels)之一，允許 Agent 主動採取行動並自主執行，無需每個操作都獲得使用者的明確核准。

### Autopilot 預設啟用

**設定**：`chat.permissions.default`、`chat.tools.global.autoApprove`（此設定由組織層級管理。請聯繫您的管理員以變更。）

Autopilot 現在在 VS Code 中預設啟用。組織仍可透過 `chat.tools.global.autoApprove`（此設定由組織層級管理。請聯繫您的管理員以變更。）設定來控制 Autopilot 的可見性和使用。

您可以使用 `chat.permissions.default` 設定新聊天的預設權限等級。您可以隨時在聊天輸入框中變更目前的權限等級。

### Advanced Autopilot

**設定**：`chat.autopilot.advanced.enabled`

判斷 Agent 何時真正完成任務是困難的。過早停止則工作不完整，迴圈太久則浪費時間和 Token。Advanced Autopilot 改變了 Autopilot（Preview）決定何時繼續迭代和何時完成的方式，讓您獲得更完整的結果而無需手動監控迴圈。

它不依賴固定規則，而是由一個小型公用模型讀取聊天紀錄並判斷任務是否完成。Autopilot 努力達成的目標會顯示在聊天上方的工具提示中，讓您始終可以看到它正在嘗試完成什麼。為了保持有限度，Autopilot 最多迴圈三次後就會停止。

將 `chat.autopilot.advanced.enabled` 設為 `true` 以試用此功能。

---

## 編輯器體驗

### 從簡易檔案對話框開啟資料夾時建立資料夾

當您從簡易檔案對話框（`files.simpleDialog.enable`）開啟資料夾時，現在可以直接在對話框中輸入要建立的資料夾名稱並按 Enter 或選取 **OK** 來建立新資料夾。

### 整合式瀏覽器

#### 歷史記錄

**設定**：`workbench.browser.maxHistoryEntries`

整合式瀏覽器現在保留造訪頁面的歷史記錄。歷史項目在網址列輸入時作為建議顯示，並可在瀏覽器分頁中使用 ⌘H（Windows、Linux Ctrl+H）進行管理。要記住的歷史項目最大數量可透過 `workbench.browser.maxHistoryEntries` 調整。

![截圖顯示整合式瀏覽器中的網址列，輸入「Wiki」後顯示來自歷史記錄的數個 Wikipedia 頁面。](https://code.visualstudio.com/assets/updates/1_124/browser-history.webp)

#### 改善的工具列自訂性

先前，只有 **Add Element to Chat** 和 **Toggle Developer Tools** 可以在瀏覽器工具列右側設為持續顯示。

現在，溢出選單中出現的所有操作都可以透過右鍵點擊 URL 輸入右側的工具列區域來設為持續顯示：

![截圖顯示右鍵選單，允許使用者切換可見的選單項目。](https://code.visualstudio.com/assets/updates/1_124/browser-toolbar.webp)

#### 更快的 Agent 文字輸入

先前，輸入文字和按 Enter 需要兩個獨立的工具呼叫。現在，`typeInPage` 工具支援 `submit` 參數，允許 Agent 在一次工具呼叫中輸入文字並按 Enter。

這減少了常見文字輸入場景的來回次數。

![截圖顯示 Agent 在頁面中輸入文字並按 Enter 的工具呼叫。](https://code.visualstudio.com/assets/updates/1_124/browser-type-tool.webp)

---

## 企業

### 企業管理的 Copilot 外掛政策（實驗性）

VS Code 現在從與 [Copilot CLI 企業外掛標準](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/configure-enterprise-plugin-standards)相同的設定檔讀取政策，因此單一政策定義適用於兩個客戶端。未來，VS Code 和 CLI 將在此共用來源的政策管理上進一步對齊。

企業管理員現在可以集中控制哪些聊天外掛和外掛市集可供開發者使用，而無需要求每個開發者在本機設定。

在 1.123 中引入的三個新政策支援設定，可透過 Copilot 企業設定檔或現有的 MDM 解決方案進行設定：

- `chat.plugins.enabledPlugins`（此設定由組織層級管理。請聯繫您的管理員以變更。）：組織明確啟用或停用的外掛 ID 允許清單。
- `chat.plugins.extraMarketplaces`：組織想要提供的額外外掛市集。
- `chat.plugins.strictMarketplaces`（此設定由組織層級管理。請聯繫您的管理員以變更。）：啟用後，僅信任由政策提供的市集。

被政策封鎖的外掛仍然可見但顯示為已停用。由政策管理的市集在市集選擇器中標記為受管理。

---

## 已棄用的功能和設定

無。

---

## 感謝

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

對 `vscode` 的貢獻：

- [@1Burhanuddin (Burhanuddin Mundrawala)](https://github.com/1Burhanuddin)：修正：更正 parcelWatcher.ts 註解中的拼寫錯誤 occured -> occurred [PR #319721](https://github.com/microsoft/vscode/pull/319721)
- [@ajasad25 (Asad Meeran)](https://github.com/ajasad25)：修正問題回報器的「Preview on GitHub」開啟儲存庫根目錄而非 new-issue 表單 [PR #319577](https://github.com/microsoft/vscode/pull/319577)
- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
  - 改善擴充功能更新後關於重新啟動/重新載入的訊息（修正 #297278）[PR #307353](https://github.com/microsoft/vscode/pull/307353)
  - 恢復 quickpick 中的符號圖示顏色（修正 #299650）[PR #299753](https://github.com/microsoft/vscode/pull/299753)
- [@guomaggie](https://github.com/guomaggie)：從 Copilot Proxy 切換至 CAPI [PR #318443](https://github.com/microsoft/vscode/pull/318443)
- [@ishaq2321 (Muhammad Ishaq Khan)](https://github.com/ishaq2321)
  - 偵錯：載入儲存的中斷點/監看運算式時記錄錯誤（#\_319805）[PR #319806](https://github.com/microsoft/vscode/pull/319806)
  - 編輯器：在 shadowCaretRangeFromPoint 中快取 getComputedStyle 結果（#\_319803）[PR #319804](https://github.com/microsoft/vscode/pull/319804)
- [@KirtiRamchandani (Kirtikumar Anandrao Ramchandani)](https://github.com/KirtiRamchandani)：修正：顯示遺漏的 Git LFS git 錯誤 [PR #319973](https://github.com/microsoft/vscode/pull/319973)
- [@maruthang (Maruthan G)](https://github.com/maruthang)：修正（explorer）：防止檔案總管捲動處理程式因暫時性的 tree-map 不同步而出錯（#\_188365）[PR #310833](https://github.com/microsoft/vscode/pull/310833)
- [@mohanrajvenkatesan23-04 (Mohanraj Venkatesan)](https://github.com/mohanrajvenkatesan23-04)：html-language-features：在懸停中包含 JSDoc 摘要和標籤

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agents window | Agents 視窗 |
| Autopilot | Autopilot |
| auxiliary bar | 輔助列 |
| background send | 背景傳送 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Changes view | Changes 檢視 |
| chevron | 箭頭 |
| context window | 上下文視窗 |
| diff editor | 差異編輯器 |
| enterprise | 企業 |
| extension | 擴充功能 |
| Integrated Browser | 整合式瀏覽器 |
| MDM | MDM |
| model picker | 模型選擇器 |
| multi-file diff | 多檔差異 |
| overflow menu | 溢出選單 |
| permission level | 權限等級 |
| pin / unpin | 釘選 / 取消釘選 |
| plugin | 外掛 |
| plugin marketplace | 外掛市集 |
| Quick Pick | Quick Pick |
| sandbox | 沙箱 |
| session | 工作階段 |
| simple file dialog | 簡易檔案對話框 |
| single-file diff | 單檔差異 |
| terminal | 終端機 |
| token | Token |
| toolbar | 工具列 |
| utility model | 公用模型 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.124 發行說明](https://code.visualstudio.com/updates/v1_124)*
