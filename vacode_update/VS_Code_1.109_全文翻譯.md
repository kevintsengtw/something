# Visual Studio Code 1.109 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.109（2026 年 1 月）
**發行日期：** 2026 年 2 月 4 日
**原文：** https://code.visualstudio.com/updates/v1_109

---

## 版本更新說明

**更新 1.109.1**：此更新解決了這些安全問題。

**更新 1.109.2**：此更新解決了與聊天相關的問題。

**更新 1.109.3**：此更新解決了這些問題並引入了多項重要功能：
- 訊息導向和佇列排隊：在請求仍在執行時發送後續訊息
- Agent hooks：在 Agent 生命週期的關鍵點執行自訂Shell命令
- Claude 相容性：直接在 VS Code 中重複使用您的 Claude 設定檔案
- 將 Skills 作為斜線命令使用：從聊天中按需調用 Agent Skills

**更新 1.109.4**：此更新解決了這些問題。

**更新 1.109.5**：此更新解決了這些問題並新增了多項功能以改進背景 Agents：
- 支援斜線命令，包括提示檔案、hooks 和 skills
- 重新命名背景 Agent 工作階段的功能
- Kitty 鍵盤支援現已可供所有使用者使用

---

歡迎使用 2026 年 1 月版的 Visual Studio Code。在此版本中，我們進一步發展 VS Code，使其成為**多 Agent 開發的主頁**。

- **聊天使用體驗** - 透過更快的串流、改進的推理結果和改版的編輯器內聊天，聊天體驗變得更好更快速
- **Agent 工作階段管理** - 現在更容易在本機、背景和雲端 Agents 之間委派工作，並在需要時快速介入
- **Agent 自訂** - 使用 Agent 協調建立自己的工作流程，並透過 Agent Skills 和組織層級的自訂來確保一致的結果
- **Agent 擴充性** - 透過 Claude Agent 支援和新的 Anthropic 模型功能重複使用您的知識，並透過 MCP Apps 享受豐富的聊天互動
- **Agent 最佳化** - Agents 透過 Copilot 記憶體更聰慧地運作，並透過外部索引體驗更快的程式碼搜尋
- **Agent 安全和信任** - 透過沙箱和有效的自動批准規則自信地執行終端機命令
- **工作台和生產力** - 使用新的整合式瀏覽器在不離開編輯器的情況下測試您的應用程式
- **終端機增強功能** - 品質改進，使您的終端機體驗更順暢可靠
- **程式碼和編輯器** - 多項重大改進使日常程式碼編寫更流暢
- **擴充功能和 API** - 為擴充功能作者新增功能以建立更豐富的體驗

---

## 即將舉辦的活動

### Agent Sessions Day

加入我們於 2 月 19 日舉辦的 Agent Sessions Day，親眼看到這些最新更新的現場演示！探索 VS Code 如何發展成統一的 Agent 使用體驗，同時保持開放性、擴充性和開發者選擇的核心價值。

---

## 聊天使用體驗

更快的回應、更清楚的推理過程，以及更少的摩擦。此版本帶來了串流改進，可在進行時顯示進度，改版的行內聊天可保持不干擾，以及更好地呈現模型的思考過程，讓您在 AI 運作時保持流暢狀態。

### 訊息導向和佇列排隊（實驗功能）

**設定**：chat.requestQueuing.enabled、chat.requestQueuing.defaultAction

**更新 1.109.3**：進行較長的工作任務時，您通常會在目前任務完成前想到下一個任務，或注意到 Agent 走錯了方向。過去，您必須等待回應完成或完全取消。現在，您可以在請求仍在執行時發送後續訊息。

當請求正在進行時，**發送**按鈕會變為下拉式選單，提供三個選項：
- **加入佇列**：您的訊息等待並在目前回應完成後自動發送。
- **使用訊息導向**：訊號目前請求在完成工具執行後產生，然後立即處理您的新訊息。當 Agent 走錯方向時，使用此選項重新導向。
- **停止並發送**：完全取消目前請求並立即發送您的新訊息。

當您有多個待處理訊息時，拖放它們以變更其處理順序。

使用 chat.requestQueuing.defaultAction 將「發送」按鈕的預設動作設定為 `steer`（預設）或 `queue`。

### Anthropic 模型現在顯示思考 Token

**設定**：chat.thinking.style、chat.agent.thinking.collapsedTools、chat.agent.thinking.terminalTools、chat.tools.autoExpandFailures

您中的許多人在 VS Code 中使用 Anthropic 的 Claude 模型。這些模型現在支援思考 Token，以便您更清楚了解模型的推理過程。

在此版本中，我們增強了聊天使用體驗以更有效地呈現思考 Token。更多資訊，更少雜訊！

- 在詳細或精簡思考風格之間選擇，以符合您的偏好（chat.thinking.style）。
- 您將看到模型的思考過程與工具呼叫和回應交錯顯示（chat.agent.thinking.terminalTools）。
- 失敗的工具呼叫會自動展開以顯示更多內容（chat.tools.autoExpandFailures）。
- 多種視覺增強以使追蹤模型活動更直觀，例如可捲動的思考內容和閃爍動畫。

### 聊天回應中的 Mermaid 圖表

聊天回應現在可以使用 `renderMermaidDiagram` 工具呈現互動式 Mermaid 圖表。這讓模型可以使用流程圖、時序圖和其他視覺化方式來視覺化分解複雜概念。這些圖表是互動式的，讓您可以平移和縮放以詳細探索，或在全屏編輯器中開啟以便更容易檢視。

使用下列控制項與 Mermaid 圖表互動：
- **平移和縮放** - 按住 Alt/Option 並使用滑鼠滾輪縮放，或在觸控板上輕觸縮放。按住 Alt/Option 並拖曳以平移圖表。
- **按一下縮放** - 按住 Alt/Option 並按一下縮放。加上 Shift 可縮小。
- **在編輯器中開啟** - 使用按鈕在全屏編輯器中開啟圖表，以便更好地檢視較大的圖表。
- **複製來源** - 在圖表上按右鍵並選擇 `Copy diagram source` 以複製其 Mermaid 源程式碼。

### 提出問題工具（實驗功能）

**設定**：chat.askQuestions.enabled

當有不清楚的地方時，Agent 現在可以使用 `askQuestions` 工具在聊天對話中提出澄清問題，而不是做出假設。它在聊天中直接呈現一個或多個問題，提供單選/多選選項、自由文字輸入，以及建議答案以供快速決策。

使用鍵盤上下鍵在答案之間導航，或輸入與對應答案相符的數字（使用 Escape 跳過剩餘問題）。

我們已改版內建的計畫 Agent，也充分利用 `askQuestions` 工具，確保您的實施計畫符合您的期望及更多！

### 計畫 Agent

內建的計畫 Agent 讓您在開始程式碼開發前建立結構化的實施計畫。這有助於確保 AI 理解工作要求並產生滿足您期望的高品質程式碼。

