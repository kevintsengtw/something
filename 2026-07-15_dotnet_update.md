# Microsoft .NET 2026 年 7 月安全性更新整理

> 文件整理日期：2026-07-15（Asia/Taipei）  
> 原始發布日期：2026-07-14（美國時間／UTC）；換算台北時間為 2026-07-15 凌晨  
> 適用範圍：.NET 8、.NET 9、.NET 10、ASP.NET Core、WPF、.NET SDK、.NET Framework

## 1. 摘要

Microsoft 在 2026 年 7 月的 Patch Tuesday 發布了一批規模相當大的 .NET 安全性更新。

- 現代 .NET（.NET 8、9、10）發布 **17 份安全性公告**。
- .NET Framework 2026 年 7 月累積更新列出 **18 項安全性修正**。
- 兩邊有 9 個重疊 CVE，合計涉及 **26 個不同 CVE**。
- 現代 .NET 的 17 項公告中：
  - 16 項為 High。
  - 1 項為 Medium。
  - 最高 CVSS 為 8.8。
- 高風險範圍包括：
  - ASP.NET Core Negotiate 驗證與授權。
  - WPF/XAML 遠端程式碼執行與權限提升。
  - TLS、SSL、X.509 與 XML Encryption。
  - HTTP/2 記憶體耗盡。
  - SignalR Stateful Reconnect。
  - SMTP 訊息偽冒。
  - .NET SDK 容器映像建置流程。

### 建議立即升級至

