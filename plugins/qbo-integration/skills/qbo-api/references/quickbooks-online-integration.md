---
name: quickbooks-online-integration
description: |
  Comprehensive QuickBooks Online API integration for Python applications.

  Use when: OAuth 2.0 auth setup, creating journal entries or batch transactions,
  REST/GraphQL queries, financial report generation, document attachments,
  year-end close, Form 1120 data extraction, or multi-entity consolidation.
---

# QuickBooks Online Integration

Python-first integration patterns for QuickBooks Online REST and GraphQL APIs.

## Quick Start

### 1. Install Dependencies

```bash
pip install python-quickbooks intuit-oauth requests openpyxl
```

### 2. OAuth 2.0 Authentication

```python
from intuitlib.client import AuthClient
from intuitlib.enums import Scopes

auth_client = AuthClient(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET",
    redirect_uri="YOUR_REDIRECT_URI",
    environment="sandbox"  # or "production"
)

# Generate authorization URL
auth_url = auth_client.get_authorization_url([Scopes.ACCOUNTING])
# Redirect user to auth_url, then exchange code for tokens
auth_client.get_bearer_token(auth_code, realm_id=realm_id)
# CLI alternative: scripts/oauth_setup.py auth-url | exchange | refresh
```

### 3. Create QuickBooks Client

```python
from quickbooks import QuickBooks

client = QuickBooks(
    auth_client=auth_client,
    refresh_token=auth_client.refresh_token,
    company_id=realm_id
)
```

---

## Core Workflows

### Journal Entry Creation

```python
from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

je = JournalEntry()
je.TxnDate = "2024-01-15"
je.PrivateNote = "Dividend income from investments"

# Debit line (Asset increase)
debit = JournalEntryLine()
debit.JournalEntryLineDetail = {
    "PostingType": "Debit",
    "AccountRef": {"value": "35", "name": "Checking"}
}
debit.Amount = 1000.00
debit.Description = "Dividend received"

# Credit line (Income)
credit = JournalEntryLine()
credit.JournalEntryLineDetail = {
    "PostingType": "Credit",
    "AccountRef": {"value": "79", "name": "Dividend Income"}
}
credit.Amount = 1000.00
credit.Description = "Dividend income"

je.Line = [debit, credit]
je.save(qb=client)
```

### Query Accounts

```python
from quickbooks.objects.account import Account

# Get all accounts
accounts = Account.all(qb=client)

# Query specific account type
income_accounts = Account.query(
    "SELECT * FROM Account WHERE AccountType = 'Income'",
    qb=client
)

# Get single account by ID
checking = Account.get(35, qb=client)
```

### Create Customer

```python
from quickbooks.objects.customer import Customer

customer = Customer()
customer.DisplayName = "ACME Corporation"
customer.CompanyName = "ACME Corporation"
customer.PrimaryEmailAddr = {"Address": "billing@acme.example.com"}
customer.save(qb=client)
```

---

## Reference Documentation