- 計畫 Agent 現在遵循結構化的四階段反覆工作流程，產生更高品質的實施計畫：
  1. **探索** - 自主探索您的程式碼庫，搜尋相關檔案並理解專案結構。
  2. **對齊** - 在提交計畫前暫停提出澄清問題，提前發現歧義。
  3. **設計** - 草擬包含清晰步驟、檔案位置和程式碼片段的綜合實施計畫。
  4. **精進** - 新增驗證標準並記錄規劃期間所做的決策。

- 您現在可以在聊天中輸入 `/plan` 後跟工作描述來調用計畫 Agent。

### 上下文視窗詳細資訊

為了追蹤模型如何使用其上下文視窗，您現在可以在聊天輸入區域看到上下文視窗指示器。將滑鼠懸停在指示器上可查看按類別分類的Token使用情況詳細資訊。

### 行內聊天使用體驗改版（預覽）

**設定**：inlineChat.affordance、inlineChat.renderMode

我們持續改版行內聊天體驗，推出兩項預覽功能：
- 選擇文字時更容易觸發行內聊天的親和力（inlineChat.affordance）
- 輕量級且更容易使用的上下文語境呈現（inlineChat.renderMode）

### 模型選擇器中的模型描述

懸停或鍵盤聚焦模型選擇器中的模型時，您現在可以快速查看其詳細資訊。

### 終端機命令輸出

#### 更豐富的命令詳細資訊

為了更清楚地說明執行了哪個命令，終端機工具現在顯示額外詳細資訊：
- **Node、Python 和 Ruby 的內聯語法突顯**
- **工作目錄**
- **命令意圖的描述**

#### 輸出串流

終端機輸出現在會在命令執行需要時間時自動展開，讓您立即看到發生了什麼。快速命令保持摺疊狀態以減少視覺雜訊。

#### 互動式輸入

嵌入的終端機現在完全互動。您可以聚焦終端機並直接輸入，這在命令提示確認或需要使用者輸入時很有用。游標現在可見，清楚表明終端機何時準備好進行輸入。

#### 刪除所有隱藏終端機

終端機面板中的**隱藏終端機**項目現在包含刪除圖示，可透過一個動作清除所有隱藏終端機。

### 告訴我們您對我們新主題的看法（實驗功能）

我們正在開發新的實驗性 `VS Code Light` 和 `VS Code Dark` 主題，以增加焦點並透過使用陰影和透明度為使用者介面帶來提升感和輕盈感。這些主題仍在開發中。

**設定**：chat.editMode.hidden

編輯模式現在預設在聊天中的 Agent 下拉式選單中隱藏。使用 Agents 提供了編輯模式功能的超集並為程式碼編輯工作產生更好的結果。

如果您的組織已停用 Agent 模式，編輯模式在 Agent 下拉式選單中保持可見。您也可以透過停用 chat.editMode.hidden 設定手動復原編輯模式。

---

## Agent 工作階段管理

委派、監控和切換而不失去焦點。您現在可以在本機、背景和雲端環境中並行執行多個 Agent 工作階段，全部從單一統一檢視進行。在工作階段之間跳躍、一目了然地追蹤進度，並讓 Agents 獨立運作，同時保持您的生產力。

### 在 Agent 類型之間切換和委派

VS Code 可輕鬆跨不同環境啟動 Agent 工作階段：本機在 VS Code 中、背景中、雲端中，或使用其他 Agent 提供者。我們透過在聊天輸入區域引入新的工作階段類型選擇器，使在不同 Agent 類型之間切換變得更容易。

選擇器有兩個主要用途：
- 選擇您想開始的 Agent 工作階段類型
- 將正在進行的工作階段轉交給不同的 Agent 類型（例如在本機規劃任務，在雲端實施）

> **提示：** 我們新增了一個新的 `workbench.action.chat.newLocalChat` 命令來建立新的本機聊天工作階段。將此命令繫結到鍵盤快捷方式以加快存取速度。

### 追蹤 Agent 工作階段

#### Agent 工作階段檢視

我們進一步改進了 VS Code 中的 Agent 工作階段檢視：
- 並排顯示時調整工作階段列表的大小
- 多選工作階段以執行批量操作
- 更好的堆疊檢視以改進導航工作階段和套用篩選

#### Agent 狀態指示器

**設定**：chat.agentsControl.enabled、chat.agentsControl.clickBehavior

指示器顯示不同類型的狀態資訊：進行中、未讀和需要您注意的工作階段。選擇指示器以快速開啟和篩選工作階段列表。

我們也更新了命令中心中的聊天按鈕，讓您設定點擊時的行為（chat.agentsControl.clickBehavior）。預設現在會循環顯示不同的聊天檢視狀態：側邊欄、最大化或隱藏。

### 子 Agents

**設定**：chat.customAgentInSubagent.enabled

Agents 可以使用子 Agents 執行子工作，以將複雜工作分解為較小的部分。子 Agents 的好處是它們在自己專屬的上下文視窗中運作，不會累加到主 Agent 的上下文視窗。

子 Agents 現在可以並行執行，這可以大幅加快可分割為獨立工作的任務。

為了提供更多可見性來了解不同的子 Agents 在做什麼，聊天對話現在顯示詳細資訊，例如它在執行什麼工作、用於子 Agent 的自訂 Agent，以及目前使用的工具。您可以展開子 Agent 資訊以顯示它在做什麼的完整詳細資訊，包括提供給它的完整初始提示和它傳回的結果。

#### 搜尋子 Agent（實驗功能）

**設定**：github.copilot.chat.searchSubagent.enabled

搜尋您的程式碼庫通常是一項可能涉及多次反覆的工作，並且可能會快速累加到您的上下文視窗限制。我們新增了對搜尋子 Agent 的支援，它在隔離的 Agent 迴圈中執行，使其能夠反覆精進搜尋、嘗試多個查詢並探索工作區的不同部分。

這改進了複雜查詢的搜尋結果品質，其中單一搜尋是不夠的。它也保留了主 Agent 的上下文視窗，使其能夠在搜尋子 Agent 完成工作時繼續運作。

### 雲端 Agents

當您啟動新的雲端 Agent 工作階段時，您現在有更多選項：
- **雲端 Agents 的模型選擇**
- **第三方程式碼 Agents**（預覽）：如果您有第三方程式碼 Agents（例如 Claude 和 Codex）配置為 GitHub Copilot 訂閱的一部分，VS Code 將顯示這些選項。
- **雲端 Agents 的自訂 Agents**：從您的目標 GitHub 存放庫預設分支中可用的自訂 Agents 中選擇。
- **多根工作區和空白工作區改進**：選擇用於雲端 Agents 的資料夾。
- **檢視總是顯示**：即使未安裝 GitHub Pull Requests 擴充功能，檢視選項現在也會顯示。

### 背景 Agents

