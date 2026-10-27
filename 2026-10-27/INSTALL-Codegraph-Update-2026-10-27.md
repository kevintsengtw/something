# 公司建圖流程完整更新包：安裝方式

日期：2026-10-27。這一包包含先前尚未傳到公司的 Skill，以及從 ClosingRate 完成畫面之後整理的共用文件。只需使用本包，不必先安裝前一份 ZIP。

## 安裝

1. 人工把 `Company-Codegraph-Update-2026-10-27.zip` 複製到公司，先解壓到一個新的暫存資料夾，保留隱藏的 `.agents` 目錄。尚未複製或安裝前，不算公司已更新。
2. 在公司先將以下三個既有項目備份到 `D:\Projects\FX\_kw\work\backups` 下新的獨立備份資料夾；不存在的項目跳過。Skill 備份放在 `.agents\skills` 以外，避免讀到兩個同名入口：
   - `D:\Projects\FX\.agents\skills\kw-build-graphs` 整個資料夾。
   - `D:\Projects\FX\_kw\work\code-graph-common-procedure.md`。
   - `D:\Projects\FX\_kw\work\code-graph-build-prompt.md`。
3. 將解壓內容的 `.agents` 與 `_kw` 合併複製到 `D:\Projects\FX`，只更新本包所列檔案，保留其他 Skills 與其他 `_kw` 內容。若有舊的同名 Skill 資料夾，備份後只換入本包的 `kw-build-graphs`，不要整個替換 `.agents\skills`。更新清單及校驗檔位於 `_kw\work\codegraph-update-20261027`。

完成後應有：

```text
D:\Projects\FX\.agents\skills\kw-build-graphs\SKILL.md
D:\Projects\FX\.agents\skills\kw-build-graphs\workflows\codebase-graphs.md
D:\Projects\FX\_kw\work\code-graph-common-procedure.md
D:\Projects\FX\_kw\work\code-graph-build-prompt.md
D:\Projects\FX\_kw\work\codegraph-update-20261027\CHANGES.md
```

先讀回確認上述檔案及 `SHA256SUMS.txt` 的內容，再維持在 `D:\Projects\FX`，用原本方式開啟新的 Codex CLI session，使用：

```text
$kw-build-graphs
```

## 保留範圍

本包保留公司已驗證的 JSON 讀法、同份命令 ParseInput、變數／路徑展開修正與提交前後的停止條件；不包含需要先跑的安裝程式、PowerShell runner、INVOKE.txt 或本機新對話提示詞。

沿用既有 `D:\Projects\FX\_kw\tool-node25-ps7`。不把本包放進該工具目錄，不覆蓋 Node、CGR、CLI 程式、lock、schema、release manifest、project/local、任何業務 repo、既有 graph 或 reports。公司原共用文件的比較表、歷史紀錄與完整原文保留在步驟 2 的備份，不由本機照片重建。

## 驗證界線

本機已驗證封裝、來源一致性、實際包內固定範例與更新合併情境；這些不代表公司已安裝或已實測。先前 60000 ms timeout 根因、CLI 重複 probe 與診斷資訊缺口沒有在本包修正，不宣稱所有新 repo 都免排錯。真正執行仍讀取 exit code、JSON、warnings 與 skips，不修改 manifest 去消除標記。
