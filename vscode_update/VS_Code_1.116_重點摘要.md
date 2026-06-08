# Visual Studio Code 1.116 版本重點摘要

**版本：** 1.116
**發行日期：** 2026 年 4 月 15 日
**原文：** https://code.visualstudio.com/updates/v1_116

---

本次發行持續強化聊天與 Agent 的功能與效率。以下為官方列出的四大亮點：

## 一、Agent Debug Logs — 偵錯先前的 Agent 工作階段

- Agent Debug Log 面板顯示聊天工作階段期間 Agent 互動的時序事件紀錄
- 現可檢視目前工作階段及**先前工作階段**的紀錄，紀錄持久化儲存於本地磁碟
- 工作階段結束後仍可回顧與偵錯
- 設定：`github.copilot.chat.agentDebugLog.fileLogging.enabled`

## 二、Copilot CLI 思考力度（Thinking Effort）

- 可在 Copilot CLI 工作階段中透過語言模型選擇器配置推理模型的思考力度
- 思考力度控制模型對每次請求投入多少推理，可平衡回應品質與延遲
- 非推理模型不顯示子選單

## 三、終端機 Agent 工具

- `send_to_terminal` 與 `get_terminal_output` 現支援**前景終端機**，不再僅限於 Agent 建立的背景終端機
- Agent 可讀取任何可見終端機的輸出並向其送出輸入（如執行中的 REPL 或互動式腳本）
- 多項終端機輸入改善：移除 LLM 式輸入偵測（降低延遲與 Token 消耗）、進度訊息顯示正在回答的問題、Focus Terminal 按鈕
- 背景終端機通知預設啟用（`chat.tools.terminal.backgroundNotifications`）

## 四、GitHub Copilot 成為內建擴充功能

- GitHub Copilot Chat 現為 VS Code 內建擴充功能，新使用者無需安裝即可使用 Chat、Inline Suggestions 與 Agents
- 既有使用者不受影響
- 可用 `chat.disableAIFeatures` 停用 AI 功能

---

## 其他重要更新

- **自訂項目歡迎頁面**：Chat Customizations 對話框新增歡迎頁面，提供所有 Agent 自訂項目概覽，並可透過自然語言描述讓 VS Code 起草 agents、skills、instructions
- **工具確認輪播（實驗性）**：以輪播控制項依序審查與核准多個工具呼叫（`chat.tools.confirmationCarousel.enabled`）
- **VS Code Agents 應用程式（Insiders）**：新增推理等級選擇、Plan 模式自動啟動、Files 分頁預設顯示、應用程式更名為 Visual Studio Code Agents - Insiders
- **Chat UX 改善**：Diff 直接在聊天對話中渲染、渲染效能提升、聊天送出效能修正、子代理進度視覺區分
- **Agents 應用程式無障礙功能**：無障礙說明對話框（Alt+F1）、鍵盤導航命令（Focus Changes View / Chat Customizations View / Files Explorer View）、ARIA 標籤與地標
- **螢幕閱讀器鍵盤快捷鍵搜尋說明**
- **整合式瀏覽器新入口**：View 選單與鍵盤快捷鍵
- **JS/TS Chat Features 擴充功能（Preview）**：增強 Copilot 處理 TypeScript/JavaScript 的能力（`jsts-chat-features.skills.enabled`）
- **企業群組政策 — Agent 網路存取過濾**：管理員可控制 Agent 工具可存取的網域（`ChatAgentNetworkFilter`、`ChatAgentAllowedNetworkDomains`、`ChatAgentDeniedNetworkDomains`）
- **GitHub Pull Requests 擴充功能**：新增建立 PR 的聊天工具、Worktree 可從「Delete Local Branches and Remotes」命令刪除
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）

---

*資料來源：[Visual Studio Code 1.116 發行說明](https://code.visualstudio.com/updates/v1_116)*
