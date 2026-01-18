---
created: 2026-01-16T04:49:40Z
last_updated: 2026-01-16T14:17:20Z
version: 1.1
author: Claude Code PM System
---

# Project Overview

## Summary

**US College Advisor Agent** - AI 美國升學選校顧問

為台灣 14-18 歲學生及家長提供個人化美國大學推薦，並引導至真人顧問服務。

## Current State

| 項目 | 狀態 |
|------|------|
| PRD | ✅ 完成 |
| Context | ✅ 完成 |
| 資料庫 | ✅ Top 50 學校 |
| Workflows | ✅ 已部署 |
| 測試 | ⏳ 待執行 |

## Feature List

### MVP Features

| 功能 | 說明 | 狀態 |
|------|------|------|
| 學生資料表單 | GPA、SAT、科系等輸入 | ⏳ |
| AI 推薦引擎 | Claude API 選校邏輯 | ⏳ |
| 分層推薦 | Reach/Match/Safety | ⏳ |
| 結果展示 | 學校卡片和資訊 | ⏳ |
| 顧問 CTA | 預約轉化機制 | ⏳ |

### Future Features (Phase 2+)

| 功能 | 說明 |
|------|------|
| Essay 輔助 | AI 協助撰寫申請文書 |
| 時間軸規劃 | 個人化申請時程 |
| 學校資料更新 | 自動爬取最新資訊 |
| 用戶帳號 | 儲存歷史查詢 |
| 多語言 | 英文介面 |

## Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   前端表單   │────▶│   n8n Flow  │────▶│ Claude API  │
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                    ┌──────┴──────┐
                    ▼             ▼
             ┌──────────┐  ┌──────────┐
             │ 學校資料庫 │  │ 顧問 CRM │
             └──────────┘  └──────────┘
```

## Integration Points

| 整合 | 類型 | 狀態 |
|------|------|------|
| Claude API | AI 推理 | ✅ 已整合 |
| n8n | Workflow | ✅ 已部署 |
| 前端 | n8n Form | ✅ 內建 |
| CRM | Email 通知 | ✅ 已設定 |

## Tech Stack

- **Orchestration**: n8n
- **AI**: Claude API (claude-3-5-sonnet)
- **Data**: JSON (Phase 1) → Database (Phase 2)
- **Frontend**: TBD (可能嵌入現有網站)

## Key Metrics

| 指標 | 目標 |
|------|------|
| 表單完成率 | > 60% |
| 推薦產生時間 | < 10 秒 |
| 顧問預約率 | > 10% |
| 用戶滿意度 | > 4.0/5.0 |

## Risk Summary

| 風險 | 影響 | 緩解 |
|------|------|------|
| 資料準確性 | 推薦不準 | 顧問驗證 |
| API 費用 | 成本控制 | 優化 prompt |
| 轉化不佳 | ROI 不足 | A/B 測試 CTA |
