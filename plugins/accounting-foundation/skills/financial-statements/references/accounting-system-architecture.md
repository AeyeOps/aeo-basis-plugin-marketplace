---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Accounting System Architecture Reference

Patterns and data models from a production-grade double-entry accounting system.
Covers: chart of accounts, GL posting engine, transaction flows, financial reports, budgets, assets, multi-currency, and period closing.

---

## 1. Architecture

### Document-Oriented Design
- Every business object is a **document type** (schema definition + controller logic)
- Document types have lifecycle hooks: `validate()`, `before_submit()`, `on_submit()`, `on_cancel()`
- `docstatus`: 0=Draft, 1=Submitted, 2=Cancelled
- Submissions are immutable — cancellation creates reverse entries

### Module Layout
```
accounts/
  doctype/          # ~100+ document types (Account, GL Entry, Journal Entry, etc.)
  report/           # ~50+ reports (P&L, BS, Trial Balance, GL, Cash Flow, etc.)
  general_ledger    # Core GL posting engine
  utils             # Balance queries, fiscal year, payment ledger, reconciliation
  party             # Customer/Supplier account resolution
  deferred_revenue
assets/
  doctype/          # Asset, Asset Category, Asset Depreciation Schedule, etc.
  report/           # Depreciation ledger, asset balance reports
controllers/
  accounts_controller  # Base class for all accounting transactions
  taxes_and_totals     # Tax calculation engine
  budget_controller    # Budget validation engine
setup/
  utils              # Exchange rate fetching
```

### Controller Hierarchy (Inheritance)
```
Document
  +-- TransactionBase
       +-- AccountsController          # Base for all accounting docs
            |-- SellingController       # Sales Invoice, Sales Order
            |    +-- SalesInvoice
            |-- BuyingController        # Purchase Invoice, Purchase Order
            |    +-- PurchaseInvoice
            |-- PaymentEntry
            |-- JournalEntry
            +-- Asset
```

### Core Ledgers (3 parallel ledgers)
- **GL Entry** — double-entry general ledger; every accounting transaction creates GL entries
- **Payment Ledger Entry** — tracks outstanding amounts for Receivable/Payable accounts; drives reconciliation
- **Stock Ledger Entry** — inventory valuation (not covered here)

---

## 2. Chart of Accounts (COA)

### Account Data Model
- Uses a **nested set tree** structure with `lft`/`rgt` materialized path columns
- Parent link field: `parent_account`
- Auto-naming pattern: `"{account_number} - {account_name} - {company_abbr}"` or `"{account_name} - {company_abbr}"`

### Account Fields
- `root_type`: `Asset | Liability | Income | Expense | Equity` — the 5 fundamental types
- `report_type`: `Balance Sheet | Profit and Loss` — derived from root_type:
  - Balance Sheet = Asset, Liability, Equity
  - Profit and Loss = Income, Expense
- `account_type`: ~30 specialized types that drive system behavior:
  - **Transactional**: `Bank`, `Cash`, `Receivable`, `Payable`
  - **Stock**: `Stock`, `Stock Adjustment`, `Stock Received But Not Billed`
  - **Asset**: `Fixed Asset`, `Accumulated Depreciation`, `Capital Work in Progress`, `Depreciation`
  - **P&L**: `Cost of Goods Sold`, `Direct Income`, `Indirect Income`, `Direct Expense`, `Indirect Expense`, `Expense Account`, `Income Account`
  - **Special**: `Tax`, `Round Off`, `Round Off for Opening`, `Temporary`, `Equity`, `Chargeable`
- `balance_must_be`: enforces Debit-only or Credit-only constraint
- `is_group`: 1 = group/parent account (cannot post to), 0 = ledger (can post to)
- `account_currency`: per-account currency (defaults to company currency)
- `freeze_account`: prevents posting unless user has allowed role

### Standard COA Template
- Nested dictionary structure: `{root_account: {child: {grandchild: {account_type: "...", account_category: "..."}, ...}}}`
- Root nodes: Application of Funds (Assets), Expenses, Income, Source of Funds (Liabilities), Equity
- Each leaf can specify `account_type`, `account_category`, `is_group`
- Country-specific COA templates available (40+ countries)

### COA Import
- Imports COA from CSV or JSON
- CSV format: columns = Account Name, Parent Account, Account Number, Account Type, Root Type, Is Group

