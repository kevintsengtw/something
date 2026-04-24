# NotebookLM + Claude Chat 簡報製作流程

> 利用 NotebookLM 產出內容素材，再透過 Claude Chat 的 Design 與 PowerPoint 功能，製作出文字正確、風格精美的簡報。

---

## 前置說明

### 為什麼需要這個流程？

NotebookLM 產出的簡報 PDF 視覺效果佳，但中文字是以「繪製」方式處理（向量路徑），並非真正的文字層，因此常出現幻覺字（錯字、亂碼）。

**解決方案：** 將「文字內容」與「視覺風格」分離，各取所長。

| 素材來源 | 用途 |
|---|---|
| NotebookLM 文字稿 | 確保文字內容正確 |
| NotebookLM PDF | 提供視覺風格參考 |
| Claude Chat | 重製視覺、輸出可編輯 PPTX |

---

## 製作步驟

### Step 1：在 NotebookLM 建立簡報文字與設計說明文稿

在 NotebookLM 中，請它產出一份**純文字的簡報腳本**，內容包含：

- 每張投影片的標題
- 每張投影片的文字內容（條列或段落）
- 設計風格說明，例如：
  - 主色調、強調色
  - 版面配置（左右分割、上下、全版圖片等）
  - 圖示或插圖風格描述

**建議的 NotebookLM Prompt：**

```
請根據以下資料，產出一份簡報製作文稿，包含：
1. 每張投影片的標題與文字內容
2. 每張投影片的版面設計說明（主色、版面配置、視覺風格）
請以純文字格式輸出，不要產生圖片。
```

---

### Step 2：在 NotebookLM 產出簡報 PDF

使用 NotebookLM 的簡報功能，產出視覺化的簡報 PDF。

> 此 PDF 僅作為**視覺風格參考**，不用於文字辨識。

---

### Step 3：準備兩份素材

確認手邊有以下兩份檔案：

- `簡報文字與設計說明.txt`（或 .md）— Step 1 產出的純文字稿
- `簡報視覺參考.pdf` — Step 2 產出的 PDF

---

### Step 4：上傳至 Claude Chat

開啟 [claude.ai](https://claude.ai) 的對話介面，**同時上傳兩份檔案**，並輸入以下提示：

```
這是簡報的文字稿與設計說明（內容來源），以及 NotebookLM 產出的 PDF（視覺風格參考）。
請參考 PDF 的色調、版面風格與設計語言，使用文字稿的內容，
為每張投影片製作視覺設計稿。
```

---

### Step 5：Claude Design 製作視覺稿（SVG 即時預覽）

Claude 會在對話中以 SVG 方式渲染每張投影片的視覺稿，可即時預覽。

**這個階段可以進行迭代調整：**

- 調整色調：「請將主色改為深藍色系」
- 調整版面：「第三張改為左圖右文的版面」
- 調整字體大小、強調重點等

> 反覆確認直到滿意為止，再進行下一步。

---

### Step 6：輸出為 PowerPoint 檔案

視覺稿確認完成後，輸入以下指令：

```
請根據剛才設計好的投影片內容，產出可下載的 PowerPoint (.pptx) 檔案。
```

Claude 會使用 python-pptx 重建投影片並提供下載連結。

---

### Step 7：下載並後製

下載 `.pptx` 檔案後，可在 PowerPoint 或 LibreOffice Impress 中：

- 微調字型、間距
- 替換或新增圖片
- 加入動畫或轉場效果

---

## 注意事項

### 文字稿要夠詳細

設計說明越具體，Claude 重製的風格越接近原版。建議包含：

- 主色 HEX 色碼（如果 NotebookLM 有標示）
- 每張投影片的核心視覺概念（「資料圖表為主」、「大標題全版」等）

### PPTX 與 SVG 的差異

Claude Design 的 SVG 預覽與最終 PPTX 可能有些微差異，因為：

- SVG 是向量渲染，PPTX 是以 python-pptx 重建
- 複雜的漸層、陰影效果在 PPTX 中重現程度有限

### 執行環境

此流程**完全在 Claude Chat 執行**，不需要 Claude Cowork。

---

## 流程總覽

```
NotebookLM
  ├──① 產出文字稿與設計說明（純文字）
  └──② 產出簡報 PDF（視覺參考）
          ↓
     上傳至 Claude Chat
          ↓
  ③ Claude Design
     參考 PDF 風格 + 文字稿內容
     → SVG 即時預覽，反覆迭代調整
          ↓
     確認滿意
          ↓
  ④ Claude To PowerPoint
     → 輸出 .pptx 可編輯檔案
          ↓
  ⑤ 本機後製（PowerPoint / LibreOffice）
```

---

*適用環境：Claude Chat（claude.ai）｜建議搭配 NotebookLM 使用*
