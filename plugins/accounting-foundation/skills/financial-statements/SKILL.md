---
name: financial-statements
description: >
  Contains verified C-corp year-end close sequence with closing journal entries
  (revenue/expense to Income Summary to Retained Earnings), Form 1120 tax-line mapping
  tables (Schedule M-1 book-tax differences, Schedule L balance sheet, Schedule M-2
  retained earnings rollforward), and ratio analysis formulas (current, quick, DSO, DPO,
  gross/operating/net margin, ROE, ROA, ROIC, D/E, interest coverage). Covers GAAP
  classified balance sheet presentation, multi-step P&L structure, indirect-method cash
  flow construction, stockholders' equity statement, trial balance validation, common-size
  analysis, variance analysis, comparative reporting, materiality thresholds, notes
  disclosures, and SSARS compilation/review basis. Consult when generating or reading
  financial statements, preparing year-end closing entries, computing financial ratios,
  performing common-size or variance analysis, investigating trial balance anomalies
  (Opening Balance Equity, Income Summary, suspense accounts), tracing interstatement
  linkages (net income to equity to cash flow), mapping GL accounts to Form 1120 lines,
  answering "how did we do this month," or explaining GAAP presentation requirements
  to bankers or auditors.
---

# Financial Statements

Operational reference for producing, structuring, reading, and analyzing C-corporation
financial statements under US GAAP. Covers the four primary statements, trial balance
mechanics, GAAP presentation rules, analytical techniques, and the close-to-statements
workflow.

## The Four Statements

US GAAP requires a complete set of financial statements to include: (1) balance sheet
(statement of financial position), (2) income statement (statement of operations),
(3) statement of cash flows, and (4) statement of stockholders' equity. Notes to the
financial statements are an integral part of the presentation. The trial balance is the
working document that feeds all four statements.

## Trial Balance — The Starting Point

The trial balance lists every GL account with its debit or credit balance as of a date.
It is the raw data from which all statements are derived.

**Structure**: Each row shows account number, account name, debit balance, and credit
balance. Total debits must equal total credits — if they do not, a posting error exists.

**Types**:
- **Unadjusted TB** — balances before adjusting entries (accruals, deferrals, depreciation)
- **Adjusted TB** — balances after all adjusting entries; this is what feeds the statements
- **Post-closing TB** — balances after year-end close; only balance sheet accounts remain (all temporary accounts are zero)

**Validation checks**:
- Debits equal credits (fundamental)
- No negative balances on accounts that should not have them (negative cash, negative AR without offset)
- No balance in suspense/clearing accounts (indicates incomplete transactions)
- Opening Balance Equity is zero (any balance means setup error)
- Income Summary is zero outside of year-end close

**Accounting basis matters**: The TB must be generated on the entity's elected accounting
method (accrual or cash). Accrual is standard for C-corps under GAAP. Cash-basis
financial statements are acceptable under OCBOA but do not conform to GAAP.

## Balance Sheet (Statement of Financial Position)

Reports assets, liabilities, and equity at a point in time. The accounting equation —
Assets = Liabilities + Equity — must hold exactly.

### Classification

**Current vs non-current** distinction drives the classified balance sheet format required
under GAAP:

**Current assets** (expected to convert to cash or be consumed within one year or operating
cycle, whichever is longer):
- Cash and cash equivalents (1000s)
- Short-term investments / marketable securities (1100s)
- Accounts receivable, net of allowance (1200s)
- Inventory (1300s)
- Prepaid expenses (1400s)
- Other current assets

**Non-current assets**:
- Property, plant, and equipment, net of accumulated depreciation (1500s/1700s)
- Right-of-use assets (ASC 842 — operating and finance leases)
- Intangible assets, net of accumulated amortization (1800s)
- Long-term investments (1900s)
- Deferred tax assets
- Other non-current assets

**Current liabilities** (due within one year or operating cycle):
- Accounts payable (2000s)
- Accrued liabilities — wages, taxes, interest, utilities (2100s)
- Short-term debt / current portion of long-term debt (2200s/2300s)
- Unearned revenue / contract liabilities (2400s)
- Current portion of lease liabilities (ASC 842)
- Payroll liabilities (2500s)
- Income taxes payable

