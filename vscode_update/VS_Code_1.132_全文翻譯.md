# Visual Studio Code 1.132 (Insiders)

在 [LinkedIn](https://www.linkedin.com/showcase/vs-code)、[X](https://go.microsoft.com/fwlink/?LinkID=533687)、[Bluesky](https://bsky.app/profile/vscode.dev) 上追蹤我們 | 在 [X](https://x.com/VSCodeChangelog) 或 [Bluesky](https://bsky.app/profile/vscodechangelog.bsky.social) 上追蹤 Insiders 變更日誌

---

_最後更新：2026 年 8 月 3 日_

歡迎來到 Visual Studio Code 1.132 Insiders 版本。

本發行說明涵蓋 VS Code 的 Insiders 組建，並會隨著新功能的加入而持續演進。

您仍然可以透過[提交日誌](https://github.com/Microsoft/vscode/commits/main)和我們的[已關閉問題](https://github.com/Microsoft/vscode/issues?q=is%3Aissue%20is%3Aclosed%20milestone%3A1.132.0)清單追蹤我們的進度。

Happy Coding!

---

## 2026 年 7 月 31 日

- 為語音模式和聽寫加入重新設計、感知佈景主題的環境光暈，以區分聆聽和說話狀態。_[#328483](https://github.com/microsoft/vscode/issues/328483)_
- 新增將聽寫的數字格式化為數字符號的支援。_[#328344](https://github.com/microsoft/vscode/issues/328344)_
- 在語音模式引導卡片中加入本地化的語音預覽。_[#328084](https://github.com/microsoft/vscode/issues/328084)_
- 新增使用 Nemotron 3.5 語音轉文字模型進行多語言裝置端聽寫的支援。_[#328366](https://github.com/microsoft/vscode/issues/328366)_
- 新增在整合式瀏覽器中為元素加上註解的支援。_[#291482](https://github.com/microsoft/vscode/issues/291482)_
- 新增在您調整面板或側邊欄大小時，動態重新換行聊天中行內終端機命令輸出以符合其容器寬度的支援。_[#328307](https://github.com/microsoft/vscode/issues/328307)_

## 2026 年 7 月 30 日

- 新增 **Chat: Install Dictation Model from Local Package...** 命令，可手動安裝裝置端聽寫模型。_[#328154](https://github.com/microsoft/vscode/issues/328154)_
- 新增在評估 Agent Host 終端機自動核准規則時，以專用文法解析 PowerShell 語法的支援。_[#328052](https://github.com/microsoft/vscode/issues/328052)_

## 2026 年 7 月 29 日

- 新增在您首次使用語音模式功能時顯示設定選項的橫幅。_[#327203](https://github.com/microsoft/vscode/issues/327203)_
- 新增在使用 `${cwd}` 變數的終端機分頁標題中顯示 `~` 而非完整家目錄路徑的支援，減少家目錄下巢狀路徑的視覺雜訊。_[#274200](https://github.com/microsoft/vscode/issues/274200)_
- 新增執行 `code serve-web` 時對 `--enable-proposed-api` 旗標的支援，讓在 `package.json` 中宣告 `enabledApiProposals` 的擴充功能可在瀏覽器中正確啟動，與現有的桌面行為一致。_[#228781](https://github.com/microsoft/vscode/issues/228781)_
- 新增支援 **Remove Manual Folding Ranges** 命令僅移除游標位置最內層的手動摺疊範圍，而非移除每個與游標相交的手動範圍。_[#212599](https://github.com/microsoft/vscode/issues/212599)_

## 2026 年 7 月 28 日

- 新增以 `dictation.md` 和 `voice.md` 檔案自訂聽寫和語音模式的支援，這些檔案存放在您的使用者設定檔（`~/.copilot/dictation.md`、`~/.copilot/voice.md`）或受信任工作區的 `.github` 資料夾中。_[#327334](https://github.com/microsoft/vscode/issues/327334)_
- 新增在 Markdown 預覽中複製程式碼區塊的支援，每個圍籬程式碼區塊上都有懸停觸發的複製按鈕。_[#322269](https://github.com/microsoft/vscode/issues/322269)_

## 2026 年 7 月 27 日

- 新增將同一瀏覽器元素的多個附件合併為單一項目的支援，包含其文字內容和截圖。_[#327733](https://github.com/microsoft/vscode/issues/327733)_
- 在編輯器分頁的右鍵選單中新增 **Save** 和 **Save As**，讓您可以直接從分頁儲存已修改的檔案，無需開啟 Explorer 或檔案選單。_[#327504](https://github.com/microsoft/vscode/issues/327504)_

## 2026 年 7 月 24 日

- 修正在語音轉文字模型仍在下載或載入時取消聽寫沒有效果的問題，該問題會讓您卡在等待下載完成的狀態。_[#327292](https://github.com/microsoft/vscode/issues/327292)_

---

我們非常感謝大家在新功能準備好後儘早試用，所以請經常回來查看並了解最新內容。

---

## 術語對照表

| 英文 | 繁體中文 |
|------|----------|
| agent host | agent host |
| ambient glow | 環境光暈 |
| annotate | 註解 |
| auto-approve rules | 自動核准規則 |
| banner | 橫幅 |
| dictation | 聽寫 |
| dirty file | 已修改的檔案 |
| extension | 擴充功能 |
| fenced code block | 圍籬程式碼區塊 |
| folding range | 摺疊範圍 |
| grammar | 文法 |
| home directory | 家目錄 |
| Insiders | Insiders |
| Integrated Browser | 整合式瀏覽器 |
| Markdown preview | Markdown 預覽 |
| Nemotron | Nemotron |
| numerals | 數字符號 |
| on-device | 裝置端 |
| onboarding card | 引導卡片 |
| PowerShell | PowerShell |
| rewrap | 重新換行 |
| speech-to-text | 語音轉文字 |
| terminal | 終端機 |
| theme-aware | 感知佈景主題 |
| trusted workspace | 受信任工作區 |
| user profile | 使用者設定檔 |
| Voice Mode | 語音模式 |
| workspace | 工作區 |

*資料來源：[Visual Studio Code 1.132 發行說明](https://code.visualstudio.com/updates/v1_132)*
