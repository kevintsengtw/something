# VS Code 1.140 重點摘要

- 版本：1.140.0（Stable）
- 發行日期：2026 年 9 月 30 日
- 原文連結：<https://code.visualstudio.com/updates/v1_140>

## 版本概觀

本版擴充代理程式工作流程、改善 worktree 的重複使用，並新增企業 AI 控管。官方列出的六項亮點為：Copilot harness、多資料夾工作階段（實驗性）、遠端委派（實驗性）、HydraFusion（研究預覽）、共用 worktree 資料夾（實驗性）、企業控管。

---

## 一、代理程式（Agents）

### 1. Copilot harness

- 由 Copilot SDK 驅動，行為與功能和其他 Copilot 產品一致，包括獨立的 GitHub Copilot 應用程式與 Copilot CLI；同時維持熟悉的工作方式。
- 啟用方式：在聊天輸入框的 harness 選擇器中選取；本版中它可能已經是預設選項。
- 架構：在以 Agent Host Protocol（AHP）為基礎的專屬 agent host 處理程序中執行，可以從多個 VS Code 視窗連線到同一個代理程式工作階段。

### 2. HydraFusion 模型協調（研究預覽）

- 自適應的模型協調系統，為每個程式設計工作選擇模型與工作流程。
- 處理方式有三種：用單一模型解決、升級到更強的模型、讓另一個模型評論並修改結果。
- 目標：兼顧速度與成本的同時提升結果品質，不需使用者自行協調模型。
- 限制：已啟用預覽功能的合格使用者，才能在模型選擇器中選取。

### 3. 多資料夾工作階段（實驗性）

- 以往多聊天工作階段中的所有聊天共用同一個資料夾與 checkout；本版起每個聊天可以使用自己的資料夾或 worktree，變更不會在聊天之間互相滲漏。
- 每個聊天的終端機、工作、變更、提取要求與 Agent merge 狀態都依其資料夾分開；使用相同資料夾的聊天則共用這些狀態。
- 工作階段清單：停留在工作階段上可看到所有資料夾的摘要；停留在巢狀聊天上只看到該聊天資料夾的細節。
- 使用情境：
  - 跨儲存庫實作功能：主聊天處理一個儲存庫，再為第二個儲存庫建立對等聊天。
  - 在不同 worktree 比較做法：主聊天建立多個對等聊天，各自使用同一儲存庫的全新 worktree，各有自己的分支、變更與提取要求。
- 設定（預設停用，不會顯示在設定編輯器中，需在使用者範圍的 `settings.json` 設為 `true` 後建立工作階段）：
  - Copilot：`chat.agentHost.copilotAgent.multiRootEnabled`
  - Claude：`chat.agentHost.claudeAgent.multiRootEnabled`
  - Codex：`chat.agentHost.codexAgent.multiRootEnabled`
- 新的工作階段直接套用設定變更，不需重新啟動 Agent Host。
- 限制：沒有新增資料夾或選擇對等聊天資料夾的 UI，必須請主聊天建立對等聊天，並描述它應使用的儲存庫或 worktree。

### 4. 封存工作階段中的聊天

- 選取 **Mark as Done** 只封存已完成的那個對等聊天，不會封存整個工作階段，其餘部分持續運作。
- 還原方式：在工作階段清單選取 **Done** 篩選條件，標題與對話紀錄完整保留。

### 5. 自動清理 Dev Container

- 設定：`chat.agentHost.devContainer.enabled`、`chat.agentHost.devContainer.idleTimeout`（僅限 Agents 視窗）。
- 所有使用某個 Dev Container 的工作階段都閒置五分鐘後，VS Code 會停止該容器；繼續工作時自動重新啟動。
- 將最後一個工作階段標記為完成或刪除時，會移除該容器。

### 6. 更快的 Dev Container 啟動速度

- 設定：`chat.agentHost.devContainer.enabled`（僅限 Agents 視窗）。
- Dev Container 工作階段讓代理程式在隔離環境中使用專案的工具與相依套件。
- 全新容器可重複使用已快取的 VS Code Server 與 CLI 下載內容，縮短設定時間並減少跨專案的重複下載。

### 7. 為委派的工作選擇工作階段關係與工作區

- 委派工作時要做兩個獨立決定：先決定工作是否屬於目前工作階段的計畫或交付成果；再決定是否需要工作區（只有需要儲存庫檔案時才指定），以及是否需要 worktree（只有檔案變更需要隔離時才要求）。相關工作仍可使用不同的工作區。
- 為相關工作建立對等聊天：要重複使用目前工作區與 checkout 時需在提示中說明；因為共用 checkout，適合研究、規劃或不需隔離檔案變更的相關工作。
- 為無關工作建立獨立工作階段：適用於不需要儲存庫檔案的另一項交付成果，新的工作階段不會繼承來源資料夾；若需要儲存庫，請指名專案或資料夾，需要隔離變更時明確要求 worktree。
- 原文提供兩則範例提示（審查驗證流程且不修改檔案、為另一專案研究授權選項）。

