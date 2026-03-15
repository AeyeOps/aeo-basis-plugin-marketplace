# Tax Rate Parameters and Reform Modeling

Reference for tax law parameter catalogs, rate/bracket/threshold structures, reform
modeling via parameter overrides, tax function execution order, and distributional
analysis. Focused on US federal individual income and payroll taxes, but the parameter
architecture and reform modeling patterns are applicable to any tax computation system
including corporate returns.

## Architecture Overview

Four core concepts form the simulation pipeline:

- **Policy** -- tax law parameters (rates, brackets, thresholds) for a range of tax years (e.g., 2013-2035)
- **Records** -- taxpayer microdata (one row per filing unit) with growth factors and sample weights
- **Calculator** -- binds Policy + Records, runs all tax functions, produces results
- **Parameters** -- abstract base providing parameter management, validation, and indexing

Supporting concepts:
- **Growth factors** -- CSV-based growth rates (wage, price, benefit categories) for extrapolating data across years
- **Growth differentials** -- modifications to baseline growth factors for economic scenario modeling
- **Consumption** -- marginal propensity to consume parameters and benefit valuation

## Policy Parameters

### Parameter Catalog Structure

A comprehensive parameter catalog (~800+ parameters) defines current law across all
relevant tax years. Each parameter entry specifies:
- Title and description
- Section classification (e.g., "Payroll Taxes > Social Security FICA")
- Whether the parameter is indexable and whether it is currently indexed
- Data type (float, int, bool)
- Year-value pairs (the parameter's value for each tax year)
- Validation ranges (min/max)

### Parameter Dimensions

Parameters are dimensioned by:
- **Year** (e.g., 2013-2035)
- **Filing status** (single, married filing jointly, married filing separately, head of household, qualifying widow/er) -- 5 values
- **EITC children count** (0, 1, 2, 3+) -- 4 values

Array parameters use the filing status dimension (5 values) or EITC dimension (4 values).

### Indexing

- Most dollar-amount parameters are **price-indexed** using CPI-U inflation rates
- Two parameters are **wage-indexed**: Social Security earnings cap and threshold
- A CPI offset parameter allows shifting the inflation rate used for indexing (always zero in current law; nonzero in reforms like TCJA's chained-CPI switch)
- Indexing applied by multiplying by `(1 + rate)` per year beyond the last known value

## Key Parameter Families

### Payroll Taxes
- OASDI rates (employer/employee, 6.2% each)
- Medicare/HI rates (employer/employee, 1.45% each)
- Social Security earnings cap (wage-indexed, e.g., $113,700 in 2013)
- Additional Medicare Tax threshold (filing-status-dependent)

### Income Tax Rates and Brackets
- 7 ordinary income tax rates (`rate_1` through `rate_7`)
- 6 bracket breakpoints (filing-status arrays, 5 values each)
- Personal exemption amount and phaseout start

### Standard Deduction
- Base amounts by filing status (5-element array)
- Additional amounts for age 65+ by filing status
- Dependent standard deduction

### Itemized Deductions
- Medical expense floor rate (AGI fraction)
- SALT cap by filing status (unlimited pre-TCJA as 9e99, $10K under TCJA)
- Charity contribution rate ceiling
- Pease limitation parameters (phaseout start, reduction rate, ceiling)

### Capital Gains
- 3 bracket breakpoints for preferential rates
- 4 tax rates (0%, 15%, 20%, plus NIIT)

### Child Tax Credit
- CTC amount per child ($2,000 under TCJA)
- Phaseout start by filing status
- Additional (refundable) CTC maximum
- Other Dependent Credit amount

### EITC
- Maximum credit by number of children (4-element array)
- Credit rates, phaseout start, phaseout rates

### AMT
- AMT exemption by filing status
- AMT exemption phaseout start
- AMT tax rates (two brackets)

### QBI Deduction (TCJA)
- QBI deduction rate (20% under TCJA)
- Income threshold and phaseout range

### Hypothetical/Reform-Only Parameters
- Universal Basic Income amounts (zero in current law)
- Fair Share Tax thresholds
- Lump Sum Tax amount
- AGI surtax rate and threshold

## Taxpayer Microdata

### Data Sources

Multiple microdata sources can drive the simulation:
- **Public survey data** (e.g., Current Population Survey) -- bundled, publicly available
- **IRS statistical samples** (e.g., Public Use File) -- private, requires separate access
- **Tax microdata** -- synthetic or actual tax return data

Custom microdata can also be supplied as a data frame with required columns.

### Variable Categories

**Input variables** (from microdata):
- Filing status (1-5 mapping)
- Income components: wages, interest, dividends, qualified dividends, Schedule C, IRA, pensions, Schedule E, farm, unemployment, Social Security
- Split variables (taxpayer + spouse must sum to total): wages, Schedule C, farm
- Capital gains: short-term, long-term
- Itemized deduction components: medical, state/local taxes, real estate taxes, interest, cash charity, non-cash charity, miscellaneous
- Demographics: age (head/spouse), exemptions count, EIC qualifying children (0-3), dependent status, blind status, counts by age group
- Benefits: Medicare, Medicaid, SSI, SNAP, housing, TANF, veterans, WIC
- Sample weight (represents number of filing units in population)

**Computed variables** (outputs):
- AGI, taxable Social Security, above-the-line deductions
- Itemized deductions (after limitations), personal exemptions (after phaseout)
- Regular taxable income, tax from rate schedule
- Total income tax before credits, nonrefundable credits, income tax after credits
- AMT liability, regular tax before credits
- Standard deduction amount
- Final individual income tax liability, total payroll tax, combined tax
- Expanded income (broad measure including benefits), after-tax income
- EITC, total CTC, total refundable credits
- NIIT, self-employment tax, FICA components, Additional Medicare Tax
- Itemized deduction components, QBI deduction
- Marginal tax rates (payroll, income, combined)

### Data Extrapolation

When growth factors and weights are provided, microdata are extrapolated across years:
- Each income variable multiplied by its corresponding growth factor (e.g., wages by wage growth, interest by interest growth)
- Sample weights change by year
- Price inflation derived from CPI-U growth factors
- Wage growth derived from wage growth factors

## Tax Function Execution Order

The core computation follows this sequence:

```
1.  Payroll Tax          -- OASDI + HI, self-employment tax, earned income
2.  Dependent Care       -- above-the-line dependent care deduction
3.  Adjustments          -- above-the-line deductions
4.  Investment Income    -- investment income exclusion base
5.  Capital Gains        -- capital gains, modified income variables
6.  Social Security      -- taxable Social Security benefits
7.  AGI                  -- Adjusted Gross Income + personal exemption
8.  Miscellaneous Ded    -- senior/overtime/tip deductions
9.  Itemized Deductions  -- with SALT cap, Pease limitation
10. Additional Medicare  -- Additional Medicare Tax on high earners
11. Standard Deduction   -- standard deduction computation

[Optimal deduction choice: compute taxes with std ded vs itemized, pick lower]

12. Taxable Income       -- with QBI deduction
13. Rate Schedule Tax    -- 7-bracket rate schedule
14. Capital Gains Tax    -- preferential rates on LTCG + qualified dividends
15. AGI Surtax           -- (reform-only)
16. NIIT                 -- 3.8% on net investment income
17. AMT                  -- Alternative Minimum Tax

18. Dependent Care Credit    -- Form 2441
19. EITC                     -- Earned Income Tax Credit
20. Payroll Tax Credit       -- (reform-only)
21. Personal Tax Credit      -- (reform-only)
22. Education Credits        -- American Opportunity + Lifetime Learning
23. Elderly/Disabled Credit  -- Schedule R
24. Charity Credit           -- (reform-only)
25. Child Tax Credit         -- CTC + ODC + ACTC
26. Nonrefundable Credits    -- aggregate
27. Additional CTC           -- refundable portion
28. Form 1040 Computation   -- tax after credits + other taxes
29. Alternative CTC          -- (reform-only)
30. Final Income Tax         -- iitax = tax - refund + surtaxes
```

### Key Computation Details

**Payroll Tax**: OASDI at statutory rates on earnings up to the cap; Medicare/HI on all earnings (no cap); self-employment tax on Schedule C + farm + partnership income; deductible half of SE tax

**Capital Gains**: Loss limitation (e.g., -$3,000 max); modified income variables for downstream calculations

**Social Security Taxability**: Two-threshold formula (50% and 85% inclusion tiers)

**AGI**: Sum of all income components minus above-the-line deductions plus taxable Social Security

**Itemized Deductions**: Medical (AGI floor), SALT (capped), interest (capped), charity (rate-limited), Pease limitation (phase-out)

**Optimal Deduction Choice**: System automatically computes taxes under both standard and itemized deductions, picks the lower-tax option

**Taxable Income**: AGI - max(itemized, standard) - personal exemption - QBI deduction

**Rate Schedule Tax**: 7-bracket progressive rates applied to taxable income

**Capital Gains Tax**: Preferential rates (0%/15%/20%) on LTCG and qualified dividends, stacked on top of ordinary income for bracket determination

**AMT**: AMT income = AGI + preference items - AMT exemption (with phaseout); two-bracket AMT rates; liability = max(0, tentative AMT - regular tax)

**NIIT**: 3.8% on lesser of net investment income or AGI above threshold

**EITC**: Phase-in rate on earned income up to max credit; phase-out above AGI threshold; investment income disqualification test

**Child Tax Credit**: Per-child amount with AGI phaseout; split into nonrefundable (limited by tax liability) and refundable (15% of earned income above threshold)

**Final Tax**: Income tax after credits + other taxes - refundable credits + surtaxes

## Reform Modeling

### Reform Dictionary Format

Reforms are expressed as parameter override dictionaries:
```
parameter_name: {year: new_value}
```

Examples:
- Scalar change: top rate to 41% in 2024
- Array change: standard deduction to [15000, 30000, 15000, 22000, 30000] in 2024 (by filing status)
- Indexing change: turn on/off inflation indexing for a parameter starting in a given year

Values set for a given year are automatically indexed forward from that year (unless indexing is turned off).

### TCJA as Example Reform

Key TCJA changes encoded as parameter overrides:
- Lower rates (e.g., 12% replacing 15%, 22% replacing 25%)
- Higher standard deduction ($12,000/$24,000 replacing ~$6,350/$12,700)
- Eliminated personal exemption ($0 replacing ~$4,050)
- SALT cap ($10,000 replacing unlimited)
- Higher CTC ($2,000 replacing $1,000, higher phaseout start)
- QBI deduction (20% rate)
- Higher AMT exemption
- Chained CPI (CPI offset of -0.25%)
- Most provisions sunset in 2026 (reform specifies both 2018 and 2026 values)

### Built-in Reform Files

Pre-encoded reform scenarios:
- Pre-TCJA law extrapolated forward (baseline for TCJA analysis)
- TCJA (Tax Cuts and Jobs Act)
- ARPA (American Rescue Plan Act)
- CARES Act
- TCJA extension provisions
- Various legislative proposals

### Comparing Reforms

Standard workflow:
1. Create baseline calculator (e.g., pre-TCJA law)
2. Create reform calculator (e.g., TCJA applied on top of baseline)
3. Run both calculators for the same tax year
4. Compare weighted totals, distribution tables, or per-unit results

## Distributional Analysis

### Distribution Table Variables

Key variables for distributional analysis:
- Expanded income (broad income including benefits)
- AGI, after-tax income
- Standard deduction, itemized deductions, personal exemptions
- Taxable income, regular tax, AMT income, AMT liability
- Tax before credits, nonrefundable credits
- Income tax, payroll tax, combined tax
- Refundable credits, UBI, benefit costs/values

### Grouping Options

- **Weighted deciles**: 10 deciles plus top-decile detail (90-95, 95-99, 99-100) and bottom-decile split (negative, zero, positive income)
- **Standard income bins**: dollar income ranges
- **SOI AGI bins**: IRS Statistics of Income AGI categories

### Marginal Tax Rates

Computed via finite difference method:
- Perturb income by a small amount
- Measure change in tax
- Reports separate payroll, income, and combined marginal rates
- Can include employer payroll tax share in denominator for full-compensation analysis

## Important Design Notes

- Policy and Records are deep-copied at calculator construction -- original objects are reusable for multiple scenarios
- Tax functions must be called before reading computed variables
- Data extrapolation only moves forward in time, never backward
- The optimal standard-vs-itemized deduction choice is made automatically
- "Effectively infinite" caps use very large sentinel values (meaning no cap applies)
- Filing status codes: 1=single, 2=joint, 3=separate, 4=head-of-household, 5=widow(er)
- All dollar amounts are in nominal (not inflation-adjusted) terms
- Sample weights represent population counts (one record may represent thousands of filing units)
