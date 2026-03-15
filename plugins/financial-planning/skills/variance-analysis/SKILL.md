---
name: variance-analysis
description: >
  Contains verified 4-tier benchmarking data source hierarchy (RMA/BizMiner/IRS SOI/SEC EDGAR
  with 5-criteria peer group selection and quartile ranking), management reporting KPI computation engine
  (accounting expressions, pro-rata budget comparison, PDF/XLSX/dashboard output), and
  price/volume/mix/timing variance decomposition formulas with materiality thresholds. Budget-vs-actual
  analysis, common-size P&L and balance sheet, financial ratio comparison (DSO, margins, current
  ratio, debt-to-equity, ROIC), executive commentary, management reporting. Consult when computing
  BvA variances, decomposing unfavorable cost or revenue drivers, designing KPI dashboards with
  accumulation methods, benchmarking client ratios against industry peers by NAICS code,
  producing board-ready management reports, writing variance narrative, or comparing effective
  tax rates to industry norms for C-corporations.
---

# Variance Analysis

Operational skill for budget-vs-actual analysis, KPI computation, industry benchmarking,
and management report production for C-corporations. This skill turns raw financial data
into actionable performance insights for management, lenders, and boards.

## BvA Variance Computation

### Core Formulas

- **Dollar variance** = Actual - Budget (positive = favorable for revenue, unfavorable for expense)
- **Percentage variance** = (Actual - Budget) / |Budget|
- **Year-over-year variance** = Current Period - Prior Period (same computation, different baseline)

Sign convention matters: for revenue lines, positive variance is favorable (beat budget);
for expense lines, positive variance is unfavorable (overspent). Always label F/U
(favorable/unfavorable) explicitly rather than relying on sign alone.

### Materiality Thresholds for Investigation

Not every variance warrants analysis. Apply dual threshold (both must be met for
immaterial classification):

- **Percentage gate**: variance exceeds 10% of budget line
- **Dollar gate**: variance exceeds $1,000 (adjust for entity size; $5,000+ for entities over $10M revenue)

Variances below both gates can be noted but need not be explained. Variances exceeding
either gate require decomposition and narrative explanation.

### Period Alignment

When comparing actuals to budget, periods must align precisely. Budget amounts stored on
a per-period basis should be pro-rated if the reporting period does not match the budget
period exactly:

- **Sum-type KPIs** (revenue, expenses): `adjusted_budget = annual_budget * (reporting_days / total_days)`
- **Average-type KPIs** (headcount, ratios): weighted average across overlapping periods
- **YTD analysis**: sum all completed periods; partial-month pro-rata for mid-month reports

For MIS-style reporting systems, pro-rata temporis adjustment handles this automatically:
when a budget item spans a longer period than the report column, the system computes
`amount * overlap_days / item_total_days` for sum-accumulation KPIs and weighted averages
for avg-accumulation KPIs.

## Variance Decomposition

Break aggregate variances into actionable components. The goal is to answer "why did we
miss?" not just "by how much?"

### Price Variance

Measures the impact of rate/price differences holding volume constant.

`Price variance = (Actual price - Budget price) * Actual quantity`

Examples: hourly billing rate changes, material cost increases, salary rate differences.

### Volume Variance

Measures the impact of quantity/activity differences holding price constant.

`Volume variance = (Actual quantity - Budget quantity) * Budget price`

Examples: units sold above/below plan, billable hours over/under target, headcount changes.

### Mix Variance

Measures the impact of product/service/channel mix shifts on blended margins.

`Mix variance = (Actual mix % - Budget mix %) * Actual total volume * Budget margin per unit`

Relevant when the entity sells multiple products/services at different margins. A shift
toward lower-margin services reduces blended margin even if total volume meets budget.

### Timing Variance

Measures the impact of revenue/expense recognition timing differences between periods.

Common causes: accelerated or deferred revenue recognition, prepaid expense amortization
timing, seasonal patterns not reflected in straight-line budget allocation. Timing
variances often net to zero over the full year but distort monthly and quarterly reports.

### Decomposition Hierarchy

For a complete explanation, decompose in order: (1) volume, (2) price, (3) mix. Residual
after these three is the interaction effect (price * volume cross-term) which is typically
small and can be allocated proportionally or disclosed as "interaction."

## KPI Design and Tracking

### KPI Selection Framework

Choose KPIs that connect operational activity to financial outcomes. A well-designed KPI
set covers four perspectives:

- **Financial** -- revenue growth rate, gross margin %, operating margin %, net margin %, EBITDA margin, revenue per employee
- **Efficiency** -- DSO (days sales outstanding), DPO (days payable outstanding), inventory turnover, asset turnover, working capital cycle days
- **Liquidity** -- current ratio, quick ratio, cash ratio, operating cash flow ratio
- **Leverage** -- debt-to-equity, debt-to-assets, interest coverage (EBIT / interest expense; below 1.5x is a warning)

For C-corporations, also track: effective tax rate (tax expense / pre-tax income), cash
tax rate, return on equity (ROE), return on invested capital (ROIC = NOPAT / invested
capital).

