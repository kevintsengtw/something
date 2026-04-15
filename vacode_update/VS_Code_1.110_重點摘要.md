# VS Code 2026 年 2 月更新（版本 1.110）— 重點摘要

> 發布日期：2026 年 3 月 4 日
> 來源：[官方更新頁面](https://code.visualstudio.com/updates/v1_110)

---

## 核心主題

本次更新的核心主題是**讓 AI 代理（Agent）能夠處理更長時間、更複雜的任務**，為使用者提供更多控制權與可見性、全新的代理擴展方式，以及更智慧的工作階段管理。

---

## 重點功能一覽

### 1. 代理外掛（Agent Plugins）（預覽版）
- 全新的「代理外掛」系統，可安裝包含技能、命令、代理、MCP 伺服器和掛鉤的預打包套件
- 在擴展檢視中輸入 `@agentPlugins` 或執行 `Chat: Plugins` 即可搜尋安裝
- 預設從 `copilot-plugins` 和 `awesome-copilot` 儲存庫取得

### 2. 代理瀏覽器工具（Agentic Browser Tools）（實驗性）
- AI 代理可在編輯器內操作瀏覽器：點擊元素、擷取截圖、讀取主控台日誌
- 啟用方式：`workbench.browser.enableChatTools` 設定
- 代理可自行驗證所做的前端變更

### 3. 工作階段記憶（Session Memory）
- Plan 代理的計畫持久化至工作階段記憶中
- 跨對話輪次保持可用，進行增量修改而非重建

### 4. 上下文壓縮（Context Compaction）
- 自動壓縮：上下文視窗達上限時自動觸發
- 手動壓縮：新增 `/compact` 斜線命令，適用於背景代理

### 5. 分叉聊天工作階段（Fork Chat Session）
- `/fork` 命令建立獨立的對話分支，繼承完整歷史
- 適合比較不同實作策略

### 6. 代理除錯面板（Agent Debug Panel）（預覽版）
- 即時查看代理事件、工具呼叫和已載入的自訂設定
- 顯示系統提示詞和工具呼叫詳情

### 7. 休眠防護（Sleep Prevention）
- 聊天請求執行期間防止系統自動暫停
- 注意：合上未接電源的筆電仍會觸發休眠

### 8. 編輯模式淘汰（Edit Mode Deprecated）
- Edit Mode 正式被標記為淘汰，Agent Mode 已涵蓋其所有功能
- 預設從模式選擇器中隱藏，將於版本 1.125 完全移除

### 9. 自動核准（Auto-Approve）
- `/autoApprove` 斜線命令可跳過工具確認提示
- npm/pnpm/yarn 腳本在 package.json 中定義時預設自動核准
- 沙盒模式的 MCP 伺服器工具確認自動核准

### 10. 新增代理工具
- `/getDiagnostics`：直接將編輯器的警告和錯誤拉入聊天
- `askQuestions` 工具移入 VS Code 核心，改善可靠性

---

## 其他重要改進

### MCP 改進
- Claude Agent 支援 MCP 伺服器，自動偵測已安裝的 MCP 伺服器
- 新增本地 MCP 伺服器沙盒選項（stdio 傳輸），提供檔案系統和網路存取隔離
- 沙盒伺服器僅能存取明確允許的路徑和網域

### 聊天無障礙功能
- 問題輪播完全支援螢幕閱讀器（如「問題 1/3」的位置朗讀）
- 聊天詢問或需要確認時播放無障礙信號並顯示系統通知
- `⇧⌘T`（Ctrl+Shift+T）快速在代理 TODO 清單和聊天輸入間切換焦點

### 工作台（Workbench）
- 通知位置可自訂：右上、右下或左下
- 聊天設定移至設定編輯器的獨立頂層分類
- 新增「複製導覽路徑（Copy Breadcrumbs Path）」命令

### 終端機
- 支援 Kitty 圖形協定，可在整合式終端中顯示行內圖片
- 終端機調整大小時支援像素尺寸回報
- macOS 和 Linux 支援 Ghostty 作為外部終端機

### 編輯器
- 「反轉行」功能在單行選取時套用至整份文件
- 修復多個可能導致編輯器延遲的版面重排問題
- 「前往工作區符號」搜尋包含 `#` 字元時不再錯誤過濾結果（修正 rust-analyzer 相容性）

### 語言支援
- 改進 Shebang 語言偵測：如 `#!/usr/bin/env -S deno -A` 正確識別為 TypeScript

---

*本摘要根據 VS Code 官方更新頁面及多個相關報導整理翻譯。建議參閱[原文](https://code.visualstudio.com/updates/v1_110)以獲取最完整的資訊。*
