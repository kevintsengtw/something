# Visual Studio Code 更新 — 2026 年 3 月（版本 1.111）

> 來源：[https://code.visualstudio.com/updates/v1_111](https://code.visualstudio.com/updates/v1_111)
> 發布日期：2026 年 3 月 9 日

---

## 更新概覽

歡迎來到 Visual Studio Code 2026 年 3 月的更新。**這是 VS Code 首個每週穩定版本（Weekly Stable Release）。**

在此之前，Microsoft 以每月一次的頻率更新 Visual Studio Code。Microsoft 傑出工程師、VS Code 工程團隊負責人 Kai Maetzel 表示，在精簡開發和交付流程之後，VS Code 將**每週發布一個新的穩定版本**。這代表了 VS Code 發布策略的重大轉變，旨在加速功能交付的節奏。

本次發布版本增強了代理體驗，包含以下主要功能：

- **代理權限**：為每個工作階段調整代理的自主等級
- **自動駕駛（預覽版）**：讓代理自主迭代直到完成任務
- **代理範圍掛鉤（預覽版）**：為代理附加前處理和後處理邏輯，而不影響其他聊天互動
- **代理除錯**：使用除錯事件快照排查代理行為和自訂設定

---

## GitHub Copilot

### 代理權限選擇器（Agent Permissions Picker）

聊天檢視中新增了一個權限選擇器，讓您決定給予代理多大的自主權。權限等級僅適用於當前工作階段，可隨時透過選擇不同的等級來變更。

#### 權限等級

**預設核准（Default Approvals）**

使用您已設定的核准設定。需要核准的工具會在執行前顯示確認對話框。這是最安全的模式，讓您完全掌控代理可以執行的每個操作。

**略過核准（Bypass Approvals）**

自動核准所有工具呼叫，不顯示確認對話框，並自動重試錯誤。此等級跳過了手動確認步驟，但代理仍會在遇到需要使用者輸入的問題時暫停。

**自動駕駛（Autopilot）**（預覽版）

自動核准所有工具呼叫，自動重試錯誤，自動回應問題，代理持續自主工作直到任務完成。這是最高自主等級，代理不會因等待回覆而停滯。

> **安全注意事項**：略過核准和自動駕駛會繞過手動核准提示，並忽略您已設定的核准設定，包括檔案編輯、終端機命令和外部工具呼叫等潛在破壞性操作。由於生成式 AI 的非確定性本質及其對提示注入的脆弱性，自動核准存在安全風險。讓代理透過 MCP（模型上下文協定）呼叫第三方工具會增加風險，因為它將代理的範圍擴展到程式碼編輯環境之外，且容易受到程式碼品質不佳的工具或工具投毒等攻擊的影響。

### 自動駕駛模式（Autopilot）（預覽版）

自動駕駛是本次更新最受關注的功能，也是 Copilot Chat 中的一個權限等級。在此模式下：

- 所有工具呼叫都會自動核准
- 錯誤會自動重試
- 工具提出的問題會自動回應，「使代理不會因等待回覆而停滯」
- 代理持續自主工作直到任務完成

**啟用方式**：在穩定版中，啟用 `chat.autopilot.enabled` 設定。

自動駕駛的概念類似於 Google 也在推動的「不需手動核准的代理式 AI 開發」方向。Microsoft 和 Google 都在鼓勵開發者讓 AI 代理更自主地運作，但這同時也帶來了安全性方面的考量。

### 代理範圍掛鉤（Agent-Scoped Hooks）（預覽版）

自訂代理的 frontmatter 現在支援代理範圍掛鉤。這些掛鉤**僅在您選擇特定代理或透過 `runSubagent` 調用該代理時才會執行**，不會影響其他聊天互動。

要建立代理範圍掛鉤，請在您的 `.agent.md` 檔案的 YAML frontmatter 的 `hooks` 區段中定義它：

```yaml
---
name: my-agent
description: 我的自訂代理
hooks:
  preToolCall:
    command: "echo '工具即將執行'"
  postToolCall:
    command: "echo '工具已完成執行'"
---

# 我的代理指令

這裡是代理的指令內容...
```

**啟用方式**：設定 `chat.useCustomAgentHooks` 為 `true`。

代理範圍掛鉤的主要用途：

- **前處理邏輯（preToolCall）**：在工具執行前進行驗證、日誌記錄或環境準備
- **後處理邏輯（postToolCall）**：在工具執行後進行清理、格式化或結果驗證
- **範圍隔離**：每個代理可以有自己的掛鉤，不會干擾其他代理或一般聊天

### 代理除錯：除錯事件快照（Debug Events Snapshot）

為了幫助您理解和排查代理行為，現在可以使用 `#debugEventsSnapshot` 將代理除錯事件的快照作為上下文附加到聊天中。

使用此功能可以：

- **查詢已載入的自訂設定**：了解哪些提示詞檔案、技能、掛鉤等正在作用
- **監控 Token 消耗**：追蹤代理在對話中消耗了多少 Token
- **排查代理行為**：理解代理為何做出某些決定或採取某些行動

使用方式：在聊天輸入中加入 `#debugEventsSnapshot`，代理會將當前除錯面板中的事件快照納入上下文進行分析。

### 聊天提示改進（Chat Tips）

聊天提示體驗經過重新設計，以便在您的聊天旅程中更好地在正確的時機展示相關提示：

- **結構化入門提示**：首次使用 VS Code 時，會顯示結構化的入門引導提示
- **生活品質提示**：入門提示完成後，會顯示進階提示，如實驗性設定的介紹
- **情境感知**：提示會根據您目前的使用情境顯示最相關的內容

---

## 擴充功能開發

### 在地化 IntelliSense（Localization IntelliSense）

VS Code 支援在擴充功能的 `package.json` 中使用在地化字串。本次迭代為這些在地化字串新增了基本的 IntelliSense 功能，使得與在地化字串的互動更加便利：

#### 前往定義（Go to Definition）

讓您跳轉到或預覽 `package.nls.json` 檔案中在地化字串的定義。當您在 `package.json` 中看到一個在地化字串的參考（例如 `%extensionDescription%`），可以使用「前往定義」直接跳到 `package.nls.json` 中對應的字串值。

#### 尋找所有參考（Find All References）

顯示在地化字串在 `package.json` 或 `package.nls.json` 中被引用的所有位置。這對於想要了解某個在地化字串在哪些地方被使用的擴充功能作者非常有用。

---

## 工程流程（Engineering）

隨著轉向每週穩定版本發布，VS Code 持續改進工程流程，以更快的步調交付高品質功能。

### 一鍵建立測試計畫項目

新增了從功能請求 Issue 一鍵建立測試計畫項目的體驗。這減少了為新功能設定結構化測試計畫所需的手動步驟。

### 自動產生驗證步驟

測試計畫項目會隨機分配給工程師，而清楚的驗證步驟對於高效且有效的測試至關重要。因此在相關 Issue 上新增了一個按鈕，可以自動產生驗證步驟，幫助確保 Issue 在關閉前有清楚、結構化的驗證步驟。

---

## 安全性考量

本次更新中關於自動駕駛和略過核准的功能引發了社群的安全性討論：

### 非確定性風險
生成式 AI 的非確定性本質意味著代理的行為無法完全預測。在自動核准模式下，代理可能執行非預期的操作。

### MCP 工具風險
讓代理透過 MCP 呼叫第三方工具會擴大代理的操作範圍，超出程式碼編輯環境。這帶來了額外的風險：

- **程式碼品質不佳的工具**：第三方 MCP 工具可能有漏洞或錯誤
- **工具投毒（Tool Poisoning）**：惡意工具可能被偽裝成合法工具
- **提示注入（Prompt Injection）**：外部內容可能包含操控代理行為的指令

### 建議
使用略過核准或自動駕駛模式時，建議：

- 在可控的環境中使用
- 定期檢查代理的操作記錄
- 謹慎選擇和審核 MCP 工具
- 考慮使用沙盒環境限制代理的存取範圍

---

## 可用性

VS Code 1.111 現已推出，Microsoft 提供了 Windows、macOS 和 Linux 三大平台的安裝程式。此後，VS Code 將以每週的頻率發布新的穩定版本。

---

## 詞彙對照表

| 英文 | 繁體中文 |
|------|----------|
| Weekly Stable Release | 每週穩定版本 |
| Agent Permissions | 代理權限 |
| Permissions Picker | 權限選擇器 |
| Default Approvals | 預設核准 |
| Bypass Approvals | 略過核准 |
| Autopilot | 自動駕駛 |
| Agent-Scoped Hooks | 代理範圍掛鉤 |
| Debug Events Snapshot | 除錯事件快照 |
| Chat Tips | 聊天提示 |
| Localization IntelliSense | 在地化智慧感知 |
| Go to Definition | 前往定義 |
| Find All References | 尋找所有參考 |
| YAML Frontmatter | YAML 前置資料 |
| Tool Calling | 工具呼叫 |
| Tool Poisoning | 工具投毒 |
| Prompt Injection | 提示注入 |
| Pre-processing | 前處理 |
| Post-processing | 後處理 |
| Token Consumption | Token 消耗 |
| Structured Onboarding | 結構化入門引導 |
| Test Plan Items | 測試計畫項目 |
| Verification Steps | 驗證步驟 |
| Feature Request Issues | 功能請求 Issue |
| Model Context Protocol (MCP) | 模型上下文協定 |
| Non-deterministic | 非確定性 |
| Sandbox | 沙盒 |

---

*注意：由於原始網頁（code.visualstudio.com）在此環境中無法直接存取完整全文，本翻譯內容是根據官方更新頁面的搜尋摘要、GitHub 上的原始 Markdown 檔案描述，以及多個相關報導來源整理而成。原始頁面中可能包含更多細節內容（如截圖、GIF 動畫示範、完整的程式碼範例等）。強烈建議參閱[原文頁面](https://code.visualstudio.com/updates/v1_111)以獲取最完整的資訊。*

*翻譯來源：*
- [VS Code 官方更新頁面](https://code.visualstudio.com/updates/v1_111)
- [GitHub 原始碼 - v1_111.md](https://github.com/microsoft/vscode-docs/blob/main/release-notes/v1_111.md)
- [The Register 報導](https://www.theregister.com/2026/03/11/visual_studio_code_moves_to/)
- [DevClass 報導](https://www.devclass.com/development/2026/03/12/microsoft-ships-vs-code-weekly-adds-autopilot-mode-so-ai-can-wreak-havoc-without-bothering-you/5208978)
- [Visual Studio Magazine 報導](https://visualstudiomagazine.com/articles/2026/03/11/vs-code-1-111-debuts-weekly-stable-cadence-expands-agent-controls.aspx)
- [Neowin 報導](https://www.neowin.net/news/microsoft-switches-to-weekly-releases-of-visual-studio-code-here-is-version-1111/)
