# VS Code 1.134 更新重點

**版本：** 1.134｜**發行日期：** 2026 年 8 月 19 日
**原文：** https://code.visualstudio.com/updates/v1_134

---

本版四大主軸為並排聊天、提示時間軸、聊天內搜尋與 HTML 預覽：

- **聊天格線佈局（並排聊天）**：可將聊天與子代理聊天拖曳成水平或垂直群組，並排比較結果或監控進度。建立側邊聊天可在目前聊天旁開新對話，也可在 **Chats** 選擇器中 Alt+選取開至側邊。返回工作階段或重新載入視窗時，佈局與焦點都會還原。
- **提示時間軸**（`sessions.chatTimeline.display`）：Agents 視窗的逐字稿邊界顯示時間軸，每個圓點代表一則提示，醒目圓點為目前位置。懸停可預覽、點選可跳轉；有變更檔案的提示會顯示增／刪行數並可直接開啟審閱。設定值 `ruler` 顯示於捲軸旁、`off` 隱藏。
- **在聊天中尋找**：⌘F／Ctrl+F 可在 Chat 檢視、聊天編輯器與 Agents 視窗中搜尋，範圍涵蓋整個對話（含尚未渲染的內容）。移動到符合項目時會自動捲入視野並展開含有結果的摺疊工作摘要，支援大小寫比對、全字比對與規則運算式。
- **HTML 檔案預設用整合式瀏覽器開啟**（`workbench.editorAssociations`）：常預覽而非編輯 HTML 時，可將整合式瀏覽器設為預設編輯器（也可從編輯器標頭設定）。功能與獨立瀏覽器分頁相同，並維持與該 HTML 檔的關聯，因此連結與導覽會開在新分頁。

其他更新：

- **Agent host**：可從多個視窗連線同一工作階段，依 AHP 在專用程序執行 Agent 工具鏈，Copilot Agent 由 Copilot SDK 驅動。
- **側邊窗格佈局改善**（`sessions.layout.singlePaneDetailPanel`、`workbench.editor.showTabs`）：佈局改為遵循 `showTabs` 設定，`single`／`none` 使用緊湊單一標題標頭；文字編輯器改用與 Changes 編輯器一致的標頭（含檔案階層連結）；切換工作階段時側邊窗格保持大小與可見性。
- **從分頁關閉其他編輯器**：按住 Alt 讓分頁上的關閉按鈕變成 **Close Other Editors**，在想保留的分頁上點一下即可。

**總結**：本版圍繞「長對話與多聊天的可管理性」——把聊天排成格線比較、用時間軸定位提示、用 Ctrl+F 直接搜尋，再加上 HTML 預覽與分頁操作的實用改善。

---

*資料來源：[Visual Studio Code 1.134 發行說明](https://code.visualstudio.com/updates/v1_134)*
