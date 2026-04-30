# Visual Studio Code 1.117 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.117
**發行日期：** 2026 年 4 月 22 日
**原文：** https://code.visualstudio.com/updates/v1_117

---

歡迎使用 Visual Studio Code 1.117 版本。本次發行為 Copilot Enterprise 與 Business 使用者新增功能，並進一步改善 VS Code 中的 Agent 體驗。以下是本次發行的亮點：

- **[BYOK 支援 Business 與 Enterprise](#自帶金鑰支援-copilot-business-與-enterprise)**：在 VS Code 聊天中直接連接您自己的 API 金鑰，使用偏好或專門的模型。
- **[漸進式聊天渲染](#聊天回應漸進式渲染實驗性)**：體驗更流暢的聊天回應串流。
- **[終端機改善](#終端機)**：從任何終端機設定檔啟動 Copilot CLI。

Happy Coding!

---

## 自帶金鑰支援 Copilot Business 與 Enterprise

團隊經常因合規、效能或成本原因需要特定模型，但在不同工具之間切換以使用這些模型會拖慢開發者的速度。自帶語言模型金鑰（BYOK）讓 Copilot Business 與 Enterprise 使用者可以連接自己的 API 金鑰，支援 OpenRouter、Ollama、Google、OpenAI 等供應商，在 VS Code 聊天中直接使用這些模型。

預設情況下，BYOK 為啟用狀態，管理員可透過 GitHub.com 上 [Copilot 政策設定](https://github.com/settings/copilot/features)中的 **Bring Your Own Language Model Key** 政策來停用。這讓管理員可以控制哪些模型供應商可用於其組織，同時讓開發者留在既有的工作流程中。

政策啟用後，組織成員可以[從內建供應商新增模型](https://code.visualstudio.com/docs/copilot/customization/language-models#_bring-your-own-language-model-key)或安裝語言模型供應商擴充功能。

---

## 聊天體驗

### 聊天回應漸進式渲染（實驗性）

聊天回應透過漸進式渲染（incremental rendering）變得更加流暢與自然，內容以區塊為單位（block-by-block）串流，Token 到達時附帶可選動畫。這種實驗性方式取代預設的計時器式渲染，在每個區塊準備好時即進行渲染，降低較長回應的感知等待時間。

透過以下設定配置漸進式回應渲染：

- `chat.experimental.incrementalRendering.enabled`：啟用或停用漸進式回應渲染，串流聊天回應時附帶可選的區塊層級動畫。預設：`true`。
- `chat.experimental.incrementalRendering.animationStyle`：配置漸進式回應渲染的動畫風格。選項：`none`、`fade`、`rise`、`blur`、`scale`、`slide`、`reveal`。預設：`fade`。
- `chat.experimental.incrementalRendering.buffering`：配置漸進式回應渲染期間內容在渲染前的緩衝方式。較低的緩衝等級渲染更快，但可能顯示不完整的句子或部分形成的 Markdown。選項：`off`、`word`、`paragraph`。預設：`word`。

### 依最近活動排序 Agent 工作階段

當您累積了許多 Agent 工作階段時，找到正確的那一個可能會很困難。**Agent Sessions** 檢視支援依工作階段的建立時間或最後更新時間排序，讓您可以快速回到中斷的地方繼續。

### 背景終端機命令的系統通知

當 Agent 在背景執行一個長時間運作的終端機命令時，很容易失去對其進度的追蹤。這些命令現在會以**系統通知**的形式出現在聊天回應中，讓您無需切換到終端機即可監控其狀態。

---

## Agent 體驗

### Visual Studio Code Agents（Insiders）

> **注意**：Visual Studio Code Agents 應用程式目前為預覽版，僅在安裝 VS Code Insiders 時可用。

Visual Studio Code Agents 應用程式是與 VS Code Insiders 一同發行的伴隨應用程式，提供一個專注的、agent-native 環境，讓您可以跨儲存庫執行平行工作階段、內嵌審查 diff，並反覆處理多步驟編碼任務。此應用程式在 [1.115](https://code.visualstudio.com/updates/v1_115#_visual-studio-code-agents-preview) 中推出，並持續根據回饋進行演進。

本次發行的更新：

- **建立子工作階段**：在工作階段標題中選取 **+** 即可從目前工作階段衍生子工作階段。這對於在上下文中啟動額外工作（例如平行研究或程式碼審查）很方便，不會失去您在父工作階段中的位置。
- **行內變更渲染**：改善了行內變更的渲染方式，讓您在 Agent 編輯程式碼時更容易掃視與比較 diff。
- **更新體驗**：跨作業系統的更新流程改善，讓保持在最新版本更加順暢。
- **主題、聊天回應與 UX 精進**：持續改進主題、工作階段清單與回應渲染，以及應用程式整體的 UX。

如同先前版本，您可以透過相同方式開啟應用程式：

- 從作業系統的開始功能表或應用程式資料夾啟動 **Visual Studio Code Agents - Insiders**。
- 從 VS Code Insiders 命令面板執行 **Chat: Open Agents Application**。
- 從 VS Code Insiders 歡迎頁面選取 **Try out the new Agents app**。

---

## 終端機

### 從自訂終端機設定檔啟動 Copilot CLI

Copilot CLI 終端機設定檔現在可以從終端機面板啟動，即使您的預設終端機設定檔設為非預設 Shell，例如 macOS 或 Linux 上的 `fish`，或 Windows 上的 Git Bash。

先前，在此配置下從終端機設定檔選擇器中選取 **GitHub Copilot CLI** 會產生 `No terminal profile options provided for id 'copilot-cli'` 錯誤，且終端機無法啟動。

### Agent CLI 的終端機標題

像 Copilot CLI、Claude Code 和 Gemini CLI 這樣的 Agent CLI 通常作為 `node` 程序執行，這意味著終端機標題會顯示通用的 `node` 標籤。這讓人很難分辨每個終端機中執行的是哪個 Agent。終端機現在將這些 Agent CLI 偵測為獨立的 Shell 類型，並使用 CLI 發出的 OSC 標題序列作為終端機標題，讓每個終端機清楚地標示它正在承載的 Agent。

改善後的偵測涵蓋 macOS、Linux 和 Windows 上的 Copilot CLI、Claude Code 和 Gemini CLI。Codex 目前在 macOS 上尚未被偵測，因為它目前不會發出 OSC 標題序列。此行為預設啟用，可透過 `terminal.integrated.tabs.allowAgentCliTitle` 設定切換。

---

## 語言

### TypeScript 6.0.3

本次發行包含 [TypeScript 6.0.3](https://github.com/microsoft/typescript/issues?q=milestone%3A%22TypeScript%206.0.3%22) 修復版本。此次小版本更新修正了數個匯入（import）錯誤與回歸。

---

## 已棄用的功能與設定

### 本次發行的新棄用項目

（無）

### 即將棄用的項目

（無）

---

## 感謝

Issue 追蹤貢獻者：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

`vscode` 程式碼貢獻者：

- [@abadawi591 (abadawi-msft)](https://github.com/abadawi591)：Abadawi/send has image to router [PR #308321](https://github.com/microsoft/vscode/pull/308321)
- [@andysharman](https://github.com/andysharman)：修正：預設工作階段模式實驗在首次工作階段時未套用 [PR #308905](https://github.com/microsoft/vscode/pull/308905)
- [@bocan (Chris Funderburg)](https://github.com/bocan)：修正 launch.json configurations 陣列中 null 項目導致的崩潰 [PR #308235](https://github.com/microsoft/vscode/pull/308235)
- [@jamestut (James Nugraha)](https://github.com/jamestut)：在終端機編輯器分割中 await openEditor 以防止影子分頁 [PR #309167](https://github.com/microsoft/vscode/pull/309167)
- [@maruthang (Maruthan G)](https://github.com/maruthang)
  - 修正（tasks）：在 taskDefinitions 貢獻 schema 中為必要屬性新增懸停描述 (#275670) [PR #310764](https://github.com/microsoft/vscode/pull/310764)
  - 修正（debug）：以解析後的位址識別指令斷點，允許在 instructionReference 變更時移除 (#289678) [PR #310763](https://github.com/microsoft/vscode/pull/310763)
  - 修正（terminal-chat）：去重終端機工具工作階段註冊以防止監聽器洩漏 (#309906) [PR #310740](https://github.com/microsoft/vscode/pull/310740)
  - 修正（chat）：在 renderWelcomeViewContentIfNeeded 中防範未釋放的 input part (#310356) [PR #310822](https://github.com/microsoft/vscode/pull/310822)
  - 修正：防止語言狀態中重複 status ID 導致的監聽器洩漏 (#309042) [PR #309159](https://github.com/microsoft/vscode/pull/309159)
  - 修正（chat）：當回應被取消時，取消進行中的串流工具呼叫 (#288701) [PR #310979](https://github.com/microsoft/vscode/pull/310979)
- [@matts1 (Matt)](https://github.com/matts1)：功能：支援切換至主視窗 [PR #306573](https://github.com/microsoft/vscode/pull/306573)
- [@NikolaRHristov (Nikola Hristov)](https://github.com/NikolaRHristov)：修正：將 protected 成員改為 public 以解決 mangler 建置錯誤 [PR #310195](https://github.com/microsoft/vscode/pull/310195)
- [@OscarPalafox (Oscar Palafox Verna)](https://github.com/OscarPalafox)：統一 theme-defaults 中新 2026 年的 include 路徑 [PR #309880](https://github.com/microsoft/vscode/pull/309880)
- [@RieBi (Sviatoslav Zubar)](https://github.com/RieBi)：除了最新發佈版本外，另外顯示目前已安裝的套件版本 [PR #308569](https://github.com/microsoft/vscode/pull/308569)
- [@yogeshwaran-c (Yogeshwaran C)](https://github.com/yogeshwaran-c)
  - json：修正語言模型快取在容量達上限時就驅逐而非溢出時才驅逐 [PR #309176](https://github.com/microsoft/vscode/pull/309176)
  - 當 openDebug 為 openOnDebugBreak 時，不在首次工作階段啟動時開啟除錯檢視 [PR #309133](https://github.com/microsoft/vscode/pull/309133)
  - testing：對齊壓縮結果列上的右鍵選單與懸停列 [PR #309139](https://github.com/microsoft/vscode/pull/309139)
  - 為內建 CSS 伺服器採用 CodeAction 類型 [PR #310055](https://github.com/microsoft/vscode/pull/310055)

---

我們非常感謝大家在新功能準備就緒時便立即試用，請經常回來查看並了解最新動態。

> 如果您想閱讀 VS Code 先前版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Bring Your Own Key (BYOK) | 自帶金鑰（BYOK） |
| Copilot Business / Enterprise | Copilot Business / Enterprise |
| API Keys | API 金鑰 |
| Language Model Provider | 語言模型供應商 |
| OpenRouter, Ollama, Google, OpenAI | OpenRouter、Ollama、Google、OpenAI |
| Policy Settings | 政策設定 |
| Incremental Rendering | 漸進式渲染 |
| Block-by-block | 以區塊為單位 |
| Animation Style | 動畫風格 |
| Buffering | 緩衝 |
| Perceived Wait Time | 感知等待時間 |
| Agent Sessions View | Agent Sessions 檢視 |
| Sort by Created / Updated | 依建立時間／更新時間排序 |
| System Notifications | 系統通知 |
| Background Terminal Commands | 背景終端機命令 |
| VS Code Agents App | VS Code Agents 應用程式 |
| Companion App | 伴隨應用程式 |
| Agent-native Environment | Agent 原生環境 |
| Sub-session | 子工作階段 |
| Parent Session | 父工作階段 |
| Inline Change Rendering | 行內變更渲染 |
| Update Experience | 更新體驗 |
| Theming | 主題 |
| UX Polish | UX 精進 |
| Command Palette | 命令面板 |
| Copilot CLI | Copilot CLI |
| Terminal Profile | 終端機設定檔 |
| Terminal Profile Picker | 終端機設定檔選擇器 |
| Default Shell | 預設 Shell |
| Agent CLIs | Agent CLI |
| Claude Code | Claude Code |
| Gemini CLI | Gemini CLI |
| Codex | Codex |
| Shell Type | Shell 類型 |
| OSC Title Sequence | OSC 標題序列 |
| terminal.integrated.tabs.allowAgentCliTitle | terminal.integrated.tabs.allowAgentCliTitle 設定 |
| TypeScript 6.0.3 | TypeScript 6.0.3 |
| Recovery Release | 修復版本 |
| Import Bugs and Regressions | 匯入錯誤與回歸 |
| Deprecated | 已棄用 |

---

*資料來源：[Visual Studio Code 1.117 發行說明](https://code.visualstudio.com/updates/v1_117)*
