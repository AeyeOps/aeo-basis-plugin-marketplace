---
name: chart-of-accounts
description: >
  Contains verified C-corp COA procedures, account numbering decision trees, and Form 1120
  tax-line mappings that produce more precise answers than reasoning from general training
  alone. Account type taxonomy (1xxx-9xxx), hierarchical grouping, book-tax differences,
  equity accounts (APIC, retained earnings, treasury stock), and COA restructuring. Consult
  when choosing account numbers, classifying accounts, mapping GL to tax lines, or asking
  "what account for X."
---

# Chart of Accounts

Structural foundation for GL account design and classification in C-corporations under
US GAAP. Covers the five root account types, standard numbering ranges, hierarchical
grouping, C-corp equity accounts, and Form 1120 tax-line mapping.

## Account Type Taxonomy

Every GL account belongs to exactly one of five root types that determine which financial
statement it appears on and its normal balance direction.

**Balance Sheet accounts** (cumulative, carry forward):
- **Asset** — Normal debit. Current (cash, receivables, inventory, prepaids) and non-current (fixed assets, intangibles, investments).
- **Liability** — Normal credit. Current (AP, accrued expenses, current debt, unearned revenue) and non-current (long-term debt, deferred tax liabilities).
- **Equity** — Normal credit. Common stock, APIC, retained earnings, treasury stock (contra-equity, debit), AOCI.

**Income Statement accounts** (periodic, reset at year-end close):
- **Revenue** — Normal credit. Gross revenue, returns/allowances (contra-revenue, debit), other income.
- **Expense** — Normal debit. COGS, operating expenses, depreciation, interest expense, tax expense.

Contra accounts reverse the normal balance of their parent type (accumulated depreciation
is contra-asset with credit balance; sales returns is contra-revenue with debit balance).

## Numbering Convention

Standard 4-digit ranges for C-corporations. Leave gaps between active accounts for
future inserts — at least 10 between accounts, 100 between categories.

- **1000–1999 Assets** — 1000s cash/equivalents, 1100s investments, 1200s receivables, 1300s inventory, 1400s prepaids, 1500–1699 fixed assets, 1700s accumulated depreciation, 1800s intangibles, 1900s other non-current
- **2000–2999 Liabilities** — 2000s AP, 2100s accrued liabilities, 2200s credit cards/short-term notes, 2300s current portion LTD, 2400s unearned revenue, 2500s payroll liabilities, 2600–2799 long-term debt, 2800s deferred tax liabilities
- **3000–3999 Equity** — 3000s common stock, 3100s APIC, 3200s retained earnings, 3300s treasury stock, 3400s AOCI, 3500s dividends declared
- **4000–4999 Revenue** — 4000s gross revenue, 4100s returns/allowances/discounts, 4200s other operating income, 4800s interest/dividend income, 4900s non-operating income/gains
- **5000–5999 COGS** — 5000s direct materials, 5100s direct labor, 5200s subcontractors, 5300s overhead
- **6000–7999 Operating Expenses** — 6000s compensation, 6100s rent/occupancy, 6200s professional fees, 6300s insurance, 6400s office/admin, 6500s technology, 6600s marketing, 6700s travel, 6800s depreciation/amortization, 6900s repairs, 7000s utilities, 7100s taxes/licenses, 7200s bad debt, 7300s contributions, 7400s non-deductible penalties/fines
- **8000–8999 Other Income/Expense** — 8000s interest expense, 8100s gain/loss on disposal, 8200s unrealized gain/loss, 8300s FX gain/loss
- **9000–9999 Income Tax** — 9000s federal current, 9100s state current, 9200s deferred tax expense/benefit

Group parent/child accounts numerically (1500 PP&E group → 1510 Land, 1520 Buildings).
Mirror contra accounts near their parent (1520 Buildings → 1720 Accum Depr – Buildings).
Separate deductible from non-deductible expenses at the account level to simplify M-1
reconciliation.

## Hierarchical Structure

Accounts form a tree: group (parent) accounts aggregate child balances for reporting;
only leaf (detail) accounts receive postings. 3–4 levels is typical for mid-market C-corps
(root type → classification → sub-group → detail).

```
Assets [group — no posting]
├── Current Assets [group]
│   ├── Cash and Equivalents [group]
│   │   ├── 1010 Operating Checking [leaf — postable]
│   │   ├── 1020 Payroll Checking [leaf]
│   │   └── 1030 Money Market [leaf]
│   └── Accounts Receivable [group]
│       ├── 1200 Trade Receivables [leaf]
│       └── 1210 Allowance for Doubtful Accounts [leaf, contra]
├── Fixed Assets [group]
│   ├── 1510 Land, 1520 Buildings, 1530 Equipment [leaves]
│   └── 1720 Accum Depr – Buildings, 1730 Accum Depr – Equipment [contra leaves]
└── ...
```

## C-Corporation Equity Accounts

- **3000 Common Stock** — Par value × shares issued. For no-par stock, entire issuance price here or APIC per board resolution.
- **3100 APIC** — Excess over par from issuance. Also receives stock compensation (ASC 718).
- **3200 Retained Earnings** — Cumulative net income less cumulative dividends. Receives year-end close entry.
- **3300 Treasury Stock** — Contra-equity (debit). Shares repurchased but not retired, typically at cost method.
- **3400 AOCI** — Unrealized items bypassing P&L: FX translation, available-for-sale securities, pension adjustments, cash flow hedges.
- **3500 Dividends Declared** — Temporary equity. Debited on declaration, closed to retained earnings at year-end.

