# Visual Studio Code 1.120 版本重點摘要

**版本：** 1.120
**發行日期：** 2026 年 5 月 13 日
**原文：** https://code.visualstudio.com/updates/v1_120

---

本次發行將 **Agents 視窗帶入 Stable**，改善 BYOK 模型的可見性與控制，並新增 Markdown 生活品質改善及 Agent 安全功能。以下為官方列出的五大亮點：

## 一、Agents 視窗進入 Stable（Preview）

- Agents 視窗從 Insiders 進入 VS Code Stable，作為 Preview 功能提供
- 這是一個專為 Agent 驅動開發打造的全新視窗類型，可跨多個專案探索、迭代和審閱任務，並在任務之間無縫切換
- 支援選擇 Agent 工具鏈、在遠端機器上執行 Agent、自訂環境（色彩主題、鍵盤繫結、擴充功能）
- 本週改善：偏好設定跨新工作階段保留、從 Changes 面板直接捨棄變更、同步上游變更按鈕、已完成工作階段預設顯示所有變更、最近工作階段導覽箭頭、可針對 Agents 視窗覆寫設定
- 擴充功能支援：僅提供靜態內容的擴充功能自動啟動，其他可透過 `extensions.supportAgentsWindow` 設定加入

## 二、BYOK 模型改善

- **檢視 BYOK 模型 Token 使用量**：聊天檢視的上下文視窗控制項現在為 BYOK 模型顯示準確的 Token 使用量和已用百分比（先前一律顯示 0%）
- **設定推理模型的思考力度**：可直接從聊天檢視的模型選擇器設定 BYOK 推理模型的思考力度（thinking effort），在延遲/成本與回答品質之間取捨；適用於 OpenAI 相容端點（OpenAI、xAI (Grok)、OpenRouter、自訂 OpenAI / Azure OpenAI 部署），Anthropic 模型先前已支援
- **依供應商分組的模型選擇器**：模型選擇器依供應商分組，最近使用的模型旁顯示灰色供應商名稱，可透過 `/models` 快速存取

## 三、Markdown 改善

- **Markdown 差異預覽（Preview）**：從原始碼控制檢視開啟 Markdown 檔案時，可使用 VS Code 的渲染 Markdown 預覽查看差異，支援並排和行內兩種檢視模式，可透過 `workbench.diffEditorAssociations` 設定為預設
- **Markdown 預覽預設值變更**：停用 `markdown.preview.doubleClickToSwitchToEditor` 和 `markdown.preview.markEditorSelection` 兩項預設功能
- **HTML id 支援**：Markdown 路徑補全和連結驗證現在識別 Markdown 檔案中 HTML 元素的 `id` 屬性
- **Markdown 表格的智慧選取**：使用「展開選取」從儲存格擴展至列再到整個表格

## 四、終端機命令風險評估（實驗性）

- 設定：`chat.tools.riskAssessment.enabled`
- 終端機命令確認現在包含 AI 生成的風險徽章和說明
- 三個等級：**Safe**（綠色，唯讀操作）、**Caution**（橙色，修改工作區/安裝套件/傳送網路資料）、**Review carefully**（紅色，難以復原的操作如強制推送或刪除工作區外檔案）

## 五、終端機工具輸出壓縮（Preview）

- 設定：`chat.tools.compressOutput.enabled`
- 將 `git diff`、`ls -l`、`npm install` 等命令的長輸出進行後處理後再傳送給模型，減少上下文視窗佔用
- 壓縮方式：摺疊 diff 中未變更的大區塊、丟棄 lockfile 和 snapshot 差異、精簡 `ls -l` 為項目名稱、去除 `npm install` 進度條/棄用警告/稽核摘要
- 壓縮輸出前附加簡短橫幅，讓模型知道觸發了哪些篩選器及如何停用壓縮

---

## 其他

- **Copilot CLI 外掛自動探索**：以 GitHub Copilot CLI 安裝的 Agent 外掛會被 VS Code 自動偵測，單次 `copilot plugin install` 即可同時涵蓋 CLI 和 VS Code
- **計畫模式控制改善**：Claude Agent 和 Copilot CLI 的計畫模式新增行內編輯器、更清楚的回饋模式指示（`chat.planWidget.inlineEditor.enabled`）
- **Proposed API**：Custom editor diffs API、Separate custom editor priorities（diff/merge）、Document diff API
- **GitHub Pull Requests 擴充功能**：支援複製/貼上和上傳按鈕上傳圖片至 PR 留言、更具描述性的 worktree 資料夾名稱、`${issueType}` 範本變數
- **重要修正**：整合式瀏覽器 localhost 目標包含 All-Interfaces 連結

---

*資料來源：[Visual Studio Code 1.120 發行說明](https://code.visualstudio.com/updates/v1_120)*
