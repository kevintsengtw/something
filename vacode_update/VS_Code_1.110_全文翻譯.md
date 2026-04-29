# Visual Studio Code 1.110 版本更新 — 全文翻譯（繁體中文）

| 項目 | 內容 |
|-----|------|
| **版本** | 1.110 |
| **發行日期** | 2026 年 3 月 4 日 |
| **原文 URL** | https://code.visualstudio.com/updates/v1_110 |

---

## 概述

**安全更新**：下列擴充功能已進行安全更新：GitHub.copilot-chat。

**更新 1.110.1**：此更新解決了核心中的這些安全問題以及 GitHub Copilot Chat 擴充功能中的這些安全問題。

歡迎使用 2026 年 2 月版本的 Visual Studio Code。**此版本使代理程式 (Agent) 對於執行時間更長且更複雜的任務變得更加實用**，為您提供更多控制和可見性、代理程式擴充的新方法，以及更聰明的工作階段管理。

* **Agent 外掛程式**：從擴充功能檢視安裝技能、工具和鉤子的預先封裝組合
* **代理程式瀏覽器工具**：讓代理程式自動驅動瀏覽器與您的應用程式互動並驗證其自身變更
* **工作階段記憶體**：在對話轉換間持久化計畫和指導
* **上下文壓縮**：手動壓縮對話歷史記錄以釋放上下文空間
* **分支聊天工作階段**：建立新的獨立工作階段，繼承對話歷史記錄以探索替代方法
* **Agent 偵錯面板**：即時查看代理程式事件、工具呼叫和已載入的自訂設定
* **聊天無障礙支援**：使用螢幕閱讀器改進、鍵盤導覽和通知訊號充分利用聊天
* **從聊天建立代理程式自訂**：直接從對話產生提示、技能、代理程式和鉤子
* **Kitty 圖形協定**：在整合終端機中直接呈現高保真影像

祝您編碼愉快！

---

## Agent 控制項

無論您是偵錯代理程式行為、調整核准流程，還是將工作交接給背景程序，這些更新都可讓您更好地查看和控制代理程式的執行方式。

### 背景代理程式

利用背景代理程式，您可以將任務交接給 Copilot CLI，同時仍在 VS Code 中追蹤這些任務。我們進行了多項改進，以配合背景代理程式與本機和雲端代理程式的功能和體驗。

* **上下文壓縮**：當上下文視窗達到上限時，Copilot 會自動壓縮對話歷史記錄。您現在也可以使用 `/compact` 斜線命令為背景代理程式手動觸發壓縮。
* **使用 /slash 命令**：聊天自訂選項（如提示檔案、鉤子和技能）現在也在背景代理程式工作階段中以斜線命令的形式提供。
* **重新命名背景代理程式工作階段**：您現在可以重新命名背景代理程式工作階段，以便更輕鬆地追蹤它們。

### Claude Agent

上個月，我們新增了 Claude Agent，讓您可以使用 GitHub Copilot 訂閱中包含的 Claude 模型來與 Claude Agent SDK 互動。

這個月，我們利用新功能和改進進一步擴展了此體驗：

* **轉向和佇列** 功能，讓您在對話中途傳送後續訊息以改變代理程式的方法或排隊其他要求。
* **工作階段重新命名**
* **使用壓縮的上下文視窗呈現**
* **其他斜線命令**
  * `/compact` 用於按需壓縮
  * `/agents` 用於管理自訂代理程式
  * `/hooks` 用於管理 Claude 鉤子
* **新增 `getDiagnostics` 工具** 讓代理程式存取編輯器和工作區問題
* **顯著的效能改進**

更多改進已在計畫中。請在 GitHub 上分享您的意見反應！

### Agent 偵錯面板（預覽）

使用不同的代理程式自訂（如鉤子、技能和自訂代理程式），有時很難理解向代理程式傳送訊息時發生的情況。Agent 偵錯面板可讓您更深入地查看聊天工作階段以及聊天自訂的載入方式。

Agent 偵錯面板會即時顯示聊天事件，包括聊天自訂事件、系統提示、工具呼叫等。您可以準確查看為工作階段載入了哪些提示檔案、技能、鉤子和其他自訂設定，從而更輕鬆地理解和疑難排解代理程式設定。這將取代舊的「診斷」聊天動作，具有更豐富、更詳細的檢視。

從命令面板使用 **Developer: Open Agent Debug Panel** 開啟面板，或選擇「聊天」檢視頂部的齒輪圖示，然後選擇 **View Agent Logs**。

此面板也包含圖表檢視，顯示事件的視覺階層，因此您可以快速理解聊天工作階段期間發生的結構和序列。

此體驗仍在預覽中，請試用並分享您的意見反應！

> **注意**：Agent 偵錯面板目前僅適用於本機聊天工作階段。記錄資料不會持久化，因此您只能檢視目前 VS Code 工作階段中的聊天工作階段記錄。

### 用於啟用自動核准的斜線命令

您現在可以使用斜線命令直接從聊天輸入切換全域自動核准，無需瀏覽設定：

