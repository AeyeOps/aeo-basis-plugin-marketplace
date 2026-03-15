# Payroll Tax Compliance — Forms 940, 941, W-2/W-3

**Purpose**: Employment tax obligations for employers — FICA, FUTA, deposit schedules, quarterly/annual filing, W-2 reporting, trust fund recovery penalty, and Florida reemployment tax.

**Skills**: `bk/payroll-recording` (payroll journal entry posting)

**Tier**: 1 (year-indexed — deposit thresholds, wage bases, and rates change annually)

---

## Employment Tax Overview

Employers withhold and remit three categories of federal employment tax:

- **FICA — Social Security**: 6.2% employer + 6.2% employee on wages up to the annual wage base
- **FICA — Medicare**: 1.45% employer + 1.45% employee on all wages (no cap)
- **Additional Medicare Tax**: 0.9% employee-only on wages exceeding $200,000 ($250K MFJ, $125K MFS). Employer does not match. Effective 2013+.
- **FUTA**: 6.0% employer-only on first $7,000 of wages per employee. Reduced by SUTA credit (max 5.4%) to 0.6% effective rate for employers current on state unemployment taxes.
- **Federal Income Tax Withholding**: Per employee W-4 elections and IRS withholding tables.

Total employer cost per employee: 6.2% SS + 1.45% Medicare + 0.6% FUTA (effective) = 8.25% on wages up to the SS wage base, 2.05% on wages above the SS wage base (up to $7K FUTA limit, then 1.45% only).

## Form 941 — Employer's Quarterly Federal Tax Return

Reports total wages, federal income tax withheld, FICA taxes (employer + employee shares), adjustments, quarterly liability breakout, and deposits made.

**Due dates**: Q1 4/30, Q2 7/31, Q3 10/31, Q4 1/31. Weekend/holiday pushes to next business day. 10-day extension if all deposits were timely.

**Common errors**: reporting gross vs taxable wages, omitting third-party sick pay, mismatching deposits to quarters, W-3 totals not reconciling to sum of four 941s.

## Form 940 — Annual FUTA Return

Due January 31 (10-day extension if all deposits timely). FUTA: 6.0% on first $7,000 per employee, reduced to 0.6% effective by SUTA credit (5.4%). Credit reduction states (DOL annual list) increase the effective rate. Deposit quarterly if liability exceeds $500. Amended 940: check "Amended" box on the form.

## Deposit Schedules

### Determining Your Schedule — Lookback Period

The deposit schedule is determined by total taxes reported on Form 941 during a 4-quarter lookback period (July 1 through June 30 of the year before the prior year):

- **Monthly depositor**: Lookback period taxes were $50,000 or less. Deposit by the 15th of the following month.
- **Semi-weekly depositor**: Lookback period taxes exceeded $50,000. Deposit by Wednesday (for Sat–Tue paydays) or Friday (for Wed–Fri paydays).
- **New employers**: Default to monthly schedule until a lookback period is established.

### $100,000 Next-Day Deposit Rule

If an employer accumulates $100,000 or more in employment taxes on any day during a deposit period, the tax must be deposited by the **next business day**. Once triggered, the employer becomes a semi-weekly depositor for the remainder of the calendar year and the following calendar year.

### EFTPS Requirement

All federal tax deposits must be made electronically via the Electronic Federal Tax Payment System (EFTPS). Checks and money orders are not accepted for deposits.

## W-2 / W-3 Filing

### Deadlines

- **W-2 to employees**: January 31 (paper or electronic)
- **W-2/W-3 to SSA**: January 31 (paper and electronic — same deadline since 2017)
- **State copies**: Varies by state. Florida has no state income tax, so no state W-2 copy is required for FL-only employers.

### Corrected W-2c

Filed when errors are discovered on previously filed W-2s. Common corrections: wrong SSN, wrong wage amounts, wrong tax withheld. File W-2c with a W-3c transmittal to SSA. No deadline, but correct promptly to avoid penalties.

### Electronic Filing Threshold

- 2023 and prior: E-file required if filing 250+ W-2s
- 2024+: Threshold reduced to 10 information returns (aggregate across all return types — W-2, 1099, etc.)

## Trust Fund Recovery Penalty (TFRP) — IRC 6672

### What It Is

