# 美男 SGE 驗證

## 驗證流程
  1. 活動開始驗證時，要去日方驗證排程表 (Google Sheet) 對應活動欄位設定狀態
  2. 依照驗證單表格 (Google Sheet) 內各分頁與每列指定項目進行驗證
  3. 碰到後台資料與驗證單不一致的問題，於日方 Redmine 主活動下開單，並回報給 Yuri，Yuri 修正後回單，循環流程直至問題修正完成
  4. 驗證結束後，
     1. 需開「本番反映」需求單，一樣於日方 Redmine 主活動單下開單
     2. 通知 Yuri 推送QA服資料至正式服
     3. 更新排程表狀態，並於 Slack 通知已完成指定活動的驗證

## 相關連結與功能
- [Slack 頻道 (Slack Web)](https://app.slack.com/client/T01GAGR304T/C0666R2V5BL)
- [日方驗證排程表 (Google Sheet)](https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/edit?pli=1&gid=521618271#gid=521618271)
  用於告知 SGE 活動及其各任務的排程

- [Redmine 專案頁面 (Redmine)](https://redmine.ls.aws.cybird.ne.jp/projects/sgkus)
  用於紀錄驗證任務的執行狀態，對應活動將在該專案中獨立開設對應 Redmine 單

- [驗證單雲端路徑 (Google Sheet)](https://drive.google.com/drive/folders/1nbo6mzkR2ujjFuqceYQFk2umZyRBZ4Cl)
- 驗證教程
  [by Yuri (Google Sheet)](https://docs.google.com/spreadsheets/d/1IhDhYq01_YYfxgboKKHMjkJKJin_EsQN)
  [by Nina (Google Sheet)](https://docs.google.com/spreadsheets/d/1dvd7KvEob-w02pW75pnfH_9Aw3oliASWlHHMhifViZA)

- [美男戰國後台 (Website)](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top)
  > **後台登入資訊**
  > id : ussgk
  > pass : ussgkpass

  後台配置資料查詢頁面
  - 橫幅
    - [配置資料頁面](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/campaign)
  - 卡片 Card 或稱 Still
    - [配置資料頁面](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/still)
    - [發送頁面](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/user/still/top/6767)
  - 禮包
    - [配置資料頁面](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/combinedsale)
  - 用戶道具
    - [資料頁面 + 發送道具頁面](https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/user/user_item_list/)

---

## 驗證單 Google Sheet 處理備忘

後台比對參照表資訊參考：[Link](https://docs.google.com/spreadsheets/d/1Kcdd362YLFmY8gAMyXpgurQSMXtUnD2SySEaRNmrpog) 

1. 格式化填色儲存格時，使用 `Trim()` 移除字串前後空白，避免日方資料不規範
2. 拆解後台配置資料表時，使用 `VALUE()` 處理 ID 類、數量類純數字資料，避免因數據類型錯誤導致 Excel 視為不同值
3. 填寫驗證紀錄時
   若有公式自動比對欄位 “Result”，直接使用 `ARRAYFORMULA()`填寫值 + 對非 “OK” 行自動填色

- 若比對值相等，則填入 {OK, IW, Bill} 值
- 否則，保持為空白並填入對應行 “備忘” 欄位文字

4. 後台 Bonus 表貼入 Google Sheet 進一步處理備忘
   根據後台活動一覽表樣式特徵進行處理，便於 Sheet 中進行匹配
   - 集合所有禮物數據至相同欄位的公式示例
     ```
     =ARRAYFORMULA(LET(
     	_present, B5:B,
     	_special_story, I5:I,
     	_voice, L5:L,
     	_still, M5:M,
     	IFS(
     		LEN(_present), _present,
     		LEN(_special_story), _special_story,
     		LEN(_voice), _voice,
     		LEN(_still), _still
     		)
     ))
     ```
   - 集合後拆分各資訊至獨立欄位的公式示例

     ```
     =ARRAYFORMULA(IF(ISTEXT(R5:R),SPLIT(R5:R,"▸〈〉"),))
     ```


- 活動備忘資訊模板

  | 後台主地址 | https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top |
  | Redmine | https://redmine.ls.aws.cybird.ne.jp/issues/{Redmine 單ID }|
  | Slack 對應活動討論串| https://isweetygroup.slack.com/archives/C0666R2V5BL/{討論串ID} |
  | 活動 Google Sheet 驗證單 | https://docs.google.com/spreadsheets/d/{驗證單 ID}|

---

## 各類通知模板文字

- 驗證回報

  ```
  **「{活動名稱}」驗證回報**

  ★分頁【{Google Sheet 分頁名稱}】
  1. 第  列 (No )：{驗證問題說明}
  https://docs.google.com/spreadsheets/d/
  ```

  - 用例
    ```
    「あやかし恋遊戯」測試環境後台驗證回報
    測試環境後台已驗證完畢，昨日日方更新修正項目也已經確認~ 目前還有以下問題再麻煩確認

    ★分頁【特典】
    1. 第 93 ~ 114 列 (No 91~112)：
    https://docs.google.com/spreadsheets/d/1vH1S61DedrOWkWLxSuQJA8qcWXHs_uk1b8dkfXolObM/edit?gid=254067378#gid=254067378&range=93:114

    ★分頁【コレクション特典】
    2. 第 328 ~ 386 列 (No 326~388)：驗證單資料 (故事名稱) 缺少 Bonus
    https://docs.google.com/spreadsheets/d/1vH1S61DedrOWkWLxSuQJA8qcWXHs_uk1b8dkfXolObM/edit?gid=664278455#gid=664278455&range=328:386

    ★分頁【セット販売】
    3. 第 23 列 (No 21)：後台資料缺少 "Special obi and Kimono included!"
    https://docs.google.com/spreadsheets/d/1vH1S61DedrOWkWLxSuQJA8qcWXHs_uk1b8dkfXolObM/edit?gid=612592853#gid=612592853&range=23:23
    ```

- Redmine「本番反映」需求單
  用於測試環境驗證完成且無誤後，告知 Yuri / 日方將測試環境資料推送至正式環境
  ```
  **【本番化依頼】活動名稱**

  テスト環境での確認が完了いたしました。
  お手数をお掛けしますが、本番反映のご対応をよろしくお願いいたします。

  後台配置資料已於測試環境驗證完畢，資料與驗證單相符，未驗證出異常。請更新資料至生產環境~
  https://docs.google.com/spreadsheets/d/
  ```
  - 用例
    ```
    後台配置資料已於測試環境驗證完畢，資料與驗證單相符，未驗證出異常。請更新資料至生產環境~
    https://docs.google.com/spreadsheets/d/1vH1S61DedrOWkWLxSuQJA8qcWXHs_uk1b8dkfXolObM
    ```

- 正式環境驗證完畢 + Slack 通知

  ```
  本次活動後台配置資料、測試環境、生產環境已按單驗證完成，且無資料異常~
  以下文件狀態也已更新，再麻煩確認~ 感謝！

  ★ 驗證排程表
  https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/

  ★ 驗證單
  https://docs.google.com/spreadsheets/d/

  ★ Redmine
  https://redmine.ls.aws.cybird.ne.jp/issues/

  ★ Slack 已回報
  ```
  - 用例
    本次活動後台配置資料、測試環境、生產環境已按單驗證完成，且無資料異常~
    以下文件狀態也已更新，再麻煩確認~ 感謝！(moon big smile)

    ★ 驗證排程表
    https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/edit?pli=1&gid=521618271#gid=521618271&range=H522

    ★ 驗證單
    https://docs.google.com/spreadsheets/d/1vH1S61DedrOWkWLxSuQJA8qcWXHs_uk1b8dkfXolObM

    ★ Redmine
    https://redmine.ls.aws.cybird.ne.jp/issues/315661
---