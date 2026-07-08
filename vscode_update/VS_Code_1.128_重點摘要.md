# Visual Studio Code 1.128 版本重點摘要

**版本：** 1.128
**發行日期：** 2026 年 7 月 8 日
**原文：** https://code.visualstudio.com/updates/v1_128

---

本次發行帶來更豐富的多聊天 Agent 工作階段、正式可用的圖片支援，以及作業系統層級的鍵盤快速鍵。以下為官方列出的五大亮點：

## 一、多聊天 Agent 工作階段支援 Claude Agent

- Claude agent host 工作階段現在支援多聊天，可在單一工作階段中包含多個相關聊天
- 可比較不同做法、從較早的回合分支、並行執行工作
- 新增聊天、分支聊天、切換對等聊天、同時發送回合
- 每個聊天保持獨立的歷史紀錄、標題和模型選取，重啟後隨父工作階段一同還原

## 二、Agents 視窗中無需工作區的快速聊天

- 可在 Agents 視窗中不選取工作區直接開始聊天，適合不綁定資料夾的問題
- 這些聊天出現在 **Chats** 區段，不顯示工作區專屬的 **Changes** 或 **Files** 側邊窗格
- 快速聊天在重新載入後與其他工作階段一同還原，且與工作區工作階段分開

## 三、Copilot Vision 正式可用

- 多模態支援正式可用，可將圖片和 PDF 貼上、拖放至 Chat 中
- Agent 也可透過工具呼叫讀取圖片

## 四、整合式瀏覽器分頁配置

- **設定**：`workbench.browser.newTabPlacement`
- 可設定瀏覽器分頁開啟的位置：`activeGroup`（預設，在使用中的編輯器群組開啟）、`sideGroup`（在側邊的專用群組開啟）、`window`（在專用輔助視窗開啟）
- 從現有分頁開啟的頁面仍在與父頁面相同的群組中開啟

## 五、作業系統層級鍵盤快速鍵

- VS Code 現在可貢獻作業系統層級的鍵盤快速鍵，即使 VS Code 不在焦點也能生效
- 在 `keybindings.json` 中為按鍵繫結定義新增 `systemWide` 屬性即可啟用

---

## 其他

- **唯讀子代理聊天（Preview）**：當工作階段產生子代理時，其逐字稿以唯讀對等聊天形式出現，可從 Conversations 選單、執行中子代理膠囊或父逐字稿中的行內子代理膠囊開啟
- **Agents 視窗中的聊天鍵盤快速鍵**：支援建立聊天、重新開啟最後關閉的聊天、切換上/下一個聊天、關閉/刪除活動聊天分頁、開啟聊天搜尋選擇器等鍵盤操作
- **BYOK 模型用於 agent host Copilot 工作階段（實驗性）**：啟用 `chat.agentHost.byokModels.enabled` 後可在 agent host 工作階段使用 BYOK 模型
- **自訂端點模型的取樣參數設定**：可為每個自訂端點模型設定 `temperature` 和 `top_p`
- **設定 BYOK 的預設公用模型**：透過 `chat.byokUtilityModelDefault` 控制使用 BYOK 模型時的公用流程（如聊天標題產生、提交訊息產生）行為
- **深層連結至特定聊天**：`vscode://` 深層連結可直接開啟工作區並聚焦至連結指定的特定聊天
- **企業：使用 OpenTelemetry 管理 Copilot 遙測匯出**：管理員可透過受管理設定中的 `telemetry` 區塊強制指定 OTel 資料的匯出端點、協定、服務名稱、標頭及內容擷取策略

---

*資料來源：[Visual Studio Code 1.128 發行說明](https://code.visualstudio.com/updates/v1_128)*