### Company Default Accounts
- `default_bank_account`, `default_cash_account`
- `default_receivable_account`, `default_payable_account`
- `default_expense_account`, `default_income_account`
- `stock_received_but_not_billed`, `stock_adjustment_account`
- `write_off_account`, `round_off_account`
- `exchange_gain_loss_account`, `unrealized_exchange_gain_loss_account`
- `accumulated_depreciation_account`, `depreciation_expense_account`
- `disposal_account` (gain/loss on asset disposal)
- `default_deferred_revenue_account`, `default_deferred_expense_account`

---

## 3. GL Entry — The Core Ledger Record

### GL Entry Fields
- `account` — the Account being debited/credited
- `debit` / `credit` — amounts in company currency
- `debit_in_account_currency` / `credit_in_account_currency` — amounts in account's own currency
- `debit_in_reporting_currency` / `credit_in_reporting_currency` — for multi-currency reporting
- `debit_in_transaction_currency` / `credit_in_transaction_currency` — original transaction currency
- `voucher_type` / `voucher_no` — the source document (Journal Entry, Sales Invoice, etc.)
- `against_voucher_type` / `against_voucher` — linked counter-document (for payment matching)
- `party_type` / `party` — Customer/Supplier (required for Receivable/Payable accounts)
- `cost_center` — required for P&L accounts
- `project`, `finance_book` — optional dimensions
- `posting_date`, `fiscal_year`
- `is_opening` — "Yes" for opening balance entries
- `is_cancelled` — 1 when reversed
- `against` — comma-separated list of counter-accounts

### GL Entry Validation Rules
- **Mandatory**: account, voucher_type, voucher_no, company
- **Party required** for Receivable/Payable accounts
- **No zero entries** (either debit or credit must be non-zero)
- **Cost center mandatory** for P&L accounts
- **No P&L in opening entries**
- **Account must be ledger** (not group), active, same company
- **Currency validation** — GL entry currency must match account currency
- **Frozen account check** — blocked unless user has allowed role
- **Balance constraint** — respects `balance_must_be` on Account
- **Reporting currency** — auto-calculated using exchange rate to reporting currency

### GL Entry Indexes
- `(voucher_type, voucher_no)` — fast lookup by source document
- `(posting_date, company)` — date-range queries
- `(party_type, party)` — party-based queries

---

## 4. General Ledger Engine

### GL Posting Entry Point
This is the **central entry point** for all GL posting. Every accounting transaction calls this.

**Flow (non-cancel):**
1. Budget validation — check budget limits
2. Accounting dimension offsetting entries — auto-create balancing entries for accounting dimensions
3. Validate accounting period — block posting in closed periods
4. Validate disabled accounts — reject disabled accounts
5. Process GL map:
   - Distribute entries across sub-cost-centers by allocation percentage
   - Merge similar entries — combine entries with same (account, cost_center, party, against_voucher, project, finance_book, + accounting dimensions)
   - Toggle debit/credit if negative — normalize negative debits to credits and vice versa
6. Create payment ledger entry — parallel entry in Payment Ledger for Receivable/Payable tracking
7. Save entries:
   - Validate CWIP accounts — block CWIP accounts in Journal Entries
   - Process debit/credit difference — auto-create round-off entry if diff within allowance
   - Check freezing date — block posting before frozen date
   - Validate against PCV — block posting before Period Closing Voucher date
   - Create and submit each GL Entry record

**Flow (cancel):**
- Create reverse GL entries — mirror entries with swapped debit/credit
- Mark original entries as `is_cancelled = 1` (or create new reversal entries if immutable ledger enabled)

### Round-Off Handling
- Allowance: 5/(10^precision) for JE/PE, 0.5 for others
- Auto-creates round-off GL entry using company's `round_off_account` and `round_off_cost_center`
- Separate `round_off_for_opening` account for opening entries

### Merge Properties (what makes entries "similar")
```
[account, cost_center, party, party_type, voucher_detail_no,
 against_voucher, against_voucher_type, project, finance_book,
 voucher_no, advance_voucher_type, advance_voucher_no]
+ accounting_dimensions
```

---

## 5. Transaction Flow: Invoice to Payment to Reconciliation

