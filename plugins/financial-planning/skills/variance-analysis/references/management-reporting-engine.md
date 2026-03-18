---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Management Reporting Engine — KPI-Based Financial Reporting

Architecture for a KPI computation and management reporting engine. KPIs in rows, time periods in columns. Produces P&L, Balance Sheet, and budget comparison reports with PDF, XLSX, and dashboard output.

## Architecture Overview

Three functional layers:
- **Core engine** — report templates, instances, styles, expressions, accounting expression processor (AEP), PDF/XLSX export
- **Budget integration** — budget data sources (by KPI and by account), pro-rata temporis
- **Demo/reference data** — sample reports and templates

Dependencies: general ledger, reporting framework, date range management

## Data Model

### Report Template
- Time-independent definition of KPI rows
- Fields: name, description, default style, move line source model, account model (derived from move line source)
- Children: KPI definitions, sub-KPI definitions, non-accounting queries, sub-report references
- `all_kpi_ids` — computed union of own KPIs plus sub-report KPIs

### KPI Row
- Fields: name (valid Python identifier), description, sequence, parent report
- **Type** — `num` (numeric), `pct` (percentage), `str` (string)
- **Compare method** — `diff`, `pct`, `none`
- **Accumulation method** — `sum` (pro-rata add), `avg` (weighted average), `none`
- **Multi-value** — if True, KPI has multiple sub-values via sub-KPIs
- **Expression** — computed/inverse field wrapping expression definitions
- **Auto-expand accounts** — show detail rows per account
- **Style** — style override; style expression is evaluated dynamically (returns style name or None)
- Separate style for account detail rows

### KPI Expression
- Links a KPI to a formula string, optionally scoped to a sub-KPI
- Fields: expression string, parent KPI, sub-KPI scope
- Unique constraint: (sub-KPI, KPI)

### Sub-KPI
- Enables multi-column KPIs (e.g., Initial/Debit/Credit/Ending for trial balance)
- Fields: name, description, sequence, parent report
- When KPI multi-value is True, each expression maps to one sub-KPI

### Non-Accounting Query
- Fetches data from any data model with a date field
- Fields: name (Python identifier), source model, fields, aggregate method (sum/avg/min/max/None), date field, filter domain, company field
- If aggregate is set, returns a single record; otherwise returns a list
- Filter domain is expanded with date range filters at runtime
- Result available in KPI expressions as `query_name.field_name` or `sum([r.field for r in query_name])`

### Sub-Report
- Reference another report template by name
- Fields: name (Python identifier), parent report, referenced report
- Evaluated first; results injected into evaluation context as `subreport_name.kpi_name`
- Loop detection via recursive constraint

### Report Instance
- Binds a template to periods and configuration
- Fields: name, report template, period definitions, transaction filter (posted/all), company, multi-company flag, company list, currency
- **Comparison mode** — True when using multiple periods (columns); False for simple single-period mode
- **Pivot date** — base date for relative period calculation
- **Suppress account detail** — instance-level override to hide account expansion
- **Analytic filter** — JSON domain applied on all move line queries (report-level filter)
- PDF orientation, column description display, widget configuration
- **Effective companies** — intersection of report companies and user context companies

### Report Period (Column)
- Fields: name (label), sequence, parent report instance
- **Data source type**:
  - `actuals` — standard journal entry lines via AEP
  - `actuals_alt` — alternative move-line-like model (user picks source model)
  - `sumcol` — sum/diff of other columns
  - `cmpcol` — comparison of two columns
  - `budget_by_kpi` — budget data by KPI (with pro-rata temporis)
  - `budget_by_account` — budget data by account (with pro-rata temporis)
- **Date mode** — `fix` (manual dates), `relative` (offset from pivot), `none` (for sum/compare cols)
- **Period granularity** for relative mode: day, week, month, year, date_range
- **Offset** — integer offset from current period (e.g., -1 = previous)
- **Duration** — number of periods to span
- **YTD flag** — forces date_from to Jan 1 of the relevant year
- **Sub-KPI filter** — optional filter to show only specific sub-KPIs
- **Normalize factor** — for comparison normalization
- **Analytic filter** — column-level additional domain (ANDed with report-level)

