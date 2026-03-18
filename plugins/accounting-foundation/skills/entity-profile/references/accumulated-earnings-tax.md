---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Accumulated Earnings Tax (IRC 531-537): C-Corporation Rules

**Purpose**: AET computation, reasonable business needs analysis, Bardahl working capital formula, and audit defense for C-corporations retaining earnings beyond the accumulated earnings credit.

**Skills**: —

---

## AET Overview

The **Accumulated Earnings Tax** is a penalty tax on C-corporations that retain earnings beyond reasonable business needs to avoid shareholder-level tax on dividends. It targets closely-held corporations where controlling shareholders can direct dividend policy. The AET is imposed in addition to the regular corporate income tax and is assessed on a per-year basis.

The AET does not apply to S-corporations, tax-exempt organizations, passive foreign investment companies, or personal holding companies (IRC 542 — the PHC tax and AET are mutually exclusive).

## Tax Rate and Computation

**Accumulated Taxable Income** is the base on which the AET is imposed:

1. Start with taxable income (Form 1120, Line 30)
2. Adjust for: federal income tax accrued, charitable contributions in excess of the 10% limit, net capital loss adjustment, net capital gain (less tax attributable to it)
3. Subtract the **dividends-paid deduction** (dividends actually paid during the year plus consent dividends)
4. Subtract the **accumulated earnings credit** (see below)
5. The result is accumulated taxable income — multiply by the AET rate

## Year-by-Year Variance Table

| Tax Year | AET Rate | Accumulated Earnings Credit | Corp Tax Rate | Notes |
|----------|----------|----------------------------|---------------|-------|
| 2007-2012 | **15%** | $250K ($150K PSC) | 15%-35% graduated | AET rate aligned with qualified dividend rate |
| 2013-2017 | **20%** | $250K ($150K PSC) | 15%-35% graduated | ATRA made 20% dividend rate permanent; AET followed |
| 2018-2025 | **20%** | $250K ($150K PSC) | **21% flat (TCJA)** | Flat 21% rate makes retention more attractive; IRS AET audit risk increases |
| 2026+ | **20%** (current law) | $250K ($150K PSC) | 21% flat (unless changed) | Watch for legislative changes |

**Key shift at TCJA**: The spread between the corporate rate (21%) and the top individual rate on dividends (23.8% including NIIT) makes retention economically attractive, which increases IRS scrutiny for AET.

## Accumulated Earnings Credit

The credit is the **greater of**:
- The amount of E&P retained for reasonable business needs (minus accumulated E&P at the close of the preceding year), OR
- $250,000 minus accumulated E&P at the close of the preceding year ($150,000 for certain personal service corporations — health, law, engineering, architecture, accounting, actuarial science, performing arts, consulting)

**Practical effect**: If total accumulated E&P has never exceeded $250,000, there is no AET exposure regardless of whether the corporation can demonstrate reasonable business needs.

## Reasonable Business Needs Test

Retention must be for **specific, definite, and feasible** plans. Vague or general growth aspirations do not satisfy the test (Treas. Reg. 1.537-1(b)).

### Recognized Reasonable Needs

- **Business expansion** — Documented plans for new facilities, equipment, product lines, geographic markets. Must include cost estimates, timelines, and board authorization
- **Working capital** — Bardahl formula computation (see below)
- **Debt retirement** — Scheduled principal payments or planned early payoff
- **Equipment replacement** — Anticipated capital expenditures with vendor quotes or depreciation schedules
- **Self-insurance reserves** — Quantified risk exposure with actuarial or historical loss data
- **Product liability contingencies** — Documented pending or anticipated claims
- **Stock redemption under Section 303** — Redemption to pay estate taxes of a deceased shareholder (specifically sanctioned by IRC 537(a)(2))
- **Investments required by business** — Reserves mandated by contract, regulation, or industry practice

### Unreasonable Accumulation Indicators

The following create a rebuttable presumption that earnings are not retained for reasonable business needs:

- **Loans to shareholders** — Especially interest-free or below-market loans; treated as constructive dividends and evidence of tax avoidance motive
- **Investments unrelated to business operations** — Passive portfolio investments, personal-use assets, loans to related parties
- **Excessive liquid assets** — Cash and marketable securities far exceeding operational needs without documented plans
- **No history of dividends** — Extended periods of zero distributions combined with growing retained earnings
- **Personal expenses paid by the corporation** — Travel, entertainment, or assets benefiting shareholders personally

## Bardahl Formula: Working Capital Needs

The Bardahl formula (from *Bardahl Manufacturing Corp. v. Commissioner*, 24 T.C.M. 1030 (1965)) calculates the amount of working capital a corporation reasonably needs for one operating cycle.

