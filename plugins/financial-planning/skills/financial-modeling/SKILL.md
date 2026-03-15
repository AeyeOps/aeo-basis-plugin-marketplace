---
name: financial-modeling
description: >
  Contains verified covenant ratio formulas (DSCR, leverage, FCCR, TNW with agreement-specific
  definitions), consolidation/segment reporting models (NCI calculations, elimination entries,
  10% threshold test), and three-statement linkage architecture (P&L-to-BS-to-CF circular
  reference handling). Consult when building or auditing an integrated financial model,
  running base/upside/downside scenarios, modeling covenant breach headroom, designing
  sensitivity tables or tornado charts, calculating break-even/CVP/operating leverage,
  setting up Monte Carlo simulations, preparing multi-entity consolidation packages with
  segment reporting, or any "what happens if" projection question for a C-corporation.
---

# Financial Modeling

Operational skill for constructing, auditing, and stress-testing integrated financial
models for C-corporation clients. Models connect P&L, balance sheet, and cash flow
projections through a shared assumption layer, enabling scenario analysis, covenant
compliance forecasting, and consolidation across multi-entity groups.

## Three-Statement Integrated Model

An integrated model links three statements so that a single assumption change cascades
correctly through all outputs. The linkage discipline is what separates a model from
three disconnected spreadsheets.

### Architecture

Build models with four layers, each on its own sheet or section:

- **Assumptions** — all variable inputs in one place (revenue growth rates, COGS %,
  opex line items, capex schedule, tax rate, debt terms, working capital days). Never
  hard-code assumptions inside formulas.
- **Income Statement** — driven by revenue assumptions flowing through margin and
  expense assumptions. Net income is the first linkage point.
- **Balance Sheet** — working capital accounts driven by DSO/DPO/DIO assumptions
  against revenue/COGS; fixed assets driven by capex schedule and depreciation; debt
  driven by borrowing schedule; equity driven by net income less dividends.
- **Cash Flow Statement** — indirect method. Starts with net income from P&L, adds
  back non-cash items, adjusts for working capital changes derived from BS deltas,
  includes investing and financing flows. Ending cash must tie to BS cash.

### Statement Linkages

These connections must hold in every period or the model is broken:

- Net income (P&L) flows to retained earnings (BS) and is the starting point for
  operating cash flow (CF)
- Working capital changes on CF = current-period BS working capital minus prior-period
- Depreciation appears on P&L (expense), CF (add-back), and BS (accumulated depreciation)
- Capex appears on CF (investing outflow) and BS (PP&E increase)
- Debt proceeds/repayments appear on CF (financing) and BS (debt balance change)
- Dividends appear on CF (financing outflow) and BS (retained earnings reduction)
- Ending cash on CF = cash on BS

### Circular Reference Handling

Interest expense depends on debt balance, which depends on cash flow, which depends
on interest expense. Break the circularity with one of:

- **Iteration toggle** — enable iterative calculation; interest uses average of beginning
  and ending debt balance
- **Prior-period plug** — interest calculated on prior-period ending debt (simpler,
  slightly less accurate, avoids iteration)
- **Cash sweep** — model the revolver draw/paydown as a balancing item after operating
  and investing flows; interest on the revolver uses the prior-period balance

## Scenario Analysis

### Framework

Define three named scenarios with distinct assumption sets:

- **Base case** — management's best estimate; anchored to historical trends and confirmed
  pipeline. Use trailing 3-year average growth rates adjusted for known changes.
- **Upside case** — reasonable optimism. Revenue +10-20% above base; faster collections
  (lower DSO); margin expansion from operating leverage. Document the thesis (new contract,
  market expansion, price increase).
- **Downside case** — plausible stress. Revenue -15-25% below base; margin compression
  from fixed cost deleveraging; working capital deterioration (longer DSO, shorter DPO).
  Test whether the entity can still service debt and meet covenants.

### Implementation

Use a scenario switch cell (1/2/3 or dropdown) that drives INDEX/CHOOSE functions
across all assumption inputs. Every assumption that changes by scenario must reference
the switch — never create three parallel models.

Structure the assumption block as a table:

```
Assumption          | Base    | Upside  | Downside | Active (=CHOOSE)
Revenue growth      | 5.0%    | 12.0%   | -10.0%   | [formula]
Gross margin        | 42.0%   | 44.0%   | 38.0%    | [formula]
DSO (days)          | 45      | 38      | 55       | [formula]
CapEx (% of rev)    | 4.0%    | 5.0%    | 2.5%     | [formula]
```

### Probability-Weighted Valuation

When scenarios serve a valuation or decision: assign probability weights that sum to
100%. Typical starting weights: Base 50-60%, Upside 15-25%, Downside 15-25%. Expected
value = sum of (scenario value x probability). Document the rationale for weights —
they are subjective but must be defensible.

## Sensitivity Analysis and Data Tables

### One-Variable Sensitivity

Vary a single assumption across a range while holding all others at base case. Choose
assumptions with the highest model impact — typically revenue growth, gross margin,
interest rate, or tax rate.

Present as a row or column of output values (net income, DSCR, free cash flow) indexed
against the input range. Highlight the base case value and any threshold crossings
(covenant breach, negative cash, loss).

### Two-Variable Data Table

Vary two assumptions simultaneously in a matrix. Row input = first variable range,
column input = second variable range, cell formula = the output metric. Color-code
cells: green above target, yellow in warning zone, red below threshold.

Common pairings for C-corp models:

- Revenue growth vs. gross margin (impact on operating income)
- Interest rate vs. leverage ratio (impact on DSCR)
- Revenue growth vs. capex intensity (impact on free cash flow)
- Price change vs. volume change (impact on break-even)

### Tornado Chart

Rank assumptions by impact magnitude. For each key assumption, calculate the output
swing between its low and high values while holding all others at base. Present as
horizontal bars sorted by absolute impact — the widest bar is the most sensitive driver.

## Break-Even and CVP Analysis

### Break-Even Point

- **Units:** Fixed Costs / (Price per Unit - Variable Cost per Unit)
- **Revenue:** Fixed Costs / Contribution Margin Ratio
- **Contribution margin ratio:** (Price - Variable Cost) / Price

For service firms (common in C-corp client base): fixed costs include rent, salaries,
insurance, technology; variable costs include subcontractor fees, direct materials,
commissions. Break-even revenue = fixed costs / (1 - variable cost ratio).

### Margin of Safety

- **Dollar:** Actual Revenue - Break-Even Revenue
- **Percentage:** (Actual Revenue - Break-Even Revenue) / Actual Revenue
- Target margin of safety depends on revenue volatility — 20%+ for cyclical businesses,
  10-15% for stable recurring revenue

### Multi-Product CVP

When the entity sells multiple products or service lines, compute the weighted-average
contribution margin based on the sales mix, then apply the standard break-even formula.
Sensitivity-test the mix — if the high-margin product share drops, break-even revenue
rises.

### Operating Leverage

Degree of operating leverage (DOL) = Contribution Margin / Operating Income. High DOL
means small revenue changes produce large profit swings — important context for scenario
analysis and covenant headroom assessment.

## Monte Carlo Simulation Design

For models where discrete scenarios are insufficient — typically when multiple uncertain
inputs interact in non-linear ways.

### Design Steps

1. **Identify uncertain inputs** — select 3-8 key assumptions with meaningful variance
   (revenue growth, customer churn, input costs, interest rates, FX rates)
2. **Assign probability distributions** — normal for symmetric outcomes, lognormal for
   always-positive variables (revenue), triangular when only min/mode/max are known,
   uniform when no shape information exists
3. **Define correlations** — revenue and COGS often move together (positive correlation);
   revenue and default rates may move inversely. Ignoring correlations can produce
   implausible combined outcomes.
4. **Run iterations** — 1,000-10,000 iterations; each draws a random value from each
   input distribution and flows it through the model to produce output metrics
5. **Analyze output distributions** — report mean, median, 10th/25th/75th/90th
   percentiles, and probability of breaching key thresholds (covenant breach, negative
   cash, loss)

### Output Presentation

