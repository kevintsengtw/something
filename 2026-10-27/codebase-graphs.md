# 原始碼專案 codebase 圖譜標準流程

本流程供 `kw-build-graphs` 使用，處理一個已授權 Git 原始碼專案的固定 commit，連續完成設定、CGR 建圖及產物驗證。規格以 `PHASE1-HANDOFF.md` 的唯讀與第一階段範圍為準；文件／UA／語意工作另見 [knowledge.md](knowledge.md)。使用者透過對話提供必要路徑，Agent 處理 CLI、設定、雜湊與報告。

不得呼叫 Codebase-Memory MCP，不將公司程式或完整紀錄送到外部。不要把過去成功的截圖、公司工作紀錄或本文件當成本次執行結果。

## 入口與完成範圍

1. 每次新的建圖要求先確認本次 repo；單獨呼叫 `$kw-build-graphs` 且未指定目標時，第一則回覆只先詢問：「這次要建立圖譜的 repo 完整本機路徑是什麼？」等待回答。此前只讀 Skill／共用規則，不列出 `_kw/work/repos`，不開啟專案設定、報告或圖譜，不執行 Git，也不從舊對話摘要、最近完成案、啟動目錄或工具路徑推定專案。使用者已為本次任務提供路徑，或明確接續目標唯一的未完成任務時才沿用；回答後不重問。工具根目錄及獨立工作區若無既有資料，才補問必要缺口。
2. 明確找出規則根目錄與既有工具根目錄；後者的 `package.json` 必須為 `@knowledge-workflow/cli`，執行入口保持 `node <toolRoot>/bin/kw.mjs`。不在業務 repo 安裝 Skills 或設定。
3. 完整建圖授權包含下述來源確認、設定準備、命令檢查、一次建圖與成功後一次驗證。正常成功不在階段間停止詢問；只準備設定、只驗證、使用者要求暫停時按其範圍執行。

既有成功 artifact／報告只能作歷史資料；一般建圖要求不能因找到舊 `graph.created=pass`／`graph.verified=pass` 就改成沿用或結束。只有使用者明確要求沿用／只驗證／查看既有結果，才能進入該模式。沒有執行本次 CLI，就不能把歷史 exit code、counts 或固定 commit 報成新一次建圖／驗證的結果。

## 來源與獨立設定

- 以唯讀 Git 確認所選 working tree 根目錄，將指定 revision 解析為完整 commit；使用者未指定 revision 時固定當時 HEAD，後續不用浮動 `master`／`main` 取代。設定 project ID、source ID、repository ID，避免不同 repo 共用相同設定或輸出。
- 記錄分支、staged、tracked unstaged、untracked 與實際路徑；ahead／behind 只作觀察，不 fetch、pull、push 或切換分支。不以乾淨 working tree、untracked=0 或歷史的 untracked=12 作通過門檻。圖譜取固定 commit，工作檔不混入已提交來源。
- Git 讀取使用既有 `gitEnvironment` 的無互動／唯讀選項，或等效的 `GIT_OPTIONAL_LOCKS=0`、`GIT_NO_LAZY_FETCH=1`、`GIT_TERMINAL_PROMPT=0`、`GIT_NO_REPLACE_OBJECTS=1`。不要繼承會重導 Git repository 的變數，不觸發 lazy fetch 或登入。
- 每個 repo 使用獨立 `project.json`、`local.json`、`workspace`、`cache`、圖譜輸出路徑與 `reports`；公司 Windows 全部位於已指定 D 槽的 `_kw` 工作區。檢查路徑與 reparse point，所有寫入均須在業務 repo、Git 管理目錄、工具程式與既有 frozen artifact 外。不同執行使用唯一 run tag，不覆寫舊報告或圖譜。
- **最終 `--out` 目錄必須不存在**，即使空目錄也不能預先建立。`$GraphDir` 只綁定為新路徑字串，由 CLI 的 atomic writer 建立；可建立的只有它的父目錄、workspace／cache／reports 及程序 temp。若使用 `<run>/graphs` 作為最終輸出，可建 `<run>` 與 `<run>/reports`，不可先建 `<run>/graphs`；若使用 `graphs/<run>`，可建 `graphs` 父目錄，不可建 `graphs/<run>`。報告與暫存不得放入最終輸出內。提交前檢查目的路徑不存在，不能只確認「目錄已備妥」。
- 沿用目前工具的 `schemas/project.schema.json`、`schemas/local.schema.json`。project 使用 `kw.project/1`、明確 `toolchain_lock`、來源、target platform、core/reader 的 Node 及 builder 的 Node/CGR、`code_graph.source_id/repository_id/revision`。local 沒有 format 欄位，保存 workflow_root、workspace_dir、cache_dir、sources 的機器 root 及原生 CGR 綁定。Node 是啟動 CLI 的 runtime，不用 local.tools.node 重導向。不要自行加語言、exclude 或業務規則。
- 已有設定先讀取並核對，不覆寫；確需修改的獨立設定先保留原件，設定不符不能靠更改業務來源來通過。設定回報中的「不符」必須來自實際 JSON 比較，不能來自讀取命令本身失敗。

