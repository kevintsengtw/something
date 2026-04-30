# Visual Studio Code 1.117 版本重點摘要

**版本：** 1.117
**發行日期：** 2026 年 4 月 22 日
**原文：** https://code.visualstudio.com/updates/v1_117

---

本次發行為 Copilot Enterprise 與 Business 使用者新增功能，並進一步改善 VS Code 中的 Agent 體驗。以下為官方列出的三大亮點：

## 一、自帶金鑰（BYOK）支援 Copilot Business 與 Enterprise

- 讓 Copilot Business 與 Enterprise 使用者可以連接自己的 API 金鑰（支援 OpenRouter、Ollama、Google、OpenAI 等供應商），在 VS Code 聊天中直接使用這些模型
- 預設啟用，管理員可透過 GitHub.com 上 Copilot 政策設定中的 **Bring Your Own Language Model Key** 政策停用
- 管理員可控制哪些模型供應商可用於組織，同時讓開發者留在既有的工作流程中

## 二、聊天回應漸進式渲染（實驗性）

- 聊天回應以區塊為單位（block-by-block）串流渲染，附帶可選動畫，取代預設的計時器式渲染
- 降低較長回應的感知等待時間
- 三個相關設定：
  - `chat.experimental.incrementalRendering.enabled`：啟用／停用（預設 `true`）
  - `chat.experimental.incrementalRendering.animationStyle`：動畫風格（`none`、`fade`、`rise`、`blur`、`scale`、`slide`、`reveal`，預設 `fade`）
  - `chat.experimental.incrementalRendering.buffering`：渲染前的緩衝方式（`off`、`word`、`paragraph`，預設 `word`）

## 三、終端機改善

- **Copilot CLI 可從自訂終端機設定檔啟動**：即使預設終端機設為非預設 Shell（如 macOS/Linux 上的 `fish`、Windows 上的 Git Bash），現在也可以從終端機面板啟動 Copilot CLI
- **Agent CLI 終端機標題**：Copilot CLI、Claude Code、Gemini CLI 被偵測為獨立的 Shell 類型，終端機標題會顯示 CLI 發出的 OSC 標題序列，而非通用的 `node`。可透過 `terminal.integrated.tabs.allowAgentCliTitle` 設定切換

---

## 其他重要更新

- **Agent Sessions 排序**：Agent Sessions 檢視支援依建立時間或最後更新時間排序
- **背景終端機命令的系統通知**：Agent 在背景執行的長時間終端機命令，現以系統通知方式呈現在聊天回應中
- **VS Code Agents 應用程式（Insiders）**：新增子工作階段（sub-session）、改善行內變更渲染、更新流程改善、主題與 UX 持續精進
- **TypeScript 6.0.3**：包含修復數個匯入錯誤與回歸的修復版本
- **切換至主視窗**：支援從輔助視窗切換回主視窗
- **package.json 相依性懸停**：同時顯示已安裝版本與最新發佈版本

---

*資料來源：[Visual Studio Code 1.117 發行說明](https://code.visualstudio.com/updates/v1_117)*
