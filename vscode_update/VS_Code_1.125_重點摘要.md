# Visual Studio Code 1.125 版本重點摘要

**版本：** 1.125
**發行日期：** 2026 年 6 月 17 日
**原文：** https://code.visualstudio.com/updates/v1_125

---

本次發行帶來更智慧的整合式瀏覽器、更多擴充功能更新控制，以及更強的 Copilot 企業管理。以下為官方列出的四大亮點：

## 一、安裝模型供應商

- 語言模型編輯器新增 **Install Model Providers** 按鈕，開啟已篩選至貢獻模型供應商的擴充功能檢視
- 安裝後模型出現在模型選擇器中，與其他已設定的模型並列
- 先前需知道正確的標籤（`language-models`）才能在擴充功能檢視中搜尋

## 二、整合式瀏覽器

- **網址列搜尋**（`workbench.browser.searchEngine`）：在網址列輸入查詢即可使用設定的搜尋引擎搜尋，無需離開 VS Code
- **透過遠端連線瀏覽（Preview）**（`workbench.browser.enableRemoteProxy`）：在遠端工作區中，HTTP(S) 網路流量可透過遠端連線代理，安全連線至僅從遠端機器可存取的埠或服務
- **改善與轉發埠的 Agent 互動**：Agent 請求已轉發的埠時，URL 自動重寫並通知 Agent 變更

## 三、可設定的擴充功能自動更新延遲

- **設定**：`extensions.autoUpdateDelay`（由組織層級管理）
- 可設定自動更新延遲的小時數，預設為兩小時
- **擴充功能自動更新設定簡化**（`extensions.autoUpdate`）：簡化為 `on` 和 `off` 值，先前的 `true`、`false`、`onlyEnabledExtensions`、`delayed` 會自動遷移；啟用時僅更新已啟用的擴充功能，已停用的不再自動更新
- 管理員可透過企業政策集中管理這兩個設定

## 四、透過原生 MDM 傳遞受管理的 Copilot 設定

- 管理員可透過 Windows 和 macOS 上的原生裝置管理（MDM）管道傳遞受管理的 GitHub Copilot 設定
- 透過 MDM 傳遞的設定在 VS Code 中顯示為政策強制執行，無法在本機覆寫
- 不依賴每位使用者的登入即可套用政策

---

## 其他

- **在 VS Code 中檢視額外花費用量**：Copilot 狀態儀表板顯示已消耗的額外 Copilot 預算百分比，可在達到設定上限前調整使用量
- **Language Server Protocol**：更新至 LSP 3.18 版，對應套件為 `vscode-languageclient@10.0.0` 和 `vscode-languageserver@10.0.0`

---

*資料來源：[Visual Studio Code 1.125 發行說明](https://code.visualstudio.com/updates/v1_125)*