### Cell Annotation
- Cell-level notes (text annotations on report cells)
- Fields: period, KPI, sub-KPI, note text, annotation context (JSON)
- Context includes effective company list for multi-company scoping
- Access controlled by read and edit permission groups

### Report Style
- Cascading style model with inheritance flags
- Properties: color, background_color, font_style (normal/italic), font_weight (normal/bold), font_size, indent_level, prefix, suffix, decimal places, divider (1e-6 to 1e6), hide_empty, hide_always
- Each property has an inherit boolean; when True, value is inherited from parent style
- `merge(styles)` — merges list of styles, last wins for non-inherited props
- `render(lang, style_props, var_type, value)` — formats value to display string
- `compare_and_render()` — computes delta (diff or pct) between two values, renders result
- Style export to CSS and XLSX format dictionaries

## Expression Engine

### Accounting Expressions (AEP)
Regex pattern: `{field}{mode}(.fieldname)?[account_selector][move_line_domain]`

**Fields:**
- `bal` — balance (debit - credit)
- `deb` — sum of debits
- `crd` — sum of credits
- `pbal` — positive balances only (accounts where debit >= credit)
- `nbal` — negative balances only (accounts where debit < credit)
- `fld` — custom numeric field (requires `.fieldname` suffix, mode `p` only)

**Modes:**
- `p` (or omitted) — variation over the period
- `i` — initial balance (at start of period)
- `e` — ending balance (at end of period)
- `u` — unallocated P&L of previous fiscal years

**Account selectors** (inside first `[]`):
- Comma-separated codes: `[70,60]`
- Wildcard: `[1%]`, `[40%]`
- Empty `[]` — all accounts
- Domain on account model: `[("account_type", "=", "asset_receivable")]`

**Move line domain** (optional second `[]`):
- Standard domain filter on journal entry lines
- Example: `[('journal_id.code', '=', 'BNK1')]`

**Expression examples:**
- `balp[70]` — revenue variation for account 70
- `bali[70,60]` — initial balance of accounts 70 and 60
- `bale[1%]` — ending balance of all accounts starting with 1
- `crdp[40%]` — credits on accounts 40x during period
- `debp[55%][('journal_id.code', '=', 'BNK1')]` — debits on 55x in journal BNK1
- `balu[]` — unallocated P&L of prior fiscal years
- `fldp.quantity[60%]` — sum of quantity field on 60x move lines

### Python Expressions
- Any valid Python expression in KPI formulas
- Can reference other KPI names: `revenue - cost_of_goods`
- Can reference query results: `sales_query.amount_untaxed`
- Conditional: `revenue if revenue > 0 else AccountingNone`
- Sub-report KPIs: `balance_sheet.current_assets / balance_sheet.total_assets`
- Sub-KPI access: `kpi_name.subkpi_name`

### Evaluation Context
- `sum`, `min`, `max`, `len`, `avg` — aggregate functions (return None on empty lists, not 0)
- `time`, `datetime`, `dateutil` — Python stdlib modules
- `AccountingNone` — null value singleton
- `SimpleArray` — named tuple-like class for multi-valued KPIs
- `date_from`, `date_to` — period boundaries as date objects
- All other KPI values computed so far (dependency resolution via recompute queue)
- Query results by query name

### AccountingNone
- Singleton that dissolves in arithmetic: `1 + AccountingNone == 1`, `AccountingNone + None == AccountingNone`
- Behaves as 0 in comparisons: `AccountingNone == 0` is True
- `bool(AccountingNone)` is False
- Division by AccountingNone raises ZeroDivisionError
- `AccountingNone * 2 == 0.0`, but `AccountingNone * AccountingNone == AccountingNone`
- Supports `int()`, `float()`, `round()`

### Safe Evaluation
- Compiles expressions with restricted opcodes, then evaluates with restricted builtins
- Returns value or error subclass:
  - `NameError("#NAME", traceback)` — unresolved variable (triggers recompute)
  - `DataError("#DIV/0", traceback)` — division by zero
  - `DataError("#ERR", traceback)` — any other exception

