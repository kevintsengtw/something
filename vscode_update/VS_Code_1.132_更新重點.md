# VS Code 1.132 更新重點

**版本：** 1.132｜**發行日期：** 2026 年 8 月 5 日
**原文：** https://code.visualstudio.com/updates/v1_132

---

本版四大主軸為瀏覽器元素留言、多語言聽寫、側邊聊天與 Markdown 差異：

- **整合式瀏覽器留言**：可選取網頁元素並加上 Agent 回饋，一次選多個元素、各自留言後再送出。快速鍵為 `workbench.action.browser.addElementCommentToChat`。
- **多語言聽寫**：改用多語言的 Nemotron 3.5 裝置端模型，音訊留在本機，遵循 `agents.voice.language`（自動模式會採系統／瀏覽器地區設定，否則由模型偵測）。首次使用有麥克風確認引導（可用 **Voice Mode: Show Introduction** 重開）；可用 **Voice: Configure Dictation Instructions** 搭配 `~/.copilot/dictation.md` 與工作區 `.github/dictation.md` 自訂術語與格式；網路受限時可從磁碟匯入 Foundry Local 模型套件。
- **側邊聊天 `/btw`**：在聊天輸入打 `/btw` 開啟側邊聊天，共用主聊天的上下文與提示快取，不中斷目前回合即可提問；也可選取回應文字提問，或用拖曳分頁／`#chat:` 引用其他聊天。
- **Markdown 差異（實驗性）**：混合式 Markdown 編輯器現在可開啟差異，修改後的文件仍可編輯，邊界指示器標示增／改／刪，並可用編輯器類型下拉選單在文字差異與 Markdown 差異之間切換。

其他更新：

- **Agent host**：可從多個視窗連線同一工作階段，依 AHP 在專用程序執行 Copilot／Claude／Codex，Copilot Agent 由 Copilot SDK 驅動。
- **Agents 視窗**：聊天輸入上方新增即時狀態膠囊 **Changes**／**Previews**／**Subagents**／**Browsers**，可直接追蹤並跳轉；新增編輯器類型下拉選單（位於階層連結列右側，編輯器視窗需開 `breadcrumbs.showEditorType`）。
- **終端機**：Chat 中的終端機輸出會隨檢視寬度重新流動；終端機聽寫套用感知 Shell 的清理，例如「git commit dash m hello world」會輸出 `git commit -m "Hello World"`。
- **提議的 API**：`customEditors.priority` 可為文字與差異編輯器分別設定優先順序，差異預設 `explicit`，下一版預計推向穩定。
- **棄用**：移除 `ChatAgentHostEnabled` 政策，管理員無法再集中停用 agent host，開發者仍可用 `chat.agentHost.enabled` 自行決定。

**總結**：本版把「給 Agent 回饋」這件事做得更精準——網頁能指著元素講、Markdown 能對著渲染結果審閱、問題能開側邊聊天不打斷主線；語音輸入也從單語推進到多語言且可自訂。

---

*資料來源：[Visual Studio Code 1.132 發行說明](https://code.visualstudio.com/updates/v1_132)*