- Histogram of the key output metric (e.g., DSCR distribution across iterations)
- Cumulative probability curve showing likelihood of achieving at least a target value
- Probability of covenant breach = count of iterations below threshold / total iterations
- Conditional tail analysis: "If DSCR falls below 1.20x (covenant), the median DSCR
  in that scenario is X" — helps size the equity cure

## Debt Covenant Modeling

Integrate covenant compliance testing into the model so that each scenario automatically
flags covenant risk. The procedures below are distilled from the debt covenant analysis
reference.

### Covenant Ratio Formulas

Build these as named calculated rows in the model, using credit-agreement definitions
(not textbook GAAP):

- **DSCR** = Net Operating Income / Total Debt Service. Typical minimum 1.20x-1.50x.
  NOI definition varies by agreement — may be EBITDA, may exclude specific items. Total
  debt service = principal + interest due in the measurement period.
- **Leverage (Debt/EBITDA)** = Total Funded Debt / TTM EBITDA. Typical maximum
  3.0x-5.0x. Net leverage variant subtracts unrestricted cash from debt.
- **Current Ratio** = Current Assets / Current Liabilities. Typical minimum 1.0x-1.5x.
  Watch: classification of current portion of LTD, revolver treatment.
- **Fixed Charge Coverage** = (EBITDA - Unfinanced CapEx - Cash Taxes - Distributions)
  / (Interest + Scheduled Principal + Lease Payments). Typical minimum 1.10x-1.25x.
  Broader than DSCR — captures leases, taxes, and distributions.
- **Tangible Net Worth** = Total Equity - Intangible Assets. Often a ratchet floor:
  prior-period TNW + percentage of net income, never decreasing.
- **Interest Coverage** = EBIT / Interest Expense. Typical minimum 2.0x-3.0x.

### Headroom and Early Warning

For each ratio in each projected period, calculate covenant headroom:

- **Green (>20% cushion):** ratio exceeds threshold by more than 20% of the threshold value
- **Yellow (10-20% cushion):** proactive client discussion; explore remediation levers
- **Red (<10% cushion or breach):** immediate notification; model equity cure or amendment

Trend the headroom across projection periods — declining headroom even with green status
signals future risk.

### Breach Remediation Modeling

When a scenario shows a projected breach, model the cure options:

- **Equity cure** — calculate the equity injection needed to restore the ratio above
  the threshold. Check whether the agreement permits equity cures (typically limited
  to 2-3 over the loan term).
- **Covenant amendment** — model revised thresholds; calculate amendment fee (0.10-0.50%
  of commitment).
- **Operational lever** — model the impact of cost cuts, capex deferral, or accelerated
  collections on the breached ratio.
- **Forbearance cost** — model default interest (+2-5% above contract rate) for the
  projected breach period.

### Cross-Default Risk

If the client has multiple credit facilities, a breach in one can trigger cross-default
in all. The model should track all facilities simultaneously and flag cross-default
exposure.

## Consolidation and Segment Reporting Models

For multi-entity C-corp groups that require consolidated financial statements and segment
disclosures. The procedures below are distilled from the consolidation-segments reference.

### Consolidation Model Structure

1. **Entity-level trial balances** — import each subsidiary's TB for the reporting period.
   Map each entity's native accounts to the consolidated chart of accounts.
2. **Combined trial balance** — aggregate all entity TBs into a single combined TB.
   Each row retains its entity identifier for traceability.
3. **Elimination entries** — remove intercompany transactions so the consolidated view
   reflects only external activity:
   - Intercompany revenue/expense elimination
   - Intercompany receivable/payable elimination
   - Intercompany investment/equity elimination
   - Intercompany profit in inventory (if applicable)
4. **Consolidated statements** — combined TB + eliminations = consolidated BS, P&L, CF

### Non-Controlling Interest (NCI)

For less-than-100%-owned subsidiaries, allocate the minority share:

- **NCI % of net income** = subsidiary net income x (1 - parent ownership %)
- **NCI equity** = subsidiary equity x (1 - parent ownership %)

NCI appears as a separate line in consolidated equity and as an allocation line on the
consolidated income statement. Recalculate whenever ownership percentages change.