* `/autoApprove` 為所有工具啟用全域自動核准
* `/disableAutoApprove` 停用全域自動核准

`/yolo` 和 `/disableYolo` 是相同命令的別名。

> **注意**：全域自動核准會略過所有工具確認提示，讓代理程式執行工具和終端機命令而無需等待您的核准。這可以加快更長、多步驟的任務，但意味著您無法有機會取消可能具破壞性的動作。在啟用此功能前，請務必瞭解安全性含義，並考慮使用終端機沙箱進行額外保護。

### 編輯和詢問模式變更

**設定**：`chat.editMode.hidden`

隨著代理程式的發展，Agent 模式現在可以處理編輯模式能做的所有事情，甚至更多，具有更好的效能和可靠性。編輯模式現在預設會從代理程式選擇器中隱藏，使用者可從最強大的模式受惠，而無需在選項間選擇。您可以透過停用 `chat.editMode.hidden` 設定來將其還原。

詢問模式現在由自訂代理程式定義支援，使其成為完全的代理程式體驗。這可解決先前的限制，例如在詢問和代理程式模式間切換時需要建立新工作階段。

兩項變更都示範了如何自訂自己的代理程式。如果您偏好編輯模式或想要自己的詢問模式版本，請透過定義其工具、提示和語言模型來建立符合您需求的自訂代理程式。停用 `chat.editMode.hidden` 時，您可以在代理程式選擇器中選擇 **View edit agent** 動作來查看為編輯模式供電的代理程式宣告，這可作為自訂代理程式的起點。

在自訂代理程式文件中瞭解如何建立自訂代理程式。

### 詢問問題工具

`askQuestions` 工具在聊天互動期間呈現問題輪播 UI，已移至 VS Code 核心。這改進了取消要求時的可靠性，並使工具能在不同內容中一致運作，包括子代理程式。

輪播為使用中時，您現在可以傳送轉向訊息，而無需先回復或解除待處理的問題。這可讓您即時重新導向代理程式的回應，甚至在問題序列中途。使用鍵盤以 ⌥N (Windows、Linux Alt+N)（下一個）和 ⌥P (Windows、Linux Alt+P)（上一個）在問題間導覽。

### 防止聊天期間自動休眠

VS Code 現在會要求作業系統在聊天要求執行時不要自動休眠機器。您可以離開電腦，無需擔心中斷代理程式的回應。

請注意，關閉未插電筆記本電腦的蓋子仍會觸發休眠。

---

## Agent 擴充性

代理程式的實用性取決於您提供給它們的工具和自訂。此版本讓擴展代理程式能做的事情變得更容易，從可安裝的外掛程式組合到瀏覽器自動化和新的程式碼感知工具。

### Agent 外掛程式（實驗性）

**設定**：`chat.plugins.enabled`、`chat.plugins.marketplaces`、`chat.plugins.paths`

VS Code 現在支援代理程式外掛程式，這些是聊天自訂的預先封裝組合。外掛程式可以包含技能、命令、代理程式、MCP 伺服器和鉤子。

您可以直接從 VS Code 內的「擴充功能」檢視中搜尋和安裝代理程式外掛程式。在搜尋框中輸入 `@agentPlugins` 或從命令面板執行 **Chat: Plugins** 命令。

根據預設，VS Code 從 `copilot-plugins` 和 `awesome-copilot` 存放庫擷取外掛程式。您可以透過以下設定來設定更多來源：

* `chat.plugins.marketplaces`：透過指定 GitHub 或純 git 存放庫來新增其他外掛程式市場。此設定也可以支援 Claude 風格的市場，例如 `anthropics/claude-code`。
* `chat.plugins.paths`：透過指定其路徑並啟用或停用它們來註冊本機外掛程式目錄。

在 Agent 外掛程式文件中瞭解更多資訊。

### 代理程式瀏覽器工具（實驗性）

**設定**：`workbench.browser.enableChatTools`

在上一版本中，我們新增了 VS Code 桌面中新的整合瀏覽器，可讓您直接在編輯器內與網頁互動。但如果您的代理程式可以自動使用此瀏覽器並在建置網站時驗證變更呢？

在此版本中，我們為代理程式新增了一組工具來讀取和與整合瀏覽器互動。當代理程式與頁面互動時，它會看到頁面內容的更新和主控台中的任何錯誤和警告。這些工具可立即使用，無需安裝任何額外相依性。

* **頁面導覽**：`openBrowserPage`、`navigatePage`
* **頁面內容和外觀**：`readPage`、`screenshotPage`
* **使用者互動**：`clickElement`、`hoverElement`、`dragElement`、`typeInPage`、`handleDialog`
* **自訂瀏覽器自動化**：`runPlaywrightCode`

這些工具讓代理程式能夠同時執行 Web 應用程式的編寫和驗證，並為代理程式關閉開發迴圈。

根據預設，代理程式開啟的頁面在私用的記憶體內工作階段中執行。這可讓您控制代理程式可以存取的瀏覽資料。若要讓代理程式存取整合瀏覽器中的特定網頁，您可以明確地與代理程式共享頁面以提供臨時存取和任何已儲存的資料。