## 沿用環境與執行邊界

沿用已核准、與實際工具 lock／release binding 相符的工具。不同 repo 不重跑 `npm test`、整套 doctor 或三條獨立 CGR 診斷。只有環境缺少／變更時，才依 [environment.md](environment.md) 處理受影響項目。release 不符就停止，不在公司部署資料夾偷偷重寫 manifest。

公司 Windows 已選定 PowerShell 7 時，使用其已知完整路徑，不因 PATH／預設殼改用 5.1；一般其他環境不因此被要求安裝 PowerShell 7。明確使用客戶端現有的合法單次命令核准機制，不先在已知不適用的預設沙箱試錯。核准、管理員終端或 autoaccept 都不表示已取消沙箱；無可用核准方式或被拒時停止，不用 Full Access、不改 execution policy、不換殼繞過限制。

啟動新的命令程序，在啟動 Node **以前**設定該程序專用 TEMP／TMP／TMPDIR；公司 Windows 使用已指定 D 槽 temp 根目錄下的新子目錄。以同一 Node 確認 `os.tmpdir()` 與該目錄相符；CGR 繼承 Node 的環境，不能只設定 CGR 而讓 Node 探測仍落在 C 槽。檢查或子程序失敗就停止，不改父程序、User／Machine 環境。OS、客戶端自身日誌不在此應用暫存路徑保證內。

先把本次唯一 run tag 綁定為具體的 `$RunTag`，再用已確認暫存根目錄與該 tag 組成 `$RunTemp`。`$RunTemp` 的最後一層必須等於本次 `$RunTag`，不能使用共用的 `tmp` 根目錄、其他 run 的目錄或字面值 `${st.tag}`／`${runTag}`／`$(...)`。檢查的是**變數求值後的資料值**，不是整段 PowerShell 文字；合法的 `${probeCode}:` 或會展開的 `${RunTag}` 語法仍可使用。

在建立目錄、修改子程序環境或啟動 Node 前，先檢查 run tag 與本次產生的暫存／輸出／報告路徑沒有未展開模板，並確認 temp 最後一層與本次 tag 相符；之後再檢查 Node 的 `os.tmpdir()`。不能只因 Node 與預期字串同樣都是 `${st.tag}` 就判定隔離通過。執行紀錄保存本次 tag、預期 temp、實際 TEMP／TMP／TMPDIR 與 Node 觀察值；不能事後改寫舊紀錄來冒充原本符合要求。

保留既有 CLI 的版本、binary SHA-256、原生 CGR、path／source guards 與 schema 檢查；不新增每個 repo 專屬的 runner、watchdog 或程序管理器。不要以每個 repo 的整套測試代替工具維護。

