---
name: sge-qa-verification
description: Automate QA verification process for SGE (Ikemen Sengoku US version) game events. Use when users need to verify event configurations against verification sheets, compare backend data with expected values, and update verification status in Google Sheets. Handles test and production environment verification workflows.
---

# SGE QA Verification Skill

This skill automates the Quality Assurance (QA) verification process for SGE (美男戰國 US/English version) game events. It systematically verifies that backend configurations match the verification sheet requirements.

## Core Responsibilities

1. **Read and parse verification sheets** from Google Sheets
2. **Access backend admin system** to retrieve actual configuration data
3. **Compare verification items** with backend data
4. **Update verification sheets** with results (OK/Issues)
5. **Handle multi-environment verification** (Test/Production)
6. **Generate reports and notifications** for stakeholders

## Prerequisites

Before starting any verification task, Claude must:

1. **Fetch background information** from:
   - Notion page: https://www.notion.so/SGE-2cba4cbac5dd80be9729ccbab819af99
   - Reference Google Sheet: https://docs.google.com/spreadsheets/d/1IhDhYq01_YYfxgboKKHMjkJKJin_EsQN/edit?gid=1602671084#gid=1602671084

2. **Obtain from user**:
   - Verification sheet name and Google Sheet URL
   - Which environment to verify (テスト環境 = Test / 本番環境 = Production)
   - Current date for filling in verification records

3. **Access credentials** (stored in Notion):
   - Backend admin URL: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top
   - Login: ussgk / ussgkpass
   - Redmine project: https://redmine.ls.aws.cybird.ne.jp/projects/sgkus

## Verification Process Flow

According to the "驗證具體流程" document, follow these steps for each verification item:

### Step 1: Understand Verification Requirements

For each row with **yellow header cells** in the verification sheet:

1. **確認方法 (Verification Method)** column: 
   - Indicates where in the backend to find the data
   - For Japanese backend info, cross-reference with the blue-background column with the same name

2. **確認項目 (Verification Item)** column:
   - Specific content that needs to be verified

### Step 2: Execute Verification

1. Navigate to the backend location specified in "確認方法"
2. Extract the actual data from the backend
3. Compare with expected values in the verification sheet

### Step 3: Record Results

Fill in the appropriate environment section (テスト環境 or 本番環境):

1. **確認 (Confirmation)** column:
   - White background cells = Items pending verification
   - Gray background cells = No verification needed
   - If verified and correct: Enter "OK"
   - If verified but incorrect: Fill cell with red background and enter explanation of discrepancy

2. **担当 (Team)** column:
   - After verification completion: Enter "IW"

3. **担当者 (Person in Charge)** column:
   - After verification completion: Enter "Agent"

4. **日付 (Date)** column:
   - After verification completion: Enter today's date in MM/DD format

## Backend System Navigation

### Main Admin Pages

- **Homepage**: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top
- **Banners (Campaign)**: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/campaign
- **Cards/Stills**: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/still
- **Card Distribution**: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/user/still/top/6767
- **Package Sales**: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/combinedsale
- **User Items**: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/user/user_item_list/

### Data Extraction Tips

1. Use `Trim()` to remove leading/trailing whitespace from backend data
2. Use `VALUE()` for numeric data (IDs, quantities) to ensure proper type matching
3. Backend data comparison reference: https://docs.google.com/spreadsheets/d/1Kcdd362YLFmY8gAMyXpgurQSMXtUnD2SySEaRNmrpog

## Google Sheet Operations

### Reading Verification Sheets

1. Use `google_drive_fetch` to retrieve the verification sheet content
2. Identify sheets/tabs to verify (usually multiple tabs per event)
3. Parse yellow-header rows to understand verification structure
4. Note which cells are white (pending) vs gray (skip)

### Updating Verification Results

1. Prepare batch updates for all verified items
2. Apply conditional formatting:
   - Red background for cells with discrepancies
   - Regular formatting for "OK" cells
3. Fill in IW, Agent, and date in respective columns
4. Use `ARRAYFORMULA()` where possible for auto-computed comparisons

### Formula Patterns for Auto-Comparison

If the sheet has auto-comparison formulas:

```
=ARRAYFORMULA(IF(A:A=B:B, "OK", ""))
```

Then auto-fill colors for non-OK rows.

## Error Handling and Issue Reporting

### When Discrepancies Are Found

1. **Document the issue clearly**:
   - Sheet tab name
   - Row number(s)
   - Item description
   - Expected vs Actual values
   - Screenshot or data excerpt if helpful

2. **Report to stakeholders**:
   - Use the verification report template (see Communication Templates)
   - Include Google Sheet link with range parameter
   - Post in appropriate Slack channel

3. **Create Redmine ticket** if needed:
   - Under main event Redmine issue
   - Follow up with Yuri for fixes

4. **Pause and inform user**:
   - If repeated errors or unexpected results occur
   - Request user assistance to troubleshoot

## Communication Templates

### Verification Report (Issues Found)

```
**「{活動名稱}」驗證回報**

★分頁【{Google Sheet 分頁名稱}】
1. 第 {列號} 列 (No {項目編號})：{驗證問題說明}
https://docs.google.com/spreadsheets/d/{sheet_id}/edit?gid={gid}#gid={gid}&range={row_range}
```

### Production Deployment Request (本番反映)

After test environment verification is complete with no issues:

