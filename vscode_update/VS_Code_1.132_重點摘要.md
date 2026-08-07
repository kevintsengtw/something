# Visual Studio Code 1.132 版本重點摘要

**版本：** 1.132
**發行日期：** 2026 年 8 月 5 日
**原文：** https://code.visualstudio.com/updates/v1_132

---

本次發行帶來整合式瀏覽器中的元素層級回饋、多語言聽寫、側邊聊天，以及混合式 Markdown 編輯器中的 Markdown 差異。以下為官方列出的四大亮點：

## 一、整合式瀏覽器中的留言

- 整合式瀏覽器新增支援選取網頁元素並以 Agent 回饋為其加上註解
- 可透過 `workbench.action.browser.addElementCommentToChat` 鍵盤快速鍵觸發此模式
- 在此模式下，可選取多個元素並為每個元素加上留言，然後一併發送至聊天

## 二、多語言聽寫

- 聽寫現在使用多語言的 Nemotron 3.5 作為預設的裝置端模型，音訊保留在您的裝置上
- 遵循 `agents.voice.language` 設定；使用自動語言選取時，在支援的情況下採用您的系統或瀏覽器地區設定，否則讓模型自動偵測語言
- **引導體驗**：首次使用時協助您在開始前確認選取的麥克風，顯示即時麥克風波形、多支麥克風時提供裝置選擇器，並連結至聽寫設定與自訂項目；可透過命令面板的 **Voice Mode: Show Introduction** 重新開啟
- **自訂**：執行 **Voice: Configure Dictation Instructions**，VS Code 會合併 `~/.copilot/dictation.md` 與受信任工作區中 `.github/dictation.md` 的指令，補充內建的清理規則（保留語意、適當時偏好使用數字符號）
- **離線安裝**：若網路限制導致裝置端模型無法下載，錯誤通知會提供從磁碟匯入官方 Foundry Local 模型套件的操作；正常首次下載時，麥克風按鈕會顯示進度而非在輸入框放置下載文字

## 三、使用 `/btw` 的側邊聊天

- 在聊天輸入中輸入 `/btw` 可開啟側邊聊天，在不中斷目前回合的情況下提問
- 側邊聊天共用主要聊天的上下文和提示快取，因此可針對目前回合提問
- 同樣地，可選取聊天回應中的文字，針對該回應提出情境問題
- 也可透過將聊天分頁拖曳至輸入框，或輸入 `#chat:` 後挑選聊天標題，在聊天之間引用彼此以共用上下文

## 四、混合式 Markdown 編輯器中的 Markdown 差異（實驗性）

- 延續上一版[引入的混合式 Markdown 編輯器](https://code.visualstudio.com/updates/v1_131#_hybrid-markdown-editor-experimental)（結合已渲染的 Markdown、就地編輯與 Agent 可據以行動的留言）
- 本版 Markdown 差異可在混合式 Markdown 編輯器中開啟，修改後的文件仍可編輯，同時邊界指示器會醒目顯示新增、變更和刪除的內容
- 可使用編輯器類型下拉選單在文字差異與帶有差異註解的 Markdown 編輯器之間切換

---

## 其他

- **Agent host**：讓您可從多個 VS Code 視窗連線至同一 Agent 工作階段，依 [AHP](https://microsoft.github.io/agent-host-protocol/) 在專用程序中執行 Copilot、Claude、Codex 等工具鏈；Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動
- **從聊天輸入追蹤工作階段活動**：聊天輸入上方的即時狀態膠囊——**Changes**（變更數量，可檢視即時更新的多檔差異）、**Previews**（Agent 建立或編輯檔案的 Markdown 預覽）、**Subagents**（在獨立聊天中開啟子代理工作）、**Browsers**（跟隨 Agent 與整合式瀏覽器的互動）
- **切換編輯器類型**：Agents 視窗中的編輯器類型下拉選單顯示目前編輯器，可在一般與差異編輯器之間切換並變更預設編輯器，位於階層連結列右側；編輯器視窗中可透過 `breadcrumbs.showEditorType` 啟用
- **終端機輸出重新流動**：Chat 中展開的終端機輸出會隨檢視調整大小而重新流動至可用寬度，適用於 Local Agent 工具鏈和 agent host 上的 Copilot 工具鏈
- **終端機命令的聽寫改善**：終端機聽寫套用感知 Shell 的清理，例如說出「git commit dash m hello world」會產生 `git commit -m "Hello World"`；選取使用中的麥克風按鈕可停止錄音
- **提議的 API：依編輯器模式設定自訂編輯器優先順序**：`customEditors.priority` 讓擴充功能為文字和差異編輯器選擇不同優先順序；現有單一優先順序值仍可運作，差異編輯器預設使用 `explicit`，計劃於下一版本推向穩定
- **已棄用**：`ChatAgentHostEnabled` 政策已移除，管理員無法再透過政策集中停用 agent host；開發者仍可使用 `chat.agentHost.enabled` 選擇 Agent 是否在獨立的 agent host 程序中執行。無即將到來的棄用項目

---

*資料來源：[Visual Studio Code 1.132 發行說明](https://code.visualstudio.com/updates/v1_132)*
