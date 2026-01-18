---
created: 2026-01-16T04:49:40Z
last_updated: 2026-01-16T04:49:40Z
version: 1.0
author: Claude Code PM System
---

# Project Style Guide

## Naming Conventions

### Files and Directories
| 類型 | 慣例 | 範例 |
|------|------|------|
| 目錄 | kebab-case | `school-data/`, `n8n-workflows/` |
| Markdown | kebab-case | `project-brief.md`, `tech-context.md` |
| JSON 資料 | lowercase | `schools.json`, `majors.json` |
| Workflow | kebab-case | `main-recommendation.json` |

### Code and Variables
| 語言/環境 | 慣例 | 範例 |
|-----------|------|------|
| n8n 變數 | camelCase | `studentGpa`, `satScore` |
| JSON 欄位 | snake_case | `acceptance_rate`, `top_majors` |
| 環境變數 | UPPER_SNAKE | `ANTHROPIC_API_KEY` |

### Prompt Templates
| 元素 | 格式 | 範例 |
|------|------|------|
| 變數 | `{{variable}}` | `{{gpa}}`, `{{major}}` |
| 區塊 | Markdown heading | `## 學生資料` |
| 指令 | 清晰中文 | `請根據以上資料推薦學校` |

## Documentation Standards

### Markdown Files
```markdown
---
created: YYYY-MM-DDTHH:MM:SSZ
last_updated: YYYY-MM-DDTHH:MM:SSZ
version: X.X
author: Author Name
---

# Title

## Section 1

Content...
```

### Code Comments
```javascript
// 單行註解：簡短說明
/*
 * 多行註解：
 * 詳細說明複雜邏輯
 */
```

## Data Formatting

### School Data Schema
```json
{
  "id": "school-slug",
  "name": "English Name",
  "name_zh": "中文名稱",
  "ranking": 1,
  "acceptance_rate": 0.05,
  "sat_25th": 1400,
  "sat_75th": 1550,
  "tuition": 55000,
  "region": "West|East|Midwest|South",
  "state": "CA",
  "top_majors": ["CS", "Engineering"],
  "international_rate": 0.10
}
```

### User Input Schema
```json
{
  "gpa": 3.7,
  "major": "computer-science",
  "sat": 1450,
  "toefl": 105,
  "region_preference": ["West"],
  "budget": "50-70k"
}
```

### Recommendation Output
```json
{
  "reach": [
    {"school_id": "stanford", "probability": "low"}
  ],
  "match": [
    {"school_id": "ucsd", "probability": "medium"}
  ],
  "safety": [
    {"school_id": "asu", "probability": "high"}
  ]
}
```

## UI/UX Guidelines

### 語言
- 介面語言：繁體中文
- 學校名稱：英文 + 中文並列
- 術語：使用台灣常見說法（如「成績」而非「分數」）

### 色彩建議
| 用途 | 顏色 | 說明 |
|------|------|------|
| 衝刺校 | 紅/橘 | 高風險 |
| 適中校 | 藍/綠 | 平衡 |
| 安全校 | 綠 | 低風險 |
| CTA | 品牌主色 | 強調行動 |

### 表單設計
- 必填欄位標記星號 (*)
- 提供輸入提示和範例
- 錯誤訊息清楚明確
- 支援鍵盤導航

## Prompt Engineering Style

### Structure
```markdown
# 任務說明
[清楚說明 AI 要做什麼]

## 輸入資料
[列出學生資料]

## 學校資料庫
[提供學校清單]

## 輸出要求
[說明輸出格式]

## 範例
[提供 few-shot 範例]
```

### Tone
- 專業但友善
- 使用正面語言
- 避免絕對性表述（如「一定」「保證」）

## Version Control

### Commit Message Format
```
<type>: <description>

Types:
- feat: 新功能
- fix: 修復
- docs: 文件更新
- data: 資料更新
- refactor: 重構
```

### Branch Naming
```
feature/add-essay-helper
fix/school-data-update
data/add-top-50-schools
```

## Quality Checklist

### Before Commit
- [ ] 檔案命名符合規範
- [ ] JSON 格式正確
- [ ] Markdown frontmatter 完整
- [ ] 無敏感資訊外洩

### Before Release
- [ ] 所有功能測試通過
- [ ] 文件已更新
- [ ] 版本號已更新
