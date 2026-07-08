# Visual Studio Code 1.128

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 7 月 8 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.128.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.128.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.128.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.128.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.128.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.128.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.128.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.128.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.128.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.128 版本。本次發行帶來更豐富的多聊天 Agent 工作階段、正式可用的 Chat 圖片支援，以及作業系統層級的鍵盤快速鍵。

- [**多聊天 Agent 工作階段**](#工作階段中的多聊天現在支援-claude-agent)：在一個 Claude 工作階段中執行多個相關聊天，以比較不同做法並並行工作。
- [**快速聊天**](#agents-視窗中無需選取工作區的聊天)：在 Agents 視窗中不需先開啟工作區即可提問。
- [**Copilot Vision**](#copilot-vision-正式可用)：透過貼上、拖曳或放置將圖片和 PDF 附加至 Chat，現已正式可用。
- [**瀏覽器分頁配置**](#可設定的整合式瀏覽器分頁配置)：選擇整合式瀏覽器分頁開啟的位置：使用中的群組、專用側邊群組或獨立視窗。
- [**作業系統層級鍵盤快速鍵**](#作業系統層級鍵盤快速鍵)：使用即使 VS Code 不在焦點也能運作的按鍵繫結觸發 VS Code 命令。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### 工作階段中的多聊天現在支援 Claude Agent

[Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)中的 Claude agent host 工作階段提供由 Anthropic 的 Claude Agent SDK 驅動的 Agent 編碼能力，直接在 VS Code 中使用。多聊天將相關的對話執行緒保留在一個工作階段中，而非分散在不同的頂層工作階段。

單聊天 Claude 工作階段仍然是與 Agent 合作的專注方式。透過多聊天，一個工作階段可以包含相關聊天，讓您可以比較不同做法、從較早的回合分支，並且並行執行工作。您可以新增聊天、從現有回合分支聊天、在對等聊天之間切換，以及同時發送回合。每個聊天保持自己的歷史紀錄、標題和模型選取，並在重啟後隨父工作階段一同還原。對等聊天歸類在 Claude 工作階段之下，不會顯示為獨立的頂層工作階段。

以下影片展示包含多個聊天的單一 Claude 工作階段：主要聊天為 Express 應用程式新增 `/health` 端點，同時一個對等聊天並行為其撰寫測試，而一個分支聊天探索替代實作。每個聊天獨立執行，並保持歸類在同一工作階段之下。

### Agents 視窗中無需選取工作區的聊天

Agents 視窗為您提供一個專用空間來建立、恢復和管理 Agent 工作階段。對於專案工作，這些工作階段將聊天、檔案和變更與工作區保持在一起。

對於不綁定資料夾的問題，您現在可以在 Agents 視窗中不選取工作區直接開始聊天。這些聊天出現在 **Chats** 區段中，並以聚焦且可立即輸入的狀態開啟。使用未指派的按鈕或 **Chats** 區段標題上的加號按鈕開始快速聊天。

由於快速聊天沒有工作區，因此不會顯示工作區專屬的 **Changes** 或 **Files** 側邊窗格。快速聊天在重新載入後與您的其他工作階段一同還原，並與工作區工作階段保持分開。

無工作區的聊天僅由 agent host 工作階段支援，因此此體驗在以 `chat.agentHost.enabled`（此設定由組織層級管理。請聯繫您的管理員以變更。）啟用 agent host 時可用。

**Pinned** 和 **Chats** 群組在空時預設保持可見。使用 `sessions.list.showEmptyDefaultGroups` 在這些預設群組包含工作階段之前隱藏它們。

### Agents 視窗中的唯讀子代理聊天（Preview）

當 Agent 將工作委派給子代理時，您可以追蹤每個工作者的進度，而不會中斷或引導主要對話。Agents 視窗為跨專案的 Agent 驅動工作流程提供專用空間，以聊天和工作階段作為主要介面。

當工作階段產生[子代理](https://code.visualstudio.com/docs/agents/subagents)時，它們的逐字稿以唯讀對等聊天形式出現。子代理聊天在您從 Conversations 選單、執行中子代理膠囊或父逐字稿中的行內子代理膠囊開啟之前，會從分頁列中隱藏。開啟的子代理聊天顯示其即時進度、在可用時使用子代理標題，並省略撰寫器和可變動的聊天操作，讓工作者逐字稿保持僅供檢視。

### Agents 視窗中的聊天鍵盤快速鍵

Agents 視窗支援多聊天工作階段，一個 Agent 工作階段可以包含多個相關聊天。鍵盤驅動的聊天導覽幫助您在聊天之間移動和管理聊天分頁，無需離開鍵盤。

- 使用快速鍵建立聊天。
- 使用快速鍵重新開啟最後關閉的聊天。
- 使用快速鍵前往下一個或上一個聊天。
- 使用快速鍵在開啟的聊天之間快速切換。
- 使用快速鍵關閉活動聊天分頁。
- 使用快速鍵刪除活動的非主要聊天。
- 使用快速鍵開啟可搜尋的開啟和關閉聊天選擇器。

這些快速鍵的作用範圍限於 Agents 視窗，當沒有聊天專屬的操作可執行時，會退回到現有的工作階段層級行為。

---

## Chat

### Copilot Vision 正式可用

多模態支援現已在 VS Code 中正式可用，從本次最新發行開始。透過貼上、拖曳和放置或使用內容選單，將圖片和 PDF 附加至 Chat。Agent 也可以透過工具呼叫讀取圖片。

請參閱此 [GitHub 變更日誌](https://github.blog/changelog/2026-07-01-copilot-vision-is-generally-available/)以了解支援的格式和可用性詳情。

### BYOK 模型用於 Agent Host Copilot 工作階段（實驗性）

**設定**：`chat.agentHost.byokModels.enabled`

在 agent host 上執行工作階段時使用 [Bring Your Own Key（BYOK）模型](https://code.visualstudio.com/docs/agent-customization/language-models#_bring-your-own-language-model-key)。啟用 `chat.agentHost.byokModels.enabled` 並重新啟動 agent host 程序以使變更生效。

此功能為實驗性，仍在積極開發中。

### 為自訂端點模型設定取樣參數

您可以為每個[自訂端點模型](https://code.visualstudio.com/docs/agent-customization/language-models#_add-a-custom-endpoint-model)設定 `temperature` 和 `top_p`，讓請求可與具有嚴格參數要求的供應商配合使用。

在模型的 JSON 設定中新增 `modelOptions` 物件：

```json
{
   ...
   "models": [
   {
     "id": "<model-id>",
     "modelOptions": {
       "temperature": 1,
       "top_p": null
     },
     ...
   }
}
```

將屬性設為數字以覆寫 VS Code 發送的預設值。將其設為 `null` 以從請求中省略該參數並使用模型伺服器的預設值。這些選項適用於 Chat Completions、Responses 和 Messages 相容的端點。

### 設定 BYOK 的預設公用模型

**設定**：`chat.byokUtilityModelDefault`

當您使用 Bring Your Own Key（BYOK）模型作為主要 Agent 模型時，您可以變更內建公用流程（如產生聊天標題或提交訊息）使用的預設行為。設定 `chat.byokUtilityModelDefault` 以使用主要 Agent 模型、使用 GitHub Copilot 提供的模型，或不使用預設公用模型。

> **注意**：預設行為是使用 BYOK 模型作為主要 Agent 時不使用公用模型。背景任務如聊天標題產生和提交訊息產生在未設定此選項時不會運作。

此設定在主要 Agent 模型由 GitHub Copilot 提供時無效。以 `chat.utilityModel` 或 `chat.utilitySmallModel` 設定的模型優先於此預設值。

### 深層連結至特定聊天

Agents 視窗幫助您管理 Agent 工作階段並返回其關聯的工作區和聊天。深層連結可以直接帶您回到相關的聊天，讓您不必先開啟工作區然後手動在 Chat 中尋找工作階段。

當應用程式開啟工作階段的 `vscode://` 深層連結時，VS Code 會開啟工作區並聚焦至連結 `session` 查詢參數識別的特定聊天。Agents 視窗中的 **Open in VS Code** 操作使用相同的行為，在新的 VS Code 視窗中同時開啟工作階段的工作區資料夾和其活動聊天。

---

## 編輯器體驗

### 可設定的整合式瀏覽器分頁配置

**設定**：`workbench.browser.newTabPlacement`

保持分頁井然有序可能是一個挑戰。在本次發行中，您可以透過 `workbench.browser.newTabPlacement` 設定來設定瀏覽器分頁開啟的位置。此設定可接受以下值：

- **`activeGroup`**（預設）：瀏覽器分頁始終在使用中的編輯器群組中開啟。
- **`sideGroup`**：瀏覽器分頁在側邊的專用群組中開啟。此群組已鎖定，因此只有瀏覽器分頁會在此開啟。
- **`window`**：瀏覽器分頁在專用的輔助視窗中開啟。此視窗群組同樣鎖定為僅限瀏覽器分頁。

您仍可依需要重新組織分頁，從現有分頁開啟的頁面（例如，透過使用 `Ctrl` 選取連結）會在與父頁面相同的群組中開啟。

### 作業系統層級鍵盤快速鍵

VS Code 現在可以貢獻作業系統層級的鍵盤快速鍵。這些快速鍵即使 VS Code 不在焦點也能生效。在 `keybindings.json` 中為您的按鍵繫結定義新增 `systemWide` 以使其成為作業系統層級。例如，以下是在 macOS 上聚焦 Agents 視窗的 `keybindings.json` 按鍵繫結：

```json
{
  "key": "cmd+shift+a",
  "command": "workbench.action.openAgentsWindow",
  "systemWide": true
}
```

---

## 企業

### 使用 OpenTelemetry 管理 Copilot 遙測匯出

組織可以強制指定 GitHub Copilot 發送 [OpenTelemetry](https://opentelemetry.io/)（OTel）資料的目的地，讓遙測資料流向核准的收集器，而無需每位開發者設定 `OTEL_*` 環境變數。受管理的設定同時適用於 Copilot Chat 擴充功能和 agent host 程序。

管理員透過 [Copilot 受管理設定](https://code.visualstudio.com/docs/enterprise/ai-settings#_deploy-copilot-managed-settings)中的 `telemetry` 區塊傳遞這些設定，使用任何支援的傳遞管道。此區塊控制：

- OTLP 匯出端點和協定。
- OTel 服務名稱和資源屬性。
- 匯出器標頭，例如收集器的驗證 Token。
- 是否擷取提示和回應內容，以及開發者是否可以變更該設定。

受管理的值始終優先，取代環境變數和使用者設定。

若要了解更多，請參閱[使用 OpenTelemetry 設定遙測匯出](https://code.visualstudio.com/docs/enterprise/ai-settings#_configure-telemetry-export-with-opentelemetry)和[使用 OpenTelemetry 監控 Agent 使用量](https://code.visualstudio.com/docs/agents/guides/monitoring-agents)。

---

## 感謝

對 `vscode` 的貢獻：

- [@accnops (Arthur Cnops)](https://github.com/accnops)：chat/voice：在問題輪播上落地語音回答（修正 Skipped）[PR #323161](https://github.com/microsoft/vscode/pull/323161)
- [@dobbydobap (varshitha)](https://github.com/dobbydobap)：修正第二次 Rerun Last Task 對 reevaluateOnRerun 任務無法啟動的問題 [PR #324571](https://github.com/microsoft/vscode/pull/324571)
- [@JeffreyCA](https://github.com/JeffreyCA)：為 Azure Developer CLI (azd) 更新 Fig 規範 [PR #321221](https://github.com/microsoft/vscode/pull/321221)
- [@yavanosta (Dmitry Guketlev)](https://github.com/yavanosta)：在 growUntilVariableBoundaries 中使用 startColumn [PR #324523](https://github.com/microsoft/vscode/pull/324523)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@jasonperry1231hou-lang (jasonperry1231hou-lang)](https://github.com/jasonperry1231hou-lang)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@luo2430 (luo2430)](https://github.com/luo2430)

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
| auxiliary window | 輔助視窗 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Chat Completions | Chat Completions |
| Claude Agent SDK | Claude Agent SDK |
| composer | 撰寫器 |
| Copilot Vision | Copilot Vision |
| credits | 點數 |
| custom endpoint | 自訂端點 |
| deep link | 深層連結 |
| editor group | 編輯器群組 |
| enterprise policy | 企業政策 |
| extension | 擴充功能 |
| fork | 分支 |
| harness | 工具鏈 |
| Integrated Browser | 整合式瀏覽器 |
| keybinding | 按鍵繫結 |
| managed settings | 受管理設定 |
| MDM | MDM（裝置管理） |
| model picker | 模型選擇器 |
| multimodal | 多模態 |
| OpenTelemetry (OTel) | OpenTelemetry（OTel） |
| OTLP | OTLP |
| peer chat | 對等聊天 |
| pill | 膠囊 |
| sampling parameters | 取樣參數 |
| session | 工作階段 |
| subagent | 子代理 |
| systemWide | systemWide |
| tab placement | 分頁配置 |
| terminal | 終端機 |
| token | Token |
| transcript | 逐字稿 |
| utility model | 公用模型 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.128 發行說明](https://code.visualstudio.com/updates/v1_128)*
