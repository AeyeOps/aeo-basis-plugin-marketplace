---
name: monthly-close
description: >
  Contains verified five-phase close sequence, ASC 842 lease classification with ROU
  measurement mechanics, ASC 606 five-step revenue recognition model, and COSO
  segregation-of-duties compensating controls that produce more precise answers than
  reasoning from general training alone. Month-end close, adjusting entries, accruals,
  deferrals, depreciation, year-end closing entries, period lock, variance analysis.
  Consult when "how do I close the books for December" or posting period-end adjustments.
---

# Monthly Close

Operational skill for period-end close procedures in C-corporations. The monthly
close converts a month of raw transactions into reliable financial statements through
a strict five-phase sequence. Skipping phases or reordering creates compounding errors.

**Target turnaround**: 5-7 business days for routine clients; 10 business days for
complex (multi-entity, inventory, payroll).

## Close Sequence Overview

The close follows five phases in strict order. Each phase gates the next.

1. **Transaction Completeness** — every economic event for the period is recorded
2. **Reconciliation** — balance sheet accounts tied to independent third-party evidence
3. **Adjusting Entries** — accruals, deferrals, depreciation, lease and revenue entries
4. **Review and Reporting** — trial balance, financial statements, variance analysis
5. **Period Lock** — lock the period, archive documents, deliver client package

## Phase 1: Transaction Completeness

Complete before any reconciliation work begins.

- Clear the bank feed inbox — review and categorize all downloaded transactions; do not leave unmatched items
- Post all invoices issued during the month (AR); review aging for collectibility
- Enter all bills received (AP); match to POs where applicable
- Post payroll — if external provider (ADP, Gusto), post summary journal entry for gross wages, employer taxes, withholdings, and net pay. Never record net payroll only; this understates wages and omits employer tax liabilities
- Categorize all credit card charges; match to receipts
- Record cash transactions — petty cash, owner draws, capital contributions
- Post recurring journal entries — rent straight-lining, insurance amortization, loan interest accruals

> Invoke `bookkeeping:reconciliation` as prerequisite — all accounts must be reconciled before Phase 3.

## Phase 2: Reconciliation

Reconcile every balance sheet account to independent evidence. Do not proceed to adjusting entries until all reconciliations are complete and differences are investigated.

- **Bank accounts** — reconcile each account to the statement ending balance
- **Credit cards** — reconcile each card to the issuer statement
- **Loans** — tie balances to lender statements; verify principal/interest split
- **Intercompany** — if multi-entity, confirm intercompany balances net to zero
- **AR** — tie aging total to the GL control account
- **AP** — tie aging total to the GL control account

### Bank Reconciliation Decision Thresholds

- Difference < $5 — write off to miscellaneous income/expense; document in close memo
- Difference $5–$100 — investigate one round; if unresolved, write off with manager approval
- Difference > $100 — must resolve before closing; escalate if unresolved after 2 business days

Outstanding checks > 90 days require investigation; > 180 days may trigger escheatment review.

## Phase 3: Adjusting Entries

Post entries that align recognition with the accounting period.

### Standard Adjustments

- **Accrued expenses** — expenses incurred but not yet billed (utilities, professional fees, interest). Reverse prior-month accruals first to avoid double-counting when the actual bill arrives.
- **Accrued revenue** — revenue earned but not yet invoiced (milestone completions, time-based services)
- **Prepaid amortization** — expense the current-month portion of prepaid insurance, rent, subscriptions
- **Depreciation and amortization** — post monthly depreciation for fixed assets; amortize intangibles. Invoke `accounting-foundation:chart-of-accounts` for asset class ranges and accumulated depreciation contra accounts.
- **Inventory adjustments** — if applicable, adjust for shrinkage, obsolescence, lower-of-cost-or-market
- **Deferred revenue** — recognize the portion earned during the month per ASC 606
- **Payroll tax true-up** — reconcile accrued payroll taxes to actual deposits; adjust variance

### Missing Document Handling

- Client missing documents at close — accrue based on best estimate, reverse next month. Note in close memo and follow up with client.
- Payroll not posted by Day 3 — accrue based on prior month; reverse when actual posts.
- Prior-period adjustment discovered — post to current period if immaterial; restate prior period if material (materiality = 5% of net income or $5,000, whichever is less).

### Lease Accounting — ASC 842