若要試用新工具，請啟用 `workbench.browser.enableChatTools` 並在聊天工具選擇器中啟用瀏覽器工具。

請參閱瀏覽器代理程式測試指南以取得逐步教學課程。

### 從聊天建立代理程式自訂

您現在可以使用代理程式模式中的新 `/create-*` 斜線命令直接從聊天對話產生代理程式自訂檔案：

* `/create-prompt`：產生可重複使用的提示檔案
* `/create-instruction`：為專案慣例產生指令檔案
* `/create-skill`：將多步驟工作流程擷取到技能套件中
* `/create-agent`：建立特定的自訂代理程式角色
* `/create-hook`：建立生命週期自動化的鉤子設定

每個命令都會引導您完成建立流程，並讓您選擇使用者層級（全帳戶）或工作區層級（特定於專案）儲存。

這些命令也可以從進行中的對話擷取模式。例如，在多個轉換上偵錯問題後，使用 `/create-skill` 將該程序捕捉為可重複使用的技能，或使用 `/create-instruction` 將更正轉換為專案慣例。

您無需記住確切的斜線命令。您也可以使用自然語言，例如「將此工作流程儲存為技能」或「從此擷取指令」，代理程式會辨識您的意圖並啟動正確的建立流程。

相同的產生選項也可從提示、指令、技能和代理程式的快速選擇功能表中取得，用閃閃發光的圖示表示。

### 用途和重新命名的工具

我們已更新 `usages` 工具，也新增了 `rename` 工具。這些工具可重複使用現有擴充功能或 LSP 功能，並讓代理程式能以高精度和最佳效能導覽和重構程式碼。

代理程式應該會自動取得這些新工具。但是，我們發現代理程式強烈傾向於改用 grep，這對此案例來說較不理想。您可以透過明確地 #-提及工具名稱（如 `Use #rename and change the name of fib to fibonacci`）或設定 `SKILL.md` 檔案來協助代理程式。

---

## 更聰明的工作階段

對於執行時間更長和多轉換的任務，當代理程式記住上下文、有效地委派研究，並保持內聯編輯同步時，效果會更好。這些改進讓工作階段更具復原力和上下文感知。

### 計畫的工作階段記憶體

由「計畫代理程式」(Plan Agent) 建立的計畫現在會持久化到工作階段記憶體，並在對話轉換中保持可用。當您詢問改進時，代理程式會以現有計畫為基礎，而不是從零開始。

計畫也會在同一工作階段中的不相關訊息後召回，因此您可以回到計畫而無需重複上下文。在較長的實作工作期間，即使舊的對話歷史記錄被壓縮以釋放上下文空間，該計畫仍可在記憶體中存取。

### 上下文壓縮

隨著對話成長，累積的訊息和上下文可能會填滿模型的上下文視窗。上下文壓縮會總結對話歷史記錄以釋放空間，因此您可以在同一工作階段中繼續工作，而無需遺失重要詳細資訊。

當上下文視窗達到上限時，VS Code 會自動壓縮對話，但您也可以手動觸發壓縮。手動壓縮適用於本機、背景和 Claude Agent 工作階段。若要手動壓縮，請使用以下任一方法：

* 在聊天輸入欄位中輸入 `/compact`。您可選擇在命令後新增自訂指示以引導摘要的產生方式，例如 `/compact focus on the database schema decisions`。
* 選擇聊天輸入框中的上下文視窗控制項，然後選擇 **Compact Conversation**。

在文件中深入瞭解上下文壓縮。

### 使用 Explore 子代理程式的程式碼庫搜尋

**設定**：`chat.exploreAgent.defaultModel`

「計畫代理程式」現在始終將程式碼庫研究委派給專用的 **Explore** 子代理程式。Explore 是一個唯讀代理程式，僅使用搜尋和檔案讀取工具，並專注於快速的平行程式碼庫探索。透過將研究卸載到 Explore，「計畫代理程式」可以產生參考工作區中特定檔案和程式碼路徑的計畫。

Explore 根據預設在快速模型上執行（Claude Haiku 4.5、Gemini 3 Flash），以保持研究快速，而「計畫代理程式」使用完整模型進行計畫。您可以使用 `chat.exploreAgent.defaultModel` 設定覆寫模型。將滑鼠懸停在聊天中的探索任務上，以查看用於研究的模型。

> **注意**：Explore 不能直接作為獨立代理程式叫用。它只能作為按需使用的子代理程式提供。

### 內聯聊天和聊天工作階段

當代理程式工作階段已經變更檔案時，內聯聊天現在始終將新訊息排隊到該工作階段，而不是隔離地進行變更。這確保使用完整上下文，在檢查代理程式編輯時也很有用。

### 分支聊天工作階段

您現在可以分支聊天工作階段來建立新的獨立工作階段，繼承原始工作階段的對話歷史記錄。當您想要探索替代方法、詢問附加問題或不失原始上下文的情況下以不同方向分支長對話時，這很有用。

分支工作階段有兩種方法：

