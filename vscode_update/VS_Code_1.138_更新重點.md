# VS Code 1.138 更新重點

> 原文：<https://code.visualstudio.com/updates/v1_138>
> 發布日期：2026 年 9 月 16 日

## 本版主軸

1.138 幾乎全部集中在 Agents 視窗與 agent host：讓 agent 在專案自己的開發環境裡工作、給 Codex 工作階段更多彈性，並讓已完成的工作階段自動整理乾淨。原文註明這份版本說明是用 GitHub Copilot 產生的，可能含有不精確之處。

## 官方三大亮點

1. **在 Dev Container 中執行 agent 工作階段**：在本機 Dev Container 裡用專案的工具與相依套件執行 agent，不再依賴本機環境。需要安裝 Docker，功能逐步推出。
2. **擴充 Codex harness**：Codex 可用 GitHub Copilot 或 ChatGPT 訂閱、同一個工作階段可在 ChatGPT 應用程式與 VS Code 之間接續、可重用 ChatGPT 應用程式的電腦操作設定，並能使用 VS Code 的內建、擴充套件與 MCP 工具。
3. **工作階段清理（預覽）**：PR 全部合併的閒置工作階段會建議標記為完成，並可設定自動標記完成與寬限期後自動刪除（預設關閉）。

## 其他值得注意的更新

- Codex quick chat 也能像上一版的 Copilot 一樣附加本機資料夾，轉成工作區工作階段而不遺失對話。
- Claude 與 Codex 的 agent SDK 只要缺失就會顯示下載提示，不再只在未登入的設定流程中出現。
- 可從 agent 工作階段用單一表單建立 pull request；「Agent Merge」為實驗性選項。
- 應用程式圖示上的徽章可顯示哪些工作階段需要處理（預覽）。
- 統一的工作區與儲存庫選擇器，把本機資料夾、GitHub、Cloud 與遠端目標放在同一份清單（實驗性）。
- Agents 視窗的 chat 背景可依深色／淺色主題分別設定圖片與版面配置（實驗性）。
- Voice Mode 可找出、切換並回報 agent 工作階段狀態（實驗性）。

## 棄用與移除

- `chat.agentSessions.backgroundImageLayout` 由 preferredDark／preferredLight 兩個設定取代。
- 「Chat: Clear Background」命令移除，改用「Chat: Set Background...」中的「No Background」。

## 社群貢獻

`vscode` 有 9 位貢獻者（含記憶體洩漏、主程序 OOM、Codex MCP 工具進度、commit 訊息 UTF-8 等修正），`vscode-chat-customizations-evaluation` 1 位，issue tracking 4 位。

## 其他資訊

- 頁面頂端公告：《The Story of VS Code》影片、Agent Host 架構部落格文章（2026 年 8 月 26 日）、GitHub Copilot Day 直播（9 月 10 日）。
- VS Code 逐步推送給所有使用者，可用 Check for Updates 立即取得，或下載 Insiders 每夜建置版搶先體驗。
