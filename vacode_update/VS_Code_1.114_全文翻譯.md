# Visual Studio Code 1.114 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.114
**發行日期：** 2026 年 4 月 1 日
**原文：** https://code.visualstudio.com/updates/v1_114

---

歡迎使用 Visual Studio Code 1.114 版本。本次發行專注於簡化您的聊天體驗。

以下是本次發行的主要亮點：

- **影片預覽**：在聊天附件和檔案總管右鍵選單的圖片輪播中預覽影片
- **複製聊天回應**：複製最終的 Markdown 聊天回應，方便分享
- **聊天疑難排解**：使用 /troubleshoot 診斷先前工作階段中的聊天自訂項目問題
- **簡化的工作區搜尋**：取得更快速、更一致的語意搜尋結果
- **TypeScript 6.0**：JavaScript 和 TypeScript 支援現已更新至 TypeScript 6.0
- **企業原則控制**：管理員可透過群組原則控制 AI 功能

---

## GitHub Copilot

### 影片預覽（Video Preview）

在 1.113 版中引入的圖片輪播（Image Carousel）現在也支援影片。您可以從聊天附件或檔案總管右鍵選單播放和導覽影片。

當您在聊天中使用影片，或 Agent 產生影片輸出時，您可以選取影片附件在全功能的媒體檢視器中開啟它。影片支援播放控制、時間軸導覽等標準影片操作功能。

相關設定：`imageCarousel.chat.enabled` 和 `imageCarousel.explorerContextMenu.enabled` 控制這些功能的啟用狀態。

### 複製最終回應（Copy Final Response）

聊天右鍵選單現在提供「Copy Final Response」（複製最終回應）命令，可複製 Agent 回應的最後 Markdown 部分，即在所有工具呼叫完成之後的部分。

先前，複製聊天回應時經常會一併帶入工具呼叫和除錯日誌，在嘗試分享乾淨的程式碼區塊時會造成剪貼簿內容雜亂。新的右鍵選單命令隔離了實際的輸出，讓您可以直接將其貼入文件或 commit 訊息中。

聊天檢視支援多種複製聊天訊息為 Markdown 格式到剪貼簿的選項，可透過在訊息或聊天背景上按右鍵存取右鍵選單。

### 聊天疑難排解增強（Chat Troubleshooting）

您可以使用 `/troubleshoot` 來診斷先前工作階段中的聊天自訂項目問題。疑難排解技能（透過 `/troubleshoot` 呼叫）透過分析 Agent 除錯日誌並呈現有關 Agent 行為的洞察，幫助診斷聊天問題。例如，可用來調查為何自訂指令被忽略，或回應速度為何緩慢。

在本次發行中，您現在可以在疑難排解時參考任何先前的聊天工作階段，讓事後調查問題變得更加容易，而無需重現這些問題。

若要疑難排解先前的工作階段，請使用 `/troubleshoot` 命令並在提示中加入 `#session`，這將觸發一個工作階段選擇器，讓您可以從先前的聊天工作階段列表中選取。

### 簡化的工作區搜尋（Simplified Workspace Search）

取得更快速、更一致的語意搜尋結果。Microsoft 透過移除令人困惑的本地索引與遠端索引區分，並讓 Copilot 在幕後自動處理語意索引，來簡化搜尋功能。

#### 統一的索引模型

先前存在獨立的「本地索引」和「遠端索引」概念——本地索引限於幾千個檔案且不一定是語意的，而遠端索引儲存在遠端，可支援數百萬個檔案。這已被統一為一個模型。

現在只有單一狀態：您的程式碼庫是否已建立語意索引。在幕後，索引的某些部分可能仍儲存在您的機器上，某些可能來自遠端來源，但您不再需要管理這些索引。

#### #codebase 純語意搜尋

`#codebase` 工具現在純粹用於語意搜尋，而先前它可能會退回到較不準確且效率較低的模糊文字搜尋。不過，Agent 仍可在需要時執行文字和模糊搜尋。