配合本機和雲端 Agents，我們為背景 Agents 新增了多項改進：
- **背景 Agents 的自訂 Agents**
- **附加影像作為內容**
- **多根工作區改進**
- **每回合末端自動提交**：背景 Agent 迴圈在每回合末端將更變提交到 Git worktree。這消除了保留/復原動作。
- **背景 Agents 的斜線命令**（更新 1.109.5）：提示檔案、hooks 和 skills 現在可用作斜線命令。
- **重新命名背景 Agents 工作階段**（更新 1.109.5）

#### Agent 工作階段歡迎頁面（實驗功能）

**設定**：workbench.startupEditor

我們正在實驗新的歡迎頁面。透過將 workbench.startupEditor 設定為 `agentSessionsWelcomePage` 來啟用它。

---

## Agent 自訂

塑造 AI 如何使用您的程式碼庫，並在您的團隊中共享。

### Agent Hooks（預覽）

**設定**：chat.hooks.enabled

**更新 1.109.3**：Hooks 讓您在 Agent 工作階段期間的關鍵生命週期點執行自訂Shell命令。與指導 Agent 行為的指示或自訂提示不同，hooks 以確定性方式執行您的程式碼，提供保證的結果。使用它們來強制安全政策、自動化程式碼品質檢查、建立稽核軌跡或注入專案特定的內容。

VS Code 支援八個 hook 事件，在工作階段期間的特定點觸發，包括 `PreToolUse` 和 `PostToolUse` 用於攔截工具呼叫，`SessionStart` 和 `Stop` 用於工作階段生命週期，以及 `SubagentStart` 和 `SubagentStop` 用於追蹤巢狀 Agent 使用。

VS Code 使用與 Claude Code 和 Copilot CLI 相同的 hook 格式，因此您可以跨工具重複使用現有的 hook 設定。

若要開始，在聊天中使用 `/hooks` 斜線命令來設定新的 hook，或手動建立 hook 檔案。

### 將 Skills 作為斜線命令使用

Agent Skills 現在可在聊天中以斜線命令的形式提供，與提示檔案並存。在聊天輸入中輸入 `/` 以查看所有可用的 Skills 和提示，並選擇 Skill 以立即調用它。您可以在命令後新增額外內容。

預設情況下，所有 Skills 都顯示在 `/` 選單中。在您的 Skill 檔案中使用 `user-invocable` 和 `disable-model-invocation` 前置資料屬性來控制如何存取每個 Skill：
- 設定 `user-invocable: false` 從選單隱藏 Skill，同時仍讓模型自動載入。
- 設定 `disable-model-invocation: true` 在選單中顯示 Skill，但防止模型自行載入。

### 使用 /init 為您的工作區設定 AI

透過類似 `copilot-instructions.md` 或 `AGENTS.md` 的常時開啟自訂指示，您可以確保 AI 使用您的專案慣例來產生程式碼。

為了使用基於您的程式碼庫的初始指示集來初始化您的專案，您現在可以在聊天中使用 `/init` 斜線命令來產生或更新您的工作區指示。

當您執行 `/init` 時，Agent 會探索工作區中的現有 AI 慣例、分析專案結構和程式碼模式，並產生為您的專案量身定製的綜合工作區指示。`/init` 命令實作為貢獻的提示檔案，因此您可以透過修改基礎提示來自訂其行為。

### Agent Skills 正式上線

**設定**：chat.useAgentSkills、chat.agentSkillsLocations

Agent Skills 現在正式上線且預設已啟用。Skills 提供專業功能、網域知識和精製工作流程。

您現在可以用管理提示檔案、指示或自訂 Agents 的相同方式在 VS Code 中管理 Skills。使用**聊天：設定 Skills** 命令檢視所有可用 Skills，或使用**聊天：新建 Skill 檔案**來建立新 Skill。

預設情況下，VS Code 在工作區的 `.github/skills` 和 `.claude/skills` 資料夾，或您的使用者主目錄中的 `~/.copilot/skills` 或 `~/.claude/skills` 資料夾中查找 Skills。您可以使用 chat.agentSkillsLocations 設定指定自訂路徑。

如果您是擴充功能作者，您也可以透過將 Skills 包含在您的擴充功能中並使用 `package.json` 中的 `chatSkills` 貢獻點進行註冊，來與您的擴充功能一起打包和分發 Skills：

```json
{
  "contributes": {
    "chatSkills": [
      {
        "path": "./skills/my-skill"
      }
    ]
  }
}
```

`path` 必須指向包含 `SKILL.md` 檔案的目錄，而 `SKILL.md` 前置資料中的 `name` 欄位必須與上層目錄名稱相符。

### 組織層級指示

**設定**：github.copilot.chat.organizationInstructions.enabled

過去，VS Code 已新增了對組織層級自訂 Agents 的支援。在此版本中，VS Code 現在也支援組織層級自訂指示。如果您的 GitHub 組織已為 Copilot 設定自訂指示，它們會自動套用到您的聊天工作階段，確保整個團隊的一致指導。

此功能預設啟用。您可以透過將 github.copilot.chat.organizationInstructions.enabled 設定為 `false` 來停用組織指示。

### 自訂 Agent 檔案位置

**設定**：chat.agentFilesLocations

與其他自訂檔案相同，您現在可以設定 VS Code 查找自訂 Agent 定義（`.agent.md`）的位置。

預設情況下，VS Code 在工作區的 `.github/agents` 資料夾中搜尋 Agent 檔案。透過新的 chat.agentFilesLocations 設定，您可以新增額外目錄。

```json
{
  "chat.agentFilesLocations": {
    "~/.vscode/agents": true,
    "shared/team-agents": true
  }
}
```

### 控制如何調用自訂 Agents

**設定**：chat.customAgentInSubagent.enabled

自訂 Agents 現在支援前置資料標題中的額外屬性：

- `user-invocable`：控制是否可從聊天中的 Agents 下拉式選單選取該 Agent。設定為 `false` 以建立只能透過程式設計方式或作為子 Agents 存取的 Agents。
- `disable-model-invocation`：當啟用時，防止 Agent 由其他 Agents 作為子 Agent 調用。
- `agents`：限制目前 Agent 可以調用哪些子 Agents。

啟用 chat.customAgentInSubagent.enabled 以啟用自訂 Agents 作為子 Agents。

```yaml
---
name: my-internal-agent
user-invocable: false
---

This agent can only be invoked as a subagent
```

下列範例顯示僅能調用 `Modify` 和 `Search` 子 Agents 的 Agent：

```yaml
---
name: Foo
tools: ['agent']
agents: ['Modify', 'Search']
---

This agent can only use the Modify and Search subagents.
```

### 自訂 Agents 的多模型支援

自訂 Agents 現在可以在前置資料標題中指定一個以上的模型。使用列表中第一個可用的模型，在慣用的模型無法使用時提供後備選項。

```yaml
---
name: my-agent
model: ['Claude Sonnet 4.5 (copilot)', 'GPT-5 (copilot)']
---

This agent prefers Claude Sonnet 4.5 but falls back to GPT-5 if unavailable.
```