| Document | Purpose |
|----------|---------|
| [references/api-entities.md](references/api-entities.md) | REST API entity reference (Account, JournalEntry, Invoice, Bill, Payment) |
| [references/graphql-patterns.md](references/graphql-patterns.md) | GraphQL queries, mutations, and pagination |
| [references/batch-patterns.md](references/batch-patterns.md) | Batch operations for bulk processing |
| [references/reports-financial.md](references/reports-financial.md) | Financial reports: P&L, Balance Sheet, Cash Flow, GL |
| [references/reports-analysis.md](references/reports-analysis.md) | Trial Balance, comparative reports, Excel export |
| [references/attachments-upload.md](references/attachments-upload.md) | Document upload and transaction linking |
| [references/attachments-management.md](references/attachments-management.md) | Query, download, delete, batch operations |
| [references/investments-accounting.md](references/investments-accounting.md) | Investment journal entries: buy, sell, dividends |
| [references/investments-tax-reporting.md](references/investments-tax-reporting.md) | 1099 processing, brokerage reconciliation |
| [references/catchup-planning.md](references/catchup-planning.md) | Catch-up assessment, COA setup, opening balances |
| [references/catchup-transaction-entry.md](references/catchup-transaction-entry.md) | Transaction entry, bank statement import |
| [references/catchup-reconciliation.md](references/catchup-reconciliation.md) | Bank reconciliation, validation checks |
| [references/chart-of-accounts.md](references/chart-of-accounts.md) | COA setup for holding companies and C-Corps |
| [references/compliance-checklist.md](references/compliance-checklist.md) | Form 5472, F-1120, year-end procedures |
| [references/error-codes.md](references/error-codes.md) | Common errors and troubleshooting |
| [references/tax-form-1120-page1.md](references/tax-form-1120-page1.md) | Form 1120 Page 1 income/deduction mappings |
| [references/tax-schedule-l-m.md](references/tax-schedule-l-m.md) | Schedules L, M-1, M-2, C for Form 1120 |
| [references/tax-estimated-payments.md](references/tax-estimated-payments.md) | Quarterly estimated tax calculations and JEs |
| [references/audit-trial-balance.md](references/audit-trial-balance.md) | Trial balance extraction for CPA workpapers |
| [references/audit-pbc-lists.md](references/audit-pbc-lists.md) | PBC list generation and tracking |
| [references/audit-lead-schedules.md](references/audit-lead-schedules.md) | Lead schedules with standard references |
| [references/audit-aje-patterns.md](references/audit-aje-patterns.md) | Adjusting Journal Entry patterns |
| [references/audit-rje-patterns.md](references/audit-rje-patterns.md) | Reclassifying Journal Entry patterns |
| [references/consolidation-intercompany.md](references/consolidation-intercompany.md) | Intercompany tracking, due to/from, eliminations |
| [references/consolidation-statements.md](references/consolidation-statements.md) | Combined TB, consolidated BS/IS |
| [references/consolidation-segments.md](references/consolidation-segments.md) | Segment definitions and reporting, NCI |


## Assets

| Asset | Purpose |
|-------|---------|
| [assets/coa-template.json](assets/coa-template.json) | Holding company Chart of Accounts template |

---

## API Endpoints

### Base URLs

| Environment | REST API | GraphQL |
|-------------|----------|---------|
| Sandbox | `https://sandbox-quickbooks.api.intuit.com` | `https://public.api.intuit.com/2020-04/graphql` |
| Production | `https://quickbooks.api.intuit.com` | `https://public.api.intuit.com/2020-04/graphql` |

### Common Endpoints

```
POST   /v3/company/{realmId}/account          # Create account
GET    /v3/company/{realmId}/account/{id}     # Read account
POST   /v3/company/{realmId}/journalentry     # Create journal entry
POST   /v3/company/{realmId}/invoice          # Create invoice
POST   /v3/company/{realmId}/bill             # Create bill
POST   /v3/company/{realmId}/payment          # Create payment
POST   /v3/company/{realmId}/batch            # Batch operations
GET    /v3/company/{realmId}/query            # Query entities
GET    /v3/company/{realmId}/reports/{name}   # Run reports
```

---

## Document Ingestion Patterns

### Excel Tax Sheet Processing

```python
from scripts.parse_tax_sheet import TaxSheetParser

parser = TaxSheetParser("tax_data_2023.xlsx")
transactions = parser.extract_transactions()

for txn in transactions:
    je = create_journal_entry(
        date=txn["date"],
        debit_account=txn["debit_account"],
        credit_account=txn["credit_account"],
        amount=txn["amount"],
        memo=txn["description"]
    )
    je.save(qb=client)
```

### 1099-DIV Processing

```python
# Map 1099-DIV boxes to QBO accounts
DIV_MAPPING = {
    "1a": "Ordinary Dividends",      # Box 1a -> Dividend Income
    "1b": "Qualified Dividends",     # Box 1b -> subset of 1a
    "2a": "Capital Gain Distributions",  # Box 2a -> Capital Gains
    "3":  "Nondividend Distributions",   # Box 3 -> Return of capital
    "4":  "Federal Tax Withheld"     # Box 4 -> Tax liability
}

def process_1099_div(data: dict, date: str):
    entries = []
    if data.get("1a"):
        entries.append(create_dividend_entry(
            date=date,
            amount=data["1a"],
            income_account="Dividend Income",
            asset_account="Checking"
        ))
    return entries
```

---

## Multi-Year Catch-Up Workflow

### Sequencing

1. **Establish opening balances** for earliest year
2. **Process each year sequentially** (2019 -> 2020 -> ... -> current)
3. **Run year-end close** before moving to next year
4. **Verify balances** match source documents

### Year-End Close

