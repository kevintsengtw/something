# Visual Studio Code 更新 — 2026 年 1 月（版本 1.109）

> 來源：[https://code.visualstudio.com/updates/v1_109](https://code.visualstudio.com/updates/v1_109)
> 發布日期：2026 年 2 月 4 日

---

## 更新概覽

歡迎來到 Visual Studio Code 2026 年 1 月的更新。本次發布版本進一步將 VS Code 發展為**多代理開發（Multi-Agent Development）的大本營**。

本次更新包含三大主軸：

1. **聊天 UX 改進**：更快的串流速度、改進的推理結果展示、以及重新設計的編輯器行內聊天
2. **代理工作階段管理**：更容易在本地、背景和雲端之間委派任務給代理，並在需要時介入
3. **代理自訂**：使用代理協調建立自己的工作流程，透過代理技能和組織層級自訂獲得一致的結果

---

## GitHub Copilot — 聊天 UX 改進

### 串流改進與思考過程可視化

本次更新帶來了顯著的串流改進，回應速度明顯更快。如果您使用 Anthropic Claude 模型進行聊天和程式碼協助，現在可以**即時查看模型的推理過程**。

思考過程的顯示方式可透過 `chat.thinking.style` 設定進行調整：

- **詳細模式（Detailed）**：完整顯示模型的推理步驟
- **精簡模式（Compact）**：以摘要形式顯示推理過程

在推理過程中，工具呼叫也會同步顯示，讓您完全掌握模型正在做什麼。

### 重新設計的行內聊天（Inline Chat）

行內聊天經過重新設計，以更加不干擾的方式運作。當 AI 在背景工作時，您可以保持在程式碼流程中，不會被中斷。新的設計讓行內聊天「不礙事」，同時提供更好的進度可見性。

### 排隊傳送後續訊息

在處理較長的任務時，您現在可以在**請求仍在進行中時傳送後續訊息**。以前您必須等待回應完成或取消它。現在：

- 當請求進行中，「傳送」按鈕會變為下拉選單
- 選項包括「加入佇列（Add to Queue）」，讓您的訊息在當前回應完成後自動傳送
- 這讓您可以預先排列多個問題或指令

### 互動式 Mermaid 圖表

聊天回應現在可以使用 `renderMermaidDiagram` 工具渲染互動式 Mermaid 圖表。模型可以使用流程圖、序列圖和其他視覺化方式來**視覺化分解複雜概念**。

圖表具有互動性：

- 可平移和縮放以探索細節
- 可在全尺寸編輯器中開啟以便於檢視
- 支援多種圖表類型（流程圖、序列圖、甘特圖等）

---

## GitHub Copilot — 代理工作階段管理

### 代理工作階段檢視（Agent Sessions View）

VS Code 側邊欄中新增了一個名為「Agent Sessions」的全新檢視。它提供了一個**統一的地方來管理您所有的代理**，無論它們是在本地還是在雲端運行。

功能包括：

- 查看哪些代理正在運行及其狀態
- 一鍵在不同工作階段之間切換
- 統一管理本地、背景和雲端代理

### 工作階段類型選擇器（Session Type Picker）

全新的工作階段類型選擇器允許無縫切換。例如，您可以在本地規劃功能，然後將繁重的實作委派給雲端代理。

### 本地代理與雲端代理

- **本地代理**：當您需要快速、互動式的幫助時使用
- **雲端代理**：用於非同步委派較長時間運行的任務
- 透過 1.109，您可以在本地或雲端運行 Claude 和 Codex 代理，全部使用同一個 GitHub Copilot 訂閱

### 背景代理改進

背景代理在隔離的工作區中運行，不會干擾您的活動工作，同時支援多個背景任務並行。

版本 1.109.5 的更新新增了：

- 支援斜線命令（包括提示詞檔案、掛鉤和技能）
- 重新命名背景代理工作階段的功能

### 子代理（Subagents）與 runSubagent 工具

新增 `runSubagent` 工具幫助您管理上下文。子代理的特點：

- **獨立運行**：與主要聊天分開，擁有自己的上下文
- **使用方式**：在提示詞中加入 `#runSubagent` 工具來呼叫
- **結果回傳**：完成後僅將最終結果回傳至主要聊天，只有該結果加入主要上下文
- **並行執行**：子代理現在可以並行運行，顯著加速可拆分為獨立任務的工作

---

## GitHub Copilot — 代理自訂

### 代理技能（Agent Skills）（正式可用）

代理技能現已**正式可用並預設啟用**。技能是專門的功能，幫助代理產生高品質的輸出，為測試策略、API 設計或效能最佳化等領域提供經過驗證的指令。

主要特點：

- 將領域專業知識打包為可重複使用的工作流程
- 作為斜線命令在聊天中使用，與提示詞檔案並列
- 組織層級指令確保一致性
- 新的控制機制提供細粒度的代理調用控制

### 代理協調（Agent Orchestrations）

VS Code 已確定將代理協調視為**一級架構模式**的構建模組，結合：

- 自訂代理
- 子代理
- 細粒度調用控制

### 工作區初始化（/init 命令）

當您執行 `/init` 時，代理會：

1. **探索現有 AI 慣例**：發現工作區中的 `copilot-instructions.md`、`AGENTS.md` 等檔案
2. **分析專案結構**：了解您的專案架構和編碼模式
3. **產生工作區指令**：生成量身定制的綜合工作區指令

`/init` 命令以貢獻的提示詞檔案（contributed prompt file）形式實作，因此您可以透過修改底層提示詞來自訂其行為。

### 組織層級自訂

- **組織層級指令**：確保整個組織的一致性
- **跨工具設定共享**：VS Code 直接讀取 Claude 設定檔案，使您的代理、技能、指令和掛鉤可在不同工具之間無需重複設定

---

## 第三方代理整合

### Claude 代理支援（公開預覽版）

VS Code 1.109 新增 Claude 代理支援，讓開發者可以直接利用 Anthropic 的官方 Agent SDK。

主要特點：

- 使用 Anthropic 官方的 Claude Agent 框架
- 獲得與其他 Claude 實作相同的提示詞、工具和整體架構
- 使用 GitHub Copilot 訂閱中的 Claude 模型
- 可在本地或雲端運行

### OpenAI Codex 支援

本次更新也歡迎 OpenAI Codex 加入。Microsoft 表示正在努力將更多代理引入 Copilot+ 訂閱。

---

## 代理掛鉤（Agent Hooks）（預覽版）

掛鉤讓您在代理工作階段的關鍵生命週期點執行自訂 Shell 命令。與指引代理行為的指令或自訂提示詞不同，掛鉤以**確定性方式運行您的程式碼，保證結果**。

### 八種掛鉤事件

| 掛鉤事件 | 說明 |
|----------|------|
| `PreToolUse` | 工具執行前觸發 |
| `PostToolUse` | 工具執行後觸發 |
| `SessionStart` | 工作階段開始時觸發 |
| `SessionStop` | 工作階段結束時觸發 |
| `SubagentStart` | 子代理啟動時觸發 |
| `SubagentStop` | 子代理結束時觸發 |
| `PreRequest` | 請求發送前觸發 |
| `PostRequest` | 請求完成後觸發 |

### 使用場景

- **執行安全政策**：確保代理操作符合安全規範
- **自動化程式碼品質檢查**：在工具呼叫後自動執行 Lint 或測試
- **建立稽核軌跡**：記錄代理的所有操作
- **注入專案特定上下文**：在工作階段開始時提供額外的專案資訊

### 跨工具相容性

VS Code 使用與 Claude Code 和 Copilot CLI 相同的掛鉤格式，因此您可以在不同工具之間重用現有的掛鉤設定。

---

## 安全性增強

### 終端機沙盒（Terminal Sandbox）（實驗性）

終端機沙盒功能限制代理執行命令的檔案和網路存取：

- **檔案系統限制**：僅允許存取您的工作區資料夾
- **網路限制**：僅允許存取可信的網域
- **平台支援**：目前為實驗性功能，支援 macOS 和 Linux
- 固定的封鎖清單（blocklist）機制

### 自動核准規則（Auto-Approval Rules）

有效的自動核准規則改善了代理驅動操作的安全性和控制：

- 對安全操作跳過確認提示
- 減少不必要的手動確認
- 同時維持對潛在危險操作的控制

---

## MCP 應用程式（MCP Apps）

VS Code 新增了 MCP Apps 支援，為 MCP 伺服器提供在客戶端中顯示豐富互動式 UI 的能力。

工具呼叫現在可以回傳互動式 UI 元件，直接在對話中渲染：

- **儀表板（Dashboards）**：即時資料視覺化面板
- **表單（Forms）**：互動式輸入表單
- **視覺化圖表（Visualizations）**：圖表和圖形
- **多步驟工作流程**：引導式多步驟互動

這使得 Copilot 在 VS Code 內部能提供更多工具驅動的互動式體驗。

---

## 工作台（Workbench）

### 實驗性主題

新增兩個實驗性主題，仍在積極開發中：

- **VS Code Light**：全新的淺色主題
- **VS Code Dark**：全新的深色主題

這些主題旨在提供更現代化的視覺體驗，但目前仍在開發階段，可能會在未來版本中持續調整。

---

## 編輯器

### 括號比對顏色自訂

開發者現在可以使用新的 `editorBracketMatch.foreground` 色彩主題 Token 來自訂比對括號的文字顏色。這讓您可以根據自己的偏好來設定括號比對的視覺樣式。

---

## 終端機

### Kitty 鍵盤協定支援

終端機新增 Kitty 鍵盤協定支援，改善鍵盤輸入的處理方式。

### win32-input-mode 支援

新增 win32-input-mode 支援，改善 Windows 平台上的終端機輸入體驗。

### SGR 跳脫序列

新增 SGR（Select Graphic Rendition）跳脫序列支援，改善文字格式化功能，如粗體、斜體、底線等。

### 終端機沙盒

實驗性的終端機沙盒功能限制代理執行命令時的檔案和網路存取。詳見上方「安全性增強」章節。

---

## 更新歷程（Recovery Releases）

VS Code 1.109 經歷了多次修復更新：

| 版本 | 發布說明 |
|------|----------|
| **1.109.0** | 初始發布（2026 年 2 月 4 日） |
| **1.109.1** | Recovery 1 — 修正問題 |
| **1.109.2** | Recovery 2 — 修正問題 |
| **1.109.3+** | 新增代理掛鉤（Agent Hooks）功能 |
| **1.109.5+** | 背景代理改進：支援斜線命令（提示詞檔案、掛鉤、技能）、重新命名背景代理工作階段 |

---

## 相關部落格文章

本次更新伴隨了多篇重要的部落格文章：

- **[Your Home for Multi-Agent Development](https://code.visualstudio.com/blogs/2026/02/05/multi-agent-development)**：詳細介紹 VS Code 如何成為多代理開發的大本營
- **[Giving Agents a Visual Voice: MCP Apps Support in VS Code](https://code.visualstudio.com/blogs/2026/01/26/mcp-apps-support)**：介紹 MCP Apps 如何讓代理擁有視覺化表達能力
- **[GitHub Copilot in VS Code v1.109 - January Release](https://github.blog/changelog/2026-02-04-github-copilot-in-visual-studio-code-v1-109-january-release/)**：GitHub 官方更新日誌

---

## 詞彙對照表

| 英文 | 繁體中文 |
|------|----------|
| Multi-Agent Development | 多代理開發 |
| Agent Sessions View | 代理工作階段檢視 |
| Session Type Picker | 工作階段類型選擇器 |
| Local Agent | 本地代理 |
| Cloud Agent | 雲端代理 |
| Background Agent | 背景代理 |
| Subagent | 子代理 |
| Agent Skills | 代理技能 |
| Agent Orchestrations | 代理協調 |
| Agent Hooks | 代理掛鉤 |
| Inline Chat | 行內聊天 |
| Streaming | 串流 |
| Thinking Tokens | 思考 Token |
| Mermaid Diagrams | Mermaid 圖表 |
| Terminal Sandbox | 終端機沙盒 |
| Auto-Approval Rules | 自動核准規則 |
| MCP Apps | MCP 應用程式 |
| Workspace Priming | 工作區初始化 |
| Prompt Files | 提示詞檔案 |
| YAML Frontmatter | YAML 前置資料 |
| Blocklist | 封鎖清單 |
| Bracket Match | 括號比對 |
| SGR (Select Graphic Rendition) | SGR 跳脫序列 |
| Kitty Keyboard Protocol | Kitty 鍵盤協定 |
| Recovery Release | 修復版本 |
| Audit Trail | 稽核軌跡 |
| Deterministic | 確定性 |
| Follow-up Messages | 後續訊息 |
| Add to Queue | 加入佇列 |
| Organization-wide Customizations | 組織層級自訂 |
| Contributed Prompt File | 貢獻的提示詞檔案 |
| Fine-grained Control | 細粒度控制 |
| Claude Agent SDK | Claude 代理 SDK |

---

*注意：由於原始網頁（code.visualstudio.com）在此環境中無法直接存取完整全文，本翻譯內容是根據官方更新頁面的搜尋摘要、GitHub Changelog、以及多個相關報導來源整理而成。原始頁面中可能包含更多細節內容（如截圖、GIF 動畫示範、完整的程式碼範例、設定 JSON 片段等）。強烈建議參閱[原文頁面](https://code.visualstudio.com/updates/v1_109)以獲取最完整的資訊。*

*翻譯來源：*
- [VS Code 官方更新頁面](https://code.visualstudio.com/updates/v1_109)
- [GitHub Changelog](https://github.blog/changelog/2026-02-04-github-copilot-in-visual-studio-code-v1-109-january-release/)
- [VS Code 多代理開發部落格](https://code.visualstudio.com/blogs/2026/02/05/multi-agent-development)
- [Visual Studio Magazine 報導](https://visualstudiomagazine.com/articles/2026/02/05/vs-code-1-109-deemed-multi-agent-development-platform.aspx)
- [How-To Geek 報導](https://www.howtogeek.com/visual-studio-code-has-new-experimental-themes-and-more-ai-coding-features/)
- [Heise Online 報導](https://www.heise.de/en/news/Visual-Studio-Code-1-109-Watch-AI-Models-Think-11166499.html)
- [InfoWorld 報導](https://www.infoworld.com/article/4128274/visual-studio-code-update-shines-on-coding-agents.html)
- [UbuntuHandbook 報導](https://ubuntuhandbook.org/index.php/2026/02/vs-code-1-109-released-claude-agent/)
