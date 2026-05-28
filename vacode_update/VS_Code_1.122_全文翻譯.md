# Visual Studio Code 1.122

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_最後更新：2026 年 5 月 23 日_

歡迎來到 Visual Studio Code 1.122 版本。

Happy Coding!

---

## 2026 年 5 月 23 日

- 在整合式瀏覽器中新增「Add Screenshot to Chat」操作，可擷取當前頁面的截圖並附加至聊天訊息。_[#318065](https://github.com/microsoft/vscode/issues/318065)_

---

## 2026 年 5 月 22 日

- Agents 視窗工作階段清單中的懸停工具提示顯示完整的工作階段標題和資料夾路徑。_[#317858](https://github.com/microsoft/vscode/issues/317858)_

- 透過 `vscode:mcp/install` 協定 URL 安裝 MCP 伺服器時保留 `gallery` 欄位，確保來自登錄檔的伺服器保留其中繼資料，用於更新檢查和啟用。_[#293481](https://github.com/microsoft/vscode/issues/293481)_

- MCP OAuth 支援在 `mcp.json` 的 `oauth` 區段中指定自訂 `clientId`，以支援不支援動態客戶端註冊的伺服器。客戶端秘密安全儲存在作業系統秘密存放區中。_[#257415](https://github.com/microsoft/vscode/issues/257415)_

---

## 2026 年 5 月 21 日

- Bring Your Own Key（BYOK）模型現在可在無 GitHub 驗證的隔離環境（air-gapped）中運作。_[#317428](https://github.com/microsoft/vscode/pull/317428)_

- 本機 agent host 在 Insiders 組建中預設啟用。_[#317667](https://github.com/microsoft/vscode/pull/317667)_

- 當 tsgo 處於活動狀態時，「Sort imports」和「Remove unused imports」操作不再出現，因為 tsgo 以不同方式處理這些操作。_[#317656](https://github.com/microsoft/vscode/pull/317656)_

- 推理力度選擇器現在為未指定明確預設值的模型家族顯示有效的等級，而非「undefined」。_[#317622](https://github.com/microsoft/vscode/pull/317622)_

---

## 2026 年 5 月 20 日

- 搜尋面板新增「Search only in changed files」切換，將結果限制為有未提交原始碼控制變更的檔案。_[#314790](https://github.com/microsoft/vscode/pull/314790)_

- 語言模型編輯器顯示供應商群組的精細操作，例如更新 API 金鑰、新增模型、前往設定檔、重新命名和刪除。_[#317419](https://github.com/microsoft/vscode/pull/317419)_

- 在聊天中執行的終端機命令顯示點/ASCII 載入動畫。_[#317416](https://github.com/microsoft/vscode/pull/317416)_

- 新的問題回報精靈讓您可以直接從 VS Code 建立高品質的問題報告，包括截圖和錄影。透過 `issueReporter.wizard.enabled` 啟用。_[#317577](https://github.com/microsoft/vscode/pull/317577)_

- Mermaid 圖表使用從當前 VS Code 色彩主題衍生的主題，在新編輯器中開啟的圖表顯示完整內容。_[#317617](https://github.com/microsoft/vscode/pull/317617)_、_[#317244](https://github.com/microsoft/vscode/pull/317244)_

- GitHub Enterprise 登入引導使用帶即時驗證的行內表單取代模態對話框。_[#317205](https://github.com/microsoft/vscode/pull/317205)_

- 使用 `sudo -S` 透過 stdin 傳遞密碼的命令在自動核准模式中不再被自動取消。_[#317594](https://github.com/microsoft/vscode/pull/317594)_

- 修正遠端 Agent 已變更檔案的編輯器標籤顯示內部 URI 而非使用者可見路徑的問題。_[#316812](https://github.com/microsoft/vscode/pull/316812)_

---

## 2026 年 5 月 19 日

- Agent 現在可以在遠端機器上觸發任務。_[#312052](https://github.com/microsoft/vscode/issues/312052)_

- Agent 提交、同步或執行其他 git 操作後，Agents 視窗中的原始碼控制狀態現在會自動重新整理。_[#317317](https://github.com/microsoft/vscode/pull/317317)_

- 在聊天輸入中使用 `/models` 開啟模型選擇器。_[#317060](https://github.com/microsoft/vscode/pull/317060)_

- Mermaid C4 圖表中的行內 data-URI 圖片現在在聊天和 Markdown 預覽中正確渲染。_[#317235](https://github.com/microsoft/vscode/pull/317235)_

- 螢幕朗讀程式現在在使用 F8 開啟錯誤預覽小工具時朗讀問題訊息。_[#316835](https://github.com/microsoft/vscode/pull/316835)_

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| agent host | agent host |
| Agents window | Agents 視窗 |
| air-gapped | 隔離環境 |
| auto-approve mode | 自動核准模式 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Dynamic Client Registration | 動態客戶端註冊 |
| gallery | gallery（欄位名稱） |
| Integrated Browser | 整合式瀏覽器 |
| issue reporter wizard | 問題回報精靈 |
| MCP | MCP |
| Mermaid | Mermaid |
| model picker | 模型選擇器 |
| OAuth | OAuth |
| reasoning effort | 推理力度 |
| screen reader | 螢幕朗讀程式 |
| secret store | 秘密存放區 |
| session | 工作階段 |
| Source Control | 原始碼控制 |
| terminal | 終端機 |
| token | Token |
| tsgo | tsgo |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.122 發行說明](https://code.visualstudio.com/updates/v1_122)*
