# Visual Studio Code 1.114 版本重點摘要

**版本：** 1.114
**發行日期：** 2026 年 4 月 1 日
**原文：** https://code.visualstudio.com/updates/v1_114

---

本次發行聚焦於**精簡您的聊天體驗**。以下為官方列出的四大亮點：

## 一、影像輪播中的影片預覽

- 影像輪播現在也支援影片，可在聊天附件或檔案總管右鍵選單中播放與瀏覽影片
- 檢視器包含影片播放控制項，以及透過箭頭或縮圖瀏覽所有圖片與影片的導航功能
- 相關設定：`imageCarousel.chat.enabled`、`imageCarousel.explorerContextMenu.enabled`

## 二、複製聊天最終回應

- 新增 **Copy Final Response** 命令，可在聊天右鍵選單中複製 Agent 最後一段 Markdown 回應（排除思考過程與工具呼叫），方便分享

## 三、疑難排解先前的聊天工作階段（Preview）

- `/troubleshoot` 技能可分析 Agent 偵錯日誌，診斷聊天問題（如自訂指令被忽略或回應緩慢）
- 現可引用任何先前的聊天工作階段進行疑難排解，無需重現問題
- 使用 `/troubleshoot` 搭配 `#session` 可觸發工作階段選擇器
- 相關設定：`github.copilot.chat.agentDebugLog.enabled`、`github.copilot.chat.agentDebugLog.fileLogging.enabled`

## 四、工作區搜尋簡化

- `#codebase` 工具現在純粹用於語意搜尋，提供一致的結果
- 不再區分「本機索引」與「遠端索引」，簡化為單一狀態：程式碼庫是否已建立語意索引
- Copilot 會自動按需建立索引並使用，使用者無需自行管理
- 即使工作區未建立語意索引，仍可透過 Copilot 的其他搜尋方法（文字、grep、符號）取得良好結果

---

## 其他

- **TypeScript 6.0**：JavaScript 和 TypeScript 支援現使用 TypeScript 6.0，包含重要修正與改善，並棄用多個舊選項以為 TypeScript 7.0 重寫做準備
- **Python**：Python Environments 擴充功能多項錯誤修正（工作區直譯器選擇優先順序、env 檔案變更通知新增「不再顯示」選項），新增 Pixi 環境偵測與推薦
- **企業群組政策 — 停用 Claude Agent**：管理員可透過群組政策停用聊天中的 Claude Agent 整合（政策金鑰 `Claude3PIntegration`）
- **GitHub Pull Requests 擴充功能**：建立 PR 檢視中分支名稱快取加速載入、GitHub 永久連結可開啟本機對應檔案
- **提案 API — 細粒度工具核准**：語言模型工具可將核准範圍限定至特定參數組合
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）
- **重要修正**：修正整合式瀏覽器中 VS Code 快捷鍵優先於頁面快捷鍵的問題、修正偵錯暫停時「Add Element to Chat」無法運作的問題

---

*資料來源：[Visual Studio Code 1.114 發行說明](https://code.visualstudio.com/updates/v1_114)*
