# Codex CLI Windows 10 / 11 免安裝（Portable Package）部署指南

> 適用對象：Windows 10 / Windows 11 x64、公司環境禁止安裝軟體、需使用
> Codex CLI 的開發團隊。

## 1. 目的

本文件說明如何在 **不安裝 Codex CLI** 的前提下，於 Windows
環境部署與使用 Codex CLI。

> 建議使用 **Portable Package**，不要使用單一
> `codex-x86_64-pc-windows-msvc.exe`。

------------------------------------------------------------------------

## 2. 適用環境

-   Windows 10 x64
-   Windows 11 x64
-   不需系統管理員權限
-   可於使用者目錄或工具目錄執行

------------------------------------------------------------------------

## 3. 下載

前往 OpenAI Codex GitHub Releases。

下載目前版本的：

``` text
codex-package-x86_64-pc-windows-msvc.tar.gz
```

不要下載：

``` text
codex-x86_64-pc-windows-msvc.exe
```

原因：新版 Code Mode 已採用完整 Package，單一 EXE
無法保證包含所有必要元件。

------------------------------------------------------------------------

## 4. 解壓縮

Windows 11（或已安裝 bsdtar）：

``` powershell
tar -xzf codex-package-x86_64-pc-windows-msvc.tar.gz
```

或使用 7-Zip：

1.  解開 `.tar.gz`
2.  再解開 `.tar`

------------------------------------------------------------------------

## 5. 建議目錄

``` text
C:\Tools\
└── Codex\
    └── <version>\
        ├── bin\
        ├── codex-path\
        ├── codex-resources\
        └── codex-package.json
```

------------------------------------------------------------------------

## 6. 執行

請執行：

``` text
bin\codex.exe
```

不要直接執行 Package 外的單一 EXE。

------------------------------------------------------------------------

## 7. PATH（可選）

可加入：

``` text
C:\Tools\Codex\<version>\bin
```

之後即可：

``` powershell
codex --version
```

------------------------------------------------------------------------

## 8. 驗證

``` powershell
codex --version
codex doctor
```

若版本正常且診斷通過，即表示部署成功。

------------------------------------------------------------------------

## 9. 更新

1.  下載新版 Package。
2.  解壓至新版本資料夾。
3.  更新 PATH（若使用版本化資料夾）。
4.  驗證 `codex --version`。

------------------------------------------------------------------------

## 10. 常見問題

### `codex-code-mode-host.exe` 不存在

通常原因：

-   使用了單一 EXE。
-   Package 解壓不完整。
-   `bin` 或 `codex-resources` 被移動或刪除。

請重新下載完整 Package 並保持原始目錄結構。

### Agent Orchestrator

請指定：

``` text
<PackageRoot>\bin\codex.exe
```

不要指定單一下載的 executable。

------------------------------------------------------------------------

## 11. 建議

-   使用完整 Package。
-   不要混用不同版本元件。
-   每個版本放在獨立資料夾。
-   團隊統一使用相同版本。

------------------------------------------------------------------------

## 12. 參考

-   OpenAI Codex GitHub Releases
-   OpenAI Codex README
-   OpenAI Package 說明