**Non-current liabilities**:
- Long-term debt (2600s–2700s)
- Long-term lease liabilities (ASC 842)
- Deferred tax liabilities (2800s)
- Other long-term liabilities

**Stockholders' equity** (C-corp specific):
- Common stock at par (3000)
- Additional paid-in capital / APIC (3100)
- Retained earnings (3200)
- Treasury stock — contra-equity, debit balance (3300)
- Accumulated other comprehensive income / AOCI (3400)

### Presentation Order

Assets: liquidity order (most liquid first). Liabilities: maturity order (earliest due
first). Equity: contributed capital first, then earned capital (retained earnings), then
adjustments (AOCI, treasury).

### Balance Sheet Checks

- Total assets equals total liabilities plus equity (the equation)
- Retained earnings = prior-year retained earnings + current-year net income - dividends
- If prior year's ending balance sheet does not match current year's beginning, investigate (restatement, missed prior-period adjustment, or year-end close error)
- Accumulated depreciation should move in one direction (increasing) unless an asset was disposed
- Treasury stock should only appear if shares were repurchased

## Income Statement (Statement of Operations / P&L)

Reports revenue, expenses, and net income for a period. Derived from all temporary accounts
(4xxx through 9xxx ranges).

### Structure — Multi-Step Format

The multi-step format is standard for C-corps and provides more analytical value than
single-step:

```
Revenue (4000s)
  Less: Returns and allowances (4100s)
= Net Revenue
  Less: Cost of goods sold / cost of revenue (5000s)
= Gross Profit
  Less: Operating expenses (6000s–7999)
    Compensation and benefits
    Rent and occupancy
    Professional fees
    Depreciation and amortization
    ...
= Operating Income (EBIT proxy)
  Plus/Minus: Other income / expense (8000s)
    Interest expense
    Gain/loss on asset disposal
    Unrealized gain/loss
    FX gain/loss
= Income Before Income Tax
  Less: Income tax expense (9000s)
    Federal current
    State current
    Deferred tax expense/benefit
= Net Income
```

### Key Metrics Derived

- **Gross margin** = Gross Profit / Net Revenue
- **Operating margin** = Operating Income / Net Revenue
- **Net margin** = Net Income / Net Revenue
- **EBITDA** = Operating Income + Depreciation + Amortization (non-GAAP but widely used)

### Income Statement Checks

- Revenue recognition timing aligns with ASC 606 (see `bookkeeping:monthly-close` for ASC 606 recording patterns)
- COGS only includes direct costs (materials, direct labor, manufacturing overhead); selling and administrative costs belong in operating expenses
- Depreciation expense agrees with the depreciation schedule
- Income tax expense includes both current and deferred components (see `financial-planning:tax-provision` for ASC 740)
- Officer compensation is segregated from non-officer wages (required for Form 1120 Line 12 vs Line 13)

## Statement of Cash Flows

Reports cash inflows and outflows for a period, classified into three activities. Required
under ASC 230.

### Indirect Method (most common for C-corps)

Starts with net income and adjusts for non-cash items and working capital changes:

**Operating activities**:
- Start: Net income
- Add back non-cash expenses: depreciation, amortization, deferred tax expense, loss on asset disposal, stock compensation
- Subtract non-cash income: gain on asset disposal, unrealized gains
- Adjust for working capital changes:
  - Increase in AR = cash outflow (subtract)
  - Decrease in AR = cash inflow (add)
  - Increase in inventory = cash outflow (subtract)
  - Increase in AP = cash inflow (add)
  - Increase in accrued liabilities = cash inflow (add)
  - Decrease in deferred revenue = cash outflow (subtract)
  - Increase in deferred revenue = cash inflow (add)

**Investing activities**:
- Purchase of property/equipment (outflow)
- Proceeds from sale of assets (inflow)
- Purchase of investments (outflow)
- Proceeds from investment maturities/sales (inflow)
- Acquisitions of businesses, net of cash acquired (outflow)

