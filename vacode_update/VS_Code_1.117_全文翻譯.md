# Visual Studio Code 1.117 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.117
**發行日期：** 2026 年 4 月 22 日
**原文：** https://code.visualstudio.com/updates/v1_117

---

歡迎使用 Visual Studio Code 1.117 版本。本次發行以 Agent 權限管理、終端機工具改善、Agent Host Protocol 協定強化以及 Agents 應用程式的自動更新為核心，持續深化 agent-native 開發體驗。

以下是本次發行的主要亮點：

- **Run VS Code Command Agent 工具**：新增命令白名單與更精細核准支援
- **Agent Sessions 排序**：可依建立或更新時間排序工作階段
- **編輯佇列中的聊天訊息**：佇列訊息新增右鍵 Edit 動作
- **終端機 Agent 工具擴展至前景終端機**：`send_to_terminal` 與 `get_terminal_output` 不再限於背景終端機
- **背景終端機通知改為系統通知**：提升可發現性與無障礙性
- **Autopilot 權限模式跨工作階段持續**：新增 `chat.permissions.default` 設定
- **Agent Host Protocol 子代理與代理團隊**：支援 worktree 與 Git 隔離
- **Copilot CLI 有意義分支名稱**：根據提示自動產生描述性分支名
- **Copilot CLI / Claude Code / Gemini CLI 認定為 Shell 類型**
- **Agents 應用程式 macOS 自動更新**
- **package.json 相依性懸停**：同時顯示已安裝版本與最新發佈版本

---

## GitHub Copilot

### Run VS Code Command Agent 工具 — 指令白名單與更精細核准

Run VS Code command Agent 工具已更新，新增對**允許清單（allowlisting）** 特定命令的支援，以及**更精細（narrower）的核准** 機制。

此功能讓使用者能夠更精確地控制 Agent 可以執行哪些 VS Code 命令。您可以將信任的命令加入白名單，Agent 執行這些命令時不需要每次都手動核准；而對於未列入白名單的命令，則仍會要求確認。這在自動化工作流程中，既提升了效率又保持了安全性。

### Agent Sessions 檢視排序

Agent Sessions 檢視（Agent Sessions view）新增**排序支援**，可依照以下方式排列工作階段：

- **建立時間（Created）**：依照工作階段建立的時間排序
- **更新時間（Updated）**：依照工作階段最後更新的時間排序

當您同時管理多個 Agent 工作階段時，此功能讓您能快速找到最近使用或最新建立的工作階段。

### 編輯已排入佇列的聊天訊息

您現在可以透過佇列訊息右鍵選單（context menu）中新增的**「編輯」（Edit）動作**，直接修改已排入佇列的聊天訊息。

此改進消除了「取消排程 → 重新輸入」的繁瑣流程。當您發現已排入佇列但尚未送出的訊息需要調整時，可以直接就地編輯。

### Autopilot 權限模式跨工作階段持續生效

Autopilot 權限模式現在**跨工作階段持續生效（persists across sessions）**，不再於每次新開工作階段時重設。

您可以使用新的 `chat.permissions.default` 設定來配置預設的權限等級。

此外，Agent Host 現在支援**自動核准工作階段配置（auto-approve session configuration）**，提供以下三種模式：

| 模式 | 說明 |
|------|------|
| **Default Approvals**（預設核准） | 每次工具呼叫皆需使用者確認 |
| **Bypass Approvals**（略過核准） | 自動核准所有工具呼叫，不顯示確認對話框，並在錯誤時自動重試 |
| **Autopilot（Preview）**（自動駕駛，預覽版） | 自動核准所有工具呼叫、自動回應問題，並持續自主運作直到任務完成 |

---

## 終端機（Terminal）

### Agent 工具擴展至前景終端機

`send_to_terminal` 和 `get_terminal_output` 兩個 Agent 工具，先前僅限於 Agent 自行建立的背景終端機，現在**也支援前景終端機（foreground terminals）**。

這代表 Agent 現在可以：

- 讀取終端機面板中**任何可見終端機**的輸出
- 向這些終端機送出輸入

例如，當您有一個正在執行的 REPL 或互動式腳本時，Agent 可以直接與之互動。

此外，當 Agent 向終端機送出輸入時，終端機輸出會在**短暫延遲後自動包含在結果中**，省去了額外的 Agent 輪次（saving an extra agent turn），讓互動流程更加流暢。

### 背景終端機命令完成通知改為系統通知

背景終端機命令完成現在以**系統通知（system notification）** 的方式呈現，不再僅以聊天回應中的行內文字（inline text）顯示。

此變更改善了兩個面向：

- **可發現性（discoverability）**：系統通知更容易被注意到
- **無障礙性（accessibility）**：螢幕閱讀器等輔助工具能更好地識別通知

### Copilot CLI、Claude Code、Gemini CLI 認定為 Shell 類型

**Copilot CLI**、**Claude Code** 與 **Gemini CLI** 現在被 VS Code 認定為終端機中的 Shell 類型（shell types）。VS Code 能夠正確識別這些 CLI 工具正在終端機中執行，並提供相應的整合體驗。

同時，Copilot CLI 工作階段現在會**標示是由 VS Code 建立還是由外部建立**，方便使用者區分工作階段來源。

### Copilot CLI — 有意義的分支名稱

Copilot CLI 為背景 Agent 工作階段建立 worktree 時，會**根據使用者的提示（prompt）產生有意義的分支名稱**。

過去 worktree 的分支名稱可能使用通用識別碼，現在會產生描述性的名稱，讓使用者更容易辨識每個 worktree 與其對應的任務或 Agent。

---

## Agent Host Protocol