### SimpleArray
- Immutable tuple subclass supporting element-wise arithmetic (+, -, *, /)
- Division by zero in elements produces `DataError("#DIV/0")` per element (not exception)
- Named variant creates subclass with named properties (like namedtuple for SimpleArray)
- Used to represent multi-valued KPIs (one value per sub-KPI)

## Report Computation Flow

### compute_matrix()
1. Prepare AEP for all companies and currency — parse all KPI expressions, resolve account codes to IDs
2. Prepare KPI matrix — create matrix structure, declare KPI rows
3. For each period: add column — dispatches by data source type
4. Compute comparison columns — evaluate comparison columns
5. Compute sum columns — evaluate sum columns
6. Returns populated KPI matrix

### Add Column (actuals/actuals_alt)
1. Creates expression evaluator with AEP, date range, move line filter, and source model
2. Calls report declaration and computation for the period

### Declare and Compute Period
1. Evaluate sub-reports first (recursive evaluation)
2. Populate evaluation context with builtins, date_from/date_to, query results
3. Execute accounting SQL queries via AEP
4. Run main KPI evaluation loop

### Main KPI Evaluation Loop
1. For each KPI in sequence:
   - Get expressions (filtered by active sub-KPIs)
   - Evaluate expressions — AEP replaces accounting vars, then safe evaluation
   - If name error: add to recompute queue (handles forward references)
   - Otherwise: store result in evaluation context for downstream KPIs
   - Set values in matrix with drilldown arguments
   - If auto-expand accounts: evaluate expressions by account for detail rows
2. Recompute queue runs again if progress was made; stops on circular deps or true errors

### Expression Evaluator
- Wraps AEP and date range
- `eval_expressions()`: for each expression, AEP replaces accounting vars with literal values, then safe evaluation evaluates the resulting Python expression
- `eval_expressions_by_account()`: yields (account_id, values, drilldown_args, name_error) for each account

### AEP (Accounting Expression Processor)
- **Parse phase**: extracts accounting variable patterns, groups by (move line domain, mode)
- **Done parsing**: resolves account domains to account IDs (searches per company)
- **Query phase**: executes grouped queries on move lines by (account_id, company_id), aggregating debit/credit sums plus custom fields
  - Results stored indexed by (move line domain, mode) and account_id
  - Smart ending balance: `e` mode computed as `i + p` to avoid extra query
  - Currency conversion: rates fetched per company, applied to debit/credit sums
- **Replace phase**: substitutes accounting vars with literal value strings

**Date domain logic by mode:**
- `p` (variation): `date >= date_from AND date <= date_to`
- `i` (initial): `(date >= fiscal_year_start OR account.include_initial_balance) AND date < date_from`
- `e` (ending): `(date >= fiscal_year_start OR account.include_initial_balance) AND date <= date_to`
- `u` (unallocated): `date < fiscal_year_start AND NOT account.include_initial_balance`

**Convenience methods:**
- `get_balances_initial(companies, date)` — returns `{account_id: (debit, credit)}`
- `get_balances_end(companies, date)`
- `get_balances_variation(companies, date_from, date_to)`
- `get_unallocated_pl(companies, date)` — returns `(total_debit, total_credit)` tuple

### KPI Matrix
- Internal data structure for computed report
- Row per KPI, plus one per KPI/account_id detail
- Column per period, contains sub-column entries
- Cell at intersection of row and sub-column holds value, rendered value, style properties, drilldown argument
- `as_dict()` — serializes to JSON-like dict with header (2 rows: cols, subcols) and body (rows with cells)
- Rows with `hide_empty` style and all-None values are omitted; `hide_always` rows always omitted

### Comparison and Sum Columns
- Comparison columns: for each row, compute delta values using style compare_and_render
- Sum columns: accumulate values from source columns with +/- signs; only for KPIs with `accumulation_method == sum`; respects account detail flag

## Data Sources