### 聊天自訂診斷

新的診斷檢視可幫助您對聊天自訂問題進行疑難排解，方法是顯示所有目前已載入的自訂 Agents、提示檔案、指示檔案和 Skills。若要存取，請在聊天檢視中按右鍵並選擇**診斷**。

### 語言模型編輯器

語言模型編輯器為管理和設定語言模型提供集中式介面：

- **每個提供者的多個設定**：為同一個模型提供者建立具有不同 API 金鑰的多個設定。
- **從 Azure 提供者設定模型**：VS Code 開啟 `chatLanguageModels.json` 設定檔案並插入片段範本。
- **管理提供者群組**：設定或移除現有的提供者群組。
- **額外的使用者介面改進**：鍵盤存取、內容選單動作、多選。
- **語言模型設定檔案**：模型設定存儲在專屬的 `chatLanguageModels.json` 檔案中。
- **模型提供者設定使用者介面**：模型提供者可以宣告其設定結構定義。如需Proposed API 的詳細資訊，請參閱聊天模型提供者設定。

### 語言模型設定

**設定**：github.copilot.chat.implementAgent.model、inlineChat.defaultModel

- **計畫實施的預設模型（實驗功能）**：為計畫 Agent 的實施步驟設定預設語言模型。模型值格式為 `Model Name (vendor)`。
- **行內聊天的預設模型**：使用 inlineChat.defaultModel 設定進行設定。
- **為 Agent 轉交指定語言模型**：Agent 轉交現在支援選用的 `model` 參數。

### Agent 自訂 Skill（實驗功能）

**設定**：chat.agentCustomizationSkill.enabled

新的 **agent-customization** Skill 教授 Agent 如何幫助您自訂您的 AI 程式碼體驗。此 Skill 涵蓋自訂 Agents、自訂指示、提示檔案、Skills 和工作區指示。

---

## Agent 擴充性

此版本新增了 Claude Agent 支援、可呈現互動式視覺化的 MCP Apps，以及新的提供者功能。

### Claude 相容性

如果您同時使用 VS Code 和 Claude，就不再需要維護個別的設定檔案。VS Code 現在直接讀取 Claude 設定檔案。

VS Code 偵測下列 Claude 檔案位置：
- **指示**：工作區根目錄中的 `CLAUDE.md` 檔案、`.claude/CLAUDE.md`、`~/.claude/CLAUDE.md` 和 `.claude/rules` 資料夾。
- **Agents**：`.claude/agents` 資料夾中的 `.md` 檔案。
- **Skills**：`.claude/skills` 和 `~/.claude/skills` 資料夾中的 Skill 定義。
- **Hooks**：`.claude/settings.json` 和 `~/.claude/settings.json` 中的 Hook 設定。

### Agent 協調

Agent 協調將工作分配給針對特定角色最佳化的專用 Agents。優點：
- **內容效率**：每個子 Agent 在其自己的專屬上下文視窗中運作。
- **專業化**：不同的 Agents 可以使用針對其工作最佳化的不同模型。
- **並行執行**：獨立工作可以跨多個子 Agents 並行執行。

VS Code 具有所有構建塊：自訂 Agents、子 Agents 和控制 Agents 如何被調用的方式。

社群範例：
- Copilot Orchestra - 具有「Conductor」的多 Agent 系統
- GitHub Copilot Atlas - 具有專業 Agents 的擴展協調系統

### Claude Agent（預覽）

此版本引入了 Claude Agent 支援。這讓您能夠使用 GitHub Copilot 訂閱中包含的 Claude 模型透過 Claude Agent SDK 委派工作。

此整合使用 Anthropic 的官方 Claude Agent 工具，這意味著它與其他 Claude Agent 實作共享相同的提示、工具和整體架構。

### Anthropic 模型

**設定**：github.copilot.chat.anthropic.thinking.budgetTokens、github.copilot.chat.anthropic.toolSearchTool.enabled、github.copilot.chat.anthropic.contextEditing.enabled

- **具有交錯思考的 Messages API**：Anthropic 模型現在使用 Messages API，支援交錯思考。使用 github.copilot.chat.anthropic.thinking.budgetTokens 設定思考預算，或將其設定為 `0` 以停用。
- **工具搜尋工具**：啟用以協助 Claude 探索和選擇最相關的工具（github.copilot.chat.anthropic.toolSearchTool.enabled）。
- **內容編輯（實驗功能）**：清除來自先前回合的工具結果和思考 Token，協助延遲摘要（github.copilot.chat.anthropic.contextEditing.enabled）。

### 對 MCP Apps 的支援

VS Code 已新增對 MCP Apps 的支援。MCP Apps 允許伺服器在用戶端中顯示豐富的互動式使用者介面。當伺服器傳回 Apps 時，它們會自動顯示。

MCP 伺服器開發者的資源：
- MCP Apps 示範存放庫
- MCP Apps SDK 和範例
- VS Code MCP 文件
- MCP 伺服器開發指南

### 對 MCP 套件自訂登錄基礎 URL 的支援

VS Code 現在支援 MCP 伺服器資訊清單檔案中的 `registryBaseUrl` 屬性，讓組織能夠從私有或替代套件登錄部署 MCP 伺服器。

---

## Agent 優化

更聰明的內容、更快的搜尋、更好的結果。藉由 Copilot Memory，Agent 可以跨工作階段記憶重要資訊。外部索引為非 GitHub 工作區提供快速的語義搜尋。Agent 現在在獲得許可的情況下，可以讀取工作區外的檔案。

### Copilot Memory（預覽）

**設定**：github.copilot.chat.copilotMemory.enabled

如果你發現自己反覆向 AI 提供相同的內容，你現在可以使用 Copilot Memory 來儲存和跨工作階段回憶重要資訊。

透過新的 memory tool，你的聊天現在可以直接存取和更新 Copilot Memory。這使 Agent 能夠從你儲存的記憶中檢索相關內容，並在你工作時保存新的學習內容。透過將 github.copilot.chat.copilotMemory.enabled 設定為 `true` 來啟用 memory tool。

memory tool 應該能夠識別何時儲存特定資訊作為記憶（「當有疑問時，務必詢問澄清問題」），以及何時檢索相關記憶來通知其回應。

你可以從 GitHub 的 Copilot 設定中檢視和管理所有記憶。

### 非 GitHub 工作區的外部索引（預覽）

**設定**：github.copilot.chat.advanced.workspace.codeSearchExternalIngest.enabled

未在 GitHub 上託管的工作區現在可以被遠端索引，以在使用 Agent 時提供更快的程式碼搜尋。當你在非 GitHub 工作區中使用 `#codebase` 時，VS Code 會建立程式碼庫的索引，以啟用快速的語義搜尋，提供與 GitHub 託管的儲存庫相同的強大程式碼搜尋功能。

