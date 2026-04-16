# Visual Studio Code 1.116 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.116
**發行日期：** 2026 年 4 月
**原文：** https://code.visualstudio.com/updates/v1_116

---

歡迎使用 Visual Studio Code 1.116 版本。本次發行涵蓋多項與 GitHub Copilot 整合相關的重大改進、Agent 偵錯能力強化、以及 Agents 應用程式的無障礙與鍵盤導航完善。

以下是本次發行的主要亮點：

- **GitHub Copilot Chat 內建化**：移除 Copilot 擴充功能安裝需求，Chat、Inline Suggestions 與 Agents 開箱即用
- **Agent Debug Logs**：新增 Agent 偵錯紀錄面板，可追蹤失敗的工作階段
- **Foreground 終端機支援**：`send_to_terminal` 與 `get_terminal_output` Agent 工具現在亦支援前景終端機
- **Agents 應用程式無障礙鍵盤導航**：新增聚焦 Changes view、檔案樹、Chat Customizations view 的快捷鍵
- **無障礙說明對話框**：Agents 應用程式聊天輸入框新增無障礙說明（`⌥F1`）
- **`#` 檔案上下文自動完成**：Agents 應用程式支援 `#` 觸發、限定於 picker 選取工作區的檔案完成
- **CSS `@import` `node_modules` 解析**：支援 Ctrl+click 跳轉至 node_modules 中的 CSS 檔案

---

## GitHub Copilot

### Copilot Chat 成為內建擴充功能

VS Code 1.116 移除了對 Copilot 擴充功能的安裝需求，直接將 **GitHub Copilot Chat** 以內建擴充功能（built-in extension）的形式隨 VS Code 一同發行。這意味著：

- 全新安裝的使用者**無需任何設定步驟**，即可立即使用 Chat、行內建議（Inline Suggestions）、以及 Agents 等 AI 功能
- **既有已安裝 Copilot 擴充功能的使用者**，看到的功能與以往完全相同，不受影響
- 不希望使用 AI 功能的使用者，可使用 `chat.disableAIFeatures` 設定**全域停用** 所有 AI 功能

此變更是 VS Code 將 AI 功能納入核心體驗的重要一步，降低新使用者的進入門檻。

### Agent Debug Logs（Agent 偵錯紀錄）

全新的 **Agent Debug Log** 面板會顯示聊天工作階段期間 Agent 互動的**時序事件紀錄（chronological event log）**。此功能對於以下用途特別有幫助：

- 了解使用者送出提示後實際發生什麼事
- 偵錯 Chat 自訂項目（chat customizations）
- 追蹤失敗的 Agent 工作階段

紀錄會**持久化儲存於本地磁碟**，使用者可以在工作階段結束後仍然檢視過去的紀錄，這對於排查持續性問題（persistent issues）至關重要。對於需要深入了解 Agent 行為的重度使用者，此功能提供了透明度與可觀察性。

### Foreground 終端機 Agent 工具支援

`send_to_terminal` 和 `get_terminal_output` 兩個 Agent 工具，現在**也支援前景終端機（foreground terminals）**，不再僅限於先前由 Agent 建立的背景終端機。

這代表 Agent 現在可以：

- 讀取終端機面板中**任何可見終端機**的輸出
- 向這些終端機送出輸入

例如，當您有一個正在執行的 REPL 或互動式腳本時，Agent 可以與之互動，提供更豐富的整合場景。此外，終端機通知預設啟用，不再需要手動輪詢（manual polling）即可獲得結果。

### Edit Mode 棄用提醒

Edit Mode 自 VS Code 1.110 起已正式宣告棄用（deprecated）。相關設定會繼續支援至 **v1.125** 為止；之後，Edit Mode 將被**完全移除**，且無法再透過設定啟用。

老使用者需要開始準備調整工作流程，轉向更整合的 Agent 體驗。新的安裝環境則可直接享受整合度更高的體驗。

---

## VS Code Agents 應用程式

### 鍵盤導航與專屬命令

VS Code 1.116 為 Agents 應用程式新增了專屬命令與鍵盤快捷鍵，使得使用者可以透過鍵盤完整導航主要介面元件，而不必倚賴滑鼠。新增的聚焦命令包括：

- **Changes view（變更檢視）** 聚焦
- **Changes view 內的檔案樹（files tree）** 聚焦
- **Chat Customizations view（聊天自訂項目檢視）** 聚焦

### 無障礙說明對話框

Agents 應用程式的聊天輸入框新增了無障礙說明對話框（accessibility help dialog），可透過 `⌥F1`（macOS）／ `Alt+F1`（Windows）／ `Shift+Alt+F1`（Linux）開啟。

此對話框會顯示：

