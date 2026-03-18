---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QBO Journal Entry Import: Account Matching & Best Practices

Compiled: 2026-02-24. Sources linked inline throughout.

---

## 1. How Account Matching Works

### CSV / Native Import Tool

Account matching is **exact string match only** — no fuzzy matching, no partial matching, no case-insensitive fallback.

The value in the "Account Name" column of your CSV must match the QBO Chart of Accounts entry character-for-character, including:
- Exact capitalization
- Exact spacing (leading/trailing spaces will break the match)
- Exact punctuation

Source: [dancingnumbers.com – Import JEs into QBO](https://www.dancingnumbers.com/import-journal-entries-into-quickbooks-online/) | [mycloudbookkeeping.org – Import JEs via CSV](https://www.mycloudbookkeeping.org/post/how-to-import-journal-entries-into-quickbooks-online-using-excel-or-csv)

### QBO API (AccountRef)

The API does **not** match by name. It requires the numeric account **Id** in `AccountRef.value`. The `AccountRef.name` field is informational/optional and is not used for lookup.

```json
"AccountRef": {
  "value": "87",
  "name": "Accounts Payable (A/P)"
}
```

Workflow to resolve names to IDs:
1. Query `SELECT * FROM Account` (or filter by name) to retrieve Account objects
2. Use `Account.Id` as `AccountRef.value`
3. Optionally include `Account.Name` as `AccountRef.name` for readability

Source: [QBO Journal Entry API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/journalentry) | [QBO Account API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account) | [endgrate.com – Using QBO API to Get Accounts](https://endgrate.com/blog/using-the-quickbooks-api-to-get-accounts-(with-python-examples))

---

## 2. What Happens When Referenced Accounts Don't Exist

### CSV Import

- The import **fails** with an error for any row referencing an account not in the COA
- Error message: **"Account Does Not Exist"** or **"Line Account Invalid"**
- The entire batch may be rejected (not just the offending rows)
- QBO **does not auto-create accounts** from CSV import

Source: [detamoov – Common Errors During QBO Import](https://detamoov.happyfox.com/kb/article/170-common-errors-during-a-quickbooks-online-import/) | [Transaction Pro – Account Does Not Exist Error](https://tprosupport.rightworks.com/kb/article/629-account-does-not-exist-error-during-import-in-rightworks-transaction-pro-importer/)

### API

- The API **does not auto-create accounts** inline when creating JournalEntry objects
- Referencing a non-existent account Id returns an error (typically HTTP 400)
- Accounts must be created via the Account API endpoint before they can be referenced in JEs
- Required workflow: Create account → Get its Id → Use that Id in JournalEntry lines

Source: [apideck.com – Exploring the QBO Accounting API](https://www.apideck.com/blog/exploring-the-quickbooks-online-accounting-api) | [Intuit Developer – QBO Account API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)

---

## 3. Subaccount Handling

### The Core Problem

The native QBO CSV import **cannot post JE lines to subaccounts** directly. Attempting to use a subaccount name produces a **"Line Account Invalid"** error, even when the subaccount exists.

Source: [QBO Community – Importing JEs with subaccounts keeps failing](https://quickbooks.intuit.com/learn-support/en-ca/other-questions/importing-qbo-exported-journal-entries-with-subaccounts-keeps/00/1538534)

### Correct Subaccount Notation in CSV

When importing IS possible with subaccounts, the required format is:

```
ParentAccountName:SubAccountName
```

Rules:
- **No spaces** around the colon
- Include the full hierarchy path for deeply nested accounts
- Example: `6000 R&D:6010 Wages`
- Must **turn off account numbers** in QBO settings before importing

Source: [QBO Community – Can you import JEs with subaccounts?](https://quickbooks.intuit.com/learn-support/en-ca/reports-accounting/can-you-import-journal-entries-with-sub-accounts-in-qbo/00/1397385) | [mycloudbookkeeping.org – Import JEs via CSV](https://www.mycloudbookkeeping.org/post/how-to-import-journal-entries-into-quickbooks-online-using-excel-or-csv)

### Workaround for Persistent Subaccount Failures

Official Intuit workaround (widely criticized as impractical):
1. Temporarily convert subaccounts to parent accounts
2. Import the JEs
3. Revert accounts back to subaccount status

Better approach: Use the API directly (which accepts subaccount Ids without restriction) or use a third-party importer (SaasAnt, Transaction Pro, Dancing Numbers).

### API Subaccount Behavior

The API accepts any valid account Id including subaccounts. There is no "Line Account Invalid" restriction when using the API — this is specific to the CSV import tool.

---

## 4. COA Setup Requirements Before Import

### Mandatory Pre-Import Steps

1. **Create all accounts first** — Every account referenced in JEs must exist in QBO COA before import. No exceptions.
2. **Turn off account numbers** — Go to Settings → Account and Settings → Advanced → Chart of Accounts → disable "Enable account numbers". Re-enable after import.
3. **Disable duplicate JE warnings** if this preference is enabled (can block batch imports)
4. **Enable Classes/Locations** in settings if your JEs use them

Source: [insightfulaccountant.com – Batch Import JEs into QuickBooks](https://insightfulaccountant.com/accounting-tech/general-ledger/batch-import-journal-entries-into-quickbooks/) | [finoptimal.com – Master QBO JE Import Guide](https://www.finoptimal.com/resources/master-quickbooks-journal-entry-import-a-comprehensive-guide-for-smooth-financial-management)

### COA Conversion Table for Multi-Year

When migrating historical data across systems:
- Build a conversion table documenting old → new account name mappings
- Identify one-to-one and one-to-many account relationships
- Edit COA in source system first to capitalize on retroactive naming changes
- COA consistency between systems is the #1 migration success factor

Source: [asbinc.net – Migrating QuickBooks Desktop History](https://asbinc.net/migrating-quickbooks-desktop-history/)

---

## 5. CSV Format Requirements

### Required Column Headers

Standard QBO import template columns:

| Column | Required | Notes |
|--------|----------|-------|
| Journal No. | Yes | Groups rows into one JE; must be identical for all lines of same entry |
| Journal Date | Yes | Format must be MM/DD/YYYY or matching configured format |
| Account Name | Yes | Exact match to COA name |
| Debits | Yes* | One of Debit or Credit required per row; not both |
| Credits | Yes* | One of Debit or Credit required per row; not both |
| Journal Entry Description | No | Entry-level memo |
| Memo | No | Line-level description |
| Names | No | Required for A/R (Customer) and A/P (Vendor) lines |
| Class | No | Must exist in QBO if used |
| Location | No | Must exist in QBO if used |

Source: [Transaction Pro – Import JEs into QBO](https://tprosupport.rightworks.com/kb/article/881-import-journal-entries-into-quickbooks-online/) | [insightfulaccountant.com – Batch Import JEs](https://insightfulaccountant.com/accounting-tech/general-ledger/batch-import-journal-entries-into-quickbooks/) | [mycloudbookkeeping.org](https://www.mycloudbookkeeping.org/post/how-to-import-journal-entries-into-quickbooks-online-using-excel-or-csv)

### Critical Format Rules

- **One debit OR credit per row** — both populated on same row causes "Posting Type null" error
- **Negative amounts not supported** — use the opposite column instead
- **All rows for one JE must have the same Journal No. and Date**
- **Debits must equal credits** per JE; imbalanced entry fails the entire batch
- **No blank rows** between entries within a file
- **Sort by Journal No.** so all lines of each entry are grouped together

Source: [zed-systems.com – Import JEs into QBO](https://support.zed-systems.com/kb/a236/import-journal-entries-into-quickbooks-online.aspx) | [Transaction Pro](https://tprosupport.rightworks.com/kb/article/881-import-journal-entries-into-quickbooks-online/)

### A/R and A/P Special Requirements

Any JE line posting to an Accounts Receivable account **must** include a Customer name in the Names column. Any JE line posting to an Accounts Payable account **must** include a Vendor name. Omitting this causes the import to fail for those lines.

Source: [Transaction Pro – Import JEs into QBO](https://tprosupport.rightworks.com/kb/article/881-import-journal-entries-into-quickbooks-online/) | [zed-systems.com](https://support.zed-systems.com/kb/a236/import-journal-entries-into-quickbooks-online.aspx)

---

## 6. Field Length Limits and Character Restrictions

### Account Names (COA)

- **Max length**: 100 characters
- **Colons prohibited**: Colons (`:`) are reserved for subaccount hierarchy notation and cannot appear in individual account names themselves
- Account names must be unique within the COA

Source: [QBO Community CA – Character Limitations for Fields in QBO](https://quickbooks.intuit.com/learn-support/en-ca/other-questions/character-limitations-for-fields-in-quickbooks-online/00/898984) | [QBO Community – Account name 100 char limit](https://quickbooks.intuit.com/learn-support/en-us/other-questions/change-font-size-or-increase-number-of-characters-allowed-in/00/1203864)

### Other Field Limits (QBO)

| Field | Limit |
|-------|-------|
| Account name (COA) | 100 characters, no colons |
| Customer / Vendor name | 41 characters |
| Item name | 31 characters |
| DocNumber (JE reference) | ~20–21 characters |
| Custom fields | 30 characters |
| Description columns | 4,000 characters |
| Customer notes | 4,000 characters |
| Email address | 100 characters |

Source: [Mothernode – QBO Special Characters](https://support.mothernode.com/knowledge-base/quickbooks-special-characters-causing-syncing-errors/) | [QBO Community CA – Character Limitations](https://quickbooks.intuit.com/learn-support/en-ca/other-questions/character-limitations-for-fields-in-quickbooks-online/00/898984) | [QBO Character Limitations Help Article](https://quickbooks.intuit.com/learn-support/en-us/help-article/printing-preferences/character-limitations-fields-quickbooks/L7eIy5gE3_US_en_US)

### Acceptable Special Characters

QBO accepts the following special characters in most fields:
- Alphanumeric: A–Z, a–z, 0–9
- `,` `.` `?` `@` `&` `!` `#` `'` `~` `*`
- Space, `_` `-` `;` `+`

Characters that cause sync/import errors:
- Registered trademark: `®`
- Trademark: `™`
- Other Unicode/non-ASCII characters introduced by copy-paste from PDFs or Word documents
- Invisible characters (zero-width space, non-breaking space)

Source: [Mothernode – QuickBooks Special Characters Causing Syncing Errors](https://support.mothernode.com/knowledge-base/quickbooks-special-characters-causing-syncing-errors/) | [QBO Acceptable Characters Help Article](https://quickbooks.intuit.com/learn-support/en-us/help-article/account-management/acceptable-characters-quickbooks-online/L3CiHlD9J_US_en_US)

---

## 7. Bulk Import: Volume Limits and Performance

### Native CSV Import

- **Maximum rows per file**: 1,000 rows
- **File formats accepted**: CSV, XLSX
- Large files should be split into smaller batches for performance and error isolation

Source: [insightfulaccountant.com – Batch Import JEs](https://insightfulaccountant.com/accounting-tech/general-ledger/batch-import-journal-entries-into-quickbooks/) | [dancingnumbers.com – Import JEs into QBO](https://www.dancingnumbers.com/import-journal-entries-into-quickbooks-online/)

### QBO API Rate Limits

| Limit Type | Value |
|-----------|-------|
| Standard requests | 500 requests/minute per realm ID |
| Concurrent requests | 10 simultaneous per company per app |
| Batch requests | 40 batch requests/minute per realm ID |
| Max payloads per batch | 30 operations per batch request |

When limits are exceeded, the API returns **HTTP 429 Too Many Requests**. Implement exponential backoff and retry logic.

Source: [coefficient.io – QuickBooks API Rate Limits](https://coefficient.io/quickbooks-api/quickbooks-api-rate-limits) | [Intuit Developer Help – API Call Limits and Throttling](https://help.developer.intuit.com/s/article/API-call-limits-and-throttling)

### Batch API for JEs

The QBO Batch API endpoint (`/v3/company/<realmID>/batch`) can wrap up to 30 operations (create/update/delete) in a single HTTP request. Journal entry creates can be included. Execution order within a batch is not guaranteed.

This is the most efficient approach for programmatic bulk JE imports: batch 25–30 JE creates per request, pause between batches to stay under the 40 batch/minute ceiling.

Source: [Intuit Blog – Building Smarter with Intuit: Batch without a Scratch](https://blogs.intuit.com/2023/07/19/building-smarter-with-intuit-batch-without-a-scratch/) | [QBO Community – Create multiple items at once using QBO API](https://help.developer.intuit.com/s/question/0D5G000004Dk7IFKAZ/create-multiple-items-at-once-using-qbo-api-)

---

## 8. Multi-Year Historical JE Import Issues

### Common Pitfalls

1. **COA drift across years** — Account names that changed between years must be reconciled. The COA must have all accounts present before importing any year's JEs, even if some accounts were only active for certain periods.

2. **Closed period / locked books** — If you have set a closing date in QBO, importing JEs to dates before that date will prompt for the closing date password. Via the API, this same restriction applies — transactions in locked periods require the password to be posted.

3. **Opening balance complexity** — For the earliest year being imported, you must establish opening balances as a JE before importing subsequent years. Recommended: import the oldest year-end trial balance as a single JE, then import monthly JEs for subsequent periods.

4. **A/P and A/R clearing** — If you are reconstructing historical A/P and A/R, use temporary clearing accounts. Close them at the end of the reconstruction period to avoid phantom open transactions.

5. **Retained earnings conflicts** — QBO auto-calculates retained earnings. Do not post directly to Retained Earnings in historical JEs; instead let net income roll forward naturally or use an opening balance equity account.

Source: [asbinc.net – Migrating QuickBooks Desktop History](https://asbinc.net/migrating-quickbooks-desktop-history/) | [QBO Community – How to enter previous year transactions](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-enter-previous-year-transactions-in-new-quickbooks/00/181818)

### Recommended Multi-Year Strategy

The widely recommended approach for historical reconstruction:

1. Create a complete, finalized COA covering all years before importing anything
2. Import the opening trial balance for the earliest year as a single JE (e.g., 12/31/2006 if starting 2007)
3. Import monthly journal entries year-by-year, oldest to newest
4. After each year, verify trial balance matches source documents before proceeding to next year
5. Use unique DocNumbers with year and sequence embedded (e.g., `2007-001`, `2007-002`) for traceability

Source: [asbinc.net – Migrating QuickBooks Desktop History](https://asbinc.net/migrating-quickbooks-desktop-history/) | [mycloudbookkeeping.org – How to Import JEs Using Excel or CSV](https://www.mycloudbookkeeping.org/post/how-to-import-journal-entries-into-quickbooks-online-using-excel-or-csv)

### Closed Period Behavior

- QBO's "Close the books" feature sets a password-protected date boundary
- API calls posting to dates before the closing date require the closing date password (enforced at the API level too)
- For historical reconstruction where books are not yet closed, this is not an issue
- Only unlock closed periods to enter CPA adjustments; document all changes

Source: [QBO Community – How do I edit a transaction in a closed year?](https://quickbooks.intuit.com/learn-support/en-us/banking/how-do-i-edit-a-transaction-in-a-closed-year/00/769747) | [QBO Community – How to lock closed periods](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-lock-closed-periods/00/193572)

---

## 9. API: Account Creation Workflow

### Accounts Must Be Pre-Created

The QBO API does not auto-create accounts from JournalEntry payloads. You must:
1. Create each account via `POST /v3/company/<realmID>/account`
2. Capture the returned `Account.Id`
3. Use that Id in `AccountRef.value` on JE line items

### python-quickbooks: Create Account

```python
from quickbooks.objects.account import Account

account = Account.from_json({
    "AccountType": "Expense",
    "AcctNum": "6010",
    "Name": "Wages and Salaries"
})
account.save(qb=client)
account_id = account.Id
```

Source: per the python-quickbooks SDK JournalEntry class

### python-quickbooks: Query Account by Name

```python
from quickbooks.objects.account import Account

accounts = Account.filter(Name="Wages and Salaries", qb=client)
if accounts:
    account_id = accounts[0].Id
```

Or using raw query:
```python
from quickbooks.objects.account import Account

accounts = Account.query(
    "SELECT * FROM Account WHERE Name = 'Wages and Salaries'",
    qb=client
)
```

### python-quickbooks: Create JournalEntry

```python
from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine, JournalEntryLineDetail
from quickbooks.objects.base import Ref

je = JournalEntry()
je.TxnDate = "2007-12-31"
je.DocNumber = "2007-001"
je.PrivateNote = "Year-end reconstruction"

# Debit line
debit_line = JournalEntryLine()
debit_detail = JournalEntryLineDetail()
debit_detail.PostingType = "Debit"
account_ref = Ref()
account_ref.value = "87"   # Account.Id
account_ref.name = "Wages and Salaries"
debit_detail.AccountRef = account_ref
debit_line.JournalEntryLineDetail = debit_detail
debit_line.Amount = 50000.00
debit_line.DetailType = "JournalEntryLineDetail"
je.Line.append(debit_line)

# Credit line (similar structure, PostingType = "Credit")
# ...

je.save(qb=client)
```

Source: per the python-quickbooks SDK JournalEntry class | hevodata.com — Python QuickBooks Integration

---

## 10. Third-Party Import Tools

For high-volume or complex multi-year imports, these tools provide more reliable workflows than native QBO import:

- **SaasAnt Transactions** — supports bulk JE import, better error reporting, handles subaccounts
- **Transaction Pro Importer (Rightworks)** — the most-cited tool for QBO imports; requires accounts to pre-exist but provides clearer error messages
- **Dancing Numbers** — supports CSV/Excel import with account name validation
- **Zed Systems** — alternative importer with detailed field documentation

All third-party tools share the same fundamental constraint: **accounts must exist in QBO COA before import**. The difference is that some tools provide auto-create features for entities (customers, vendors) but not for accounts.

Source: [asbinc.net – Migrating QBO Desktop History](https://asbinc.net/migrating-quickbooks-desktop-history/) | [Transaction Pro – Import JEs into QBO](https://tprosupport.rightworks.com/kb/article/881-import-journal-entries-into-quickbooks-online/) | [saasant.com – Importing JEs into QBO](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/)

---

## 11. Common Errors Reference

| Error | Cause | Fix |
|-------|-------|-----|
| Account Does Not Exist | Account name not in COA, or name mismatch | Create account in COA first; verify exact name match |
| Line Account Invalid | Subaccount referenced without full path, or subaccount used when only parent allowed | Use `Parent:SubAccount` colon notation; turn off account numbers |
| Account Ref is missing | Account name not mapped or blank | Verify column mapping; ensure account name column is populated |
| Posting Type null error | Both Debit and Credit populated on same row | Use one column per row only |
| Imbalanced Journal | Debits ≠ Credits for a single JE | Recalculate; each JE group must balance independently |
| Only one detail line exists | Journal No. not grouping rows correctly | Ensure identical Journal No. and Date across all lines of same entry |
| Missing customer/vendor | A/R or A/P line missing entity | Add Customer (A/R) or Vendor (A/P) to the Names column |
| HTTP 429 | API rate limit exceeded | Implement exponential backoff; reduce request rate |

Source: [detamoov – Common Errors During QBO Import](https://detamoov.happyfox.com/kb/article/170-common-errors-during-a-quickbooks-online-import/) | [zed-systems.com – Import JEs into QBO](https://support.zed-systems.com/kb/a236/import-journal-entries-into-quickbooks-online.aspx) | [Intuit Developer – Fix Common QBO API Errors](https://developer.intuit.com/app/developer/qbo/docs/develop/troubleshooting/handling-common-errors)

---

## 12. Pre-Import Checklist

Before any JE import (CSV or API):

- [ ] All accounts referenced in JEs exist in QBO COA
- [ ] Account names match COA exactly (case, spacing, punctuation)
- [ ] Subaccount names use `Parent:Child` colon notation with no spaces around colon
- [ ] Account numbers turned off in QBO settings (for CSV import)
- [ ] Duplicate JE warnings disabled if applicable
- [ ] Classes and Locations enabled in settings if JEs use them
- [ ] Every JE balances (debits = credits)
- [ ] A/R lines include Customer name; A/P lines include Vendor name
- [ ] Negative amounts converted to opposite column entries
- [ ] Date format consistent throughout file
- [ ] File split into batches of ≤1,000 rows (CSV) or ≤30 operations (API batch)
- [ ] DocNumbers are unique and follow consistent naming scheme
- [ ] Test with a small sample batch before full import

---

## Source URLs

- [Intuit Developer – QBO JournalEntry API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/journalentry)
- [Intuit Developer – QBO Account API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)
- [Intuit Developer – Fix Common QBO Accounting API Errors](https://developer.intuit.com/app/developer/qbo/docs/develop/troubleshooting/handling-common-errors)
- [Intuit Developer Help – API Call Limits and Throttling](https://help.developer.intuit.com/s/article/API-call-limits-and-throttling)
- [QBO Community – Import JEs with subaccounts keeps failing](https://quickbooks.intuit.com/learn-support/en-ca/other-questions/importing-qbo-exported-journal-entries-with-subaccounts-keeps/00/1538534)
- [QBO Community – Can you import JEs with sub accounts in QBO?](https://quickbooks.intuit.com/learn-support/en-ca/reports-accounting/can-you-import-journal-entries-with-sub-accounts-in-qbo/00/1397385)
- [QBO Community – How to enter previous year transactions](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-enter-previous-year-transactions-in-new-quickbooks/00/181818)
- [QBO Community – How to edit a transaction in a closed year](https://quickbooks.intuit.com/learn-support/en-us/banking/how-do-i-edit-a-transaction-in-a-closed-year/00/769747)
- [QBO Community – How to lock closed periods](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-lock-closed-periods/00/193572)
- [QBO Community CA – Character Limitations for Fields in QBO](https://quickbooks.intuit.com/learn-support/en-ca/other-questions/character-limitations-for-fields-in-quickbooks-online/00/898984)
- [QBO Help – Acceptable Characters in QBO](https://quickbooks.intuit.com/learn-support/en-us/help-article/account-management/acceptable-characters-quickbooks-online/L3CiHlD9J_US_en_US)
- [QBO Help – Character Limitations for Fields in QuickBooks](https://quickbooks.intuit.com/learn-support/en-us/help-article/printing-preferences/character-limitations-fields-quickbooks/L7eIy5gE3_US_en_US)
- [QBO Help – Import Journal Entries](https://quickbooks.intuit.com/learn-support/en-us/help-article/import-export-data-files/import-journal-entries-quickbooks-online/L4tQBwbs7_US_en_US)
- [Transaction Pro – Import JEs into QBO](https://tprosupport.rightworks.com/kb/article/881-import-journal-entries-into-quickbooks-online/)
- [Transaction Pro – Account Does Not Exist Error](https://tprosupport.rightworks.com/kb/article/629-account-does-not-exist-error-during-import-in-rightworks-transaction-pro-importer/)
- [detamoov – Common Errors During QBO Import](https://detamoov.happyfox.com/kb/article/170-common-errors-during-a-quickbooks-online-import/)
- [asbinc.net – Tips for Migrating QuickBooks Desktop History](https://asbinc.net/migrating-quickbooks-desktop-history/)
- [insightfulaccountant.com – Batch Import JEs into QuickBooks](https://insightfulaccountant.com/accounting-tech/general-ledger/batch-import-journal-entries-into-quickbooks/)
- [mycloudbookkeeping.org – Import JEs Using Excel or CSV](https://www.mycloudbookkeeping.org/post/how-to-import-journal-entries-into-quickbooks-online-using-excel-or-csv)
- [finoptimal.com – Master QuickBooks JE Import Guide](https://www.finoptimal.com/resources/master-quickbooks-journal-entry-import-a-comprehensive-guide-for-smooth-financial-management)
- [saasant.com – Importing JEs into QBO: Step by Step Guide](https://www.saasant.com/blog/importing-journal-entries-into-quickbooks-online-step-by-step-guide/)
- [zed-systems.com – Import JEs into QBO](https://support.zed-systems.com/kb/a236/import-journal-entries-into-quickbooks-online.aspx)
- [dancingnumbers.com – Import JEs into QBO](https://www.dancingnumbers.com/import-journal-entries-into-quickbooks-online/)
- [endgrate.com – Using the QBO API to Get Accounts (Python examples)](https://endgrate.com/blog/using-the-quickbooks-api-to-get-accounts-(with-python-examples))
- [hevodata.com – How to Set Up Python QuickBooks Integration](https://hevodata.com/learn/python-quickbooks/)
- python-quickbooks SDK (JournalEntry class, Account class)
- [coefficient.io – QuickBooks API Rate Limits](https://coefficient.io/quickbooks-api/quickbooks-api-rate-limits)
- [apideck.com – Exploring the QBO Accounting API](https://www.apideck.com/blog/exploring-the-quickbooks-online-accounting-api)
- [Mothernode – QuickBooks Special Characters Causing Syncing Errors](https://support.mothernode.com/knowledge-base/quickbooks-special-characters-causing-syncing-errors/)
- [Intuit Blog – Building Smarter with Intuit: Batch without a Scratch](https://blogs.intuit.com/2023/07/19/building-smarter-with-intuit-batch-without-a-scratch/)
