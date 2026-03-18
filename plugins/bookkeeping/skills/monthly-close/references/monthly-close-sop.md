---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Monthly Close SOP

Standard operating procedure for month-end close of bookkeeping clients.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `bookkeeping-monthly-close`, `bookkeeping-review`, `client-deliverables`

## Contents

- [Overview](#overview)
- [Pre-Close Preparation](#pre-close-preparation)
- [Phase 1: Transaction Completeness](#phase-1-transaction-completeness)
- [Phase 2: Reconciliation](#phase-2-reconciliation)
- [Phase 3: Adjusting Entries](#phase-3-adjusting-entries)
- [Phase 4: Review and Reporting](#phase-4-review-and-reporting)
- [Phase 5: Period Lock](#phase-5-period-lock)
- [Common Pitfalls](#common-pitfalls)
- [Platform Execution](#platform-execution)
- [Decision Points](#decision-points)
- [References](#references)

---

## Overview

The monthly close converts a month of raw transactions into reliable financial statements. It follows a strict sequence: completeness first, then reconciliation, then adjustments, then review. Skipping steps or reordering creates compounding errors.

Target: close within 5-7 business days after month-end for routine clients; 10 business days for complex (multi-entity, inventory, payroll).

---

## Pre-Close Preparation

Complete before Day 1 of close:

1. **Obtain bank statements** for all accounts (checking, savings, money market, credit cards, loans)
2. **Obtain merchant processor statements** (Stripe, Square, PayPal)
3. **Confirm payroll is posted** — verify payroll provider ran and data is in the GL
4. **Collect client-supplied documents** — invoices, receipts, contracts signed during the month
5. **Review prior month's open items** — any unresolved reconciling items or pending adjustments

---

## Phase 1: Transaction Completeness

Ensure every economic event for the period is recorded before reconciling.

6. **Review and categorize bank feed transactions** — clear the bank feed inbox; do not leave unmatched items
7. **Record accounts receivable** — post all invoices issued during the month; review aging for collectibility
8. **Record accounts payable** — enter all bills received; match to POs where applicable
9. **Post payroll entries** — if using external provider (ADP, Gusto), post summary journal entries for gross wages, employer taxes, withholdings, and net pay (see `bk/payroll-recording.md`)
10. **Record credit card charges** — categorize all charges; match to receipts
11. **Record cash transactions** — petty cash, owner draws, capital contributions
12. **Post recurring journal entries** — rent straight-lining, insurance amortization, loan interest accruals

---

## Phase 2: Reconciliation

Reconcile balance sheet accounts to independent third-party evidence.

13. **Bank reconciliation** — reconcile every bank account to the statement ending balance (see `bk/bank-reconciliation.md`)
14. **Credit card reconciliation** — reconcile each card to the statement (see `bk/credit-card-reconciliation.md`)
15. **Loan reconciliation** — tie loan balances to lender statements; verify principal/interest split
16. **Intercompany reconciliation** — if multi-entity, confirm intercompany balances net to zero
17. **Accounts receivable reconciliation** — tie AR aging total to the GL control account
18. **Accounts payable reconciliation** — tie AP aging total to the GL control account

**Rule**: Do not proceed to adjusting entries until all reconciliations are complete and differences are investigated.

---

## Phase 3: Adjusting Entries

Post entries that align revenue and expense recognition with the accounting period.

19. **Accrued expenses** — record expenses incurred but not yet billed (utilities, professional fees, interest)
20. **Accrued revenue** — record revenue earned but not yet invoiced (milestone completions, time-based services)
21. **Prepaid expense amortization** — expense the current-month portion of prepaid insurance, rent, subscriptions
22. **Depreciation and amortization** — post monthly depreciation for fixed assets; amortize intangibles
23. **Inventory adjustments** — if applicable, adjust for shrinkage, obsolescence, lower-of-cost-or-market
24. **Deferred revenue recognition** — recognize the portion earned during the month
25. **Payroll tax true-up** — reconcile accrued payroll taxes to actual deposits; adjust if needed

---

## Phase 4: Review and Reporting

26. **Run trial balance** — verify debits equal credits; scan for unusual balances (negative assets, credit expense accounts)
27. **Generate financial statements** — Income Statement, Balance Sheet, Statement of Cash Flows
28. **Variance analysis** — compare to prior month and to budget; investigate variances exceeding materiality threshold (typically 10% or $1,000, whichever is greater)
29. **Analytical review** — compute key ratios (current ratio, gross margin, DSO) and flag anomalies
30. **Prepare close memo** — document adjustments made, open issues, and items requiring client attention
31. **Manager/partner review** — second set of eyes on financial statements and reconciliations before delivery

---

## Phase 5: Period Lock

32. **Lock the period in the accounting system** — prevents accidental posting to closed months
33. **Archive supporting documents** — bank statements, reconciliation reports, adjustment memos
34. **Deliver client package** — financial statements, management letter (if applicable), action items

---

## Common Pitfalls

- **Recording net payroll instead of gross** — understates wages expense and omits employer tax liability; always record the full journal entry
- **Skipping balance sheet reconciliation** — reconciling only the bank account and ignoring AR, AP, prepaids, and loans; errors accumulate silently
- **Posting to locked periods** — if the period lock date is not set, late entries slip into prior months and invalidate delivered financials
- **Ignoring stale outstanding checks** — checks outstanding >90 days should be investigated; >180 days may need escheatment review
- **Accrual omission** — forgetting to accrue large month-end expenses (rent if paid in arrears, utilities, contract labor) distorts the P&L
- **Bank feed over-reliance** — auto-matching rules can create duplicates or miscategorize; always review matches manually
- **Not reversing prior-month accruals** — accruals from the prior month must be reversed to avoid double-counting when the actual bill arrives
- **Unclear task ownership** — when multiple team members touch the close, tasks fall through cracks; use a checklist with assigned names

---

## Platform Execution

For platform-specific execution (period lock, reconciliation workflow, recurring transactions, bank feeds, reporting), invoke `qbo-integration:qbo-bookkeeping`.

---

## Decision Points

| Situation | Decision | Guidance |
|-----------|----------|----------|
| Bank rec difference < $5 | Write off to misc expense/income | Document in close memo |
| Bank rec difference $5-$100 | Investigate one round; if unresolved, write off with manager approval | |
| Bank rec difference > $100 | Must resolve before closing | Escalate if unresolved after 2 business days |
| Client missing documents | Close with accrual estimate; reverse next month | Note in close memo; follow up with client |
| Payroll not posted by Day 3 | Accrue based on prior month; reverse when actual posts | |
| Prior-period adjustment discovered | Post to current period if immaterial; restate prior period if material | Materiality = 5% of net income or $5,000, whichever is less |

---

## References

- FloQast. "Month-End Close Checklist." https://floqast.com/blog/month-end-close-checklist
- AICPA. Statements on Standards for Accounting and Review Services (SSARS)
