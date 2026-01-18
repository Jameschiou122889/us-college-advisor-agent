---
name: us-college-advisor-agent
status: in-progress
created: 2026-01-16T05:05:47Z
updated: 2026-01-16T10:53:45Z
progress: 80%
prd: .claude/prds/us-college-advisor-agent.md
github: [Will be updated when synced to GitHub]
---

# Epic: US College Advisor Agent

## Overview

實作一個 AI 選校顧問系統，使用 n8n 作為 workflow 引擎，Claude API 作為推薦核心，提供台灣學生個人化的美國大學推薦，並引導至真人顧問預約。

## Architecture Decisions

| 決策 | 選擇 | 原因 |
|------|------|------|
| Workflow 引擎 | n8n | 現有資源，可視化流程，快速迭代 |
| AI 推薦 | Claude API | 強大推理能力，支援中文 |
| 資料儲存 | JSON 檔案 | Phase 1 簡化，易於維護 |
| 前端介面 | n8n Form Trigger | 最小開發成本，內建響應式 |
| CRM 整合 | Email 通知 | Phase 1 簡化，後續可升級 |

## Technical Approach

### 資料流
```
n8n Form → 驗證 → 載入學校資料 → Claude 推薦 → 格式化輸出 → 顯示結果
                                                        ↓
                                               預約表單 → Email 通知顧問
```

### 核心元件
1. **學校資料庫** - JSON 格式，Top 50 美國大學
2. **推薦 Prompt** - 結構化 prompt，輸出分層推薦
3. **n8n Workflow** - 主流程 + 預約流程
4. **結果模板** - HTML 格式化輸出

## Implementation Strategy

**簡化原則**：
- 使用 n8n 內建 Form Trigger 取代自建前端
- 使用 Email 通知取代 CRM API 整合
- 先建 Top 50 學校資料，驗證後再擴展

## Task Breakdown

| # | Task | 說明 | 優先級 |
|---|------|------|--------|
| 1 | 建立學校資料庫 | Top 50 美國大學 JSON 資料 | P0 |
| 2 | 設計推薦 Prompt | Claude API prompt template | P0 |
| 3 | 建立主 Workflow | n8n 表單 → 推薦 → 結果 | P0 |
| 4 | 建立預約 Workflow | 預約表單 → Email 通知 | P1 |
| 5 | 測試和優化 | 端對端測試，調整 prompt | P1 |

## Dependencies

### 外部
- Claude API key (已有)
- n8n instance (已有)
- Email 發送設定 (需確認)

### 開發工具
- **n8n-mcp** - Workflow 開發、驗證、部署
- **Context7** - API 文檔查詢

### 內部
- 顧問團隊 Email 接收預約
- 驗證推薦準確度

## Development Tools

### n8n MCP 使用指南
```bash
# 搜尋節點
mcp__n8n-mcp__search_nodes "form trigger"

# 取得節點配置
mcp__n8n-mcp__get_node "n8n-nodes-base.formTrigger"

# 驗證 workflow
mcp__n8n-mcp__validate_workflow <workflow_json>

# 部署到 n8n
mcp__n8n-mcp__n8n_create_workflow <workflow_json>

# 測試執行
mcp__n8n-mcp__n8n_test_workflow <workflow_id>
```

### Context7 使用指南
```bash
# 查詢 Claude API 文檔
mcp__context7__resolve-library-id "anthropic claude api"
mcp__context7__query-docs <library_id> "messages api"

# 查詢 n8n 文檔
mcp__context7__resolve-library-id "n8n"
mcp__context7__query-docs <library_id> "form trigger"
```

## Success Criteria (Technical)

| 指標 | 目標 |
|------|------|
| 推薦回應時間 | < 10 秒 |
| Workflow 成功率 | > 99% |
| 推薦格式正確率 | 100% |

## Estimated Effort

| Task | 估算 |
|------|------|
| 學校資料庫 | 2-3 小時 |
| 推薦 Prompt | 1-2 小時 |
| 主 Workflow | 2-3 小時 |
| 預約 Workflow | 1 小時 |
| 測試優化 | 2-3 小時 |
| **總計** | **8-12 小時** |

## Risk Mitigation

| 風險 | 緩解措施 |
|------|----------|
| Claude API 費用 | 優化 prompt 長度，使用 Haiku 初篩 |
| 推薦不準確 | 先小範圍測試，顧問驗證 |
| n8n 效能 | 限制併發數，考慮 queue |

## Tasks Created

- [x] 001.md - 建立學校資料庫 (parallel: true) ✅
- [x] 002.md - 設計推薦 Prompt (parallel: true) ✅
- [x] 003.md - 建立主 Workflow (parallel: false, depends: 001, 002) ✅
- [x] 004.md - 建立預約 Workflow (parallel: false, depends: 003) ✅
- [ ] 005.md - 測試和優化 (parallel: false, depends: 003, 004)

**Total tasks**: 5
**Parallel tasks**: 2 (001, 002 可同時進行)
**Sequential tasks**: 3
**Estimated total effort**: 8-12 小時