#### 自動索引管理

Copilot 會在有意義時自動使用 `#codebase` 進行語意搜尋，按需建立索引並自動使用，您無需自行管理索引。這對大型專案特別有益。

### 釘選工作階段改善（Pinned Sessions）

釘選的聊天工作階段現在在工作階段列表中顯示釘選圖示指示器。這讓您更容易區分已釘選和未釘選的工作階段。

您可以透過將滑鼠懸停在工作階段上並選取釘選圖示來釘選或取消釘選工作階段，將重要的工作階段保持在列表頂端，以便快速存取。

---

## Edit Mode 棄用（Edit Mode Deprecated）

Edit Mode 自 VS Code 1.110 版起已正式棄用。隨著 Agent 的演進，Agent Mode 現在可以處理 Edit Mode 能做的所有事情，而且效能和可靠性更佳。Edit Mode 現在預設從 Agent 選擇器中隱藏，讓使用者無需在選項之間選擇即可受益於最強大的模式。

### 暫時重新啟用

如果您需要暫時繼續使用 Edit Mode，可以透過停用 `chat.editMode.hidden` 設定來恢復它。當您停用 `chat.editMode.hidden` 時，可以在 Agent 選擇器中選取「View edit agent」動作來檢視驅動 Edit Mode 的 Agent 宣告。

### 移除時程

`chat.editMode.hidden` 設定將支援到 1.125 版。自 1.125 版起，Edit Mode 將完全移除，無法再透過設定啟用。

### 替代方案

如果您偏好 Edit Mode 或想要自己版本的 Ask Mode，可以建立一個符合您需求的自訂 Agent，定義其工具、提示和語言模型。

---

## 企業原則控制（Enterprise Policy Controls）

### Claude Agent 整合原則

管理員現在可以使用群組原則來停用聊天中的 Claude Agent 整合。當此原則被套用時，`github.copilot.chat.claudeAgent.enabled` 設定由組織管理，使用者無法啟用 Claude Agent。

此原則設定為布林值，原則金鑰為 `Claude3PIntegration`。

### 組織層級 AI 管理

組織可以集中管理 AI 功能，以控制 AI 行為、執行安全原則並維護合規性。組織可以透過裝置管理解決方案部署企業原則來強制執行特定組態，這些原則會覆蓋受管理裝置上使用者設定的設定。

可管理的項目包括：

- **Agent 功能控制**：`chat.agent.enabled` 設定由組織層級管理
- **MCP 伺服器控制**：`ChatMCP` 原則控制 MCP 伺服器可從哪些來源安裝，並設定 `chat.mcp.access` ORG 設定
- **模型存取控制**：控制使用者可使用的 AI 模型
- **內容排除**：設定哪些內容被排除在 AI 處理之外
- **信任邊界**：強制執行安全信任邊界

---

## 語言支援（Languages）

### TypeScript 6.0

JavaScript 和 TypeScript 支援現已更新為 TypeScript 6.0。這是一個重要的更新，包含許多修正和改善。

#### 過渡版本

TypeScript 6.0 是一個重要的過渡版本，旨在為 TypeScript 7.0（即將推出的 TypeScript 編譯器原生移植版本）做準備。TypeScript 6.0 是基於目前 JavaScript 程式碼庫的最後一個版本，團隊正在以 Go 語言撰寫新的 TypeScript 編譯器程式碼庫，這將成為 TypeScript 7.0 的基礎。

#### 棄用的選項

本次 TypeScript 發行也棄用了多項舊選項，為 TypeScript 7.0 重寫做準備。雖然在 TypeScript 6.0 中棄用的選項在設定 `"ignoreDeprecations": "6.0"` 時仍可正常運作而不會產生錯誤，但這些選項將在 TypeScript 7.0（原生 TypeScript 移植版本）中完全移除。

主要棄用的選項包括：

- ES5 目標（targets）
- classic 模組解析（module resolution）
- 舊版模組格式，例如 AMD 和 UMD

#### TypeScript 7.0 效能展望