**Year-end close**: Revenue (4xxx) and expense (5xxx–9xxx) accounts net to Income Summary, which transfers to Retained Earnings (3200). Dividends Declared (3500) also closes to Retained Earnings.

## Form 1120 Tax-Line Mapping

Design the COA so each account maps to a specific Form 1120 line. Key mappings:

**Income** — 4000 gross revenue → Line 1a, 4100 returns → Line 1b, 4800 interest income → Line 5, 4810 dividends → Line 10 (Schedule C for DRD), 4900 rents → Line 6, 8100 asset sale gains → Line 8 (Schedule D / Form 4797)

**Deductions** — 6000 officer comp → Line 12, 6010 wages → Line 13, 6300 repairs → Line 14, 7200 bad debts → Line 15, 6100 rents → Line 16, 7100 taxes/licenses → Line 17, 8000 interest → Line 18, 7300 contributions → Line 19 (10% limit), 6800 depreciation → Line 20 (Form 4562), 6900–7999 other → Line 26

**Non-deductible (M-1/M-3 add-backs)** — 7400 penalties (IRC §162(f)), 7410 50% meals (IRC §274), 7420 entertainment (100% post-TCJA), 9000 federal income tax, 8200 unrealized gain/loss

## Book-Tax Differences

**Timing** (deferred tax — reverse in future periods): depreciation (GAAP straight-line vs. tax MACRS/bonus per IRC §168), revenue recognition (ASC 606 vs. constructive receipt), bad debt (allowance vs. direct write-off per IRC §166), prepaids (matching vs. 12-month rule), deferred revenue (GAAP vs. Rev. Proc. 2004-34), Section 179 expensing.

**Permanent** (never reverse): meals 50% disallowed (IRC §274), entertainment 100% disallowed post-TCJA, fines/penalties (IRC §162(f)), federal income tax, tax-exempt interest, DRD 50/65/100% (IRC §243), officer life insurance premiums (IRC §264), political contributions (IRC §162(e)).

Separate accounts for items with different tax treatment at the GL level — deductible meals vs. entertainment in separate accounts, officer comp separate from staff wages, depreciation by asset class if book/tax methods differ, interest expense by purpose if §163(j) applies.

## Special Account Roles

- **Bank/Cash** — One GL account per physical bank account. Reconciled monthly to statements.
- **AR/AP** — Controlled by sub-ledgers. GL shows aggregate; individual balances in sub-ledger.
- **Undeposited Funds** — Staging for receipts not yet deposited. Clears on deposit.
- **Suspense/Clearing** — Temporary holding. Any period-end balance means incomplete transactions.
- **Intercompany** — Parent-subsidiary transactions. Must net to zero across consolidated group.
- **Retained Earnings** — System account for year-end close. Never post directly during the year except prior-period adjustments.
- **Opening Balance Equity** — Initial setup balancing. Should be zero after setup is complete.

## COA Design Principles

1. **Start with the tax return** — Form 1120 lines define minimum granularity. Every applicable return line gets at least one GL account.
2. **Match reporting needs** — Management, tax, audit, and regulatory audiences. Create sub-accounts for management detail (6610 Digital Marketing, 6620 Print) rather than relying on tags alone.
3. **Keep it lean** — Only accounts with distinct reporting or compliance purpose. Signs of bloat: accounts that never receive postings, accounts always combined in reports.
4. **Plan for growth** — Numerical gaps (10+ between accounts, 100+ between categories). A COA that requires renumbering will accumulate workarounds.

## Supporting References

Read these for deeper detail on specific topics:

- `references/ccorp-coa-design.md` — Expanded version of the content above with full sub-range tables, detailed tree examples, complete Form 1120 mapping tables, and extended book-tax difference analysis with IRC citations. Read when you need the detailed lookup tables or are building a COA from scratch.
- `references/accounting-system-architecture.md` — Accounting system architecture showing how COA concepts map to system implementation. Read section 2 for the account data model (root_type, report_type, account_type fields, nested set tree), section 3 for GL entry validation rules, section 9 for how the financial report engine uses account hierarchy, section 14 for period closing mechanics.
- `references/guide-cpa-expert.md` — Authoritative source bibliography. Read §2 for FASB ASC topic index and Big 4 technical guides, §3 for IRC section references and IRS publications (Pub 542, 535, 946, Form 1120 instructions).

## Cross-Plugin References

For platform-specific COA implementation:
- Invoke `qbo-integration:qbo-coa` for QBO account creation, detail types, tax-line mapping within QBO, special QBO accounts and pitfalls
- Invoke `qbo-integration:qbo-api` for programmatic COA management via the QBO API

For related foundation knowledge:
- Invoke `accounting-foundation:categorization-rules` for transaction-to-account routing logic
- Invoke `accounting-foundation:financial-statements` for presentation requirements that depend on account classification
- Invoke `accounting-foundation:entity-profile` for corporate entity details that affect COA design (entity type, fiscal year, ownership structure)

## Cross-Plugin Consumers

- `bookkeeping:transaction-processing` — account classification for categorization
- `tax-prep:form-1120-prep` — tax-line mappings for return preparation
- `tax-prep:tax-planning` — book-tax difference analysis
- `financial-planning:budgeting-forecasting` — account hierarchy for budget structure