## 命令準備：固定讀法及同一份文字

每個 repo 只替換參數資料。先產生完整的 run tag 與實際路徑：來源／工具／設定須可讀，允許建立的父目錄與報告／暫存目錄須可寫，但最終 `--out` 目的路徑須不存在，不建立它來測寫入。實際檔名不得殘留 `${runTag}`、`${c.runTag}` 等尚未展開的模板。不要為檢查器再創建一條帶模板的輸入檔路徑。

使用以下 JSON 讀法。`$ProjectFile`、`$LocalFile` 必須先綁定為真實的絕對路徑，不把下列變數名稱原樣當路徑。

<!-- codebase-json-read: validated executable example -->
```powershell
$project = [System.IO.File]::ReadAllText($ProjectFile) | ConvertFrom-Json -ErrorAction Stop
$local = [System.IO.File]::ReadAllText($LocalFile) | ConvertFrom-Json -ErrorAction Stop
```

如果需要組裝 PowerShell inline 命令，直接解析待提交的同一份 `$BuildScriptText`。以下是命令準備階段，並不執行建圖；不用 parser 中間檔或另一個 parser 程序。

<!-- codebase-parse: validated executable example -->
```powershell
if ([string]::IsNullOrWhiteSpace($BuildScriptText)) { throw 'Missing build command text' }
$tokens = $null
$parseErrors = $null
$null = [System.Management.Automation.Language.Parser]::ParseInput(
    $BuildScriptText, [ref]$tokens, [ref]$parseErrors
)
if (@($parseErrors).Count -ne 0) {
    $parseErrors | ForEach-Object { [Console]::Error.WriteLine($_.Message) }
    throw 'Command text has PowerShell parser errors'
}
```

解析通過不代表 cmdlet 參數與檔案路徑一定正確：一起確認實際路徑、原生執行檔及使用到的參數。使用參數陣列，不把資料串成 shell 程式；`Join-Path` 不接 `-Raw`，`New-Item` 不接 `-LiteralPath`。建立目錄可用 `[System.IO.Directory]::CreateDirectory(...)`。字串中變數緊接冒號用 `${probeCode}:` 或格式化字串；合法 `${變數}` 不能一律當成殘留模板。不要指派 PowerShell 唯讀且大小寫不敏感的 `$PID`，用自己的變數名稱。

通過後將同一份已解析文字交給核准的 PowerShell 執行，不再產生另一份或用 Invoke-Expression。提交前可在本次授權內修正命令、路徑、檢查器並重新靜態檢查，不把尚未啟動 CLI 的錯誤當成重試建圖。無法修正或資訊不足才回報缺口；不得藉此改工具、timeout、業務來源或已核對的固定版本。

## 建圖與驗證的固定順序

實際命令維持：

```text
<node> <toolRoot>/bin/kw.mjs build-graphs --mode code --project <project.json> --local <local.json> --out <newGraphDir> --report <buildReport.json>
<node> <toolRoot>/bin/kw.mjs verify-graphs --artifact <newGraphDir> --manifest-sha256 <buildManifestSha256> --expect-project <projectId> --project <project.json> --local <local.json> --report <verifyReport.json>
```

以下 PowerShell 原生呼叫範本在**新的執行程序**中使用；資料變數必須先以具體值綁定，不讓 Agent 每個 repo 重寫這段邏輯。`$NodeExe` 是核准 Node 完整路徑，`$CliEntry` 是所選工具的 `bin/kw.mjs`。`$ProjectId`、`$FixedCommit` 必須來自已核對設定；`$RunTag` 是本次唯一且已求值的單層目錄名稱，`$RunTemp` 是以它命名的本次專用暫存路徑；其他變數為本次實際絕對路徑。設定與命令準備不使用這段程序的 `exit`。

