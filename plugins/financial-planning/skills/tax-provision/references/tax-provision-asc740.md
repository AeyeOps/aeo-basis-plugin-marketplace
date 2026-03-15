# Income Tax Provision — ASC 740

**Purpose**: Compute income tax expense for financial statements under ASC 740, including current/deferred tax, DTA/DTL measurement, valuation allowance, uncertain tax positions, and rate reconciliation.

**Skills**: —

**Tier**: 3 (evergreen GAAP standard — ASC 740 has not changed substantively)

---

## Overview

ASC 740 (formerly SFAS 109) governs accounting for income taxes in financial statements prepared under U.S. GAAP. The provision has two components:

- **Current tax expense**: Taxes payable (or refundable) for the current year based on taxable income
- **Deferred tax expense**: Future tax consequences of temporary differences between book and tax basis of assets and liabilities

Total income tax expense = current + deferred. This amount appears on the income statement; the balance sheet carries current tax payable/receivable and deferred tax assets/liabilities.

## Step 1 — Identify Temporary Differences

Compare the **book basis** (financial statement carrying amount) to the **tax basis** (amount used for tax purposes) of every asset and liability.

### Taxable Temporary Differences (create DTL)

The book basis of the asset exceeds its tax basis, OR the book basis of the liability is less than its tax basis. The difference will result in **taxable amounts in future years** when the asset is recovered or the liability is settled.

Common C-corp examples:
- **Depreciation**: MACRS tax basis < book basis (accelerated tax depreciation creates a DTL that reverses as the asset ages)
- **Installment sale receivables**: Gain recognized for book but deferred for tax
- **Prepaid expenses**: Deducted for tax when paid but amortized for book over the benefit period

### Deductible Temporary Differences (create DTA)

The book basis of the asset is less than its tax basis, OR the book basis of the liability exceeds its tax basis. The difference will result in **deductible amounts in future years**.

Common C-corp examples:
- **Accrued liabilities**: Expenses accrued for book but not deductible for tax until economic performance (warranties, litigation reserves, vacation accruals)
- **Bad debt reserves**: Book provision method vs tax specific charge-off requirement
- **NOL carryforwards**: Deductible in future years when utilized against taxable income
- **Capital loss carryforwards**: Usable against future capital gains (5-year carryforward for C-corps)
- **Tax credit carryforwards**: Reduce future tax liability when utilized

## Step 2 — Measure DTA and DTL

Multiply each temporary difference by the **enacted tax rate** expected to be in effect when the difference reverses.

- Post-TCJA C-corp federal rate: **21%** (flat rate, no graduated brackets)
- State rate: Florida corporate income tax rate (currently 5.5% on income over $50,000 exemption; the exemption is reduced/eliminated in some years)
- Combined rate: Approximate by computing federal + state net of federal benefit, or use a blended rate if the entity operates in multiple states

### Rate Changes

When a tax rate change is enacted, **remeasure all existing DTA/DTL** at the new rate in the period of enactment. The adjustment flows through income tax expense in that period. Example: If a future rate change from 21% to 25% is enacted, all DTAs and DTLs are remeasured — DTAs increase in value, DTLs increase in obligation.

### Scheduling

For jurisdictions with graduated rates or rate changes, schedule the reversal of each temporary difference by year to apply the correct rate. For a flat-rate jurisdiction (federal post-TCJA), scheduling is unnecessary — all differences reverse at 21%.

## Step 3 — Valuation Allowance

Assess whether it is **"more likely than not"** (greater than 50% likelihood) that some or all of the DTA will **not be realized**. If so, record a valuation allowance to reduce the DTA to the amount that is more likely than not to be realized.

### Sources of Evidence

**Positive evidence** (supports realization):
- Existing contracts or firm sales backlog producing future taxable income
- Appreciated asset values in excess of tax basis (built-in gains)
- History of strong, consistent earnings
- Reversing taxable temporary differences that will produce taxable income in the carryforward period
- Tax planning strategies that are prudent and feasible