索引在第一次請求時建立，根據儲存庫大小和你的網路連線，可能需要幾分鐘。後續請求會更快，因為它們使用快取的索引。索引會在你修改並儲存檔案時自動更新。

我們將在接下來的幾週內逐步推出外部索引。請注意，任何已在 GitHub 上託管的工作區都已支援遠端索引，不需要更昂貴的呼叫在第一次請求時建立索引。

### 讀取工作區外的檔案

Agent 現在可以在獲得你許可的情況下，讀取工作區外的檔案和列表目錄。先前，存取嘗試被自動拒絕。現在，當 Agent 需要存取外部檔案或資料夾時，VS Code 會提示你允許或拒絕該請求。

你也可以允許整個工作階段的存取，以避免對未來相同資料夾下讀取的重複提示。

### 效能改進

在此版本中，我們進行了許多效能改進：
- **大型聊天**：長聊天對話現在應該在開啟和捲動時感覺更順暢。我們也優化了對話的持久化方式，以使其整體上更加可靠。
- **平行相依任務**：透過 Agent 執行任務時，相依任務現在以平行方式而非順序方式處理。這可以顯著改進具有多個獨立建置步驟的專案的建置時間。

---

## Agent 安全性和信任

自信地執行 Agent 命令。新的終端機沙箱限制 Agent 執行命令的檔案和網路存取，自動批准規則略過安全操作的確認，改進的呈現方式顯示正在執行的內容和原因，因此你始終掌控局面。

### 終端機沙箱（實驗性）

**設定**：chat.tools.terminal.sandbox.enabled、chat.tools.terminal.sandbox.linuxFileSystem、chat.tools.terminal.sandbox.macFileSystem、chat.tools.terminal.sandbox.network

Agent 具有與你的使用者帳戶相同的權限。為了幫助減輕與 Agent 執行的終端機命令相關的風險，我們推出了實驗性終端機沙箱功能。終端機沙箱限制檔案系統存取僅限於工作區資料夾，並讓你限制網路存取為僅受信任的網域。

> **注意**：終端機沙箱目前僅在 macOS 和 Linux 上支援。在 Windows 上，沙箱設定無效。

若要啟用終端機沙箱，將 chat.tools.terminal.sandbox.enabled 設定設為 `true`。

啟用沙箱時：
- 命令預設具有對目前工作目錄的讀寫存取權
- 命令執行時不會顯示標準確認對話方塊，因為它們在受控環境中執行
- 預設所有網域的網路存取都被阻止

### 終端機工具生命週期改進

此版本中進行了幾項變更，以幫助解決不正確背景終端機行為的問題：

- 你現在可以手動將終端機工具呼叫推送到背景，釋放 Agent 以繼續執行其他工作。
- 進行終端機工具呼叫時，Agent 需要填入新的 `timeout` 屬性，其中 0 表示無逾時。
- 新的 `awaitTerminal` tool 讓 Agent 能夠等待背景終端機完成，這也需要 `timeout` 屬性。
- 新的 `killTerminal` tool 讓 Agent 能夠終止背景終端機以自行清理。
- 關於目前工作目錄如何運作的幾項指令變更，因為活躍的非背景終端機始終持久化目前工作目錄，而背景終端機始終在工作區目錄中啟動。

### 終端機自動批准

**設定**：chat.tools.terminal.enableAutoApprove

啟用終端機自動批准時，以下命令現在預設自動批准：
- `Set-Location`
- `dir`
- `od`
- `xxd` - 旗標和單一輸入檔案
- `docker` - 所有安全的子命令
- `npm`、`yarn`、`pnpm` - 所有安全的子命令

---

## 終端機增強

更順暢、功能更強大的終端機。

### 選擇性忽略Sticky Scroll

**設定**：terminal.integrated.stickyScroll.ignoredCommands

某些命令先前在不希望出現該行為時出現在Sticky Scroll中，例如像 `clear` 這樣的命令。從此版本開始，你可以自訂要忽略的命令，並且已包括一些常見的 Agent CLI（在普通緩衝區中執行）如 `copilot`、`claude`、`codex` 和 `gemini`。

### 移除 winpty 支援

node-pty 已移除對 winpty 的支援，這表示終端機將不再適用於 Windows 10 1809 版本（Fall 2018）之前的 Windows 版本。ConPTY 是終端機的現代機制，因此我們建議升級至較新版本的 Windows 10 或改用 Windows 11。你可以嘗試設定 `"terminal.integrated.windowsUseConptyDll": true` 來使終端機運作，但請注意這目前是實驗性的。

### 允許在受限工作區中開啟終端

**設定**：terminal.integrated.allowInUntrustedWorkspace

當未授予工作區信任時，開啟終端機會被阻止以保護使用者免受攻擊，其中 shell 可能透過設定 `.env` 檔案中的變數之類的方式執行程式碼。注重安全性的使用者通常設定其 shell 以防止這成為可能，因此有新的選擇性設定，可在受限工作區中開啟終端機。

### 新的 VT 功能

**設定**：terminal.integrated.enableKittyKeyboardProtocol

Kitty 鍵盤協定已實作，將在此版本推出至穩定。此功能旨在解決傳統上鍵擊編碼方式的許多限制，特別是：
- 允許終端機編碼更多修飾鍵和多個修飾鍵，而不僅限於 alt 和 ctrl
- 處理按下和釋放事件以及重複按下（按住按鍵）
- 區分許多鍵擊，如 Escape，通常會傳送 `ESC`（`\x1b`）序列，這也恰好是所有轉義序列的開始。

這需要在終端機中執行的程式支援協定並在其執行時要求啟用。你會立即看到的一個大優點是，shift+enter 應該在某些 Agent CLI 中無需執行 `/terminalSetup` 之類的東西即可運作。

**設定**：terminal.integrated.enableWin32InputMode（實驗性）

類似於上述，有一個實驗性版本的 win32 輸入模式，可完成類似的功能但針對 Windows 及其虛擬終端機後端 ConPTY 特別調整。此版本將保持關閉狀態。

其他：
- 獨立控制粗體和淡體 SGR 屬性（`SGR 222`、`SGR 221`）。此序列很少使用，但它是明確的，在使用時可能會損壞輸出，因此我們決定支援它。

---

## 編碼和編輯器

積少成多的小改進。

### 括號配對前景色

你現在可以使用新的 `editorBracketMatch.foreground` 色彩主題Token自訂配對括號的文字色彩。先前，你只能自訂背景（`editorBracketMatch.background`）和邊框（`editorBracketMatch.border`）色彩。

設定預設為 `null`，表示括號繼承其正常文字色彩。在 `settings.json` 的 `workbench.colorCustomizations` 下進行設定：

```json
{
  "workbench.colorCustomizations": {
    "editorBracketMatch.foreground": "#ff0000"
  }
}
```

### 雙擊選取括號和字串內容

你現在可以在開括號後或結括號前立即雙擊以選取內部的所有內容。這也適用於字串 - 雙擊開引號後或結引號前以選取字串內容。