```
**【本番化依頼】{活動名稱}**

テスト環境での確認が完了いたしました。
お手数をお掛けしますが、本番反映のご対応をよろしくお願いいたします。

後台配置資料已於測試環境驗證完畢，資料與驗證單相符，未驗證出異常。請更新資料至生產環境~
https://docs.google.com/spreadsheets/d/{sheet_id}
```

Create this as a new issue under the main event Redmine ticket.

### Completion Notification

After production environment verification:

```
本次活動後台配置資料、測試環境、生產環境已按單驗證完成，且無資料異常~
以下文件狀態也已更新，再麻煩確認~ 感謝！

★ 驗證排程表
https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/

★ 驗證單
https://docs.google.com/spreadsheets/d/{sheet_id}

★ Redmine
https://redmine.ls.aws.cybird.ne.jp/issues/{issue_id}

★ Slack 已回報
```

Post this in the Slack channel: https://app.slack.com/client/T01GAGR304T/C0666R2V5BL

## Workflow Execution Strategy

### Initialization Phase

1. Confirm task parameters with user:
   - Verification sheet URL
   - Environment (test/production)
   - Current date
   - Any specific tabs to focus on

2. Fetch and parse verification sheet structure

3. Open Chrome browser and navigate to backend admin

### Verification Loop

For each verification item (row):

1. **Read**: Parse item requirements from sheet
2. **Navigate**: Go to specified backend page
3. **Extract**: Retrieve actual data from backend
4. **Compare**: Check if actual matches expected
5. **Record**: Update sheet with result
6. **Continue**: Move to next item

### Completion Phase

1. Review all verification results
2. Generate summary of:
   - Total items checked
   - Items passed (OK)
   - Items with issues
3. Prepare appropriate notification (report or completion)
4. Ask user to confirm before sending notifications

## Special Considerations

### Multi-Tab Verification

Events typically have multiple verification tabs:
- 特典 (Bonuses)
- コレクション特典 (Collection Bonuses)
- セット販売 (Package Sales)
- バナー (Banners)
- etc.

Verify each tab systematically and keep progress tracking.

### Data Type Handling

- **IDs**: Pure numbers, use VALUE() for comparison
- **Names**: Text strings, use TRIM() to normalize
- **Dates**: Format as specified in backend
- **Quantities**: Numeric values
- **Formulas**: Respect existing sheet formulas

### Japanese Text Handling

Backend is in Japanese. The verification sheet may have:
- Japanese column headers (blue background for guidance)
- Japanese data values
- Mixed language content

Handle all text encodings properly and preserve original formatting.

## Dependencies

This skill requires:

1. **Browser access** (Windows-MCP tools or equivalent)
2. **Google Drive API access** (for reading/writing sheets)
3. **Backend admin access** (credentials in Notion)
4. **Date/time utilities** (for filling in verification dates)

## Safety and Validation

### Before Each Operation

- Confirm you're working in the correct environment (test/prod)
- Verify you're updating the correct sheet and tab
- Double-check data comparisons before marking as verified

### Error Recovery

- If backend access fails repeatedly, notify user
- If sheet structure is unexpected, pause and clarify
- If comparison is ambiguous, ask user for confirmation

### Data Integrity

- Never overwrite existing verification data without confirmation
- Preserve all existing formulas and formatting
- Create backups before bulk operations (if possible)

## Output Format

### During Verification

Provide periodic progress updates:
```
✓ Verified 15/50 items in 特典 tab
  - 14 OK
  - 1 issue found (Row 23: Missing bonus description)
```

### Final Report

```
Verification Complete for {Event Name} - {Environment}

Summary:
- Total Items: 150
- Passed: 148
- Issues: 2

Issues Found:
1. [特典 Tab, Row 23] Missing bonus description in backend
2. [バナー Tab, Row 45] Banner display period mismatch

Next Steps:
- {If issues found} Created Redmine ticket #12345
- {If test env complete} Ready for production deployment request
- {If prod env complete} Ready to notify completion in Slack
```

## Continuous Improvement

As you execute verifications:
- Learn common data patterns
- Identify frequently occurring issues
- Optimize navigation sequences
- Suggest improvements to verification process

## Notes for Skill Execution

1. **Always verify you have latest credentials** from Notion before starting
2. **Take screenshots** when data discrepancies are unclear
3. **Maintain audit trail** of what was verified and when
4. **Coordinate with human** on ambiguous cases
5. **Respect rate limits** when accessing backend systems
6. **Handle Japanese text** with proper encoding throughout
7. **Test incremental changes** before bulk updates

---

## Quick Reference

**Key URLs:**
- Notion Background: https://www.notion.so/SGE-2cba4cbac5dd80be9729ccbab819af99
- Backend Admin: https://common-legacy-app-admin.us.ikemen-sengoku.jp/admin/top
- Redmine Project: https://redmine.ls.aws.cybird.ne.jp/projects/sgkus
- Slack Channel: https://app.slack.com/client/T01GAGR304T/C0666R2V5BL
- Schedule Sheet: https://docs.google.com/spreadsheets/d/1H9P7AzDQV6fMTEUwOWbb6o6zYKYVOPUD8OWUhU595hk/

**Login Credentials:**
- Backend ID: ussgk
- Backend Password: ussgkpass

**Fill Values:**
- Team (担当): IW
- Person (担当者): Agent
- Date Format: MM/DD

**Status Values:**
- Success: OK
- Failure: Red background + explanation text