* **分支整個工作階段**：在聊天輸入框中輸入 `/fork` 以建立具有完整對話歷史記錄的新工作階段。
* **從檢查點分支**：將滑鼠懸停在任何聊天要求上，並選擇 **Fork Conversation** 以建立僅包含到該點為止的對話的新工作階段。

分支的工作階段完全獨立—一個工作階段中的變更不會影響另一個工作階段。深入瞭解分支聊天工作階段。

---

## 聊天體驗

對聊天介面的小型改進總和：更整潔的模型選擇器、更少來自工具輸出的視覺雜亂，以及即使您在另一個檔案中深入工作時也能到達您的通知。

### 重新設計的模型選擇器

我們已重新設計語言模型下拉式清單，以改進為任務選擇正確模型。新下拉式清單將模型組織到清晰的區段中：

* **自動** 始終顯示在清單頂部。
* **特色和最近使用的模型** 接著顯示。最多四個最近使用的模型與為您的帳戶精選的特色模型一起顯示。隨著您使用模型，它們會自動移至此區段。
* **其他模型** 是包含其餘可用模型的可摺疊群組。展開它也會在底部顯示 **Manage Models** 選項。
* 搜尋框可讓您快速按名稱篩選模型。

每個模型項目都顯示包含模型詳細資訊（例如功能和上下文視窗大小）的豐富懸停。在您目前的 GitHub Copilot 計畫上無法取得的模型也會顯示，但無法選擇。

### 使用內容提示探索功能（實驗性）

**設定**：`chat.tips.enabled`

VS Code 現在會在「聊天」檢視中顯示上下文提示，協助您探索功能並充分利用您的 AI 編碼體驗。提示會在您開始新的聊天工作階段時出現，並根據您的使用模式進行自訂。為了避免不堪重負，只會建議您尚未嘗試過的功能，使其相關且可操作。

提示涵蓋各種功能，包括：建立自訂代理程式、提示和技能；使用訊息佇列和轉向；切換到更好的模型；啟用實驗性功能，如 YOLO 模式和自訂思考片語。

使用導覽控制項瀏覽可用的提示，或解除您不感興趣的個別提示。提示在您使用建議的功能後會自動隱藏。您可以使用 `chat.tips.enabled` 完全停用提示。

新提示會隨著功能發行而定期新增，因此請經常查回以取得新建議。

### 自訂思考片語

**設定**：`chat.agent.thinking.phrases`

在推理或工具呼叫期間顯示的載入文字現在可自訂。您可以使用預先定義的自訂片語，以 `replace` 模式完成現有預設片語，或使用 `append` 讓自訂片語成為現有預設片語的新增項目。

此設定可讓您將聊天載入體驗個人化。

### 可摺疊的終端機工具呼叫

**設定**：`chat.tools.terminal.simpleCollapsible`

代理程式模式中的終端機工具叫用現在會顯示為可摺疊的區段。不是長終端機輸出使對話變得雜亂，而是每個終端機命令顯示為摘要標題，您可以展開以顯示完整輸出。這可減少視覺雜亂，並使掃描多步驟代理程式互動變得更容易。此功能可以用 `chat.tools.terminal.simpleCollapsible` 設定停用。

### 聊天回應和確認的作業系統通知

**設定**：`chat.notifyWindowOnResponseReceived`、`chat.notifyWindowOnConfirmation`

之前，聊天回應和確認要求的作業系統通知只在 VS Code 不在焦點時出現。這意味著，如果您在 VS Code 中主動處理另一項任務，您可能會遺漏重要更新，例如何時收到回應或代理程式何時需要您的核准才能繼續進行。

您現在可以透過將設定值設為 `always` 來設定這些通知即使在視窗在焦點時也出現。

### 內聯聊天懸停模式

**設定**：`inlineChat.renderMode`

內聯聊天正在從「行之間」UI 轉換到懸停型 UI。您可以透過 `inlineChat.renderMode` 啟用它，這使內聯聊天輸入更像重新命名體驗。提交提示後，進度和結果會顯示在右上角。

### 內聯聊天指示

**設定**：`inlineChat.affordance`

為了提供更輕鬆的方法來啟動內聯聊天，我們新增了兩種指示，在您的選擇旁邊顯示。它們與燈泡結合，不會妨礙您。

`inlineChat.affordance` 設定有三個可能的值：
* `off`：不在文字選擇上顯示指示
* `editor`：在編輯器中於選擇旁邊顯示功能表
* `gutter`：在編輯器開槽（行號區域）中於選擇旁邊顯示功能表

---

## 無障礙支援

此版本改進了螢幕閱讀器支援、鍵盤導覽和聊天互動的意識，使每位開發人員都能有效地使用 VS Code 的 AI 功能。

### 在可存取檢視中切換思考內容

螢幕閱讀器使用者現在可以在聊天回應可存取檢視中切換思考內容的包含 ⌥T (Windows、Linux Alt+T)。這可讓您選擇在讀取回應時是否包含模型的推理流程。

### 問題輪播無障礙支援

