# Visual Studio Code 1.114 版本重點摘要

**版本：** 1.114（2026 年 4 月 1 日發行）
**類型：** 每週穩定版（Weekly Stable Release）
**主題：** 本次發行專注於簡化您的聊天體驗

---

## 一、影片預覽（Video Preview）

- 圖片輪播（Image Carousel）現在也支援影片，可在聊天附件和檔案總管右鍵選單中播放和導覽影片
- 此功能是 1.113 版引入的圖片輪播的延伸
- 相關設定：`imageCarousel.chat.enabled` 和 `imageCarousel.explorerContextMenu.enabled`

## 二、複製最終回應（Copy Final Response）

- 聊天右鍵選單新增「Copy Final Response」命令
- 僅複製 Agent 回應的最後 Markdown 部分（在所有工具呼叫完成之後）
- 先前複製聊天回應時會一併帶入工具呼叫和除錯日誌，造成剪貼簿內容雜亂
- 新命令隔離了實際輸出，可直接貼入文件或 commit 訊息

## 三、聊天疑難排解增強（Chat Troubleshooting）

- `/troubleshoot` 現可參考任何先前的聊天工作階段來診斷問題
- 使用 `/troubleshoot` 並加入 `#session`，即可觸發工作階段選擇器
- 從先前工作階段列表中選取，無需重現問題即可事後調查
- 可診斷自訂指令被忽略、回應速度緩慢等問題

## 四、簡化的工作區搜尋（Simplified Workspace Search）

- 移除令人困惑的本地（local）與遠端（remote）索引區分
- 現在只有單一狀態：您的程式碼庫是否已建立語意索引
- `#codebase` 工具現在純粹用於語意搜尋（不再退回到模糊文字搜尋）
- Copilot 自動按需建立索引並自動使用，使用者無需手動管理
- Agent 仍可在需要時執行文字和模糊搜尋

## 五、TypeScript 6.0 支援

- JavaScript 和 TypeScript 支援已更新為 TypeScript 6.0
- TypeScript 6.0 是基於 JavaScript 的最後一個版本
- 棄用了多項舊選項，為 TypeScript 7.0（以 Go 重寫的編譯器）做準備
- 被棄用的選項包括 ES5 目標、classic 模組解析，以及 AMD、UMD 等舊版模組格式
- 可設定 `"ignoreDeprecations": "6.0"` 暫時抑制棄用警告

## 六、Edit Mode 正式棄用（Edit Mode Deprecated）

- Edit Mode 自 1.110 版起已被棄用，Agent Mode 取代其所有功能
- 可透過 `chat.editMode.hidden` 設定暫時重新啟用
- 此設定將支援到 1.125 版
- 1.125 版起 Edit Mode 將完全移除，無法再透過設定啟用
- 建議使用者改用 Agent Mode 或建立符合需求的自訂 Agent

## 七、企業原則控制（Enterprise Policy Controls）

- 管理員可透過群組原則停用 Claude Agent 整合
- 設定 `github.copilot.chat.claudeAgent.enabled` 由組織管理
- 原則金鑰為 `Claude3PIntegration`（布林值）
- 組織可集中管理 AI 功能，控制 Agent 功能、模型存取、MCP 伺服器安裝來源等

## 八、釘選工作階段改善（Pinned Sessions Improvement）

- 釘選的聊天工作階段現在在工作階段列表中顯示釘選圖示
- 更容易區分已釘選和未釘選的工作階段

## 九、Python 環境改善

- 工作區儲存的直譯器選擇現在優先於終端機啟動的虛擬環境或 conda 環境（跨重啟保留）
- env 檔案變更通知現在包含「不再顯示」（Don't Show Again）選項，可永久關閉提示

## 十、無障礙功能改善（Accessibility）

- 無障礙檢視（Accessible View）現在會動態串流聊天回應，即時顯示生成中的內容
- 無需關閉再重新開啟檢視即可看到更新的內容
- MCP 伺服器輸出預設從無障礙檢視中排除，以減少噪音

---

*資料來源：VS Code 1.114 發行說明 (https://code.visualstudio.com/updates/v1_114)*
