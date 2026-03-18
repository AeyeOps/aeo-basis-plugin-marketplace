---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Estimated Tax — C-Corporations (Form 1120-W / Form 2220)

**Purpose**: Quarterly estimated tax payment rules for C-corps, safe harbor thresholds, penalty calculation, and special methods for uneven income.

**Skills**: `tax-compliance`, `estimated-tax`, `penalty-analysis`

---

## Background

Form 1120-W (worksheet for estimated tax) was discontinued after tax year 2022. Estimated tax rules now appear in the Form 1120 instructions and Form 2220 (Underpayment of Estimated Tax by Corporations) instructions.

## When Estimated Tax Is Required

A corporation must make quarterly estimated tax payments if it expects its total tax liability (after credits) to be **$500 or more** for the tax year.

## Installment Due Dates

For calendar-year corporations, the four installments are due:

- **1st** — April 15 (15th day of 4th month)
- **2nd** — June 15 (15th day of 6th month)
- **3rd** — September 15 (15th day of 9th month)
- **4th** — December 15 (15th day of 12th month)

Each installment equals **25%** of the required annual payment. If a due date falls on a weekend or legal holiday, payment is due the next business day. All payments must be made via **EFTPS** (Electronic Federal Tax Payment System).

## Required Annual Payment

The required annual payment is the **lesser of**:

1. **100% of current-year tax** (shown on Form 1120, line 31, less credits), or
2. **100% of prior-year tax** (must have filed a return for a full 12-month prior year showing a tax liability)

No penalty applies if the total tax is less than $500.

## Large Corporation Rules

A corporation is "large" if it had **$1 million or more in taxable income** in any of the three preceding tax years (taxable income for this test excludes NOL and capital loss carrybacks/carryovers).

- Large corporations may use prior-year tax **only for the 1st installment**.
- Installments 2-4 must be based on **100% of current-year tax**.
- Any shortfall from using prior-year safe harbor in Q1 must be made up by Q2.

## Annualized Income Installment Method

Corporations with uneven income throughout the year can reduce penalties by annualizing income for each installment period.

**Standard annualization periods**: 3, 3, 6, and 9 months
**Standard multipliers**: 4, 4, 2, and 1.33333

Alternative annualization periods are available if elected on Form 8842 by the first installment due date.

Extraordinary items (NOL carryovers, Section 481(a) adjustments, gains from dispositions exceeding 25% of assets) have special handling rules. De minimis items under $1 million may use simplified treatment.

## Adjusted Seasonal Installment Method

Available if the corporation's base period percentage for any 6 consecutive months equals **70% or more**. The base period percentage averages that same 6-month period's share of total taxable income across the three preceding tax years.

## Underpayment Penalty (Form 2220)

### When to File Form 2220

The IRS will generally calculate the penalty automatically. File Form 2220 only if:

- Using the annualized income installment method
- Using the adjusted seasonal installment method
- Using prior-year tax for a large corporation's first installment

### Penalty Rate

Federal short-term rate **+ 5 percentage points**, calculated separately for each underpayment period. No netting — overpayment in one quarter does not offset underpayment in an earlier quarter.

### Penalty Period

Runs from the installment due date to the **earlier of**:
- The actual payment date, or
- The 15th day of the 4th month after the tax year ends (filing deadline)

### Waiver

The IRS may waive the penalty if the underpayment was due to casualty, disaster, or other unusual circumstances and imposing the penalty would be against equity and good conscience.

### Method Selection

When Schedule A (Form 2220) is completed, the form automatically applies the **smallest required installment** from:
- Adjusted seasonal installment
- Annualized income installment
- Regular 25% quarterly installment

## CAMT Relief (2025+)

For tax years beginning in 2025, the IRS waives estimated tax penalties attributable to the **Corporate Alternative Minimum Tax (CAMT)**. Per Notice 2025-27, affected corporations must exclude CAMT liability from Part I, line 1 calculations on Form 2220.

## Decision Points

1. **Is total expected tax >= $500?** If no, no estimated payments required.
2. **Is this a large corporation?** If yes, prior-year safe harbor applies only to Q1.
3. **Is income seasonal or uneven?** Consider annualized or seasonal methods.
4. **Did the corp file a full 12-month prior-year return?** If no, prior-year safe harbor is unavailable.
5. **Is CAMT applicable (2025+)?** Exclude from penalty calculation per Notice 2025-27.

## Accounting System Integration Notes

- Estimated tax payments are recorded as debits to **Income Tax Payable** (or a sub-account) and credits to the bank account.
- Each quarterly payment should be recorded on the actual payment date. Delegate journal entry execution to `qbo-integration:qbo-bookkeeping`.
- At year-end, the total of four quarterly payments offsets the computed tax liability on Form 1120, line 33.

## Year-by-Year Variance Table

| Year | Threshold | Safe Harbor (Non-Large) | Safe Harbor (Large) | Penalty Rate Basis | Notes |
|------|-----------|------------------------|--------------------|--------------------|-------|
| 2007-2009 | $500 | 100% prior or current | Prior yr Q1 only | Fed ST rate + 5% | Standard rules |
| 2010-2015 | $500 | 100% prior or current | Prior yr Q1 only | Fed ST rate + 5% | Standard rules |
| 2016-2017 | $500 | 100% prior or current | Prior yr Q1 only | Fed ST rate + 5% | Standard rules |
| 2018-2022 | $500 | 100% prior or current | Prior yr Q1 only | Fed ST rate + 5% | TCJA; Form 1120-W still available |
| 2023+ | $500 | 100% prior or current | Prior yr Q1 only | Fed ST rate + 5% | Form 1120-W discontinued; rules in Form 1120 instructions |
| 2025+ | $500 | 100% prior or current | Prior yr Q1 only | Fed ST rate + 5% | CAMT penalty waiver (Notice 2025-27) |

The $500 threshold has not changed. The core safe harbor structure (100% of lesser of current or prior year) has been stable since 2007. The primary year-over-year variance is the applicable federal short-term rate used in penalty calculations.

## Authoritative References

- IRC Section 6655 — Failure by corporation to pay estimated income tax
- IRC Section 6621 — Determination of interest rate
- Form 1120 Instructions — Estimated Tax Payments section
- Form 2220 Instructions — Underpayment penalty computation
- Notice 2025-27 — CAMT estimated tax penalty waiver
- EFTPS.gov — Electronic Federal Tax Payment System