### TypeScript 重新命名建議

TypeScript 的重新命名建議現在在鍵入現有宣告時也能運作。在下列影片中，使用者藉由鍵入新的識別碼名稱而不是使用重新命名重構，將宣告 `let index = 0;` 變更為 `let chunkIndex = 0;`。下一個編輯建議仍提議使用 Shift+Tab 將 `index` 重新命名為 `chunkIndex`。

> **注意**：此功能目前僅適用於 TypeScript。

### 改進的Ghost Text可見性

行內建議（Ghost Text）現在在顯示少於三個連續非空白字元的短建議時顯示虛線下劃線。此視覺指示符使在編輯器中區分Ghost Text與實際程式碼變得更容易。

### 程式碼片段檔案模式

你現在可以使用 `include` 和 `exclude` glob 模式控制程式碼片段出現在哪些檔案中。使用此功能限制程式碼片段至特定檔案或專案背景，防止它們出現在無關檔案中。

例如，建立僅出現在 Travis CI 設定檔案中的程式碼片段：

```json
{
  "Travis CI node_js": {
    "include": ".travis.yml",
    "prefix": "node",
    "body": ["language: node_js", "node_js:", "  - $1"],
    "description": "Node.js configuration for Travis CI"
  }
}
```

如果模式包含路徑分隔符，則模式與絕對檔案路徑相符，否則它們僅與檔案名相符。`include` 和 `exclude` 都可以是單一模式或模式陣列。

### 改進的 shebang 語言偵測

VS Code 現在具有改進的 shebang 語言偵測支援，特別是對於使用 `/usr/bin/env` 並帶有其他旗標的檔案。具有像 `#!/usr/bin/env -S deno -A` 這樣的 shebang 的檔案現在被正確偵測為 TypeScript。

---

## 工作區和生產力

測試、偵錯和發佈，無需切換視窗。

### 整合式瀏覽器（預覽）

**設定**：workbench.browser.openLocalhostLinks、simpleBrowser.useIntegratedBrowser、livePreview.useIntegratedBrowser

VS Code 長期以來包含用於開啟基本網頁的 Simple Browser。但由於它依賴於 iframe，存在幾個限制：不可能進行網站驗證，常見網站如 Google、GitHub 和 Stack Overflow 無法開啟。

此版本引入了 VS Code 桌面的新整合式瀏覽器，克服了這些限制。你現在可以登入網站並瀏覽任何頁面。

亮點包括：
- **持久性資料儲存**，具有可設定的範圍（全域、工作區或記憶中 / 臨時）
- **將元素新增到聊天**：選取元素並將其傳送給 Agent 以取得協助
- **功能完整的 DevTools**
- **鍵盤快速鍵**
- **在頁面中尋找**

若要試試，執行 **Browser: Open Integrated Browser** 命令。你也可以設定整合式瀏覽器以使用 simpleBrowser.useIntegratedBrowser 取代 Simple Browser，或由 Live Preview 擴充功能使用 livePreview.useIntegratedBrowser。

### 在開啟工作區時還原編輯器

**設定**：workbench.editor.restoreEditors

使用新的 workbench.editor.restoreEditors 設定，你可以控制開啟工作區時編輯器是否應還原。停用時，VS Code 以乾淨的編輯器區域啟動。

> **注意**：無論此設定如何，未儲存的編輯器始終還原以防止資料遺失。

### 進階設定

**設定**：workbench.settings.alwaysShowAdvancedSettings

你現在可以設定 VS Code 以在設定編輯器中始終顯示進階設定，而無需每次都套用 `@tag:advanced` 篩選。

### 透過拖曳匯入設定檔

你現在可以透過將 `.code-profile` 檔案拖曳到 VS Code 視窗上來匯入設定檔。

### 輸出頻道篩選改進

輸出面板篩選現在支援負值模式和多個篩選。使用 `!` 從輸出中排除特定行。你也可以使用逗號合併多個模式。

### 按來源篩選問題

問題面板現在支援按診斷的來源或擁有者篩選。例如，鍵入 `source:ts` 以僅顯示 TypeScript 診斷，或使用 `!source:cSpell` 隱藏所有拼字檢查器警告。

### 擴充功能編輯器顯示設定預設值

擴充功能編輯器中的「功能貢獻」索引標籤現在顯示擴充功能所貢獻的設定預設值。

### 在 git worktree 中包含其他檔案（實驗性）

**設定**：git.worktreeIncludeFiles

使用新的 git.worktreeIncludeFiles 設定，你可以指定建立 worktree 後複製到 worktree 資料夾的其他檔案或 glob 模式。當你的專案依賴被 git 忽略的檔案時，這很有用。

### SCM 檢視中的全部摺疊動作

在將檔案視為來源控制檢視的「變更」部分的樹狀結構時，你現在可以在根節點的右鍵選單中使用 **Collapse All** 動作一次摺疊所有展開的目錄結構。

### Git：刪除命令

新的 **Git: Delete** 命令讓你直接從命令調色盤執行目前開啟檔案上的 `git rm`。

### 停用 blame 編輯器裝飾懸停

**設定**：git.blame.editorDecoration.hoverEnabled

你現在可以停用在編輯器中將滑鼠移動到內聯 Git blame 裝飾上時出現的懸停快顯。

### 預設停用自動任務

**設定**：task.allowAutomaticTasks

為了改進安全性，task.allowAutomaticTasks 設定現在預設為 `off` 而非 `on`。

### 無障礙改進

- **動態串流聊天回應和在無障礙檢視中思考**：無障礙檢視現在在 AI 模型生成內容時動態串流聊天回應內容。這現在包括思考內容。
- **無障礙檢視中的穩定游標位置**：游標位置現在在內容更新期間保持穩定。
- **新聊天工作階段的 ARIA 警示**：螢幕閱讀器使用者現在在建立新聊天工作階段時會收到 ARIA 警示通知。
- **改進無障礙檢視中的工具呼叫資訊**：工具呼叫現在包括更完整的資訊。
- **公告游標位置命令**：新 `Announce Cursor Position` 使用 Ctrl/Cmd+Alt+Shift+G。

### 企業改進

改進了 GitHub 組織政策執行的可靠性。當簽入多個帳戶時，政策現在根據偏好的 GitHub Copilot 帳戶正確套用。即使在啟動時臨時網路無法使用，組織政策現在也能被一致執行。

---

## 擴充功能和 API

擴充功能作者的新構成部分。

### GitHub Pull Requests

檢視擴充功能 0.128.0 版本的變更紀錄。

### 完成的快速輸入按鈕位置 API

當你在 `QuickPick` 或 `InputBox` 上設定 `buttons` 屬性時，你現在可以使用新的 `location` 屬性指定每個按鈕的位置：
- `Title`：頂部標題區域（預設）
- `Inline`：在輸入框右側呈現
- `Input`：在輸入框內部右側呈現

