# VS Code 1.137 更新重點摘要

> 原文：<https://code.visualstudio.com/updates/v1_137>
> 發布日期：2026 年 9 月 9 日

## 1. Automations（預覽）

新增可排程的 agent 任務。啟用 `chat.automations.enabled` 後，可以在 Agents 視窗的側邊欄找到 **Automations**，用內建範本（追蹤變更、分類 issue、找 bug）或自訂 prompt 建立任務，設定成每小時、每天或每週執行，也可以隨時手動觸發。目前是 Preview 階段，逐步推送給所有使用者。

## 2. Voice Mode（實驗性）

可以用說的跟 agent 對話，而且在 agent 回應的過程中直接開口或用 push-to-talk 快捷鍵打斷它。Voice Mode 知道目前的 session 狀態，能回答執行中的 session、選用模型與已附加檔案的問題，也可以要求它開新的 session，並會告知請求被送到哪裡。相關設定為 `agents.voice.enabled`、`agents.voice.showTranscript`、`agents.voice.voice`。

## 3. GitHub issue 與 pull request 的整合強化

三個層面同時推進：Agents 視窗可以直接開啟 `github.com` 的 issue 或 PR 詳細資料（實驗性，需要 `extensions.experimental.enableAgentsWindowCapability`，且沒有開啟該儲存庫的工作區也能用）；任何 chat 輸入框的 **Add Context...** 都能把 issue 或 PR 加入 context，貼上 URL 也會自動附加；Markdown 編輯器中的 GitHub 連結會顯示即時標題與狀態（實驗性）。

## 4. 在工作區中接續 quick chat

在 Agents 視窗開始的 quick chat 不需要一開始就綁定工作區。等討論變得跟專案有關時，再請 Copilot agent 掛上本機資料夾（或建立獨立的 worktree），這段對話就會轉成工作區工作階段，並保留標題、對話紀錄與目前的請求。目前只有 Copilot harness 支援。

## 5. Agent host 與訊息佇列

Agent host 讓多個 VS Code 視窗連到同一個 agent session，它以 Agent Host Protocol（AHP）為基礎，在專屬處理程序中執行 agent harness，其中的 Copilot agent 由 Copilot SDK 驅動。搭配新的 agent 訊息佇列機制：agent 用 `send_message` 聯絡忙碌中的 chat 時，訊息會排入佇列，等目前這一輪順利完成後再依送出順序處理。
