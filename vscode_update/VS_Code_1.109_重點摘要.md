# Visual Studio Code 1.109 版本重點摘要

**版本：** 1.109（2026 年 1 月）
**發行日期：** 2026 年 2 月 4 日
**原文：** https://code.visualstudio.com/updates/v1_109

---

**安全更新**：更新 1.109.1 修正安全問題。更新 1.109.2 修正聊天相關問題。更新 1.109.3 新增訊息引導與排隊、Agent 掛鉤、Claude 相容性、技能作為斜線命令。更新 1.109.4 修正問題。更新 1.109.5 改善背景 Agent（支援斜線命令、重新命名工作階段、Kitty 鍵盤支援向所有使用者開放）。

本次發行將 VS Code 進一步發展為**多代理開發的大本營**，涵蓋聊天 UX、Agent 工作階段管理、Agent 自訂、Agent 擴展性、Agent 最佳化、Agent 安全與信任、工作台與生產力、終端機增強、編碼與編輯器、擴充功能與 API 等十大面向。

## 一、聊天 UX

- **訊息引導與排隊（實驗性）**：請求進行中可傳送後續訊息，Send 按鈕變為下拉選單，提供三個選項：**Add to Queue**（排隊等候）、**Steer with Message**（引導當前請求讓步後立即處理）、**Stop and Send**（取消當前請求並立即傳送）。可拖放調整排隊順序
- **Anthropic 模型顯示思考 Token**：Claude 模型支援思考 Token，可選擇詳細或精簡思考樣式（`chat.thinking.style`），思考過程與工具呼叫交錯顯示，失敗的工具呼叫自動展開
- **聊天回應中的 Mermaid 圖表**：使用 `renderMermaidDiagram` 工具渲染互動式 Mermaid 圖表，支援平移縮放、在編輯器中開啟、複製圖表原始碼
- **Ask Questions 工具（實驗性）**：Agent 可使用 `askQuestions` 工具在聊天中提出釐清問題，支援單選/多選、自由文字輸入、推薦答案
- **Plan agent 改進**：新的結構化四階段迭代工作流程（Discovery → Alignment → Design → Refinement），可透過 `/plan` 呼叫
- **上下文視窗詳情**：聊天輸入區新增上下文視窗指示器，懸停可查看各類別的 Token 使用明細
- **行內聊天 UX 改版（Preview）**：新的觸發提示（`inlineChat.affordance`）和輕量化情境渲染（`inlineChat.renderMode`）
- **模型選擇器中的模型描述**：懸停或鍵盤聚焦模型時可一覽模型詳情
- **終端機命令輸出改善**：語法高亮（Node、Python、Ruby）、工作目錄顯示、命令意圖描述、輸出串流自動展開、終端機可互動輸入、一鍵刪除所有隱藏終端機
- **實驗性主題**：新增 VS Code Light 和 VS Code Dark 實驗性主題（開發中）
- **Edit Mode 預設隱藏**：使用 Agent 提供 Edit Mode 的超集能力

## 二、Agent 工作階段管理

- **工作階段類型選擇器**：聊天輸入區新增工作階段類型選擇器，可選擇 Agent 工作階段類型或將進行中的工作階段交接至不同 Agent 類型
- **Agent 工作階段檢視改善**：可調整大小的工作階段清單、多選批次操作、改進的堆疊檢視
- **Agent 狀態指示器**：命令中心的狀態指示器顯示進行中、未讀和需要注意的工作階段（`chat.agentsControl.enabled`）
- **子代理（Subagents）**：子代理可並行執行，顯示任務、自訂 Agent 和當前使用的工具等詳情，可展開查看完整資訊
- **搜尋子代理（實驗性）**：在隔離的 Agent 迴圈中迭代搜尋程式碼庫，保留主 Agent 的上下文視窗
- **雲端 Agent 改善**：模型選擇、第三方編碼 Agent（Claude、Codex，Preview）、自訂 Agent、多根工作區支援、Checkout 選項改善
- **背景 Agent 改善**：自訂 Agent、附加圖片作為上下文、多根工作區支援、每輪結束自動提交、斜線命令支援（1.109.5）、重新命名工作階段（1.109.5）
- **Agent 工作階段歡迎頁面（實驗性）**：`workbench.startupEditor` 設為 `agentSessionsWelcomePage`

## 三、Agent 自訂

- **Agent 掛鉤（Preview）**：在 Agent 生命週期關鍵點執行自訂 Shell 命令，支援八種掛鉤事件（含 `PreToolUse`、`PostToolUse`、`SessionStart`、`Stop`、`SubagentStart`、`SubagentStop`），與 Claude Code 和 Copilot CLI 使用相同格式
- **技能作為斜線命令**：Agent Skills 可作為斜線命令使用，支援 `user-invocable` 和 `disable-model-invocation` 前置資料屬性控制存取方式
- **使用 /init 設定工作區**：自動探索現有 AI 慣例、分析專案結構、產生工作區指令
- **Agent Skills 正式可用**：預設啟用，VS Code 在 `.github/skills`、`.claude/skills`、`~/.copilot/skills`、`~/.claude/skills` 中尋找技能定義，擴充功能可透過 `chatSkills` 貢獻點打包分發技能
- **組織層級指令**：GitHub 組織設定的自訂指令自動套用至聊天工作階段（`github.copilot.chat.organizationInstructions.enabled`）
- **自訂 Agent 檔案位置**：新設定 `chat.agentFilesLocations` 可指定額外搜尋目錄
- **自訂 Agent 調用控制**：新增 `user-invocable`、`disable-model-invocation`、`agents` 前置資料屬性
- **自訂 Agent 多模型支援**：可在前置資料中指定多個模型作為備援
- **聊天自訂項目診斷**：右鍵聊天檢視選擇 Diagnostics，查看所有已載入的自訂項目及錯誤
- **語言模型編輯器改善**：每個供應商多組設定、Azure 模型設定、管理供應商群組、鍵盤存取、`chatLanguageModels.json` 設定檔
- **語言模型設定**：Plan 實作的預設模型、行內聊天預設模型、Agent 交接指定模型
- **Agent 自訂技能（實驗性）**：教導 Agent 如何協助自訂 AI 編碼體驗