<!-- codebase-build-verify: validated executable example -->
```powershell
$ErrorActionPreference = 'Stop'
if ([string]::IsNullOrWhiteSpace($RunTag) -or $RunTag -in @('.', '..') -or $RunTag -match '[\\/]') { throw 'Run tag must be a concrete single directory name' }
$resolvedPaths = @{ RunTag = [string]$RunTag; RunTemp = [string]$RunTemp; GraphDir = [string]$GraphDir; ReportRoot = [string]$ReportRoot; BuildReport = [string]$BuildReport; VerifyReport = [string]$VerifyReport }
foreach ($pathName in $resolvedPaths.Keys) {
    $pathValue = $resolvedPaths[$pathName]
    if ([string]::IsNullOrWhiteSpace($pathValue)) { throw ('Missing resolved path value: {0}' -f $pathName) }
    if ($pathValue.Contains('${') -or $pathValue.Contains('$(')) { throw ('Unexpanded template in resolved path value: {0}' -f $pathName) }
}
$tempLeaf = [System.IO.Path]::GetFileName([System.IO.Path]::GetFullPath($RunTemp).TrimEnd([char[]]@('\', '/')))
if ($tempLeaf -cne $RunTag) { throw 'Run temp directory must use the current run tag as its final component' }
$env:TEMP = $RunTemp
$env:TMP = $RunTemp
$env:TMPDIR = $RunTemp
$null = [System.IO.Directory]::CreateDirectory($RunTemp)
$null = [System.IO.Directory]::CreateDirectory($ReportRoot)
$graphParent = [System.IO.Path]::GetDirectoryName([System.IO.Path]::GetFullPath($GraphDir))
$null = [System.IO.Directory]::CreateDirectory($graphParent)
if (Test-Path -LiteralPath $GraphDir -ErrorAction Stop) { throw 'Graph output path already exists; preserve it and choose a new unused path before submitting CLI' }
$tmpOutput = & $NodeExe --eval 'process.stdout.write(require("node:os").tmpdir())'
$tmpExit = $LASTEXITCODE
if ($tmpExit -ne 0) { throw ('Node temp probe failed, exit {0}' -f $tmpExit) }
if (-not [string]::Equals([System.IO.Path]::GetFullPath($tmpOutput.Trim()), [System.IO.Path]::GetFullPath($RunTemp), [System.StringComparison]::OrdinalIgnoreCase)) { throw 'Node temp directory mismatch' }
$buildArgs = @($CliEntry, 'build-graphs', '--mode', 'code', '--project', $ProjectFile, '--local', $LocalFile, '--out', $GraphDir, '--report', $BuildReport)
& $NodeExe @buildArgs 1> $BuildStdout 2> $BuildStderr
$buildExit = $LASTEXITCODE
[System.IO.File]::WriteAllText($BuildExitFile, [string]$buildExit)
if ($buildExit -ne 0) { exit $buildExit }
$buildResult = [System.IO.File]::ReadAllText($BuildReport) | ConvertFrom-Json -ErrorAction Stop
if ($buildResult.status -ne 'ok' -or $buildResult.exit_code -ne 0 -or -not ($buildResult.checks | Where-Object { $_.id -eq 'graph.created' -and $_.status -eq 'pass' })) { throw 'Build report did not confirm graph.created' }
$records = @($buildResult.artifacts | Where-Object { $_.kind -eq 'graph_manifest' })
if ($records.Count -ne 1) { throw 'Expected one build manifest record' }
$manifestFile = [System.IO.Path]::Combine($GraphDir, 'manifest.json')
if (-not [string]::Equals([System.IO.Path]::GetFullPath($records[0].path), [System.IO.Path]::GetFullPath($manifestFile), [System.StringComparison]::OrdinalIgnoreCase)) { throw 'Build manifest path mismatch' }
$manifestSha = (Get-FileHash -LiteralPath $manifestFile -Algorithm SHA256 -ErrorAction Stop).Hash.ToLowerInvariant()
if ($manifestSha -cne $records[0].sha256) { throw 'Build manifest SHA-256 mismatch' }
$manifest = [System.IO.File]::ReadAllText($manifestFile) | ConvertFrom-Json -ErrorAction Stop
if ($manifest.project_id -cne $ProjectId -or $manifest.source.revision -cne $FixedCommit) { throw 'Build manifest project/revision mismatch' }
$verifyArgs = @($CliEntry, 'verify-graphs', '--artifact', $GraphDir, '--manifest-sha256', $manifestSha, '--expect-project', $ProjectId, '--project', $ProjectFile, '--local', $LocalFile, '--report', $VerifyReport)
& $NodeExe @verifyArgs 1> $VerifyStdout 2> $VerifyStderr
$verifyExit = $LASTEXITCODE
[System.IO.File]::WriteAllText($VerifyExitFile, [string]$verifyExit)
if ($verifyExit -ne 0) { exit $verifyExit }
$verifyResult = [System.IO.File]::ReadAllText($VerifyReport) | ConvertFrom-Json -ErrorAction Stop
if ($verifyResult.status -ne 'ok' -or $verifyResult.exit_code -ne 0 -or -not ($verifyResult.checks | Where-Object { $_.id -eq 'graph.verified' -and $_.status -eq 'pass' })) { throw 'Verify report did not confirm graph.verified' }
$verifiedChecks = @($verifyResult.checks | Where-Object { $_.id -eq 'graph.verified' -and $_.status -eq 'pass' })
if ($verifiedChecks.Count -ne 1) { throw 'Expected one verified graph result' }
$verified = $verifiedChecks[0].details
if ($verified.artifact_project_id -cne $ProjectId -or $verified.repository_id -cne $manifest.source.repository_id -or $verified.revision -cne $FixedCommit -or $verified.snapshot_id -cne $manifest.snapshot_id) { throw 'Verified graph identity mismatch' }
foreach ($countName in @('nodes', 'edges', 'source_files')) {
    if ($null -eq $verified.counts.$countName -or $verified.counts.$countName -ne $manifest.counts.$countName) { throw ('Verified count mismatch: {0}' -f $countName) }
}
exit 0
```