| 產品線 | Runtime | SDK | 支援狀態 | 官方下載 |
| --- | --- | --- | --- | --- |
| .NET 10 | 10.0.10 | 10.0.302／10.0.110 feature band | LTS | [.NET 10 下載](https://dotnet.microsoft.com/download/dotnet/10.0) |
| .NET 9 | 9.0.18 | 9.0.316／9.0.119 feature band | STS | [.NET 9 下載](https://dotnet.microsoft.com/download/dotnet/9.0) |
| .NET 8 | 8.0.29 | 8.0.423／8.0.129 feature band | LTS | [.NET 8 下載](https://dotnet.microsoft.com/download/dotnet/8.0) |

> SDK feature band 應依目前使用的 Visual Studio、`global.json` 與建置環境選擇。安全處置的核心是確認實際執行使用的 Runtime 已更新，且 SDK、容器基底映像與自包含部署也已重新產製。

## 2. 官方公告入口

- [Microsoft .NET Blog：.NET and .NET Framework July 2026 servicing releases updates](https://devblogs.microsoft.com/dotnet/dotnet-and-dotnet-framework-july-2026-servicing-updates/)
- [.NET Announcements：.NET July 2026 Updates](https://github.com/dotnet/announcements/issues/425)
- [.NET Core：July 2026 security updates 彙總](https://github.com/dotnet/core/issues/10474)
- [.NET 10.0.10 release notes](https://github.com/dotnet/core/blob/main/release-notes/10.0/10.0.10/10.0.10.md)
- [.NET 9.0.18 release notes](https://github.com/dotnet/core/blob/main/release-notes/9.0/9.0.18/9.0.18.md)
- [.NET 8.0.29 release notes](https://github.com/dotnet/core/blob/main/release-notes/8.0/8.0.29/8.0.29.md)
- [.NET Framework：July 2026 cumulative update](https://learn.microsoft.com/dotnet/framework/release-notes/2026/07-14-july-cumulative-update)
- [.NET Framework release notes 索引](https://learn.microsoft.com/dotnet/framework/release-notes/release-notes)
- [Microsoft Security Update Guide](https://msrc.microsoft.com/update-guide/)
- [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/)

## 3. 現代 .NET：17 項安全性公告

下表以 Microsoft 的 .NET Announcements 公告為準。除 CVE-2026-56170 外，本批公告列出的修補版本為 .NET 8.0.29、9.0.18 與 10.0.10。

| CVE | 類型 | 嚴重度／CVSS | 主要元件與情境 | 平台 | 修補版本 |
| --- | --- | --- | --- | --- | --- |
| [CVE-2026-57108](https://github.com/dotnet/announcements/issues/408) | Denial of Service | High／7.5 | Runtime 解析特製 X.509 憑證時可能發生 DoS | Linux、macOS | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-47300](https://github.com/dotnet/announcements/issues/409) | Elevation of Privilege | High／8.8 | ASP.NET Core Negotiate authentication handler 驗證不正確 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-47302](https://github.com/dotnet/announcements/issues/410) | Denial of Service | High／7.5 | XML encryption／XML parsing 造成資源耗盡或應用程式崩潰 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-47303](https://github.com/dotnet/announcements/issues/411) | Elevation of Privilege | High／8.8 | ASP.NET Core Negotiate handler 解析不正確，涉及驗證、授權及 LDAP injection | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-47304](https://github.com/dotnet/announcements/issues/412) | Security Feature Bypass | High／8.1 | `EncryptedXml` 驗證不正確，可能繞過加密保護 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50524](https://github.com/dotnet/announcements/issues/413) | Denial of Service | High／7.5 | 處理惡意 TLS handshake 時可能崩潰或停止回應 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50525](https://github.com/dotnet/announcements/issues/414) | Denial of Service | High／7.5 | 特製 encrypted XML 導致資源不受控消耗 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50526](https://github.com/dotnet/announcements/issues/415) | Tampering | High／7.0 | 本機攻擊者可能透過連結解析問題，將資源注入其他使用者建置的容器映像 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50527](https://github.com/dotnet/announcements/issues/416) | Denial of Service | High／7.5 | `EncryptedXml` 輸入驗證不正確，可能造成 DoS | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50528](https://github.com/dotnet/announcements/issues/417) | Security Feature Bypass | High／8.2 | `SslStream` 處理 TLS/SSL 連線時可能繞過授權檢查 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50646](https://github.com/dotnet/announcements/issues/418) | Remote Code Execution | High／7.8 | WPF 解析特製 XAML 時可能以目前使用者身分執行任意程式碼 | Windows | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50648](https://github.com/dotnet/announcements/issues/419) | Denial of Service | High／7.5 | 特製 encrypted XML 造成資源耗盡 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50649](https://github.com/dotnet/announcements/issues/420) | Remote Code Execution | High／7.8 | WPF 反序列化不受信任的 XAML 輸入 | Windows | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50650](https://github.com/dotnet/announcements/issues/421) | Elevation of Privilege | High／7.8 | WPF 解析特製 XAML 時可能造成權限提升 | Windows | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50651](https://github.com/dotnet/announcements/issues/422) | Denial of Service | High／7.5 | 攻擊者可利用 HTTP/2 造成 Out-of-Memory | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-50659](https://github.com/dotnet/announcements/issues/423) | Spoofing | Medium／6.5 | `System.Net.Mail` SMTP 訊息路由時可能被偽冒 | 全平台 | 8.0.29／9.0.18／10.0.10 |
| [CVE-2026-56170](https://github.com/dotnet/announcements/issues/424) | Denial of Service | High／7.5 | ASP.NET Core SignalR 啟用 Stateful Reconnect 時，可被用來阻斷其他使用者服務 | 全平台 | 8.0.26／9.0.15／10.0.6 起已修補 |

### 3.1 風險分類統計

| 類型 | 數量 | CVE |
| --- | ---: | --- |
| Denial of Service | 8 | 57108、47302、50524、50525、50527、50648、50651、56170 |
| Elevation of Privilege | 3 | 47300、47303、50650 |
| Remote Code Execution | 2 | 50646、50649 |
| Security Feature Bypass | 2 | 47304、50528 |
| Tampering | 1 | 50526 |
| Spoofing | 1 | 50659 |

### 3.2 優先處理建議

第一優先：

1. 對外提供服務且使用 ASP.NET Core Negotiate authentication 的應用程式。
2. 會載入或解析外部、不受信任 XAML 的 Windows WPF 應用程式。
3. 接收不受信任 XML、encrypted XML、TLS 或 HTTP/2 流量的網路服務。
4. 使用 `SslStream` 自訂 TLS／授權流程的服務。

第二優先：

1. 共用建置主機、CI runner 或多使用者環境中的容器映像建置流程。
2. 使用 `System.Net.Mail` 處理外部提供 SMTP 欄位的系統。
3. 啟用 SignalR Stateful Reconnect 且仍使用 8.0.25／9.0.14／10.0.5 或更舊版本的服務。

## 4. .NET Framework：18 項安全性修正

Microsoft 的 .NET Framework 2026 年 7 月累積更新列出以下 18 項修正。

| CVE | .NET Framework 公告分類 | MSRC |
| --- | --- | --- |
| CVE-2026-47302 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-47302) |
| CVE-2026-47304 | Security Feature Bypass | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-47304) |
| CVE-2026-50304 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50304) |
| CVE-2026-50324 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50324) |
| CVE-2026-50355 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50355) |
| CVE-2026-50368 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50368) |
| CVE-2026-50411 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50411) |
| CVE-2026-50525 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50525) |
| CVE-2026-50527 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50527) |
| CVE-2026-50646 | Elevation of Privilege | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50646) |
| CVE-2026-50647 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50647) |
| CVE-2026-50648 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50648) |
| CVE-2026-50649 | Remote Code Execution | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50649) |
| CVE-2026-50650 | Elevation of Privilege | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50650) |
| CVE-2026-50652 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50652) |
| CVE-2026-50653 | Denial of Service | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50653) |
| CVE-2026-50659 | Tampering | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50659) |
| CVE-2026-56158 | Remote Code Execution | [MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-56158) |

