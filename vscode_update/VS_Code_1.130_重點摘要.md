# Visual Studio Code 1.130 版本重點摘要

**版本：** 1.130
**發行日期：** 2026 年 7 月 22 日
**原文：** https://code.visualstudio.com/updates/v1_130

---

本次發行帶來 agent host 改善、Agents 視窗中更快的審閱工作流程、更好的聊天可見性，以及更智慧的終端機連結處理。以下為官方列出的四大亮點：

## 一、Agent Host

- 在專用程序中執行工作階段，可從多個 VS Code 視窗連線
- 根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）執行 Copilot、Claude 和 Codex 等 Agent 工具鏈
- 因為工作階段存在於獨立程序中，同一工作階段可同時從多個 VS Code 視窗連線和渲染
- Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，行為與 Copilot CLI、獨立版 GitHub Copilot 應用程式一致
- 啟用方式：設定 `chat.agentHost.enabled`，然後從工具鏈下拉選單選取 agent host 工具鏈

## 二、Agents 視窗改善（Preview）

- **檔案層級差異統計**：**Changes** 編輯器的每個檔案標頭在檔案路徑旁顯示即時的插入和刪除計數
- **緊湊的多檔差異檢視**：使用更緊湊的邊界，移除程式碼前的空白，讓狹窄編輯器有更多審閱空間
- **緊湊的快速聊天**：快速聊天在工作階段清單中使用緊湊的單行列，更容易區分並為專案工作階段留出更多空間
- **所有 Agent 工具鏈的 worktree 支援**：Claude 和 Codex 工作階段現在也可在 Git worktree 中執行（先前僅 Copilot 工具鏈支援）

## 三、輔助工具核准

- **設定**：`chat.assistedPermissions.enabled`
- 由語言模型評估每個工具呼叫的風險，決定工具可直接執行或需要您的核准，減少核准中斷
- 啟用後會為 agent host 上執行的 Agent 在權限選擇器中新增 **Assisted permissions**

## 四、Git 差異中可點擊的檔案連結（助記前綴）

- 當啟用 Git 的 [`diff.mnemonicPrefix`](https://git-scm.com/docs/diff-config#Documentation/diff-config.txt-diffmnemonicPrefix) 選項時，可直接從終端機的 Git 差異輸出開啟檔案連結
- VS Code 辨識 `i/`（索引）和 `w/`（工作樹）等前綴，並移除前綴以開啟正確檔案
- 也辨識 `git diff --no-index` 產生的數字前綴 `1/` 和 `2/`

---

## 其他

- **聊天時間戳記**：為聊天請求和回應顯示時間戳記，懸停訊息工具列可查看時間戳記和經過時間，可透過 `chat.verbose` 停用
- **Copilot Business 和 Enterprise 的彙總 AI 點數使用量**：當未設定使用者層級預算時，狀態選單顯示目前計費週期已使用的點數總數
- **工程：TypeScript 7**：VS Code 儲存庫使用 TypeScript 7 正式版編譯，並切換至 TypeScript 7 擴充功能的正式版

---

*資料來源：[Visual Studio Code 1.130 發行說明](https://code.visualstudio.com/updates/v1_130)*