### 完成的快速輸入按鈕切換 API

你現在可以在快速輸入中透過在 `QuickInputButton` 上設定 `toggle` 屬性為 `{ checked: boolean }` 來建立切換按鈕。

### Proposed API

#### 聊天模型提供者設定

新的Proposed API 使聊天模型提供者擴充功能能夠透過 `languageModelChatProviders` 貢獻點宣告其設定需求。VS Code 為使用者提供內建 UI 以輸入其設定，並透過 API 將此設定傳遞給擴充功能。

簡單設定範例：

```json
{
  "contributes": {
    "languageModelChatProviders": [
      {
        "vendor": "my-provider",
        "displayName": "My Provider",
        "configuration": {
          "properties": {
            "apiKey": {
              "type": "string",
              "secret": true,
              "description": "API key for My Provider",
              "title": "API Key"
            }
          },
          "required": ["apiKey"]
        }
      }
    ]
  }
}
```

具有自訂模型的進階設定範例：

```json
{
  "contributes": {
    "languageModelChatProviders": [
      {
        "vendor": "my-provider",
        "displayName": "My Provider",
        "configuration": {
          "properties": {
            "apiKey": {
              "type": "string",
              "secret": true,
              "description": "API key for authentication",
              "title": "API Key"
            },
            "models": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "id": {
                    "type": "string",
                    "description": "Unique model identifier"
                  },
                  "name": {
                    "type": "string",
                    "description": "Display name for the model"
                  },
                  "url": {
                    "type": "string",
                    "description": "Model endpoint URL"
                  },
                  "maxInputTokens": {
                    "type": "number",
                    "description": "Maximum input tokens supported by the model"
                  },
                  "maxOutputTokens": {
                    "type": "number",
                    "description": "Maximum output tokens supported by the model"
                  },
                  "toolCalling": {
                    "type": "boolean",
                    "description": "Whether the model supports tool calling"
                  },
                  "vision": {
                    "type": "boolean",
                    "description": "Whether the model supports vision capabilities"
                  }
                },
                "required": ["id", "name", "url", "maxInputTokens", "maxOutputTokens"]
              }
            }
          },
          "required": ["apiKey"]
        }
      }
    ]
  }
}
```

註冊語言模型聊天提供者時：

```typescript
vscode.lm.registerLanguageModelChatProvider('my-provider', {
  provideLanguageModelResponse: (
    messages,
    options,
    extensionToken,
    configuration,
    token
  ) => {
    const apiKey = configuration.apiKey;
    const models = configuration.models;
  }
});
```

`secret` 屬性指示欄位應安全地儲存並在 UI 中隱蔽。

提案：vscode.proposed.lmConfiguration.d.ts

#### 聊天提示檔案 API

新的Proposed API 使擴充功能能夠貢獻動態聊天資源。擴充功能可以透過程式設計方式提供提示檔案、自訂 Agent、指令和技能。

```typescript
vscode.chat.registerSkillProvider({
  onDidChangeSkills: onDidChangeEvent,
  provideSkills(context, token): ChatResource[] {
    return [{ uri: vscode.Uri.parse('my-extension:/skills/debugging/SKILL.md') }];
  }
});

// 其他資源類型也存在類似方法：
// - registerCustomAgentProvider()
// - registerInstructionsProvider()
// - registerPromptFileProvider()
```

提案：vscode.proposed.chatPromptFiles.d.ts

#### 聊天項目控制器 API

我們持續在聊天工作階段項目 API 上進行迭代。新的基於控制器的 API 讓擴充功能推送變更至 VS Code，而不是讓 VS Code 提取它們。

