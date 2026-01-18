# 測試計劃

## 線上連結

| 項目 | URL |
|------|-----|
| **主 Webhook API** | https://n8n-297809544822.us-west1.run.app/webhook/college-advisor |
| **前端表單** | 需自行部署 `public/index.html` |
| **預約表單** | https://n8n-297809544822.us-west1.run.app/form/h1E1NYNAun7tpKMP |
| **主 Workflow** | https://n8n-297809544822.us-west1.run.app/workflow/dQqoiVpqqUEsMYiD |
| **預約 Workflow** | https://n8n-297809544822.us-west1.run.app/workflow/h1E1NYNAun7tpKMP |

### Webhook API 使用方式

```bash
curl -X POST "https://n8n-297809544822.us-west1.run.app/webhook/college-advisor" \
  -H "Content-Type: application/json" \
  -d '{
    "gpa": 3.8,
    "major": "computer-science",
    "sat": 1450,
    "toefl": 105,
    "region": "West",
    "budget": "any",
    "activities": "程式競賽獲獎"
  }'
```

---

## 部署前置作業

### 1. 匯入 Workflows

1. **主推薦 Workflow** (`n8n/workflows/main-recommendation.json`)
   - 匯入到 n8n
   - 設定 Claude API Credential (HTTP Header Auth)
     - Header Name: `x-api-key`
     - Header Value: 你的 Anthropic API Key
   - 記錄 Webhook URL（用於預約表單連結）

2. **預約 Workflow** (`n8n/workflows/booking.json`)
   - 匯入到 n8n
   - 設定 SMTP Credential
   - 更新 `fromEmail` 和 `toEmail`
   - 記錄 Webhook URL

### 2. 互相連結

- 在主 Workflow 中將 `BOOKING_URL_HERE` 替換為預約表單 URL
- 在預約 Workflow 中將 `MAIN_FORM_URL_HERE` 替換為主表單 URL

---

## 測試案例

### Case 1: 高分學生 (Reach 驗證)
| 欄位 | 值 |
|------|-----|
| GPA | 3.9 |
| 目標科系 | 電腦科學 |
| SAT | 1550 |
| TOEFL | 110 |
| 地區偏好 | 不限 |
| 預算 | 不限 |

**預期結果**:
- Reach 包含 MIT, Stanford, CMU
- Match 包含 UC Berkeley, UCLA
- 回應時間 < 10 秒

---

### Case 2: 中等學生 (Match 驗證)
| 欄位 | 值 |
|------|-----|
| GPA | 3.5 |
| 目標科系 | 企業管理 |
| SAT | 1300 |
| TOEFL | 100 |
| 地區偏好 | 東岸 |
| 預算 | $55,000 - $65,000 |

**預期結果**:
- Reach 應較少或錄取率提示較低
- Match 包含多所合適學校
- 地區限制正確套用

---

### Case 3: 無 SAT 學生
| 欄位 | 值 |
|------|-----|
| GPA | 3.7 |
| 目標科系 | 心理學 |
| SAT | （留空）|
| TOEFL | 105 |

**預期結果**:
- 推薦仍正常運作
- 評估主要依據 GPA
- 可能建議考 SAT 提升競爭力

---

### Case 4: 低預算學生
| 欄位 | 值 |
|------|-----|
| GPA | 3.6 |
| 目標科系 | 工程 |
| 預算 | $40,000 以下 |

**預期結果**:
- 推薦學校學費符合預算
- 可能包含公立學校
- 建議中提到獎學金資訊

---

### Case 5: 特定地區 (加州)
| 欄位 | 值 |
|------|-----|
| GPA | 3.4 |
| 目標科系 | 資料科學 |
| 地區偏好 | 西岸 |

**預期結果**:
- 主要推薦加州學校
- UC 系列應出現在列表中

---

### Case 6: 冷門科系
| 欄位 | 值 |
|------|-----|
| GPA | 3.6 |
| 目標科系 | 音樂 |

**預期結果**:
- 推薦音樂相關強校
- 可能包含 Indiana, Juilliard 等
- 建議中提到作品集準備

---

### Case 7: 邊界條件
| 欄位 | 值 |
|------|-----|
| GPA | 2.5 |
| 目標科系 | 電腦科學 |
| SAT | 1000 |

**預期結果**:
- Safety 學校為主
- 誠實評估競爭力
- 建議提升策略

---

### Case 8: 錯誤輸入
| 測試項目 | 輸入 | 預期結果 |
|----------|------|----------|
| 無效 GPA | 5.0 | 顯示錯誤訊息 |
| 負數 GPA | -1 | 顯示錯誤訊息 |
| 空白必填欄位 | （不填 GPA）| 顯示驗證錯誤 |

---

### Case 9: 預約流程
1. 點擊結果頁的「預約免費諮詢」按鈕
2. 填寫預約表單
3. 驗證：
   - 必填欄位驗證正常
   - Email 格式驗證正常
   - 提交後顯示確認頁
   - 顧問收到 Email 通知

---

### Case 10: 手機版測試
- 使用手機瀏覽器開啟表單
- 驗證：
  - 表單正常顯示
  - 可正常輸入和選擇
  - 結果頁正常顯示
  - 按鈕可點擊

---

## 效能測試

### 回應時間測量
- 目標: < 10 秒（從提交到顯示結果）
- 測量方式: 瀏覽器開發者工具 Network tab

### 如果超時:
1. 檢查 Claude API 回應時間
2. 考慮使用 claude-3-haiku 初篩
3. 減少傳送的學校資料量

---

## 測試結果記錄

| 案例 | 結果 | 回應時間 | 備註 |
|------|------|----------|------|
| Case 1 | ✅ | 19.7s | 推薦合理，建議個人化 |
| Case 2 | ⬜ | | |
| Case 3 | ⬜ | | |
| Case 4 | ⬜ | | |
| Case 5 | ⬜ | | |
| Case 6 | ⬜ | | |
| Case 7 | ⬜ | | |
| Case 8 | ⬜ | | |
| Case 9 | ⬜ | | |
| Case 10 | ⬜ | | |

**圖例**: ✅ 通過 | ❌ 失敗 | ⬜ 未測試

### 測試詳情

**Case 1 (2026-01-16 16:43)**
- 輸入: GPA 4.0, SAT 1250, Engineering, 創業經歷
- Reach: Berkeley, Georgia Tech, UCLA (3所)
- Match: UCSD, UIUC, Wisconsin, UC Davis (4所)
- Safety: Ohio State (1所)
- 建議: SAT提升、Essay撰寫、作品集準備
- 執行 ID: 176895

---

## 優化建議追蹤

| 問題 | 建議修改 | 狀態 |
|------|----------|------|
| 回應時間 ~20s | 考慮使用 claude-3-haiku 或減少學校資料 | 待處理 |
| Safety 只有 1 所 | 調整 prompt 確保 3-5 所 | 待處理 |

---

## 上線檢查清單

- [ ] 所有測試案例通過
- [ ] 顧問確認推薦邏輯合理
- [ ] 效能達標 (< 10 秒)
- [ ] 手機版正常顯示
- [ ] Email 通知正常送達
- [ ] 錯誤處理完善
- [ ] 替換所有 placeholder URL
- [ ] 更新 Workflow 為 Active 狀態
