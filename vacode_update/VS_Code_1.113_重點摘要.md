# Visual Studio Code 1.113 版本重點摘要

**版本：** 1.113
**發行日期：** 2026 年 3 月 25 日
**原文：** https://code.visualstudio.com/updates/v1_113

---

本次發行包含跨 **Agent 與開發者體驗**的多項改善。以下為官方列出的六大亮點：

## 一、Chat Customizations 編輯器（Preview）

- 全新的集中式 UI，可在單一介面中建立和管理所有聊天自訂項目
- 以分頁方式組織不同的自訂類型：自訂指令、提示檔案、自訂 Agent、Agent 技能
- 提供內嵌程式碼編輯器，附帶語法醒目顯示和驗證
- 可從頭建立自訂項目，或使用 AI 根據專案生成初始內容
- 可直接從編輯器瀏覽 MCP 伺服器和 Agent 外掛程式市集
- 從 Chat 檢視中的齒輪圖示開啟，或從命令面板執行 **Chat: Open Chat Customizations**

## 二、模型選擇器中可配置的思考力度

- 支援推理的模型（如 Claude Sonnet 4.6、GPT-5.4）現在在模型選擇器中直接顯示 **Thinking Effort** 子選單
- 可控制模型對每次請求投入的推理程度，無需前往 VS Code 設定
- 模型選擇器標籤顯示目前的力度等級（如「GPT-5.3-Codex · Medium」）
- 先前的 `github.copilot.chat.anthropic.thinking.effort` 和 `github.copilot.chat.responsesApiReasoningEffort` 設定已棄用

## 三、巢狀子代理

- 子代理現在可以呼叫其他子代理，支援更複雜的多步驟工作流程
- 透過 `chat.subagents.allowInvocationsFromSubagents` 設定啟用

## 四、CLI Agent 能力

- Copilot CLI 和 Claude Agent 現在支援 MCP 伺服器，在 VS Code 中註冊的 MCP 伺服器會橋接至 CLI Agent
- Copilot CLI 和 Claude Agent 支援分叉工作階段（fork sessions），設定：`github.copilot.chat.cli.forkSessions.enabled`
- Agent Debug Log 面板現可用於 Copilot CLI 和 Claude Agent 工作階段
- Claude 工作階段列表改用官方 SDK API，解決先前解析 JSONL 檔案可能不同步的問題

## 五、聊天附件的圖片預覽

- 可選取任何圖片附件在全功能圖片檢視器中開啟
- 支援導航（箭頭按鈕、鍵盤方向鍵或縮圖列）、依對話輪次分組、縮放與平移
- 檔案總管右鍵選單中也可使用「Open in Images Preview」
- 相關設定：`imageCarousel.chat.enabled`、`imageCarousel.explorerContextMenu.enabled`

## 六、全新預設主題

- 新增「VS Code Light」和「VS Code Dark」預設主題，提供更現代的外觀
- 作業系統主題同步對新使用者預設使用新主題

---

## 其他

- **整合式瀏覽器自簽憑證支援**：可選擇暫時信任無法驗證的憑證，解除開發時的阻礙，信任有效期一週
- **改善的瀏覽器分頁管理**：新增 Quick Open Browser Tab 命令（⇧⌘A）、關閉所有瀏覽器分頁選項、標題列可配置按鈕（`workbench.browser.showInTitleBar`）
- **管理外掛程式市集**：新命令 **Chat: Manage Plugin Marketplaces**，可瀏覽、開啟目錄、移除市集
- **外掛程式安裝 URL 處理程式**：可透過 URL 觸發外掛程式市集安裝與擴充功能安裝
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）

---

*資料來源：[Visual Studio Code 1.113 發行說明](https://code.visualstudio.com/updates/v1_113)*
