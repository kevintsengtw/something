# Visual Studio Code 1.124

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們。

---

_發布日期：2026 年 6 月 10 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.124.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.124.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.124.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.124.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.124.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.124.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.124.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.124.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.124.0/linux-snap-x64/stable)

---

歡迎使用 Visual Studio Code 1.124 版本。本次發布讓你在多個 agent 工作階段之間的協作更為快速，並賦予 agent 更多自主權來完成你的任務。

- [Autopilot](#_autopilot-is-enabled-by-default)：Autopilot 現已預設啟用，並變得更聰明，能判斷任務是否真正完成。
- [背景工作階段](#_background-send-for-new-sessions)：快速在背景送出請求，並繼續撰寫下一個工作階段。
- [工作階段導覽](#_navigate-between-sessions)：透過鍵盤搜尋、跳轉與逐步切換 agent 工作階段。
- [瀏覽器歷史記錄](#_history)：重新瀏覽並搜尋你在整合式瀏覽器中已開啟過的頁面。

祝你 Coding 愉快！

---

## Agents 視窗（預覽）

[Agents 視窗](https://aka.ms/VSCode/Agents/docs)是一個專屬的輔助視窗，針對跨專案與跨機器探索、迭代與檢閱 agent 工作階段而最佳化。本次發布聚焦於讓你在工作階段之間切換得更快，以及在重新載入後保留你的內容（context）。

### 新工作階段的背景送出

先前，開始一個新工作階段時，你必須等它載入完成才能撰寫下一個。現在你可以改為在背景送出請求：在新工作階段檢視中按 Alt+Enter（或按住 Alt 並選擇 **Send**）。

該檢視會立即重設並保留其狀態（例如所選的模型與內容），只清除查詢文字，因此你可以持續排入請求。每個已啟動的工作階段在提交後會出現在工作階段清單中。

以 Enter 送出提示仍會像先前一樣導覽進入新工作階段。

### 在工作階段之間導覽

當你在多個 agent 工作階段之間工作時，能夠快速尋找並切換它們十分重要。本次發布新增了多種以鍵盤驅動的方式在工作階段之間移動，從可搜尋的選擇器到前後導覽，再到依位置直接跳轉。

- **工作階段選擇器（Sessions picker）**：按 Ctrl+R（macOS 上為 Cmd+R）開啟一個 Quick Pick，將你的工作階段分為兩組列出：**最近開啟（recently opened）**與**其他工作階段（other sessions）**，並預先選取目前作用中的工作階段。可同時搜尋工作階段標題與其資料夾，然後：

    - 按 Enter 開啟所選的工作階段。
    - 按 Cmd/Ctrl+Enter 在側邊開啟它。
    - 按右方向鍵在背景開啟它，同時保持選擇器開啟。

    ![顯示工作階段選擇器 Quick Pick 的螢幕截圖，工作階段分為最近開啟與其他工作階段兩組。](https://code.visualstudio.com/assets/updates/1_124/agents-recent-sessions.webp)

- **往回與往前（Go back and forward）**：按 Ctrl+Tab 往回、Ctrl+Shift+Tab 往前，依最近造訪順序在你已造訪過的工作階段之間切換。

- **上一個與下一個工作階段（Previous and Next session）**：**Go to Previous Session** 與 **Go to Next Session** 命令會依顯示順序逐步切換可見的工作階段清單，並遵循分組、篩選與已摺疊的區段，且會在清單邊緣處停住。使用 Alt+Up／Alt+Down（或 Ctrl+PageUp／Ctrl+PageDown；macOS 上為 Cmd+Alt+Left／Cmd+Alt+Right）。

- **依位置聚焦工作階段（Focus a session by position）**：按 Ctrl+1 至 Ctrl+9（macOS 上為 Cmd+1 至 Cmd+9），由左至右聚焦格線中第 N 個可見的工作階段。

### 重新載入時還原工作階段

重新載入或重新開啟 Agents 視窗不再會遺失你的版面配置。先前可見的工作階段及其狀態會自動還原，讓你回到離開時的位置。這包括：

- 可見的工作階段格線：哪些工作階段是開啟的、它們由左至右的順序、作用中的工作階段，以及任何固定（sticky）或釘選（pinned）的工作階段。
- 各工作階段的版面配置，包括輔助列（auxiliary bar）的可見性、作用中的檢視容器，以及在各工作階段中開啟的編輯器。
- 工作階段清單狀態，包括每個工作階段的釘選與已讀狀態。

### 關閉所有工作階段

類似於編輯器的 **Close All...** 命令，你現在可以透過新的 **Close All Sessions** 命令一步關閉所有工作階段。當你開啟了許多工作階段，想要快速切換到新的工作階段而不必逐一關閉時，這特別有用。

在某個工作階段取得焦點時使用鍵盤快速鍵 Ctrl+K Ctrl+W（macOS 上為 Cmd+K Cmd+W），或從命令面板（Command Palette）存取此命令。

### Changes 檢視中的單檔差異比對（預覽）

**設定**：`sessions.changes.openSingleFileDiff`

預設情況下，在 **Changes** 檢視中選取某個檔案會開啟多檔差異編輯器（multi-file diff editor）。若只想開啟該檔案的變更，你必須按住 Alt 並選取該檔案（**Open Changes**）。

若要在 Changes 檢視中選取檔案時一律開啟聚焦的單檔差異編輯器，請啟用 `sessions.changes.openSingleFileDiff`。這讓你能一次專注於一項變更，不受多檔差異中其他檔案的干擾。啟用此設定後，多餘的 **Open Changes** 行內動作會被隱藏。

### 以側邊欄箭號加寬編輯器

當你在 Agents 視窗內開啟檔案時，編輯器標題列的索引標籤右側現在會出現一個箭號（chevron）切換按鈕。選取它可摺疊次要側邊欄（輔助列，auxiliary bar）並加寬編輯器，再次選取則讓側邊欄回來。箭號方向會反映目前側邊欄的可見狀態。

## Autopilot（預覽）

Autopilot 是聊天[權限層級](https://code.visualstudio.com/docs/agents/approvals#_permission-levels)之一，允許 agent 主動採取行動並自主運作，無需針對每個動作取得使用者的明確核准。

### Autopilot 預設啟用

**設定**：`chat.permissions.default`、`chat.tools.global.autoApprove`（此設定由組織層級管理，請聯絡你的系統管理員以進行變更。）

Autopilot 現已在 VS Code 中預設啟用。組織仍可透過 `chat.tools.global.autoApprove` 設定控制 Autopilot 的可見性與使用（此設定由組織層級管理，請聯絡你的系統管理員以進行變更）。

你可以使用 `chat.permissions.default` 設定新聊天的預設權限層級。你可以隨時在聊天輸入框中變更目前的權限層級。

### 進階 Autopilot

**設定**：`chat.autopilot.advanced.enabled`

要知道 agent 何時真正完成任務並不容易。停得太早，工作就不完整；迴圈跑太久，又浪費時間與 token。進階 Autopilot 改變了 Autopilot（預覽）決定何時繼續迭代、何時結束的方式，讓你在不需手動監看迴圈的情況下，獲得更完整的結果。

進階 Autopilot 不再依賴固定規則，而是由一個小型的工具模型（utility model）閱讀聊天的對話記錄，並判斷任務是否已完成。Autopilot 所朝向的目標會顯示在聊天上方的工具提示（tooltip）中，因此你隨時都能看到它正試圖達成什麼。為了讓過程有所限制，Autopilot 在停止前最多迴圈三次。

將 `chat.autopilot.advanced.enabled` 設為 `true` 即可試用此功能。

## 編輯器體驗

### 從簡易檔案對話框開啟資料夾時建立資料夾

當你從簡易檔案對話框（`files.simpleDialog.enable`）開啟資料夾時，現在可以直接在對話框中建立新資料夾：輸入你想建立的資料夾名稱，然後按 Enter 或選擇 **OK**。

### 整合式瀏覽器

#### 歷史記錄

**設定**：`workbench.browser.maxHistoryEntries`

整合式瀏覽器現在會保留已造訪頁面的歷史記錄。歷史項目會在你於 URL 列輸入時以建議形式出現，並可在瀏覽器索引標籤內使用 ⌘H（Windows、Linux 上為 Ctrl+H）來管理。可記住的歷史項目最大數量可透過 `workbench.browser.maxHistoryEntries` 調整。

![顯示整合式瀏覽器 URL 列的螢幕截圖，輸入「Wiki」後從歷史記錄中顯示了數個 Wikipedia 頁面。](https://code.visualstudio.com/assets/updates/1_124/browser-history.webp)

#### 改進的工具列可自訂性

先前，只有 **Add Element to Chat**（將元素加入聊天）與 **Toggle Developer Tools**（切換開發人員工具）這兩個動作能夠持續顯示在瀏覽器工具列的右側。

現在，所有出現在溢位選單（overflow menu）中的動作，也都可以透過在 URL 輸入框右側的工具列區域按右鍵，使其持續顯示：

![顯示內容選單的螢幕截圖，讓使用者可切換可見的選單項目。](https://code.visualstudio.com/assets/updates/1_124/browser-toolbar.webp)

#### 更快的 agentic 文字輸入

先前，輸入文字並按 Enter 需要兩次個別的工具呼叫（tool call）。現在，`typeInPage` 工具支援 `submit` 參數，讓 agent 能在單一工具呼叫中輸入文字並按下 Enter。

這減少了常見文字輸入情境的來回往返（round-trips）。

![顯示工具呼叫的螢幕截圖，其中 agent 將文字輸入頁面並按下 Enter。](https://code.visualstudio.com/assets/updates/1_124/browser-type-tool.webp)

## 企業（Enterprise）

### 企業管理的 Copilot 外掛政策（實驗性）

VS Code 現在會從與 [Copilot CLI 的企業外掛標準](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/configure-enterprise-plugin-standards)相同的設定檔讀取政策，因此單一政策定義可同時套用於兩個用戶端。未來，VS Code 與 CLI 將在此共用來源上進一步對齊政策管理。

企業管理員現在可以集中控制哪些聊天外掛與外掛市集（plugin marketplaces）可供開發人員使用，而不必要求每位開發人員在本機各自設定。

在 1.123 中導入的三個由政策支援的設定，可透過 Copilot 企業設定檔或既有的 MDM 解決方案進行設定：

- `chat.plugins.enabledPlugins`（此設定由組織層級管理，請聯絡你的系統管理員以進行變更）：組織明確啟用或停用的外掛 ID 允許清單（allowlist）。
- `chat.plugins.extraMarketplaces`：組織想要提供的額外外掛市集。
- `chat.plugins.strictMarketplaces`（此設定由組織層級管理，請聯絡你的系統管理員以進行變更）：啟用時，只信任由政策提供的市集。

被政策封鎖的外掛仍會顯示，但呈現為已停用狀態。由政策管理的市集會在市集選擇器中標記出來。

## 已棄用的功能與設定

無

## 致謝

對我們問題追蹤（issue tracking）的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

對 `vscode` 的貢獻：

- [@1Burhanuddin (Burhanuddin Mundrawala)](https://github.com/1Burhanuddin)：fix: 修正 parcelWatcher.ts 註解中的拼字錯誤 occured -> occurred [PR #319721](https://github.com/microsoft/vscode/pull/319721)
- [@ajasad25 (Asad Meeran)](https://github.com/ajasad25)：修正問題回報器（issue reporter）的「Preview on GitHub」開啟的是儲存庫根目錄而非新問題表單的問題 [PR #319577](https://github.com/microsoft/vscode/pull/319577)
- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
    - 改善擴充功能更新後關於重新啟動／重新載入的訊息（fix #297278） [PR #307353](https://github.com/microsoft/vscode/pull/307353)
    - 在 quickpick 中恢復符號圖示顏色（fix #299650） [PR #299753](https://github.com/microsoft/vscode/pull/299753)
- [@guomaggie](https://github.com/guomaggie)：從 Copilot Proxy 切換到 CAPI [PR #318443](https://github.com/microsoft/vscode/pull/318443)
- [@ishaq2321 (Muhammad Ishaq Khan)](https://github.com/ishaq2321)
    - debug：載入已儲存的中斷點／監看式（watch expressions）時記錄錯誤（#319805） [PR #319806](https://github.com/microsoft/vscode/pull/319806)
    - editor：在 shadowCaretRangeFromPoint 中快取 getComputedStyle 的結果（#319803） [PR #319804](https://github.com/microsoft/vscode/pull/319804)
- [@KirtiRamchandani (Kirtikumar Anandrao Ramchandani)](https://github.com/KirtiRamchandani)：fix: 顯示遺漏的 Git LFS git 錯誤 [PR #319973](https://github.com/microsoft/vscode/pull/319973)
- [@maruthang (Maruthan G)](https://github.com/maruthang)：fix(explorer): 防護檔案總管捲動處理常式（scroll handler）以避免暫時性的 tree-map 不同步（#188365） [PR #310833](https://github.com/microsoft/vscode/pull/310833)
- [@mohanrajvenkatesan23-04 (Mohanraj Venkatesan)](https://github.com/mohanrajvenkatesan23-04)：html-language-features：包含 JSDoc 摘要與標籤於

[![Microsoft 首頁](https://code.visualstudio.com/assets/icons/microsoft.svg)](https://www.microsoft.com/)