QBO handles year-end close **automatically** via an invisible report-time calculation. NEVER post manual closing journal entries — they create manual adjustments that conflict with the auto-calculation and corrupt Retained Earnings.

```python
def verify_fiscal_year(client, year: int):
    """Verify year data is correct — QBO handles year-end close automatically."""
    # Run P&L report to confirm revenue/expenses posted correctly
    pnl = client.query(
        f"SELECT * FROM ProfitAndLoss WHERE StartDate='{year}-01-01' "
        f"AND EndDate='{year}-12-31'",
        qb=client
    )
    print(f"Year {year} P&L: Revenue={pnl.total_income}, Expenses={pnl.total_expense}")

    # QBO auto-calculates Retained Earnings from cumulative P&L.
    # No manual closing entry needed. RE balance updates automatically
    # when you run the Balance Sheet report.
```

---

## Error Handling

```python
from quickbooks.exceptions import QuickbooksException, AuthorizationException
from intuitlib.exceptions import AuthClientError

try:
    je.save(qb=client)
except AuthorizationException:
    # Token expired - refresh
    auth_client.refresh(refresh_token=client.refresh_token)
    client = QuickBooks(
        auth_client=auth_client,
        refresh_token=auth_client.refresh_token,
        company_id=realm_id
    )
    je.save(qb=client)
except QuickbooksException as e:
    print(f"QBO Error {e.error_code}: {e.message}")
    # See references/error-codes.md for resolution
except AuthClientError as e:
    print(f"OAuth Error: {e.status_code} - {e.content}")
```

---

## Key Concepts

### Account Types

| Type | Classification | Use Case |
|------|----------------|----------|
| Bank | Asset | Checking, savings accounts |
| Accounts Receivable | Asset | Customer invoices owed |
| Other Current Asset | Asset | Short-term investments, prepaid |
| Fixed Asset | Asset | Property, equipment |
| Accounts Payable | Liability | Vendor bills owed |
| Other Current Liability | Liability | Short-term obligations |
| Long Term Liability | Liability | Loans, mortgages |
| Equity | Equity | Retained earnings, capital stock |
| Income | Revenue | Sales, dividends, interest income |
| Cost of Goods Sold | Expense | Direct costs |
| Expense | Expense | Operating expenses |
| Other Expense | Expense | Non-operating expenses |
| Other Income | Revenue | Non-operating income |

### Transaction Types

| Entity | Purpose | Accounts Affected |
|--------|---------|-------------------|
| JournalEntry | General ledger adjustments | Any accounts |
| Invoice | Customer billing | A/R (debit), Income (credit) |
| Payment | Customer payment received | Bank (debit), A/R (credit) |
| Bill | Vendor invoice | Expense (debit), A/P (credit) |
| BillPayment | Vendor payment made | A/P (debit), Bank (credit) |
| Deposit | Bank deposit | Bank (debit), various (credit) |
| Transfer | Account-to-account transfer | Bank (both) |
| SalesReceipt | Cash sale | Bank (debit), Income (credit) |

---

## Best Practices

1. **Always use SyncToken** for updates to prevent conflicts
2. **Batch operations** for bulk inserts (max 30 items per batch)
3. **Query with pagination** for large datasets (`STARTPOSITION`, `MAXRESULTS`)
4. **Validate accounts exist** before creating transactions
5. **Use minor versions** for latest features (`?minorversion=65`)
6. **Store refresh tokens securely** - they're long-lived credentials
7. **Log all API responses** for audit trail
8. **Implement retry logic** with exponential backoff for rate limits
9. **Validate totals** - debits must equal credits in journal entries
10. **Use meaningful DocNumbers** for easy reference

---

## Production vs Sandbox

| Setting | Sandbox | Production |
|---------|---------|------------|
| Realm ID | (from .env: QBO_SANDBOX_REALM_ID) | (from .env: QBO_PROD_REALM_ID) |
| Token file | `tmp/qbo_tokens_sandbox.json` | `tmp/qbo_tokens_prod.json` |
| Base URL | `sandbox-quickbooks.api.intuit.com` | `quickbooks.api.intuit.com` |

Environment is controlled by `.env` `QBO_ENVIRONMENT` variable (`sandbox` or `production`). Always verify `QBO_ENVIRONMENT` before executing writes — production writes are irreversible.

```python
from scripts.qbo_auth import get_environment, load_qbo_client
env = get_environment()  # reads QBO_ENVIRONMENT from .env
client = load_qbo_client(environment=env)
```

