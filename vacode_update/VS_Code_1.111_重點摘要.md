# VS Code 2026 年 3 月更新（版本 1.111）— 重點摘要

> 發布日期：2026 年 3 月 9 日
> 來源：[官方更新頁面](https://code.visualstudio.com/updates/v1_111)

---

## 里程碑：轉向每週發布

版本 1.111 是 VS Code **首個每週穩定版本（Weekly Stable Release）**。Microsoft 傑出工程師 Kai Maetzel 表示，在精簡開發和交付流程後，VS Code 將從過去的每月發布改為**每週發布一個新的穩定版本**。這是 VS Code 發布策略的重大轉變，旨在加速功能交付。

---

## 核心主題

本次更新的所有新功能都與 **AI 代理體驗**相關，重點在於提升代理的自主性、可自訂性和可除錯性。

---

## 五大重點功能

### 1. 代理權限選擇器（Agent Permissions Picker）

聊天檢視中新增權限選擇器，讓您決定給予代理多大的自主權。權限等級僅適用於當前工作階段，可隨時切換：

| 權限等級 | 說明 |
|----------|------|
| **預設核准（Default Approvals）** | 使用您已設定的核准設定，需要核准的工具會顯示確認對話框 |
| **略過核准（Bypass Approvals）** | 自動核准所有工具呼叫，不顯示確認對話框，並自動重試錯誤 |
| **自動駕駛（Autopilot）**（預覽版） | 自動核准所有呼叫、自動重試錯誤、自動回應問題，代理持續自主工作直到任務完成 |

> ⚠️ **安全警告**：略過核准和自動駕駛會繞過手動核准提示，包括檔案編輯、終端機命令和外部工具呼叫等潛在破壞性操作。

### 2. 自動駕駛模式（Autopilot）（預覽版）

自動駕駛是本次更新最受關注的功能。啟用後代理將完全自主運作：

- 自動核准所有工具呼叫
- 錯誤時自動重試
- 自動回應工具提出的問題（代理不會因等待回覆而停滯）
- 持續工作直到任務完成

啟用方式：設定 `chat.autopilot.enabled` 為 `true`。

### 3. 代理範圍掛鉤（Agent-Scoped Hooks）（預覽版）

自訂代理的 frontmatter 現在支援代理範圍掛鉤，僅在選擇特定代理或透過 `runSubagent` 調用時才會執行，不影響其他聊天互動。

- 在 `.agent.md` 檔案的 YAML frontmatter 的 `hooks` 區段中定義
- 啟用方式：設定 `chat.useCustomAgentHooks` 為 `true`
- 支援前處理和後處理邏輯

### 4. 代理除錯事件快照（Debug Events Snapshot）

使用 `#debugEventsSnapshot` 可將代理除錯事件的快照作為上下文附加到聊天中，用於：

- 查詢已載入的自訂設定
- 監控 Token 消耗
- 排查代理行為問題

### 5. 在地化 IntelliSense（Localization IntelliSense）

擴充功能的 `package.json` 中在地化字串現在支援基本 IntelliSense 功能：

- **前往定義（Go to Definition）**：跳轉到或預覽 `package.nls.json` 檔案中在地化字串的定義
- **尋找所有參考（Find All References）**：顯示在地化字串在 `package.json` 或 `package.nls.json` 中被引用的所有位置

---

## 工程流程改進

- **一鍵建立測試計畫**：從功能請求 Issue 一鍵建立測試計畫項目，減少設定結構化測試計畫所需的手動步驟
- **自動產生驗證步驟**：在相關 Issue 上新增按鈕，可自動產生驗證步驟，確保 Issue 在關閉前有清楚的結構化驗證步驟
- **隨機分配測試**：測試計畫項目會隨機分配給工程師

---

## 聊天提示改進（Chat Tips）

聊天提示體驗重新設計：

- 首次使用 VS Code 時顯示結構化入門提示
- 入門提示完成後顯示生活品質提示（如實驗性設定）
- 更相關的提示在正確的時機出現

---

*本摘要根據 VS Code 官方更新頁面及多個相關報導整理翻譯。建議參閱[原文](https://code.visualstudio.com/updates/v1_111)以獲取最完整的資訊。*