**Negative evidence** (weighs against realization):
- Cumulative losses in recent years (3-year cumulative loss is significant negative evidence under ASC 740-10-30-21)
- History of NOL or credit carryforwards expiring unused
- Unsettled circumstances that could adversely affect future operations
- Brief carryback/carryforward period limiting utilization window

**Weighting**: Objectively verifiable evidence (existing contracts, historical earnings) receives greater weight than projections. Negative evidence generally requires more positive evidence to overcome — particularly a pattern of losses.

### Judgment Area

Valuation allowance is one of the most significant judgment areas in financial reporting. It directly impacts net income and can trigger restatement risk if the assessment changes. Document the analysis thoroughly.

## Step 4 — Current Tax Expense

Current tax expense = taxable income x enacted rate, adjusted for tax credits.

Taxable income is computed on the tax return (Form 1120 for C-corps). The current provision workpaper starts with book income and adjusts for permanent and temporary differences to arrive at taxable income, then applies the statutory rate.

### Components

- Federal current tax expense: Taxable income x 21%
- State current tax expense: State taxable income x state rate (Florida: 5.5% on apportioned income over $50,000)
- Tax credits applied: R&D credit (Section 41), foreign tax credit (Section 901), general business credits — reduce current tax payable dollar-for-dollar

## Step 5 — Rate Reconciliation

Reconcile the **statutory rate** (21% federal) to the **effective tax rate** (total income tax expense / pre-tax book income). The reconciliation explains every basis-point difference.

### Common Reconciliation Items

- **State income taxes**: Net of federal benefit (state rate x (1 - federal rate))
- **Permanent differences — addbacks**: Meals (50% nondeductible portion), penalties and fines, political contributions, life insurance premiums on officers
- **Permanent differences — exclusions**: Tax-exempt interest income, life insurance proceeds, dividends received deduction
- **Tax credits**: R&D credit, foreign tax credit (reduce effective rate below statutory)
- **Rate changes**: Remeasurement of DTA/DTL when enacted rates change
- **Valuation allowance changes**: Increase in VA raises effective rate; release of VA lowers effective rate
- **Return-to-provision adjustments**: True-up when the filed return differs from the original provision estimate
- **Prior year adjustments**: Audit settlements, amended returns, refund claims

### Disclosure

The rate reconciliation is a required disclosure (ASC 740-10-50-12) and is closely scrutinized by auditors and regulators. Each item exceeding 5% of the statutory rate amount must be separately disclosed.

## Uncertain Tax Positions (ASC 740-10-25)

Formerly FIN 48 (FASB Interpretation No. 48), codified into ASC 740. Applies a two-step process to every tax position taken or expected to be taken on a tax return.

### Step 1 — Recognition

Does the tax position meet the **"more likely than not"** threshold (>50% likelihood) of being sustained on examination, based solely on the technical merits? Consider:
- Tax law, regulations, and case law
- IRS rulings and administrative guidance
- Taxpayer's experience with similar positions

If yes: recognize the tax benefit. If no: do not recognize any benefit (record a liability for the full tax effect).

### Step 2 — Measurement

For recognized positions, measure the benefit at the **largest amount** that has a greater than 50% likelihood of being realized upon ultimate settlement. This is a cumulative probability assessment — list possible outcomes, assign probabilities, and find the largest amount where cumulative probability exceeds 50%.

### Interest and Penalties

ASC 740-10-45-25 permits an accounting policy election to classify interest and penalties related to uncertain tax positions as either income tax expense or a separate line item (e.g., interest expense, other expense). The election must be disclosed and applied consistently.

### Disclosure Requirements

- Tabular rollforward of unrecognized tax benefits (beginning balance, additions, reductions, settlements, lapse of statute)
- Positions that could change within 12 months
- Cumulative interest and penalties recognized
- Tax years open to examination by major jurisdiction

## Quarterly Provision — ASC 740-270

### Estimated Annual Effective Tax Rate (EAETR)

At each interim reporting period, estimate the **annual effective tax rate** for the full year and apply it to year-to-date ordinary income. The EAETR includes:
- Estimated annual pre-tax income
- Anticipated permanent differences for the full year
- Projected tax credits
- Expected changes in valuation allowance related to ordinary income