上述範本不是新的 CLI 或安裝要求，不另存為每個 repo 的 ps1 runner。可沿用已驗證的現有啟動方式；解析、核准與執行必須對應同一份命令。`exit` 用於獨立命令程序，不在使用者互動終端直接結束其工作階段。

建圖與驗證各只提交一次，建圖失敗不進驗證。立即保存真實 `$LASTEXITCODE`、stdout/stderr、命令、程序環境觀察與執行狀態；啟動失敗時 CLI exit code 為未知／未啟動，不能用外層 PowerShell exit code 冒充。外層是否 timeout 與 CLI 內部 probe timeout 分開記錄。

建圖報告的 `artifacts[kind=graph_manifest]` 提供完整 manifest 路徑與 SHA-256，直接使用，不能從截圖重打或截短。CLI `report.skipped`／report 缺失時，讀 stdout 的 JSON 與原始錯誤，保留失敗，不因沒有 report 就再跑 CLI。

讀回 verify 的 `graph.verified.details`，核對 project、repository、revision、snapshot ID、counts 與建圖 manifest，並讀取來源核對紀錄。`audit/source-readonly.json` 含來源前後觀察；來源改變或核對失敗不宣稱完成，不把他人修改還原。

## 只驗證既有圖譜

明確取得 artifact、可信的完整 manifest SHA-256、expected project ID 與適用 project/local，直接執行同一條 `verify-graphs`。缺少產物不能以空 graphs 目錄代替；回報 not_verifiable，不把沒有產物說成原始碼沒有實作，也不在只驗證任務中自動建圖。驗證不執行 CGR，但仍保留既有 Node、環境與 artifact 檢查。

## 失敗處理與已知限制

