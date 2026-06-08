# Visual Studio Code 1.115 版本重點摘要

**版本：** 1.115
**發行日期：** 2026 年 4 月 8 日
**原文：** https://code.visualstudio.com/updates/v1_115

---

本次發行以全新的 VS Code Agents 伴隨應用程式為核心，讓 agent-native 開發體驗更上層樓。以下為官方列出的三大亮點：

## 一、Visual Studio Code Agents 應用程式（Preview）

- 全新的預覽版伴隨應用程式，與 VS Code Insiders 一同發行，專為 agent-native 開發打造
- **跨專案平行任務**：可在多個儲存庫中平行啟動 Agent 工作階段（各自隔離在獨立 worktree 中），快速切換上下文，並在人工與 Agent 審查間反覆迭代
- **監控與審查**：追蹤工作階段進度、內嵌檢視 diff、對 Agent 留下回饋，並可直接在應用程式中建立 Pull Request
- **自訂項目同步**：Custom instructions、prompt files、custom agents、MCP servers、hooks、plugins 以及主題等 VS Code 自訂項目皆可在 Agents 應用程式中使用
- **無需額外安裝**：隨 VS Code Insiders 一同發行，從開始功能表/應用程式資料夾啟動，或從命令面板執行 **Chat: Open Agents Application**

## 二、整合式瀏覽器改善

- **更具描述性的工具標籤**：Agent 呼叫瀏覽器工具時，工具呼叫顯示更描述性的標籤並附上目標瀏覽器分頁的連結
- **長時間運行腳本支援**：Run Playwright Code 工具改善了長時間運行腳本的支援，超過五秒的腳本會回傳延遲結果供 Agent 輪詢
- **減少重複分頁**：Agent 被更強烈抑制重複開啟分頁，同一主機已有分頁時不會再建新分頁
- **macOS 捏合縮放**：整合式瀏覽器在 macOS 上支援觸控板捏合縮放手勢，最多放大 3x，為純視覺放大不重排版面

## 三、終端機工具改善

- **向背景終端機送出輸入**：新增 `send_to_terminal` 工具，Agent 可與背景終端機互動（先前背景終端機為唯讀）。例如 SSH 工作階段逾時等待密碼時，Agent 仍可送出所需輸入
- **背景終端機通知（實驗性）**：啟用 `chat.tools.terminal.backgroundNotifications` 設定後，Agent 在背景終端機命令完成或需要輸入時自動收到通知，也適用於前景終端機逾時移至背景的情況

---

## 其他

- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）
- **重要修正**：終端機重啟整合式 pwsh 時游標位置錯誤、Caps Lock 鍵在 VS Code 終端機中的 Claude Code 插入原始跳脫序列

---

*資料來源：[Visual Studio Code 1.115 發行說明](https://code.visualstudio.com/updates/v1_115)*