### Sales Invoice Flow
1. **Create** Sales Invoice — items, taxes, customer, debit_to account
2. **Submit**:
   - GL entries created:
     - DR: Debit To (Receivable) = grand_total
     - CR: Income Account(s) = net amounts per item
     - CR/DR: Tax accounts = tax amounts
   - Creates Payment Ledger Entry (outstanding = grand_total)
   - Updates company monthly sales
3. **Payment Entry** created against invoice:
   - `payment_type`: Receive (for sales), Pay (for purchases), Internal Transfer
   - `paid_from` / `paid_to` — accounts for the payment flow
   - `references` table links to invoices being paid
   - GL entries:
     - DR: Bank/Cash account
     - CR: Receivable account (with against_voucher = Sales Invoice)
   - Payment Ledger Entry reduces outstanding on invoice
4. **Reconciliation** — Payment Reconciliation tool matches unallocated payments to invoices

### Purchase Invoice Flow
- Mirror of Sales: CR Credit To (Payable), DR Expense/Stock accounts
- Payment Entry: Pay type, DR Payable, CR Bank

### Journal Entry
- `voucher_type` subtypes: Journal Entry, Bank Entry, Cash Entry, Credit Card Entry, Debit Note, Credit Note, Contra Entry, Write Off Entry, Opening Entry, Depreciation Entry, Asset Disposal, Exchange Rate Revaluation, Exchange Gain Or Loss, Deferred Revenue/Expense, Periodic Accounting Entry
- Line items table: each row = one GL line with account, debit, credit, party, against_voucher
- Validation: total_debit must equal total_credit
- On submit: creates GL entries from line items table
- Large JE (>100 rows) auto-queued for background submission
- Tax withholding support

### Payment Entry
- Three types: `Receive` (customer paying), `Pay` (paying supplier), `Internal Transfer`
- Key fields: `paid_from`, `paid_to`, `paid_amount`, `received_amount`, `source_exchange_rate`, `target_exchange_rate`
- `references` table links to invoices/orders being paid
- `deductions` table for write-offs/adjustments
- Tax withholding support
- Advance payment tracking in separate party account
- GL entries created for the payment + any exchange gain/loss

---

## 6. Payment Ledger & Outstanding Tracking

### Payment Ledger Entry
- Parallel to GL Entry but focused on Receivable/Payable tracking
- Tracks: voucher (invoice) + against_voucher (payment) linkages
- `account_type`: Receivable or Payable
- Used by payment ledger query builder to compute outstanding amounts

### Outstanding Amount Update
- Called after payment allocation
- Queries Payment Ledger for net outstanding
- Updates `outstanding_amount` on Sales/Purchase Invoice
- Triggers dunning updates and status changes

### Outstanding Invoice Query
- Core function for Payment Reconciliation and Payment Entry
- Queries Payment Ledger for invoices with positive outstanding
- Returns sorted by due_date
- Supports filters: min/max outstanding, accounting dimensions, specific vouchers

### Immutable Ledger Mode
- When enabled, cancellation does not set `is_cancelled=1`
- Instead creates new reversal entries with `is_cancelled=0` and current posting_date

---

## 7. Journal Entry Detailed Structure

### Validation Chain
1. Validate party — party frozen/disabled check
2. Validate entries for advance — advance payment validation
3. Validate multi-currency — exchange rate requirements
4. Set amounts in company currency — convert account currency to company currency
5. Validate debit/credit amount — no negative amounts
6. Set total debit/credit — sum all rows
7. Validate against linked journal entries — cross-reference validation
8. Validate stock accounts — stock account balance check
9. Validate reference document — linked document validation
10. Set against account — populate `against` field
11. Validate credit/debit note — return invoice checks
12. Validate inter-company accounts — multi-company validation
13. Validate depreciation account and entry voucher type — depreciation account rules

### Submit Chain
- Final balance check — total debit must equal total credit
- Post to GL — create GL entries
- Check credit limit — customer credit limit
- Update asset value — for depreciation/disposal entries
- Update inter-company journal entry — cross-company journal sync

---

## 8. Tax Handling

### Tax Templates
- **Sales Taxes and Charges Template** — applied to Sales Invoice/Order
- **Purchase Taxes and Charges Template** — applied to Purchase Invoice/Order
- **Item Tax Template** — item-level tax overrides
- Each template has rows with: charge_type (On Net Total, On Previous Row Total, Actual, On Item Quantity), account_head, rate

