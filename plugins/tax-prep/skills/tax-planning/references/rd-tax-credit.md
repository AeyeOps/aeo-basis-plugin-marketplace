---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Research & Development Tax Credit — IRC 41, Form 6765

**Purpose**: R&D tax credit eligibility (four-part test), qualified research expenses, credit calculation methods, Section 174 capitalization rules, payroll tax offset for small businesses, and documentation requirements.

**Skills**: `tx/book-tax-differences` (Section 174 book-tax timing difference)

**Tier**: 1 (year-indexed — Section 174 treatment and credit thresholds changed significantly)

---

## Four-Part Test for Qualified Research (IRC 41(d))

All four parts must be met for an activity to constitute qualified research:

### 1. Permitted Purpose (Section 174 Test)

The research must be undertaken to discover information that is **technological in nature** and intended to be useful in developing a new or improved:
- **Business component** — product, process, computer software, technique, formula, or invention
- The improvement must relate to **function, performance, reliability, or quality**

### 2. Technological Uncertainty

There must be uncertainty regarding the **capability or method** for developing or improving the product, or the **appropriate design** of the product. Uncertainty exists at the outset when the taxpayer does not know whether:
- The desired result can be achieved
- How to achieve it
- Whether the chosen approach will work

Routine engineering, cosmetic changes, or style modifications do not create technological uncertainty.

### 3. Process of Experimentation

The taxpayer must fundamentally rely on principles of the physical or biological sciences, engineering, or computer science. The research must involve a **systematic evaluation of one or more alternatives** through:
- Modeling, simulation, or systematic trial and error
- Testing hypotheses
- Refining or abandoning design alternatives

### 4. Technological in Nature

The process of experimentation must rely on the **hard sciences** — physics, chemistry, biology, engineering, computer science. Social sciences, arts, humanities, market research, and routine QC testing are excluded.

## Excluded Activities (IRC 41(d)(4))

- After commercial production (beyond the point where functional/economic requirements are met)
- Adaptation to a particular customer's requirements without technological uncertainty
- Duplication from physical examination, reverse engineering, or published specifications
- Funded research (but contract research by the payor may qualify at 65%)
- Foreign research (performed outside the U.S.)
- Internal use software (unless meeting the high threshold of innovation test, Treas. Reg. 1.41-4(c)(6))

## Qualified Research Expenses (QREs)

### In-House Research

- **Wages**: Compensation for employees who directly perform, directly supervise, or directly support qualified research. Includes salary, bonuses, and stock-based compensation allocable to qualified activities. Time allocation required — only the portion of wages attributable to qualified research counts.
- **Supplies**: Tangible property used and consumed in qualified research (lab materials, prototypes, testing materials). Does not include land, improvements to land, or depreciable property.
- **Computer use**: Rental or lease costs for computers used in qualified research (less common as a separate category post-cloud).

### Contract Research

- **65% of amounts paid** to third parties (non-employees) for qualified research performed on behalf of the taxpayer
- The taxpayer must retain **substantial rights** to the research results
- 75% inclusion if payments are to a qualified research consortium
- 100% inclusion for payments to qualified small businesses, universities, or federal laboratories (energy research credit under IRC 41(a)(3))

## Credit Calculation Methods

### Regular Credit (RC) — IRC 41(a)(1)

Credit = 20% x (QREs minus Base Amount). Base Amount = fixed-base percentage (ratio of 1984-1988 QREs to gross receipts, capped at 16%) x average gross receipts for prior 4 years. Start-ups: 3% for years 1-5, then sliding scale. Requires historical data back to 1984 — impractical for most companies.

### Alternative Simplified Credit (ASC) — IRC 41(c)(4)

Credit = 14% x (QREs minus 50% of average QREs for prior 3 years). If no prior QREs: 6% of current QREs. Most small/mid-size companies use the ASC. Election is annual on Form 6765.

### Reduced Credit Election — IRC 280C(c)(3)

