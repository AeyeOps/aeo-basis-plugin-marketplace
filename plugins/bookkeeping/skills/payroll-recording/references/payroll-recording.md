---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Payroll Recording

Procedures for recording payroll journal entries, reconciling payroll liabilities, and integrating external payroll providers with the general ledger.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `bookkeeping-payroll`, `bookkeeping-monthly-close`, `payroll-compliance`

## Contents

- [Overview](#overview)
- [Payroll Journal Entry Pattern](#payroll-journal-entry-pattern)
- [Account Setup](#account-setup)
- [Accrual vs Cash Basis](#accrual-vs-cash-basis)
- [External Provider Integration](#external-provider-integration)
- [Payroll Liability Reconciliation](#payroll-liability-reconciliation)
- [Common Errors](#common-errors)
- [Decision Points](#decision-points)
- [References](#references)

---

## Overview

Payroll is typically the largest expense for service businesses. Recording it correctly requires capturing the full economic picture: gross wages (expense), employer-side taxes (expense), employee withholdings (liability), benefits (expense and/or liability), and net pay (cash outflow). Recording only the net check amount is the most common and most damaging bookkeeping error.

---

## Payroll Journal Entry Pattern

### Standard Biweekly/Semi-Monthly Entry

**Debits (expenses and asset reductions):**

| Account | Type | Description |
|---------|------|-------------|
| Payroll Expense: Gross Wages | Expense | Total compensation before any deductions |
| Payroll Expense: Employer FICA | Expense | Employer's 6.2% SS + 1.45% Medicare match |
| Payroll Expense: FUTA | Expense | Federal unemployment (0.6% on first $7,000/employee) |
| Payroll Expense: SUTA | Expense | State unemployment (rate varies by state and experience) |
| Payroll Expense: Employer Benefits | Expense | Employer portion of health, dental, vision, 401(k) match |

**Credits (liabilities and cash):**

| Account | Type | Description |
|---------|------|-------------|
| Payroll Liabilities: Federal Income Tax | Current Liability | FIT withheld from employees |
| Payroll Liabilities: Employee FICA | Current Liability | Employee's 6.2% SS + 1.45% Medicare |
| Payroll Liabilities: Employer FICA | Current Liability | Employer's matching amount (until deposited) |
| Payroll Liabilities: State Income Tax | Current Liability | State withholding |
| Payroll Liabilities: FUTA | Current Liability | Federal unemployment (until deposited) |
| Payroll Liabilities: SUTA | Current Liability | State unemployment (until deposited) |
| Payroll Liabilities: Benefits Payable | Current Liability | Employee and employer benefit amounts |
| Payroll Liabilities: Garnishments | Current Liability | Court-ordered withholdings |
| Checking Account | Asset (Bank) | Net pay disbursed to employees |

**Verification**: Total debits = Total credits. The sum of all expense debits equals gross wages + employer taxes + employer benefits. The checking account credit equals net pay only.

### Tax Deposit Entry

When payroll taxes are remitted to the IRS/state:

| Debit | Credit |
|-------|--------|
| Payroll Liabilities: Federal Income Tax | Checking Account |
| Payroll Liabilities: Employee FICA | |
| Payroll Liabilities: Employer FICA | |

This reduces the liability and cash. Do NOT debit an expense account — the expense was already recorded at payroll time.

---

## Account Setup

### Required Expense Accounts

- `6100 Payroll Expense: Gross Wages` (or split by department/type: Officers, Salaries, Hourly)
- `6110 Payroll Expense: Employer FICA`
- `6120 Payroll Expense: FUTA`
- `6130 Payroll Expense: SUTA`
- `6140 Payroll Expense: Employer Health Insurance`
- `6150 Payroll Expense: Employer 401(k) Match`
- `6160 Payroll Expense: Workers Compensation`
- `6190 Payroll Expense: Payroll Processing Fees`

### Required Liability Accounts

- `2100 Payroll Liabilities: Federal Income Tax Payable`
- `2110 Payroll Liabilities: Social Security Payable` (or combined FICA)
- `2120 Payroll Liabilities: Medicare Payable` (or combined FICA)
- `2130 Payroll Liabilities: State Withholding Payable`
- `2140 Payroll Liabilities: FUTA Payable`
- `2150 Payroll Liabilities: SUTA Payable`
- `2160 Payroll Liabilities: Benefits Payable`
- `2170 Payroll Liabilities: Garnishments Payable`

**Do not** use a single "Payroll Liabilities" account for all tax types. Separate accounts are essential for reconciling to quarterly Form 941 and annual Form 940.

---

## Accrual vs Cash Basis

### Accrual Basis

- Record gross wages expense when employees earn the compensation (pay period end date)
- Record employer tax expense simultaneously
- Liabilities exist from pay period end until tax deposits and paychecks clear
- Month-end accrual needed when a pay period spans two months (e.g., pay period ends Feb 3 but wages were partly earned in January)

### Cash Basis

- Record wages expense when paychecks are disbursed
- Record employer taxes when deposited
- No liability accounts needed (theoretically), but most cash-basis businesses still track payroll liabilities for compliance
- Simpler but can distort monthly P&L when pay dates fall unevenly

### Month-End Pay Period Split (Accrual)

If a biweekly pay period spans month-end (e.g., Jan 20 - Feb 2):

1. Calculate wages earned through Jan 31 (8 of 10 workdays = 80%)
2. Debit Wages Expense, Credit Wages Payable for the Jan portion
3. Reverse the accrual on Feb 1
4. Record the full payroll entry on the actual pay date

---

## External Provider Integration

### ADP, Gusto, Paychex — Manual JE Method

When using a third-party payroll provider that does not directly integrate with the accounting system:

1. **Obtain the payroll summary report** from the provider after each pay run
2. **Map report fields to GL accounts** — gross wages, each tax type, each deduction type, net pay
3. **Create a journal entry in the accounting system** dated as of the check date (or pay period end for accrual)
4. **Use a consistent JE numbering scheme** — e.g., `PAYROLL-2025-01-15` or `ADP-20250115`
5. **Attach the payroll summary PDF** to the journal entry for audit trail
6. **Verify the bank clearing amount** matches the net pay on the JE by reconciling the bank account

### ADP RUN GL Integration

ADP RUN can export a GL file mapped to the chart of accounts:

- Map ADP GL accounts to the chart of accounts during initial setup
- Export runs as CSV or direct-post (if connector available)
- Verify account mappings quarterly — ADP category changes can silently break mappings
- Always reconcile: the ADP GL export is a convenience, not a guarantee of accuracy

---

## Payroll Liability Reconciliation

Perform quarterly at minimum (aligned with Form 941 filing):

1. **Pull the Balance Sheet** as of quarter-end — note each payroll liability account balance
2. **Pull Form 941 (or provider's quarterly summary)** — note total taxes reported
3. **Pull bank records** — sum all tax deposits for the quarter
4. **Reconcile**: Beginning liability + Current quarter taxes - Deposits = Ending liability
5. **Investigate differences** — common causes:
   - Timing: deposit made in Q2 for Q1 liability
   - Recording: payroll JE omitted a tax type
   - Rate change: SUTA rate changed mid-quarter
   - Rounding: cumulative rounding differences across pay periods

**Target**: Payroll liability accounts should approach zero after all deposits for the period are made (small rounding differences acceptable).

### Annual Reconciliation

At year-end, reconcile:
- Total wages per GL to total wages per W-2s/W-3
- Total FICA per GL to Form 941 annual total
- Total FUTA per GL to Form 940
- Total state withholding per GL to state quarterly returns

---

## Common Errors

| Error | Impact | Fix |
|-------|--------|-----|
| Recording net pay as wages expense | Understates expenses by 25-35%; no liability tracking | Post full JE with gross wages, taxes, deductions |
| Categorizing bank feed payroll debit as expense | Same as above — loses all detail | Delete bank feed entry; post proper JE |
| Single payroll liability account | Cannot reconcile to tax returns by type | Create separate sub-accounts; reclassify historical entries |
| Debiting expense when paying taxes | Double-counts the expense (once at payroll, once at deposit) | Debit the liability account when remitting taxes |
| Missing employer FICA | Understates tax expense by ~7.65% of wages | Always include the employer match as a separate debit |
| Omitting payroll processing fees | Bank shows debit with no GL match | Post fees to a dedicated processing fee expense account |
| Not accruing wages at month-end | P&L distorted when pay periods span months | Post accrual JE; reverse first day of next month |

---

For platform-specific execution, invoke `qbo-integration:qbo-bookkeeping`.

---

## Decision Points

| Situation | Decision | Guidance |
|-----------|----------|----------|
| Client uses integrated payroll (provider posts directly to GL) | No manual JE needed | Still reconcile quarterly to Form 941 |
| Client uses ADP/Gusto/Paychex with no direct ledger integration | Post manual JE per pay run | Map accounts once; reuse template |
| Client uses ADP with GL export | Use export but verify | Spot-check 1 pay run per quarter manually |
| Pay period spans month-end (accrual client) | Post accrual and reversal | Estimate based on workdays in each month |
| Payroll liability balance is growing | Investigate missed deposits | Compare deposits to 941 schedule |
| Small rounding difference on 941 reconciliation (<$5) | Adjust to match 941 | Post to payroll tax expense |

---

## References

- Acciyo. "Payroll Journal Entry in QuickBooks Online." https://acciyo.com/payroll-journal-entry-quickbooks-online/
- IRS Form 941 — Employer's Quarterly Federal Tax Return
- IRS Form 940 — Employer's Annual Federal Unemployment Tax Return
- IRS Publication 15 (Circular E) — Employer's Tax Guide