### Tax Calculation Engine
- Called during validation on all transaction documents
- Processes taxes table row by row
- Charge types:
  - `On Net Total` — percentage of net total
  - `On Previous Row Total` — percentage of running total
  - `On Previous Row Amount` — percentage of specific previous row
  - `Actual` — fixed amount
  - `On Item Quantity` — per-unit charge
- Supports inclusive taxes (included_in_print_rate)
- Item-wise tax detail stored as JSON

### Tax Withholding
- Tax withholding category record:
  - `rates` table: tax_withholding_rate, from_date, to_date, single_threshold, cumulative_threshold, tax_withholding_group
  - `accounts` table: company to tax account mapping
  - `tax_deduction_basis`: Gross Total or Net Total
  - `tax_on_excess_amount` — only withhold on amount exceeding threshold
  - `round_off_tax_amount` — round the withholding
- **Lower Deduction Certificate (LDC)** support — reduces withholding rate for specific suppliers
- Three withholding handlers: for Journal Entries, Payment Entries, and Sales Invoices

### Tax Withholding Entry
- Child table embedded in JE, PE, SI
- Tracks: `taxable_amount`, `withholding_amount`, `tax_rate`, `tax_withholding_category`
- Status: Settled, Under Withheld, Over Withheld, Duplicate, Cancelled
- Links taxable document to withholding document
- Validates withholding amount matches rate * taxable_amount (within 0.5 tolerance)

### Tax Rule
- Auto-selects tax template based on: customer/supplier, tax category, billing/shipping address, item group

### Tax Category
- Groups transactions for tax treatment differentiation
- Used by Tax Rule for template selection

---

## 9. Financial Reports

### Shared Report Engine
- Period list generator — generates period ranges (Monthly, Quarterly, Half-Yearly, Yearly)
- Core data fetcher:
  1. Get accounts — fetch all accounts for root_type, ordered by tree position (lft)
  2. Filter accounts — build tree structure with accounts_by_name and parent_children_map
  3. Get GL entries by account — query GL entries grouped by account
  4. Calculate values — accumulate debit-credit into period buckets
  5. Accumulate values into parents — roll up child values into parent groups
  6. Prepare data — format output rows with sign correction (Credit accounts show positive)
  7. Filter out zero-value rows — remove empty accounts (unless show_zero_values)
  8. Add total row — summary row
- **Account Closing Balance optimization**: if Period Closing Voucher exists, reads pre-computed balances from Account Closing Balance table instead of scanning all GL entries

### Profit and Loss Statement
- Fetches data twice: root_type=Income (Credit) + root_type=Expense (Debit)
- Net profit/loss = Income total - Expense total per period
- Excludes Period Closing Voucher entries
- Views: Standard, Growth (period-over-period %), Margin (% of income)

### Balance Sheet
- Fetches data three times: Asset (Debit) + Liability (Credit) + Equity (Credit)
- Includes all historical data (not limited to current fiscal year)
- Provisional profit/loss = Assets - (Liabilities + Equity) per period
- Detects unclosed fiscal years via opening balance check
- Shows "Unclosed Fiscal Years Profit/Loss" if opening balance does not zero out

### Trial Balance
- Shows all accounts with: opening_debit, opening_credit, debit, credit, closing_debit, closing_credit
- Filtered by fiscal year, date range
- Uses same GL-entries-by-account infrastructure

### Cash Flow Statement
- Three sections: Operating, Investing, Financing activities
- Based on indirect method (starts with net profit, adjusts for non-cash items)
- Uses account_types to classify: Depreciation, Fixed Asset, etc.

### General Ledger Report
- Detailed transaction-level report
- Filters: account, party, voucher type, date range, cost center, project

### Other Reports
- Accounts receivable / accounts payable — aging analysis
- Budget variance report — budget vs actual comparison
- Gross profit — item-level margin analysis
- Financial ratios — computed financial ratios
- Profitability analysis — cost center / project profitability
- Trial balance for party — per-customer/supplier trial balance
- Custom financial statement — user-defined report layouts via financial report template

---

## 10. Budget System

