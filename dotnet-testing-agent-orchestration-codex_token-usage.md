# dotnet-testing-agent-orchestration-codex Token-Usage

這個專案不計算「正式 token usage」，而是產生 `Estimated Token Usage`，用途是比較不同 workflow run 的相對成本。

原因是 Codex native SpawnAgent 沒有暴露包含 Orchestrator 與所有 Subagent 的完整 token truth source，因此無法取得可靠的 billing/runtime 實際用量。

## 核心公式

估算器使用零相依的字元啟發式：

```text
估算 token = ceil(Unicode 字元數 ÷ 3.6)
```

實作位於 [estimate-token-usage.mjs](C:/github/dotnet-testing-agent-orchestration-codex-lab/.codex/scripts/estimate-token-usage.mjs:138)。

例如：

```text
文字長度：3,600 字元
估算 token：ceil(3,600 / 3.6) = 1,000
```

它不是 BPE tokenizer；中文、程式碼、JSON 等內容的絕對誤差可能較大，但所有 workflow 都使用同一把尺，因此適合做相對比較。

## 每個 Subagent 計算哪些內容

估算器從 `.orchestrator/run-state.json` 找出 Analyzer、Writer、Executor、Reviewer 的每筆 assignment，分成 input 與 output。

Input：

```text
spawn payload
+ Agent 定義 TOML
+ 實際讀取的 source / skill / handoff 檔案
+ artifact 內引用的 build、test 等工具輸出
```

Output：

```text
寫入的測試或程式檔案
+ Subagent 交接 artifact
```

所以每筆 assignment 的計算是：

```text
Input =
  orchestratorPayloadTokens
  + agentTomlTokens
  + readFileTokens
  + skillTokens
  + toolOutputTokens

Output =
  writtenFileTokens
  + artifactTokens

Total = Input + Output
```

對應實作在 [estimate-token-usage.mjs](C:/github/dotnet-testing-agent-orchestration-codex-lab/.codex/scripts/estimate-token-usage.mjs:461)。

Subagent 不自行宣稱 token 數字，只在交接 artifact 的 `tokenEstimateInputs` 登記實際材料：

```json
{
  "tokenEstimateInputs": {
    "schemaVersion": 1,
    "estimateKind": "visible-context",
    "readFiles": [],
    "writtenFiles": [],
    "toolOutputRefs": []
  }
}
```

最後由共用估算器統一讀檔、計算。

## Low / High 區間

每筆 assignment 的 low 就是 visible-context 直接估算值；high 則套用角色 overhead：

| 階段 | High 倍率 |
|---|---:|
| Analyzer | 1.15 |
| Writer | 1.20 |
| Executor | 1.25 |
| Reviewer | 1.20 |

```text
low  = Input + Output
high = ceil(low × 角色倍率)
```

整體 summary：

```text
總 Input  = 所有 assignment Input 加總
總 Output = 所有 assignment Output 加總
總 Low    = 總 Input + 總 Output
總 High   = 每筆 assignment High 加總
```

因此 `totalTokensEstimated` 是未套 overhead 的 visible-context 數字；overhead 只反映在 `range.high`，不是另外加進 total。

## 去重方式

估算器有兩層去重：

- 同一份檔案在單一 `readFiles` 或 `writtenFiles` 清單重複出現，只計一次。
- 同一 phase 的多個 assignment 共用同一份 merged artifact，例如 two-step Writer，只在第一次計 artifact 衍生內容；每個 assignment 自己的 Agent TOML 與 payload 仍分別計算。

這避免分批 Writer 因共用 `writer-result.json` 而接近倍數高估。實作位於 [estimate-token-usage.mjs](C:/github/dotnet-testing-agent-orchestration-codex-lab/.codex/scripts/estimate-token-usage.mjs:413)。

## Confidence

因為使用字元啟發式，confidence 最高只有 `medium`：

- `medium`：artifact 存在、`tokenEstimateInputs` 完整、讀寫檔案都找得到。
- `low`：缺少 telemetry、使用 artifact fallback，或部分檔案不存在。
- `unavailable`：缺少 artifact 或完全無法產生有效估算。

整體 confidence 取所有 assignment 中最低的值。

## 執行方式

```bash
node .codex/scripts/estimate-token-usage.mjs \
  --test-project <測試專案路徑>
```

輸出：

```text
<測試專案>/.orchestrator/token-usage-estimate.json
```

完整使用說明在 [token-usage-estimation.md](C:/github/dotnet-testing-agent-orchestration-codex-lab/docs/guides/token-usage-estimation.md)。

## 必須注意的限制

這個數字不包含：

- Orchestrator 主執行緒
- Codex hidden framing
- Internal reasoning tokens
- Tool-call serialization overhead
- Cached input token accounting
- Provider 實際 billing usage

另外，某些 Analyzer artifact 會內嵌 `sourceCodeContext`，可能同時以 read、written/artifact 形式被重複計算，造成局部高估。

所以正確解讀是：

```text
同一套估算口徑下，Run A 相對於 Run B 使用了多少可見上下文
```

不能解讀為：

```text
這次 API 或 Codex 實際收費了多少 token
```

估算失敗也不會影響測試結果、Reviewer 評分或 workflow correctness gate。