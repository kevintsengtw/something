# Visual Studio Code 1.130

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們

---

_發行日期：2026 年 7 月 22 日_

下載：Windows：[x64](https://update.code.visualstudio.com/1.130.0/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.130.0/win32-arm64-user/stable) | Mac：[Universal](https://update.code.visualstudio.com/1.130.0/darwin-universal-dmg/stable) [Intel](https://update.code.visualstudio.com/1.130.0/darwin-x64-dmg/stable) [silicon](https://update.code.visualstudio.com/1.130.0/darwin-arm64-dmg/stable) | Linux：[deb](https://update.code.visualstudio.com/1.130.0/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.130.0/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.130.0/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.130.0/linux-snap-x64/stable)

---

歡迎來到 Visual Studio Code 1.130 版本。本次發行帶來 agent host 改善、Agents 視窗中更快的審閱工作流程、更好的聊天可見性，以及更智慧的終端機連結處理。

- [**Agent host**](#agent-host)：在專用程序中執行工作階段，您可以從多個 VS Code 視窗連線至它。
- [**Agents 視窗改善（Preview）**](#agents-視窗改善preview)：透過緊湊的差異、檔案層級統計和跨工具鏈的 worktree 支援，更快審閱多檔變更。
- [**輔助工具核准**](#輔助工具核准)：透過讓模型在 Agent 任務期間評估工具呼叫風險，減少核准中斷。
- [**Git 差異中可點擊的檔案連結（助記前綴）**](#git-差異中可點擊的檔案連結助記前綴)：啟用助記前綴時，直接從差異輸出開啟檔案。

Happy Coding!

---

VS Code 正逐步推出給所有使用者。在 VS Code 中使用 **Check for Updates** 立即取得最新版本。

若要儘早試用新功能，請[**下載 nightly Insiders 組建**](https://code.visualstudio.com/insiders)，其中包含最新的可用更新。

---

## Agents

### Agent Host

如同我們在上一個版本中提到的，我們正圍繞 agent host 重新架構 Agent 工作階段在 VS Code 中的運作方式——agent host 是一個專用程序，根據 [Agent Host Protocol](https://microsoft.github.io/agent-host-protocol/)（AHP）執行 Copilot、Claude 和 Codex 等 Agent 工具鏈。因為工作階段存在於自己的程序中，同一工作階段可以同時從多個 VS Code 視窗連線和渲染。Agent host 的 Copilot Agent 由 [Copilot SDK](https://www.npmjs.com/package/@github/copilot-sdk) 驅動，這意味著其行為和功能與 Copilot CLI、獨立版 GitHub Copilot 應用程式以及其他 Copilot 產品一致。

深入了解 [VS Code Agent Host 架構](https://code.visualstudio.com/docs/agents/concepts/agent-host)。

我們正積極開發 agent host 並漸進式地向編輯器視窗和 [Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)的使用者推出。若要加入，請啟用 `chat.agentHost.enabled`（此設定由組織層級管理。請聯繫您的管理員以變更。），然後從工具鏈下拉選單中選取 agent host 工具鏈。以下截圖展示如何在編輯器視窗中選取 agent host 上的 `Copilot` 工具鏈：

![截圖顯示編輯器視窗中的工具鏈下拉選單。](https://code.visualstudio.com/assets/updates/1_130/agent-host-harness-dropdown-editor.webp)

隨著我們持續投資 agent host，一些功能可能僅在 Agent 於其上執行時才可用。這些功能會連結回本區段，並在相關時註明任何額外的啟用設定（例如，`chat.agents.claude.preferAgentHost` 以在 agent host 上啟用 Claude Agent）。

如果您在使用 agent host 時有任何回饋或請求，請透過[提交問題](https://github.com/microsoft/vscode/issues)讓我們知道。

#### 輔助工具核准

**設定**：`chat.assistedPermissions.enabled`

重複的工具核准提示可能會中斷長時間執行的 Agent 任務。透過輔助權限，語言模型會評估每個工具呼叫的風險，並決定該工具是否可以執行或應該需要您的核准。

啟用此設定，為在 agent host 上執行的 Agent 在權限選擇器中新增 **Assisted permissions**。以下影片比較預設核准與輔助權限：

### Agents 視窗改善（Preview）

[Agents 視窗](https://code.visualstudio.com/docs/agents/agents-window)包含幾項更新，讓審閱變更和管理聊天更加容易。需要在 [agent host](#agent-host) 上執行工作階段的更新會在下方特別標示。

#### 檔案層級差異統計

**Changes** 編輯器中的每個檔案標頭在檔案路徑旁顯示即時的插入和刪除計數。您可以在掃視多檔差異時快速評估每個檔案變更的規模。

#### 緊湊的多檔差異檢視

多檔差異使用更緊湊的邊界，移除程式碼前的空白。檔案標頭、行號和未變更區域控制項共用一致的對齊方式，讓狹窄編輯器中有更多審閱變更的空間。

![截圖顯示多檔差異檢視中的檔案層級差異統計和緊湊、對齊的邊界。](https://code.visualstudio.com/assets/updates/1_130/agents-compact-diff.webp)

#### 緊湊的快速聊天

在 [agent host](#agent-host) 上執行的快速聊天，在工作階段清單中使用緊湊的單行列。一般工作階段保留第二行以顯示變更統計、狀態和時間戳記，讓快速聊天更容易區分，並為專案工作階段留出更多空間。

![截圖顯示工作階段清單中緊湊的快速聊天列。](https://code.visualstudio.com/assets/updates/1_130/agents-compact-quick-chat.webp)

#### 所有 Agent 工具鏈的 Worktree 支援

在 [agent host](#agent-host) 上執行的 Agent 工具鏈支援 worktree 隔離。Agents 視窗中的 **New Worktree** 核取方塊先前僅由 Copilot 工具鏈支援。Claude 和 Codex 工作階段現在也可在 Git worktree 中執行，讓您無論使用何種工具鏈，都更容易在同一工作區中為不同功能啟動並行工作階段。

![截圖顯示 Claude Agent 工作階段的 New Worktree 選項。](https://code.visualstudio.com/assets/updates/1_130/agents-worktree.webp)

---

## Chat

### 聊天時間戳記

**設定**：`chat.verbose`

為聊天請求和回應顯示時間戳記。懸停訊息工具列以查看聊天互動的時間戳記和經過時間。您可以透過 `chat.verbose` 停用此功能。

### Copilot Business 和 Enterprise 的彙總 AI 點數使用量

Copilot Business 和 Copilot Enterprise 使用者現在可以直接在 Copilot 狀態選單中查看目前計費週期的彙總 AI 點數使用量。先前，點數使用量僅在設定了使用者層級預算時才會顯示，讓許多組織管理的使用者無法看到他們已消耗多少點數。

現在，當未設定使用者層級預算時，狀態選單會顯示計費週期至今使用的點數總數。這讓您一目了然地檢視消耗量，以便在不離開編輯器的情況下更好地了解您的使用模式。

![截圖顯示 Copilot 狀態選單為 Copilot Enterprise 使用者顯示彙總點數使用量。](https://code.visualstudio.com/assets/updates/1_130/aggregate-credit-usage.webp)

---

## 終端機

### Git 差異中可點擊的檔案連結（助記前綴）

當啟用 Git 的 [`diff.mnemonicPrefix`](https://git-scm.com/docs/diff-config#Documentation/diff-config.txt-diffmnemonicPrefix) 選項時，您可以直接從終端機中的 Git 差異輸出開啟檔案連結。VS Code 辨識如 `i/`（索引）和 `w/`（工作樹）等前綴，並從連結目標中移除前綴，以便開啟正確的檔案。

當啟用助記前綴時，VS Code 也會辨識 `git diff --no-index` 產生的數字前綴 `1/` 和 `2/`。

---

## 工程

VS Code 儲存庫使用 TypeScript 7 的正式版本編譯。我們也切換至 TypeScript 7 擴充功能的正式版本。請閱讀 TypeScript 團隊的 [TypeScript 7.0 發行公告](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/)。

---

## 感謝

對 `vscode` 的貢獻：

- [@accnops (Arthur Cnops)](https://github.com/accnops)
  - Voice：退出後端自動旁白（auto_narrate: false）[PR #325799](https://github.com/microsoft/vscode/pull/325799)
  - voice：僅在工作階段上下文發送後才發送 request_narration [PR #325928](https://github.com/microsoft/vscode/pull/325928)
  - voice：針對遺失的旁白進行 NACK + 客戶端重新驗證 [PR #325966](https://github.com/microsoft/vscode/pull/325966)
- [@ahmadawais (Ahmad Awais)](https://github.com/ahmadawais)：將 Command Code 偵測為終端機分頁標題的 Agent CLI [PR #324417](https://github.com/microsoft/vscode/pull/324417)
- [@AntonioLujanoLuna (Antonio Lujano Luna)](https://github.com/AntonioLujanoLuna)：修正 BYOK Anthropic 端點將 PDF 作為圖片區塊發送的問題 [PR #324960](https://github.com/microsoft/vscode/pull/324960)
- [@arham766 (Shahrier Islam Arham)](https://github.com/arham766)：chore：將 windows-process-tree 升級至 0.8.0 以修正 Process Explorer 中的 UTF-8 命令列 [PR #324283](https://github.com/microsoft/vscode/pull/324283)
- [@clintharrison (Clint Harrison)](https://github.com/clintharrison)：在終端機連結的 git diff 解析中支援助記前綴 [PR #298490](https://github.com/microsoft/vscode/pull/298490)
- [@justjavac (迷渡)](https://github.com/justjavac)：Decorations：退回至較低優先順序的色彩 [PR #325422](https://github.com/microsoft/vscode/pull/325422)
- [@kobihikri (Kobi Hikri)](https://github.com/kobihikri)：移除已刪除的 no-package-lock / no-yarn-lock 工作流程的無效 CODEOWNERS 規則 [PR #325932](https://github.com/microsoft/vscode/pull/325932)
- [@mirimadahmed (Mir)](https://github.com/mirimadahmed)
  - 處理語音插話播放 [PR #325808](https://github.com/microsoft/vscode/pull/325808)
  - 修正語音插話協定 [PR #326159](https://github.com/microsoft/vscode/pull/326159)
  - 語音 Agent 從客戶端發送語音語言地區設定 [PR #325931](https://github.com/microsoft/vscode/pull/325931)
  - 語音 Agent 始終處於串流模式以支援插話 [PR #326165](https://github.com/microsoft/vscode/pull/326165)
  - 新增範圍限定的即時語音逐字稿 [PR #326134](https://github.com/microsoft/vscode/pull/326134)
- [@pony-maggie (Lucas Ma)](https://github.com/pony-maggie)
  - 避免過時的簡易對話框資料夾更新 [PR #321357](https://github.com/microsoft/vscode/pull/321357)
  - 允許簡易檔案對話框建立巢狀資料夾 [PR #321355](https://github.com/microsoft/vscode/pull/321355)
- [@rfeltis (Ralph Feltis)](https://github.com/rfeltis)：修正配額軌跡計費週期計算 [PR #325895](https://github.com/microsoft/vscode/pull/325895)
- [@smorimoto (Sora Morimoto)](https://github.com/smorimoto)：在設定標籤中辨識 OCaml [PR #325457](https://github.com/microsoft/vscode/pull/325457)
- [@spokodev](https://github.com/spokodev)：修正：在 fuzzyContains 中比對大寫查詢字元 [PR #324047](https://github.com/microsoft/vscode/pull/324047)
- [@UditDewan (udit)](https://github.com/UditDewan)：修正 tunnelProtocol 上下文金鑰在聚焦時始終解析為 https 的問題 [PR #325445](https://github.com/microsoft/vscode/pull/325445)

### 問題追蹤

問題追蹤的貢獻：

- [@gjsjohnmurray (John Murray)](https://github.com/gjsjohnmurray)
- [@RedCMD (RedCMD)](https://github.com/RedCMD)
- [@IllusionMH (Andrii Dieiev)](https://github.com/IllusionMH)
- [@albertosantini (Alberto Santini)](https://github.com/albertosantini)

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

> 如果您想閱讀先前 VS Code 版本的發行說明，請前往 [code.visualstudio.com](https://code.visualstudio.com/) 上的 [Updates](https://code.visualstudio.com/updates)。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| Agent | Agent |
| Agent Host Protocol (AHP) | Agent Host Protocol（AHP） |
| agent host | agent host |
| Agents window | Agents 視窗 |
| assisted permissions | 輔助權限 |
| assisted tool approvals | 輔助工具核准 |
| billing cycle | 計費週期 |
| BYOK (Bring Your Own Key) | BYOK（自帶金鑰） |
| Copilot SDK | Copilot SDK |
| credits | 點數 |
| diff | 差異 |
| file header | 檔案標頭 |
| gutter | 邊界 |
| harness | 工具鏈 |
| inline view | 行內檢視 |
| mnemonic prefix | 助記前綴 |
| multi-file diff | 多檔差異 |
| permissions picker | 權限選擇器 |
| quick chat | 快速聊天 |
| session | 工作階段 |
| side-by-side view | 並排檢視 |
| status menu | 狀態選單 |
| terminal | 終端機 |
| timestamp | 時間戳記 |
| token | Token |
| TypeScript | TypeScript |
| working tree | 工作樹 |
| workspace | 工作區 |
| worktree | worktree |
| worktree isolation | worktree 隔離 |

*資料來源：[Visual Studio Code 1.130 發行說明](https://code.visualstudio.com/updates/v1_130)*