**Financing activities**:
- Proceeds from debt issuance (inflow)
- Repayment of debt principal (outflow)
- Proceeds from stock issuance (inflow)
- Treasury stock purchases (outflow)
- Dividends paid (outflow)
- Payment of finance lease obligations (outflow)

**Reconciliation**: Net change in cash = Operating + Investing + Financing. Beginning
cash + net change = ending cash. Ending cash must equal the balance sheet cash balance.

### Direct Method

Reports actual cash receipts and payments for operating activities (cash collected from
customers, cash paid to suppliers, cash paid for wages, etc.). Provides more information
but is rarely used in practice because it requires detailed cash tracking. If used, a
reconciliation of net income to operating cash flow must still be provided.

### Cash Flow Checks

- Ending cash ties to balance sheet cash
- Depreciation add-back equals depreciation on P&L
- Gain/loss on disposal agrees with asset disposal entries
- Debt proceeds/repayments agree with debt rollforward
- Dividends paid agrees with dividends declared less any accrued but unpaid

## Statement of Stockholders' Equity

Reports changes in each equity component during the period. For C-corps, the columns
typically are: Common Stock, APIC, Retained Earnings, Treasury Stock, AOCI, and Total.

**Row structure** (for each period presented):
- Beginning balance
- Net income (flows to Retained Earnings)
- Other comprehensive income/loss (flows to AOCI)
- Stock issuance (Common Stock and APIC)
- Stock repurchase (Treasury Stock)
- Stock-based compensation (APIC)
- Dividends declared (reduces Retained Earnings)
- Prior-period adjustments (if any)
- Ending balance

**Consistency check**: Ending Retained Earnings on this statement must equal Retained
Earnings on the balance sheet. Ending total equity must equal total equity on the balance
sheet.

## Year-End Close — Producing the Statements

The close process converts a year of activity into clean financial statements. The
sequence matters.

### Close Sequence

1. **Complete all adjusting entries** — accruals, deferrals, depreciation, inventory adjustments, lease recognition (ASC 842), revenue recognition (ASC 606)
2. **Run adjusted trial balance** — verify debits equal credits, scan for anomalies
3. **Generate pre-close financial statements** — P&L, BS, cash flow, equity statement for review
4. **Manager/partner review** — analytical review, ratio computation, variance investigation
5. **Post closing entries** — zero out all temporary accounts (revenue and expense) by transferring net income to Retained Earnings via Income Summary; close Dividends Declared to Retained Earnings
6. **Run post-closing trial balance** — only balance sheet accounts should have balances; all temporary accounts are zero
7. **Lock the period** — prevent accidental posting to closed periods

### Closing Journal Entry

```
DR  Revenue accounts (4xxx)        [total revenue]
CR  Expense accounts (5xxx-9xxx)   [total expenses]
CR  Income Summary                 [net income]

DR  Income Summary                 [net income]
CR  Retained Earnings (3200)       [net income]

DR  Retained Earnings (3200)       [dividends]
CR  Dividends Declared (3500)      [dividends]
```

If net loss: the debits and credits on Income Summary reverse direction.

## GAAP Presentation Requirements

### Comparative Presentation

GAAP requires (or strongly encourages) presenting two periods side by side — current year
and prior year. This enables trend identification and is expected by auditors, bankers,
and investors.

### Materiality

Immaterial line items may be combined. The test: would omitting or misstating the item
influence the economic decisions of users? Common materiality thresholds for small-mid
C-corps: 5% of net income or 1-2% of total assets, whichever is more relevant.

### Notes to Financial Statements

Required disclosures include:
- Summary of significant accounting policies (basis of accounting, revenue recognition, depreciation methods, inventory valuation)
- Related party transactions
- Commitments and contingencies
- Subsequent events (through the date the statements are available to be issued)
- Lease obligations (ASC 842 maturity table)
- Income taxes (current/deferred breakdown, effective rate reconciliation)
- Debt terms and maturity schedule
- Stockholders' equity detail (shares authorized, issued, outstanding)

### Basis of Accounting