聊天問題輪播現在完全可供螢幕閱讀器使用者存取：
* 問題會與其位置一起宣佈（例如「第 1 個問題，共 3 個」）
* 使用 Alt+N 和 Alt+P 在問題間導覽
* 使用 ⇧⌘A (Windows、Linux Ctrl+Shift+A) 在問題輪播和聊天輸入間切換焦點
* 在螢幕閱讀器模式中，焦點不再自動移動以防止中斷

### 聊天問題和確認的通知

**設定**：`chat.notifyWindowOnConfirmation`、`accessibility.signals.chatUserActionRequired`

當聊天詢問問題或需要確認時，VS Code 現在會播放無障礙訊號，並在啟用時顯示作業系統通知。

### 鍵盤繫結以切換 TODO 清單焦點

使用 ⇧⌘T (Windows、Linux Ctrl+Shift+T) 快速在代理程式 TODO 清單和聊天輸入間切換焦點。

### 在可存取檢視中記住的游標位置

當您在內容串流時關閉可存取檢視時，游標位置現在會在您重新開啟時保留。

### 尋找和篩選無障礙協助

在任何尋找或篩選對話框中按 Alt+F1 以開啟上下文無障礙協助。

### 快速輸入螢幕閱讀器改進

前往行對話框 (⌃G) 和其他快速輸入框現在可與螢幕閱讀器更好地運作。

### 螢幕閱讀器的轉向指示器

當您在回應串流時傳送轉向訊息時，螢幕閱讀器使用者現在會收到 `aria-status` 宣佈。

### 無障礙技能

新的內建無障礙技能可協助確保新功能包含適當的無障礙支援。

### 聊天中的核對記號

**設定**：`accessibility.chat.showCheckmarks`

工具呼叫和可摺疊片段前的核對記號現在預設會被移除。`accessibility.chat.showCheckmarks` 設定將重新啟用核對記號。

---

## 編輯器體驗

### 模態編輯器（實驗性）

**設定**：`workbench.editor.useModal`、`extensions.allowOpenInModalEditor`

我們正在試驗新的模態編輯器體驗，適用於您通常會短暫開啟然後返回到您的作用中任務的編輯器。模態編輯器浮動在編輯器上方，不影響編輯器索引標籤的配置。

模態體驗適用於：設定、鍵盤快速鍵、設定檔管理、AI 和語言模型管理、工作區信任管理。

將 `workbench.editor.useModal` 設為 `some` 以選擇加入此體驗。

另一個設定 `extensions.allowOpenInModalEditor` 可將模態編輯器的使用擴展到擴充功能。

### 可設定的通知位置

**設定**：`workbench.notifications.position`

您現在可以將通知的位置設定為 `top-right`、`bottom-right` 或 `bottom-left`。預設值仍為 `bottom-right`。

### 設定編輯器清理

我們已將 VS Code 聊天設定移至設定編輯器中的自有頂層項目，附帶子類別。顯示的設定清單也會限制在所選的目錄項目。實驗性設定已移至每個區段的結尾。

---

## 程式碼編輯

### 遠距離下一個編輯建議

下一個編輯建議 (NES) 透過建議編輯不僅在您的游標處，還在附近的地方，來擴展幽靈文字。我們一直在使用遠距離下一個編輯建議來推進此體驗，這將 NES 擴展為預測和建議檔案中任何地方的編輯，而不僅是在您目前的游標位置附近。

確保您已啟用 NES (`github.copilot.nextEditSuggestions.enabled`) 和延伸的 NES 範圍 (`github.copilot.nextEditSuggestions.extendedRange`)。

### NES 急迫性

Copilot 狀態列項目現在包含下一個編輯建議的急迫性選項。此選項讓您可以選擇取得可能不太相關的更多建議，或取得更可能有用的較少建議。

---

## 原始檔控制

**設定**：`git.addAICoAuthor`

VS Code 可以在您提交包含 AI 產生貢獻的程式碼時自動附加 `Co-authored-by:` 尾部。此外，Git 責難懸停工具提示現在顯示來自提交尾部的共同作者。

使用下列其中一個值來設定 `git.addAICoAuthor`：
* `off`（預設）：不新增共同作者尾部
* `chatAndAgent`：為使用 Copilot Chat 或代理程式模式產生的程式碼新增尾部
* `all`：為所有 AI 產生的程式碼新增尾部，包括內聯完成

---

## 偵錯

### JavaScript 偵錯工具

#### 自訂屬性取代

如果物件具有使用 `Symbol.for('debug.properties')` 定義的方法，則這些屬性會根據預設顯示在偵錯工具中。

#### 模擬焦點視窗和事件接聽程式中斷點

之前，在偵錯瀏覽器時，您可以在 **Event Listener Breakpoints** 檢視中設定中斷點。我們已將此檢視重新命名為 **Browser Options**。

我們新增了額外的選項以 **Emulate a focused page**。勾選此項後，將焦點移出瀏覽器視窗將不再導致瀏覽器元素失去焦點。

---

## 終端機

### Kitty 圖形協定

**設定**：`terminal.integrated.enableImages`、`terminal.integrated.gpuAcceleration`、`terminal.integrated.windowsUseConptyDll`

