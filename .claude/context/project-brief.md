---
created: 2026-01-16T04:49:40Z
last_updated: 2026-01-16T04:49:40Z
version: 1.0
author: Claude Code PM System
---

# Project Brief

## What It Does

**US College Advisor Agent** 是一個 AI 選校顧問，幫助台灣學生快速獲得美國大學推薦清單。

用戶輸入成績和偏好 → AI 分析 → 輸出分層推薦（衝刺/適中/安全校）→ 引導預約真人顧問

## Why It Exists

### 問題
台灣學生申請美國大學時面臨：
- 3000+ 學校，不知道怎麼選
- 資訊分散，查詢耗時
- 專業顧問費用高，門檻高

### 解決方案
- 免費 AI 工具提供即時選校建議
- 降低資訊不對稱
- 作為真人顧問的前端漏斗

### 商業價值
- 擴大顧問團隊服務規模
- 提升顧問轉化率
- 建立品牌認知

## Scope

### In Scope (MVP)
- 選校推薦功能
- 分層推薦（Reach/Match/Safety）
- 顧問預約 CTA
- 繁體中文介面

### Out of Scope (Phase 1)
- Essay 輔助
- 時間軸規劃
- 用戶帳號
- 多語言

## Success Criteria

| 標準 | 達成條件 |
|------|----------|
| 可用性 | 用戶可在 5 分鐘內完成操作 |
| 準確性 | 顧問驗證推薦準確率 > 80% |
| 轉化率 | 顧問預約率 > 10% |
| 規模 | 30 天內 100+ 預約 |

## Key Stakeholders

| 角色 | 責任 |
|------|------|
| 產品負責人 | 需求定義、優先級 |
| 顧問團隊 | 驗證準確度、處理預約 |
| 開發（Claude Code） | 技術實作 |
| 資料維護 | 更新學校資訊 |

## Timeline (Reference Only)

| 階段 | 內容 |
|------|------|
| Phase 1 | 建立學校資料庫 (Top 100) |
| Phase 2 | 開發 n8n Workflow |
| Phase 3 | 整合前端介面 |
| Phase 4 | 測試和優化 |
| Phase 5 | 上線和監控 |

## Constraints

- **技術**: 使用 n8n + Claude API
- **資料**: 初期手動建立 Top 100 學校
- **資源**: 有限開發時間
- **依賴**: Claude API 穩定性

## Assumptions

- 用戶願意提供真實成績
- GPA + SAT 是選校主要依據
- 免費工具可吸引目標用戶
- 顧問團隊可處理轉介預約
