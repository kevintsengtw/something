# Visual Studio Code 1.126 版本重點摘要

**版本：** 1.126
**發行日期：** 2026 年 6 月 24 日
**原文：** https://code.visualstudio.com/updates/v1_126

---

本次發行帶來更清楚的成本透明度、更簡易的模型調整，以及更安全地瀏覽不熟悉的程式碼。以下為官方列出的三大亮點：

## 一、工作階段層級成本資訊

- 可查看整個聊天工作階段的成本，而非僅單次對話的成本
- 更好地了解哪些工作階段消耗最多點數，便於發現昂貴的對話並隨時間管理使用量

## 二、Agent Host Copilot 工作階段中的多個聊天

- Agents 視窗中從 agent host 啟動的 Copilot 工作階段可同時包含多個聊天
- 聊天共用相同的工作階段和工作上下文，可在同一工作區中同時進行多個對話
- 選取工作階段工具列中的 **New Chat**（`+`）開啟第二個聊天，可用於審閱變更、撰寫測試或文件
- 聊天在視窗重新載入後保留並還原
- 可直接在分頁中雙擊重新命名聊天，聊天標題獨立於工作階段標題
- **Agent 程式碼回饋**：在生成的程式碼上留下的留言儲存在 agent host 上，Agent 可透過 `listComments`、`resolveComments`、`addComment` 等伺服器端工具互動；支援 `/code-review` 技能和 PR 審閱留言

## 三、以限制模式開啟新資料夾

- **設定**：`security.workspace.trust.startupPrompt` 預設值從 `once` 改為 `never`
- 新資料夾現在以限制模式開啟，僅顯示信任橫幅，讓您先安全瀏覽程式碼再決定是否信任
- 工作區信任編輯器中移除了 **Trust Parent** 按鈕，避免誤操作信任過多資料夾

---

## 其他

- **統一的模型自訂選擇器**：將上下文大小和推理（思考）力度控制合併至單一模型自訂選擇器
- **簡化的模型懸停**：顯示簡潔的一字能力描述和直接跳轉至相關設定的深層連結按鈕
- **VS Code 部落格**：新增[部落格首頁](https://code.visualstudio.com/blogs)，醒目顯示近期文章；新增[部落格封存](https://code.visualstudio.com/blogs/archive)頁面
- **VS Code 文件**：重新組織文件目錄結構，Agent 相關文件歸類至「Agents」區段，編輯和設定歸類至「Editor」，語言和擴充功能分別歸類至「Languages and Runtimes」和「Extension Docs」

---

*資料來源：[Visual Studio Code 1.126 發行說明](https://code.visualstudio.com/updates/v1_126)*