> 同一 CVE 在現代 .NET 與 .NET Framework 中可能因受影響元件或產品情境不同，而出現不同的影響分類。例如 CVE-2026-50646 與 CVE-2026-50659 應以實際產品的 MSRC affected products 資料為準。

### 4.1 品質與可靠性修正

這次 .NET Framework 更新同時包含非安全性的品質修正：

- 修正同步 `HttpWebRequest` 在特定安全連線及伺服器設定下可能停滯的問題。
- 修正 Visual Studio x86 native debugger 在 mixed-code 應用程式逐步執行時，部分浮點數顯示不正確的問題。
- Microsoft 公告目前表示此版本沒有已知問題。

## 5. .NET Framework KB 對照

### 5.1 Windows 11 與 Windows Server 新版

| 作業系統 | 已安裝的 .NET Framework | 更新 KB |
| --- | --- | --- |
| Windows 11 26H1 | 4.8.1 | [KB5101002](https://support.microsoft.com/kb/5101002) |
| Windows 11 26H1 | 3.5 完整產品更新 | [KB5101014](https://support.microsoft.com/kb/5101014) |
| Windows 11 25H2 | 3.5、4.8.1 | [KB5100998](https://support.microsoft.com/kb/5100998) |
| Windows 11 24H2 | 3.5、4.8.1 | [KB5101001](https://support.microsoft.com/kb/5101001) |
| Microsoft Server OS 24H2 | 3.5、4.8.1 | [KB5100998](https://support.microsoft.com/kb/5100998) |
| Microsoft Server OS 23H2 | 3.5、4.8.1 | [KB5100999](https://support.microsoft.com/kb/5100999) |
| Windows 11 22H2／23H2 | 3.5、4.8.1 | [KB5101004](https://support.microsoft.com/kb/5101004) |

### 5.2 Windows Server 2022、Windows 10 與 Windows Server 2016／2019

| 作業系統 | 已安裝的 .NET Framework | 更新 KB |
| --- | --- | --- |
| Windows Server 2022 | 更新 offering KB | [KB5102206](https://support.microsoft.com/kb/5102206) |
| Windows Server 2022 | 3.5、4.8 | [KB5101010](https://support.microsoft.com/kb/5101010) |
| Windows Server 2022 | 3.5、4.8.1 | [KB5101005](https://support.microsoft.com/kb/5101005) |
| Windows 10 22H2 | 更新 offering KB | [KB5102203](https://support.microsoft.com/kb/5102203) |
| Windows 10 21H2 | 更新 offering KB | [KB5102202](https://support.microsoft.com/kb/5102202) |
| Windows 10 21H2／22H2 | 3.5、4.8 | [KB5101006](https://support.microsoft.com/kb/5101006) |
| Windows 10 21H2／22H2 | 3.5、4.8.1 | [KB5101000](https://support.microsoft.com/kb/5101000) |
| Windows 10 1809／Windows Server 2019 | 更新 offering KB | [KB5102201](https://support.microsoft.com/kb/5102201) |
| Windows 10 1809／Windows Server 2019 | 3.5、4.7.2 | [KB5100989](https://support.microsoft.com/kb/5100989) |
| Windows 10 1809／Windows Server 2019 | 3.5、4.8 | [KB5101008](https://support.microsoft.com/kb/5101008) |
| Windows 10 1607／Windows Server 2016 | 3.5、4.6.2、4.7、4.7.1、4.7.2 | [KB5099535](https://support.microsoft.com/kb/5099535) |
| Windows 10 1607／Windows Server 2016 | 4.8 | [KB5101007](https://support.microsoft.com/kb/5101007) |

### 5.3 Windows Server 2012／2012 R2

| 作業系統 | 已安裝的 .NET Framework | 更新 KB |
| --- | --- | --- |
| Windows Server 2012 R2 | Security and Quality Rollup | [KB5102205](https://support.microsoft.com/kb/5102205) |
| Windows Server 2012 R2 | 3.5 | [KB5100985](https://support.microsoft.com/kb/5100985) |
| Windows Server 2012 R2 | 4.6.2、4.7、4.7.1、4.7.2 | [KB5100991](https://support.microsoft.com/kb/5100991) |
| Windows Server 2012 R2 | 4.8 | [KB5101011](https://support.microsoft.com/kb/5101011) |
| Windows Server 2012 | Security and Quality Rollup | [KB5102204](https://support.microsoft.com/kb/5102204) |
| Windows Server 2012 | 3.5 | [KB5100986](https://support.microsoft.com/kb/5100986) |
| Windows Server 2012 | 4.6.2、4.7、4.7.1、4.7.2 | [KB5100990](https://support.microsoft.com/kb/5100990) |
| Windows Server 2012 | 4.8 | [KB5101009](https://support.microsoft.com/kb/5101009) |

> 「更新 offering KB」是 Windows Update 用來判斷適用性的 KB。實際安裝記錄中，通常會看到對應 .NET Framework 版本的 KB，而不一定會看到 offering KB 本身。

## 6. 更新與驗證流程

### 6.1 盤點目前版本

```bash
dotnet --info
dotnet --list-sdks
dotnet --list-runtimes
```

至少要確認下列 runtime：

- `Microsoft.NETCore.App`
- `Microsoft.AspNetCore.App`
- Windows WPF 應用程式還應確認 `Microsoft.WindowsDesktop.App`

Windows PowerShell 可搭配：

```powershell
dotnet --info
dotnet --list-sdks
dotnet --list-runtimes
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 30
```

### 6.2 Framework-dependent 應用程式

1. 更新主機上的 .NET Runtime／ASP.NET Core Runtime／Hosting Bundle。
2. 若透過 Visual Studio 安裝 SDK，應更新 Visual Studio。
3. 重新啟動 IIS、Windows Service、systemd service、容器工作負載或相關應用程式。
4. 執行健康檢查與關鍵交易測試。

### 6.3 Self-contained deployment

Self-contained 應用程式攜帶自己的 Runtime，不會因主機安裝新 Runtime 而自動完成修補。必須：

1. 使用已更新的 SDK／Runtime 重新 restore、build、publish。
2. 重新產製部署套件。
3. 重新部署所有節點。
4. 確認舊 artifact、舊映像與舊執行個體已停止使用。

範例：

```bash
dotnet restore
dotnet publish -c Release -r linux-x64 --self-contained true
```

### 6.4 容器工作負載

1. 更新 `mcr.microsoft.com/dotnet/aspnet`、`runtime` 或 `sdk` base image tag／digest。
2. 執行乾淨的 image rebuild。
3. 執行弱點掃描與 SBOM 比對。
4. 推送新映像並重新部署。
5. 確認執行中的 container digest 已切換，不能只重新啟動舊容器。

常見檢查：

```bash
docker run --rm mcr.microsoft.com/dotnet/aspnet:8.0 dotnet --info
docker run --rm mcr.microsoft.com/dotnet/aspnet:9.0 dotnet --info
docker run --rm mcr.microsoft.com/dotnet/aspnet:10.0 dotnet --info
```

### 6.5 NuGet 套件

若專案直接引用公告列出的 `System.*`、`Microsoft.NETCore.App.Runtime.*`、`Microsoft.AspNetCore.App.Runtime.*` 或相關 runtime-specific 套件，除了更新機器 Runtime，還要更新套件參考並重建。

```bash
dotnet list package --vulnerable --include-transitive
dotnet list package --outdated
```

> `dotnet list package --vulnerable` 依賴套件弱點資料來源；它不能取代 Runtime、Hosting Bundle、Visual Studio、Windows Update、容器映像及 self-contained artifact 的盤點。

## 7. 建議驗收清單

- [ ] 所有 .NET 8 主機至少已達 Runtime 8.0.29。
- [ ] 所有 .NET 9 主機至少已達 Runtime 9.0.18。
- [ ] 所有 .NET 10 主機至少已達 Runtime 10.0.10。
- [ ] ASP.NET Core Hosting Bundle 與 IIS 主機已更新並重啟。
- [ ] Visual Studio 與 build agent SDK 已更新。
- [ ] `global.json` 沒有把建置鎖在未修補 SDK。
- [ ] Self-contained artifacts 已重新 publish 並部署。
- [ ] 所有容器 base image 已重新 pull／build，執行中的 digest 已更新。
- [ ] WPF/XAML 應用程式完成針對不受信任輸入的風險檢查。
- [ ] 使用 Negotiate authentication 的 ASP.NET Core 應用程式優先完成修補。
- [ ] 使用 XML Encryption、`EncryptedXml`、`SslStream`、HTTP/2、SignalR 或 `System.Net.Mail` 的服務完成回歸測試。
- [ ] Windows／Windows Server 已安裝適用的 .NET Framework KB。
- [ ] 更新後已執行健康檢查、關鍵功能測試與安全掃描。
- [ ] 已記錄主機、Runtime、SDK、容器 digest、KB 與部署時間，供稽核追蹤。

## 8. 結論

這次更新不應只被視為一般的每月 servicing release。它同時涵蓋驗證／授權、WPF 遠端程式碼執行、TLS／SSL、XML Encryption、HTTP/2、SMTP、SignalR 與容器建置流程等多個攻擊面。

建議組織將下列工作列為同一批修補範圍：

1. 更新 .NET Runtime、SDK、ASP.NET Core Runtime 與 Hosting Bundle。
2. 更新 Visual Studio 及 CI/CD build agents。
3. 重新建置並部署 self-contained applications。
4. 更新並重新產製容器映像。
5. 透過 Windows Update 或 Microsoft Update Catalog 安裝 .NET Framework 累積更新。
6. 完成版本、KB、artifact 與 container digest 的實際驗證。

只更新開發機 SDK，或只在伺服器上安裝 Runtime，都不足以保證所有已部署工作負載已完成修補。

## 9. 主要參考資料

1. [Microsoft .NET Blog：July 2026 servicing releases updates](https://devblogs.microsoft.com/dotnet/dotnet-and-dotnet-framework-july-2026-servicing-updates/)
2. [.NET Announcements：July 2026 Updates](https://github.com/dotnet/announcements/issues/425)
3. [.NET Core：July 2026 security update status](https://github.com/dotnet/core/issues/10474)
4. [.NET Framework：July 2026 cumulative update](https://learn.microsoft.com/dotnet/framework/release-notes/2026/07-14-july-cumulative-update)
5. [Microsoft Security Update Guide](https://msrc.microsoft.com/update-guide/)
6. [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/)

---

本文件依 2026-07-15 查得的 Microsoft 與 .NET 官方公開資訊整理。Microsoft 後續可能修訂 CVSS、受影響產品、已知問題或 KB 適用範圍；實際部署前仍應再次查看各 CVE 的 MSRC 頁面及作業系統對應 KB。
