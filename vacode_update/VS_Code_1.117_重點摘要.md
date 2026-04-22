# Visual Studio Code 1.117 版本重點摘要

**版本：** 1.117（2026 年 4 月 22 日發行）
**類型：** 每週穩定版（Weekly Stable Release）
**主題：** Agent 權限管理與自動核准、終端機 Agent 工具擴展至前景終端機、Agent Host Protocol 子代理支援、Agents 應用程式自動更新

---

## 一、Run VS Code Command Agent 工具 — 指令白名單與更精細核准

- Run VS Code command Agent 工具新增支援**允許清單（allowlisting）**，可以指定特定命令
- 支援**更精細（narrower）** 的核准機制，讓使用者能更精確地控制 Agent 可以執行哪些 VS Code 命令

## 二、Agent Sessions 檢視排序

- Agent Sessions 檢視（Agent Sessions view）新增**排序支援**
- 可依照**建立時間（Created）** 或**更新時間（Updated）** 排序工作階段
- 方便管理大量工作階段時快速找到所需項目

## 三、編輯已排入佇列的聊天訊息

- 佇列中的聊天訊息現可透過右鍵選單（context menu）中新增的**「編輯」（Edit）動作** 進行修改
- 不再需要取消再重新輸入，提升多工操作時的效率

## 四、終端機 Agent 工具擴展至前景終端機

- `send_to_terminal` 與 `get_terminal_output` 兩個 Agent 工具，現在**不再限於 Agent 建立的背景終端機**
- 也支援**前景終端機（foreground terminals）**
- Agent 可以讀取終端機面板中任何可見終端機的輸出，並向其送出輸入（例如執行中的 REPL 或互動式腳本）
- 當 Agent 向終端機送出輸入時，終端機輸出會在短暫延遲後**自動包含在結果中**，省去額外的 Agent 輪次

## 五、背景終端機命令完成通知改為系統通知

- 背景終端機命令完成現以**系統通知（system notification）** 方式呈現
- 不再僅以聊天回應中的行內文字顯示
- 改善了可發現性（discoverability）與無障礙性（accessibility）

## 六、Autopilot 權限模式跨工作階段持續生效

- Autopilot 權限模式現**跨工作階段持續生效（persists across sessions）**
- 可使用 `chat.permissions.default` 設定來配置預設權限等級
- Agent Host 支援自動核准工作階段配置，提供三種模式：
  - **Default Approvals**（預設核准）
  - **Bypass Approvals**（略過核准）
  - **Autopilot（Preview）**（自動駕駛，預覽版）

## 七、Agent Host Protocol — 子代理與代理團隊支援

- Agent Host Protocol 新增支援**子代理（subagents）** 與**代理團隊（agent teams）**
- Agent Host 工作階段支援 **worktree 與 Git 隔離**
- 每個子代理可在獨立的 Git worktree 中運作，避免多個 Agent 平行執行時互相衝突

## 八、Copilot CLI — 有意義的分支名稱

- Copilot CLI 為背景 Agent 工作階段建立 worktree 時，會**根據使用者的提示（prompt）產生有意義的分支名稱**
- 讓使用者更容易辨識每個 worktree 是由哪個任務或 Agent 建立

## 九、Copilot CLI、Claude Code、Gemini CLI 認定為終端機 Shell 類型

- **Copilot CLI**、**Claude Code** 與 **Gemini CLI** 現在被認定為終端機中的 Shell 類型（shell types）
- Copilot CLI 工作階段會標示是由 VS Code 建立或是由外部建立

## 十、Agents 應用程式 — macOS 自動更新

- Agents 應用程式新增在 **macOS** 上的**自動更新（self-updating）** 支援

## 十一、工作區介面 — 輔助（浮動）視窗切換

- 新增從**輔助視窗（auxiliary / floating windows）切換回主視窗** 的支援

## 十二、package.json 相依性懸停 — 顯示已安裝與最新版本

- `package.json` 檔案中的相依性懸停（dependency hover），現在會**同時顯示目前已安裝版本與最新發佈版本**
- 讓開發者不需離開編輯器即可了解套件更新狀態

---

*資料來源：VS Code 1.117 發行說明 (https://code.visualstudio.com/updates/v1_117)*
