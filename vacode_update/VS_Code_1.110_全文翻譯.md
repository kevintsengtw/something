# Visual Studio Code 更新 — 2026 年 2 月（版本 1.110）

> 來源：[https://code.visualstudio.com/updates/v1_110](https://code.visualstudio.com/updates/v1_110)
> 發布日期：2026 年 3 月 4 日

---

## 更新概覽

歡迎來到 Visual Studio Code 2026 年 2 月的更新。本次版本包含許多更新內容，我們希望您會喜歡。

本次發行版使代理（Agent）在處理更長時間和更複雜的任務時變得更加實用，為使用者提供更多的控制權和可見性、全新的代理擴展方式，以及更智慧的工作階段管理。

---

## GitHub Copilot

### 代理外掛（Agent Plugins）（預覽版）

VS Code 現在支援代理外掛，這是一種可安裝的聊天自訂組合套件，透過外掛市集進行發佈，並可在擴展檢視中被搜尋到。外掛可以包含以下內容：

- **技能（Skills）**：為代理定義特定功能的指令
- **命令（Commands）**：可執行的聊天命令
- **代理（Agents）**：自訂的 AI 代理
- **MCP 伺服器（MCP Servers）**：模型上下文協定伺服器，提供外部工具整合
- **掛鉤（Hooks）**：在特定事件觸發時執行的動作

您可以直接從 VS Code 的擴展檢視中搜尋並安裝代理外掛。操作方式：

1. 在擴展檢視的搜尋框中輸入 `@agentPlugins`
2. 或從命令面板執行 `Chat: Plugins` 命令

預設情況下，VS Code 從 `copilot-plugins` 和 `awesome-copilot` 儲存庫取得外掛。

### 代理瀏覽器工具（Agentic Browser Tools）（實驗性功能）

VS Code 1.110 為聊天功能加入了更多「動手做」的能力，其中以原生瀏覽器整合為首。啟用 `workbench.browser.enableChatTools` 設定後，AI 代理可以：

- **與網頁元素互動**：直接操作頁面上的按鈕、表單和其他元素
- **擷取螢幕截圖**：拍攝網頁的即時畫面以進行視覺除錯和分析
- **讀取主控台日誌**：從瀏覽器主控台即時取得日誌資訊

所有這些操作都在編輯器內完成，無需離開 VS Code。代理可以開啟頁面、點擊元素、甚至擷取截圖——全部在編輯器內進行。這讓代理能夠驅動瀏覽器與您的應用程式互動，並驗證自身所做的變更。

### 工作階段記憶（Session Memory）

工作階段記憶功能現在可以跨對話輪次保存計畫和指引。具體來說：

- Plan 代理建立的計畫現在會持久化到工作階段記憶中
- 計畫在對話的各輪次之間保持可用
- 當您要求進行調整時，代理會在既有計畫的基礎上繼續修改，而非從頭開始重建

### 上下文壓縮（Context Compaction）

Copilot 會在上下文視窗達到容量限制時自動壓縮對話歷史。在本次更新中：

- 您現在可以使用 `/compact` 斜線命令手動觸發上下文壓縮
- 此功能適用於背景代理工作階段
- 手動壓縮可讓您在需要時主動釋放上下文空間，手動修剪對話歷史以釋放模型的上下文視窗

### 分叉聊天工作階段（Fork a Chat Session）

您現在可以使用 `/fork` 命令「分叉」一個聊天工作階段：

- 建立一個全新的、獨立的工作階段
- 新工作階段會繼承原始工作階段的完整對話歷史
- 這讓您可以在不影響原始對話的情況下探索替代方案
- 兩個執行緒保持獨立運作

此功能特別適合在不同實作策略之間進行比較和選擇，例如嘗試不同的程式碼風格。

### 代理除錯面板（Agent Debug Panel）（預覽版）

全新的代理除錯面板提供了對代理運作過程的即時可見性，作為預覽功能提供更深入的聊天事件即時洞察，包括：

- **系統提示詞**：查看發送給模型的系統提示詞內容
- **代理事件**：查看代理執行過程中觸發的各種事件
- **工具呼叫**：追蹤代理使用了哪些工具以及呼叫的詳細內容
- **已載入的自訂設定**：檢視每個工作階段中載入了哪些提示詞檔案、技能、掛鉤或其他自訂調整

此面板對於理解和除錯代理行為非常有價值。

### 休眠防護（Sleep Prevention）

VS Code 現在會要求作業系統在聊天請求正在執行時不要自動暫停機器。這意味著：

- 您可以離開電腦，不必擔心代理的回應被中斷
- 系統會在請求完成後恢復正常的電源管理
- **注意事項**：合上未接電源的筆記型電腦仍然會觸發系統暫停

### 編輯模式淘汰（Edit Mode Deprecated）

隨著代理的演進，Agent Mode（代理模式）現在可以處理 Edit Mode（編輯模式）能做的一切，而且效能和可靠性更佳。在本次更新中：

- Edit Mode 正式被標記為淘汰
- 預設從代理選擇器中隱藏
- 此設定將持續支援到版本 1.125，之後 Edit Mode 將被完全移除且無法再透過設定啟用
- 如果您偏好 Edit Mode 或想要自己版本的 Ask Mode，可以建立符合需求的自訂代理，定義其工具、提示詞和語言模型
- 您可以在代理選擇器中選擇「View edit agent」動作來查看驅動 Edit Mode 的代理宣告，作為建立自訂代理的起點

### 自動核准（Auto-Approve）

新增了 `/autoApprove` 斜線命令，可跳過所有工具確認提示：

- 可以為多步驟建置腳本省去大量確認時間
- 啟用全域自動核准時應僅在您確信每個步驟都安全時使用
- 透過 npm、pnpm 或 yarn 執行的 npm 腳本，若已定義在 package.json 中，現在預設自動核准，因為使用代理已經需要工作區信任（Workspace Trust）

### 新增代理工具

- **`/getDiagnostics`**：新的工具，可直接將編輯器的警告和錯誤拉入聊天，讓代理可以在您觸碰程式碼之前建議修復方案
- **`askQuestions` 工具移入核心**：呈現問題輪播 UI 的 askQuestions 工具已移入 VS Code 核心，提升了取消請求時的可靠性，並使該工具能夠在不同上下文中一致運作，包括子代理

### 聊天自訂與指令檔案

聊天自訂選項如提示詞檔案、掛鉤和技能，現在也可在背景代理工作階段中作為斜線命令使用。

- **始終啟用的指令**：自動套用於每個聊天工作階段
- **基於檔案的指令**：根據檔案路徑模式或指令描述選擇性套用
- 使用 `/init` 斜線命令快速為工作區建立自訂指令

---

## MCP 改進

### Claude Agent 支援

本次更新改進了 MCP 伺服器的支援：

- Claude Agent 現在支援 MCP 伺服器
- 透過 VS Code 或 Claude CLI 安裝的 MCP 伺服器會被「自動偵測並載入」

### 本地 MCP 伺服器沙盒

新增了針對本地 MCP 伺服器的沙盒選項：

- 使用 stdio 傳輸的本地 MCP 伺服器可啟用沙盒模式
- 沙盒化的伺服器僅能存取您明確允許的檔案系統路徑和網路網域
- 啟用沙盒後，工具確認會自動核准，因為伺服器運行在受控環境中
- 提升了安全性和資源隔離

---

## 聊天無障礙功能改進

本次更新在聊天的無障礙功能方面進行了多項重要改進：

### 問題輪播無障礙性
- 問題輪播現在完全支援螢幕閱讀器使用者
- 問題會連同其位置一起朗讀（例如「問題 1/3」）

### 無障礙信號與通知
- 當聊天詢問問題或需要確認時，VS Code 現在會播放無障礙信號並顯示作業系統通知
- 即使您在另一個視窗中工作，也能隨時注意到待處理的動作
- 可透過設定讓通知在視窗處於焦點時也持續顯示（將設定值設為 `always`）

### 鍵盤導航增強
- 使用 `⇧⌘T`（Windows/Linux 為 `Ctrl+Shift+T`）可快速在代理 TODO 清單和聊天輸入之間切換焦點
- 這對螢幕閱讀器使用者特別有幫助，可以快速概覽待處理任務並返回聊天輸入

---

## 工作台（Workbench）

### 通知位置自訂
- 通知現在可以設定為右上、右下或左下顯示
- 讓您可以選擇最適合工作流程的位置

### 聊天設定重新組織
- VS Code 聊天設定已移至設定編輯器中自己的頂層項目，並包含子分類
- GitHub Copilot Chat 擴充功能的設定項目仍保留在「擴充功能」下的獨立項目中

### 導覽路徑（Breadcrumbs）
- 新增「複製導覽路徑（Copy Breadcrumbs Path）」命令，可將導覽路徑複製到剪貼簿
- 適用於與團隊分享符號的精確位置或用於文件撰寫

---

## 編輯器

### 反轉行（Reverse Lines）
- 「反轉行」功能改進：當選取範圍僅為單行時，現在會套用至整份文件

### 符號搜尋修正
- 「前往工作區符號（Go to Symbol in Workspace）」功能修正：搜尋查詢包含 `#` 字元時不再錯誤地過濾掉所有結果
- 此修正使語言擴充功能（如 rust-analyzer）能夠使用 `#` 作為符號搜尋的修飾符

### 效能改進
- 修復了多個可能導致編輯器出現延遲的版面重排（layout thrashing）問題

### 移動編輯器命令
- 新增工作台命令，可將編輯器移至開頭和結尾位置

---

## 終端機

### Kitty 圖形協定支援
- 整合式終端機現在支援 Kitty 圖形協定
- 可直接在終端機中渲染高保真度的行內圖片

### 像素尺寸回報
- 終端機調整大小時支援像素尺寸回報

### Ghostty 支援
- macOS 和 Linux 上新增支援 Ghostty 作為外部終端機

### 終端機沙盒
- 終端機沙盒功能讓代理可以在受控環境中執行 Shell 命令

---

## 語言支援

### Shebang 語言偵測改進
- 改進了 Shebang 語言偵測支援，特別是針對使用 `/usr/bin/env` 搭配額外旗標的檔案
- 使用如 `#!/usr/bin/env -S deno -A` 的 Shebang 的檔案現在能正確被偵測為 TypeScript
- 這使得使用 Deno 或 Bun 等執行環境編寫的 TypeScript 腳本能獲得更好的語言支援，即使沒有 `.ts` 副檔名

---

## 除錯功能更新

除錯工具在本次版本中進行了改進和更新。

---

## 值得注意的修正

- 標準化導覽路徑切換選項標籤
- 「反轉行」在選取範圍為單行時套用至整份文件
- 新增工作台命令：將編輯器移至開頭和結尾
- 修復多個可能導致編輯器延遲的版面重排問題

---

## 可用性

VS Code 1.110 現已推出，Microsoft 提供了 Windows、macOS 和 Linux 三大平台的安裝程式。

---

## 詞彙對照表

以下為本文中出現的技術專有名詞對照表：

| 英文 | 繁體中文 |
|------|----------|
| Agent | 代理 |
| Agent Plugins | 代理外掛 |
| Agentic Browser Tools | 代理瀏覽器工具 |
| Session Memory | 工作階段記憶 |
| Context Compaction | 上下文壓縮 |
| Fork a Chat Session | 分叉聊天工作階段 |
| Agent Debug Panel | 代理除錯面板 |
| Sleep Prevention | 休眠防護 |
| Edit Mode | 編輯模式 |
| Agent Mode | 代理模式 |
| Auto-Approve | 自動核准 |
| MCP (Model Context Protocol) | 模型上下文協定 |
| Skills | 技能 |
| Commands | 命令 |
| Hooks | 掛鉤 |
| Slash Command | 斜線命令 |
| Extensions View | 擴展檢視 |
| Command Palette | 命令面板 |
| Screen Reader | 螢幕閱讀器 |
| Sandbox | 沙盒 |
| stdio transport | stdio 傳輸 |
| Breadcrumbs | 導覽路徑 |
| Workspace Trust | 工作區信任 |
| Kitty Graphics Protocol | Kitty 圖形協定 |
| Shebang | Shebang（腳本直譯器宣告） |
| Layout Thrashing | 版面重排 |
| Background Agent | 背景代理 |
| Prompt Files | 提示詞檔案 |
| Instruction Files | 指令檔案 |

---

*注意：由於原始網頁（code.visualstudio.com）在此環境中無法直接存取完整全文，本翻譯內容是根據官方更新頁面的搜尋摘要及多個相關報導來源整理而成。原始頁面中可能包含更多細節內容（如具體的設定名稱、截圖、GIF 動畫示範等）。強烈建議參閱[原文頁面](https://code.visualstudio.com/updates/v1_110)以獲取最完整的資訊。*

*翻譯來源：*
- [VS Code 官方更新頁面](https://code.visualstudio.com/updates/v1_110)
- [Visual Studio Magazine 報導](https://visualstudiomagazine.com/articles/2026/03/04/vs-code-1-110-ships-with-agent-plugins-browser-tools-and-session-memory.aspx)
- [VS Code Agent Plugins 文件](https://code.visualstudio.com/docs/copilot/customization/agent-plugins)
- [NTCompatible 報導](https://www.ntcompatible.com/story/visual-studio-code-1110-unlocking-aipowered-productivity-with-agents-and-browser-automation)
- [Heise Online 報導](https://www.heise.de/en/news/Visual-Studio-Code-1-110-gets-new-features-for-AI-agent-configuration-11200445.html)
- [AlternativeTo 報導](https://alternativeto.net/news/2026/3/visual-studio-code-1-110-enhanced-agent-plugins-and-persistent-session-memory/)
