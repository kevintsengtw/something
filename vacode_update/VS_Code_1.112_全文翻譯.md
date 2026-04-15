# Visual Studio Code 1.112 版本更新 — 全文翻譯（繁體中文）

**版本：** 1.112
**發行日期：** 2026 年 3 月 18 日
**原文：** https://code.visualstudio.com/updates/v1_112

---

歡迎使用 Visual Studio Code 1.112 版本。本次發行包含了 Agent 與開發人員體驗方面的多項改善。

以下是本次發行的主要亮點：

- **整合式瀏覽器除錯**：在不離開 VS Code 的情況下端對端除錯 Web 應用程式
- **Copilot CLI 權限**：賦予 Copilot CLI 工作階段更多自主權，減少中斷頻率
- **MCP 伺服器沙箱化**：在沙箱中執行本地 MCP 伺服器，限制其對機器的存取權限
- **Agent 圖片支援**：在 Agent 對話中直接使用截圖、圖表和二進位檔案
- **Monorepo 自訂項目**：在 monorepo 中的所有套件間共享 Agent 指令和技能

---

## GitHub Copilot

### 整合式瀏覽器除錯（Integrated Browser Debugging）

整合式瀏覽器可讓您直接在 VS Code 內開啟 Web 應用程式，現在您還可以使用整合式瀏覽器啟動除錯工作階段。這讓您能夠與 Web 應用程式互動、設定中斷點、逐步執行程式碼，以及檢視變數，而無需離開 VS Code。

全新的 `editor-browser` 除錯類型啟用了整合式瀏覽器分頁的除錯功能，同時支援 **Launch**（啟動）和 **Attach**（附加）兩種組態。現有 `msedge` 和 `chrome` 除錯組態的大多數選項都受到支援，因此遷移通常只需修改現有 `launch.json` 組態中的 `type` 即可。

**啟動組態範例：**

```json
{
  "type": "editor-browser",
  "request": "launch",
  "name": "在整合式瀏覽器中除錯",
  "url": "http://localhost:3000"
}
```

**附加組態範例：**

```json
{
  "type": "editor-browser",
  "request": "attach",
  "name": "附加至整合式瀏覽器"
}
```

#### 瀏覽器互動改善

在瀏覽器頁面中按右鍵現在會顯示常用選項，例如複製/貼上、在新分頁中開啟以及檢查元素。此外，整合式瀏覽器現在擁有自己的縮放層級，獨立於 VS Code 視窗的縮放，並提供鍵盤快速鍵來控制縮放。

您可以從瀏覽器工具列切換瀏覽器的開發人員工具（Developer Tools），以檢查元素、檢視主控台輸出，以及除錯頁面問題。您也可以在編輯器分頁上按右鍵，選擇「移至新視窗」將瀏覽器移至獨立的浮動視窗。

相關設定：`workbench.browser.pageZoom` — 控制整合式瀏覽器中的頁面縮放。

### Copilot CLI 權限（Copilot CLI Permissions）

您可以在聊天中為本地 Agent 工作階段設定權限，以賦予 Agent 更多的行動自主權，並減少核准請求的次數。本次發行將此功能擴展到 Copilot CLI 工作階段。

對於 Copilot CLI 工作階段，您可以在以下權限等級之間選擇：

| 權限等級 | 說明 |
|---------|------|
| **Default Permissions（預設權限）** | 使用您已設定的核准設定。需要核准的工具會在執行前顯示確認對話方塊。 |
| **Bypass Approvals（略過核准）** | 自動核准所有工具呼叫，無需顯示確認對話方塊，並在錯誤時自動重試。 |
| **Autopilot（自動駕駛）** | 自動核准所有工具呼叫、自動回應問題，並持續自主工作直到任務完成。（預設在 Insiders 版中啟用） |

### MCP 伺服器沙箱化（MCP Server Sandboxing）

MCP 伺服器沙箱化讓您可以在沙箱中執行本地 MCP 伺服器，限制其對您機器的存取權限。您現在可以在 macOS 和 Linux 上，以沙箱環境執行本地設定的 stdio MCP 伺服器。沙箱伺服器具有受限的檔案系統和網路存取權限。

#### 啟用沙箱

若要啟用沙箱，請在 `mcp.json` 檔案中為伺服器設定 `"sandboxEnabled": true`。

```json
{
  "servers": {
    "my-server": {
      "type": "stdio",
      "command": "node",
      "args": ["server.js"],
      "sandboxEnabled": true
    }
  }
}
```

#### 自訂沙箱限制

您可以進一步透過新增 `sandbox` 物件來自訂沙箱限制，指定特定的檔案系統和網路規則。

