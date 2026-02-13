# AI-Agent-Skill-Building-Experiment -  SGE QA Verification Skill

自動化 SGE（美男戰國英文版）遊戲活動的 QA 驗證流程實驗。

## 📖 項目結構

```
./
├── README.md                   # 本文檔
├── MISSING_INFO_ANALYSIS.md    # 缺失資訊分析
├── evals/
│   ├── evals.json              # 測試案例定義
│   └── files/                  # 測試用檔案（待添加）
├── sge-qa-verification/        # Agent SKILL 目錄
├──── SKILL.md                  # 主要 SKILL 定義（Claude 會讀取這個）
└──── docs/                     # 補充文檔（待添加）
    ├──── backend-navigation.md # 後台導航指南
    ├──── sheet-structure.md    # 驗證單結構說明
    └──── comparison-rules.md   # 資料比對規則
```

## 🚀 快速開始

### 前提條件

1. **訪問權限**
   - Notion 頁面訪問權限
   - Google Sheet 編輯權限
   - 後台管理系統帳號（ussgk/ussgkpass）
   - Redmine 訪問權限（可選）
   - Slack 頻道成員（可選）

2. **環境準備**
   - 確保 Claude 可以訪問 Windows-MCP 工具（瀏覽器操作）
   - 確保可以使用 Google Drive tools
   - 確保網路可以訪問所有相關系統

### 基本使用流程

1. **啟動驗證任務**
   ```
   你：請使用 sge-qa-verification skill 幫我驗證以下活動：
       驗證單 URL: [your-sheet-url]
       環境: テスト環境
       當前日期: 02/13
   ```

2. **Claude 會執行**
   - 讀取 Notion 背景資訊
   - 解析驗證單結構
   - 逐項進行驗證
   - 更新驗證結果
   - 生成報告

3. **查看結果**
   - Claude 會提供進度更新
   - 顯示通過/失敗的統計
   - 對於問題項目，會生成詳細報告

## 📋 目前狀態

### ✅ 已完成
- [x] 基本 SKILL 框架
- [x] 驗證流程定義
- [x] 通訊模板
- [x] 測試案例結構

### ⚠️ 尚待補充資訊
參見 [`TODO`](TODO.md)

## 🧪 測試 SKILL

### 運行測試案例

```bash
# 使用 skill-creator 運行所有測試
請使用 skill-creator 對 sge-qa-verification 執行 eval mode

# 或者單獨測試某個案例
請測試 sge-qa-verification 的 "Basic Verification Flow Test" 案例
```

### 現有測試案例

1. **Basic Verification Flow Test**: 測試基本的驗證流程識別
2. **Backend Navigation Test**: 測試後台系統導航說明
3. **Data Comparison Logic Test**: 測試資料比對邏輯
4. **Sheet Update Format Test**: 測試驗證單更新格式
5. **Error Reporting Template Test**: 測試錯誤報告生成

## 📚 相關資源

### 背景資訊
- [Notion 背景頁面](https://www.notion.so/SGE-2cba4cbac5dd80be9729ccbab819af99)
- [驗證教程 by Yuri](https://docs.google.com/spreadsheets/d/1IhDhYq01_YYfxgboKKHMjkJKJin_EsQN/edit?gid=1602671084)
- [後台比對參照表](https://docs.google.com/spreadsheets/d/1Kcdd362YLFmY8gAMyXpgurQSMXtUnD2SySEaRNmrpog)

### 系統連結
- [後台管理](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top)
- [Redmine 專案](https://redmine.ls.aws.cybird.ne.jp/projects/sgkus)
- [Slack 頻道](https://app.slack.com/client/T01GAGR304T/C0666R2V5BL)
- [驗證排程表](https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/)

## 🔧 故障排除

### 常見問題

**Q: Claude 無法訪問後台系統**
A: 確認：
- 網路連接正常
- 登入憑證正確
- Windows-MCP 工具可用

**Q: 驗證單解析失敗**
A: 檢查：
- Google Drive 權限設定
- Sheet 格式是否符合預期
- 黃色表頭是否清晰標記

**Q: 資料比對判斷不準確**
A: 這可能是因為：
- 比對規則尚未完全定義（參見 MISSING_INFO_ANALYSIS.md）
- 需要提供更多實際案例來完善規則

## 📈 未來改進方向

1. **自動化程度提升**
   - 完全自動化的驗證流程
   - 智能錯誤恢復
   - 批量處理多個活動

2. **整合深化**
   - 自動建立 Redmine issue
   - 自動發送 Slack 通知
   - 自動更新排程表

3. **智能化增強**
   - 學習常見錯誤模式
   - 預測潛在問題
   - 提供修復建議

## 🤝 貢獻指南

### 完善 SKILL 的方式

1. **提供實際案例**
   - 錄製真實驗證流程
   - 截圖關鍵步驟
   - 記錄遇到的問題

2. **補充文檔**
   - 後台頁面操作指南
   - 驗證單結構說明
   - 資料比對規則細節

3. **優化測試**
   - 增加測試案例
   - 提供測試數據
   - 回報測試結果

4. **回饋改進**
   - 報告 bug
   - 建議新功能
   - 分享最佳實踐

## 📞 獲取協助

如果在使用過程中遇到問題：

1. 查看 [`MISSING_INFO_ANALYSIS.md`](MISSING_INFO_ANALYSIS.md) 了解已知限制
2. 檢查測試案例是否涵蓋你的場景
3. 嘗試簡化任務，逐步排查問題
4. 記錄問題詳情以便改進 SKILL

## 📝 版本歷史

### v0.1.0 (2026-02-13)
- 初始版本
- 基本框架建立
- 核心流程定義
- 測試案例創建

---

**最後更新**: 2026-02-13