All leases with terms > 12 months must be recognized on the balance sheet as a right-of-use (ROU) asset and corresponding lease liability. Classification (finance vs. operating) determines the expense pattern, not whether the lease appears on the balance sheet.

**Finance lease** — meets any one of: ownership transfer, purchase option reasonably certain to exercise, term >= ~75% of economic life, PV of payments >= ~90% of fair value, specialized asset with no alternative use. Two P&L line items: amortization expense (straight-line on ROU) and interest expense (effective interest on liability). Front-loaded total expense.

**Operating lease** — does not meet any finance criteria. Single straight-line lease expense. ROU amortization is a plug (total expense minus interest on liability).

**Monthly journal entries:**

- Operating: Dr Lease Expense (straight-line), Cr Cash; adjust ROU Asset and Lease Liability for interest accrual and principal reduction
- Finance: Dr Amortization Expense, Cr ROU Asset; Dr Interest Expense, Dr Lease Liability, Cr Cash

**Short-term exemption**: leases <= 12 months at commencement with no purchase option may be excluded from balance sheet recognition. Election is by asset class. Expense on straight-line basis.

**Lease modifications**: if modification adds distinct right of use at commensurate price, treat as separate lease. Otherwise, remeasure liability at revised discount rate and adjust ROU asset. Reassess classification at modification date.

Maintain an external amortization schedule per lease tracking: opening liability, interest, payment, closing liability, opening ROU, amortization, closing ROU. Journal entries in the accounting system are driven from this schedule.

> For full classification criteria, IBR determination, practical expedients, transition guidance, and disclosure requirements, see `references/lease-accounting-asc842.md`.

### Revenue Recognition — ASC 606

The five-step model governs when and how much revenue to recognize:

1. **Identify the contract** — approval, identifiable rights, payment terms, commercial substance, probable collection. If collection is not probable, cash received is a deposit (liability), not revenue.
2. **Identify performance obligations** — each distinct good or service is a separate PO. A series of substantially similar services (e.g., monthly cleaning) may be a single PO.
3. **Determine transaction price** — fixed + variable consideration (discounts, bonuses, penalties). Apply the constraint: include variable consideration only when a significant revenue reversal is highly improbable.
4. **Allocate to POs** — by relative standalone selling price (SSP). Methods: adjusted market, expected cost plus margin, residual (last resort).
5. **Recognize revenue** — over time or at a point in time per control transfer criteria.

**Over-time recognition** (any one met): customer simultaneously receives and consumes benefits (recurring services); entity creates asset customer controls as created; asset has no alternative use and entity has right to payment for performance to date. Measure progress via input (cost-to-cost) or output (milestones) methods.

**Common C-corp patterns:**
- Product sales — point-in-time at delivery/shipment
- Service contracts — over-time, criterion 1 (simultaneous receipt/consumption)
- SaaS/subscriptions — over-time, ratably over period
- Bundled arrangements — identify each PO, allocate by SSP, recognize per satisfaction pattern

**Monthly close actions**: recognize deferred revenue earned during the month (Dr Deferred Revenue, Cr Revenue); accrue unbilled revenue for over-time POs (Dr Contract Asset, Cr Revenue); reassess variable consideration estimates.

> For variable consideration constraint detail, contract modifications, principal vs. agent, licensing, and effective date log, see `references/revenue-recognition-asc606.md`.

## Phase 4: Review and Reporting

- **Trial balance** — verify debits equal credits; scan for anomalies (negative assets, credit expense accounts, suspense account balances)
- **Financial statements** — generate P&L, Balance Sheet, Cash Flow Statement. Invoke `accounting-foundation:financial-statements` for presentation requirements.
- **Variance analysis** — compare to prior month and budget. Investigate variances exceeding materiality threshold (typically 10% or $1,000, whichever is greater).
- **Analytical review** — compute key ratios (current ratio, gross margin, DSO); flag anomalies
- **Close memo** — document all adjustments, open issues, and items requiring client attention
- **Manager/partner review** — second set of eyes on financial statements and reconciliations before delivery

For platform-specific execution, invoke `qbo-integration:qbo-bookkeeping`.

## Phase 5: Period Lock

- **Lock the period** — set the closing date and password in the accounting system to prevent unauthorized posting to prior periods. Configure separate lock dates for accountant vs. client access if the system supports it.
- **Archive supporting documents** — bank statements, reconciliation reports, adjustment memos
- **Deliver client package** — financial statements, management letter (if applicable), action items