---

## COA Best Practices for Historical Reconstruction

- **Build the complete COA for all years before importing any JEs** — discovering missing accounts mid-import causes rollback pain
- **NEVER post manual closing JEs to Retained Earnings** — QBO auto-calculates RE at report time via invisible year-end close. Direct RE postings create manual adjustments that conflict with the auto-calculation and corrupt financial reports
- **Use QBO standard names with AcctNum set via API** — e.g., display name "Checking" with AcctNum 1010, not "1010 - Cash - Business Checking"
- **Rename default accounts rather than creating parallels** — Intuit's recommendation; preserves transaction history, avoids duplicates
- **Set correct AccountType and AccountSubType** — these drive Prep for Taxes auto-mapping to Form 1120 lines. Wrong types corrupt P&L structure and tax categorization
- **QBO Online has NO native Form 1120 tax line mapping** — that feature is Desktop-only. All mapping is via correct account subtypes
- **A/R and A/P must use QBO's default system accounts** — custom A/R and A/P accounts don't appear in aging reports and can't route invoices or bills
- **Zero Opening Balance Equity after reconstruction** — OBE is a temporary account that must net to zero

---

## Multi-Year Import Pattern

Import oldest year first, verify, lock period, advance:

1. Year 1 (2007): Only needs incorporation entries (common stock issuance). No opening balance JE — the company starts from zero.
2. Each subsequent year: `assemble-package {year}` → `close-year {year}` → verify trial balance → next year
3. QBO accumulates RE from cumulative P&L — no opening balance JEs needed after year 1
4. Never import closing entries — QBO handles year-end close automatically
5. Verify P&L and Balance Sheet after each year before proceeding

---

## Account Name Mapping and Identity Resolution

CSV account names (e.g., "1010 - Cash - Business Checking") are transformed to canonical QBO names in two steps:

1. **At assemble time**: CSV name → canonical name via `work-product/account_name_mapping.yaml` (the production CSV uses canonical names)
2. **At close time**: canonical name → QBO account via live query (`Account.Name.lower()` lookup)

AcctNum is used by `setup_production_coa.py` for initial COA setup and as a human reference. The `AccountRef.name` field in JE payloads is informational only — QBO matches on `AccountRef.value` (the numeric Account.Id). The close-year command resolves Account.Id from live QBO queries, not from a static registry.

```python
# At close time: resolve canonical account name to QBO Account.Id
qbo_accounts = Account.all(qb=client)
account_lookup = {a.Name.lower(): {'id': a.Id, 'type': a.AccountType} for a in qbo_accounts}

canonical_name = "Cash - Business Checking"  # from production CSV
acct_info = account_lookup[canonical_name.lower()]

# Use in JE line
line.JournalEntryLineDetail = {
    "PostingType": "Debit",
    "AccountRef": {"value": acct_info['id'], "name": canonical_name}
}
```

---

## CSV Schema Conventions

When processing CSV files for QBO import (journal entries, transactions):

### Column Constants

```python
# Canonical column names - USE THESE when accessing normalized rows
CSV_COL_DATE = 'Date'
CSV_COL_NUM = 'Num'
CSV_COL_ACCOUNT = 'Account'
CSV_COL_DEBIT = 'Debit'
CSV_COL_CREDIT = 'Credit'
CSV_COL_MEMO = 'Memo'
CSV_COL_NAME = 'Name'
```

### Normalization Pattern

```python
# Always use read_csv_normalized() to handle format variations
rows = read_csv_normalized(csv_path)
for row in rows:
    je_no = row.get(CSV_COL_NUM)      # Not row.get('Num')
    debit = float(row.get(CSV_COL_DEBIT) or 0)
```

### Why This Matters

1. **Format Independence**: Handles both QBO export format (`Date`, `Debit`) and import format (`*JournalDate`, `Debits`)
2. **Single Point of Truth**: Alias mapping in `CSV_COLUMN_ALIASES` is the only place format variations are defined
3. **No Scattered Fallbacks**: Eliminates `row.get('X') or row.get('Y')` chains throughout code

See: `scripts/qbo_close_helpers.py` — implements `read_csv_normalized()`, `CSV_COLUMN_ALIASES`, and all close-year operations (validate-prerequisites, import-jes, deep-verify, verify-balance-sheet, verify-continuity).
