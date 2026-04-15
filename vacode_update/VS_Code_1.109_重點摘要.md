# VS Code 2026 年 1 月更新（版本 1.109）— 重點摘要

> 發布日期：2026 年 2 月 4 日
> 來源：[官方更新頁面](https://code.visualstudio.com/updates/v1_109)

---

## 核心主題：多代理開發的大本營

版本 1.109 將 VS Code 進一步發展為**多代理開發（Multi-Agent Development）的大本營**。此版本引入多代理工作流程、整合 Anthropic 的 Claude、使技能系統正式可用、並新增終端機命令沙盒。

---

## 重點功能一覽

### 一、聊天 UX 改進

| 功能 | 說明 |
|------|------|
| **串流改進** | 更快的串流速度，搭配 Thinking Token，回應更加靈敏 |
| **思考過程可視化** | Claude 模型的推理過程可即時顯示，支援「詳細」或「精簡」模式（`chat.thinking.style`） |
| **重新設計的行內聊天** | Inline Chat 經重新設計，不再遮擋您的工作 |
| **排隊傳送訊息** | 請求進行中可傳送後續訊息，送出按鈕變為下拉選單，含「加入佇列」選項 |
| **互動式 Mermaid 圖表** | 聊天回應中可渲染互動式 Mermaid 圖表（流程圖、序列圖等），支援平移和縮放 |

### 二、代理工作階段管理

- **工作階段類型選擇器**：可在本地、背景和雲端代理之間無縫切換
- **代理工作階段檢視**：全新側邊欄檢視，一處管理所有代理工作階段的狀態
- **並行子代理（Subagents）**：`runSubagent` 工具可獨立運行子代理，且支援並行執行，加速可拆分的獨立任務

### 三、代理自訂

- **代理技能（Agent Skills）正式可用**：將領域專業知識打包為可重複使用的工作流程，作為斜線命令在聊天中使用
- **代理協調（Agent Orchestrations）**：自訂代理、子代理和細粒度調用控制作為一級架構模式
- **組織層級自訂**：全組織指令確保一致性
- **工作區初始化（/init）**：代理自動探索現有 AI 慣例（如 `copilot-instructions.md`、`AGENTS.md`）、分析專案結構和編碼模式，產生量身定制的工作區指令

### 四、第三方代理整合

- **Claude 代理支援（預覽版）**：整合 Anthropic 官方 Claude Agent SDK，使用 GitHub Copilot 訂閱的 Claude 模型
- **OpenAI Codex 支援**：支援在本地或雲端運行 Codex 代理
- **跨工具設定共享**：VS Code 直接讀取 Claude 設定檔案，代理、技能、指令和掛鉤可跨工具共用

### 五、代理掛鉤（Agent Hooks）（預覽版）

在代理工作階段的關鍵生命週期點執行自訂 Shell 命令，支援八種掛鉤事件：

- `PreToolUse` / `PostToolUse`：攔截工具呼叫
- `SessionStart` / `SessionStop`：工作階段生命週期
- `SubagentStart` / `SubagentStop`：追蹤巢狀代理
- 與 Claude Code 和 Copilot CLI 使用相同格式，可跨工具重用

### 六、安全性增強

- **終端機沙盒（實驗性）**：限制檔案系統存取僅限工作區資料夾，限制網路存取僅限可信網域（macOS/Linux）
- **自動核准規則**：對安全操作跳過確認，減少不必要提示，同時保持安全控制

### 七、MCP 應用程式（MCP Apps）

全新 MCP Apps 支援，工具呼叫可回傳互動式 UI 元件，直接在對話中渲染：

- 儀表板（Dashboards）
- 表單（Forms）
- 視覺化圖表（Visualizations）
- 多步驟工作流程

### 八、工作台與編輯器

- **實驗性主題**：新增「VS Code Light」和「VS Code Dark」實驗性主題（開發中）
- **括號比對顏色自訂**：使用 `editorBracketMatch.foreground` 色彩主題 Token 自訂比對括號的文字顏色

### 九、終端機改進

- Kitty 鍵盤協定支援
- win32-input-mode 支援
- 新的 SGR 跳脫序列，改善文字格式化

---

## 更新歷程（Recovery Releases）

| 版本 | 主要內容 |
|------|----------|
| 1.109.0 | 初始發布 |
| 1.109.1 | 修正問題 |
| 1.109.2 | 修正問題 |
| 1.109.3+ | 新增代理掛鉤（Agent Hooks） |
| 1.109.5+ | 背景代理改進：支援斜線命令、重新命名工作階段 |

---

*本摘要根據 VS Code 官方更新頁面及多個相關報導整理翻譯。建議參閱[原文](https://code.visualstudio.com/updates/v1_109)以獲取最完整的資訊。*