```typescript
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  const controller = vscode.chat.createChatSessionItemController(
    'myExtension.chatSessions',
    async (token: vscode.CancellationToken) => {
      const sessions = await fetchSessionsFromBackend();
      const items = sessions.map(session =>
        controller.createChatSessionItem(
          vscode.Uri.parse(`my-scheme://session/${session.id}`),
          session.title
        )
      );
      controller.items.replace(items);
      setTimeout(() => {
        const currentTime = new Date().toLocaleTimeString();
        for (const item of controller.items) {
          item.label = `${item.label} - ${currentTime}`;
        }
      }, 10000);
    }
  );

  controller.onDidChangeChatSessionItemState(item => {
    console.log(`Session ${item.label} archived: ${item.archived}`);
  });
}
```

#### 聊天輸出渲染器 API 更新

我們持續致力於聊天輸出渲染器 API。我們現在將渲染器作為 `ChatOutputWebview` 而不僅僅直接傳遞 `Webview` 傳遞。這讓擴充功能能夠監視 webview 何時被處置。

查看聊天輸出渲染器樣本擴充功能以了解詳情。

#### 可攜式模式偵測

新的 `env.isAppPortable` 屬性讓擴充功能偵測到 VS Code 是否在可攜式模式下執行。

```typescript
if (vscode.env.isAppPortable) {
  // 在可攜式模式下執行
}
```

提案：vscode.proposed.envIsAppPortable.d.ts

---

## 工程

### macOS 的 DMG 映像

VS Code 現在為 macOS 提供 DMG 映像，以提供原生拖曳安裝體驗。

### Windows 11 右鍵選單

在使用右鍵選單支援安裝 VS Code 時，在 Windows 11 檔案總管中右鍵點擊檔案或資料夾現在會在頂層右鍵選單中新增項目，不需選取 `Show more options`。

### Windows 的重新設計安裝佈局

我們重新設計了 Windows 上的安裝佈局，以解決與應用程式內更新相關的長期可靠性問題。新實作汲取 Chromium 更新用戶端的靈感，並使用版本化套件路徑。

### 避免 macOS 的連續更新

如果在前一個更新仍然擱置時有新更新可用，VS Code 現在會使前一個更新失效，並繼續套用新更新。

### Copilot 擴充功能已棄用

GitHub Copilot 擴充功能已棄用。所有 AI 功能現在完全由 GitHub Copilot Chat 擴充功能提供。當你更新 VS Code 時，已棄用的 Copilot 擴充功能會自動解除安裝。

### 從 npm 套件使用 codicon

Codicon 現在透過 `@vscode/codicons` npm 套件使用，而不是直接打包。

---

## 重要修正

- vscode#276558 - 修復設定 `editor.hover.enabled` 為 `onModifierKeyPressed` 時按修飾鍵不立即觸發懸停的問題
- vscode#58814 - 洩漏檔案描述符至終端機程序

---

## 感謝

### 問題追蹤貢獻

對我們問題追蹤的貢獻：
- @gjsjohnmurray (John Murray)
- @RedCMD (RedCMD)
- @IllusionMH (Andrii Dieiev)
- @albertosantini (Alberto Santini)

### Pull Request

對 `vscode` 的貢獻：
- @ChaseKnowlden：Hover on keyboard modifier should trigger instantly PR #276582
- @dalisoft (Davlatjon Sh)：fix(typescript): `tsserver.useSyntaxServer.always` description PR #286476
- @hkleungai (Jimmy Leung)：vscode-dts: Fix typedoc for WebviewPanel.dispose() PR #289071
- @Infro (John Heilman)：If the users selects a language, let's have it actually choose the language they selected. (Yaml vs yaml) PR #288153
- @Ishiezz (Isha Singh)：Fix: Do not suggest implicit activation message when engine does not support it PR #281302
- @KanishkRanjan (Kanishk Ranjan)：fix: stabilize settings tree and also fixes during startup to stop ghost scrolls from early extension registrations. PR #278931
- @kiofaw (kiofaw)：fix: replace AsyncIterableObject with AsyncIterableProducer PR #288079
- @lucas-gomes-santana (Lucas Gomes Santana)：Improve snippet case transforms suport for non-Latin scripts (fix: #286165) PR #287150
- @newminkyung (minkyung)：fix: Screencast Mode - keyboard overlay timeout PR #238860
- @RedCMD (RedCMD)：fix: VB `increaseIndentPattern` PR #291176
- @SimonSiefke (Simon Siefke)
  - fix: memory leak in folder configuration PR #279230
  - fix: memory leak in abstract task service PR #289863
- @tamuratak (Takashi Tamura)
  - Optimize rendering performance by scheduling DOM updates at the next animation frame in NativeEditContext and TextAreaEditContext PR #285906
  - fix: prevent rendering thinking part for final answer in chat list PR #288178
- @vedbhadani (Ved BHadani)：Automatic activation event for chat context provider PR #280677
- @daviddossett (David Dossett)：Polish buttons and input PR #280457

對 `vscode-copilot-chat` 的貢獻：
- @24anisha (Anisha Agarwal)：Search subagent - set configurable exp variables PR #3205
- @alexandear (Oleksandr Redko)：tools: update message about issue reporting for validation failures PR #3113
- @bstee615 (Benjamin Steenhoek)：Log aggressiveness level and user happiness score to telemetry PR #2897
- @DanielFabian (Daniel Fabian)：Add short-lived cache for documents and filter by language in linkifier. PR #2211
- @kevin-m-kent：Fix prompttypes measure PR #2799

對 `vscode-explorer-command` 的貢獻：
- @ArcticLampyrid (ArcticLampyrid)：fix: use ShellExecuteW instead of CreateProcessW, allowing UAC dialog PR #17

對 `vscode-js-debug` 的貢獻：
- @nayeemrmn (Nayeem Rahman)：fix: don't duplicate --allow-all for deno debug configuration PR #2308

對 `vscode-python-environments` 的貢獻：
- @renan-r-santos (Renan Santos)：Fix activation icon state when using `shellStartup` PR #837
- @StellaHuang95 (Stella Huang)
  - Fix `python.defaultInterpreterPath` setting not being applied to new workspaces PR #1110
  - Add deprecation notes to some settings PR #1100

對 `vscode-windows-registry` 的貢獻：
- @thegecko (Rob Moran)：Minor code example fix PR #33

對 `language-server-protocol` 的貢獻：
- @asukaminato0721 (Asuka Minato)：Add harper PR #2222

對 `python-environment-tools` 的貢獻：
- @elprans (Elvis Pranskevichus)：Fix env duplication on merged-usr systems PR #200

---

我們真的很感謝人們盡快試用我們的新功能，所以請經常回訪此處並了解新增功能。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Multi-Agent Development | 多 Agent 開發 |
| Chat UX | 聊天使用體驗 |
| Message Steering and Queueing | 訊息引導與排隊 |
| Add to Queue | 加入佇列 |
| Steer with Message | 使用訊息引導 |
| Stop and Send | 停止並傳送 |
| Thinking Tokens | 思考 Token |
| Mermaid Diagrams | Mermaid 圖表 |
| Ask Questions Tool | 提出問題工具 |
| Plan Agent | 計畫 Agent |
| Context Window | 上下文視窗 |
| Inline Chat | 行內聊天 |
| Model Picker | 模型選擇器 |
| Terminal Command Output | 終端機命令輸出 |
| Agent Session Management | Agent 工作階段管理 |
| Session Type Picker | 工作階段類型選擇器 |
| Agent Sessions View | Agent 工作階段檢視 |
| Agent Status Indicator | Agent 狀態指示器 |
| Subagent | 子 Agent |
| Search Subagent | 搜尋子 Agent |
| Cloud Agent | 雲端 Agent |
| Background Agent | 背景 Agent |
| Agent Sessions Welcome Page | Agent 工作階段歡迎頁面 |
| Agent Customization | Agent 自訂 |
| Agent Hooks | Agent 掛鉤 |
| Skills as Slash Commands | 技能作為斜線命令 |
| Agent Skills | Agent 技能 |
| Organization-wide Instructions | 組織層級指令 |
| Custom Agent File Locations | 自訂 Agent 檔案位置 |
| Multiple Model Support | 多模型支援 |
| Chat Customization Diagnostics | 聊天自訂項目診斷 |
| Language Models Editor | 語言模型編輯器 |
| Agent Customization Skill | Agent 自訂技能 |
| Agent Extensibility | Agent 擴充性 |
| Claude Compatibility | Claude 相容性 |
| Agent Orchestration | Agent 協調 |
| Claude Agent | Claude Agent |
| MCP Apps | MCP 應用程式 |
| Copilot Memory | Copilot Memory |
| External Indexing | 外部索引 |
| Terminal Sandboxing | 終端機沙箱 |
| Terminal Tool Lifecycle | 終端機工具生命週期 |
| Terminal Auto-approval | 終端機自動核准 |
| Sticky Scroll | Sticky Scroll |
| Kitty Keyboard Protocol | Kitty 鍵盤協定 |
| Bracket Match Foreground | 括號比對前景色 |
| Ghost Text | Ghost Text |
| Snippet File Patterns | 程式碼片段檔案模式 |
| Shebang Language Detection | Shebang 語言偵測 |
| Integrated Browser | 整合式瀏覽器 |
| Quick Input Button Location API | 快速輸入按鈕位置 API |
| Quick Input Button Toggle API | 快速輸入按鈕切換 API |
| Chat Model Provider Configuration | 聊天模型提供者設定 |
| Chat Prompt Files API | 聊天提示檔案 API |
| Chat Item Controller API | 聊天項目控制器 API |
| Chat Output Renderer API | 聊天輸出渲染器 API |
| Portable Mode Detection | 可攜式模式偵測 |
| YAML Frontmatter | YAML 前置資料 |
| Prompt Files | 提示檔案 |
| Contribution Point | 貢獻點 |
| Workspace Trust | 工作區信任 |
| Auto-approval | 自動核准 |
| Deterministic | 確定性 |
| Audit Trail | 稽核軌跡 |

---

*資料來源：[Visual Studio Code 1.109 發行說明](https://code.visualstudio.com/updates/v1_109)*