## 四、Agent 擴展性

- **Claude 相容性**：VS Code 直接讀取 Claude 設定檔（`CLAUDE.md`、`.claude/agents`、`.claude/skills`、`.claude/settings.json`），跨工具共用設定
- **Agent 協調**：使用自訂 Agent、子代理和調用控制建構多 Agent 協作工作流程
- **Claude Agent（Preview）**：整合 Anthropic 官方 Claude Agent SDK，使用 GitHub Copilot 訂閱的 Claude 模型
- **Anthropic 模型改善**：Messages API 與交錯思考、工具搜尋工具（`toolSearchTool`）、上下文編輯（實驗性）
- **MCP Apps 支援**：MCP 伺服器可在客戶端顯示豐富互動式 UI
- **自訂登錄基底 URL**：支援 MCP 伺服器清單檔案中的 `registryBaseUrl` 屬性

## 五、Agent 最佳化

- **Copilot Memory（Preview）**：跨工作階段儲存和回憶重要資訊，Agent 可自動識別何時儲存或擷取記憶
- **非 GitHub 工作區的外部索引（Preview）**：非 GitHub 託管的工作區可遠端建立索引以加速語意搜尋
- **讀取工作區外的檔案**：Agent 可在取得許可後讀取工作區外的檔案和目錄
- **效能改善**：長對話更流暢、相依任務並行處理

## 六、Agent 安全與信任

- **終端機沙盒（實驗性）**：限制檔案系統存取僅限工作區資料夾、限制網路存取（macOS/Linux）
- **終端機工具生命週期改善**：手動推送至背景、`timeout` 屬性、`awaitTerminal` 工具、`killTerminal` 工具
- **終端機自動核准**：預設自動核准 `Set-Location`、`dir`、`od`、`xxd`、`docker`、`npm`、`yarn`、`pnpm` 等安全命令

## 七、終端機增強

- **選擇性忽略 Sticky Scroll**：`terminal.integrated.stickyScroll.ignoredCommands` 可自訂忽略的命令
- **移除 winpty 支援**：Windows 10 1809 之前版本的終端機不再運作
- **受限工作區允許終端機**：`terminal.integrated.allowInUntrustedWorkspace` 設定
- **Kitty 鍵盤協定**：改善按鍵編碼限制，支援更多修飾鍵、按下/放開事件、消除歧義
- **win32 input mode（實驗性）**：Windows 專用的類似功能

## 八、編碼與編輯器

- **括號比對前景色**：新增 `editorBracketMatch.foreground` 色彩主題 Token
- **雙擊選取括號和字串內容**：在開/閉括號或引號旁雙擊可選取其中內容
- **TypeScript 重新命名建議**：輸入覆蓋既有宣告時也可提供重新命名建議
- **改善 Ghost Text 可見性**：短建議顯示虛線底線
- **程式碼片段檔案模式**：使用 `include` 和 `exclude` glob 模式控制程式碼片段出現的檔案
- **改善 Shebang 語言偵測**：正確偵測 `#!/usr/bin/env -S deno -A` 為 TypeScript

## 九、工作台與生產力

- **整合式瀏覽器（Preview）**：克服 iframe 限制，支援網站驗證、持久資料儲存、DevTools、Add element to chat
- **開啟工作區時還原編輯器**：`workbench.editor.restoreEditors` 控制是否還原編輯器
- **進階設定**：`workbench.settings.alwaysShowAdvancedSettings` 預設顯示進階設定
- **拖放匯入設定檔**
- **輸出頻道篩選改善**：支援否定模式和多重篩選
- **依來源篩選問題**：`source:ts`、`!source:cSpell`
- **擴充功能編輯器顯示設定預設值**
- **Git Worktree 包含額外檔案（實驗性）**：`git.worktreeIncludeFiles`
- **SCM 檢視全部摺疊**
- **Git: Delete 命令**
- **停用 Blame 編輯器裝飾懸停**
- **自動任務預設關閉**
- **無障礙改善**：動態串流聊天回應、穩定游標位置、新工作階段 ARIA 警示、改善工具呼叫資訊、朗讀游標位置命令
- **企業改善**：改善 GitHub 組織政策執行

## 十、擴充功能與 API

- **GitHub Pull Requests 擴充功能改善**
- **Quick Input Button Location API 定案**：`Title`、`Inline`、`Input` 位置
- **Quick Input Button Toggle API 定案**：`toggle` 屬性
- **Proposed API**：Chat Model Provider Configuration、Chat prompt files API、Chat item controller API、Chat output renderer API 更新、可攜式模式偵測

---

## 工程

- **macOS DMG 安裝映像**
- **Windows 11 頂層右鍵選單整合**
- **Windows 安裝佈局重新設計**：使用版本化套件路徑解決更新可靠性問題
- **macOS 避免連續更新**
- **Copilot 擴充功能已棄用**：所有 AI 功能由 Copilot Chat 擴充功能提供
- **Codicons 改從 npm 套件取用**

---

*資料來源：[Visual Studio Code 1.109 發行說明](https://code.visualstudio.com/updates/v1_109)*
