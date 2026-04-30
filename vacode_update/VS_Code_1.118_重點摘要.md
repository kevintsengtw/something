# Visual Studio Code 1.118 版本重點摘要

**版本：** 1.118
**發行日期：** 2026 年 4 月 29 日
**原文：** https://code.visualstudio.com/updates/v1_118

---

本次發行擴展了 Copilot Agent 的使用場景並提升其效率。以下為官方列出的六大亮點：

## 一、Copilot CLI 工作階段遠端控制（實驗性）

- 可從 **GitHub.com** 或**行動裝置**遠端追蹤並控制正在執行的 Copilot CLI 工作階段
- 過去必須待在啟動工作階段的機器旁，若 Agent 暫停等待核准或提問，工作就會停滯
- 啟用 `github.copilot.chat.cli.remote.enabled` 設定後，在聊天中輸入 `/remote on` 即可開始
- 可用 `/remote` 查看狀態，`/remote off` 停用

## 二、程式碼庫搜尋與上下文（Codebase Search and Context）

- **語意索引（Semantic Indexing）已向所有使用者推出**：先前僅限 GitHub 或 ADO 儲存庫，現在所有工作區皆可使用。語意搜尋依「意義」搜尋，即使程式碼中從未出現 "authentication" 一詞，也能找到 `login`、`signIn`、`verifyCredentials` 等相關程式碼
- **GitHub 文字搜尋（`githubTextSearch`）**：新增跨 GitHub 儲存庫或整個組織的 grep 風格精確文字搜尋工具，與既有的語意搜尋工具 `githubRepo` 互補

## 三、Skill 專屬上下文（實驗性）

- 當 Skill 執行多步驟工具呼叫或引入大量參考資料時，這些輔助內容會擠壓主要聊天上下文，降低後續回應品質
- 新增 `context: fork` 屬性（在 `SKILL.md` frontmatter 中設定），讓 Skill 在專屬子代理上下文中執行，與主對話隔離
- 需啟用 `github.copilot.chat.skillTool.enabled` 設定

## 四、Chronicle — 聊天工作階段洞察（實驗性）

- 在本地 SQLite 資料庫中追蹤聊天互動，記錄工作階段中繼資料（分支、儲存庫、時間戳）、對話輪次、觸及的檔案與外部參考（PR、Issue、Commit）
- `/chronicle:standup`：產生過去 24 小時站立報告
- `/chronicle:tips`：分析 7 天使用紀錄，給予個人化建議
- `/chronicle [查詢]`：自然語言查詢工作階段歷程
- 需啟用 `github.copilot.chat.localIndex.enabled` 設定

## 五、企業控管 — 核准帳號組織政策

- 企業可使用 `ChatApprovedAccountOrganizations` 裝置政策，依據核准的 GitHub 組織成員資格來管控聊天與 AI 功能啟用
- 採用 fail-closed 行為：使用者登入核准組織帳號且帳號政策解析完成後，聊天功能才會啟用

## 六、Token 效率改善

- **提示快取效率**：策略性快取斷點放置（系統提示尾端、工具尾端、最近工具輪次尾端、對話輪次邊界），超過 93% 的請求內容可從快取重用
- **快取穩定的系統提示與工具清單**：移除跨請求的位元組漂移來源，如 `chat.experimental.symbolTools.cacheStable` 設定
- **快取友善的背景壓縮**：長工作階段的摘要重用相同快取上下文
- **最後兩則訊息斷點策略**：透過 `github.copilot.chat.anthropic.cacheBreakpoints.lastTwoMessages` 設定啟用
- **工具搜尋工具（Tool Search Tool）**：約 30 個核心工具始終載入（涵蓋約 88% 呼叫），其餘工具延遲載入。Anthropic 模型節省達 20%，正推出至 GPT-5.4 與 GPT-5.5
- **Agentic Search Tool**：以微調小型語言模型驅動的程式碼庫探索工具
- **Agentic Execution Tool**：專責終端機命令執行，每次呼叫上限 10 次終端機呼叫，過濾冗長輸出

---

## 其他重要更新

- **VS Code Agents 應用程式**：可從 VS Code Insiders 標題列直接開啟；新增 Web 用戶端（insiders.vscode.dev/agents）；與 VS Code 共享認證（Windows）、AI 自訂項目、工作區信任、最近資料夾、鍵盤快捷鍵；支援 Claude Agent；整合式瀏覽器跨工作階段持續不重新整理；變更的版面控制（並排或模態視窗）
- **OpenAI 模型 WebSocket 支援**：使用 WebSocket 模式取代每輪次新 HTTP 請求，OpenAI 模型速度提升 12%
- **沙箱預設讀取權限**：`$HOME` 目錄下不再自動啟用讀取權限，僅授權工作區資料夾與沙箱暫存資料夾
- **鍵盤快捷鍵聚焦終端機**：問題輪播中按 `⌥T`（Windows/Linux `Alt+T`）可快速返回終端機
- **Webview 大型本地資源最佳化載入**：改為分塊串流並採用 Transferable Streams，降低記憶體使用
- **TypeScript 7.0 Beta 支援**：安裝 TypeScript Native Preview 擴充功能即可試用
- **Git AI 共同作者預設啟用**：Copilot 修改檔案時自動加入 co-author，可透過 `git.addAICoAuthor` 調整
- **Copilot CLI 同步工作階段標題**：採用 SDK session-title API，所有介面保持一致
- **Chat Customizations Evaluation 擴充功能**：分析並改善聊天自訂項目（prompt files、custom agents、instructions、skills）
- **Dev Container lockfile 預設啟用**：釘選 Feature 版本與校驗碼，防範供應鏈攻擊；支援 Dependabot 自動更新
- **VS Code 開發建置使用 TypeScript 7**：型別檢查從約 60 秒縮減至約 10 秒
- **CP857 編碼支援**（土耳其 DOS 編碼）
- **Edit Mode 將於 v1.125 完全移除**（自 v1.110 棄用）

---

*資料來源：[Visual Studio Code 1.118 發行說明](https://code.visualstudio.com/updates/v1_118)*