| 現象 | 下一步與邊界 |
| --- | --- |
| 命令準備／參數／路徑錯誤，CLI 未啟動 | 在已授權範圍修正讀回、實際檔名或同一份文字的檢查，重新靜態檢查後繼續；不標成已重試建圖。parser 無法開檔不等於建圖語法錯誤。 |
| `output_exists` | 最終 `--out` 已存在，空目錄也會被拒；保留該目錄與失敗報告，不刪除／清空／覆寫後自動重試。使用者明確授權再執行時，沿用本次已確認的 repo／固定 commit 與工具，改用新 run tag、新報告及不存在的輸出路徑，只建父目錄；一次建圖成功後再驗證。 |
| 已完成圖譜，但 temp 使用字面模板／非本次 run 目錄 | 將圖譜通過與環境偏差分開記錄。已有 build／verify 通過且沒有其他產物異常證據時，不單因這項偏差要求重建；保留 artifact、報告與原 temp，不回填虛假的合規紀錄。修正未來執行前的求值／tag／temp 檢查；未知歸屬或占用狀態的 temp 不自動刪除。 |
| 核准被拒、`spawn EPERM` | 依真實工具呼叫與原始錯誤確認執行邊界，不認定是 CGR 或磁碟不足。不能用換殼／wrapper 繞過被拒操作；停止受阻動作。 |
| CLI／CGR probe timeout | 保存已知 executable、argv、timeout、起訖與原始 details；缺資訊如實寫未知。不自動延長 timeout、不重試、不把獨立診斷加到每個 repo 的例行流程。 |
| 工具／schema／lock／release 不符 | 停止，保留目前有效工具與圖譜。工具升級與 schema 修改另行授權，不用重寫 manifest 掩蓋差異。 |
| 來源核對失敗、產物不符、未知程序終止狀態 | 保留該次診斷與上一有效圖譜，不進下一階段。只依已記錄 PID／路徑／時間判定本次程序，不依名稱批次終止或刪除未知工作目錄。 |

截至 2026-10-22，本機實作的 `installedTools()` 與 `buildCodeSnapshot()` 都會呼叫 `probeTool()`；公司紀錄亦觀察到重複探測。現有 `runProcess` 的診斷不完整提供 argv、PID 或非零 native exit code。這些是待修正的共用工具項目，本流程與 Skill **沒有修正它們**，也不能虛構 telemetry。

公司曾將 CGR probe 調為每條 60000 ms，獨立診斷曾觀察約 29.85／21.19／21.14 秒且全部通過；之後原 repo 建圖成功。這不能證明前次單條 probe 的逾時根因已解決，也不是把所有公司／本機 lock 改成 60000 的授權。總回合時間、三條耗時合計與單條 timeout 不同。已鎖定工具的 SHA-256、版本與 schema 仍照原規則檢查。

## 結果回報與交接

回報建圖與驗證的真實 exit code、`graph.created`／`graph.verified`、project／repository／完整 commit、完整 manifest SHA-256、nodes／edges／source files／語言涵蓋率、來源核對、warnings／skips 與公司內結果位置。命令輸出與 report／execution 格式不同時保留原格式，不把某個 repo 的 wrapper metadata 當所有 repo 都有。

保留 `integrity.node` warning 及 `consistency_status=imported_unverified`；`graph.verified=pass` 不會將該標記改成業務語意已核准。來源版本不等於部署版本，靜態 CALLS 不等於所有 runtime 呼叫；零 flow coverage、未知模組或未定位範圍都如實揭露。內容抽查另依使用者範圍進行，不阻塞已要求的機械建圖驗證，也不自動查詢／跨 repo 合併／分享。

完成後，新的 repo 可在新 session 呼叫同一 Skill；必要狀態保存在獨立工作區，不依賴舊 session。若只是本次程式成功，不要宣稱六個來源專案的全部業務語意或兩種公司 Codex 客戶端都已驗收。
