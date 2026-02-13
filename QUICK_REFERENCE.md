# SGE QA 驗證 - 快速參考指南

## 🎯 使用前檢查清單

### 必備資訊
- [ ] 驗證單 Google Sheet URL
- [ ] 要驗證的環境（テスト環境 / 本番環境）
- [ ] 當前日期（MM/DD 格式）
- [ ] 後台登入憑證可用

### 可選資訊
- [ ] Redmine issue ID（如需回報問題）
- [ ] Slack 討論串 ID（如需通知）

## 💬 常用指令範例

### 開始驗證
```
請使用 sge-qa-verification skill 驗證以下活動：

驗證單: https://docs.google.com/spreadsheets/d/[ID]
環境: テスト環境
日期: 02/13
```

### 僅檢查特定分頁
```
請只驗證「特典」分頁的內容
```

### 產生問題報告
```
請根據驗證結果生成一份問題回報，準備發送給 Yuri
```

### 產生本番反映申請
```
測試環境驗證已完成且無誤，請生成本番反映的 Redmine issue 內容
```

## 📊 驗證結果判讀

### 成功案例
```
✓ Verified 15/50 items in 特典 tab
  - 15 OK
  - 0 issues
```
→ 所有項目通過，可以繼續

### 有問題案例
```
✓ Verified 15/50 items in 特典 tab
  - 13 OK
  - 2 issues found
  
Issues:
  Row 23: Missing bonus description
  Row 45: Quantity mismatch (Expected: 100, Actual: 200)
```
→ 需要回報問題並等待修正

## 🎨 驗證單格式速查

| 欄位 | 內容 | 格式 |
|------|------|------|
| 確認 | OK | 普通文字 |
| 確認 | 錯誤說明 | 紅底 + 錯誤描述文字 |
| 担当 | IW | 普通文字 |
| 担当者 | Agent | 普通文字 |
| 日付 | 02/13 | MM/DD 格式 |

## 🔍 後台快速導航

| 資料類型 | URL 路徑 | 搜尋方式 |
|---------|---------|---------|
| Banner | `/admin/campaign` | 活動名稱搜尋 |
| Card/Still | `/admin/still` | ID 搜尋 |
| Card 發送 | `/admin/user/still/top/6767` | User ID 查詢 |
| Package Sale | `/admin/combinedsale` | 活動篩選 |
| User Items | `/admin/user/user_item_list/` | User ID 查詢 |

## 📝 通訊模板速查

### 問題回報（簡短版）
```
「{活動名}」驗證回報

★分頁【{分頁名}】
第 {列號} 列：{問題說明}
{Sheet URL with range}
```

### 本番反映申請（簡短版）
```
【本番化依頼】{活動名}

テスト環境での確認が完了いたしました。
本番反映のご対応をよろしくお願いいたします。

{Sheet URL}
```

### 完成通知（簡短版）
```
本次活動已驗證完成，無資料異常

★ 驗證單：{URL}
★ Redmine：{URL}
```

## ⚠️ 常見陷阱

### 1. 環境混淆
❌ 在測試環境欄位填寫正式環境的結果
✅ 確認當前驗證的環境，填寫對應欄位

### 2. 日期格式錯誤
❌ 2026/02/13 或 2/13/2026
✅ 02/13 (MM/DD)

### 3. 比對過於嚴格
❌ 「2024/02/01」 vs 「2024/02/01 」視為不同
✅ 使用 TRIM() 移除空白後比對

### 4. 跳過灰底儲存格
❌ 對灰底儲存格也進行驗證
✅ 灰底表示無需驗證，直接跳過

## 🚨 緊急情況處理

### 後台無法訪問
1. 檢查網路連接
2. 確認登入憑證
3. 暫停驗證，通知相關人員

### 驗證單結構異常
1. 截圖保存當前狀態
2. 暫停操作
3. 向用戶確認是否為預期結構

### 大量項目失敗
1. 檢查是否為系統性問題（如環境錯誤）
2. 回報前 3-5 個典型問題
3. 等待修正後重新驗證

## 🔗 重要連結（快速複製）

```
Notion: https://www.notion.so/SGE-2cba4cbac5dd80be9729ccbab819af99
後台: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top
Redmine: https://redmine.ls.aws.cybird.ne.jp/projects/sgkus
Slack: https://app.slack.com/client/T01GAGR304T/C0666R2V5BL
排程表: https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/
```

## 💡 效率提升技巧

1. **批次處理同類項目**
   - 先完成所有 Banner 驗證
   - 再處理所有 Card 驗證
   - 減少系統切換次數

2. **使用快捷方式**
   - 書籤常用的後台頁面
   - 預先開啟多個分頁
   - 使用鍵盤快捷鍵

3. **記錄常見模式**
   - 建立個人的驗證筆記
   - 記錄特殊案例的處理方式
   - 累積經驗資料庫

## 📞 需要幫助時

1. 參考 `MISSING_INFO_ANALYSIS.md` 了解已知限制
2. 查看 `README.md` 獲取詳細文檔
3. 檢查測試案例中是否有類似場景
4. 向維護者或團隊成員諮詢

---