### Budget Record
- `budget_against`: Cost Center or Project (or any Accounting Dimension)
- `account` — the expense account being budgeted (must be P&L type, ledger)
- `budget_amount` — total budget allocation
- `from_fiscal_year` / `to_fiscal_year` — budget period (can span multiple years)
- Budget distribution child table — period-by-period allocation (Monthly/Quarterly/Half-Yearly/Yearly)
- `distribute_equally` — auto-calculate equal distribution
- Actions on budget exceeded:
  - `action_if_annual_budget_exceeded`: Stop | Warn | Ignore
  - `action_if_accumulated_monthly_budget_exceeded`: Stop | Warn | Ignore
  - Separate actions for Material Request and Purchase Order stages
- `applicable_on_*` flags: booking_actual_expenses, material_request, purchase_order
- Duplicate validation: same account + dimension + overlapping fiscal years = error

### Budget Validation Flow
- Called on every GL posting
- Checks if any active Budget exists for the account + dimension combination
- Two-level check:
  1. **Annual**: total actual expense vs annual budget amount
  2. **Accumulated Monthly**: actual expense through posting month vs cumulative budget distribution
- Get actual expense — sums GL entries (debit - credit) for account + dimension + date range
- Get requested amount — uncommitted Material Requests
- Get ordered amount — unbilled Purchase Orders
- Exception budget approver role — company-level role override (downgrades Stop to Warn)
- Budget validation controller validates at GL map level

### Budget Variance Report
- Shows budget vs actual by dimension, account, and period
- Builds nested structure: dimension -> account -> fiscal_year -> month -> {budget, actual}

---

## 11. Asset Accounting

### Asset Record
- `item_code` — linked Item (must be flagged as fixed asset)
- `asset_category` — determines depreciation accounts
- `purchase_amount`, `additional_asset_cost`, `total_asset_cost`
- `opening_accumulated_depreciation`, `value_after_depreciation`
- Finance books child table — multiple depreciation books per asset
- `depreciation_method`: Straight Line, Double Declining Balance, Written Down Value, Manual
- `frequency_of_depreciation` — months between depreciation entries
- `total_number_of_depreciations` — total scheduled entries
- `calculate_depreciation` flag — enables auto-depreciation
- Status lifecycle: Draft -> Submitted -> Partially Depreciated -> Fully Depreciated -> Sold/Scrapped

### Asset Category to Account Mapping
- Per-company mapping child table:
  - `fixed_asset_account` — where the asset sits on BS
  - `accumulated_depreciation_account` — contra-asset
  - `depreciation_expense_account` — P&L expense
  - `capital_work_in_progress_account` — CWIP staging

### Asset Depreciation Schedule
- Separate record linked to Asset
- Schedule child table — each row: `schedule_date`, `depreciation_amount`, `accumulated_depreciation_amount`, `journal_entry` (filled after posting)
- One schedule per finance book
- Depreciation schedule controller handles calculation methods:
  - **Straight Line**: (cost - salvage) / useful_life, optionally pro-rated
  - **Double Declining Balance**: 2/useful_life * book_value each period
  - **Written Down Value**: rate * book_value (reducing balance)
  - **Manual**: user-specified amounts per period
- `daily_prorata_based` — pro-rate partial periods by actual days
- `shift_based` — adjust depreciation by shift factor

### Depreciation Posting
- Scheduled job processes all due depreciation entries
- Queries assets with auto-depreciation enabled, submitted status, and schedule rows with no journal entry and schedule_date <= current date
- Creates Journal Entry:
  - DR: Depreciation Expense Account (P&L)
  - CR: Accumulated Depreciation Account (BS)
  - Cost center from asset or company default
  - Voucher type = "Depreciation Entry"
- Catch-up depreciation to disposal date when asset is disposed
- Error handling: failed assets logged, notification sent to accounts managers

### Asset Disposal
- Creates Journal Entry with voucher_type "Asset Disposal"
- GL entries for gain/loss on disposal:
  - DR: Accumulated Depreciation (to zero it out)
  - DR/CR: Disposal Account (gain or loss)
  - CR: Fixed Asset Account (to remove asset)

### Asset Capitalization
- Capitalize stock items or services into a new asset
- Creates GL entries moving cost from expense/stock to fixed asset account

---

## 12. Multi-Currency

### Account-Level Currency
- Each Account has `account_currency` — transactions must match
- GL Entry stores amounts in three currencies:
  1. **Account currency**: debit_in_account_currency, credit_in_account_currency
  2. **Company currency**: debit, credit (converted at transaction exchange rate)
  3. **Reporting currency**: debit_in_reporting_currency, credit_in_reporting_currency (for group reporting)

