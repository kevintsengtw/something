# Visual Studio Code 1.122 版本重點摘要

**版本：** 1.122
**最後更新：** 2026 年 5 月 23 日
**原文：** https://code.visualstudio.com/updates/v1_122

---

本版本為每週滾動式發行說明格式，涵蓋 2026 年 5 月 19 日至 5 月 23 日的更新。以下依主題分類整理重點：

## 一、Agents 與遠端 Agent

- Agent 現在可以在遠端機器上觸發任務
- Agents 視窗工作階段清單的懸停工具提示顯示完整工作階段標題和資料夾路徑
- Agent 提交、同步或執行其他 git 操作後，Agents 視窗中的原始碼控制狀態現在會自動重新整理
- 本機 agent host 在 Insiders 組建中預設啟用
- 修正遠端 Agent 已變更檔案的編輯器標籤顯示內部 URI 而非使用者可見路徑的問題

## 二、整合式瀏覽器

- 新增「Add Screenshot to Chat」操作，可擷取整合式瀏覽器中當前頁面的截圖並附加至聊天訊息

## 三、語言模型與 BYOK

- BYOK 模型現在可在無 GitHub 驗證的隔離環境（air-gapped）中運作
- 語言模型編輯器顯示供應商群組的精細操作（更新 API 金鑰、新增模型、前往設定檔、重新命名、刪除）
- 推理力度選擇器現在為未指定明確預設值的模型家族顯示有效的等級，而非「undefined」

## 四、MCP 與擴充功能

- 透過 `vscode:mcp/install` 協定 URL 安裝 MCP 伺服器時保留 `gallery` 欄位，確保來自登錄檔的伺服器保留其中繼資料
- MCP OAuth 支援在 `mcp.json` 的 `oauth` 區段指定自訂 `clientId`，支援不支援動態客戶端註冊的伺服器；客戶端秘密安全儲存在作業系統秘密存放區

## 五、搜尋與編輯器

- 搜尋面板新增「Search only in changed files」切換，將結果限制為有未提交原始碼控制變更的檔案
- 在聊天輸入中使用 `/models` 開啟模型選擇器

## 六、終端機與聊天

- 在聊天中執行的終端機命令顯示點/ASCII 載入動畫
- 使用 `sudo -S` 透過 stdin 傳遞密碼的命令在自動核准模式中不再被自動取消

## 七、語言支援

- 當 tsgo 處於活動狀態時，「Sort imports」和「Remove unused imports」操作不再出現，因為 tsgo 以不同方式處理這些操作
- Mermaid 圖表使用從當前 VS Code 色彩主題衍生的主題，在新編輯器中開啟的圖表顯示完整內容
- Mermaid C4 圖表中的行內 data-URI 圖片現在在聊天和 Markdown 預覽中正確渲染

## 八、無障礙與其他

- 螢幕朗讀程式現在在使用 F8 開啟錯誤預覽小工具時朗讀問題訊息
- 新的問題回報精靈可直接從 VS Code 建立高品質的問題報告（含截圖和錄影），透過 `issueReporter.wizard.enabled` 啟用
- GitHub Enterprise 登入引導使用帶即時驗證的行內表單取代模態對話框

---

*資料來源：[Visual Studio Code 1.122 發行說明](https://code.visualstudio.com/updates/v1_122)*
