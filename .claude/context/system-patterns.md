---
created: 2026-01-16T04:49:40Z
last_updated: 2026-01-16T04:49:40Z
version: 1.0
author: Claude Code PM System
---

# System Patterns

## Architectural Style

**Event-Driven Workflow Architecture**

```
用戶輸入 → Webhook 觸發 → n8n Workflow → Claude API → 結果回傳
```

此專案採用 n8n 作為 orchestration layer，Claude API 作為 AI 推理核心。

## Design Patterns

### 1. Pipeline Pattern
資料處理流程如管線般依序執行：

```
Input Validation → Data Enrichment → AI Processing → Result Formatting → Response
```

### 2. Strategy Pattern (推薦邏輯)
根據不同條件使用不同的推薦策略：

```
if (has_sat_score):
    use detailed_recommendation()
else:
    use estimated_recommendation()
```

### 3. Template Pattern (Prompt)
使用可複用的 prompt template：

```markdown
# 選校推薦 Prompt Template

## 學生資料
- GPA: {{gpa}}
- SAT: {{sat}}
- 目標科系: {{major}}

## 任務
根據以上資料，從學校列表中推薦適合的學校...
```

## Data Flow

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  表單輸入  │───▶│  資料驗證  │───▶│  AI 推薦  │───▶│  結果展示  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
                     │                │
                     ▼                ▼
              ┌──────────┐    ┌──────────┐
              │  學校資料  │    │   CRM    │
              └──────────┘    └──────────┘
```

## Workflow Structure

### Main Flow
```
1. Webhook Trigger (接收表單)
2. Validate Input (驗證必填欄位)
3. Load School Data (載入學校資料)
4. Build Prompt (組合 AI prompt)
5. Call Claude API (執行推薦)
6. Parse Response (解析結果)
7. Format Output (格式化輸出)
8. Return Response (回傳結果)
```

### Error Handling Pattern
```
try:
    execute_step()
except ValidationError:
    return user_friendly_error()
except APIError:
    log_error()
    return fallback_response()
```

## Integration Patterns

### Webhook Integration
- 前端表單 POST 到 n8n webhook URL
- n8n 處理後直接回傳 JSON 結果

### CRM Integration (Phase 1)
- 簡單方式：Email 通知顧問
- 進階方式：API 寫入 CRM 系統

## Prompt Engineering Patterns

### Few-Shot Example
```markdown
範例 1:
學生: GPA 3.5, SAT 1400, CS
推薦: [Match] UCSD, UCI, UW...

範例 2:
學生: GPA 3.9, SAT 1550, CS
推薦: [Reach] MIT, Stanford... [Match] CMU, Berkeley...
```

### Chain of Thought
```markdown
請按以下步驟分析：
1. 評估學生整體競爭力
2. 根據 GPA 和 SAT 篩選學校
3. 考慮科系競爭度調整
4. 分類為 Reach/Match/Safety
5. 輸出推薦清單
```

## Naming Conventions

| 類型 | 慣例 | 範例 |
|------|------|------|
| Workflow | kebab-case | `main-recommendation-flow` |
| Variable | camelCase | `studentGpa`, `satScore` |
| Prompt | Title Case | `School Recommendation Prompt` |
| Data Key | snake_case | `acceptance_rate`, `top_majors` |