- **GAAP** — full US GAAP (accrual basis, all ASC standards). Required for SEC filers, expected by lenders and investors.
- **OCBOA / Special Purpose Frameworks** — cash basis, tax basis, or regulatory basis. Acceptable for private companies that do not need full GAAP. Must disclose the basis prominently.
- **Compilation** — accountant assists in presenting financial statements without providing assurance. Follows SSARS (AR-C 80).
- **Review** — accountant performs analytical procedures and inquiries to provide limited assurance. Follows SSARS (AR-C 90).
- **Audit** — full assurance engagement under GAAS (AU-C standards). Required when mandated by contract, regulation, or lender.

## Analytical Techniques

### Common-Size Analysis

Express every line item as a percentage of a base:
- **Common-size P&L**: each line as % of net revenue
- **Common-size BS**: each line as % of total assets

Enables comparison across periods and across companies of different sizes.

### Ratio Analysis

**Liquidity**:
- Current ratio = Current Assets / Current Liabilities (target: >1.5 for most industries)
- Quick ratio = (Cash + Receivables + Short-term Investments) / Current Liabilities
- Cash ratio = Cash / Current Liabilities

**Profitability**:
- Gross margin = Gross Profit / Net Revenue
- Operating margin = Operating Income / Net Revenue
- Net margin = Net Income / Net Revenue
- ROA = Net Income / Average Total Assets
- ROE = Net Income / Average Stockholders' Equity
- ROIC = NOPAT / Invested Capital

**Efficiency**:
- DSO = (Average AR / Net Revenue) x 365
- DPO = (Average AP / COGS) x 365
- Inventory turnover = COGS / Average Inventory
- Asset turnover = Net Revenue / Average Total Assets

**Leverage**:
- Debt-to-equity = Total Liabilities / Total Equity
- Debt-to-assets = Total Liabilities / Total Assets
- Interest coverage = EBIT / Interest Expense
- Times interest earned = (Net Income + Interest Expense + Tax Expense) / Interest Expense

### Trend Analysis

Compare ratios and line items across 3-5 periods to identify:
- Revenue growth trajectory
- Margin compression or expansion
- Working capital trends (growing DSO = collection slowdown)
- Leverage changes (increasing debt-to-equity = risk indicator)
- Cash flow sustainability (operating cash consistently below net income = accrual quality concern)

### Variance Analysis

Compare actual results to budget, prior period, or forecast:
- Dollar variance = Actual - Budget
- Percentage variance = (Actual - Budget) / Budget
- Investigate variances exceeding materiality threshold (typically 10% or $1,000, whichever is greater)
- Distinguish volume variances from rate/price variances where possible

## Financial Statement Interrelationships

The four statements are interconnected — changes flow between them:

- **Net income** on the P&L flows to the equity statement (retained earnings) and is the starting point for the cash flow statement (indirect method)
- **Ending retained earnings** on the equity statement ties to retained earnings on the balance sheet
- **Ending cash** on the cash flow statement ties to cash on the balance sheet
- **Working capital changes** on the cash flow statement tie to the difference between current-year and prior-year balance sheet items (AR, inventory, AP, accruals)
- **Depreciation** appears on the P&L (expense), cash flow (add-back), and balance sheet (accumulated depreciation)
- **Dividends** appear on the equity statement (reducing retained earnings), cash flow (financing outflow), and balance sheet (reducing cash and equity)

If any of these linkages do not tie, there is an error in the close process.

## Tax Return Connection

Financial statements (book basis) serve as the starting point for Form 1120 preparation:

- **P&L net income** becomes Schedule M-1 Line 1 (or M-3 Part I)
- **Balance sheet** becomes Schedule L (book basis)
- **Retained earnings rollforward** becomes Schedule M-2

Book-tax differences (permanent and temporary) are the bridge between book income and
taxable income. Common differences: depreciation (book straight-line vs tax MACRS), meals
(50% disallowed), entertainment (100% disallowed post-TCJA), penalties (non-deductible),
federal income tax expense (non-deductible), DRD (reduces taxable income). See
`tax-prep:form-1120-prep` for the full reconciliation workflow.

## Supporting References

Read these for deeper detail on specific topics:

- `references/accounting-system-architecture.md` — Accounting system architecture encoding financial report production mechanics. Read section 9 for the financial report engine (P&L, Balance Sheet, Trial Balance, Cash Flow, General Ledger report construction including hierarchical account aggregation, period list generation, GL-to-period bucketing, sign correction for credit-balance accounts, and the Account Closing Balance optimization). Read section 14 for period closing and fiscal year mechanics (Period Closing Voucher that zeros P&L to Retained Earnings, Account Closing Balance pre-computation, accounting period locks, and account freezing). These sections define the mechanical "how" of statement generation from raw GL data.

- `references/guide-cpa-expert.md` — Authoritative source bibliography for GAAP and tax standards. Read section 2 for FASB ASC topic index and Big 4 technical handbooks (KPMG income taxes/consolidation/revenue, Deloitte roadmaps, PwC Viewpoint guides, EY FRDs) — these are the interpretive resources for resolving GAAP presentation questions. Read section 9 for SEC EDGAR as a source of real-world GAAP financial statement examples. Read section 11 for open textbooks covering intermediate financial accounting (Lyryx/Athabasca volumes cover the full Kieso-equivalent curriculum).

- `references/guide-fpa.md` — FP&A resource guide covering financial analysis frameworks and tools. Read section 2 for financial modeling resources (Damodaran 3-statement models, CFI templates, Wall Street Prep variance analysis) — these inform how to read and analyze financial statements, not just produce them. Read section 5 for reporting and dashboard best practices (Vena report type taxonomy, board reporting, KPI selection). Read section 4 for strategic finance resources (cash forecasting methods, capital allocation) that consume financial statement data.

- `references/monthly-close-sop.md` — Step-by-step monthly close procedure that produces the adjusted trial balance feeding financial statements. Read Phase 3 (adjusting entries: accruals, prepaids, depreciation, deferred revenue, inventory adjustments) and Phase 4 (review and reporting: run TB, generate statements, variance analysis, analytical review, compute ratios, prepare close memo). The close SOP is the operational workflow upstream of statement generation — if the close is incomplete, the statements are unreliable.

- `references/ccorp-coa-design.md` — C-corporation chart of accounts design guide. Read section 1 for account type taxonomy and report type derivation (which accounts appear on which statement), section 3 for hierarchical structure (group vs leaf accounts, aggregation rules), section 4 for C-corp equity structure and year-end close mechanics, and section 5 for Form 1120 tax-line mapping that connects financial statements to the tax return.

## Cross-Plugin References

For platform-specific report generation:
- Invoke `qbo-integration:qbo-reporting` for QBO P&L, Balance Sheet, Cash Flow, General Ledger, and Trial Balance API endpoints, report parameters, response parsing, comparative reporting, and Excel export

For related foundation knowledge:
- Invoke `accounting-foundation:chart-of-accounts` for account type taxonomy, numbering convention, hierarchical structure, and Form 1120 tax-line mapping that drives financial statement classification
- Invoke `accounting-foundation:categorization-rules` for transaction classification rules that determine which account (and therefore which statement line) each transaction hits
- Invoke `accounting-foundation:entity-profile` for entity details (fiscal year, entity type, state of incorporation) that affect statement presentation

For downstream consumers:
- Invoke `bookkeeping:monthly-close` for the operational close procedure (adjusting entries, reconciliation, period lock) that produces the adjusted TB feeding statements
- Invoke `bookkeeping:audit-support` for lead schedule preparation, PBC lists, and SSARS compilation/review standards
- Invoke `tax-prep:form-1120-prep` for the book-to-tax reconciliation workflow (M-1/M-3, Schedule L, Schedule M-2) that starts from financial statements
- Invoke `financial-planning:budgeting-forecasting` for budget-vs-actual variance analysis built on statement data
- Invoke `financial-planning:variance-analysis` for KPI computation, management reporting, and industry benchmarking
- Invoke `financial-planning:financial-modeling` for 3-statement financial modeling that projects future financial statements
- Invoke `financial-planning:tax-provision` for ASC 740 income tax provision (current and deferred) that appears on both the P&L and balance sheet
