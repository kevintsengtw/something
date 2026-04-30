# Visual Studio Code 1.114 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.114
**發行日期：** 2026 年 4 月 1 日
**原文：** https://code.visualstudio.com/updates/v1_114

---

歡迎使用 Visual Studio Code 1.114 版本。本次發行聚焦於**精簡您的聊天體驗**。

- **[影片預覽](#影像輪播中的影片預覽)**：在聊天附件和檔案總管右鍵選單的影像輪播中預覽影片。
- **[複製聊天回應](#複製聊天最終回應)**：複製最終的 Markdown 聊天回應，方便分享。
- **[聊天疑難排解](#疑難排解先前的聊天工作階段preview)**：使用 `/troubleshoot` 診斷先前工作階段中的聊天自訂項目問題。
- **[簡化的工作區搜尋](#工作區搜尋簡化)**：取得更快速、更一致的語意搜尋結果。

Happy Coding!

---

## 聊天體驗

### 影像輪播中的影片預覽

**設定**：`imageCarousel.chat.enabled`、`imageCarousel.explorerContextMenu.enabled`

在 [1.113 版中引入](https://code.visualstudio.com/updates/v1_113#_images-preview-for-chat-attachments)的影像輪播，現在也支援影片。您可以從聊天附件或檔案總管右鍵選單播放和瀏覽影片。

檢視器包含：

- **影片播放**，附帶控制項
- **導航**，使用箭頭或縮圖瀏覽所有圖片與影片

### 複製聊天最終回應

聊天檢視已有複製整個對話或特定回應的命令。然而，這也包含了 Agent 的思考過程和工具呼叫。

對於只想複製最終回應的情況，現在聊天右鍵選單中新增了 **Copy Final Response** 命令，可複製 Agent 回應的最後一段 Markdown 部分，即在所有工具呼叫執行完畢之後的部分。

### 工作區搜尋簡化

`#codebase` 工具讓 Copilot 對您的程式碼庫進行語意搜尋。這在擁有數萬到數十萬個檔案的程式碼庫中，對於尋找相關的程式碼片段特別有用。

當 `#codebase` 工具首次推出時，它是為 Copilot 的詢問流程而設計的：您提出問題或請求編輯，Copilot 直接在其回應中產生結果。現在幾乎所有 Copilot 互動都是以 Agent 方式進行，Agent 能夠執行多個工具並反覆迭代後再產生編輯或回應，原始 `#codebase` 設計的許多部分已不再適用。

第一個重要的變更是 `#codebase` 現在純粹用於語意搜尋。先前，它可能會退回到較不準確（且效率較低）的模糊文字搜尋。Agent 仍可在需要時執行文字和模糊搜尋，但我們希望讓 `#codebase` 純粹聚焦於語意搜尋。

我們也簡化了程式碼庫索引的管理方式。此索引是讓 `#codebase` 工具能夠快速提供語意搜尋結果的基礎。先前，我們有「本機索引」和「遠端索引」兩個概念。本機索引限於幾千個檔案，且不一定是語意的。遠端索引儲存在遠端，可針對特定儲存庫在團隊間共享，且可支援數百萬個檔案。

現在只有單一狀態：您的程式碼庫是否已建立語意索引？不再區分本機與遠端。在幕後，索引的某些部分可能仍儲存在您的機器上，某些可能來自遠端來源，但您不再需要自行管理這些索引。

以下是這些變更對使用 Copilot 的意義：

- `#codebase` 工具現在始終是語意的，並提供一致的結果。
- Copilot 在有意義時自動使用 `#codebase` 進行語意搜尋。我們按需為您建立索引並自動使用。您無需自行管理索引。
- 先前顯示為已索引的工作區可能需要重新索引。這通常是因為它們使用的是本機的非語意索引。
- 特別大型且沒有 GitHub 儲存庫的程式碼庫目前可能無法索引。我們正在逐步推出對這些程式碼庫的索引支援。

即使您的工作區未建立語意索引，我們發現您仍可透過 Copilot 的其他搜尋方法（文字、grep、符號）取得良好結果。

所有這些變更應使與 Agent 的協作更快速，並為模型提供更高品質的上下文。我們也相信這些變更簡化了 Copilot 的使用方式，以及對其可用工具的理解。

更多詳情請參閱[工作區指南](https://code.visualstudio.com/docs/copilot/reference/workspace-context)。

### 疑難排解先前的聊天工作階段（Preview）

**設定**：`github.copilot.chat.agentDebugLog.enabled`、`github.copilot.chat.agentDebugLog.fileLogging.enabled`

疑難排解技能（透過 `/troubleshoot` 呼叫）透過分析 Agent 偵錯日誌並呈現有關 Agent 行為的洞察，幫助診斷聊天問題。例如，可用來調查為何自訂指令被忽略，或回應速度為何緩慢。

在本次發行中，您現在可以在疑難排解時參考任何先前的聊天工作階段。這讓事後調查問題變得更加容易，而無需重現這些問題。

若要疑難排解先前的工作階段，請使用 `/troubleshoot` 命令並在提示中加入 `#session`。這將觸發一個工作階段選擇器，讓您可以從先前的聊天工作階段列表中選取。

> **提示**：您也可以透過選取 `+`（**Add Context**）> **Sessions** 來附加工作階段。

---

## 語言

### TypeScript 6.0

我們的 JavaScript 和 TypeScript 支援現在使用 TypeScript 6.0。這個重大更新包含重要的修正和改善。值得注意的是，此 TypeScript 版本也棄用了多項舊選項，為 [TypeScript 7.0 重寫](https://devblogs.microsoft.com/typescript/typescript-native-port/)做準備。

您可以在 [TypeScript 部落格](https://devblogs.microsoft.com/typescript/announcing-typescript-6.0/)上閱讀有關 TypeScript 6.0 版本的所有內容。

### Python

- Python Environments 擴充功能中多項與 env 檔案通知和環境管理器選擇優先順序相關的錯誤修正：
  - 工作區儲存的直譯器選擇現在優先於終端機啟動的虛擬環境或 conda 環境（跨重啟保留）。
  - env 檔案變更通知現在包含「Don't Show Again」（不再顯示）選項，可永久關閉此提示。

  *[vscode-python#25867](https://github.com/microsoft/vscode-python/issues/25867)、[vscode-python-environments#1347](https://github.com/microsoft/vscode-python-environments/issues/1347)、[vscode-python-environments#1393](https://github.com/microsoft/vscode-python-environments/pull/1393)*

- Python Environments 擴充功能現在在偵測到 Pixi 環境時推薦社群 Pixi 擴充功能，並在環境管理器優先順序中加入 Pixi。*[vscode-python-environments#1291](https://github.com/microsoft/vscode-python-environments/pull/1291)*

---

## 企業

### 停用 Claude Agent 的群組政策

管理員現在可以使用群組政策來停用聊天中的 Claude Agent 整合。當此政策被套用時，`github.copilot.chat.claudeAgent.enabled` 設定由組織管理，使用者無法啟用 Claude Agent。

此政策設定為布林值，政策金鑰為 `Claude3PIntegration`。更多資訊請參閱企業文件中的[裝置管理政策](https://code.visualstudio.com/docs/setup/enterprise#_device-management)。

---

## 擴充功能貢獻

### GitHub Pull Requests

[GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) 擴充功能持續進展，讓您可以處理、建立和管理 Pull Request 與 Issue。新功能包括：

- 建立 PR 檢視中的分支名稱現在已快取，加速目標分支的載入。
- PR 和 Issue 概覽 Webview 中的 GitHub 永久連結現在會在工作區中存在對應檔案時開啟本機檔案。

請查閱擴充功能 [0.134.0 版本的變更日誌](https://github.com/microsoft/vscode-pull-request-github/blob/main/CHANGELOG.md#01340)以了解本次發行的所有內容。

---

## 提案 API

### 細粒度工具核准

具有核准流程的語言模型工具現在可以將核准範圍限定至特定的參數組合。

例如，內建的「Run VS Code Command」工具可以執行任何 VS Code 命令。使用者可能覺得始終核准 `editor.action.formatDocument` 沒問題，但不希望核准其他命令。透過此 API，工具實作可以將核准範圍限定至特定命令，讓使用者逐一核准每個命令。

```typescript
export interface LanguageModelToolConfirmationMessages {
  /**
   * 設定後，會顯示一個按鈕，讓使用者核准此特定的工具與參數組合。
   * 該值會作為核准選項的標籤顯示。
   *
   * 例如，一個讀取檔案的工具可以將此設為 `"Allow reading 'foo.txt'"`，
   * 讓使用者可以核准該特定檔案，而無需核准該工具的所有呼叫。
   */
  approveCombination?: string | MarkdownString;
}
```

更多詳情請參閱完整的 API 提案：[Fine grain tool approval](https://github.com/microsoft/vscode/blob/af50a47c13e23e0b3c46719dbd92fe00144362a5/src/vscode-dts/vscode.proposed.toolInvocationApproveCombination.d.ts)。

請參閱 [Copilot Chat 擴充功能](https://github.com/microsoft/vscode-copilot-chat/blob/a71b716b1ea82855b90bdab8cd307396b601475e/src/extension/tools/node/vscodeCmdTool.tsx#L101-L105)中的 API 使用範例。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

無

### 即將棄用的項目

- **Edit Mode** 自 VS Code 1.110 版本起正式棄用。使用者可透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用 Edit Mode。此設定將支援至 1.125 版本。從 1.125 版本開始，Edit Mode 將被完全移除，且無法再透過設定啟用。

---

## 重要修正

- [microsoft/vscode #303908](https://github.com/microsoft/vscode/issues/303908) — 修正整合式瀏覽器中 VS Code 快捷鍵優先於頁面快捷鍵的問題
- [microsoft/vscode #299777](https://github.com/microsoft/vscode/issues/299777) — 修正偵錯暫停時整合式瀏覽器中「Add Element to Chat」無法運作的問題

---

## 感謝

Issue 追蹤貢獻者：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@a77ming](https://github.com/a77ming)：修正 Agent Sessions 歡迎頁面中換行標題的間距 [PR #304686](https://github.com/microsoft/vscode/pull/304686)
- [@AshtonYoon (Ashton Yoon)](https://github.com/AshtonYoon)：修正含程式碼區塊的 Markdown 預覽中不順暢的捲動 [PR #287050](https://github.com/microsoft/vscode/pull/287050)
- [@buley (Tay)](https://github.com/buley)：修正：銷毀讀取串流以防止檔案描述符洩漏 [PR #303395](https://github.com/microsoft/vscode/pull/303395)
- [@ConsoleTVs (Erik C. Forés)](https://github.com/ConsoleTVs)：修正（mcp）：解析 Agent 外掛程式 MCP 伺服器定義中的環境變數 [PR #303156](https://github.com/microsoft/vscode/pull/303156)
- [@jonathanrao99 (Jonathan Thota)](https://github.com/jonathanrao99)：瀏覽器：防止新分頁在快速選取中閃爍 [PR #304297](https://github.com/microsoft/vscode/pull/304297)
- [@ShehabSherif0 (Shehab Sherif)](https://github.com/ShehabSherif0)
  - 修正 fuzzyScore2 測試斷言中的運算子優先順序 [PR #304449](https://github.com/microsoft/vscode/pull/304449)
  - 修正效能檢視中阻擋啟動計數的複製貼上錯誤 [PR #304452](https://github.com/microsoft/vscode/pull/304452)
- [@Tyriar (Daniel Imms)](https://github.com/Tyriar)：從 notify、classifier、events 等移除 self [PR #304498](https://github.com/microsoft/vscode/pull/304498)
- [@xingsy97 (xingsy97)](https://github.com/xingsy97)
  - 鍵盤配置 — 替換 macOS 配置標籤中的所有破折號/點號 [PR #303971](https://github.com/microsoft/vscode/pull/303971)
  - 編輯器 — 修正貼上偏好設定篩選器匹配所有提供者的問題 [PR #304044](https://github.com/microsoft/vscode/pull/304044)
  - 設定編輯器 — 避免重複的擴充功能清單重新整理 [PR #303957](https://github.com/microsoft/vscode/pull/303957)
  - 設定：使用本機 StopWatch 以避免並行搜尋之間的計時錯亂 [PR #304361](https://github.com/microsoft/vscode/pull/304361)
  - mergeEditor：將 removeDiffs 從 O(K*N) 最佳化為單次遍歷 O(N) [PR #304404](https://github.com/microsoft/vscode/pull/304404)
  - timeline：修正切換面板可見性時的記憶體洩漏 [PR #304668](https://github.com/microsoft/vscode/pull/304668)
  - notebook：修正未使用的儲存格查找和損壞的選取去重 [PR #305105](https://github.com/microsoft/vscode/pull/305105)
  - Chat — 移除已棄用的 prompt 屬性拼寫 [PR #301976](https://github.com/microsoft/vscode/pull/301976)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - 修正：防止終端機面板覆寫 terminalEditorActive 上下文鍵 [PR #304802](https://github.com/microsoft/vscode/pull/304802)
  - 修正：現代化 HTML 範例程式碼片段 [PR #304818](https://github.com/microsoft/vscode/pull/304818)
  - 修正：讓測試圖示顏色繼承自清單的 error/warning 前景色 [PR #304959](https://github.com/microsoft/vscode/pull/304959)
  - 修正：為沒有明確寬度/高度尺寸的 SVG 啟用縮放 [PR #304973](https://github.com/microsoft/vscode/pull/304973)
  - 修正：跨工作階段持久化測試涵蓋率排序順序 [PR #304979](https://github.com/microsoft/vscode/pull/304979)
  - 修正：即使沒有可見的編輯器也向 TS 伺服器傳送使用者偏好設定 [PR #304987](https://github.com/microsoft/vscode/pull/304987)

`vscode-pull-request-github` 程式碼貢獻者：

- [@Daniel-Aaron-Bloom](https://github.com/Daniel-Aaron-Bloom)：在 Webview 中將永久連結連結至本機檔案 [PR #8583](https://github.com/microsoft/vscode-pull-request-github/pull/8583)

`monaco-editor` 程式碼貢獻者：

- [@pgoslatara (Pádraic Slattery)](https://github.com/pgoslatara)：chore：更新過時的 GitHub Actions 版本 [PR #5214](https://github.com/microsoft/monaco-editor/pull/5214)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Image Carousel | 影像輪播 |
| Video Playback | 影片播放 |
| Explorer Context Menu | 檔案總管右鍵選單 |
| Copy Final Response | 複製最終回應 |
| Chat Response | 聊天回應 |
| Tool Calls | 工具呼叫 |
| Markdown | Markdown |
| Context Menu | 右鍵選單 |
| Troubleshoot | 疑難排解 |
| Agent Debug Logs | Agent 偵錯日誌 |
| Session Picker | 工作階段選擇器 |
| Custom Instructions | 自訂指令 |
| Workspace Search | 工作區搜尋 |
| Semantic Search | 語意搜尋 |
| Semantic Index | 語意索引 |
| Local Index | 本機索引 |
| Remote Index | 遠端索引 |
| Fuzzy Text Search | 模糊文字搜尋 |
| #codebase | #codebase 工具 |
| Agentic | Agent 方式 |
| TypeScript 6.0 | TypeScript 6.0 |
| TypeScript 7.0 Rewrite | TypeScript 7.0 重寫 |
| Deprecated Options | 棄用的選項 |
| Python Environments Extension | Python Environments 擴充功能 |
| Interpreter Selection | 直譯器選擇 |
| Precedence | 優先順序 |
| Virtual Environment | 虛擬環境 |
| Conda Environment | Conda 環境 |
| Pixi | Pixi |
| Environment Manager Priority | 環境管理器優先順序 |
| Group Policy | 群組政策 |
| Claude Agent Integration | Claude Agent 整合 |
| Claude3PIntegration | Claude3PIntegration 政策金鑰 |
| Device Management | 裝置管理 |
| GitHub Pull Requests | GitHub Pull Requests 擴充功能 |
| Permalink | 永久連結 |
| Proposed APIs | 提案 API |
| Fine-grained Tool Approval | 細粒度工具核准 |
| Approval Flow | 核准流程 |
| approveCombination | approveCombination 屬性 |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |
| Notable Fixes | 重要修正 |
| Integrated Browser | 整合式瀏覽器 |

---

*資料來源：[Visual Studio Code 1.114 發行說明](https://code.visualstudio.com/updates/v1_114)*