### 8. 在遠端 agent host 上建立無工作區的聊天

- 設定：`sessions.chat.unifiedWorkspacePicker.enabled`（僅限 Agents 視窗）。
- 新增在遠端主機上執行無工作區聊天的支援：從統一工作區選擇器選取已連線的主機，不必選擇工作區。
- **Chat** 項目會標示所選主機，開始前可確認工作階段的執行位置。
- 遠端資料夾現在與本機資料夾一起顯示在主選擇器中，不必開啟 **Remote** 子選單。

### 9. 將工作委派給遠端 agent host（實驗性）

- 設定：`chat.remoteAgentHostsEnabled`、`chat.remoteSessions.tools.enabled`（僅限 Agents 視窗）；工具預設關閉，兩項設定都要啟用並連線要使用的主機。
- 代理程式可從 Agents 視窗委派工作，不需使用者逐項在選擇器中選主機。
- 新的內建工具：
  - `list_agent_hosts`：探索主機、模型、資源容量與工作階段負載。
  - `create_remote_session`：啟動工作階段；可指定主機，或依作業系統（Windows、Linux、macOS）、最低記憶體、邏輯 CPU 數量及選用模型自動配置；符合條件時優先選擇執行中與待建立工作階段最少的主機。
  - `get_remote_session`：檢查遠端工作階段的狀態與最新回應。
  - `send_remote_message`：傳送後續工作，或將結果與問題回報給發起的聊天。
- 原文範例：要求在至少 16 GiB 記憶體、八個邏輯 CPU 的 Linux 主機上啟動工作階段，檢查 Node.js 與 Python 版本並回報。
- 限制與注意事項：
  - 未指定時工作階段沒有工作區；儲存庫工作需使用目標主機上既有的受信任資料夾（直接使用或在新的 Git worktree 中使用）。
  - 工具不會 clone 或複製發起端的工作區，一般核准流程仍適用。
  - 負責協調的 Agents 視窗必須保持開啟並連線，訊息才能傳遞。
  - 遠端代理程式透過 `send_remote_message` 回報，最終回答不會自動轉送。

### 10. 個人化新工作階段歡迎語（實驗性）

- 設定：`sessions.chat.experimental.welcomePhrases`、`sessions.chat.experimental.welcomeName`、`accessibility.verbosity.newSessionWelcome`（僅限 Agents 視窗）。
- 在 Agents 視窗建立新工作階段時顯示選用的歡迎標題，從五種語句中擇一顯示（例如 **What are we building?**、**Let's ship something**），可附加 GitHub 個人檔案中的名字。
- 預設關閉；啟用後可從標題、內容功能表或命令選擇區使用 **Set Welcome Name** 改名，名字跨裝置同步。
- 清除名字後：有 GitHub 名字就使用它，個人檔案沒有名字則顯示通用語句。
- 螢幕助讀程式最佳化模式下，撰寫區出現時會朗讀一次標題；不想聽到可停用 `accessibility.verbosity.newSessionWelcome`。

### 11. 改良的工作階段撰寫區控制項（實驗性）

- 設定：`sessions.chat.experimental.newSessionComposerLayout`、`sessions.chat.unifiedWorkspacePicker.enabled`（僅限 Agents 視窗）；兩項都啟用才能使用新版面。
- 新版面把工作區、分支、worktree 與 harness 控制項集中在聊天輸入框上方，減少在視窗各區域之間移動。
- 鍵盤操作（只需統一工作區選擇器，不需新版面）：
  - Ctrl+K Ctrl+F 聚焦工作區選擇器；Ctrl+K Ctrl+H 聚焦 harness 選擇器。
  - 停留提示顯示已設定的按鍵繫結，各選擇器的內容功能表提供 **Configure Keybinding**。
- 分支選擇器：
  - 建立新的 worktree 工作階段時，預設為目前分支的上游分支（例如 `origin/main`）。
  - 建立 worktree 前會自動嘗試 fetch 所選的遠端分支，從最新遠端狀態開始。
  - 每個工作區只載入一次分支並在本機篩選，大幅降低搜尋延遲，大型儲存庫尤其明顯。

### 12. 優先查看最近完成的工作階段

- Agents 視窗的 **Done** 區段改依標記完成的時間排序，最近完成的排最前面。
- 切換 **Created** 與 **Updated** 排序、重新載入 VS Code 後，順序維持不變。

### 13. 更可靠的代理程式協調（實驗性）

