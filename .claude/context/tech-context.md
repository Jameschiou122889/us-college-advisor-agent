---
created: 2026-01-16T04:49:40Z
last_updated: 2026-01-20T12:18:01Z
version: 1.3
author: Claude Code PM System
---

# Tech Context

## ⚠️ 開發規範 (MUST READ)

### MCP 工具使用規範

| 場景 | 必須使用 | 說明 |
|------|----------|------|
| **Workflow 操作** | `n8n-mcp` | 所有 n8n workflow 的讀取、更新、測試、執行記錄查詢 |
| **程式碼/API 問題** | `context7` | 查詢最新文件和正確用法，避免使用過時方法 |

**n8n-mcp 常用工具：**
- `n8n_get_workflow` - 讀取 workflow 詳情
- `n8n_update_partial_workflow` - 增量更新 workflow
- `n8n_executions` - 查詢執行記錄和錯誤
- `n8n_test_workflow` - 測試 workflow

**Context7 使用時機：**
- 遇到 API 用法不確定時
- 需要查詢 library 最新文件時
- 程式碼報錯需要找正確解法時

### ⚠️ n8n 架構限制

| 限制 | 說明 | 解決方案 |
|------|------|----------|
| Form Trigger + Respond to Webhook | **不相容** | 改用 Webhook Trigger |
| `$env` 環境變數 | Cloud Run 不支援 | 資料內嵌於 Code Node |

## Technology Stack

### Core Technologies

| 技術 | 用途 | 版本/說明 |
|------|------|----------|
| n8n | Workflow 自動化 | GCP Cloud Run 部署, v2.33.2 |
| Claude API | AI 推薦引擎 | `claude-sonnet-4-20250514` |
| JSON | 學校資料庫 | 內嵌於 workflow (不用 $env) |

### n8n 部署環境

| 項目 | 值 |
|------|-----|
| Instance | `n8n-297809544822.us-west1.run.app` |
| 平台 | GCP Cloud Run |
| 版本 | v2.33.2 |
| Webhook API | `POST /webhook/college-advisor` |
| 限制 | ⚠️ 不支援 `$env`、Form Trigger 不支援 Respond to Webhook |

### Frontend

- ⚠️ ~~n8n Form Trigger~~ → **獨立 HTML 表單** (`public/index.html`)
- 響應式 HTML 結果頁 (由 Respond to Webhook 回傳)

### Integration

| 整合項目 | 用途 | 狀態 |
|----------|------|------|
| **Airtable** | 資料儲存 | ✅ 選校推薦 + 預約記錄 |
| **Gmail** | Email 發送 | ✅ 推薦報告發送 |
| **GitHub Pages** | 前端託管 | ✅ 已部署 |
| Email (SMTP) | 顧問通知 | ✅ 已設定 |
| 顧問 CRM | 預約管理 | Phase 2 |
| LINE Bot | 通知/互動 | Phase 2 |

### Airtable 設定

| 項目 | 值 |
|------|-----|
| Base ID | `appGgw7TZ0D2IcU2s` |
| 選校推薦紀錄 | `tbluHOpvcXxw72XaY` |
| 預約諮詢 | `tblWg5DVxWhVm1eo7` |

### Gmail 設定

| 項目 | 值 |
|------|-----|
| Credential ID | `DrLHnjdMLy71E29h` |
| 用途 | 發送選校推薦報告 |

## Development Tools

| 工具 | 用途 | 必要性 |
|------|------|--------|
| Claude Code | 開發輔助 | 必要 |
| n8n-mcp | Workflow 管理 | **必要** |
| Context7 | 文件查詢 | **必要** |
| Git | 版本控制 | 必要 |

## API Dependencies

### Claude API
```
Endpoint: https://api.anthropic.com/v1/messages
Model: claude-sonnet-4-20250514 (目前使用)
Rate Limit: 依方案
Headers: x-api-key, anthropic-version: 2023-06-01
```

### n8n Nodes (預計使用)
- HTTP Request (Claude API)
- Webhook (接收表單)
- Set / Code (資料處理)
- IF / Switch (邏輯分支)

## Data Schema

### School Data (`schools.json`)
```json
{
  "id": "stanford",
  "name": "Stanford University",
  "name_zh": "史丹佛大學",
  "ranking": 3,
  "acceptance_rate": 0.039,
  "sat_25th": 1500,
  "sat_75th": 1570,
  "tuition": 56169,
  "region": "West",
  "state": "CA",
  "top_majors": ["CS", "Engineering", "Biology"],
  "international_rate": 0.12
}
```

### User Input Schema
```json
{
  "email": "student@example.com",
  "gpa": 3.7,
  "major": "computer-science",
  "sat": 1450,
  "toefl": 105,
  "region": "West",
  "budget": "40k-55k",
  "activities": "Robotics club, Math competition"
}
```

### Phase 2 新增欄位 (規劃中)
```json
{
  "level": "undergraduate|graduate",
  "gpaScale": "4.0|4.3|5.0|100",
  "courses": "AP Calculus, IB Physics..."
}
```

## Environment Requirements

### n8n (GCP Cloud Run)
- ⚠️ 不支援 `$env` 存取環境變數
- ⚠️ 資料需內嵌於 Code Node
- Credentials 透過 n8n UI 設定

### 開發環境
- Claude Code CLI + MCP servers
- n8n-mcp (必要)
- Context7 (必要)
- Git

## Security Considerations

- API Key 透過 n8n Credentials 管理（不寫入程式碼）
- 用戶資料不永久儲存（除非預約）
- HTTPS 傳輸 (GCP Cloud Run 強制)
- 符合個資法要求