VS Code 終端機現在支援 Kitty 圖形協定，能夠直接在終端機中呈現高保真影像。支援此協定的程式可以傳輸和顯示具有豐富一組功能的影像：

* **影像格式**：PNG、24 位 RGB 和 32 位 RGBA
* **顯示配置**：將影像調整為特定欄/列尺寸、裁剪來源區域、套用子儲存格像素偏移，並控制 z 索引堆疊順序
* **傳輸**：直接內聯 base64，並附上分塊傳輸和 zlib 壓縮支援
* **影像管理**：一步傳輸和顯示、儲存影像並稍後以不同位置放置、依 ID 或全部刪除，以及重新傳輸以更新現有影像
* **游標控制**：選擇游標是否在呈現影像後超過該影像或保持在原位
* **終端機整合**：影像與文字一起捲動，並在終端機重設或清除時適當清理

若要啟用影像呈現，請將 `terminal.integrated.enableImages` 設為 `true`，並確保 `terminal.integrated.gpuAcceleration` 設為 `on` 或 `auto`。在 Windows 上，您也需要啟用 `terminal.integrated.windowsUseConptyDll`。

可以使用像 kitten icat (macOS/Linux) 或 VT CLI 等工具在終端機中顯示影像。

> **注意**：某些 Kitty 圖形協定功能尚未支援，包括動畫、相對放置、Unicode 預留位置和檔案型傳輸。

### 外部終端機的 Ghostty 支援

**設定**：`terminal.external.osxExec`、`terminal.external.linuxExec`

Ghostty 現在在 macOS 和 Linux 上支援作為外部終端機。

### 外部終端機的工作區資料夾選擇

當您在多根工作區中開啟外部終端機時，VS Code 現在會提示您選擇工作區資料夾。

### 終端機沙箱改進（預覽）

**設定**：`chat.tools.terminal.sandbox.enabled`、`chat.tools.terminal.sandbox.linuxFileSystem`、`chat.tools.terminal.sandbox.macFileSystem`、`chat.tools.terminal.sandbox.network`

信任的網域現在可以選擇用於網路隔離。在 macOS 上不需要安裝即可啟用終端機沙箱，在 Linux 上您無需安裝 ripgrep 即可啟用。

---

## 語言

### 統一的 JavaScript 和 TypeScript 設定

為了為即將推出的 TypeScript 6.0 和 7.0 版本做準備，我們已整合並清理了內建的 JavaScript 和 TypeScript 設定 ID。現在所有這些設定已在 `js/ts.*` 前置詞下移動。舊的 `javascript.*` 和 `typescript.*` 設定仍然有效，但現在已標示為已棄用。

### Python

#### Python 環境擴充功能推出給所有使用者

Python 環境擴充功能在預覽中一年後現在推出給所有使用者。主要功能包括：快速建立、Python 專案、uv 整合、內建套件管理、可攜式設定。

---

## 對擴充功能的貢獻

### GitHub Pull Requests

新功能包括：多個拉取要求和問題描述可以同時開啟，設定 `githubPullRequests.autoRepositoryDetection` 可以設為 `true` 以包含工作區外的存放庫，不含相符問題的存放庫現在會在「問題」檢視中隱藏。

---

## 擴充功能編寫

### Webview 和自訂編輯器現在可以使用 ThemeIcons 作其圖示路徑

Webview 面板和自訂編輯器現在可以使用 ThemeIcon 作其編輯器索引標籤圖示。

### 可攜式模式偵測 API 最終化

`env.isAppPortable` API 現在是穩定的，可供所有擴充功能使用。

---

## 提議的 API

### 聊天項目控制項 API

我們繼續改進聊天工作階段 API。值得注意的變更包括 `ChatSessionItemControllerNewItemHandler` 和 `ChatSessionProviderOptions.newSessionOptions`。

---

## 工程

### VS Code 工程的 TypeScript-Go

我們將 `vscode` 工作區預設為使用 TSGo 進行開發。我們現在也使用 TypeScript-Go (tsgo) 在開發期間編譯 VS Code 的內建擴充功能。

### 使用 esbuild 的擴充功能組合

我們已將大多數內建擴充功能遷移為使用 esbuild 而不是 webpack 進行組合。

---

## 已棄用的功能和設定

### 此版本中的新棄用功能

* **編輯模式** 自 VS Code 版本 1.110 起正式棄用。使用者可以透過 VS Code 設定 `chat.editMode.hidden` 暫時重新啟用編輯模式。此設定將在版本 1.125 之前保持支援。從版本 1.125 開始，編輯模式將完全移除，且無法再透過設定啟用。

### 即將棄用

無

---

## 值得注意的修正

* vscode#251722：對擴充功能提供的樹狀檢視項目進行內聯動作，即使在可見的捲動區域內也可以執行，即使 `"workbench.list.horizontalScrolling": true`

---

## 感謝

### 問題追蹤貢獻

對我們問題追蹤的貢獻：

* @gjsjohnmurray (John Murray)
* @RedCMD (RedCMD)
* @IllusionMH (Andrii Dieiev)
* @tamuratak (Takashi Tamura)
* @robotsnh (robotsnh)

