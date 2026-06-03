# Visual Studio Code 1.111 版本重點摘要

**版本：** 1.111
**發行日期：** 2026 年 3 月 9 日
**原文：** https://code.visualstudio.com/updates/v1_111

---

本版本是 **VS Code 首個每週穩定版發行**，進一步強化 Agent 體驗。以下為官方列出的四大亮點：

## 一、Agent 權限

- 聊天檢視中新增權限選擇器，可控制 Agent 的自主程度
- 權限等級僅適用於目前工作階段，可隨時變更
- 三種等級：**Default Approvals**（使用已設定的核准設定）、**Bypass Approvals**（自動核准所有工具呼叫並自動重試錯誤）、**Autopilot**（自動核准、自動回應問題、持續自主工作直到任務完成）
- Bypass Approvals 和 Autopilot 會略過手動核准提示，包括具潛在破壞性的操作（檔案編輯、終端機命令、外部工具呼叫），首次啟用時會顯示警告對話方塊

## 二、Autopilot（Preview）

- 在 Insiders 中預設啟用，Stable 版可透過 `chat.autopilot.enabled` 啟用
- Agent 持續控制並反覆迭代，直到呼叫 `task_complete` 工具表示完成

## 三、Agent 範圍的掛鉤（Preview）

- 自訂 Agent 的 YAML 前置資料現在支援 Agent 範圍的掛鉤
- 僅在選取特定 Agent 或透過 `runSubagent` 呼叫時執行
- 可為特定 Agent 附加前處理和後處理邏輯，不影響其他聊天互動
- 透過 `chat.useCustomAgentHooks` 設定啟用

## 四、Agent 疑難排解

- 可透過 `#debugEventsSnapshot` 將 Agent 偵錯事件快照作為上下文附加至聊天
- 用於詢問已載入的自訂項目、Token 消耗，或疑難排解 Agent 行為
- 也可從 Agent Debug 面板右上角的聊天圖示新增快照附件

---

## 其他

- **聊天提示改善**：重新設計的聊天提示體驗，引導使用者經歷結構化的入門旅程，新增 `/init` 和 `/fork` 斜線命令提示
- **AI CLI 設定檔群組（實驗性）**：AI CLI 終端機設定檔顯示在終端機設定檔下拉選單頂部的專用群組中（`terminal.integrated.experimental.aiProfileGrouping`）
- **擴充功能 package.json 本地化字串的基本 IntelliSense**：支援 Go to Definition 和 Find all References
- **工程改善**：每週穩定版發行流程改善，包括一鍵建立測試計畫項目、自動生成驗證步驟、PR 媒體自動附加至關聯的 Issue、Chat Showcase 自動化流水線
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）

---

*資料來源：[Visual Studio Code 1.111 發行說明](https://code.visualstudio.com/updates/v1_111)*
