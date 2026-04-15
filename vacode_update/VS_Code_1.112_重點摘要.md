# Visual Studio Code 1.112 版本重點摘要

**版本：** 1.112（2026 年 3 月 18 日發行）
**類型：** 每週穩定版（Weekly Stable Release）

---

## 一、整合式瀏覽器除錯（Integrated Browser Debugging）

- 全新 `editor-browser` 除錯類型，支援在 VS Code 內建瀏覽器中直接啟動除錯工作階段
- 支援 **Launch** 與 **Attach** 兩種組態模式
- 可在不離開 VS Code 的情況下設定中斷點、逐步執行程式碼、檢視變數
- 相容既有 `msedge` 和 `chrome` 除錯組態，遷移僅需修改 `launch.json` 中的 `type` 欄位
- 內建瀏覽器新增右鍵選單（複製/貼上、在新分頁開啟、檢查元素）
- 內建瀏覽器擁有獨立縮放層級，可透過 `workbench.browser.pageZoom` 設定控制
- 可從瀏覽器工具列切換開發人員工具（DevTools）

## 二、Copilot CLI 權限管理（Copilot CLI Permissions）

- Copilot CLI 工作階段現可配置權限等級，減少中斷頻率
- 三種權限等級：
  - **Default Permissions（預設權限）**：使用已設定的核准設定，需要核准的工具會顯示確認對話方塊
  - **Bypass Approvals（略過核准）**：自動核准所有工具呼叫，自動重試錯誤
  - **Autopilot（自動駕駛）**：自動核准所有工具呼叫、自動回應問題、持續自主工作直到任務完成

## 三、MCP 伺服器沙箱化（MCP Server Sandboxing）

- 可在 macOS 和 Linux 上以沙箱環境執行本地 stdio MCP 伺服器
- 沙箱伺服器具備受限的檔案系統和網路存取權限
- 在 `mcp.json` 中設定 `"sandboxEnabled": true` 即可啟用
- 透過 `sandbox` 物件自訂檔案系統規則（讀/寫權限）和網路規則（允許的網域）
- 當沙箱伺服器需要額外資料夾或網域存取權時，VS Code 會提示使用者授權
- **目前不支援 Windows**（WSL 和 SSH 遠端情境仍可運作）

## 四、Agent 圖片與二進位檔案支援（Agent Image Support）

- Agent 可直接從磁碟讀取圖片檔案和二進位檔案
- 二進位檔案以 hexdump 格式呈現給 Agent
- Agent 或工具產生的圖片輸出（如內建瀏覽器截圖）現可在聊天回應中選取
- 新增圖片輪播檢視器（Image Carousel），可在專用檢視中瀏覽圖片
- 啟用 `imageCarousel.explorerContextMenu.enabled` 後，可在檔案總管右鍵選單中開啟圖片輪播

## 五、Monorepo 自訂項目（Monorepo Customizations）

- 改善跨套件的自訂檔案探索機制，無需開啟完整儲存庫即可共享設定
- 新設定 `chat.useCustomizationsInParentRepositories` 可從父資料夾向上探索至儲存庫根目錄
- 適用於所有聊天自訂類型：
  - 始終啟用的指令：`copilot-instructions.md`、`AGENTS.md`、`CLAUDE.md`
  - 指令檔案、提示檔案、自訂 Agent、技能（Skills）、掛鉤（Hooks）

## 六、外掛程式與 MCP 伺服器管理

- 外掛程式與 MCP 伺服器現可在不解除安裝的情況下啟用/停用
- 支援全域和每個工作區的啟用/停用控制
- 外掛程式自動更新功能，依據 `extensions.autoCheckUpdates` 設定運作
- 來自 npm 和 pypi 的外掛程式更新需要使用者核准（安全考量）

## 七、聊天體驗改善

- 複製符號（類別名稱、函式、方法名稱）並貼入聊天時，自動轉換為符號參考
- 可使用 `Ctrl+Shift+V`（macOS: `Cmd+Shift+V`）以純文字貼上
- 新增 `/troubleshoot` 技能，可直接在對話中分析 Agent 除錯日誌
- 支援匯出和匯入 Agent 工作階段的除錯日誌

## 八、無障礙功能改善（Accessibility）

- 在任何尋找或篩選對話方塊中按 `Alt+F1` 可開啟情境式無障礙說明
- 涵蓋編輯器尋找與取代、終端機尋找、跨檔案搜尋、輸出/問題/除錯主控台篩選
- 聊天問題輪播現完全支援螢幕閱讀器，可透過 `Alt+N` 和 `Alt+P` 導覽
- 各種尋找和篩選小工具新增 ARIA 提示

## 九、終端機改善（Terminal）

- 支援 Kitty 圖形協定（Kitty Graphics Protocol）的高品質終端機圖片渲染
- Kitty 鍵盤協定正式可用（GA），解決終端模擬器中數十年的按鍵模糊問題

## 十、工作區（Workbench）

- 新增 `${activeEditorLanguageId}` 變數可用於 `window.title` 設定
- 顯示目前活動編輯器的語言識別碼，適用於 Talon 等無障礙工具

---

*資料來源：VS Code 1.112 發行說明 (https://code.visualstudio.com/updates/v1_112)*