```json
{
  "servers": {
    "my-server": {
      "type": "stdio",
      "command": "node",
      "args": ["server.js"],
      "sandboxEnabled": true,
      "sandbox": {
        "filesystem": [
          { "path": "${workspaceFolder}", "permission": "write" },
          { "path": "~/.ssh", "permission": "deny" }
        ],
        "network": [
          { "domain": "api.example.com", "permission": "allow" }
        ]
      }
    }
  }
}
```

`sandbox` 物件支援：

- **檔案系統規則（filesystem）**：控制檔案系統存取，支援讀取/寫入/拒絕權限
- **網路規則（network）**：指定伺服器可存取的網域

#### 權限提示

當沙箱伺服器需要存取額外的資料夾或網域時，VS Code 會提示您授予該權限，並為該 `mcp.json` 檔案更新沙箱組態。

> **注意：** 本地執行的 MCP 伺服器沙箱化目前不支援 Windows。遠端情境（例如 WSL 和 SSH）仍可正常運作。

### Agent 圖片與二進位檔案支援（Agent Image Support）

Agent 現在可以直接從磁碟讀取圖片檔案和二進位檔案，讓您可以使用 Agent 執行更多樣化的任務，例如分析截圖、讀取二進位檔案中的資料等。

#### 二進位檔案格式

二進位檔案以 hexdump 格式呈現給 Agent，方便分析和理解二進位資料結構。

#### 圖片輪播檢視器（Image Carousel）

當 Agent 或工具產生圖片輸出（例如來自整合式瀏覽器的截圖）時，這些圖片現在可在聊天回應中選取，並可在專用的圖片輪播檢視中開啟。

當啟用 `imageCarousel.explorerContextMenu.enabled` 時，您可以在檔案總管檢視中對圖片檔案或資料夾按右鍵，選擇「在輪播中開啟圖片」（Open Images in Carousel）以在輪播檢視中瀏覽圖片。

### 符號參考貼上（Symbol Reference Paste）

當您複製一個符號（例如類別名稱、函式或方法名稱）並將其貼入聊天時，VS Code 現在會自動將其轉換為符號參考，為 Agent 提供該符號的自動上下文，使其能更快速且有效地完成任務。

如果您希望在不轉換為符號參考的情況下貼上符號，可以使用「以純文字貼上」命令，透過 `Ctrl+Shift+V`（macOS 上為 `Cmd+Shift+V`）。

### /troubleshoot 技能（Troubleshoot Skill）

新增的 `/troubleshoot` 技能可直接在對話中分析 Agent 除錯日誌，並提供有關 Agent 行為的深入洞察。您可以在聊天輸入中輸入 `/troubleshoot`，然後跟上問題描述或您所遇到的問題。

此技能會讀取從聊天工作階段匯出的 JSONL 除錯日誌檔案，可幫助您了解：

- 工具或子 Agent 為何被使用或跳過
- 指令或技能為何未載入
- 回應時間緩慢的原因
- 是否發生網路連線問題

若要使用聊天中的 `/troubleshoot` 技能，請啟用 `github.copilot.chat.agentDebugLog.enabled` 設定，然後重新載入 VS Code。

#### 除錯日誌匯出與匯入

您現在可以匯出和匯入 Agent 工作階段的除錯日誌，讓您可以與他人分享或離線分析，這對於疑難排解和分享有關 Agent 行為的洞察特別有用。

### Monorepo 自訂項目（Monorepo Customizations）

本次發行改善了探索機制，讓您更容易在 monorepo 中的所有套件間共享儲存庫層級的指引和工具，無需將完整儲存庫作為工作區開啟。

透過新設定 `chat.useCustomizationsInParentRepositories`，VS Code 還可以從父資料夾向上探索自訂檔案，直到儲存庫根目錄。

當探索功能啟用時，它適用於所有聊天自訂類型，包括：

- **始終啟用的指令**：例如 `copilot-instructions.md`、`AGENTS.md` 和 `CLAUDE.md`
- **指令檔案**（Instruction Files）
- **提示檔案**（Prompt Files）
- **自訂 Agent**（Custom Agents）
- **技能**（Skills）
- **掛鉤**（Hooks）

這項改善使得在 monorepo 中處理個別套件時，仍可受益於在儲存庫根目錄層級定義的集中式指令、自訂 Agent 和技能。

---

## 外掛程式與 MCP 伺服器管理（Plugin and MCP Server Management）

### 啟用/停用外掛程式與 MCP 伺服器

先前，外掛程式和 MCP 伺服器只能透過安裝或解除安裝來停用或啟用。本次發行引入了在不解除安裝的情況下啟用或停用外掛程式和 MCP 伺服器的功能。

外掛程式和 MCP 伺服器現在都可以在全域和每個工作區層級啟用或停用。您可以透過開啟 MCP 或外掛程式頁面，或在擴充功能檢視（Extensions view）或聊天：開啟自訂項目（Chat: Open Customizations）檢視中對其項目按右鍵來執行此操作。