### Computation Steps

1. **Operating cycle** = Inventory cycle + Accounts receivable cycle
   - **Inventory cycle** = (Average inventory / COGS) x 365
   - **AR cycle** = (Average accounts receivable / Net sales) x 365
2. **Operating expenses for one cycle** = (Annual operating expenses / 365) x Operating cycle days
3. Subtract **credit cycle**: (Average accounts payable / COGS) x 365 — this offsets the cash need
4. **Net working capital need** = Operating expenses for one cycle minus credit cycle offset
5. Compare to **current liquid assets** — excess above this amount is potentially unreasonable accumulation

### Peak vs. Average Method

- **Average method** — Uses average balances over the year (standard Bardahl)
- **Peak method** — Uses peak balances during seasonal high points. Appropriate for businesses with significant seasonal variation. Courts have accepted the peak method when the business demonstrates cyclical cash needs

One operating cycle of working capital is **presumed reasonable**. Amounts beyond one cycle require additional justification (expansion plans, debt retirement, etc.).

## Interaction with Personal Holding Company Tax

IRC 541-547 imposes a **20% PHC tax** on undistributed personal holding company income. A corporation is a PHC if:

1. Five or fewer individuals own more than 50% of the stock (attribution rules apply), AND
2. At least 60% of adjusted ordinary gross income is personal holding company income (rents, royalties, dividends, interest, certain personal service contracts)

**Mutual exclusivity**: If the PHC tax applies to a corporation for a given year, the AET does **not** apply for that year (IRC 532(b)(1)). The PHC tax is self-assessed on Schedule PH (Form 1120); the AET is typically asserted by the IRS on examination.

## Documentation Requirements

Contemporaneous documentation is critical. The IRS can assert AET at any point during the statute of limitations period, and the burden of proof is on the taxpayer unless the IRS issues a formal notice of deficiency.

### Essential Records

- **Board resolutions** — Minutes documenting specific business purposes for retained earnings, adopted at or before the close of the tax year
- **Capital budgets** — Detailed projections with cost estimates, vendor quotes, financing alternatives
- **Expansion plans** — Market analysis, feasibility studies, site selection documents
- **Bardahl computation** — Working capital analysis prepared annually with supporting financial data
- **Dividend policy memorandum** — Written rationale for dividend decisions (including decisions not to pay)

### Burden of Proof

- **General rule**: Taxpayer bears the burden of proving accumulations are for reasonable business needs
- **IRC 534 shift**: If the IRS sends a notification that it proposes to assert AET, the taxpayer must respond with a statement of grounds for retention within 60 days. If the taxpayer responds timely, the **burden shifts to the IRS** to prove the accumulation was beyond reasonable needs
- **Filing Section 531 defense**: Include the statement with the response to the 60-day letter; this is the primary procedural protection

## Accounting System Notes

- **Retained earnings tracking**: The Balance Sheet report shows cumulative retained earnings (Schedule L, Line 25). Year-over-year growth above $250K triggers review
- **E&P vs. book retained earnings**: The accounting system tracks book retained earnings; E&P requires tax adjustments (depreciation differences, tax-exempt income, nondeductible expenses). Maintain a separate E&P schedule outside the accounting system
- **Dividend recording**: Post dividends as a debit to Retained Earnings (or Dividends Declared), credit to Cash/Dividends Payable. Tag with class or memo for AET defense documentation
- **Working capital analysis**: Pull Balance Sheet and P&L data for Bardahl formula inputs — inventory, AR, AP averages from quarterly reports

For platform-specific reporting, delegate to `qbo-integration:qbo-reporting`.

---

**Authoritative References**:
- IRC 531 — Imposition of accumulated earnings tax
- IRC 532 — Corporations subject to accumulated earnings tax
- IRC 533 — Evidence of purpose to avoid income tax
- IRC 534 — Burden of proof (notification and 60-day response)
- IRC 535 — Accumulated taxable income
- IRC 536 — Income not placed on annual basis
- IRC 537 — Reasonable needs of the business
- Treas. Reg. 1.537-1 — Reasonable needs of the business
- Treas. Reg. 1.537-2 — Grounds for accumulation
- Bardahl Manufacturing Corp. v. Commissioner, 24 T.C.M. 1030 (1965) — Working capital formula
- Bardahl International Corp. v. Commissioner, 25 T.C.M. 935 (1966) — Peak method variant
- Ivan Allen Co. v. United States, 422 U.S. 617 (1975) — Stock redemption as reasonable need
- IRS Publication 542 — Corporations (AET overview)
- TCJA Section 13001 — Corporate rate reduction to 21%
- ATRA (2012) — Permanent 20% qualified dividend rate, AET rate alignment
