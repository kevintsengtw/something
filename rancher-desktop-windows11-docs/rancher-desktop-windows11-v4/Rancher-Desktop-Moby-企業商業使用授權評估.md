# Rancher Desktop 與 Moby 開源授權及企業商業使用評估

> 文件版本：1.0｜查核日期：2026-11-06｜用途：企業內部軟體授權／資安審查參考
>
> 本文件為公開授權條款之技術與合規解讀，不取代企業法務意見；實際部署須檢查使用版本所附第三方授權與公司政策。

## 1. 執行摘要

**結論：Rancher Desktop 與 Moby 專案均以 Apache License 2.0（Apache-2.0）授權。該授權允許公司內部、營利性與商業軟體開發使用，沒有按企業員工數、營收、開發者席次或公司規模收費的條件。** 因此，公司在 Windows 11 上使用 Rancher Desktop + Moby 進行 .NET Testcontainers 整合測試，**不會僅因為企業規模較大而需要購買 Docker Desktop 訂閱**。

但「可以商用」**不等於完全沒有義務**：散布軟體、改作與再包裝時需遵守 Apache-2.0 第 4 節的授權／著作權／NOTICE 等要求；專案商標未因授權而一併開放；專利授權存在特定終止條件。Rancher Desktop 隨附的其他元件、使用的容器映像與外部服務可能另有授權、訂閱、存取及合規要求。

## 2. 評估範圍與技術架構

```text
Windows 11 公司電腦
├── Visual Studio 2022 / Rider、.NET 8/9、xUnit、Testcontainers
├── Rancher Desktop（Windows 圖形化應用程式）
│   └── WSL2：Rancher Desktop 管理的 Linux 環境
│       └── Moby（dockerd）／containerd
│           └── Linux Containers、Images、Volumes
└── 不安裝 Docker Desktop；不必另裝 Ubuntu
```

評估的授權主體：**Rancher Desktop 專案**與 **Moby 專案**。不應把 Docker Desktop、Docker Hub、Docker 官方映像、Microsoft SQL Server 容器、第三方擴充套件的條款視為相同授權。

## 3. 原始授權證據

| 專案 | 官方來源 | 確認事項 |
|---|---|---|
| Rancher Desktop | https://github.com/rancher-sandbox/rancher-desktop/blob/main/LICENSE | 根目錄 LICENSE 為 Apache License 2.0 |
| Rancher Desktop | https://github.com/rancher-sandbox/rancher-desktop | Repository 標示 Apache-2.0 |
| Moby | https://github.com/moby/moby/blob/master/LICENSE | 根目錄 LICENSE 為 Apache License 2.0 |
| Moby | https://github.com/moby/moby | 官方專案說明 Licensing: Apache License, Version 2.0 |
| Apache Software Foundation | https://www.apache.org/licenses/LICENSE-2.0 | 授權原文及第 2–9 節 |
| Docker 官方 | https://docs.docker.com/subscription-billing/desktop-license/ | Docker Desktop 的訂閱限制，並註明 Docker / Moby 開源專案授權未改變 |

**查核注意：** 上述為查核當日預設分支的證據。正式採購／資產審查應記錄實際安裝版本、Release Tag、套件來源、LICENSE/NOTICE 與第三方套件清單，不應只依賴預設分支的授權狀態。

## 4. Apache-2.0 對企業使用的授權範圍

| 使用情境 | 授權判斷 | 說明 |
|---|---|---|
| 個人電腦使用 | 允許 | 不需付費授權 |
| 公司配發的 Windows 電腦 | 允許 | 不是只限個人或非營利用途 |
| 企業內部開發與測試 | 允許 | 包括商業產品的開發、xUnit、Testcontainers |
| 公司超過 250 名員工 | 允許 | Apache-2.0 無此人數門檻 |
| 公司年營收超過 1,000 萬美元 | 允許 | Apache-2.0 無此營收門檻 |
| 多人團隊／多台電腦部署 | 允許 | 無席次授權或按台計價條件 |
| 政府機關／金融業等組織 | 授權本身允許 | 另須符合產業法規、採購、資安政策 |
| 修改原始碼供內部使用 | 允許 | 若對外散布修改版本，須履行相關義務 |
| 散布原始碼或二進位檔 | 允許但有條件 | 需保留授權、著作權、修改說明、適用 NOTICE |
| 以 Rancher/Moby 名稱宣稱官方認證或背書 | 不可逕行主張 | Apache-2.0 不授予一般商標使用權 |

