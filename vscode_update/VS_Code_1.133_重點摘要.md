# Visual Studio Code 1.133 版本重點摘要

**版本：** 1.133
**發行日期：** 2026 年 8 月 12 日
**原文：** https://code.visualstudio.com/updates/v1_133

---

本次發行讓您在 Claude 工作階段中有更多彈性、讓長聊天更容易追蹤，並在您工作時即時重新整理本機 HTML 預覽。以下為官方列出的三大亮點：

## 一、變更 Claude 工作階段的模型供應商

- 先前 Claude 工作階段完全透過您的 GitHub Copilot 訂閱或 Claude 的現有設定（例如 API 金鑰）執行，切換供應商需要重新設定 agent host
- 現在模型選擇器同時顯示兩個群組，讓您可以在回合之間切換供應商
- 您選取的模型會用於下一個回合
- **Anthropic** 下的模型會向您的 API 金鑰計費，**Copilot** 下的模型則使用您的 Copilot 訂閱

## 二、無需 GitHub 登入即可開啟 Agents 視窗（實驗性）

- **設定**：`chat.agentHost.allowSignedOutWhenUsable`
- 先前 [Agents 視窗](https://code.visualstudio.com/docs/agents/run/agents-window)開啟時會出現無法關閉的 GitHub 登入提示，阻擋了機器無法連線至 github.com 的使用者，以及不使用 GitHub 的使用者；已用 API 金鑰設定好 Claude、不需 GitHub 登入的使用者也得多做這一步
- 啟用此設定後，可在未登入 GitHub 的情況下開啟 Agents 視窗，GitHub 驗證會改為關聯至個別的 Agent 或模型，而非 Agents 視窗
- 本次發行中此行為僅支援 Claude；對使用自有模型金鑰的 Copilot 以及 Codex 的支援計劃於未來版本推出

## 三、在整合式瀏覽器中自動重新載入 HTML 檔案

- **設定**：`workbench.browser.autoReloadOnFileChange`
- 當本機 HTML 檔案在[整合式瀏覽器](https://code.visualstudio.com/docs/debugtest/integrated-browser)中開啟時，該檔案在磁碟上變更時會自動重新整理
- 這有助於您立即看到 Agent 的編輯或您自己儲存的變更
- 可為個別瀏覽器分頁切換自動重新載入，並透過 `workbench.browser.autoReloadOnFileChange` 設定預設值

---

## 其他

- **Agent host**：讓您可從多個 VS Code 視窗連線至同一 Agent 工作階段，依 [AHP](https://microsoft.github.io/agent-host-protocol/) 在專用程序中執行 Agent 工具鏈；Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，行為與 Copilot CLI、獨立版 GitHub Copilot 應用程式一致
- **提示的黏性捲動**（`chat.stickyScroll.enabled`）：捲動長對話時，已捲過的提示會像編輯器的黏性捲動一樣釘選在聊天頂部；釘選的提示會顯示其在對話中的位置，選取它可跳回該提示，或使用旁邊的上一個／下一個按鈕逐一瀏覽您的提示
- **已棄用的功能和設定**：無

---

*資料來源：[Visual Studio Code 1.133 發行說明](https://code.visualstudio.com/updates/v1_133)*