- 該輸入框可用的命令
- 對應的鍵盤快捷鍵
- **朗讀詳細度（announcement verbosity）控制選項**

對於依賴螢幕閱讀器的使用者特別有用，能更快了解可使用的功能與互動方式。

### `#` 觸發的檔案內容自動完成

Agents 應用程式新增 `#` 觸發的檔案上下文（file-context）自動完成功能。此功能的範圍限定在 **picker 中選取的工作區（workspace）** 內，讓您可以使用 `#檔名` 的方式快速插入並參考工作區中的檔案內容到 Agent 對話脈絡中。

---

## 編輯器（Editor）

### CSS `@import` 解析至 `node_modules`

VS Code 1.116 新增對 CSS `@import` 解析至 `node_modules` 的支援。當您使用 bundler（如 webpack、Vite、Parcel 等）時，撰寫類似下列的匯入：

```css
@import "some-module/style.css";
```

現在可以使用 `Ctrl+click`（macOS 為 `Cmd+click`）直接跳轉到對應 `node_modules` 模組中的 CSS 檔案，提升 CSS 專案的導航體驗。

---

## 無障礙功能（Accessibility）

VS Code 1.116 在 Agents 應用程式上強化了多項無障礙功能：

- 新增專屬命令與鍵盤快捷鍵以聚焦 Changes view、檔案樹及 Chat Customizations view，提供完整鍵盤導航
- 新增聊天輸入框的無障礙說明對話框（`⌥F1` / `Alt+F1` / `Shift+Alt+F1`），列出可用命令與快捷鍵，並提供朗讀詳細度控制，便於螢幕閱讀器使用者操作

---

## 終端機（Terminal）

- 終端機通知（notifications）預設啟用，不再需要手動輪詢即可取得命令完成結果
- `send_to_terminal` 與 `get_terminal_output` 工具支援前景終端機，Agent 可以讀取/送出輸入至任何可見終端機

---

## 新／更新設定摘要

| 設定 | 說明 |
|------|------|
| `chat.disableAIFeatures` | 全域停用 VS Code 中的所有 AI 功能 |

---

## 新／更新命令與快捷鍵摘要

| 命令／快捷鍵 | 說明 |
|-------------|------|
| 聚焦 Changes view | Agents 應用程式新增聚焦命令 |
| 聚焦 Changes view 內檔案樹 | Agents 應用程式新增聚焦命令 |
| 聚焦 Chat Customizations view | Agents 應用程式新增聚焦命令 |
| `⌥F1` / `Alt+F1` / `Shift+Alt+F1` | 開啟 Agents 應用程式聊天輸入框無障礙說明對話框 |

---

## 棄用與移除（Deprecations & Removals）

| 項目 | 狀態 |
|------|------|
| Edit Mode | 自 v1.110 棄用，將於 **v1.125 完全移除** |

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Built-in Extension | 內建擴充功能 |
| GitHub Copilot Chat | GitHub Copilot Chat |
| Inline Suggestions | 行內建議 |
| Agents | Agents（AI 代理） |
| chat.disableAIFeatures | chat.disableAIFeatures 設定 |
| AI Features | AI 功能 |
| Agent Debug Logs | Agent 偵錯紀錄 |
| Agent Debug Log Panel | Agent 偵錯紀錄面板 |
| Chronological Event Log | 時序事件紀錄 |
| Persisted Locally | 持久化儲存於本地 |
| Failed Sessions | 失敗的工作階段 |
| Chat Customizations | 聊天自訂項目 |
| Foreground Terminals | 前景終端機 |
| Background Terminals | 背景終端機 |
| send_to_terminal | send_to_terminal 工具 |
| get_terminal_output | get_terminal_output 工具 |
| REPL | REPL（互動式直譯器） |
| Manual Polling | 手動輪詢 |
| VS Code Agents App | VS Code Agents 應用程式 |
| Changes View | 變更檢視 |
| Files Tree | 檔案樹 |
| Chat Customizations View | 聊天自訂項目檢視 |
| Keybindings | 鍵盤快捷鍵 |
| Keyboard Navigation | 鍵盤導航 |
| Accessibility Help Dialog | 無障礙說明對話框 |
| Announcement Verbosity | 朗讀詳細度 |
| Screen Reader | 螢幕閱讀器 |
| File-context Completions | 檔案上下文自動完成 |
| `#` Trigger | `#` 觸發符 |
| Workspace Picker | 工作區選擇器 |
| CSS @import | CSS @import 匯入 |
| node_modules Resolution | node_modules 解析 |
| Bundler | 打包器（bundler） |
| Edit Mode | 編輯模式（Edit Mode） |
| Deprecated | 已棄用 |
| Removed | 已移除 |

---

*資料來源：VS Code 1.116 發行說明 (https://code.visualstudio.com/updates/v1_116)*