### 4.1 企業內部使用是否需要公開公司程式碼？

**不需要。** Apache-2.0 不是強制公開衍生作品原始碼的 copyleft 授權。公司使用 Rancher Desktop 或 Moby 來開發閉源商業產品，不會因此被要求公開公司 .NET 專案原始碼。

### 4.2 企業內部使用是否必須購買 SUSE／Rancher 支援？

**Apache-2.0 本身不要求購買商業支援或訂閱。** 但開源授權也不保證企業級 SLA、修補時效、支援服務或賠償。若公司需要商業支援，應另查詢供應商當期服務條款及報價。

### 4.3 只在公司內部安裝與使用，與「對外散布」的差異

單純安裝、操作開源程式與對外提供修改版安裝包的義務程度不同。Apache-2.0 第 4 節主要針對重製與散布時的條件。即使只是內部部署，仍建議軟體資產管理系統保留授權、來源、版本與第三方元件紀錄，以便稽核。

## 5. Apache-2.0 仍須遵守的條件

1. **保留授權條款**：再散布作品或衍生作品時，向接收者提供 Apache-2.0 授權副本（第 4(a) 節）。
2. **標示修改**：修改過的檔案應附明顯修改聲明（第 4(b) 節）。
3. **保留原有聲明**：保留適用的著作權、專利、商標、歸屬聲明（第 4(c) 節）。
4. **處理 NOTICE**：原作品若附 NOTICE，散布衍生作品時須按第 4(d) 節保留適用歸屬資訊。
5. **商標限制**：Apache-2.0 第 6 節不授予一般商標使用權，不得誤稱官方合作、認證或背書。
6. **專利條款**：第 3 節包含貢獻者的專利授權；若就相關作品提起特定專利訴訟，授權可能終止。
7. **無擔保／責任限制**：第 7、8 節明定軟體依現狀提供，不能把開源授權視為供應商服務承諾。

## 6. 與 Docker Desktop 商業授權的明確區別

| 比較 | Rancher Desktop + Moby | Docker Desktop |
|---|---|---|
| 授權基礎 | Apache-2.0 開源專案 | Docker Subscription Service Agreement |
| 大型企業內部商業使用 | Apache-2.0 不因規模要求付費 | 通常需要付費訂閱 |
| 員工數門檻 | 無 | 免費商用要求少於 250 人 **且** 年營收少於 1,000 萬美元 |
| 營收門檻 | 無 | 同上 |
| 政府機關 | Apache-2.0 無特殊禁令 | Docker Desktop 依官方條款需付費訂閱 |
| 公司閉源 .NET 專案 | 允許 | 使用權依 Docker 訂閱條款判定 |

Docker 官方特別指出：**Docker 與 Moby 開源專案（如 Docker Engine）的授權與散布條款並未因 Docker Desktop 訂閱政策改變**。因此，「使用 Docker CLI／Docker API／Moby」不等於「使用 Docker Desktop」，也不會自動觸發 Docker Desktop 訂閱義務。

**但** 若公司仍安裝或使用 Docker Desktop，不能以「底層 Engine 是開源」為由忽略 Docker Desktop 本身的合約。Docker Hub、Docker Build Cloud、Docker 商業服務另有其使用條件。

Docker 官方條款（查核時顯示 2026-08-26 更新）：https://www.docker.com/legal/docker-subscription-service-agreement/

## 7. 企業部署常見授權與合規邊界

### 7.1 第三方相依套件

Rancher Desktop 是由多種開源元件組成的整合產品，不能推論所有隨附程式碼都只有 Apache-2.0。正式部署應保留該**發行版本**的第三方授權清單，針對 GPL、LGPL、MIT、BSD 等個別條款履行適用義務。不同授權不代表不能商用，但義務可能不同。

### 7.2 容器映像與應用程式

Docker Engine 的 Apache-2.0 **不會覆蓋容器內的軟體授權**。例如 SQL Server、Oracle、商業中介軟體、基底 OS 映像、字型、資料庫驅動與應用程式，都要分別評估。開發／測試授權與正式環境授權也可能不同。

### 7.3 Docker Hub 與 Registry

透過 Rancher Desktop／Moby 從 Docker Hub 拉取映像，仍可能受 Docker Hub 的服務條款、速率限制與帳戶方案約束。應區分「容器引擎授權」與「映像倉儲服務使用權」。

