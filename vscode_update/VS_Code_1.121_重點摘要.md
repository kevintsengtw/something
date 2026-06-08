# Visual Studio Code 1.121 版本重點摘要

**版本：** 1.121
**發行日期：** 2026 年 5 月 20 日
**原文：** https://code.visualstudio.com/updates/v1_121

---

本次發行新增了內建的 Mermaid 和 HTML 預覽，簡化了 Agent 的終端機工具行為，並可在遠端機器上執行 Agent 工作階段。以下為官方列出的五大亮點：

## 一、遠端 Agent（Preview）

- Agents 視窗實驗性支援在您擁有的遠端機器上執行 Agent 工作階段，可透過 SSH 或 Dev Tunnels 連線
- 連線後啟動輕量級「agent host」程序，基於 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 建構
- 遠端 agent host 為長期執行程序，即使客戶端中斷連線，工作階段仍繼續執行
- 引入全新開放協定 **Agent Host Protocol（AHP）**，支援多個客戶端同時協調 Agent 工作階段，任何人皆可建構相容的客戶端或 agent host

## 二、模型可設定性

- **設定公用模型**：新增 `chat.utilityModel` 和 `chat.utilitySmallModel` 設定，可覆寫用於產生標題、摘要、提交訊息、重新命名建議、提示分類和意圖偵測等背景任務的模型，支援 BYOK 模型
- **Custom Endpoint 供應商（Insiders）**：全新 BYOK 供應商，可將任何 Chat Completions、Responses 或 Messages 相容端點接入 Copilot Chat，取代已棄用的 OpenAI Compatible (`customoai`) 供應商

## 三、Mermaid 圖表預覽

- 將 Matt Bierner 的 Markdown Preview Mermaid Support 擴充功能合併為 VS Code 內建擴充功能 `Mermaid Markdown Features`
- 在 Markdown 預覽、Notebook 的 Markdown 儲存格和聊天中渲染 Mermaid 圖表
- 支援平移、縮放和右鍵複製 Mermaid 原始碼

## 四、HTML 檔案預覽

- 無需安裝擴充功能即可在整合式瀏覽器中預覽本地 HTML 檔案
- 可透過檔案總管右鍵選單、編輯器分頁右鍵選單或編輯器標題列的 Preview 圖示開啟

## 五、終端機工具最佳化

- **Agent 感知的終端機命令**：VS Code 為 Agent 發起的終端機命令設定 `VSCODE_AGENT` 環境變數，CLI 可據此切換為機器可讀輸出、抑制進度動畫或跳過互動式提示
- **背景執行指示器**：工具呼叫回傳後命令仍在執行時，聊天 UI 顯示「Running `<command>` in background - Show」
- **背景 Agent 終端機清理**：命令完成後自動釋放背景終端機，減少資源佔用，聊天 UI 中仍保留命令輸出
- **更廣泛的輸出壓縮**（`chat.tools.compressOutput.enabled`）：擴展壓縮範圍至 `pytest`、`jest`、`cargo test`、`tsc`、Docker 命令和套件管理員等
- **敏感終端機提示攔截**：密碼、PIN 或驗證碼提示時，VS Code 會攔截並引導使用者在終端機中直接輸入，防止 Agent 捕獲或重放秘密

---

## 其他

- **Agents 視窗（Preview）持續改善**：持續根據回饋改進，繼續開發擴充功能支援
- **Agents 可觀測性與 OpenTelemetry 和 Grafana**：與 Azure Managed Grafana 團隊合作，提供預建 Grafana 儀表板，視覺化 Agent 操作、Token 使用量、聊天工作階段、工具呼叫和每模型回應時間
- **Claude Agent Auto 權限模式（Preview）**：`github.copilot.chat.claudeAgent.allowAutoPermissions` 設定啟用 Auto 模式，Agent 無需權限提示即可執行，透過分類器請求進行背景安全檢查
- **改善將元素加入聊天的體驗**：重新設計元素選取 UI，支援拖曳選取元素範圍，右鍵選單快速附加元素
- **YAML 前置資料在 Markdown 預覽**：`markdown.preview.frontMatter` 設定控制 YAML 前置資料的渲染方式（表格/程式碼區塊/隱藏）

---

*資料來源：[Visual Studio Code 1.121 發行說明](https://code.visualstudio.com/updates/v1_121)*