對 `vscode` 的貢獻：

* @a-stewart (Anthony Stewart)：修正格式文件命令導致未知詳細原因的錯誤 PR #288934
* @accesswatch (Jeff Bishop)
  * 修正：改進快速輸入對螢幕閱讀器的無障礙支援 PR #292339
  * 修正（無障礙支援）：在尋找微件中新增 ARIA 提示並修正虛假宣佈 PR #292376
  * 功能（無障礙支援）：為尋找/篩選對話框新增無障礙協助系統 PR #292373
* @aturzone (ATUR)：修正/資源洩漏 osreleaseinfo PR #293027
* @EmrecanKaracayir (Emrecan Karaçayır)
  * 在終端機內聯聊天中使用 inlineChat.border PR #293116
  * 修正代理程式狀態徽章的不一致著色 PR #293224
* @erezak (Erez Korn)：恢復統一快速存取前置詞切換 PR #292203
* @gjsjohnmurray (John Murray)
  * 防止 symbol-* codicons 在工具列上顯示彩色（修正 #267766）PR #267787
  * 比較 cwd 和 userHome 時標準化 Windows 磁碟機代號（修正 #293049）PR #293065
* @hkleungai (Jimmy Leung)：vscode-dts：新增 LineCommentConfig 介面並更新 lineComment PR #289457
* @jainampatel27 (Jainam Patel)
  * 修正：更正 debug.ts 中 nls.localize 字串中的拼字錯誤 PR #296730
  * 修正擴充功能啟用事件中 nls.localize 字串中的拼字錯誤 PR #297378
* @JeffreyCA：更新 Azure Developer CLI (azd) 的 Fig 規格 PR #292894
* @murataslan1 (Murat Aslan)：功能（測試）：在測試執行時在活動列上顯示執行徽章 PR #292257
* @n-gist (n-gist)：修正診斷未從問題比對器重新推送到 markerServ PR #292109
* @na3shkw (Naoto Ishikawa)：修正：在輸入變數對話框上按 ESC 時取消偵錯啟動 PR #293837
* @prasanthpul (Prasanth Pulavarthi)：A/B 實驗：關閉按鈕與登入對話框上的「暫時跳過」PR #295867
* @RedCMD (RedCMD)：修正：字串包含逃逸字元時字串字面值的選擇 PR #295302
* @remcohaszing (Remco Haszing)：啟用 npm 指令碼 PR #283432
* @renan-r-santos (Renan Santos)：修正遠端終端機環境變數集合使用錯誤的工作區範圍 PR #293628
* @sam-shubham (Sam Shubham)：右對齊動作樹狀檢視 PR #295266
* @SimonSiefke (Simon Siefke)：修正：隧道檢視中的記憶體洩漏 PR #287142
* @SongXiaoXi (SXX)：修正：停止無界 websocket inflate-byte 記錄 PR #293819
* @tamuratak (Takashi Tamura)
  * 修正 markdown 呈現邏輯中的最終答案偵測 PR #293746
  * 聊天：使用釘選邏輯和重新位置化增強最終回應呈現 PR #293597
* @Vedag812 (Vedant Agarwal)：修正：在可存取檢視導覽提示中新增遺失的結尾 '>' PR #295412

對 `vscode-copilot-chat` 的貢獻：

* @24anisha (Anisha Agarwal)
  * 更新設定以變更 agentic proxy 搜尋端點的名稱 PR #3672
  * 搜尋子代理程式 -> 微調模型飛行測試 PR #3864
  * 修正 settings.json 以移除 agentic proxy 設定 PR #4006
* @aashna (Aashna Garg)：將 Copilot auth 權杖新增至 CAPI proxy auth 的路由器決策提取器 PR #3980
* @alexweininger (Alex Weininger)
  * 遷移 Copilot CLI 整合 PR #3529
  * 僅在設定已啟用時啟用 copilotCLI.addFileReference 命令 PR #3593
  * 在 ide 目錄中建立鎖定檔案 PR #3583
  * 新增工作區信任以鎖定檔案 PR #3602
* @ashatabak786：將提示 A 的凍結提示新增至 0129 的 VSC 聊天模型 PR #3452
* @bharatvansh (Ayush Singh)：功能：將 /summarize 命令新增至代理程式模式 PR #3352
* @bstee615 (Benjamin Steenhoek)
  * 適應性積極性更新修正 PR #3441
  * 實作 xtab275Aggressiveness 提示 PR #3524
* @dennyac (Denny Abraham Cheriyan)：將 vscodeRequestId 新增至 panel_request 事件 PR #4007
* @devm33 (Devraj Mehta)：將 VS Code clientName 新增至 Copilot SDK 工作階段 PR #3449
* @FAStre：在影像提示中包含檔案路徑 PR #3790
* @IanMatthewHuff (Ian Huff)：新增 1p repo 遙測資訊比較提交的最大日期 PR #3774
* @lsby：修正工具呼叫偵測和 Ollama 模型支援 PR #3566
* @MRayermannMSFT (Matthew Rayermann)
  * Copilot CLI：修正初始化連線 PR #3618
  * Copilot CLI：從工作階段選擇發送參考時 PR #3619
  * Copilot CLI：用戶端中斷連線時關閉 Diff PR #3626
  * Copilot CLI：透過 MCP 呼叫從 CLI 接收工作階段名稱 PR #3638
  * Copilot CLI：透過命令/內容功能表發送選擇 PR #3668