### 外掛程式自動更新（Plugin Auto-Updates）

外掛程式現在可以根據 `extensions.autoCheckUpdates` 設定自動更新。然而，來自 npm 和 pypi 的外掛程式需要使用者核准才能更新，因為更新這些外掛程式可能會導致新程式碼在您的機器上執行。

---

## 無障礙功能（Accessibility）

### 尋找/篩選對話方塊的無障礙說明

您可以在任何尋找或篩選對話方塊中按 `Alt+F1` 來開啟情境式無障礙說明。此功能為螢幕閱讀器使用者提供鍵盤快速鍵、導覽指示和情境特定的指引。

支援的對話方塊包括：

- 編輯器尋找與取代（Editor Find and Replace）
- 終端機尋找（Terminal Find）
- 跨檔案搜尋（Search Across Files）
- 輸出、問題和除錯主控台篩選（Output, Problems, and Debug Console Filters）

### 聊天問題輪播

聊天問題輪播現在完全支援螢幕閱讀器使用者。問題會依位置宣告，並可使用 `Alt+N` 和 `Alt+P` 在問題之間導覽。

### ARIA 提示改善

各種尋找和篩選小工具新增了 ARIA 提示，宣告「按 Alt+F1 以取得無障礙說明」。這適用於包括終端機尋找小工具、Web 檢視尋找小工具和樹狀檢視篩選器在內的多個元件。

---

## 終端機（Terminal）

### Kitty 圖形協定支援

VS Code 現在支援使用 Kitty 圖形協定（Kitty Graphics Protocol）在終端機內進行高品質圖片渲染。這使得 VS Code 的終端機功能與獨立終端機模擬器的功能更加對齊。

Kitty 圖形協定是一種靈活且高效能的協定，允許在終端機中執行的程式將任意像素（光柵）圖形渲染到終端機模擬器的螢幕上。圖形與文字整合，可以在文字下方或上方繪製帶有 Alpha 混合的圖形，且圖形會隨文字自動捲動。

### Kitty 鍵盤協定正式可用

Kitty 鍵盤協定（Kitty Keyboard Protocol）已正式可用（GA），對所有使用者開放。這是一種現代終端機輸入標準，透過明確編碼所有按鍵事件來解決終端機模擬器中數十年的按鍵模糊問題，讓終端機應用程式能支援之前與控制碼衝突的快速鍵。

---

## 工作區（Workbench）

### activeEditorLanguageId 視窗標題變數

新的 `${activeEditorLanguageId}` 變數現在可用於 `window.title` 設定。此變數顯示目前活動編輯器的語言識別碼，適用於需要判斷目前程式語言以啟用適當語音命令的無障礙工具（如 Talon）。

**使用範例：**

```json
{
  "window.title": "${activeEditorLanguageId} - ${activeEditorShort}"
}
```

---

## 術語對照表

| 英文 | 繁體中文 |
|------|---------|
| Integrated Browser | 整合式瀏覽器 |
| Debugging | 除錯 |
| Breakpoints | 中斷點 |
| Launch Configuration | 啟動組態 |
| Attach Configuration | 附加組態 |
| Copilot CLI Permissions | Copilot CLI 權限 |
| Bypass Approvals | 略過核准 |
| Autopilot | 自動駕駛 |
| MCP Server Sandboxing | MCP 伺服器沙箱化 |
| Sandbox | 沙箱 |
| Filesystem Rules | 檔案系統規則 |
| Network Rules | 網路規則 |
| Agent Image Support | Agent 圖片支援 |
| Binary Files | 二進位檔案 |
| Hexdump | 十六進位傾印 |
| Image Carousel | 圖片輪播 |
| Symbol Reference Paste | 符號參考貼上 |
| Troubleshoot Skill | 疑難排解技能 |
| Debug Logs | 除錯日誌 |
| Monorepo Customizations | Monorepo 自訂項目 |
| Custom Instructions | 自訂指令 |
| Prompt Files | 提示檔案 |
| Custom Agents | 自訂 Agent |
| Skills | 技能 |
| Hooks | 掛鉤 |
| Plugin Auto-Updates | 外掛程式自動更新 |
| Enable/Disable | 啟用/停用 |
| Accessibility | 無障礙功能 |
| Screen Reader | 螢幕閱讀器 |
| ARIA Hints | ARIA 提示 |
| Kitty Graphics Protocol | Kitty 圖形協定 |
| Kitty Keyboard Protocol | Kitty 鍵盤協定 |
| Terminal | 終端機 |
| Workbench | 工作區 |
| Developer Tools | 開發人員工具 |

---

*資料來源：VS Code 1.112 發行說明 (https://code.visualstudio.com/updates/v1_112)*
