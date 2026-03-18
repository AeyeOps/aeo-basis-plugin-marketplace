---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Client Onboarding

Procedures for onboarding new bookkeeping clients, from pre-engagement screening through first-month deliverables.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `client-onboarding`, `bookkeeping-setup`, `bookkeeping-monthly-close`, `client-deliverables`

---

## Pre-Engagement Screening

Evaluate before accepting: team capacity, client complexity (entities, volume, industry), condition of existing books, and red flags (prior accountant fired unexpectedly, IRS notices without explanation, reluctance to share financials, unrealistic fee expectations). Decline or price for risk if red flags present.

## Engagement Letter

Execute before any work. Must specify: scope (bookkeeping only vs bookkeeping + tax), deliverable frequency, client responsibilities (document delivery, response time), fee structure and payment terms, period covered, software responsibility (firm or client pays accounting system subscription), and termination provisions.

## Information Gathering Checklist

**Entity**: legal name, EIN, entity type (C-Corp/S-Corp/LLC/partnership/sole prop), state of formation, fiscal year-end, articles of incorporation

**Financial history**: prior 2+ years tax returns, existing books (cloud accounting software/desktop/spreadsheets/none), current year partials, open AR/AP aging

**Banking**: all bank accounts (institution, type, last 4), all credit cards (issuer, last 4), loan statements with amortization schedules, merchant processors (Stripe, Square, PayPal), POS system

**Payroll**: provider, employee count (W-2) and contractor count (1099), pay frequency, states where employees work

**Tax/compliance**: sales tax permit numbers and registered states, prior IRS/state notices, pending audits

## Accounting System Company Setup

For platform-specific setup steps, invoke `qbo-integration:qbo-bookkeeping`.

1. Create company (or accept accountant invitation to existing)
2. Use legal name (not DBA)
3. Select correct industry, entity type (tax form), fiscal year start month, accounting method (per prior returns)
4. Enable features as needed: class tracking (departmental), location tracking (multi-location), projects (job costing)

## Chart of Accounts

**Do not accept accounting system defaults without review.** Procedure:
1. Review prior tax returns -- line items indicate required accounts
2. Start from firm's entity-type template (invoke `qbo-integration:qbo-coa` for platform-specific setup)
3. Customize: add industry-specific, remove unused, rename for clarity
4. Enable account numbers: 1xxx Assets, 2xxx Liabilities, 3xxx Equity, 4xxx Revenue, 5xxx COGS, 6xxx-7xxx OpEx, 8xxx Other
5. Limit sub-account nesting to two levels
6. Map each account to tax form line

## Opening Balance Entry

**Approach A — Opening Balance Equity** (simplest): enter balances directly in each account; the accounting system offsets to OBE; reclassify OBE to Retained Earnings after all entered. Best for known trial balance from tax return.

**Approach B — Historical JE**: single journal entry debiting all assets, crediting liabilities and equity. No OBE cleanup. Best for CPA-prepared balances.

**Approach C — Import/Migration**: use accounting system import tools or migration from prior platform. Verify imported balances against source trial balance.

**Source**: prior year Schedule L (corps) or accountant's year-end trial balance.

## Bank and Credit Card Connections

1. Link each account in the accounting system's banking tab (client connects via invite flow or shares credentials)
2. Set import start date to first day of engagement (do not import years unless doing catch-up)
3. Create bank rules for recurring transactions
4. Match imported items to manual entries to prevent duplicates
5. Verify account type: bank accounts = "Bank"; credit cards = "Credit Card"

## Historical Data Migration

For catch-up engagements: determine scope (current year vs prior years), gather statements/invoices/receipts for all periods, enter chronologically, reconcile each month as entered (do not defer), document assumptions in a catch-up memo, lock completed periods.

## User Access Setup

- **Accountant**: invite firm via the accounting system's accountant portal (full access + accountant tools). One firm at a time.
- **Admin**: business owner — full access
- **Standard User**: create/edit transactions, run reports (office managers)
- **Reports Only**: view-only (passive business partners)

Principle of least privilege. Review user list during onboarding — remove former employees/prior accountants.

## Recurring Transactions

Set up during onboarding: **Scheduled** (auto-posts) for fixed amounts (rent, loan payments). **Reminder** for variable recurring. **Unscheduled** (template only) for payroll JE templates. Configure type, payee, amount, account, frequency, start/end dates.

## Client Communication Framework

Establish: primary contact (name, email, phone), response time expectation (recommend 3 business days), document delivery method (shared folder or accounting system receipt capture), question protocol (batch weekly), monthly review meeting (first 3 months, then quarterly), escalation path if primary is unresponsive.

## First Month Deliverables

1. Reconciled books (all bank and CC accounts)
2. Financial statements (P&L and Balance Sheet)
3. Open items list (unresolved transactions, missing docs, client questions)
4. COA summary for client confirmation
5. Onboarding close-out memo (setup decisions, assumptions, for permanent file)

## Accounting System Integration Notes

For platform-specific UI paths and procedures, invoke `qbo-integration:qbo-bookkeeping`.

- **Accountant toolbox**: reclassify transactions, write-off invoices, close books, voided/deleted report -- accountant user only
- **Invitation flow**: client initiates via the accounting system's user management
- **Data import**: supports CSV for COA, customers, vendors, invoices, bank transactions; use system template files
- **App integrations**: evaluate during onboarding (payment processors, e-commerce, bill pay, payroll); connect to reduce manual entry
- **Bank feed auth**: some banks require periodic re-authentication by the client

## Decision Points

- Client has existing accounting system -- get accountant access; do not create new company unless irreparable
- Client on desktop software -- migrate using the platform's conversion tool; verify all balances
- No prior books — opening balances from most recent tax return; no return = start from zero with bank statements
- Catch-up 3+ years — price as separate project; do not absorb into monthly fee
- Prior accountant unresponsive — use tax returns for opening balances; file Form 8821 for IRS transcripts
- Cash basis books but accrual returns -- clarify with CPA; set accounting method to match tax return method
- Mixed personal/business transactions — separate during onboarding; set clear policy; use owner draws/contributions
- Multiple entities, one owner -- separate company per entity in the accounting system; intercompany accounts for transfers

## References

- AICPA SSARS -- engagement letter requirements
- For platform-specific setup and import procedures, invoke `qbo-integration:qbo-bookkeeping`