A penalty equal to **100% of the unpaid trust fund taxes** (the employee's share of FICA plus withheld federal income tax). The trust fund portion does not include the employer's share of FICA.

### Who Is Liable

Any **responsible person** who **willfully** fails to collect, truthfully account for, or pay over the trust fund taxes. Responsible persons include:

- Officers and directors with authority over financial affairs
- Employees who sign checks or have authority to direct payment of creditors
- Payroll service providers in some circumstances

### Willfulness Standard

Willfulness does not require intent to defraud. It means the responsible person was aware of the outstanding taxes and either intentionally disregarded the obligation or was plainly indifferent. Paying other creditors ahead of the IRS when aware of the tax debt is sufficient.

### Assessment Process

IRS conducts interviews (Form 4180), issues proposed assessment via Letter 1153 (60-day letter), allows administrative appeal, then assesses. Each responsible person is jointly and severally liable for the full amount.

## Florida Reemployment Tax (SUTA/SUI)

### Key Parameters

- **Wage base**: $7,000 per employee (unchanged since inception)
- **Rate determination**: Experience-rated based on employer's claims history. New employer rate: 2.7% (standard) or industry-specific rate.
- **Filing**: Quarterly on Form RT-6 (UCT-6), due the last day of the month following the quarter
- **Reporting**: Florida requires electronic filing and payment for most employers
- **No state income tax withholding**: Florida does not impose a state income tax, so no state withholding is required

## Common Payroll Tax Errors

- **Worker misclassification**: Treating employees as independent contractors avoids payroll taxes but triggers back-tax assessments, penalties, and TFRP exposure. See `tx/worker-classification`.
- **Late deposits**: Penalty is 2% (1-5 days late), 5% (6-15 days late), 10% (16+ days late), 15% (10+ days after first IRS notice). These compound quickly.
- **Wrong deposit schedule**: Using monthly schedule when semi-weekly is required results in late deposit penalties even if the total quarterly amount is correct.
- **Failure to file**: Penalty is 5% per month (up to 25%) plus failure-to-pay penalty of 0.5% per month. Combined max: 47.5% of tax due.
- **Mismatched 941/W-2 totals**: SSA and IRS cross-match. Discrepancies trigger CP2100 notices.

## Decision Points

1. **Monthly or semi-weekly depositor?** Check lookback period totals each December for the following year's schedule.
2. **FUTA deposit required this quarter?** Only if cumulative FUTA liability exceeds $500.
3. **Credit reduction state?** Check DOL list before filing Form 940 — affects FUTA credit calculation.
4. **Officer on payroll?** C-corp officers who perform services are statutory employees. See `tx/officer-compensation`.
5. **Payroll service provider?** If using a third-party payroll provider, the employer remains ultimately liable for deposits. Verify deposits are being made.

## Accounting System Integration Notes

For platform-specific payroll posting procedures, invoke `qbo-integration:qbo-bookkeeping` or `bookkeeping:payroll-recording`.

- Payroll services handle W-2 generation, tax deposit scheduling, and quarterly 941/annual 940 filing.
- Payroll journal entries post automatically: debit Payroll Expense accounts, credit Payroll Liabilities (withholding, employer taxes) and Cash/Bank.
- For reconstructed years without payroll service: manual journal entries to Officers' Compensation, Payroll Tax Expense, and corresponding liability accounts. W-2s must be filed separately.
- Verify payroll tax liability accounts reconcile to actual deposits — most accounting systems do not auto-clear liability accounts on deposit.

## Year-by-Year Variance Table

| Tax Year | SS Wage Base | SS Rate (each) | Addl Medicare | W-2 E-File Threshold |
|----------|-------------|----------------|---------------|---------------------|
| 2007 | $97,500 | 6.2% | N/A | 250 returns |
| 2008 | $102,000 | 6.2% | N/A | 250 returns |
| 2009-2010 | $106,800 | 6.2% | N/A | 250 returns |
| 2011 | $106,800 | 4.2% ee / 6.2% er | N/A | 250 returns |
| 2012 | $110,100 | 4.2% ee / 6.2% er | N/A | 250 returns |
| 2013 | $113,700 | 6.2% | 0.9% >$200K | 250 returns |
| 2014 | $117,000 | 6.2% | 0.9% >$200K | 250 returns |
| 2015-2016 | $118,500 | 6.2% | 0.9% >$200K | 250 returns |
| 2017 | $127,200 | 6.2% | 0.9% >$200K | 250 returns |
| 2018 | $128,400 | 6.2% | 0.9% >$200K | 250 returns |
| 2019 | $132,900 | 6.2% | 0.9% >$200K | 250 returns |
| 2020 | $137,700 | 6.2% | 0.9% >$200K | 250 returns |
| 2021 | $142,800 | 6.2% | 0.9% >$200K | 250 returns |
| 2022 | $147,000 | 6.2% | 0.9% >$200K | 250 returns |
| 2023 | $160,200 | 6.2% | 0.9% >$200K | 250 returns |
| 2024 | $168,600 | 6.2% | 0.9% >$200K | 10 returns |
| 2025 | $176,100 | 6.2% | 0.9% >$200K | 10 returns |
| 2026 | $184,500 | 6.2% | 0.9% >$200K | 10 returns |

**Constants across all years**: Medicare rate 1.45% each (employer + employee), FUTA rate 6.0% on $7,000 wage base (0.6% effective with full SUTA credit).

**Notes on variances**:
- 2011-2012: Employee SS rate temporarily reduced to 4.2% under the payroll tax holiday (Tax Relief Act of 2010, extended by Middle Class Tax Relief Act of 2012). Employer rate remained 6.2%.
- 2013+: Additional Medicare Tax of 0.9% on wages exceeding $200,000 (single) / $250,000 (MFJ) enacted by ACA. Employee-only; employer does not match.
- 2024+: E-file threshold dropped from 250 to 10 aggregate information returns (T.D. 9972).

---

**Authoritative References**:
- IRC 3101-3102 — FICA taxes (employee and employer)
- IRC 3111 — Employer's share of FICA
- IRC 3301-3306 — FUTA
- IRC 3402 — Federal income tax withholding
- IRC 6672 — Trust fund recovery penalty
- IRC 3509 — Determination of employer's liability for certain employment taxes (misclassification)
- IRS Publication 15 (Circular E) — Employer's Tax Guide
- IRS Publication 15-A — Employer's Supplemental Tax Guide
- IRS Publication 15-T — Federal Income Tax Withholding Methods
- Form 941 Instructions (current revision)
- Form 940 Instructions (current revision)
- Florida Statute 443 — Reemployment Assistance (unemployment tax)
- SSA: Annual Social Security wage base announcements