### Exchange Rate Sources
- Exchange rate lookup fetches from stored currency exchange records or external APIs
- Rates cached and stored in currency exchange records
- `source_exchange_rate` / `target_exchange_rate` on Payment Entry

### Exchange Rate Revaluation
- Revalues foreign-currency denominated account balances at period-end rates
- Finds accounts with currency != company currency, computes gain/loss
- `gain_loss = new_balance_in_base_currency - balance_in_base_currency`
- Generates Journal Entry with voucher_type "Exchange Rate Revaluation"
- Splits into gain_loss_booked (zero-balance accounts) and gain_loss_unbooked (open balances)
- Posts to company's unrealized exchange gain/loss account

### Exchange Gain/Loss on Payment
- Auto-created when payment settles invoice at different exchange rate
- JE voucher_type = "Exchange Gain Or Loss"

---

## 13. Accounting Dimensions

### Custom Accounting Dimensions
- Any existing record type can be linked as an accounting dimension (e.g., Department, Branch)
- Cannot use: Cost Center, Project, Company, Account, Finance Book (these are built-in)
- Creates custom fields on all accounting transaction types + GL Entry
- Dimension defaults child table — per-company default values, mandatory flags

### Dimension Enforcement
- `mandatory_for_pl` / `mandatory_for_bs` — require dimension on P&L or BS accounts
- Dimension filter — restrict which dimension values are allowed per account
- Offsetting entries: if a GL map has multiple dimension values, auto-creates balancing entries

### Built-in Dimensions
- `cost_center` — always present, mandatory for P&L, tree-structured
- `project` — optional, links to project record
- `finance_book` — for parallel books (e.g., tax vs GAAP depreciation)

---

## 14. Period Closing & Fiscal Year

### Fiscal Year
- `from_date` to `to_date` range, company-specific
- Fiscal year lookup returns (name, start_date, end_date)
- Results cached for performance

### Period Closing Voucher
- Closes P&L accounts for a period:
  1. Reverses all P&L account balances to zero
  2. Posts net amount to a designated closing account (usually Retained Earnings)
- Creates Account Closing Balance records for fast balance lookups
- After PCV, no entries allowed before `period_end_date`
- Process runs asynchronously, processing day-by-day

### Accounting Period
- Defines closed periods where specific document types cannot be created
- Closed document list specifies which types are blocked
- `exempted_role` — role that can override the period lock
- Checked during accounting period validation in the GL engine

### Account Freezing
- Company-level `accounts_frozen_till_date` — blocks all GL posting before this date
- `role_allowed_for_frozen_entries` — override role
- Per-account `freeze_account` flag — blocks individual accounts

---

## 15. Reconciliation

### Payment Reconciliation Tool
- Matches unallocated payments/advances to outstanding invoices
- Works through Payment Ledger Entry
- Finds invoices with positive outstanding
- Allocation creates new Payment Ledger Entries linking payment to invoice
- Auto-creates exchange gain/loss JE if currencies differ

### Bank Reconciliation Tool
- Matches bank statement transactions to system entries
- Bank transaction record holds imported bank statement lines
- Fields: date, deposit/withdrawal, description, reference_number, unallocated_amount
- Matching creates links between bank transactions and GL vouchers
- Sets `clearance_date` on matched entries

### Unreconcile Payment
- Reverses reconciliation allocations

---

## 16. Key Utility Functions

### Balance & Fiscal Year
- Get balance on account as of date — supports group accounts (uses lft/rgt nested set), cost center filtering, finance book
- Get fiscal year for a date and company -> (name, start_date, end_date)
- Get all matching fiscal years for a date

### Payment Ledger
- Create payment ledger entry from GL entries for Receivable/Payable
- Update voucher outstanding — recalculates outstanding via payment ledger query
- Payment ledger query builder — complex query builder for outstanding calculations

### GL Operations
- Compare stock ledger vs GL balance for an account
- Get account currency — cached lookup
- Cancel exchange gain/loss journal — cancels linked FX journals
- Create gain/loss journal — creates exchange gain/loss JE

---

## 17. Accounts Controller Base Class

