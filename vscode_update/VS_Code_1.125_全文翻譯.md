# Visual Studio Code 1.125

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 6 月 17 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.125.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.125.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.125.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.125.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.125.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.125.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.125.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.125.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.125.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.125 版本。本次發行帶來更智慧的整合式瀏覽器、更多擴充功能更新控制，以及更強的 Copilot 企業管理。

- [**安裝模型供應商**](#從語言模型編輯器安裝模型供應商)：透過 Marketplace 探索和安裝額外的模型。
- [**整合式瀏覽器**](#整合式瀏覽器)：搜尋網路並透過遠端連線安全瀏覽，無需離開 VS Code。
- [**可設定的自動更新延遲**](#可設定的擴充功能自動更新延遲)：選擇 VS Code 在安裝擴充功能更新前等待多久。
- [**Copilot 政策**](#透過原生-mdm-傳遞受管理的-copilot-設定)：透過現有的裝置管理工具傳遞受管理的 Copilot 設定。

Happy Coding!

---

## Agents

### 在 VS Code 中檢視額外花費用量

為了確保您能在超額費用之前做好準備，Copilot 狀態儀表板現在顯示您已消耗的額外 Copilot 預算百分比，讓您可以在達到設定上限前調整使用量。

![截圖顯示 Copilot 狀態儀表板中的額外花費限制。](https://code.visualstudio.com/assets/updates/1_125/additional_budget.webp)

您可以在 [Copilot 設定](https://github.com/settings/copilot/features)中檢視詳細用量並管理您的額外花費。

---

## 語言模型

### 從語言模型編輯器安裝模型供應商

除了 Bring Your Own Key（BYOK）模型之外，擴充功能可以貢獻自己的模型供應商。先前，要找到這類擴充功能，您需要知道正確的標籤（`language-models`）才能在擴充功能檢視中搜尋。

現在語言模型編輯器有一個 **Install Model Providers** 按鈕，可開啟已篩選至貢獻模型供應商的擴充功能檢視，更容易探索和安裝它們。安裝供應商後，其模型會出現在模型選擇器中，與您已設定的任何其他模型並列。

若要了解更多，請參閱[語言模型文件](https://code.visualstudio.com/docs/agent-customization/language-models)。

---

## 整合式瀏覽器

### 從網址列搜尋網路

**設定**：`workbench.browser.searchEngine`

無需離開 VS Code 即可查詢資訊：在整合式瀏覽器的網址列中輸入查詢，它會使用您設定的搜尋引擎執行，與在獨立瀏覽器中的方式相同。使用 `workbench.browser.searchEngine` 設定來選擇要使用的搜尋引擎。

![截圖顯示從整合式瀏覽器的網址列進行網路搜尋。](https://code.visualstudio.com/assets/updates/1_125/browser-search.webp)

### 透過遠端連線瀏覽（Preview）

**設定**：`workbench.browser.enableRemoteProxy`

當整合式瀏覽器在遠端工作區中開啟時，HTTP(S) 網路流量現在可以透過遠端連線進行代理。這讓您可以安全連線至僅從遠端機器可存取的任何埠或服務。

這是 Preview 功能，因此您可能會遇到 Bug。啟用 `workbench.browser.enableRemoteProxy` 設定以試用，並在 [VS Code 儲存庫](https://github.com/microsoft/vscode/issues)中回報您遇到的任何問題。

### 改善與轉發埠的 Agent 互動

如果您在遠端工作區中轉發了一個埠，先前 Agent 可能因為潛在不同的埠號而難以開啟瀏覽器。

現在，如果 Agent 請求一個已轉發的埠（且遠端代理未啟用），URL 會被重寫且 Agent 會被通知變更。

---

## 編輯器體驗

### 擴充功能自動更新設定

**設定**：`extensions.autoUpdate`（此設定由組織層級管理。請聯繫您的管理員以變更。）

您可以透過 `extensions.autoUpdate`（此設定由組織層級管理。請聯繫您的管理員以變更。）設定來啟用或停用擴充功能自動更新。在本次發行中，我們將設定簡化為使用 `on` 和 `off` 值。先前的值如 `true`、`false`、`onlyEnabledExtensions` 和 `delayed` 會自動遷移。

啟用自動更新時，VS Code 僅更新已啟用的擴充功能。已停用的擴充功能不再自動更新，它們會在您下次啟用時更新。

> **注意**：管理員可以透過[企業政策](https://code.visualstudio.com/docs/enterprise/policies)集中管理 `extensions.autoUpdate`（此設定由組織層級管理。請聯繫您的管理員以變更。）和 `extensions.autoUpdateDelay`（此設定由組織層級管理。請聯繫您的管理員以變更。）設定。

### 可設定的擴充功能自動更新延遲

**設定**：`extensions.autoUpdateDelay`（此設定由組織層級管理。請聯繫您的管理員以變更。）

為了給您更多控制權來決定何時安裝擴充功能更新，您現在可以設定自動更新的延遲。這是建立在[上一個版本引入的延遲擴充功能自動更新](https://code.visualstudio.com/updates/v1_123#_delayed-extension-autoupdates)功能之上。

使用 `extensions.autoUpdateDelay`（此設定由組織層級管理。請聯繫您的管理員以變更。）設定來設定延遲的小時數。預設情況下，VS Code 在安裝擴充功能更新前等待兩小時。延遲僅在啟用自動更新時適用。

### Language Server Protocol

建置語言伺服器的擴充功能作者現在可以透過更新至 [Language Server Protocol 3.18 版](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.18/specification/)來採用最新的協定功能。對應的 VS Code 客戶端和伺服器套件以 `vscode-languageclient@10.0.0` 和 `vscode-languageserver@10.0.0` 提供。如需完整的協定新增和重大變更清單，請參閱 [vscode-languageserver-node 變更日誌](https://github.com/microsoft/vscode-languageserver-node/blob/main/README.md#3180-protocol-900-json-rpc-1000-client-and-1000-server)。

---

## 企業

### 透過原生 MDM 傳遞受管理的 Copilot 設定

管理員現在可以透過 Windows 和 macOS 上的原生裝置管理（MDM）管道傳遞受管理的 GitHub Copilot 設定，作為帳戶型企業設定檔的補充。這是建立在[企業管理的 Copilot 外掛政策](https://code.visualstudio.com/updates/v1_124#_enterprise)之上，讓組織可以使用現有的裝置管理工具強制執行 Copilot 設定，而無需依賴每位使用者的登入來套用政策。

對開發者而言，透過 MDM 傳遞的設定在 VS Code 中顯示為政策強制執行，無法在本機覆寫。未來的更新將擴展跨 Copilot 介面支援的政策金鑰集合。

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@arun-357 (Arunachalam Nachiappan)](https://github.com/arun-357)
  - 修正圖片輪播標題中顯示原始 Markdown [PR #320754](https://github.com/microsoft/vscode/pull/320754)
  - 修正圖片輪播在模態編輯器標題中懸停時顯示 UUID [PR #320739](https://github.com/microsoft/vscode/pull/320739)
  - 為 Images Preview 編輯器標籤使用媒體圖示 [PR #320951](https://github.com/microsoft/vscode/pull/320951)
- [@dymaaaj7 (Dimitrije)](https://github.com/dymaaaj7)：修正 CompletionItemKind 中 File 和 Reference 的宣告順序 [PR #314958](https://github.com/microsoft/vscode/pull/314958)
- [@g0w6y (ⳕⲛτⲉⲅⲥⲉⳏτⲟⲅ 🕵🏻)](https://github.com/g0w6y)：在 MCP HTTP 客戶端中驗證重新導向 scheme 並去除跨來源重新導向的憑證 [PR #320347](https://github.com/microsoft/vscode/pull/320347)
- [@guomaggie](https://github.com/guomaggie)：從 Copilot Proxy 切換至 CAPI V3 [PR #320472](https://github.com/microsoft/vscode/pull/320472)
- [@kangarko (Matej)](https://github.com/kangarko)：新增設定以在編輯器中開啟已變更的聊天檔案而非差異 [PR #320948](https://github.com/microsoft/vscode/pull/320948)
- [@lucaspar (Lucas Parzianello)](https://github.com/lucaspar)：修正 CLI 更新中的拼寫錯誤 [PR #245751](https://github.com/microsoft/vscode/pull/245751)
- [@merfanian (Mahdi Erfanian)](https://github.com/merfanian)：跨聊天參考 API 邊界保留圖片來源出處 [PR #320624](https://github.com/microsoft/vscode/pull/320624)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)：修正：將繼續註解限制為以空白分隔的斜線 [PR #321230](https://github.com/microsoft/vscode/pull/321230)
- [@Tyriar (Daniel Imms)](https://github.com/Tyriar)：修正（terminal）：追蹤連字附加元件設定以進行變更偵測 [PR #318992](https://github.com/microsoft/vscode/pull/318992)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| auto-update | 自動更新 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Copilot status dashboard | Copilot 狀態儀表板 |
| enterprise policy | 企業政策 |
| extension | 擴充功能 |
| forwarded port | 轉發埠 |
| Integrated Browser | 整合式瀏覽器 |
| Language Models editor | 語言模型編輯器 |
| Language Server Protocol (LSP) | Language Server Protocol（LSP） |
| MDM (Mobile Device Management) | MDM（裝置管理） |
| model picker | 模型選擇器 |
| model provider | 模型供應商 |
| remote proxy | 遠端代理 |
| remote workspace | 遠端工作區 |
| search engine | 搜尋引擎 |
| session | 工作階段 |
| terminal | 終端機 |
| token | Token |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.125 發行說明](https://code.visualstudio.com/updates/v1_125)*
