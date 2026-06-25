# Visual Studio Code 1.126

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 6 月 24 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.126.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.126.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.126.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.126.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.126.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.126.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.126.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.126.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.126.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.126 版本。本次發行帶來更清楚的成本透明度、更簡易的模型調整，以及更安全地瀏覽不熟悉的程式碼。

- [**工作階段層級成本**](#工作階段層級成本資訊)：查看聊天工作階段的總成本以發現昂貴的對話。
- [**每工作階段多個聊天**](#agent-host-copilot-工作階段中的多個聊天)：在一個 agent host Copilot 工作階段中並排執行多個聊天。
- [**工作區信任**](#以限制模式開啟新資料夾)：以限制模式安全瀏覽新資料夾。

Happy Coding!

---

## 成本管理

### 工作階段層級成本資訊

您現在可以查看整個聊天工作階段的成本，而非僅單次對話的成本。這讓您更好地了解哪些工作階段消耗最多點數，更容易發現昂貴的對話並隨時間管理您的使用量。

![截圖顯示工作階段資訊彈出框，包含以點數計算的工作階段成本和整個聊天工作階段的上下文視窗 Token 使用量。](https://code.visualstudio.com/assets/updates/1_126/session-token-usage.webp)

---

## 語言模型

### 統一的模型自訂選擇器

為了簡化語言模型設定，我們將上下文大小和推理（思考）力度控制合併至單一模型自訂選擇器。從一個地方，您就可以在調整模型時調整兩個設定，而不用操作兩個分開的下拉選單。

![截圖顯示模型自訂選擇器，包含合併的上下文大小和推理力度控制。](https://code.visualstudio.com/assets/updates/1_126/model-customization-picker.webp)

### 簡化的模型懸停

我們清理了模型懸停以使其更容易掃視。它現在顯示模型能力的簡潔一字描述，並包含深層連結按鈕，可直接帶您前往相關設定。

![截圖顯示簡化的模型懸停，包含一字能力描述和深層連結設定按鈕。](https://code.visualstudio.com/assets/updates/1_126/model-hover.webp)

---

## Agents 視窗（Preview）

[Agents 視窗](https://aka.ms/VSCode/Agents/docs)是一個專用的輔助視窗，針對跨專案和機器探索、迭代和審閱 Agent 工作階段進行最佳化。

### Agent Host Copilot 工作階段中的多個聊天

Agents 視窗讓您可以並排執行和管理多個 Agent 工作階段。在本次發行中，從 agent host 啟動的 Copilot 工作階段可以同時包含多個聊天。由於聊天共用相同的工作階段和工作上下文，您可以在同一工作區中同時進行多個對話。

假設您的主要聊天正忙於實作一個功能。與其等待或中斷它，不如選取工作階段工具列中的 **New Chat**（`+`）在同一工作階段中開啟第二個聊天，然後用它來審閱目前的變更、撰寫測試或寫文件。兩者同時執行，每個聊天保持自己的對話。您可以在分頁之間切換，並從離開的地方繼續。

聊天在視窗重新載入後會被保留和還原。離開後回來可以看到工作階段中的每個對話，而不僅是第一個。

您可以直接在分頁中重新命名聊天以追蹤每個聊天的用途，就像從工作階段標題重新命名工作階段一樣：

- **雙擊**分頁，或從其右鍵選單選取 **Rename**，以就地編輯標題。
- 按 **Enter** 確認重新命名，或按 **Escape** 取消。在編輯時選取另一個分頁也會取消編輯並切換至該分頁。

聊天的標題獨立於工作階段標題，因此重新命名工作階段不會覆寫您重新命名的聊天。

### 使用 Agent Host 工具鏈的 Agent 程式碼回饋

在 Agents 視窗中，您在生成的程式碼上留下的留言現在儲存在 agent host 上，因此 Agent 可以透過使用 `listComments` 和 `resolveComments` 等伺服器端工具與您的回饋互動。即使您中斷客戶端連線，這也能運作，因為留言存在於伺服器上而非您的本機工作階段中。

Agent 也可以透過使用 `addComment` 工具為您建立留言。當您執行像 `/code-review` 這樣的審閱技能時，它會審閱您的程式碼並行內新增留言，您可以在將它們提交給 Agent 處理之前接受或刪除。

Pull Request 審閱留言以相同方式運作。您可以接受 PR 審閱留言並將它們提交給 Agent，或要求 Agent 解決所有 PR 留言。當您要求 Agent 解決您尚未接受的 PR 留言時，它會先請求您的許可來查看，一旦您授予存取權，它就會處理 PR 審閱項目。

---

## 編輯器體驗

### 以限制模式開啟新資料夾

**設定**：`security.workspace.trust.startupPrompt`

[工作區信任](https://code.visualstudio.com/docs/editing/workspaces/workspace-trust)讓您決定專案資料夾是否可以自動執行程式碼，這在您處理不熟悉的程式碼時增加了一層安全性。

先前，開啟新資料夾會立即以對話框中斷您，詢問是否信任該資料夾，然後您才能查看其內容。現在，新資料夾以[限制模式](https://code.visualstudio.com/docs/editing/workspaces/workspace-trust#_restricted-mode)開啟，僅顯示信任橫幅。這讓您可以先安全瀏覽程式碼，在準備好時再信任該資料夾。

![截圖顯示新資料夾開啟時出現的限制模式橫幅，包含限制模式用於安全程式碼瀏覽的訊息和信任該資料夾的連結。](https://code.visualstudio.com/assets/updates/1_126/restricted-mode-banner.webp)

這將 `security.workspace.trust.startupPrompt` 設定的預設值從 `once` 變更為 `never`。若要恢復先前的行為並在第一次開啟資料夾時收到提示，請將值設回 `once`。

### 從工作區信任編輯器中移除 Trust Parent

工作區信任編輯器先前在 **Trust** 按鈕旁顯示一個 **Trust Parent** 按鈕。因為它看起來與 **Trust** 一樣但會信任整個父資料夾，很容易誤選並信任比您預期更多的資料夾。

為了降低該風險，**Trust Parent** 按鈕已被移除。您仍可以透過將路徑新增至工作區信任編輯器中的 **Trusted Folders & Workspaces** 清單來信任父資料夾。

![截圖顯示移除 Trust Parent 按鈕後，工作區信任編輯器僅有一個 Trust 按鈕。](https://code.visualstudio.com/assets/updates/1_126/trust-parent-button.webp)

---

## 網站

### VS Code 部落格

隨著團隊接連撰寫越來越多的部落格文章，我們意識到部落格區段需要改進。先前，當您開啟部落格區段時，會被直接帶到最後一篇文章，之前的文章經常被忽略。我們現在新增了一個[部落格首頁](https://code.visualstudio.com/blogs)，醒目顯示最近的幾篇文章。

![截圖顯示新的部落格首頁，包含近期文章清單和部落格封存的連結。](https://code.visualstudio.com/assets/updates/1_126/blog-landing-page.webp)

如果您正在尋找所有部落格文章的完整清單，現在可以在[部落格封存](https://code.visualstudio.com/blogs/archive)中找到。

### VS Code 文件

我們重新組織了文件目錄結構，使其更容易掃視和導覽。所有 Agent 相關文件現在歸類在單一的「Agents」區段下，與編輯程式碼和設定 VS Code 相關的內容歸類在「Editor」下。

先前，支援的語言和特定擴充功能的文件分別列在目錄中。我們現在將它們分別移至「Languages and Runtimes」和「Extension Docs」下，讓您可以在一個地方找到所需的所有資訊。

歡迎在 microsoft/vscode-docs 儲存庫中[提交回饋](https://github.com/microsoft/vscode-docs/issues)，讓我們知道您對新結構的看法。

---

## 已棄用的功能和設定

無。

---

## 感謝

對 `vscode` 的貢獻：

- [@bikeshgyawali (Bikesh)](https://github.com/bikeshgyawali)：為 uuid.ts 中的 prefixedUuid 新增遺漏的單元測試覆蓋 [PR #322146](https://github.com/microsoft/vscode/pull/322146)
- [@Bryan2333 (BryanLiang)](https://github.com/Bryan2333)：修正 issue 300307 [PR #322104](https://github.com/microsoft/vscode/pull/322104)
- [@carlbrochu (Carl Brochu)](https://github.com/carlbrochu)：新增 SKU 以增強 GH 遙測事件 [PR #321046](https://github.com/microsoft/vscode/pull/321046)
- [@cavalloJustinEmery (Justin Emery)](https://github.com/cavalloJustinEmery)：修正：連線至遠端時外掛技能檔案無法存取 [PR #309465](https://github.com/microsoft/vscode/pull/309465)
- [@guomaggie](https://github.com/guomaggie)：選取正確的子代理模型 [PR #321061](https://github.com/microsoft/vscode/pull/321061)
- [@mjbvz (Matt Bierner)](https://github.com/mjbvz)
  - 更新貢獻名稱 [PR #321503](https://github.com/microsoft/vscode/pull/321503)
  - 完全將一般 `npm run compile` 切換為也使用 tsgo [PR #321646](https://github.com/microsoft/vscode/pull/321646)
  - 在監看模式期間終止並重啟 esbuild 執行個體 [PR #321219](https://github.com/microsoft/vscode/pull/321219)
- [@rfeltis (Ralph Feltis)](https://github.com/rfeltis)：為聊天配額通知橫幅新增遙測 [PR #321793](https://github.com/microsoft/vscode/pull/321793)
- [@romalpani (Rohan Malpani)](https://github.com/romalpani)：更新工作階段中新聊天的提示文字 [PR #321965](https://github.com/microsoft/vscode/pull/321965)
- [@wszgrcy (chen)](https://github.com/wszgrcy)：修正：registerToolDefinition 遺失 tags [PR #319922](https://github.com/microsoft/vscode/pull/319922)

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
| agent host | agent host |
| Agents window | Agents 視窗 |
| Autopilot | Autopilot |
| blog archive | 部落格封存 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| chat | 聊天 |
| context size | 上下文大小 |
| context window | 上下文視窗 |
| cost | 成本 |
| credits | 點數 |
| deep link | 深層連結 |
| diff | 差異 |
| extension | 擴充功能 |
| harness | 工具鏈 |
| hover | 懸停 |
| Integrated Browser | 整合式瀏覽器 |
| model customization picker | 模型自訂選擇器 |
| model picker | 模型選擇器 |
| reasoning effort | 推理力度 |
| Restricted Mode | 限制模式 |
| session | 工作階段 |
| terminal | 終端機 |
| token | Token |
| Workspace Trust | 工作區信任 |

*資料來源：[Visual Studio Code 1.126 發行說明](https://code.visualstudio.com/updates/v1_126)*