### Key Methods
- `validate()` chain includes: party validation, pricing rules, tax calculation, payment schedule
- `make_gl_entries()` — template method, delegates to child class for GL map construction
- `get_gl_dict(args)` — constructs GL entry dict with company, posting_date, fiscal_year, dimensions
- `set_amounts_in_company_currency()` — applies exchange rate conversion
- `validate_account_head()` — ensures account belongs to correct company
- `set_payment_schedule()` — generates payment terms rows
- `calculate_taxes_and_totals()` — delegates to taxes and totals controller
- `repost_accounting_entries()` — re-creates GL entries (used after reconciliation adjustments)
- `check_if_fields_updated()` — detects changes that require GL reposting

### Payment Terms
- Payment schedule child table on invoices
- Generated from payment terms template linked to customer/supplier
- Each row: due_date, payment_amount, discount terms
- Drives aging calculations and dunning

---

## 18. Deferred Revenue & Expense

### Mechanism
- Items with `enable_deferred_revenue` or `enable_deferred_expense`
- Service period: `service_start_date` to `service_end_date`
- Revenue/expense recognized proportionally over service period
- Journal Entries created by scheduled job with voucher_type "Deferred Revenue" or "Deferred Expense"
- Books from deferred account (BS) to income/expense account (P&L) monthly

---

## 19. Finance Book

### Purpose
- Parallel accounting books — same transaction, different GL treatment
- Common use: GAAP book vs Tax book (different depreciation methods)
- `finance_book` field on GL Entry filters which book an entry belongs to
- Company has `default_finance_book` — entries without explicit book use this
- Reports filter by finance_book; `include_default_book_entries` option merges default + specific

---

## 20. Cost Center Allocation

### Mechanism
- Defines how to split a cost center's expenses across sub-cost-centers
- Main cost center splits by percentage to child cost centers
- `valid_from` date — enables time-based allocation changes
- Applied during GL map processing — before GL entries are saved
- Round-off entries go to first allocation target

---

## 21. Key Scheduled Jobs

- Post depreciation entries — auto-book due depreciation (daily)
- Book deferred income and expense — deferred revenue/expense recognition
- Process payment entries — process payment reconciliation queue
- Rename GL/stock entry records — rename temporarily-named entries
- Auto-create exchange rate revaluation — if configured

---

## 22. Subscription & Recurring

### Subscription
- Recurring billing: generates invoices on schedule
- Subscription plan — defines item, price, billing interval
- Status: Active, Past Due Date, Cancelled, Unpaid

---

## 23. Regional / Localization

### Structure
- Country-specific tax logic modules
- Decorator-based hooks for regional overrides
- Country COA templates available
- Tax withholding groups support region-specific rates

---

## 24. Invoice Discounting

- Factor/discount invoices with financial institutions
- Moves receivables to short-term loan account
- GL entries: DR Discounted Invoice Account, CR Receivable
- Special handling in outstanding calculations (excluded from normal outstanding)

---

## 25. Share Management

- Share transfer records — buy/sell/transfer company shares
- Share balance — current holdings per shareholder
- Ledger and balance reports
- Shareholder records with contact details

---

## 26. Important Patterns & Gotchas

### Naming
- GL Entry uses hash-based temp name for fast insertion, renamed later by scheduled job
- Account name format: `{number} - {name} - {company_abbr}`

### Cancellation
- Never deletes — always creates reverse entries
- `is_cancelled` flag on GL entries
- Immutable ledger option creates new entries instead of flagging

### Reposting
- Accounting ledger repost — re-creates GL entries for modified documents
- Payment ledger repost — re-creates Payment Ledger entries
- Controlled by repost settings — whitelist of allowed voucher types

### Multi-Company
- Child companies inherit COA from parent (root_company)
- Account creation cascades to child companies
- Override flag allows account creation against child company
- Inter-company journal entries with linked references

### Performance
- Account tree uses `lft`/`rgt` nested set for fast subtree queries
- GL Entry indexes on (voucher_type, voucher_no), (posting_date, company), (party_type, party)
- Account Closing Balance pre-computes balances at PCV dates
- Idle system check — blocks account rename/merge if GL entries modified in last 5 minutes

### Validation Order (GL Posting)
```
Budget -> Accounting Period -> Disabled Accounts -> Cost Center Allocation ->
Merge Similar -> Toggle Debit/Credit -> CWIP Check -> Debit/Credit Difference ->
Freezing Date -> Period Closing Voucher -> Dimension Filters -> Save
```