- 設定：`chat.agentHost.agentOrchestrationLimits`，預設 `on`；變更不需重新啟動 Agent Host；設為 `off` 可移除協調上限。
- 提高整個處理程序範圍內，代理程式建立的工作階段、聊天、工作階段間訊息與遞迴建立工作階段的上限。
- 範例：代理程式分類 CI 失敗、把每項修正委派給不同工作階段並加以協調，不會在完成前用盡訊息上限。
- 達到上限時只會阻擋新的協調動作，不中斷執行中的工作；確認要求與輸入驗證仍然適用。

### 14. 透過 URL 在 Agents 視窗中開啟新工作階段

- URL 格式：`agents/new` 路徑，加上選用的 `prompt` 與 `workspace` 查詢參數。
- 提示會解碼後放入 **New Session** 撰寫區，不會自動送出。
- `workspace` 參數需填入 URL 編碼的資料夾 URI；省略時草稿使用 **No Workspace**。
- 已有內容的撰寫區維持不變，開啟連結不會覆寫進行中的工作。
- VS Code Insiders 要使用 `vscode-insiders://` 取代 `vscode://`。

---

## 二、聊天（Chat）

### 1. 代理程式回應的持續進度顯示（實驗性）

- 設定：`chat.experimental.persistentProgress`、`chat.experimental.persistentProgressVerbosity`。
- 啟用後，聊天底部持續顯示進度指示器，直到回應結束。
- 推理內容以獨立的可摺疊預覽呈現，工具呼叫執行期間保持可見。
- 詳細程度：**Compact** 會在回應文字繼續輸出或回應結束時，把已完成的工具群組摺疊成可展開摘要；**Verbose** 保留完整工具呼叫細節。
- `chat.experimental.persistentProgress` 可關閉指示器，或選擇彩色或單色的 Draw 動畫；Insiders 預設為 **Draw**，Stable 逐步推出中。

### 2. 終端機輸出重排

- 設定：`chat.tools.terminal.outputReflow`。
- 自 1.132 起聊天中的終端機輸出會依預覽寬度重排；新設定讓使用者控制此行為。
- 停用後輸出維持固定寬度，以水平捲動閱讀長行；預設仍為啟用。

### 3. 在編輯器視窗中使用多個聊天

- 以往主聊天與巢狀對等聊天只出現在 Agents 視窗。
- 現在編輯器視窗的 **Sessions** 檢視（位於 **Chat** 檢視旁）會列出主聊天，對等聊天巢狀顯示在下方；選取任一列即在 **Chat** 檢視開啟。

---

## 三、MCP

### 將 MCP 伺服器加入可攜式設定檔

- 可從 **MCP: Add Server** 流程將伺服器加入可攜式設定檔，在相容的 Copilot 工具間共用設定，不必手動編輯 JSON。
- 全域伺服器：選擇 **Copilot Global**，儲存到 `$COPILOT_HOME/mcp-config.json`；未設定 `COPILOT_HOME` 時儲存到 `~/.copilot/mcp-config.json`。原有的使用者設定在畫面中標示為已淘汰。
- 工作區伺服器：選擇 `.mcp.json`，儲存在工作區根目錄。原有的 `.vscode/mcp.json` 在畫面中標示為已淘汰。

---

## 四、協助工具（Accessibility）

### 聽見彩帶出現

- 設定：`accessibility.signals.confetti`。
- 新的 **Confetti** 訊號，讓看不到動畫的使用者也能獲得慶祝回饋。
- 觸發時機：聊天中按讚的彩帶出現時，以及在 Agents 視窗成功將工作階段標記為完成時。

---

## 五、原始檔控制（Source Control）

### 在 worktree 之間重複使用被忽略的資料夾（實驗性）

- 設定：`git.worktreeSymlinkFolders`，以 `.gitignore` 風格的模式指定被忽略的資料夾（例如 `node_modules`）。
- VS Code 建立新的 worktree 時（包含為代理程式工作階段建立），會為目前 checkout 中符合模式的資料夾建立符號連結。
- 效果：避免在每個 worktree 重新安裝相依套件或複製大型建置產物。

---

## 六、程式碼編輯（Code Editing）

### 控制選取文字比對大小寫敏感度的新選項

- 設定：`editor.selectedTextMatchMode`。
- `findOptions`（預設）：與尋找小工具連動，與先前行為相同。
- `caseSensitive`：區分大小寫比對選取範圍。
- `caseInsensitive`：不區分大小寫比對選取範圍。

---

## 七、終端機（Terminal）

### macOS 上更清晰的終端機文字（實驗性）

- 設定：`terminal.integrated.fontRendering`。
- 在 macOS 高 DPI 顯示器上，終端機文字可能比其他終端機的相同字型柔和；設為 `grayscale` 可讓文字更清晰。

