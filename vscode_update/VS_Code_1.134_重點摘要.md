# Visual Studio Code 1.134 版本重點摘要

**版本：** 1.134
**發行日期：** 2026 年 8 月 19 日
**原文：** https://code.visualstudio.com/updates/v1_134

---

本次發行協助您跨視窗工作、將相關聊天並排整理，並更快速地瀏覽長對話。以下為官方列出的四大亮點：

## 一、並排聊天（工作階段中聊天的格線佈局）

- 透過將聊天排列為水平或垂直群組，讓相關對話保持可見
- 將聊天或子代理聊天拖曳至群組中，以比較結果或並排監控工作
- 當您返回工作階段或重新載入視窗時，VS Code 會還原聊天群組佈局和焦點
- 建立側邊聊天可在目前聊天旁開啟新對話
- 將子代理聊天拖放至群組中，即可與目前聊天並排檢視
- 也可在 **Chats** 選擇器中以 Alt+選取聊天，將其開啟至側邊

## 二、提示時間軸

- **設定**：`sessions.chatTimeline.display`
- 長 Agent 工作階段可能難以找到較早的提示，並辨識哪些提示變更了檔案
- Agents 視窗在逐字稿邊界中包含一條時間軸，每個圓點代表您的一個提示，醒目標示的圓點標記您目前的位置
- 懸停時間軸可檢視您的提示，然後選取其中一個跳至該處
- 對於變更了檔案的提示，清單會顯示新增和移除的行數，並讓您直接開啟變更以供審閱
- 使用設定將時間軸顯示在捲軸旁（`ruler`）或隱藏它（`off`）

## 三、在聊天中尋找

- 使用 ⌘F（Windows、Linux 為 Ctrl+F）在 Chat 檢視、聊天編輯器和 Agents 視窗中搜尋對話
- 搜尋涵蓋整個對話，即使是目前未渲染在畫面上的內容
- 在符合項目之間移動時，VS Code 會將每個符合項目捲動至檢視中，並在摺疊的工作摘要包含符合項目時將其展開
- 支援大小寫比對、全字比對，或使用規則運算式

## 四、預設在整合式瀏覽器中開啟 HTML 檔案

- **設定**：`workbench.editorAssociations`
- 若您經常預覽本機 HTML 檔案而非編輯它們，可將整合式瀏覽器設為其預設編輯器
- 可透過 `workbench.editorAssociations` 設定或從編輯器標頭來設定此行為
- 整合式瀏覽器提供與獨立瀏覽器分頁相同的功能，同時保持與 HTML 檔案的關聯；為保留此關聯，連結和其他導覽會在新分頁中開啟

---

## 其他

- **Agent host**：讓您可從多個 VS Code 視窗連線至同一 Agent 工作階段，依 [AHP](https://microsoft.github.io/agent-host-protocol/) 在專用程序中執行 Agent 工具鏈；Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，行為與 Copilot CLI、獨立版 GitHub Copilot 應用程式一致
- **側邊窗格佈局改善**（`sessions.layout.singlePaneDetailPanel`、`workbench.editor.showTabs`）：佈局遵循 `workbench.editor.showTabs` 設定，多個分頁保持可見，而 `single` 和 `none` 值使用緊湊的單一標題標頭；文字檔案編輯器使用與 Changes 編輯器相同的標頭結構，標頭中含檔案階層連結；側邊窗格在切換工作階段時保持其大小和可見性，避免非預期的佈局變動
- **從分頁關閉其他編輯器**：按住 Alt 可將每個編輯器分頁上的關閉操作變更為 **Close Other Editors**，然後在您想保留的分頁上選取該操作

---

*資料來源：[Visual Studio Code 1.134 發行說明](https://code.visualstudio.com/updates/v1_134)*