### Discrete Items

Certain items are recognized entirely in the quarter they occur, NOT spread through the EAETR:
- Effects of enacted tax law changes
- Changes in judgment about valuation allowances
- Return-to-provision true-ups
- Tax effects of unusual or infrequent items
- Settlements of uncertain tax positions

### Interim Calculation

Quarter tax expense = (year-to-date ordinary income x EAETR) - tax expense recognized in prior quarters + discrete items for the current quarter.

## Common C-Corp DTA/DTL Items

| Item | DTA or DTL | Typical Source |
|------|-----------|----------------|
| MACRS depreciation > book depreciation | DTL | Fixed assets |
| Accrued expenses not yet deductible | DTA | Warranty, vacation, litigation reserves |
| NOL carryforward | DTA | Prior year losses |
| Capital loss carryforward | DTA | Net capital losses (5-year CF) |
| Bad debt reserve | DTA | Allowance for doubtful accounts |
| Prepaid expenses deducted for tax | DTL | 12-month rule prepayments |
| Section 263A UNICAP adjustment | DTA | Additional capitalized costs in inventory |
| Tax credit carryforward | DTA | R&D, FTC, general business credits |
| Deferred revenue taxable on receipt | DTA | Advance payments (IRC 451(c)) |
| Section 174 capitalization (2022-2024) | DTA | R&D costs amortized for tax, expensed for book |

## Decision Points

1. **Is a valuation allowance needed?** Weigh positive vs negative evidence; 3-year cumulative loss is a strong indicator. Document the analysis regardless of conclusion
2. **Which rate to use for measurement?** Use the enacted rate expected at reversal. For federal C-corp post-TCJA: 21%. If rate change legislation is proposed but not enacted, do not use the proposed rate
3. **Does an uncertain tax position exist?** Any position where the technical merits are debatable should be evaluated under the two-step process
4. **Is the position more likely than not?** If below 50%, no benefit is recognized — full liability accrued
5. **How to handle state taxes?** Compute state DTA/DTL separately or as part of a blended rate, depending on complexity and materiality
6. **Quarterly vs annual?** Public companies must compute quarterly provisions using EAETR; private companies may compute only annually but best practice is quarterly

## Accounting System Integration Notes

- **Tax provision is computed outside the accounting system** in a workpaper (Excel, dedicated tax provision software, or manual schedule)
- **Single journal entry**: Post the provision as one or two journal entries — (1) Current: debit Income Tax Expense, credit Income Tax Payable; (2) Deferred: debit/credit Deferred Tax Asset and Deferred Tax Liability with corresponding debit/credit to Deferred Income Tax Expense
- **Balance sheet classification**: All deferred tax is classified as **noncurrent** under ASU 2015-17 (effective for public entities 2016+, all entities 2017+)
- **Valuation allowance**: Record as a contra-asset to the DTA in the accounting system (credit Valuation Allowance, debit Income Tax Expense)
- **Quarterly entries**: If computing quarterly provisions, post adjusting entries each quarter and true up at year-end when the return is filed
- **Reconciliation**: The income tax expense per the accounting system income statement should tie to the provision workpaper. Any difference indicates a posting error
- For platform-specific journal entry posting, invoke `qbo-integration:qbo-audit` for QBO environments

## Authoritative References

- ASC 740-10 — Income Taxes (overall)
- ASC 740-10-25 — Recognition (uncertain tax positions, formerly FIN 48)
- ASC 740-10-30 — Initial measurement (DTA/DTL, valuation allowance)
- ASC 740-10-45 — Other presentation matters (classification, interest/penalty policy)
- ASC 740-10-50 — Disclosure requirements (rate reconciliation, DTA/DTL detail, uncertain positions)
- ASC 740-270 — Interim reporting (EAETR methodology)
- ASU 2015-17 — Balance Sheet Classification of Deferred Taxes (all noncurrent)
- IRC Section 11 — Tax rate (21% post-TCJA)
- Form 1120, Schedule J — Tax Computation
- IRS Publication 542 — Corporations
