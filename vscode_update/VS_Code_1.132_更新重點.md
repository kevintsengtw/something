# VS Code 1.132 (Insiders) 更新重點

**版本：** 1.132（Insiders）｜**最後更新：** 2026 年 8 月 3 日
**原文：** https://code.visualstudio.com/updates/v1_132

> 本版為 Insiders 滾動式更新記錄（7/24–7/31），內容會隨新功能加入持續演進。

---

- **語音模式與聽寫（本版重心）**：新增感知佈景主題的環境光暈以區分聆聽／說話狀態；支援 Nemotron 3.5 的多語言裝置端聽寫；聽寫數字可格式化為數字符號；引導卡片加入本地化語音預覽與首次使用設定橫幅；可用 `dictation.md`／`voice.md`（放在 `~/.copilot/` 或受信任工作區的 `.github`）自訂行為；新增 **Chat: Install Dictation Model from Local Package...** 手動安裝模型；並修正模型下載中無法取消聽寫的問題。
- **整合式瀏覽器**：可為頁面元素加上註解；同一元素的多個附件（文字內容＋截圖）合併為單一項目。
- **聊天與終端機**：行內終端機輸出會隨面板／側邊欄大小動態重新換行；Agent Host 終端機自動核准規則改用專用文法解析 PowerShell；`${cwd}` 分頁標題以 `~` 取代完整家目錄路徑。
- **編輯器體驗**：`code serve-web` 支援 `--enable-proposed-api`，讓宣告 `enabledApiProposals` 的擴充功能在瀏覽器版正確啟動；**Remove Manual Folding Ranges** 改為只移除游標處最內層範圍；Markdown 預覽的程式碼區塊新增懸停複製按鈕；編輯器分頁右鍵選單新增 **Save** 與 **Save As**。

**總結**：本版明顯以語音輸入為主軸，把聽寫從「能用」推進到可多語言、可自訂、可離線裝置端執行；其餘則是瀏覽器註解、終端機與編輯器的一系列體驗打磨。

---

*資料來源：[Visual Studio Code 1.132 發行說明](https://code.visualstudio.com/updates/v1_132)*