* @Sid200026 (Siddharth Singha Roy)：群組：新增自動模式路由後備的遙測事件 PR #3780
* @spboyer (Shayne Boyer)：功能：改進代理程式自訂技能 — 中等 → 高合規性 PR #3866
* @zelinms (Zeqi Lin)：修正：更正回應 API 輸出的正確遙測回應 PR #3733

對 `vscode-css-languageservice` 的貢獻：

* @Arecsu (Alejandro Romano)：修正 `@scope` 剖析以支援選擇器清單 PR #474
* @ej-shafran (ej shafran)
  * 適當地剖析 `@container` 查詢 PR #473
  * 支援新 CSS `if()` PR #472

對 `vscode-js-debug` 的貢獻：

* @igorlfs (Igor Lacerda)
  * 群組：不建議 neovim 的過時擴充功能 PR #2314
  * 修正：將模組範圍標記為昂貴 PR #2312

對 `vscode-json-languageservice` 的貢獻：

* @Legend-Master (Tony)：只在必要時逃逸到 `&amp;nbsp;` PR #309
* @nsajko (Neven Sajko)：修正 `additionalProperties` JSON Schema 屬性描述中的拼字錯誤 PR #310

對 `vscode-languageserver-node` 的貢獻：

* @andrewbraxton (Andrew Braxton)：為元模型中的 incomingCalls 和 outgoingCalls 新增功能 PR #1720

對 `vscode-pull-request-github` 的貢獻：

* @gvilums (Georgijs)：修正平面檔案配置的 PR 樹狀揭露錯誤 PR #8522

對 `vscode-python-debugger` 的貢獻：

* @renan-r-santos (Renan Santos)：修正：使用 `run.executable` 代替 `activatedRun.executable` 進行解譯器識別 PR #949

對 `vscode-python-environments` 的貢獻：

* @qq157755587 (Zhao Yuanjie)：修正 defaultInterpreterPath 變數擴展並穩定解譯器選擇測試 PR #1234
* @StellaHuang95 (Stella Huang)：修正當終端機環境變數從 .env 檔案中註解或刪除時未被移除的問題 PR #1131

對 `vscode-test` 的貢獻：

* @DanTup (Danny Tuppeny)：允許測試輸出的自訂 stdout/stderr 串流 PR #324

對 `debug-adapter-protocol` 的貢獻：

* @Be-ing (Be)：將 Kate 新增至實作者清單 PR #589

對 `language-server-protocol` 的貢獻：

* @orien (Orien Madgwick)：新增 Pony 語言伺服器 PR #2229
* @SeanDictionary (SeanDictionary)：新增 SageMath 語言伺服器 PR #2231
* @stefanvanburen (Stefan VanBuren)：新增 Buf 語言伺服器 PR #2225

---

## 術語對照表

| 英文 | 繁體中文 | 備註 |
|-----|--------|-----|
| Agent / Agentic | 代理程式 / 代理式 | 指 AI 代理程式 |
| Browser tools | 瀏覽器工具 | |
| Chat | 聊天 | |
| Compaction | 壓縮 | 指上下文壓縮 |
| Debug panel | 偵錯面板 | |
| Edit mode | 編輯模式 | |
| Extension | 擴充功能 | |
| Fork | 分支 | 指分支工作階段 |
| Geist | Geist | 字體名稱，保留原文 |
| Ghostty | Ghostty | 終端機名稱，保留原文 |
| Hover mode | 懸停模式 | |
| Inline chat | 內聯聊天 | |
| Kitty graphics protocol | Kitty 圖形協定 | |
| Language model | 語言模型 | |
| LSP | LSP | 語言伺服器協定 |
| Modal editor | 模態編輯器 | |
| Next edit suggestions (NES) | 下一個編輯建議 (NES) | |
| Plan | 計畫 | |
| Plugin | 外掛程式 | |
| Playwright | Playwright | 測試框架名稱，保留原文 |
| Quick input | 快速輸入 | |
| Sandbox | 沙箱 | |
| Screen reader | 螢幕閱讀器 | |
| Session | 工作階段 | |
| Skill | 技能 | |
| Slash command | 斜線命令 | |
| Subagent | 子代理程式 | |
| Symbol.for() | Symbol.for() | JavaScript API，保留原文 |
| Terminal | 終端機 | |
| TypeScript-Go (TSGo) | TypeScript-Go (TSGo) | 開發工具名稱 |
| Unified | 統一 | |
| WebView | Webview | |
| YOLO mode | YOLO 模式 | |
| esbuild | esbuild | 打包工具名稱 |
| webpack | webpack | 打包工具名稱 |

---

**原始文本來源**：https://code.visualstudio.com/updates/v1_110

*翻譯日期：2026 年 4 月 30 日*
