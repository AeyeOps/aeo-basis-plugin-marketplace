# QBO Multi-Year Historical Reconstruction — Knowledge Base

Research compiled 2026-02-24. Focus: multi-year historical bookkeeping reconstruction in QuickBooks Online, delinquent/catch-up scenarios, opening balances, COA evolution, and retained earnings behavior.

---

## 1. Strategic Approach: Choosing a Method

### Option A — Annual Trial Balance Journal Entries (Recommended for historical catch-up)

Post one summary JE per fiscal year, dated the last day of that year, containing net amounts per account derived from a trial balance. For the current (or most recent) year, break into quarters or months. This is the standard CPA approach for closed prior periods where individual transaction detail is not needed.

- Request monthly or annual trial balance reports (debit/credit by account) from any prior system.
- Create one JE per month (or per year for older periods) capturing net amounts per account.
- No bank reconciliation is needed for summary JE imports — the entries go in as final, reconciled amounts.
- This yields trend-capable historical P&L and balance sheet reports without requiring transaction-level detail.

**Source:** [I need to build 3 years of history into a new quickbooks implementation](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-need-to-build-3-years-of-history-into-a-new-quickbooks/00/1179709) | [Entering Historical Data by JE only](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/entering-historical-data-by-je-only/00/1240771)

### Option B — Transaction-Level Entry (for full-detail reconciliation)

Import or manually enter individual transactions. Enter in this order to prevent balance errors:
1. Past sales and Accounts Receivable
2. Past vendor bills and Accounts Payable
3. Bank and credit card transactions

Reconcile each account month-by-month before advancing to the next month. Note: QBO bank feeds only provide 90 days of downloads; anything older must be manually entered or imported via CSV/QBO file.

**Source:** [How do I enter previous year transactions](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-enter-previous-year-transactions-in-new-quickbooks/00/181818) | CPA-reviewed historical reconciliation guides

### Option C — Convert from QB Desktop Backup

If a prior bookkeeper used QuickBooks Desktop, request the `.QBB` backup file. Restore it in a QB Desktop trial and convert to QBO. This is the most efficient path when the data already exists.

**Source:** [I need to build 3 years of history — community thread](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-need-to-build-3-years-of-history-into-a-new-quickbooks/00/1179709)

---

## 2. QBO Setup for Historical Reconstruction

### Company Start Date

Set the company start date to the earliest date you intend to enter data. In QBO Simple Start, the start date set during setup cannot be changed directly; a new company file may be required if the wrong date was chosen.

