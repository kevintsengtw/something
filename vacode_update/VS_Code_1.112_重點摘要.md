# Visual Studio Code 1.112 版本重點摘要

**版本：** 1.112
**發行日期：** 2026 年 3 月 18 日
**原文：** https://code.visualstudio.com/updates/v1_112

---

本次發行包含跨 **Agent 與開發者體驗**的多項改善。以下為官方列出的五大亮點：

## 一、整合式瀏覽器除錯

- 全新 `editor-browser` 除錯類型，可在 VS Code 整合式瀏覽器中直接啟動除錯工作階段
- 支援 Launch 和 Attach 組態，大多數既有 `msedge` 和 `chrome` 除錯組態可直接遷移（僅需修改 type）
- 可設定中斷點、逐步執行程式碼、檢視變數，全程無需離開 VS Code
- 整合式瀏覽器 UX 改善：新增右鍵選單（複製/貼上、在新分頁開啟、檢查元素）、獨立縮放層級（`workbench.browser.pageZoom`）

## 二、Copilot CLI 權限等級

- Copilot CLI 工作階段現可配置權限等級，減少核准中斷
- 三種等級：**Default Permissions**（使用已設定的核准設定）、**Bypass Approvals**（自動核准所有工具呼叫並自動重試錯誤）、**Autopilot**（自動核准、自動回應問題、持續自主工作直到任務完成，設定：`chat.autopilot.enabled`）

## 三、MCP 伺服器沙箱化

- 可在 macOS 和 Linux 上以沙箱環境執行本地 stdio MCP 伺服器
- 沙箱伺服器具備受限的檔案系統和網路存取權限
- 在 `mcp.json` 中設定 `"sandboxEnabled": true` 即可啟用
- 當沙箱伺服器需要額外資料夾或網域存取權時，VS Code 會提示授權
- 目前不支援 Windows（WSL 和 SSH 遠端情境仍可運作）

## 四、Agent 圖片與二進位檔案支援

- Agent 可原生讀取磁碟上的圖片檔案和二進位檔案（二進位以 hexdump 格式呈現）
- Agent 或工具產生的圖片輸出可在聊天回應中選取，並在專用的圖片輪播檢視中開啟
- 相關設定：`chat.imageCarousel.enabled`、`imageCarousel.explorerContextMenu.enabled`

## 五、Monorepo 自訂項目探索

- 新設定 `chat.useCustomizationsInParentRepositories` 可從父資料夾向上探索至儲存庫根目錄
- 適用於所有聊天自訂類型：始終啟用的指令（`copilot-instructions.md`、`AGENTS.md`、`CLAUDE.md`）、指令檔案、提示檔案、自訂 Agent、技能、掛鉤
- 無需開啟完整儲存庫即可在 Monorepo 中跨套件共享自訂項目

---

## 其他

- **Copilot CLI 訊息引導與排隊**：可在前一個請求執行中時發送訊息以引導 Agent 或排隊後續訊息
- **委派至 Copilot CLI 前預覽變更**：Chat 檢視現在顯示待處理的變更清單
- **Copilot CLI 終端機可點擊檔案連結**：終端機連結偵測器可識別 Copilot CLI 產生的路徑（`github.copilot.chat.cli.terminalLinks.enabled`）
- **自動符號參考**：複製符號並貼入聊天時自動轉換為符號參考 `#sym:Name`
- **/troubleshoot 技能（Preview）**：分析 Agent 偵錯日誌，診斷工具或子代理被使用或跳過的原因、指令未載入原因、回應緩慢原因等
- **匯出和匯入 Agent 偵錯日誌（Preview）**：可分享或離線分析 Agent 工作階段的偵錯日誌
- **外掛程式與 MCP 伺服器啟用/停用**：無需解除安裝即可全域或按工作區啟用/停用
- **外掛程式自動更新**：依據 `extensions.autoUpdate` 設定運作，npm 和 pypi 來源的更新需核准
- **MCP 引出表單改善 UI**：使用與 Ask Questions 工具相同的 UI
- **終端機 IME 輸入改善**：修正接近終端機右緣時 IME 合成預覽文字溢出的問題
- **搜尋後自動關閉尋找對話方塊**：新設定 `editor.find.closeOnResult`
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）
- **重要修正**：修正 fish + kitty 鍵盤協定中 ^C 無法結束的問題、防止 Python 雙重/三重啟動

---

*資料來源：[Visual Studio Code 1.112 發行說明](https://code.visualstudio.com/updates/v1_112)*