### Actuals
- Default: queries journal entry lines via AEP
- Move line source on report template defines the model (can be overridden)
- Filter: posted entries only, or posted + draft based on transaction filter
- Additional filters: report-level analytic domain, column-level analytic domain, context-level analytic domain

### Actuals Alternative
- User selects a custom model as move line source
- Model must have: debit, credit, account_id, date, company_id fields
- Same AEP processing, different underlying table (e.g., committed purchases view)

### Budget by KPI
- Budget linked to a report template, contains budget items
- Budget items: KPI expression reference, date_from, date_to, amount
- Pro-rata temporis adjustment:
  - `accumulation_method == sum`: `amount * overlap_days / item_days`
  - `accumulation_method == avg`: weighted average by overlap days
- Budget-aware expression evaluator overrides standard evaluation: returns budget data for budgetable KPIs, falls back to standard eval for non-budgetable ones

### Budget by Account
- Budget contains items with: account_id, debit, credit, balance, date_from, date_to, company_id
- Overrides grouped read to apply pro-rata temporis on sum aggregates
- Works as an alternative move line source (same AEP flow, but budget table instead of real move lines)

### Non-Accounting Queries
- Arbitrary data model, filtered by date field and optional domain
- Results injected into evaluation context by query name
- Without aggregate: list of record objects
- With aggregate: single record with aggregated values

## Output Formats

### PDF
- Landscape orientation flag on instance controls layout
- Renders via compute_matrix -> as_dict()

### Excel (XLSX)
- Computes matrix, writes column headers (with merge for colspan), sub-column headers
- For each row: applies XLSX style (font, color, number format), writes values with divider applied
- Cell annotations written as Excel comments
- Auto-adjusts column widths, adds date/time footer
- Number format: `#,##0` with decimal places, prefix/suffix in quotes
- Percentage format: `0.0%`

### Dashboard Widget
- JavaScript component calls compute method on report instance
- Returns JSON structure (header + body)
- Supports drill-down (click cell to view underlying journal entries)
- Filter bar with search view, pivot date control
- Cell annotations: dialog for add/edit/remove notes

## Multi-Company

- Multi-company flag enables multi-company mode on the report instance
- Company list defines allowed companies; effective companies = intersection with user's allowed companies
- AEP constructor takes companies; queries grouped by (account_id, company_id)
- Currency conversion: if companies have different currencies, currency on instance is required
  - Rates fetched per company at period end date
  - Debit/credit multiplied by rate before accumulation
- Account detail rows: append company name in multi-company mode
- Branch companies supported: multiple groups with same account_id due to branches; accumulated additively

## Key Extension Points

- Additional move line filter on report period — override to add analytic/organizational unit filters
- Additional query filter on report period — add domain to non-accounting queries
- Filter domain on report instance — combines transaction filter + analytic domains
- Filter descriptions on report instance — override to add subtitle text on PDF/XLSX exports
- Expression evaluator — subclass to provide custom data sources (see budget-aware evaluator)
- Move line source on report template — change the default model for actuals columns

## Programmatic Evaluation

Simplified evaluation without creating an instance:

```
report = get_report_template(report_id)
aep = report.prepare_aep(companies, currency)
result = report.evaluate(
    aep,
    date_from="2024-01-01",
    date_to="2024-12-31",
    target_move="posted",
)
# result is a dict: {kpi_name: value, ...}
```

## Accumulation Methods (Budget Pro-Rata)

- **Sum**: budget item spanning longer period is adjusted proportionally to overlap with reporting period. `adjusted = amount * overlap_days / item_total_days`
- **Average**: weighted average across overlapping items, weighted by overlap days
- **None**: no accumulation; used for string-type KPIs

## Drilldown

- Click on a cell in the widget triggers drilldown on the instance
- Argument contains period_id, expression (accounting expression), optional account_id, KPI_id
- AEP constructs a domain on move lines matching the expression, account, and date range
- Returns an action opening list/form/pivot/graph views of underlying journal entries
- Budget drilldown shows budget item records instead

## Temporary Report Cleanup

- Temporary report instances (created via wizard) have a temporary flag
- Scheduled job deletes temporary instances older than 24 hours
- Can be saved permanently via save action
