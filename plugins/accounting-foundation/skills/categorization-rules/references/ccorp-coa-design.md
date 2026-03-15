# C-Corporation Chart of Accounts Design Guide

Operational reference for COA structure, numbering, tax-line mapping, and equity
accounts specific to C-corporations filing Form 1120 under US GAAP.

## Table of Contents

1. [Account Type Taxonomy](#1-account-type-taxonomy)
2. [Numbering Convention](#2-numbering-convention)
3. [Hierarchical Structure](#3-hierarchical-structure)
4. [C-Corporation Equity Structure](#4-c-corporation-equity-structure)
5. [Form 1120 Tax-Line Mapping](#5-form-1120-tax-line-mapping)
6. [Book-Tax Differences](#6-book-tax-differences)
7. [Special Account Types](#7-special-account-types)
8. [COA Design Principles](#8-coa-design-principles)

---

## 1. Account Type Taxonomy

Every GL account belongs to exactly one of five root types. The root type determines
which financial statement the account appears on and its normal balance direction.

**Balance Sheet accounts** (cumulative, carry forward across periods):
- **Asset** — Resources owned or controlled. Normal balance: debit. Sub-classifications: current assets (cash, receivables, inventory, prepaids) and non-current assets (fixed assets, intangibles, investments, other long-term).
- **Liability** — Obligations owed. Normal balance: credit. Sub-classifications: current liabilities (AP, accrued expenses, current portion of debt, unearned revenue) and non-current liabilities (long-term debt, deferred tax liabilities, other long-term).
- **Equity** — Residual ownership interest. Normal balance: credit. For C-corps: common stock, additional paid-in capital (APIC), retained earnings, treasury stock (contra-equity, debit balance), accumulated other comprehensive income (AOCI).

**Income Statement accounts** (periodic, reset to zero at year-end close):
- **Revenue** — Inflows from operations. Normal balance: credit. Includes gross revenue, returns/allowances (contra-revenue, debit), other income, interest/dividend income.
- **Expense** — Costs incurred to generate revenue. Normal balance: debit. Includes COGS, operating expenses, depreciation/amortization, interest expense, tax expense.

### Report Type Derivation

| Root Type | Report | Normal Balance | Increase | Decrease |
|-----------|--------|----------------|----------|----------|
| Asset | Balance Sheet | Debit | Debit | Credit |
| Liability | Balance Sheet | Credit | Credit | Debit |
| Equity | Balance Sheet | Credit | Credit | Debit |
| Revenue | Profit & Loss | Credit | Credit | Debit |
| Expense | Profit & Loss | Debit | Debit | Credit |

Contra accounts reverse the normal balance of their parent type (e.g., accumulated
depreciation is a contra-asset with a credit balance; sales returns is contra-revenue
with a debit balance).

---

## 2. Numbering Convention

A well-designed numbering scheme encodes account type, sub-classification, and hierarchy
in the account number itself. The standard approach for C-corporations:

### Range Assignments

- **1000–1999** — Assets
  - 1000–1099: Cash and cash equivalents
  - 1100–1199: Short-term investments
  - 1200–1299: Accounts receivable
  - 1300–1399: Inventory
  - 1400–1499: Prepaid expenses and other current assets
  - 1500–1699: Fixed assets (property, plant, equipment)
  - 1700–1799: Accumulated depreciation (contra-asset)
  - 1800–1899: Intangible assets and amortization
  - 1900–1999: Other non-current assets, deposits, long-term investments

- **2000–2999** — Liabilities
  - 2000–2099: Accounts payable
  - 2100–2199: Accrued liabilities (wages, taxes, interest)
  - 2200–2299: Credit cards and short-term notes
  - 2300–2399: Current portion of long-term debt
  - 2400–2499: Unearned / deferred revenue
  - 2500–2599: Payroll liabilities (withholding, employer taxes)
  - 2600–2799: Long-term debt (loans, bonds, notes)
  - 2800–2899: Deferred tax liabilities
  - 2900–2999: Other long-term liabilities

- **3000–3999** — Equity
  - 3000–3099: Common stock (par value)
  - 3100–3199: Additional paid-in capital (APIC)
  - 3200–3299: Retained earnings
  - 3300–3399: Treasury stock (contra-equity)
  - 3400–3499: AOCI
  - 3500–3599: Dividends declared (temporary equity, closes to retained earnings)
  - 3900–3999: Prior period adjustments, equity adjustments

- **4000–4999** — Revenue
  - 4000–4099: Gross service/product revenue
  - 4100–4199: Returns, allowances, discounts (contra-revenue)
  - 4200–4299: Other operating income
  - 4800–4899: Interest and dividend income
  - 4900–4999: Other non-operating income, gains

- **5000–5999** — Cost of Goods Sold / Cost of Revenue
  - 5000–5099: Direct materials
  - 5100–5199: Direct labor
  - 5200–5299: Subcontractor / outsourced costs
  - 5300–5399: Manufacturing overhead applied
  - 5900–5999: Other cost of revenue

- **6000–7999** — Operating Expenses
  - 6000–6099: Compensation and benefits
  - 6100–6199: Rent and occupancy
  - 6200–6299: Professional fees (legal, accounting, consulting)
  - 6300–6399: Insurance
  - 6400–6499: Office and administrative
  - 6500–6599: Technology and software
  - 6600–6699: Marketing and advertising
  - 6700–6799: Travel and entertainment
  - 6800–6899: Depreciation and amortization expense
  - 6900–6999: Repairs and maintenance
  - 7000–7099: Utilities
  - 7100–7199: Taxes and licenses (non-income)
  - 7200–7299: Bad debt expense
  - 7300–7399: Charitable contributions
  - 7400–7499: Penalties and fines (non-deductible — flag for M-1)
  - 7500–7999: Other operating expenses

- **8000–8999** — Other Income / Expense (non-operating)
  - 8000–8099: Interest expense
  - 8100–8199: Gain/loss on asset disposal
  - 8200–8299: Unrealized gain/loss
  - 8300–8399: Foreign exchange gain/loss
  - 8900–8999: Extraordinary items (rare under current GAAP)

- **9000–9999** — Income Tax
  - 9000–9099: Federal income tax expense (current provision)
  - 9100–9199: State income tax expense (current provision)
  - 9200–9299: Deferred tax expense/benefit
  - 9900–9999: Prior year tax adjustments

### Numbering Principles

- **Leave gaps** between active accounts — inserting accounts later without renumbering is critical
- **Group parent/child** accounts numerically (e.g., 1500 PP&E group, 1510 Land, 1520 Buildings, 1530 Equipment)
- **Mirror contra accounts** near their parent (e.g., 1520 Buildings → 1720 Accum Depr – Buildings)
- **Separate deductible from non-deductible** expenses at the account level — simplifies tax-line mapping and M-1 reconciliation
- **Use consistent width** — 4-digit minimum; 5-digit for organizations needing department/location segments

---

## 3. Hierarchical Structure

Accounts are organized in a tree: group (parent) accounts contain child accounts, and
only leaf (detail) accounts receive postings.

```
Assets (1000) [group — no posting]
├── Current Assets (1000) [group]
│   ├── Cash and Equivalents (1000) [group]
│   │   ├── 1010 Operating Checking [leaf — postable]
│   │   ├── 1020 Payroll Checking [leaf]
│   │   └── 1030 Money Market [leaf]
│   ├── Accounts Receivable (1200) [group]
│   │   ├── 1200 Trade Receivables [leaf]
│   │   └── 1210 Allowance for Doubtful Accounts [leaf, contra]
│   └── ...
├── Fixed Assets (1500) [group]
│   ├── 1510 Land [leaf]
│   ├── 1520 Buildings [leaf]
│   ├── 1530 Equipment [leaf]
│   ├── 1540 Furniture & Fixtures [leaf]
│   ├── 1550 Vehicles [leaf]
│   ├── 1560 Leasehold Improvements [leaf]
│   ├── 1720 Accum Depr – Buildings [leaf, contra]
│   ├── 1730 Accum Depr – Equipment [leaf, contra]
│   └── ...
└── ...
```

### Design Rules

- **Group accounts never receive journal entries** — they aggregate child balances for reporting
- **Leaf accounts are the posting targets** — every transaction debits/credits a leaf account
- **Depth should match reporting needs** — too flat loses detail; too deep adds friction without value
- **3–4 levels is typical** for mid-market C-corps (root type → classification → sub-group → detail)

---

## 4. C-Corporation Equity Structure

C-corp equity accounts differ significantly from partnerships, sole proprietors, and S-corps.
The key accounts and their behavior:

- **3000 Common Stock** — Par value × shares issued. Rarely changes after incorporation. For no-par stock, entire issuance price goes here or to APIC per board resolution.
- **3100 Additional Paid-In Capital (APIC)** — Excess over par from stock issuance. Also receives stock compensation expense (ASC 718) and other capital transactions.
- **3200 Retained Earnings** — Cumulative net income less cumulative dividends. The year-end close process nets all revenue and expense accounts into retained earnings.
- **3300 Treasury Stock** — Contra-equity (debit balance). Shares repurchased but not retired. Reported at cost method (most common) or par value method.
- **3400 AOCI** — Unrealized items that bypass the income statement: foreign currency translation adjustments, unrealized gains/losses on available-for-sale securities, pension adjustments, cash flow hedge gains/losses.
- **3500 Dividends Declared** — Temporary equity account. Debited when board declares dividends, closed to retained earnings at year-end. Tracks dividend activity separately from other retained earnings movements.

### Year-End Close Mechanics

1. Revenue accounts (4xxx) credited to Income Summary
2. Expense accounts (5xxx–9xxx) debited to Income Summary
3. Income Summary net balance transferred to Retained Earnings (3200)
4. Dividends Declared (3500) closed to Retained Earnings
5. All temporary accounts begin next period at zero

---

## 5. Form 1120 Tax-Line Mapping

Each GL account should map to a specific Form 1120 line for tax reporting. This mapping
drives book-to-tax reconciliation and enables automated tax return preparation.

### Income (Form 1120, Page 1)

- 4000 Gross revenue → Line 1a (Gross receipts or sales)
- 4100 Returns/allowances → Line 1b (Returns and allowances)
- 4800 Interest income → Line 5 (Interest)
- 4810 Dividend income → Line 10 (see Schedule C for DRD)
- 4900 Gross rents → Line 6 / 4910 Royalties → Line 7
- 8100 Net gain on asset sales → Line 8 (via Schedule D / Form 4797)
- 4950 Other income → Line 10 (Other income)

### Deductions (Form 1120, Page 1)

- 6000 Officers compensation → Line 12
- 6010 Salaries and wages (non-officer) → Line 13
- 6300 Repairs and maintenance → Line 14
- 7200 Bad debts → Line 15
- 6100 Rents → Line 16
- 7100 Taxes and licenses → Line 17
- 8000 Interest expense → Line 18
- 7300 Charitable contributions → Line 19 (10% limitation applies)
- 6800 Depreciation → Line 20 (from Form 4562; book vs. tax difference hits M-1)
- 6900–7999 Other deductions → Line 26

### Non-deductible Items (Schedule M-1 / M-3 adjustments)

- 7400 Penalties and fines → M-1 Line 5 (add back — not deductible per IRC §162(f))
- 7410 50% meals disallowance → M-1 Line 5 (non-deductible portion)
- 7420 Entertainment → fully non-deductible post-TCJA
- 9000 Federal income tax expense → M-1 Line 2 (add back — never deductible)
- 8200 Unrealized gains/losses → M-1 adjustment (book only, no tax event)

---

## 6. Book-Tax Differences

### Timing Differences (deferred tax — reverse in future periods)

- **Depreciation**: GAAP straight-line vs. tax MACRS (IRC §168) / bonus depreciation (IRC §168(k))
- **Revenue recognition**: GAAP ASC 606 timing vs. tax constructive receipt
- **Bad debt**: GAAP allowance method vs. tax direct write-off (IRC §166)
- **Prepaid expenses**: GAAP matching vs. tax 12-month rule
- **Deferred revenue**: GAAP deferral vs. tax advance payment rules (Rev. Proc. 2004-34)
- **Section 179 expensing**: full deduction in year 1 for tax, capitalized for book

### Permanent Differences (never reverse)

- **Meals**: 50% disallowance for tax (IRC §274)
- **Entertainment**: 100% disallowed for tax post-TCJA
- **Fines and penalties**: non-deductible (IRC §162(f))
- **Federal income tax**: non-deductible
- **Tax-exempt interest income**: book income, not taxable
- **Dividends received deduction**: 50%/65%/100% (IRC §243) — reduces taxable income below book
- **Officers' life insurance**: premiums non-deductible if corporation is beneficiary (IRC §264)
- **Political contributions**: non-deductible (IRC §162(e))

### Designing for Tax Compliance

Separate accounts for items with different tax treatment at the GL level rather than
relying on year-end reclassification:

- Deductible meals (50%) vs. entertainment (0%) in separate accounts, not lumped together
- Officer compensation separate from staff wages (Form 1120 reports them on different lines)
- Depreciation expense by asset class if book and tax methods differ
- Interest expense separated by purpose if §163(j) limitation applies

---

## 7. Special Account Types

Certain accounts serve system-level functions beyond normal classification. Understanding
their role prevents misuse and ensures the GL operates correctly.

- **Bank / Cash** — The primary operating accounts. Reconciled to bank statements monthly. Each physical bank account gets its own GL account.
- **Accounts Receivable** — Controlled by the sub-ledger. Individual customer balances tracked in the AR sub-ledger; the GL account shows the aggregate. Must be reconciled to the sub-ledger regularly.
- **Accounts Payable** — Same sub-ledger pattern as AR but for vendor obligations.
- **Undeposited Funds** — Staging account for receipts not yet deposited. Clears when the deposit posts to the bank account. Prevents timing mismatches between receipt and deposit.
- **Suspense / Clearing** — Temporary holding accounts. Any balance here at period-end indicates an incomplete transaction that needs investigation.
- **Intercompany** — Used when a parent entity transacts with subsidiaries. Must net to zero across the consolidated group.
- **Retained Earnings** — The system account that receives year-end close entries. Should never be directly debited/credited during the year except for prior-period adjustments.
- **Opening Balance Equity** — Used during initial setup to balance the opening trial balance. Should be zero after setup is complete; any remaining balance indicates an out-of-balance condition.
- **Income Summary** — Temporary account used only during year-end close to accumulate net income before transfer to retained earnings. Balance should always be zero outside the close process.

---

## 8. COA Design Principles

### Start with the Tax Return

For C-corporations, the Form 1120 line items define the minimum account granularity.
Every line on the return that applies to the entity should have at least one dedicated
GL account. Working backward from the tax return ensures nothing falls through the cracks
at filing time.

### Match Reporting Needs

The COA serves multiple audiences — management reporting, tax compliance, audit, and
regulatory. Design accounts to satisfy all without requiring extensive reclassification
at period-end. If management wants to see marketing spend by channel, create sub-accounts
(6610 Digital Marketing, 6620 Print, 6630 Events) rather than relying on classes or
tags alone.

### Keep It Lean

Every account adds cognitive load for bookkeepers and review burden for accountants.
Only create accounts that serve a distinct reporting or compliance purpose. Two signs of
bloat: accounts that never receive postings, and accounts whose balances are always
combined in every report.

### Plan for Growth

Leave numerical gaps (at least 10 between active accounts, 100 between categories).
A COA that requires renumbering to add accounts is a COA that will accumulate workarounds.