### 子代理與代理團隊支援

Agent Host Protocol 新增對**子代理（subagents）** 與**代理團隊（agent teams）** 的支援。這讓多個 Agent 能夠在同一個工作流程中協作，主 Agent 可以將子任務委派給子代理執行。

### Worktree 與 Git 隔離

Agent Host 工作階段新增 **worktree 與 Git 隔離** 支援。每個子代理可以在獨立的 Git worktree 中運作，防止多個 Agent 平行執行時互相衝突，確保各自的變更不會影響主工作區或其他 Agent 的工作。

### 自動核准工作階段配置

Agent Host 支援自動核准工作階段配置，提供 Default Approvals、Bypass Approvals 與 Autopilot（Preview）三種模式（詳見上方「Autopilot 權限模式」章節）。

---

## VS Code Agents 應用程式

### macOS 自動更新

Agents 應用程式新增在 **macOS** 上的**自動更新（self-updating）** 支援。當有新版本可用時，應用程式可以自動更新，無需使用者手動下載與安裝。

---

## 工作區介面（Workbench）

### 輔助（浮動）視窗切換

新增從**輔助視窗（auxiliary windows / floating windows）切換回主視窗**的支援。當您使用浮動視窗功能時，可以更輕鬆地在主視窗與輔助視窗之間來回切換。

---

## 編輯器與語言（Editor & Languages）

### package.json 相依性懸停 — 顯示已安裝與最新版本

在 `package.json` 檔案中，相依性的**懸停資訊（dependency hover）** 現在會**同時顯示目前已安裝版本（installed version）** 與**最新發佈版本（latest published version）**。

先前懸停只會顯示單一版本資訊，現在開發者不需要離開編輯器、也不需要執行 `npm outdated`，即可直觀了解哪些套件有可用的更新。

---

## 新／更新設定摘要

| 設定 | 說明 |
|------|------|
| `chat.permissions.default` | 配置預設的權限等級（用於 Autopilot 等權限模式） |

---

## 新功能摘要

| 功能 | 說明 |
|------|------|
| Run VS Code Command 白名單 | Agent 工具支援命令允許清單與更精細核准 |
| Agent Sessions 排序 | 可依建立或更新時間排序工作階段 |
| 編輯佇列中訊息 | 右鍵選單新增 Edit 動作 |
| 前景終端機 Agent 工具 | `send_to_terminal` / `get_terminal_output` 支援前景終端機 |
| 終端機輸出自動包含 | Agent 送出輸入後自動附帶輸出，省去額外輪次 |
| 背景終端機系統通知 | 命令完成改以系統通知呈現 |
| Autopilot 跨工作階段 | 權限模式持續生效 |
| Auto-approve 三模式 | Default / Bypass / Autopilot（Preview） |
| 子代理與代理團隊 | Agent Host Protocol 新增支援 |
| Worktree / Git 隔離 | Agent Host 工作階段支援 |
| 有意義分支名稱 | Copilot CLI 依提示產生描述性分支名 |
| Shell 類型辨識 | Copilot CLI / Claude Code / Gemini CLI |
| CLI 工作階段來源標示 | 標示由 VS Code 或外部建立 |
| Agents App macOS 自更新 | 自動更新支援 |
| 輔助視窗切換 | 從浮動視窗切回主視窗 |
| package.json 懸停 | 同時顯示已安裝版本與最新版本 |

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Run VS Code Command | 執行 VS Code 命令 |
| Allowlisting | 允許清單（白名單） |
| Narrower Approvals | 更精細核准 |
| Agent Sessions View | Agent Sessions 檢視 |
| Sorting by Created / Updated | 依建立時間／更新時間排序 |
| Queued Chat Messages | 已排入佇列的聊天訊息 |
| Context Menu | 右鍵選單 |
| Edit Action | 編輯動作 |
| send_to_terminal | send_to_terminal 工具 |
| get_terminal_output | get_terminal_output 工具 |
| Foreground Terminals | 前景終端機 |
| Background Terminals | 背景終端機 |
| REPL | REPL（互動式直譯器） |
| Extra Agent Turn | 額外的 Agent 輪次 |
| System Notification | 系統通知 |
| Inline Text | 行內文字 |
| Discoverability | 可發現性 |
| Autopilot Permission Mode | Autopilot 權限模式 |
| Persists Across Sessions | 跨工作階段持續生效 |
| chat.permissions.default | chat.permissions.default 設定 |
| Auto-approve Session Configuration | 自動核准工作階段配置 |
| Default Approvals | 預設核准 |
| Bypass Approvals | 略過核准 |
| Autopilot (Preview) | 自動駕駛（預覽版） |
| Agent Host Protocol | Agent Host Protocol（協定） |
| Subagents | 子代理 |
| Agent Teams | 代理團隊 |
| Worktree Isolation | Worktree 隔離 |
| Git Isolation | Git 隔離 |
| Copilot CLI | Copilot CLI |
| Claude Code | Claude Code |
| Gemini CLI | Gemini CLI |
| Shell Types | Shell 類型 |
| Meaningful Branch Names | 有意義的分支名稱 |
| Created by VS Code or Externally | 由 VS Code 或外部建立 |
| Self-updating | 自動更新 |
| Agents App | Agents 應用程式 |
| Auxiliary Windows | 輔助視窗 |
| Floating Windows | 浮動視窗 |
| Main Window | 主視窗 |
| Dependency Hover | 相依性懸停（資訊） |
| Installed Version | 已安裝版本 |
| Latest Published Version | 最新發佈版本 |
| package.json | package.json |

---

*資料來源：VS Code 1.117 發行說明 (https://code.visualstudio.com/updates/v1_117)*