---

## 八、企業（Enterprise）

### 1. 說明最低版本需求

- 管理員可要求開發人員更新 VS Code 後才能使用 AI 功能（例如為了採用較新的沙箱防護）。
- 不再顯示 **Update Required** 對話方塊，改在 AI 功能無法使用處說明：
  - Chat：顯示所需版本與已安裝版本，並提供更新動作。
  - 編輯器視窗：Chat 關閉時也顯示橫幅，其他編輯器功能仍可使用。
  - Agents 視窗：顯示阻擋式覆蓋層，提供 **Open Editor Window** 動作。
- 有可用更新時提供對應的 **Check for Updates**、**Download Update**、**Install Update** 或 **Restart to Update**；組織以原則停用內建更新時，改為請使用者聯絡管理員。
- 符合需求後通知自動清除；強制執行方式沒有改變。

### 2. 設定預設的 Auto 層級

- Auto 模型有三種層級：效率、平衡、智慧。
- 管理員以 `autoTier` 受管理設定指定預設層級，可接受值為 `efficiency`、`balance`、`intelligence`。
- 套用範圍：同一台電腦上 Local harness 與 Copilot agent host 中的新聊天；在模型選擇器中顯示為 **Default**。
- 受管理層級只是起點不是限制，開發人員仍可選其他層級；受管理層級變更或移除時，VS Code 會保留明確做出的選擇與已還原的選擇。

### 3. 在 OpenTelemetry 中擷取使用者身分

- 設定：`github.copilot.chat.otel.captureIdentity`，預設關閉，且與內容擷取彼此獨立。
- 啟用後 Local 聊天工作階段加入的屬性：
  - 代理程式呼叫 span 上的 `user.name`（包含子代理程式與內嵌聊天）。
  - 資源屬性 `process.user.name` 與 `host.name`。
- 管理員控制：`telemetry.capture.identity` 受管理設定（`CopilotOtelCaptureIdentity` 原則），優先於 `COPILOT_OTEL_CAPTURE_IDENTITY` 與使用者設定。
- 原則拒絕擷取時，VS Code 不需重新載入即從後續匯出中移除身分資訊，包括明確設為資源屬性的身分屬性。
- 受管理遙測設定的解析方式變更：
  - 只套用優先順序最高的傳遞管道中的 `telemetry` 區塊（原生 MDM、伺服器、檔案依序），省略的欄位不再由較低優先順序的管道補上。
  - 受管理的 `telemetry.resourceAttributes` 現在優先於 `OTEL_RESOURCE_ATTRIBUTES`。
- 限制：身分擷取目前僅適用於 Local harness，agent host 的支援在 #337413 中追蹤。

---

## 九、社群媒體

- VS Code 開設 Instagram 帳號 @vscode.ig，發布更新、新功能與使用技巧。

---

## 十、提議的 API（Proposed APIs）

### 驗證工作階段上的授權伺服器

- 1.101 為 MCP 引入的 `authIssuers` 提議，讓擴充功能指名要進行驗證的 OAuth 授權伺服器；本版新增反方向資訊：`AuthenticationSession` 新增選用的唯讀屬性 `authorizationServer?: Uri`，標示簽發該工作階段的授權伺服器。
- 用途：擴充功能與同一服務的多個部署溝通時（例如公開 GitHub 與 GitHub Enterprise 主機），可將每組認證與其簽發時的目的地配對，不必讀取使用者可獨立變更的額外設定，避免有效權杖被送到錯誤的主機。
- 注意事項：
  - 此值識別的是 OAuth 簽發者，不是 API 端點，需自行對應到自己的端點。
  - 內建 GitHub 提供者對公開 GitHub 與企業工作階段都會填入此值，有此值不代表就是企業工作階段。
- 回饋管道：API 提議 issue #248775。

---

## 十一、已淘汰的功能與設定

- 無。

---

## 十二、感謝

- 對 `vscode` 的貢獻：
  - na2co3（@na2co3-ftw）：1 個 PR，修正 Modern UI 中已連接編輯器索引標籤多餘的索引標籤動作淡出效果。
  - Simon Siefke（@SimonSiefke）：29 個 PR，其中 28 個修正記憶體流失（涵蓋終端機、語意權杖、搜尋編輯器、聊天、筆記本、擴充主機、整合式瀏覽器、SCM、多重差異編輯器、測試觀察器、註解討論串等），另 1 個為終端機在處置時取消不必要的遙測逾時。
- 對問題追蹤的貢獻：John Murray、RedCMD、Andrii Dieiev、Alberto Santini。

---

資料來源：<https://code.visualstudio.com/updates/v1_140>
