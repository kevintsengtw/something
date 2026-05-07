# GCP Vertex AI 無金鑰實作指南：ADC 與 .NET 10 深度整合全手冊

本文件詳解如何透過 **Application Default Credentials (ADC)** 達成「代碼與金鑰解耦」，並紀錄從環境建置到 .NET 10 實作過程中的所有技術細節。

---

## 1. Google Cloud SDK 環境安裝
本章節以官方安裝程式 (Installer) 為主，確保系統路徑與元件完整。

### 1.1 執行安裝程式
1.  **下載**：前往 [Google Cloud SDK 官網](https://cloud.google.com/sdk/docs/install#windows) 下載 **Google Cloud CLI 安裝程式**。
2.  **安裝步驟**：
    * 執行 `.exe` 檔案。
    * 安裝過程中建議勾選 **"Bundled Python"**，以確保 `gcloud` 指令能正常運作。
    * 結束前勾選 **"Run 'gcloud init'"**，這會引導你完成初始專案設定。
3.  **確認安裝**：
    開啟 PowerShell 執行以下指令，確認版本資訊：
    ```powershell
    gcloud version
    ```

---

## 2. 三階段 ADC 授權流程 (關鍵指令序列)
這是確保 C# SDK 在不手動載入 JSON 檔案的情況下，能自動獲取權限的核心步驟。

### 步驟 A：授權 CLI 工具
讓 `gcloud` 指令有權限管理您的雲端專案資源。
```powershell
gcloud auth login
```

### 步驟 B：建立應用程式預設憑證 (ADC)
這是開發環境最重要的步驟，它會在本機產出一個供 SDK 偵測的身分連結。
```powershell
gcloud auth application-default login
```
* **關鍵動作**：網頁跳出後，務必勾選 **「查看、編輯、設定和刪除您的 Google Cloud 資料...」**（對應 `cloud-platform` 權限）。
* **產出位置**：`%AppData%\gcloud\application_default_credentials.json`。

### 步驟 C：設定配額專案 (解決 Quota 警告)
解決呼叫時出現「Cannot find a quota project」的黃色警告，確保 API 計費路徑正確。
```powershell
gcloud auth application-default set-quota-project [您的專案ID]
# 範例：gcloud auth application-default set-quota-project project-128ddc6f-f88b-4d01-a55
```

---

## 3. 核心偵測機制：優先權解析
在 PowerShell 執行 `$env:GOOGLE_APPLICATION_CREDENTIALS` 若結果為 `null` 是**完全正確**的。Google SDK 尋找通行證的優先順序如下：

1.  **環境變數**：若設定了 `GOOGLE_APPLICATION_CREDENTIALS`，則 SDK 強迫讀取該路徑。
2.  **ADC 標準路徑**：若變數為空，則自動尋找步驟 B 產出的 AppData 憑證。
3.  **Metadata Server**：若前兩者皆無，則詢問雲端內網 (169.254.169.254)。

---

## 4. LINQPad 9 驗證腳本 (避坑修正版)
本代碼已解決 `CS0104 (Value 衝突)` 與 `FailedPrecondition (API 路由錯誤)`。

```csharp
#r "nuget: Google.Cloud.AIPlatform.V1, 3.16.0"

using Google.Cloud.AIPlatform.V1;

async Task Main()
{
	// 1. 配置參數
	string projectId = "project-128ddc6f-f88b-4d01-a55"; 
	string location = "us-central1";
	string modelId = "gemini-3.0-flash";
	string endpoint = $"{location}-aiplatform.googleapis.com";

	// 2. 初始化 Client (SDK 會依據優先權自動抓取 ADC)
	var client = await new PredictionServiceClientBuilder { Endpoint = endpoint }.BuildAsync();

	// 3. 構建 Gemini 專用請求 (使用強型別 GenerateContentRequest 避開 Value 類別衝突)
	var request = new GenerateContentRequest
	{
		Model = $"projects/{projectId}/locations/{location}/publishers/google/models/{modelId}",
		Contents = 
		{
			new Content
			{
				Role = "user",
				Parts = { new Part { Text = "請用後端工程師的口吻，簡短介紹 ADC 的優點？" } }
			}
		}
	};

	try
	{
		"正在發送請求至 Vertex AI (GenerateContent)...".Dump();
		
		// 呼叫 GenerateContentAsync (Gemini 模型專用接口)
		var response = await client.GenerateContentAsync(request);

		// 解析結果：Candidates -> Content -> Parts -> Text
		var responseText = response.Candidates[0].Content.Parts[0].Text;
		responseText.Dump("Gemini 回覆內容");
	}
	catch (Exception ex)
	{
		ex.Dump("發生錯誤");
	}
}
```

---

## 5. .NET 10 Web 專案實戰整合 (DI 模式)
將上述邏輯整合進 Web API 的標準做法，透過 Options 模式達成環境解耦。

### 5.1 配置定義 (appsettings.json)
```json
"VertexAI": {
  "ProjectId": "project-128ddc6f-f88b-4d01-a55",
  "Location": "us-central1",
  "ModelId": "gemini-3.0-flash"
}
```

### 5.2 依賴注入註冊 (Program.cs)
```csharp
builder.Services.AddSingleton(sp => {
    var config = builder.Configuration.GetSection("VertexAI");
    var endpoint = $"{config["Location"]}-aiplatform.googleapis.com";
    // 註冊 Singleton Client，執行時會自動偵測 ADC
    return new PredictionServiceClientBuilder { Endpoint = endpoint }.Build();
});
```

---

## 6. 排錯與避坑指南 (Lessons Learned)

### 🚨 錯誤 1：CS0104 'Value' is an ambiguous reference
* **原因**：`AIPlatform.V1` 與 `Protobuf.WellKnownTypes` 同時包含名為 `Value` 的類別。
* **對策**：改用強型別物件 **`GenerateContentRequest`**，避開直接操作動態 JSON 的 `Value` 物件。

### 🚨 錯誤 2：FailedPrecondition (Gemini cannot be accessed through Predict API)
* **原因**：Gemini 屬於生成式 AI，不支援通用型 `Predict()` 或 `RawPredict()`。
* **對策**：必須改用專門的 **`GenerateContentAsync()`** 接口。

### 🚨 錯誤 3：環境變數為 null
* **現象**：執行 `$env:GOOGLE_APPLICATION_CREDENTIALS` 得到 `null`。
* **對策**：**此為正常現象！** 代表 SDK 已成功進入 ADC 自動偵測模式。除非要「手動覆蓋」身分，否則不需設定此變數。

---

## 7. Cloud Run 部署原理
部署到 Cloud Run 後，身分驗證會自動切換：
1.  SDK 偵測到環境變數為空，會自動向 **Metadata Server** (169.254.169.254) 請求憑證。
2.  自動獲取該服務指派之 **Service Account** 的臨時權杖。
3.  **結論**：本地與雲端代碼達成 100% 相同，真正實現「零金鑰管理」。

---
*Documented for Kevin Tseng | 2026-05*