The credit normally reduces the Section 174 deduction. To avoid this, elect a reduced credit = credit x (1 minus max corporate rate). At 21%: reduced credit = 79% of computed credit. Most taxpayers elect this.

## Section 174 — R&E Expenditure Treatment

### Pre-2022 (Tax Years Beginning Before January 1, 2022)

- **Immediate expensing**: R&E expenditures could be deducted currently under Section 174(a)
- Alternative: elect to capitalize and amortize over 60+ months under Section 174(b)
- Most taxpayers elected immediate expensing

### 2022-2024 (TCJA Section 13206, as Originally Enacted)

- **Mandatory capitalization**: ALL Section 174 costs (including software development) must be capitalized
- **Domestic R&E**: Amortized ratably over **5 years** (60 months), beginning at the midpoint of the year incurred
- **Foreign R&E**: Amortized ratably over **15 years** (180 months)
- No current deduction available — this was the most significant tax change affecting R&D-intensive companies
- Half-year convention applies: only 10% of first-year costs are deductible in year 1 (6 months / 60 months)

### 2025+ (P.L. 119-21, Signed January 2025)

- **Domestic R&E**: Current deduction restored. Taxpayer may deduct currently, OR capitalize and amortize over 60+ months, OR elect 10-year amortization
- **Foreign R&E**: Still must capitalize and amortize over **15 years**
- Retroactive application: taxpayers may choose to apply the restored deduction to tax years beginning after December 31, 2021 (catch-up deductions available on amended returns or in the 2025 tax year)

### Book-Tax Difference

Under ASC 730, GAAP requires **immediate expensing** of R&D costs (with narrow exceptions for software under ASC 350-40). During 2022-2024, this created a significant **temporary book-tax difference**: book expense in full in year 1, tax amortization over 5/15 years. This difference reverses over the amortization period.

## Payroll Tax Offset — IRC 41(h)

### Eligibility — Qualified Small Business (QSB)

A QSB is a corporation (or individual) with:
- **Gross receipts less than $5 million** for the current tax year, AND
- **No gross receipts for any tax year preceding the 5-tax-year period** ending with the current year (i.e., the business has existed for 5 years or less)

### Credit Amount Against Payroll Tax

- Up to **$250,000** per year against the employer's share of Social Security tax (pre-2023)
- Up to **$500,000** per year (2023 and later, per Inflation Reduction Act)
- Elected on Form 6765, applied on Form 8974 (Qualified Small Business Payroll Tax Credit)
- Offset begins in the **first calendar quarter after filing** the income tax return on which the credit is elected
- Unused payroll tax credits carry forward up to 5 years

For pre-revenue startups with no income tax liability, this converts a non-refundable income tax credit into a cash-flow benefit against payroll taxes actually being paid.

## Documentation Requirements

The IRS emphasizes **contemporaneous documentation**. After-the-fact reconstruction is permissible but carries less weight.

- **Project-level**: Technical narratives (uncertainty, alternatives, results), time tracking by project/activity, expense allocation to specific projects
- **Company-level**: QRE calculation workpapers, credit computation (RC/ASC), Section 174 amortization schedule (2022-2024), Form 6765 with backup

Common failures: no time tracking, no technical narrative, including non-qualifying activities (routine development, bug fixes), missing contract research agreements.

## Decision Points

1. **Does the activity meet all four parts of the test?** The permitted-purpose and technological-uncertainty prongs are where most claims fail. If the work is routine application of known techniques, it does not qualify.
2. **Regular credit or ASC?** Unless the company has reliable 1984-1988 QRE data, use the ASC.
3. **Reduced credit election?** Almost always yes — avoids the Section 280C(c) deduction reduction complexity.
4. **Payroll tax offset?** Check QSB eligibility (gross receipts <$5M, existence <=5 years). If eligible, the payroll offset provides immediate cash benefit.
5. **Section 174 treatment?** For 2022-2024 returns, mandatory 5-year/15-year amortization. For 2025+, current deduction is restored. Consider amending 2022-2024 returns to claim retroactive catch-up deductions under P.L. 119-21.
6. **State R&D credits?** Many states offer conforming or separate R&D credits. Florida does not have a general R&D tax credit at the state level (FL corporate tax starts with federal taxable income, so the federal credit indirectly reduces FL tax base).