TypeScript 7.0 代表著以 Go 語言完全重寫編譯器和語言服務。VS Code 有大約 150 萬行 TypeScript 程式碼，目前基於 JS 的 tsc 需要 89 秒編譯，而基於 Go 的 tsgo 只需 8.74 秒完成相同的工作——速度提升 10.2 倍。

---

## Python

### 直譯器選擇優先順序

Python 環境擴充功能中修正了多項與 env 檔案通知和環境管理員選擇優先順序相關的問題。工作區儲存的直譯器選擇現在優先於終端機啟動的虛擬環境或 conda 環境，且在重啟後保持一致。

### env 檔案變更通知

env 檔案變更通知現在包含「不再顯示」（Don't Show Again）選項，可永久關閉此提示。這讓使用者可以控制工作流程中的通知頻率。

---

## 無障礙功能（Accessibility）

### 無障礙檢視動態串流

無障礙檢視（Accessible View）現在會動態串流聊天回應，在回應生成時即時顯示。先前，您需要關閉再重新開啟無障礙檢視才能看到更新的內容。現在，您可以留在無障礙檢視中，在輸出進入時即時監控，讓即時跟隨 AI 回應變得更加容易。

### MCP 伺服器輸出排除

為了減少噪音，MCP（Model Context Protocol）伺服器輸出現在預設從無障礙檢視中排除。標準聊天輸出保持完全可存取，因為它以文字區域呈現，與螢幕閱讀器配合良好。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Video Preview | 影片預覽 |
| Image Carousel | 圖片輪播 |
| Copy Final Response | 複製最終回應 |
| Chat Response | 聊天回應 |
| Tool Calls | 工具呼叫 |
| Debug Logs | 除錯日誌 |
| Clipboard | 剪貼簿 |
| Chat Troubleshooting | 聊天疑難排解 |
| Session Picker | 工作階段選擇器 |
| Simplified Workspace Search | 簡化的工作區搜尋 |
| Semantic Search | 語意搜尋 |
| Semantic Index | 語意索引 |
| Local Index | 本地索引 |
| Remote Index | 遠端索引 |
| Fuzzy Text Search | 模糊文字搜尋 |
| #codebase | #codebase 工具 |
| Automatic Indexing | 自動索引 |
| Pinned Sessions | 釘選工作階段 |
| Pin Icon | 釘選圖示 |
| Edit Mode | 編輯模式 |
| Agent Mode | Agent 模式 |
| Agent Picker | Agent 選擇器 |
| Custom Agent | 自訂 Agent |
| chat.editMode.hidden | chat.editMode.hidden 設定 |
| Enterprise Policy Controls | 企業原則控制 |
| Group Policy | 群組原則 |
| Claude3PIntegration | Claude3PIntegration 原則金鑰 |
| Organization | 組織 |
| Device Management Solutions | 裝置管理解決方案 |
| Trust Boundaries | 信任邊界 |
| Content Exclusions | 內容排除 |
| TypeScript 6.0 | TypeScript 6.0 |
| TypeScript 7.0 | TypeScript 7.0 |
| Native Port | 原生移植版本 |
| Go Rewrite | Go 重寫 |
| Deprecated Options | 棄用的選項 |
| ignoreDeprecations | ignoreDeprecations 設定 |
| ES5 Targets | ES5 目標 |
| Classic Module Resolution | Classic 模組解析 |
| AMD / UMD | AMD / UMD 模組格式 |
| Interpreter Selection | 直譯器選擇 |
| Precedence | 優先順序 |
| Virtual Environment | 虛擬環境 |
| Conda Environment | Conda 環境 |
| Don't Show Again | 不再顯示 |
| Accessible View | 無障礙檢視 |
| Dynamic Streaming | 動態串流 |
| Screen Reader | 螢幕閱讀器 |
| MCP Server Output | MCP 伺服器輸出 |

---

*資料來源：VS Code 1.114 發行說明 (https://code.visualstudio.com/updates/v1_114)*