**Source:** [Change QuickBooks Online Start Date](https://www.justanswer.com/accounting-software/se0um-new-quickbooks-online-started-company.html)

### Chart of Accounts First

Build the complete Chart of Accounts before entering any transactions or opening balances. Include all accounts needed across all years you intend to reconstruct. Accounts referenced in journal entries must exist in QBO before import.

**Source:** [Opening Balance Equity in QuickBooks — Synder](https://synder.com/blog/what-is-opening-balance-equity-in-quickbooks-and-how-to-manage-it/) | [Import Journal Entries QBO](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/)

### Account Numbers

If account numbers are enabled, turn them OFF before importing journal entries via CSV. Account numbers cause import failures. Re-enable them after import is complete.

**Source:** [Can you import Journal entries with sub accounts in QBO](https://quickbooks.intuit.com/learn-support/en-ca/reports-accounting/can-you-import-journal-entries-with-sub-accounts-in-qbo/00/1397385)

---

## 3. Opening Balance Handling

### Method 1 — Journal Entry (Preferred by CPAs)

Enter the balance sheet as of your earliest start date as a single JE:
- Assets: debit entries
- Liabilities and equity: credit entries
- Contra accounts (e.g., accumulated depreciation): credit for contra-assets, debit for contra-liabilities
- Debits must equal credits exactly — no Opening Balance Equity residual

This is preferred over the direct COA entry method because it forces balance and keeps Opening Balance Equity at zero.

**Source:** QuickBooks Community forums, CPA opening balance guides

### Method 2 — Direct COA Entry (Creates OBE, not recommended)

Entering opening balances directly in individual accounts auto-creates an "Opening Balance Equity" offset. This results in a non-zero OBE balance that must be cleaned up. Accountants generally discourage this approach.

### Special Accounts

- **Accounts Receivable**: Each customer must be created in QBO's customer list first before an AR balance can be posted.
- **Accounts Payable**: Same requirement — each vendor must exist.
- **Inventory**: Import individual items if needed; post totals to OBE temporarily if items are not yet set up.
- **Retained Earnings**: Cannot receive a direct opening balance via journal entry in standard QBO (system-generated account). See Section 5.

**Source:** CPA opening balance guides

### Zeroing Out Opening Balance Equity

OBE is a temporary clearing account. After setup it must reach $0:
1. Run a trial balance as of the setup date.
2. Create a JE: Debit OBE, Credit Retained Earnings (or Owner's Equity / Partner Capital accounts as appropriate).
3. Verify trial balance shows OBE = $0.

For partnerships: OBE clears to each partner's capital account (not RE). For corporations: OBE clears to Retained Earnings or Paid-in Capital.

**Source:** [Opening Balance Equity and Retained Earnings — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/opening-balance-equity-and-retained-earnings/00/250138) | [Opening Balance Equity — Tax Hub Solutions](https://taxhubsolutions.squarespace.com/knowledge-center/opening-balance-equity-and-how-to-resolve-it-in-qbo)

---

## 4. Chart of Accounts Evolution Over Multiple Years

### Core Principle: Stability

The COA should remain stable year-over-year. Experts recommend: "companies should not be changing COA once they have a good one setup." When changes are needed, add new accounts rather than modifying existing ones.

**Source:** [What do I do about history if I change chart of accounts from year to year](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/what-do-i-do-about-history-if-i-change-chart-of-accounts-from/00/595806)

### Renaming Accounts

Renaming an account in QBO retroactively changes that account name on all historical reports — the account history transfers to the new name. This is useful for minor name corrections but can cause confusion if the change is material (e.g., reports sent to tax preparers or management may no longer match prior-year labels).

### Merging Accounts

To consolidate two accounts:
1. Note the Account Type, Detail Type, and Name of the account to KEEP.
2. Edit the account to REMOVE so its Type, Detail Type, and Name exactly match the keeper.
3. Save — QBO prompts "merge accounts?" Confirm.

Transaction history from the removed account moves to the keeper. This is **irreversible**. Accounts linked to online banking feeds cannot be merged.

**Critical caveat**: Accounts with historical reconciliation reports may lose reconciliation history data in the merge. Save reconciliation reports as PDFs before merging.

**Source:** QuickBooks Community forums, Intuit Accountant documentation

### Sub-Account Strategy for COA Evolution

When a business changes account structure over time (e.g., splits one expense account into several), making old accounts sub-accounts of new parent accounts is an option. This fixes collapsed report views across old and new transactions but retroactively alters the structure of historical financial statements — use with caution.

**Source:** [QBO chart of accounts evolution — search results](https://liveflow.com/knowledge-center/how-to-map-group-and-consolidate-gl-accounts-across-entities-in-quickbooks-online-best-tools-compared)

### Sub-Account CSV Import Format

When referencing sub-accounts in journal entry CSV imports, use a colon without spaces:
```
ParentAccount:SubAccount
```

**Source:** [Can you import Journal entries with sub accounts in QBO](https://quickbooks.intuit.com/learn-support/en-ca/reports-accounting/can-you-import-journal-entries-with-sub-accounts-in-qbo/00/1397385)

---

## 5. Retained Earnings Behavior in QBO — Critical Understanding

### Automatic Year-End Close (Invisible Transaction)

QBO does NOT create an actual journal entry when closing net income to Retained Earnings at year-end. The year-end close is:

- Calculated at **report time**, not stored as a real transaction
- An "invisible transaction" — it does not appear in any register or journal
- Performed automatically: on the first day of each new fiscal year, QBO takes the prior year's net income and adds it to the Retained Earnings balance on the balance sheet

When you view the Retained Earnings account, you get a QuickReport (not a register), which displays calculated "Closing Entry" line items per year. These are not clickable transactions — they are computed values.

**Source:** [Manually creating prior year's retained earnings — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/manually-creating-prior-year-s-retained-earnings/00/1139782) | [Retained Earnings account details — Intuit](https://quickbooks.intuit.com/learn-support/en-us/help-article/financial-reports/view-retained-earnings-account-details/L7d6Ugx58_US_en_US)

### Implication for Multi-Year JE Imports

When importing historical JEs dated in prior years:
- QBO automatically rolls forward net income per year to RE as each fiscal year boundary is crossed.
- You do NOT need to post a manual closing entry to move net income to RE — QBO handles this.
- If you post a manual closing JE (debiting income/expense accounts and crediting RE), it will DOUBLE-COUNT because QBO also applies its invisible close.
- Do NOT manually create JEs to move net income to retained earnings.

**Source:** [Entering Historical Data by JE only](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/entering-historical-data-by-je-only/00/1240771)

### Verifying Retained Earnings Is Correct

Run the Profit & Loss report for ALL years (custom date range from company inception to present). Sum all net income figures. That total should equal the Retained Earnings balance on the balance sheet.

**Source:** [How to adjust retained earnings in QuickBooks — Method](https://www.method.me/blog/adjust-retained-earnings-in-quickbooks/)

### Setting an Opening RE Balance (Workaround)

QBO does not allow a direct opening balance entry for the Retained Earnings account (it is system-generated). Workarounds:

1. **Use an equity adjustment account**: Create an equity account called "Accumulated Adjustments" (or similar) and post the historical opening RE balance there. Then, once all historical income/expense JEs are entered and the system-computed RE matches expected, zero out the adjustment account.

2. **OBE to RE JE**: When zeroing out Opening Balance Equity, credit Retained Earnings to transfer the accumulated historical equity into RE. This works if the JE method is used for opening balances.

3. **Implied RE through P&L history**: If all historical P&L JEs are entered correctly per year, QBO will auto-compute the correct RE on the balance sheet without any manual RE entry.

**Source:** [Opening Balance Equity and Retained Earnings — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/opening-balance-equity-and-retained-earnings/00/250138) | [Brought forward open balance for retained earnings](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/brought-forward-open-balance-for-retained-earnings/00/828725)

### Adjusting a Prior Year's RE Balance

If a previously filed tax year needs a RE correction:
- Do NOT edit the historical transactions (distorts filed return data).
- Use a Retained Earnings adjustment JE in the current year, or in the first day of the year following the erroneous year.
- This is the standard "prior period adjustment" approach — post to RE directly via an equity account.

**Source:** [How can I correct my 2021 retained earnings balance?](https://quickbooks.intuit.com/learn-support/en-us/account-management/how-can-i-correct-my-2021-retained-earnings-balance/00/1074722) | [Fix Retained Earning for Different Fiscal Year](https://quickbooks.intuit.com/learn-support/en-us/account-management/fix-retained-earning-for-different-fiscal-year/00/1139294)

---

## 6. Period Locking

### How Closing the Books Works in QBO

Go to: Gear > Account and Settings > Advanced > Accounting section > "Close the books" (toggle on). Set a closing date and optionally a password. Any attempt to create or edit a transaction on or before the closing date will either warn the user or require the password.

**Source:** [Close your books in QuickBooks Online — Intuit](https://quickbooks.intuit.com/learn-support/en-us/help-article/close-books/close-books-quickbooks-online/L59LelyPM_US_en_US) | [Closing Dates in QuickBooks Online — Paygration](https://paygration.com/closing-dates-in-quickbooks-online/)

### Recommended Locking Protocol for Historical Reconstruction

1. Enter and verify all JEs for a given year.
2. Reconcile bank/credit card accounts for that year.
3. Run P&L and Balance Sheet to verify against source documents.
4. Post CPA adjusting journal entries if needed.
5. Set closing date to December 31 of that year with a password.
6. Lock before moving to the next year.

This prevents app re-syncs, accidental edits, or back-dated entries from corrupting completed years.

**Source:** CPA year-end period locking guides, historical reconciliation resources

### Unlocking for CPA Adjusting Entries

The only time to unlock a closed period after tax filing is to enter adjusting journal entries from the CPA. Procedure:
1. Temporarily clear the closing date.
2. Enter the CPA's AJEs.
3. Re-lock the period.

**Source:** [How to close books in QuickBooks Online — Method](https://www.method.me/blog/close-books-quickbooks-online/)

### Closing Date Does Not Trigger Year-End Close

Setting a closing date does not cause QBO to perform any automated income/expense-to-RE closing entries. The automatic RE close is driven purely by the fiscal year boundary and is always happening in the background at report time.

**Source:** [Will it perform a year end close when I close a period?](https://quickbooks.intuit.com/learn-support/global/manage-suppliers-and-expenses/will-it-perform-a-year-end-close-transferring-net-income-to/00/1487926)

---

## 7. Importing Journal Entries in Bulk

### Native QBO CSV Import — US Version Status

The native CSV journal entry import feature is **unavailable in the US version of QBO** (available in Canada and UK). It may be accessible in QBO Advanced tier or QBO Accountant, but availability is inconsistent and users report loading failures even when the option appears.

**Source:** [Importing Journal Entries in Quickbooks Online US version](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/importing-journal-entries-in-quickbooks-online-us-version/00/1312709)

### Native Import: When It Works

When the native import is available (Settings > Import Data > Journal Entries):
- Format: CSV with headers — Journal Date, Ref Number (Journal No.), Account Name, Debit, Credit, Description/Memo
- All lines of one JE must share the same Ref Number
- Limit: 1,000 rows per file
- Account names must exactly match existing COA accounts
- Turn off account numbers before importing; re-enable after
- For sub-accounts, use `ParentAccount:SubAccount` format (no spaces around colon)

**Source:** [Import Journal Entries into QBO — SaaSAnt guide](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/) | [Import journal entries — Intuit help](https://quickbooks.intuit.com/learn-support/en-us/help-article/import-export-data-files/import-journal-entries-quickbooks-online/L4tQBwbs7_US_en_US)

### QBO API for Bulk JE Import

The QBO Accounting API supports back-dated journal entries via the `TxnDate` field. If not supplied, the server's current date is used.

Rate limits:
- 500 requests/minute per company (realm ID)
- 10 concurrent requests maximum per app
- Batch operations: 40 batch requests/minute
- HTTP 429 returned when exceeded

For historical reconstructions with hundreds of JEs, use batch operations and space requests to stay under limits. Third-party managed connectors (e.g., SaaSAnt, Transaction Pro) handle rate-limit logic automatically.

**Source:** [QuickBooks Online Journal Entry API reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/journalentry) | [QuickBooks API Rate Limits — Coefficient](https://coefficient.io/quickbooks-api/quickbooks-api-rate-limits)

### Third-Party Import Tools

- **SaaSAnt Transactions**: Supports Excel, CSV, IIF; handles bulk imports; available as QBO app. Good for large-scale historical migrations.
- **Transaction Pro Importer**: ~$19.99/month; frequently recommended in QBO community for bulk JE imports.
- **Coefficient**: Managed connector that handles rate limits automatically.

**Source:** [Import Journal Entries into QBO — SaaSAnt](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/) | [How do I enter previous year transactions](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-enter-previous-year-transactions-in-new-quickbooks/00/181818)

---

## 8. CPA-Reviewed Historical Reconciliation Workflow

The following is a synthesis of the professional CPA workflow for historical catch-up:

### Pre-Work
- Collect all bank/credit card/loan PDF or CSV statements for the backlog period
- Gather prior reconciliation records, trial balances, and AR/AP aging reports
- Identify all connected app payout schedules (payment processors, e-commerce platforms)

### Year-by-Year Process
1. Build or verify the COA covers all accounts needed for the year.
2. Post opening balance JE for the first year being reconstructed (balance sheet as of start date).
3. Post P&L JEs for each month of the year (or one annual JE for distant years).
4. For transaction-level reconstructions: reconcile each bank/credit card account to statements, month by month, before advancing.
5. Verify P&L and Balance Sheet tie to source documents.
6. Post any CPA adjusting entries (depreciation, accruals, prepaids).
7. Export/save reconciliation reports and trial balance as PDFs.
8. Set closing date for December 31 of that year.
9. Advance to next year.

### Post-Reconciliation Checks
- AR and AP aging reports match the general ledger
- Undeposited Funds balance is zero or explained
- Balance sheet retained earnings matches sum of all historical net income

### Documentation Protocol
- Monthly folders: statements, reconciliation exports, workpapers
- Memo attached to every adjusting JE linking to source document
- Master checklist tracking status per account per month

**Source:** CPA-reviewed historical reconciliation resources, year-end closing best-practice guides

---

## 9. Common Pitfalls and Failure Modes

### Pitfall 1 — Manually Posting RE Closing Entries

Do not create a JE to move net income to Retained Earnings at year-end. QBO does this automatically at report time. Manual closing JEs will double-count and corrupt retained earnings.

**Source:** [Manually creating prior year's retained earnings](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/manually-creating-prior-year-s-retained-earnings/00/1139782)

### Pitfall 2 — Non-Zero Opening Balance Equity

Leaving OBE non-zero after setup distorts the balance sheet and raises red flags during tax filing, audits, and loan applications. Always zero it out by transferring to appropriate equity accounts.

**Source:** [Opening Balance Equity and How to Resolve It in QBO](https://taxhubsolutions.squarespace.com/knowledge-center/opening-balance-equity-and-how-to-resolve-it-in-qbo)

### Pitfall 3 — Back-Dating Entries After Filing

Any JE or transaction posted to a prior closed period will affect the current balance sheet (balance sheet is a running total from day one). If the prior year's tax return has been filed, do not alter historical data — post a current-year adjustment instead to avoid creating discrepancies with filed returns.

**Source:** [Adjusting journal entry dated prior year affects current balance sheet — search results](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/year-end-adjustments/00/999558)

### Pitfall 4 — Merging Accounts Destroys Reconciliation History

Merging two accounts is irreversible and may destroy reconciliation history for the consumed account. Save reconciliation reports before merging. Never merge accounts linked to bank feeds.

**Source:** QuickBooks account merging documentation

### Pitfall 5 — Advancing Without Monthly Reconciliation

When entering transaction-level historical data, skipping monthly reconciliation and running all entries at once causes compounding errors. The running bank balance shifts with each entry, making it impossible to isolate where discrepancies originated.

**Source:** [How do I enter previous year transactions](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-enter-previous-year-transactions-in-new-quickbooks/00/181818)

### Pitfall 6 — Importing Against Accounts That Don't Exist in COA

Journal entry CSV imports silently fail or create import errors when account names in the CSV don't match COA exactly (case-sensitive in some versions). Pre-create all accounts before attempting import.

**Source:** [Import Journal Entries — SaaSAnt](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/)

---

## 10. QBO Accountant Features Relevant to Historical Work

### Working Trial Balance (QBOA Only)

QuickBooks Online Accountant (the free accountant version) includes a **Working Trial Balance** report that shows opening and closing balances per period — standard QBO does not have this. This is critical for year-by-year verification during reconstruction.

**Source:** [How do I get Detailed Trial Balance With Opening & Closing balances](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-get-detailed-trial-balance-with-opening-closing/00/788991)

### Adjusting Journal Entries Flag

QBOA allows marking JEs as "Adjusting Journal Entries" — this flags them in reports so they are visually distinguishable from operating entries. Useful for separating CPA AJEs from the reconstructed historical transactions.

**Source:** [QuickBooks Online Basics: Making Journal Entries](https://insightfulaccountant.com/accounting-tech/general-ledger/quickbooks-online-basics-making-journal-entries-series-3-le/)

### Sending Books to Accountant

QBO has a "Prep for Taxes" feature that maps QBO accounts to tax form lines and exports to ProConnect. After completing historical reconstruction, this is the pathway for generating tax return inputs from QBO data.

**Source:** [Start new tax returns for clients — QuickBooks](https://quickbooks.intuit.com/learn-support/en-us/help-article/tax-return/start-new-tax-returns-clients-quickbooks-online/L4Vo52upD_US_en_US)

---

## 11. IRS Audit Considerations

When QBO files are subpoenaed or reviewed:
- The IRS receives access to all data including journal entries, reconciliation history, and the audit trail log.
- QBO maintains an audit trail that logs who made changes and when — this applies to back-dated entries as well.
- For historical reconstructions, attaching source document memos to every JE creates a defensible paper trail.
- Period copies (QB Desktop only) allow extraction of just a specific time range for review.

**Source:** [Implications of IRS Examination of Accounting Software Data — Tax Adviser](https://www.thetaxadviser.com/issues/2014/may/morris-may2014/) | [QuickBooks Online Records to Keep for IRS 7-Year Audit](https://www.justanswer.com/tax/qac78-again-follow-up-question-quickbooks-online.html)

---

## Source Index

- [How do I enter previous year transactions in new QuickBooks?](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-enter-previous-year-transactions-in-new-quickbooks/00/181818)
- [I need to build 3 years of history — can I use journal entries?](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-need-to-build-3-years-of-history-into-a-new-quickbooks/00/1179709)
- [Entering Historical Data by JE only](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/entering-historical-data-by-je-only/00/1240771)
- [Manually creating prior year's retained earnings](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/manually-creating-prior-year-s-retained-earnings/00/1139782)
- CPA opening balance journal entry guides
- [Opening Balance Equity and How to Resolve It in QBO — Tax Hub Solutions](https://taxhubsolutions.squarespace.com/knowledge-center/opening-balance-equity-and-how-to-resolve-it-in-qbo)
- [Opening Balance Equity in QuickBooks — SaaSAnt](https://www.saasant.com/blog/opening-balance-equity-in-quickbooks/)
- [Opening Balance Equity in QuickBooks — Synder](https://synder.com/blog/what-is-opening-balance-equity-in-quickbooks-and-how-to-manage-it/)
- [Opening Balance Equity and Retained Earnings — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/opening-balance-equity-and-retained-earnings/00/250138)
- [Fix Opening Balance Equity in QuickBooks Online — Optimum](https://optimumresultsbusiness.com/how-to-fix-opening-balance-equity-in-quickbooks-online/)
- [Understanding Retained Earnings in QuickBooks Online — Gentle Frog](https://gentlefrog.com/understanding-retained-earnings-in-quickbooks-online/)
- [How to adjust retained earnings in QuickBooks — Method](https://www.method.me/blog/adjust-retained-earnings-in-quickbooks/)
- [Fix Retained Earning for Different Fiscal Year](https://quickbooks.intuit.com/learn-support/en-us/account-management/fix-retained-earning-for-different-fiscal-year/00/1139294)
- [How can I correct my 2021 retained earnings balance?](https://quickbooks.intuit.com/learn-support/en-us/account-management/how-can-i-correct-my-2021-retained-earnings-balance/00/1074722)
- [Close Equity into Retained Earnings (QB Online) — Candus Kampfer](https://canduskampfer.com/close-equity-into-retained-earnings-in-quickbooks-online/)
- [Brought forward open balance for retained earnings](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/brought-forward-open-balance-for-retained-earnings/00/828725)
- [Will it perform a year end close when I close a period?](https://quickbooks.intuit.com/learn-support/global/manage-suppliers-and-expenses/will-it-perform-a-year-end-close-transferring-net-income-to/00/1487926)
- [What do I do about history if I change chart of accounts from year to year](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/what-do-i-do-about-history-if-i-change-chart-of-accounts-from/00/595806)
- [Taming the chart of accounts in QuickBooks Online — Firm of the Future](https://www.firmofthefuture.com/accounting/taming-the-chart-of-accounts-in-quickbooks-online/)
- QuickBooks account merging documentation
- [How to merge accounts in QBO — eBizCharge](https://ebizcharge.com/tutorials/how-to-merge-accounts-in-quickbooks-online/)
- [How to lock closed periods?](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-lock-closed-periods/00/193572)
- [Closing Dates in QuickBooks Online — Paygration](https://paygration.com/closing-dates-in-quickbooks-online/)
- CPA year-end period locking guides
- [Record Year-End Adjusting Entries in QuickBooks — Mahoney CPAs](https://mahoneycpa.com/record-year-end-adjusting-entries-in-quickbooks-and-close-your-books/)
- [How to close books in QuickBooks Online — Method](https://www.method.me/blog/close-books-quickbooks-online/)
- CPA-reviewed historical reconciliation resources
- [Import Journal Entries into QBO — SaaSAnt guide](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/)
- [Import journal entries — Intuit help article](https://quickbooks.intuit.com/learn-support/en-us/help-article/import-export-data-files/import-journal-entries-quickbooks-online/L4tQBwbs7_US_en_US)
- [Importing Journal Entries in QBO US version — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/importing-journal-entries-in-quickbooks-online-us-version/00/1312709)
- [Can you import Journal entries with sub accounts in QBO](https://quickbooks.intuit.com/learn-support/en-ca/reports-accounting/can-you-import-journal-entries-with-sub-accounts-in-qbo/00/1397385)
- [QuickBooks Online Journal Entry API reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/journalentry)
- [QuickBooks API Rate Limits — Coefficient](https://coefficient.io/quickbooks-api/quickbooks-api-rate-limits)
- QuickBooks tax season cleanup guides
- [Implications of IRS Examination of Accounting Software Data — Tax Adviser](https://www.thetaxadviser.com/issues/2014/may/morris-may2014/)
- Year-end closing best-practice guides
- [QuickBooks Online Basics: Making Journal Entries — Insightful Accountant](https://insightfulaccountant.com/accounting-tech/general-ledger/quickbooks-online-basics-making-journal-entries-series-3-le/)
- [Start new tax returns for clients — QuickBooks](https://quickbooks.intuit.com/learn-support/en-us/help-article/tax-return/start-new-tax-returns-clients-quickbooks-online/L4Vo52upD_US_en_US)
