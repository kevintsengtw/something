# Visual Studio Code 1.119 版本重點摘要

**版本：** 1.119
**發行日期：** 2026 年 5 月 6 日
**原文：** https://code.visualstudio.com/updates/v1_119

---

本次發行聚焦於**更流暢的 Agent 互動、增強的可觀測性，以及更高效的信任與安全控制**。以下為官方列出的五大亮點：

## 一、與 Agent 共享瀏覽器分頁

- Agent 不會自動取得整合式瀏覽器的存取權，需明確共享瀏覽器頁面
- 新增多種共享方式：透過建議的上下文、上下文選擇器、拖放操作將瀏覽器分頁附加至聊天
- 分頁附加後進入共享狀態，Agent 可讀取並互動該頁面；可透過瀏覽器中的共享按鈕停止共享
- Agent 可得知您有多少未共享的開啟分頁，並可在需要時請求共享，您可核准或拒絕
- 當 Agent 嘗試在同一網域開啟新分頁時，會提示是否重用既有分頁以減少雜亂

## 二、最佳化 Token 使用量（實驗性）

- 設定：`github.copilot.chat.agent.backgroundTodoAgent.enabled`
- 將 Todo 清單管理卸載至輕量背景 Agent，主模型不再擁有 todo 工具，節省 Token 成本
- 背景 Agent 監視主 Agent 活動並同步更新 Todo 清單
- 若手動新增 todo 工具（如 `#todo`）或自訂 Agent 指定該工具，背景 Agent 不會執行

## 三、OpenTelemetry 追蹤 Agent 工作階段

- 設定：`github.copilot.chat.otel.enabled`、`github.copilot.chat.otel.otlpEndpoint`
- Copilot Chat Agent 工作階段（本機 Agent、Copilot CLI 背景 Agent、Claude Agent）現在發出 OpenTelemetry 追蹤、指標和事件
- 遵循 GenAI 語義慣例，可在任何 OTLP 相容後端監控 Agent 行為、延遲和 Token 使用
- 每個使用者請求產生 `invoke_agent` 根 span，包含巢狀的 `chat`、`execute_tool`、`execute_hook` 子 span
- 子 Agent 呼叫自動掛載在呼叫 Agent 的 `execute_tool` span 下，提供單一連結追蹤的完整可見性

## 四、信任與安全

- **允許 Agent 沙箱中的網路存取**：`chat.agent.sandbox.enabled` 新增 `allowNetwork` 模式，保留檔案系統限制但移除網路封鎖，減少網路存取中斷
- **自動核准對暫存資料夾的寫入**：啟用「Allow All Commands in Session」時，對作業系統暫存資料夾的寫入（`/tmp`、`%TEMP%`）不再需要核准，其他工作區外位置仍需確認

## 五、Markdown 預覽切換

- 新增工具列按鈕和命令，可快速在 Markdown 原始碼和預覽之間切換
- **Markdown: Switch to Preview View** 和 **Switch to Editor View** 按鈕/命令更容易發現
- Markdown 設定已重新組織，相關預覽設定歸類在「Preview」子區段

---

## 其他

- **Visual Studio Code Agents（Insiders）**：重新設計的新工作階段 Repo 選擇器、子工作階段改進、Web 和行動裝置改善、進度 UX 改善、開發者樂趣彩蛋
- **顯示 Copilot CLI 和 Claude Agent 回應的模型詳情**：聊天回應顯示模型名稱和計費倍率徽章（`github.copilot.chat.agent.modelDetails.enabled`），Auto 模式顯示實際使用的模型
- **用量計費更新**：GitHub Copilot 自 6 月 1 日起過渡至用量計費，內部 UI 變更已準備就緒但尚未對使用者可見
- **Markdown 設定重新組織**：內建 Markdown 支援的設定在設定編輯器中新增基本分組
- **工程**：Webview 完成遷移至 CSS 錨點定位（改善效能）、TypeScript 7 用於所有型別檢查（Copilot 擴充功能型別檢查從 22 秒降至 4 秒）
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）

---

*資料來源：[Visual Studio Code 1.119 發行說明](https://code.visualstudio.com/updates/v1_119)*
