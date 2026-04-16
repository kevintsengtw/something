# Visual Studio Code 1.116 版本重點摘要

**版本：** 1.116（2026 年 4 月發行）
**類型：** 每週穩定版（Weekly Stable Release）
**主題：** GitHub Copilot Chat 內建化、Agent 偵錯與終端機互動強化、Agents 應用程式無障礙鍵盤導航

---

## 一、GitHub Copilot Chat 內建為 VS Code 預設擴充功能

- VS Code 1.116 移除了 Copilot 擴充功能的安裝需求，**GitHub Copilot Chat 現以內建擴充功能（built-in extension）** 形式直接隨 VS Code 一起發行
- 使用者**無需任何安裝步驟** 即可使用 Chat、Inline Suggestions（行內建議）、以及 Agents 等 AI 功能
- 已安裝 Copilot 擴充功能的現有使用者**功能不變**
- 不希望使用 AI 功能的使用者，可使用 `chat.disableAIFeatures` 設定**全域停用** 所有 AI 功能

## 二、Agent Debug Logs（Agent 偵錯紀錄）

- 全新的 **Agent Debug Log** 面板顯示聊天工作階段中 Agent 互動的時序事件紀錄（chronological event log）
- 對於了解送出提示後實際發生什麼事、以及偵錯 Chat 自訂項目特別有幫助
- 紀錄會**持久化儲存於本地磁碟**，工作階段結束後仍可檢視，方便排查持續性問題
- 重度使用者可透過此功能追蹤失敗的 Agent 工作階段

## 三、Foreground 終端機 Agent 工具支援

- `send_to_terminal` 與 `get_terminal_output` 兩個 Agent 工具，**現在也支援前景終端機（foreground terminals）**
- 先前僅支援由 Agent 建立的背景終端機
- Agent 現在可以讀取終端機面板中任何可見終端機的輸出（例如執行中的 REPL 或互動式腳本），並向其送出輸入
- 終端機通知預設啟用，不再需要手動輪詢結果

## 四、Agents 應用程式 — 無障礙與鍵盤導航強化

- 為 Agents 應用程式新增專屬命令與鍵盤快捷鍵，可聚焦至：
  - **Changes view（變更檢視）**
  - **Changes view 內的檔案樹（files tree）**
  - **Chat Customizations view（聊天自訂項目檢視）**
- 啟用完整鍵盤導航
- 為 Agents 應用程式聊天輸入框新增無障礙說明對話框（`⌥F1` / Windows `Alt+F1` / Linux `Shift+Alt+F1`），列出可用命令與鍵盤快捷鍵，並提供調整朗讀詳細度的選項，便於螢幕閱讀器使用者操作

## 五、Agents 應用程式 — `#` 觸發的檔案內容自動完成

- Agents 應用程式新增 `#` 觸發的檔案上下文（file-context）自動完成功能
- 範圍會限定在 picker 選取的工作區（workspace）內
- 讓您能夠快速以 `#檔名` 的方式將檔案內容加入 Agent 對話脈絡

## 六、CSS `@import` 解析至 `node_modules`

- VS Code 1.116 支援 CSS `@import` 解析至 `node_modules`
- 例如：`@import "some-module/style.css"` 現在可以使用 `Ctrl+click` 直接跳轉到對應模組檔案
- 適用於使用 bundler 的專案

## 七、Edit Mode 棄用提醒

- Edit Mode 自 VS Code 1.110 起已正式棄用（deprecated）
- 將支援至 v1.125，之後 Edit Mode 將被**完全移除**，且無法再透過設定啟用
- 老使用者需要開始調整工作流程，新環境則可直接享受更整合的體驗

---

*資料來源：VS Code 1.116 發行說明 (https://code.visualstudio.com/updates/v1_116)*