### Segment Reporting

Operating segments are defined by internal management reporting structure. A segment is
reportable if it meets the 10% threshold on any of: revenue, absolute profit/loss, or
assets.

For each segment, report: revenue, operating expenses, operating income, depreciation,
capital expenditures, and total assets. Segment totals must reconcile to consolidated
totals through a reconciliation column (corporate/unallocated items, intercompany
eliminations).

### Consolidation Package Export

The standard deliverable is a workbook containing:
- **Combined TB** tab — entity, account number, account name, debit, credit, mapped account
- **Eliminations** tab — type, description, debit account/amount, credit account/amount
- **Segment Report** tab — segment, revenue, operating expenses, operating income, total assets

Best practices: standardize COA mapping across entities, eliminate consistently each
period, reconcile eliminations to zero net impact, track intercompany balances by segment,
version-control the workbook, and document NCI ownership percentages.

## Model Audit and Error Checking

### Structural Checks

Run these before any analysis:

- **Balance sheet balances** — Assets = Liabilities + Equity in every period. Any
  imbalance means a linkage is broken.
- **Cash flow reconciliation** — ending cash on CF = cash on BS in every period
- **No hard-coded values in formulas** — all inputs trace to the assumption layer
- **Consistent formula structure** — each row uses the same formula across all periods
  (no manual overrides in individual cells)
- **Sign conventions** — verify that revenues are positive, expenses are positive (or
  consistently negative), and the CF indirect method add-backs have correct signs

### Reasonableness Checks

- Revenue growth rate within plausible range for the industry
- Margins do not exceed theoretical maximums or go negative without explanation
- Working capital days are consistent with industry norms
- Capex as % of revenue is sustainable
- Tax rate approximates effective rate (21% federal + state for C-corps)
- Debt balance never goes negative (would imply overpayment)

### Sensitivity Audit

- Toggle each scenario and verify the model recalculates without errors
- Check that extreme assumptions (zero revenue, maximum leverage) do not produce
  circular reference errors or division-by-zero
- Verify covenant ratios correctly flag breach in stress scenarios

## Supporting References

Read these for detailed procedures and implementation patterns:

- `references/debt-covenant-analysis.md` — Comprehensive debt covenant procedures
  including covenant type taxonomy (affirmative, negative, financial), detailed ratio
  formulas with agreement-specific variations, quarterly compliance monitoring cycle,
  early warning thresholds, breach consequences (cross-default, default interest,
  revolving line restrictions), and cure/amendment/forbearance mechanics.
  Read when building covenant compliance models, preparing compliance certificates,
  or advising on breach remediation.

- `references/consolidation-segments.md` — Segment reporting and NCI implementation
  including segment definition data model (entity-to-segment assignment, 10% revenue
  threshold), segment report generation logic (aggregation of revenue, operating income,
  assets by segment), NCI calculation and journal entry patterns (income allocation and
  equity recording), and consolidation package export structure (combined TB, elimination
  schedule, segment report). Read when building multi-entity consolidation models or
  designing segment reporting deliverables.

## Cross-Plugin References

For model inputs and historical data:
- Invoke `accounting-foundation:financial-statements` for historical P&L, BS, CF, trial
  balance data, ratio analysis formulas, and statement interrelationship rules that the
  model must preserve in projections
- Invoke `financial-planning:budgeting-forecasting` for FP&A modeling methodology,
  rolling forecast patterns, and cash flow projection frameworks that feed model assumptions
- Invoke `financial-planning:variance-analysis` for historical variance patterns, trend
  analysis, and KPI benchmarks used to calibrate model assumptions

For platform data extraction:
- Invoke `qbo-integration:qbo-reporting` for QBO financial report extraction (P&L,
  Balance Sheet, Cash Flow, GL) that provides the historical data inputs for modeling
  and the consolidation source data for multi-entity groups

## Cross-Plugin Consumers

- `financial-planning:strategic-advisory` — consumes scenario outputs for M&A due
  diligence, debt capacity analysis, and capital allocation recommendations
- `financial-planning:tax-provision` — uses projected pre-tax income from model scenarios
  for ASC 740 provision estimates