## Accounting System Integration Notes

For platform-specific account setup and journal entry posting, invoke `qbo-integration:qbo-bookkeeping`.

- R&D expenses should be posted to specific expense accounts (e.g., "R&D Wages," "R&D Supplies," "R&D Contract Research") to facilitate QRE identification at year-end
- Section 174 amortization for 2022-2024: book the full expense in the accounting system (GAAP treatment), then make a tax adjustment on the return. No GL journal entry needed for the tax capitalization — it is a return-level M-1/M-3 adjustment.
- If electing to capitalize for book purposes (rare), set up an intangible asset account and amortize via monthly journal entries
- Payroll tax offset: the credit reduces payroll tax deposits. Most payroll systems do not automatically apply Form 8974 credits — manual adjustment to deposit amounts required.

## Year-by-Year Variance Table

| Tax Year | Section 174 Treatment | R&D Credit Rate (RC/ASC) | Payroll Tax Offset Cap | Key Changes |
|----------|----------------------|-------------------------|----------------------|-------------|
| 2007-2009 | Current deduction (elective) | 20% RC / N/A (ASC not yet available for all) | N/A | ASC became available for tax years ending after 12/31/2006 |
| 2010-2014 | Current deduction (elective) | 20% RC / 14% ASC | N/A | Credit extended repeatedly but temporarily |
| 2015 | Current deduction (elective) | 20% RC / 14% ASC | $250,000 | PATH Act: credit made permanent, payroll offset enacted for QSBs |
| 2016-2021 | Current deduction (elective) | 20% RC / 14% ASC | $250,000 | Stable period |
| 2022 | **5-yr domestic / 15-yr foreign amortization (mandatory)** | 20% RC / 14% ASC | $250,000 | TCJA Section 13206 takes effect — no current deduction |
| 2023 | 5-yr / 15-yr amortization (mandatory) | 20% RC / 14% ASC | **$500,000** | IRA doubles payroll offset cap |
| 2024 | 5-yr / 15-yr amortization (mandatory) | 20% RC / 14% ASC | $500,000 | Continued mandatory capitalization |
| 2025 | **Current deduction restored (domestic)** / 15-yr foreign | 20% RC / 14% ASC | $500,000 | P.L. 119-21 restores Section 174 expensing; retroactive election available for 2022-2024 |
| 2026+ | Current deduction (domestic) / 15-yr foreign | 20% RC / 14% ASC | $500,000 | Unless Congress changes |

---

**Authoritative References**:
- IRC 41 — Credit for increasing research activities
- IRC 174 — Research and experimental expenditures (as amended by TCJA Section 13206 and P.L. 119-21)
- IRC 280C(c) — Reduction of deduction for R&D credit
- IRC 41(h) — Payroll tax offset for qualified small businesses
- Treas. Reg. 1.41-1 through 1.41-9 — R&D credit regulations
- Treas. Reg. 1.174-1 through 1.174-4 — Section 174 regulations
- Form 6765 Instructions — Credit for Increasing Research Activities
- Form 8974 Instructions — Qualified Small Business Payroll Tax Credit
- ASC 730 — Research and Development (GAAP)
- ASC 350-40 — Internal-Use Software (GAAP capitalization exception)
- PATH Act of 2015 (P.L. 114-113) — Made R&D credit permanent, added payroll offset
- Inflation Reduction Act of 2022 (P.L. 117-169) — Doubled payroll offset to $500K
- P.L. 119-21 — Restored Section 174 current deduction for domestic R&E (2025+)
