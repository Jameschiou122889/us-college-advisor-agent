---
created: 2026-01-16T04:49:40Z
last_updated: 2026-01-17T13:14:17Z
version: 1.1
author: Claude Code PM System
---

# Project Structure

## Current Directory Layout

```
us-college-advisor-agent/
├── .claude/
│   └── context/              # 專案 context 文件
│       ├── progress.md
│       ├── project-structure.md
│       ├── tech-context.md
│       ├── system-patterns.md
│       ├── product-context.md
│       ├── project-brief.md
│       ├── project-overview.md
│       ├── project-vision.md
│       └── project-style-guide.md
│
├── data/                     # 資料檔案
│   ├── schools.json          # 學校資料庫 (Top 50)
│   └── majors.json           # 科系清單
│
├── docs/                     # 文件
│   └── testing.md            # 測試計劃和 API 說明
│
├── n8n/                      # n8n Workflow 設定
│   └── workflows/            # Workflow JSON 匯出
│       ├── main-recommendation.json
│       └── booking.json
│
├── prompts/                  # Claude API Prompts
│   └── recommendation-prompt.md
│
└── public/                   # 前端靜態檔案
    └── index.html            # 學生資料表單
```

## Key Directories

| 目錄 | 用途 | 狀態 |
|------|------|------|
| `/` | 專案根目錄 | ✅ 已建立 |
| `/.claude/context/` | Context 文件 | ✅ 已完成 |
| `/data/` | 學校和科系資料 | ✅ 已完成 |
| `/n8n/workflows/` | Workflow JSON | ✅ 已完成 |
| `/prompts/` | Claude prompts | ✅ 已完成 |
| `/public/` | 前端表單 | ✅ 已建立 (2026-01-17) |
| `/docs/` | 測試文件 | ✅ 已完成 |

## File Naming Conventions

- **Markdown**: `kebab-case.md` (如 `project-structure.md`)
- **JSON 資料**: `lowercase.json` (如 `schools.json`)
- **Workflow**: `descriptive-name.json` (如 `main-flow.json`)

## Module Organization

此專案為 n8n + Claude API 架構，主要模組：

1. **資料層** - `/data/` 學校資料庫
2. **邏輯層** - `/n8n/` Workflow + `/prompts/` Claude prompts
3. **介面層** - 前端表單（形式待定）
4. **整合層** - CRM 預約系統連接
