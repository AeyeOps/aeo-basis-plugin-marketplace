---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Budget Data Model — Budget Management System

Budget management system providing budgetary positions, budget headers with budget lines, and time-proportional theoretical amount computation. Connects budgets to GL accounts via budgetary positions and to analytic accounting via analytic accounts. Tracks planned vs. actual (practical) amounts with achievement percentage.

## Data Model

### Budgetary Position
- **Name** — required descriptive label
- **Account links** — many-to-many relationship to GL accounts (filters deprecated accounts, scoped to company)
- **Budget lines** — one-to-many to budget lines (inverse: general_budget_id)
- **Company** — required, defaults to current company
- Constraint: must have at least one account (validation error on create/write if accounts list is empty)

### Budget Header
- Supports activity tracking (chatter/audit trail)
- **Name** — required ("Budget Name")
- **Responsible** — creating user, defaults to current user
- **Date range** — `date_from` / `date_to`, required (budget period)
- **State** — `draft` | `cancel` | `confirm` | `validate` | `done`; default `draft`, tracked
- **Budget lines** — one-to-many to budget lines, copied on duplicate
- **Company** — required

### Budget Line
- **Budget header** — required, cascade delete
- **Analytic account** — optional in model, required in UI when analytic accounting is enabled
- **Budgetary position** — required ("Budgetary Position")
- **Date range** — `date_from` / `date_to`, required (line-level period, can differ from header)
- **Paid date** — optional; affects theoretical amount logic
- **Planned amount** — required (the budgeted amount; negative for expenses)
- **Practical amount** — computed (actual spend from analytic lines)
- **Theoretical amount** — computed (time-proportional expected spend)
- **Percentage** — computed, label "Achievement"
- **Company** — derived from budget header, stored

### Analytic Account (extended)
- Adds budget lines relationship — one-to-many to budget lines
- Enables viewing/editing budget lines directly from analytic account form

## Core Computation Logic

### Practical Amount Computation
- Depends on: budgetary position's GL accounts, date_from, date_to, analytic_account
- Queries analytic lines directly:
  ```
  SELECT SUM(amount) FROM analytic_lines
  WHERE account = <analytic_account>
    AND date BETWEEN <date_from> AND <date_to>
    AND general_account = ANY(<accounts from budgetary position>)
  ```
- Returns 0.0 if analytic account or dates are missing
- Note: reads from analytic lines, NOT journal entry lines — requires analytic lines to exist (posted journal entries with analytic distribution)

### Theoretical Amount Computation
- Depends on: paid_date, date_from, date_to, planned_amount
- Uses current datetime as "today" reference (datetime precision for sub-day proportionality)
- Logic branches:
  1. **`paid_date` is set**: If `date_to <= paid_date` then `0.0`, else full `planned_amount`
  2. **No `paid_date`, dates exist**:
     - If today < `date_from` (not started): `0.0`
     - If `date_from` <= today < `date_to` (in progress): `(elapsed_seconds / total_seconds) * planned_amount`
     - If today >= `date_to` (completed): full `planned_amount`
  3. **Missing dates**: `0.0`
- Time-proportional: uses `total_seconds()` for sub-day granularity (e.g., at noon = half-day prorated)
- Verified by test: 364-day budget with -364 planned yields -1.0/day, -1.5 at 36hrs, -308.5 at day 308 noon

### Achievement Percentage
- Depends on: theoretical_amount, practical_amount
- Formula: `(practical_amount / theoretical_amount) * 100` if `theoretical_amount != 0`, else `0.0`
- Represents how much of the time-prorated budget has been realized

## State Machine

```
draft --> confirm --> validate --> done
  ^         |           |
  |         v           v
  +------ cancel <------+
```

- **Confirm** — draft -> confirm
- **Validate** — confirm -> validate (approval)
- **Done** — validate -> done
- **Cancel** — confirm/validate -> cancel
- **Reset to Draft** — cancel -> draft
- All transitions are simple state writes with no guards beyond access rules
- Budget lines are only editable in UI when `state == 'draft'`

## API Surface

### Budgetary Position
- Create — validates accounts not empty before persisting
- Write — validates accounts not empty before persisting
- Internal validation raises error if resulting accounts list is empty

### Budget Header
- State transition methods: confirm, draft, validate, cancel, done
- Standard CRUD; no custom create/write overrides

### Budget Lines
- Practical amount — raw SQL sum from analytic lines
- Theoretical amount — time-proportional budget proration
- Achievement percentage — ratio computation
- No custom CRUD overrides

## Security & Access Control

### Access Rights
- **Account Manager** — read-only on budget headers and budgetary positions
- **Accountant** — full CRUD on all three models
- **Internal User** — CRUD except delete on budget lines

### Record Rules
- Multi-company rules (global) on all three models: company matches user's allowed companies or is unset

### UI Visibility
- `paid_date` field: only visible in debug/superuser mode
- `analytic_account`: only visible when analytic accounting feature is enabled; required in form/list views
- `company`: only visible in multi-company mode

## Integration Points

### Core Accounting
- **GL Accounts**: Budgetary positions link to GL accounts via many-to-many relationship. Domain filters: non-deprecated accounts scoped to company.
- **Analytic Lines**: Practical amount computed by summing amounts from analytic lines matching the budget line's analytic account, date range, and GL accounts. This is the ONLY data source for actuals.
- **Analytic Accounts**: Budget lines can be viewed/edited from the analytic account form via inherited view. Each analytic account shows its associated budget lines.
- **Activity Tracking**: Budget header supports chatter; state changes are tracked.

### What is NOT Integrated
- No direct link to journal entries — all actuals flow through analytic lines
- No budget warnings/blocks on invoice validation or payment
- No report models in this module (reporting handled by separate reporting engine)
- No fiscal year or period model dependency (uses raw date ranges)

## UI Structure

### Menu Locations
- **Budgets list**: Accounting > Journal Entries > Budgets
- **Budgetary Positions**: Accounting > Configuration > Budgetary Positions
- **Budget Lines**: Accounting > Reporting > Budgets
- **Settings**: Accounting Settings > Analytics section > "Budgets" card with link to Budgetary Positions

### Views
- Budget header: list (color-coded by state), kanban (mobile-optimized), form (with statusbar workflow buttons, chatter)
- Budget lines: standalone list/form, and embedded in budget form and analytic account form
- Budgetary position: list, form (with embedded accounts list showing code+name)
- Budget line form context passes default date_from/date_to from parent budget

## Key Implementation Notes
- Planned amounts are **negative for expenses** by convention (matching analytic line sign conventions)
- Practical amount uses raw SQL, not ORM — no access rule filtering on the analytic lines query
- Theoretical amount uses datetime precision (not date-only), enabling sub-day precision in proration
- Budget module is mutually exclusive with enterprise budget modules — cannot coexist
- Depends only on the core accounting module (which transitively provides analytic)