### KPI Properties

Each KPI should be defined with:

- **Name** (valid identifier for formula references)
- **Type** -- numeric (`num`), percentage (`pct`), or string (`str`)
- **Compare method** -- `diff` (absolute delta), `pct` (percentage change), or `none`
- **Accumulation method** -- `sum` (additive across periods), `avg` (weighted average), `none`
- **Expression** -- the formula that computes the value from GL data or other KPIs

### KPI Expression Patterns

KPIs can reference GL account balances directly or build on other KPIs:

- Direct from GL: reference account ranges for balance, debit, or credit sums over the period, at period start (initial), or at period end (ending)
- Derived: reference other KPI names in Python-style expressions (e.g., `gross_profit / net_revenue`)
- Conditional: use conditional expressions to avoid division by zero (e.g., `operating_income / net_revenue if net_revenue > 0 else None`)
- Cross-statement: reference sub-report KPIs to pull balance sheet values into a P&L-based report (e.g., `balance_sheet.current_assets / balance_sheet.current_liabilities`)

### Dashboard Reporting

For dashboard-style KPI presentation:

- Arrange KPIs in rows, time periods in columns
- Support comparison modes: actual vs budget, actual vs prior year, multi-period trend
- Comparison columns compute deltas automatically using the KPI's compare method (diff or pct)
- Sum columns aggregate across source columns respecting accumulation method
- Style KPIs with conditional formatting: font weight, color, decimal places, dividers (thousands/millions), prefix/suffix
- Hide rows that have no data (`hide_empty`) or are intermediate calculations (`hide_always`)
- Enable drill-down from any cell to the underlying journal entries for auditability

## Industry Benchmarking

### Data Source Selection

Choose the benchmarking source based on budget and precision needs:

- **Tier 1 -- RMA Annual Statement Studies**: Gold standard. 260,000+ financial statements from commercial bank lending officers. Organized by 6-digit NAICS with revenue-size breakdowns. Provides quartile distributions (upper quartile, median, lower quartile) for 16 common-size ratios. Subscription required (~$300+/year). Limitation: skews toward borrowing companies.
- **Tier 2 -- Commercial services**: BizMiner (stronger small-business and geographic coverage, pay-per-report), IBISWorld (better for market context and trend analysis, institutional subscription).
- **Tier 3 -- Free government sources**: IRS Statistics of Income (aggregate corporate data by industry/asset size, 2-3 year lag, no quartiles), Census Bureau Annual Business Survey (sizing data), BLS QCEW (payroll/employment for comp-to-revenue benchmarking).
- **Tier 4 -- Public company comparables**: SEC EDGAR 10-K/10-Q filings. Directional reference only for private company clients due to size and complexity differences.

### Peer Group Construction

Peer group quality determines benchmarking validity. Select in priority order:

1. **NAICS code** -- use most specific level available (6-digit preferred); a general contractor (236220) should not benchmark against highway paving (237310)
2. **Revenue size band** -- a $2M company operates differently from a $50M company in the same industry
3. **Geographic market** -- labor, real estate, and competitive dynamics vary by region; national data acceptable when regional unavailable but note the limitation
4. **Business maturity** -- exclude startups (<3 years) from established-business benchmarks and vice versa
5. **Legal form** -- C-corps, S-corps, and partnerships have structurally different financials (officer compensation, tax treatment); match form when possible

### Quartile Positioning and Interpretation

Position each client ratio within the industry distribution: upper quartile (top 25%),
median, lower quartile (bottom 25%).

- Ratios at median are average, not necessarily "good" -- may not align with client goals
- Outlier ratios (above UQ or below LQ) are the primary discussion points
- Investigate before presenting: an unusually high current ratio may signal idle cash (inefficiency), not superior liquidity; an unusually low margin may reflect a deliberate pricing strategy
- Always present trend alongside point-in-time: a company at the 40th percentile but improving tells a different story than one at the 60th and declining

### Translating Ratios to Narrative

Raw numbers without context are meaningless to most clients. Translate:

- "DSO is 52 days vs industry median 38 days" becomes "Collecting 14 days slower than peers, tying up ~$X in working capital"
- Identify what is actionable (collection procedures, pricing) vs structural (business model, geography)
- Acknowledge limitations: NAICS grouping breadth, size effects, accounting method differences (cash vs accrual), survivorship bias in benchmark databases, data lag (1-3 years)

### Benchmarking Frequency

- Annual for most clients
- Quarterly for clients actively improving performance or preparing for financing
- On-demand for loan applications (lenders compare applicants to industry norms)

## Common-Size Analysis

Express every line item as a percentage of a base to enable cross-period and cross-company
comparison regardless of size differences.

- **Common-size P&L**: each line as % of net revenue
- **Common-size balance sheet**: each line as % of total assets

### Trend Identification (3-5 Period Minimum)

Compare common-size percentages across periods to surface:

- **Margin compression/expansion** -- rising COGS % or SG&A % signals cost pressure
- **Revenue concentration** -- single customer or product line growing disproportionately
- **Working capital drift** -- AR % of assets growing (collection slowdown), inventory % growing (overstocking)
- **Leverage creep** -- total liabilities % of assets rising period over period
- **Cash flow sustainability** -- operating cash flow consistently below net income suggests accrual quality concern

## Management Report Production

### Report Structure

A standard management report (MIS package) includes:

1. **Executive summary** -- 1 page maximum. Key results, critical variances, recommended actions.
2. **P&L with BvA comparison** -- actual, budget, dollar variance, % variance columns. Current month and YTD.
3. **Balance sheet** -- current vs prior period, with key ratio computations.
4. **Cash flow summary** -- operating/investing/financing with beginning/ending cash reconciliation.
5. **KPI dashboard** -- 8-12 KPIs with traffic-light status (green/yellow/red vs target).
6. **Variance commentary** -- narrative explanations for each material variance.
7. **Industry benchmarking** -- annual or quarterly positioning vs peers (when available).
8. **Appendix** -- supporting schedules, GL detail for investigated items.

### Output Formats

- **PDF** -- for distribution and archival; use landscape orientation for wide reports
- **XLSX** -- for interactive review; apply styles (font, color, number format), cell annotations as comments, auto-adjusted column widths, date/time footer
- **Dashboard widget** -- for real-time monitoring; supports drill-down from any cell to underlying journal entries

### Executive Commentary Guidelines

Write variance commentary in this pattern:

1. **State the variance** -- "Revenue was $X above/below budget by $Y (Z%)"
2. **Explain the driver** -- "Driven by [price/volume/mix/timing factor]"
3. **Quantify the impact** -- "This contributed $W to the overall variance"
4. **Recommend action** -- "Management should consider [specific action]" (or "No action required -- timing variance expected to reverse in Q[N]")

Avoid: vague language ("slightly above"), unexplained jargon, presenting raw numbers
without context, burying bad news in footnotes.

## Data Extraction from Accounting System

For pulling actuals and comparative financial data:

- Pull **P&L (Standard)** and **Balance Sheet (Standard)** for the analysis period
- Use custom date ranges to match benchmark data periods (calendar year for most sources)
- Pull **P&L by Month** for granularity needed in seasonality-adjusted comparisons and trend analysis
- For trend analysis, pull comparative periods (current year vs 2-4 prior years) in a single report run
- Export to spreadsheet for ratio computation if the accounting system does not compute financial ratios natively
- Ensure COA separates COGS from operating expenses to compute gross margin correctly
- For platform-specific report extraction, invoke `qbo-integration:qbo-reporting`

## Supporting References

Read these for deeper detail on specific topics:

- `references/industry-benchmarking.md` -- Complete benchmarking methodology and data source guide. Read "Data Source Hierarchy" for detailed coverage of RMA, BizMiner, IBISWorld, IRS SOI, and public comparables with strengths and limitations of each. Read "Peer Group Selection" for the 5-criteria prioritized selection framework. Read "Financial Ratio Categories" for full definitions and interpretation notes across liquidity, leverage, efficiency, and profitability ratio families. Read "Analysis and Presentation" for quartile ranking methodology, trend vs point-in-time guidance, and narrative context patterns. Read "Limitations and Caveats" before presenting any benchmarking analysis.

- `references/management-reporting-engine.md` -- Management reporting engine architecture for KPI-based financial reports. Read "Data Model" for KPI row definition (type, compare method, accumulation method, expressions, sub-KPIs, auto-expand accounts, style overrides). Read "Expression Engine" for accounting expression syntax (bal/deb/crd/pbal/nbal with period/initial/ending modes, account selectors, move line domains) and Python expression patterns (cross-KPI references, conditionals, subreport access). Read "Report Computation Flow" for the full matrix computation pipeline (compute_matrix, add_column, expression evaluation, recompute queue, comparison/sum column generation). Read "Data Sources" for actuals, alternative move line models, budget-by-KPI with pro-rata temporis, and budget-by-account sources. Read "Output Formats" for PDF/XLSX/dashboard rendering details including style conversion and drill-down mechanics.

## Cross-Plugin References

For upstream data sources:
- Invoke `financial-planning:budgeting-forecasting` for the budget baseline used as comparison target in BvA analysis, budget governance workflow (draft/confirmed/validated), and time-proportional pacing computation
- Invoke `accounting-foundation:financial-statements` for actuals (P&L, balance sheet, cash flow, trial balance), GAAP presentation rules, ratio analysis definitions, common-size analysis methodology, and year-end close workflow
- Invoke `qbo-integration:qbo-reporting` for accounting system report endpoints (P&L, Balance Sheet, Cash Flow, General Ledger), report parameters, comparative reporting, and Excel export

For downstream consumers:
- Invoke `financial-planning:strategic-advisory` for board reporting, M&A due diligence, and strategic decisions that consume variance analysis and benchmarking output
- Invoke `financial-planning:financial-modeling` for 3-statement models that project future performance and reference historical variance patterns