### Year-End Close Extensions

Year-end close adds: close revenue (4xxx) and expense (5xxx–9xxx) to Income Summary, transfer net income to Retained Earnings (3200), close Dividends Declared (3500) to Retained Earnings. Invoke `accounting-foundation:chart-of-accounts` for year-end closing entry mechanics and equity account structure.

## Internal Controls During Close

The close process itself is a key control activity. Apply COSO 2013 principles proportionate to entity size.

### Segregation of Duties

Four functions should be separated: authorization, custody, recording, reconciliation. When staff size prevents full segregation (common in small C-corps), implement compensating controls:

- **Owner review** — owner independently reviews bank statements, canceled checks, and monthly financials. This is the single most important compensating control.
- **External reconciliation** — engage outside accountant for monthly or quarterly bank reconciliations
- **Rotation of duties** — periodically rotate responsibilities to prevent concealment and cross-train staff

### Key Close Controls

- Bank reconciliation performed by someone other than the person recording transactions or handling cash
- Approval thresholds for expenditures (e.g., > $500 requires owner sign-off, > $5,000 requires dual approval)
- Journal entry posting restricted to authorized personnel via accounting system user roles
- Audit log reviewed for unexpected entries — the system should track all user activity in a log that cannot be edited or deleted
- Period-lock password prevents unauthorized posting to prior periods

### Deficiency Handling

- **Material weakness** — reasonable possibility of material misstatement not prevented/detected; requires immediate remediation and disclosure
- **Significant deficiency** — less severe but merits governance attention; address within one to two quarters
- **Deficiency** — track and address as resources allow

> For the full COSO five-component/17-principle framework, IT general controls, control documentation methods, and testing procedures, see `references/internal-controls-coso.md`.

## Platform Execution

For platform-specific execution, invoke `qbo-integration:qbo-bookkeeping`.

## Supporting References

Read these for deeper detail on specific topics:

- `references/monthly-close-sop.md` — Full 34-step close procedure with numbered checklist, pre-close preparation steps, and decision tables for bank reconciliation thresholds and missing-document handling. Read when walking through the close step-by-step or training new staff.
- `references/lease-accounting-asc842.md` — Complete ASC 842 guidance: five finance-lease classification criteria with bright-line thresholds, initial and subsequent measurement mechanics for both lease types, short-term exemption rules, modification accounting, practical expedients (package of three, hindsight, portfolio, non-lease components), disclosure requirements, account setup and journal entry templates, and effective date log through ASU 2024-02. Read when setting up a new lease, handling modifications, or preparing lease disclosures.
- `references/revenue-recognition-asc606.md` — Full five-step model with detailed guidance on each step, over-time vs. point-in-time criteria, variable consideration constraint, contract modifications (three treatments), principal vs. agent determination, licensing (right-to-access vs. right-to-use), common C-corp revenue patterns, recording templates, and effective date log through ASU 2024-13. Read when evaluating a new revenue arrangement or complex contract modification.
- `references/internal-controls-coso.md` — COSO 2013 five-component/17-principle framework, small entity considerations, common detective and preventive controls for C-corps, IT general controls for cloud accounting environments, segregation of duties with compensating controls, control documentation methods (narratives, flowcharts, matrices), testing effectiveness (inquiry, observation, inspection, reperformance), deficiency classification and communication requirements. Read when designing controls, responding to audit findings, or assessing control environment.

## Cross-Plugin References

- Invoke `bookkeeping:reconciliation` — prerequisite: all balance sheet accounts must be reconciled before Phase 3
- Invoke `accounting-foundation:chart-of-accounts` — for depreciation asset class ranges, contra accounts, year-end closing entry mechanics, and period-close account structure
- Invoke `accounting-foundation:financial-statements` — for statement presentation requirements and financial statement generation during Phase 4
- Invoke `qbo-integration:qbo-reporting` — for platform-specific report generation (P&L, Balance Sheet, Trial Balance with period comparison) during Phase 4

## Cross-Plugin Consumers

- `tax-prep:form-1120-prep` — clean closed financials are the starting point for tax return preparation
- `tax-prep:tax-planning` — book-tax differences identified during close feed planning analysis
- `financial-planning:budgeting-forecasting` — closed actuals provide budget-vs-actual variance data