### 7.4 公司 IT 與資安政策

授權允許不代表公司必然准許安裝。WSL2、虛擬化、網路、Docker Socket、系統管理員權限、容器映像來源及弱點掃描仍應遵循公司內控。公司電腦若限制安裝／解除安裝，也不改變 Apache-2.0 的商業使用許可。

### 7.5 出口管制與商標

Moby 專案另提醒其使用與轉移可能受相關國家出口法規限制；跨國散布應由法務確認。商標及官方品牌素材的使用也不是 Apache-2.0 一概授權。

## 8. 依企業實際使用情境進行判斷

**情境 A：** 企業員工在公司 Windows 11 電腦安裝 Rancher Desktop，選用 Moby，在 Visual Studio / Rider 執行 xUnit + Testcontainers；不使用 Docker Desktop。**結論：** Rancher Desktop/Moby 的 Apache-2.0 允許此類內部商業用途，不因公司規模需要購買 Docker Desktop 訂閱。

**情境 B：** 大型集團在 500 台開發機部署相同工具。**結論：** Apache-2.0 沒有每台／每人授權費；仍應做好版本、第三方授權、資安與部署管控。

**情境 C：** 公司修改 Rancher Desktop 原始碼並重新封裝，提供外部客戶。**結論：** Apache-2.0 原則允許，但須遵守散布、NOTICE、修改標示、商標與其他相依套件條款。

**情境 D：** 公司使用 Moby 執行 Microsoft SQL Server 容器。**結論：** Moby 授權允許，但 SQL Server 容器內的 Microsoft 軟體授權需另外審查。

**情境 E：** 公司同時保留 Docker Desktop。**結論：** Docker Desktop 的使用仍須獨立依 Docker 訂閱條款評估；改裝 Rancher Desktop 並不自動解決過去或仍在持續的 Docker Desktop 授權義務。

## 9. 建議提交公司法務／資安的審查清單

- [ ] 記錄 Rancher Desktop 版本、安裝來源與 SHA256。
- [ ] 記錄 Moby／Docker Engine 版本。
- [ ] 留存各版本 LICENSE、NOTICE、第三方授權清單。
- [ ] 確認僅使用 Rancher Desktop + Moby，不使用需另行授權的 Docker Desktop。
- [ ] 列出會拉取的 Container Images 與各自授權。
- [ ] 檢查 Docker Hub／私有 Registry 的使用條件。
- [ ] 依公司政策核准 WSL2、虛擬化、Docker API 與映像來源。
- [ ] 評估弱點修補、版本更新、支援窗口及軟體資產盤點。
- [ ] 若有再散布／客製化，建立 NOTICE、著作權與修改紀錄流程。

## 10. 可供內部審查引用的結論文字

> 經查核 Rancher Desktop 與 Moby 官方原始碼儲存庫，兩專案皆採 Apache License 2.0 授權。此授權允許企業於商業用途下安裝、執行及使用軟體，並未設定公司員工數、營業額、開發者席次或裝置數量之收費門檻。因此，企業使用 Rancher Desktop 搭配 Moby 作為 Windows 11 開發環境之 Linux Container Runtime，不會僅因企業規模而產生 Docker Desktop 的付費訂閱義務。惟仍須遵守 Apache-2.0 適用條款，並就隨附第三方元件、容器映像、外部服務及公司資訊安全政策進行獨立審查。本結論不構成對所有相依軟體、雲端服務或商業支援之授權保證。

## 11. 官方參考資料（2026-11-06 查核）

1. Rancher Desktop LICENSE — https://github.com/rancher-sandbox/rancher-desktop/blob/main/LICENSE
2. Rancher Desktop 專案 — https://github.com/rancher-sandbox/rancher-desktop
3. Rancher Desktop 官方網站 — https://rancherdesktop.io/
4. Moby LICENSE — https://github.com/moby/moby/blob/master/LICENSE
5. Moby 專案與 Licensing 說明 — https://github.com/moby/moby
6. Apache License 2.0 原文 — https://www.apache.org/licenses/LICENSE-2.0
7. Docker Desktop License — https://docs.docker.com/subscription-billing/desktop-license/
8. Docker Subscription Service Agreement — https://www.docker.com/legal/docker-subscription-service-agreement/
9. Docker 官方訂閱 FAQ — https://www.docker.com/pricing/faq/
