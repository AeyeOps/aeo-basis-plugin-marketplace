---
name: audit-support
description: >
  Contains verified lead schedule reference map (A–EQ with supporting codes), SSARS engagement
  level decision tree (AR-C 70/80/90 independence gates), and AJE/RJE posting patterns with
  workpaper cross-references. PBC lists, tickmark conventions, workpaper packaging, management
  representation letters, compilation vs review. Consult when "building the PBC list for the
  year-end audit" or preparing lead schedules, posting auditor adjustments, choosing compilation
  vs review, or assembling workpaper packages.
---

# Audit Support

Operational skill for preparing books and deliverables for external audit, compilation, and
review engagements. Covers the bookkeeper's responsibilities — not the auditor's. The
bookkeeper assembles supporting schedules, posts adjustments, generates PBC lists, and ensures
books are audit-ready before the external team arrives.

## Prerequisites

Books must be through monthly close before audit prep begins. Invoke `bookkeeping:monthly-close`
to confirm close status. All bank reconciliations, subledger reconciliations, and accruals
must be complete for the period under audit.

---

## SSARS Engagement Levels

Three levels of service in ascending assurance order. The engagement level determines what the
bookkeeper must prepare and how financial statements are presented.

### Preparation (AR-C 70)

- No assurance provided, independence not required
- No report required (optional disclaimer: "No assurance is provided on these financial statements")
- Bookkeeper assembles financial statements from client-provided information
- No verification, corroboration, or analytical procedures required
- Appropriate for: internal use, bank loan packages not requiring compilation, tax return support

### Compilation (AR-C 80)

- No assurance provided, independence not required (must disclose lack if impaired)
- Report required — must accompany the financial statements
- Accountant reads statements for appropriate form and obvious material misstatement
- May omit substantially all disclosures if not done to mislead (disclose in report)
- Independence impairment: performing bookkeeping for the client impairs independence; can still compile with disclosure

### Review (AR-C 90)

- Limited assurance (negative: "not aware of any material modifications")
- Independence required — cannot perform if firm does bookkeeping and independence is impaired
- Requires inquiry, analytical procedures (horizontal, vertical, ratio, reasonableness), and management representation letter
- Management representation letter refusal = withdraw from engagement
- Going concern evaluation required (one year from statement date)

### Choosing the Level

- **Preparation** when no third-party assurance needed (internal, tax-basis)
- **Compilation** when lender or other party needs CPA-associated statements but not assurance
- **Review** when limited assurance required (lender covenant, investor, owner preference)
- If the firm performs bookkeeping, independence is impaired — compilation with disclosure is the ceiling unless a separate independent firm performs the review

---

## Engagement Letters

Required for all three levels. Must be signed before engagement begins.

Required elements: objective/scope, management responsibilities (fair presentation, internal
controls, fraud prevention, completeness of information), practitioner responsibilities
(assemble / present in proper form / obtain limited assurance), limitations, financial
reporting framework (GAAP, tax basis, cash basis), fee arrangement.

---

## Lead Schedules

Lead schedules summarize account groups and link to supporting schedules. Each uses a standard
alphabetic reference.

### Standard Reference Map

**Balance Sheet**:
- **A** — Cash and Cash Equivalents (Bank accounts). Supporting: A-1 Bank Reconciliations, A-2 Outstanding Checks
- **B** — Investments (Other Current Asset, Other Asset matching investment/securities/brokerage). Supporting: B-1 Investment Schedule, B-2 Realized Gains/Losses
- **C** — Accounts Receivable. Supporting: C-1 AR Aging, C-2 Subsequent Collections, C-3 Allowance Roll
- **D** — Prepaid Expenses (Other Current Asset matching "prepaid"). Supporting: D-1 Prepaid Schedule
- **E** — Fixed Assets. Supporting: E-1 Fixed Asset Roll, E-2 Additions, E-3 Depreciation
- **F** — Intangible Assets (Other Asset matching intangible/goodwill). Supporting: F-1 Intangibles Schedule
- **AA** — Accounts Payable. Supporting: AA-1 AP Aging, AA-2 Subsequent Disbursements
- **BB** — Accrued Liabilities (Other Current Liability). Supporting: BB-1 Accruals Schedule
- **CC** — Debt (Long Term Liability). Supporting: CC-1 Debt Schedule, CC-2 Interest Calculation
- **EQ** — Equity. Supporting: EQ-1 Equity Rollforward

**Income Statement**:
- **PL-R** — Revenue (Income). Supporting: PL-R-1 Revenue Analysis
- **PL-E** — Operating Expenses (Expense). Supporting: PL-E-1 Expense Analysis

### Lead Schedule Structure

Each lead schedule contains: prior year balance, current year balance, adjustments (AJEs/RJEs),
per-audit balance (current + adjustments), and links to supporting schedules. The per-audit
column is what appears in the final financial statements.

### Best Practices

- Cross-reference every supporting schedule to its lead
- Date and initial all workpapers (preparer and reviewer)
- Use consistent tickmarks from the standard legend (see Tickmark Conventions below)
- Highlight variances exceeding materiality threshold
- Document what was done, not just the result

---

## Tickmark Conventions

Standard tickmarks used across all workpapers:

- **^** — Footed and cross-footed
- **T** — Traced to trial balance
- **S** — Traced to subsidiary ledger
- **V** — Vouched to supporting documentation
- **C** — Confirmed
- **R** — Recalculated
- **I** — Inspected
- **@** — Agreed to prior year workpapers
- **PY** — Per prior year audit
- **TB** — Agreed to trial balance
- **GL** — Traced to general ledger
- **BS** — Traced to bank statement
- **BR** — Agreed to bank reconciliation
- **INV** — Traced to invoice
- **CTR** — Traced to contract

Include a tickmark legend sheet in every workpaper package.

---

## PBC (Prepared by Client) Lists

PBC lists document every item the client must provide to auditors. Generate early — the PBC
list drives audit preparation timelines.

### Standard PBC Categories

- **General** — Trial balance, bank reconciliations, bank statements (full year + subsequent month), GL detail, chart of accounts, board minutes, articles of incorporation, org chart
- **Cash and Investments** — Bank recs, 12 months + January statements, investment statements, brokerage confirmations, outstanding check list, deposit in transit detail
- **Accounts Receivable** — AR aging, subsequent collections, bad debt write-offs with authorization, credit memos, revenue recognition policy, major customer contracts
- **Fixed Assets** — Fixed asset register, additions/disposals detail, depreciation schedule, invoices for major additions (>$5,000), capitalization policy
- **Accounts Payable** — AP aging, vendor reconciliations (major vendors), subsequent disbursements, unrecorded liabilities search
- **Debt and Equity** — Loan agreements/amendments, amortization schedules, interest rate documentation, stock ledger/cap table, shareholder agreement, dividend declarations
- **Payroll** — Payroll registers (all periods), W-2 reconciliation to GL, 401(k) contribution reconciliation, 941s, workers comp policy
- **Taxes** — Prior year returns (federal/state), tax provision calculation, estimated payments, sales tax returns, property tax bills
- **Related Party** — Related party transaction listing, intercompany reconciliations, officer/shareholder loan agreements, related party lease agreements
- **Inventory** (if applicable) — Inventory listing with quantities/costs, count procedures, obsolescence analysis, cost flow documentation

### PBC Status Tracking

Track each item through these statuses: **pending** (not received) → **requested** (follow-up sent) → **received** (not yet reviewed) → **reviewed** (no issues) or **issue** (follow-up needed). Mark **n/a** for items not applicable to the engagement.

---

## Adjusting and Reclassifying Journal Entries

### Entry Types

- **AJE** (Adjusting Journal Entry) — Affects the books. Changes account balances and financial statement amounts. Posted to the GL.
- **RJE** (Reclassifying Journal Entry) — Presentation only. Moves amounts between accounts for proper classification without changing totals. Does not affect the underlying books.
- **PJE** (Proposed/Passed Journal Entry) — Identified but not posted. Auditor determined the amount is below materiality or management declined to record. Tracked in the summary of unadjusted differences.

### Common AJE Patterns

**Revenue Recognition**:
- Defer unearned revenue — DR Revenue, CR Deferred Revenue (liability). Workpaper ref: PL-R-1
- Accrue unbilled revenue — DR Unbilled AR, CR Revenue. Workpaper ref: C-1

**Accrued Expenses**:
- Accrue expense — DR Expense, CR Accrued Liabilities. Workpaper ref: BB-1
- Reclassify to prepaid — DR Prepaid Expense (asset), CR Expense. Workpaper ref: D-1

**Bad Debt**:
- Allowance method — DR Bad Debt Expense, CR Allowance for Doubtful Accounts. Workpaper ref: C-3
- Direct write-off — DR Bad Debt Expense, CR Accounts Receivable. Workpaper ref: C-3

**Fixed Assets**:
- Depreciation adjustment — DR Depreciation Expense, CR Accumulated Depreciation. Workpaper ref: E-3
- Capitalize expense — DR Fixed Asset, CR Expense. Workpaper ref: E-2

### Adjustment Documentation

Every adjustment must include: reference number (AJE-1, RJE-3), entry type, description,
workpaper reference linking to the lead schedule, preparer/reviewer initials, date prepared,
date posted, and platform JE ID once posted. All entries must balance (total debits = total credits).

---

## Workpaper Package Assembly

A complete audit workpaper package contains:

1. **Trial balance** — Current year-end, with prior year comparison
2. **PBC request list** — All categories, with status tracking
3. **Lead schedules** — One per standard reference (A through PL-E), each showing prior balance, adjustments, and per-audit balance
4. **Supporting schedules** — Detail behind each lead (bank recs, aging reports, asset rolls, etc.)
5. **Tickmark legend** — Standard legend sheet included in every workbook
6. **Adjustment summary** — All AJEs and RJEs with posting status

---

## Platform Execution Notes

For platform-specific trial balance extraction, adjustment posting, and period lock, invoke `qbo-integration:qbo-audit`.

---

## Review Engagement Analytical Procedures

When supporting a review engagement (AR-C 90), the bookkeeper may need to prepare supporting
analyses for the reviewer's analytical procedures:

- **Horizontal analysis** — Period-over-period comparisons; flag changes exceeding materiality
- **Vertical analysis** — Each line as percentage of base (revenue for P&L, total assets for BS)
- **Ratio analysis** — Liquidity (current, quick), profitability (gross margin, net margin), leverage (debt-to-equity), activity (inventory turnover, DSO)
- **Reasonableness tests** — Independent estimates (headcount x average salary = expected payroll; square footage x rate = expected rent)

The reviewer documents expectations, investigation thresholds, results, and conclusions.

---

## Management Representation Letter

Required for review engagements only (AR-C 90). Obtained at or near the report date.

Key representations: financial statements conform to framework, all transactions recorded,
estimates are reasonable, related party transactions disclosed, subsequent events handled,
no fraud or suspected fraud, no known legal violations, all litigation disclosed, contractual
compliance, information provided is complete and accurate.

If management refuses to sign, the accountant must withdraw from the engagement.

---

## Decision Points

- **Engagement level selection** — See SSARS Engagement Levels above. Independence status is the primary gate between compilation and review.
- **Framework selection** — GAAP, tax basis (IRC), cash basis, modified cash basis, regulatory basis. Tax basis is common for small C-corps needing statements primarily for tax return preparation. Document in the engagement letter.
- **Omit disclosures** — Permitted in compilation only, if not done to mislead. Must disclose in report. Never omit in review.
- **Change in engagement level** — Downgrade from review to compilation: acceptable if change in circumstances or scope misunderstanding, unacceptable if done to avoid disclosing a departure. Compilation report must not reference the original review.
- **Scope limitation in review** — If unable to perform required procedures (e.g., no representation letter, analytical procedures impossible for significant area), consider withdrawal rather than modified report.
- **Going concern** — Review: must evaluate. Compilation: no requirement, but disclose if aware. Preparation: no requirement.

---

## SSARS Effective Date Log

- **SSARS 21** (periods ending on/after 2015-12-15) — Established AR-C 60/70/80/90 codification; introduced preparation engagements
- **SSARS 23** (reports dated on/after 2016-06-15) — Revised report language, clarified withdrawal requirements
- **SSARS 24** (periods ending on/after 2019-06-15) — Predecessor accountant and group financial statement guidance
- **SSARS 25** (periods ending on/after 2021-12-15) — Aligned materiality concepts with audit standards; revised analytical procedures
- **SSARS 26** (periods ending on/after 2025-12-15) — SQMS integration, enhanced documentation, updated group financial statement considerations

---

## Supporting References

Read these for implementation detail and code patterns beyond what is covered above:

- `references/audit-aje-patterns.md` — Full data structures for AJE/RJE/PJE entries with Python implementations. Read for the `AuditAdjustment` and `AdjustmentLine` data models, and complete factory functions for revenue, accrued expense, bad debt, and fixed asset adjustments.
- `references/audit-lead-schedules.md` — Lead schedule generation logic with account-to-lead mapping, workpaper package assembly function, and tickmark legend implementation. Read when building lead schedules programmatically or generating a complete workpaper package.
- `references/audit-pbc-lists.md` — PBC list generation with full category definitions, status tracking model, and Excel export implementation. Read when generating PBC lists for distribution to clients.
- `references/ssars-compilation-review.md` — Comprehensive SSARS guidance covering engagement letters, procedures for all three levels, management representation letter content, report language (compilation and review), modified reports, going concern evaluation, supplementary information, and the full effective date log with SSARS 21–26. Read for engagement letter drafting, report wording, or standards questions.

## Cross-Plugin References

- Invoke `qbo-integration:qbo-audit` for QBO-specific audit data extraction, trial balance pulls, and adjustment posting patterns
- Invoke `accounting-foundation:financial-statements` for financial statement presentation requirements and formatting
- Invoke `accounting-foundation:chart-of-accounts` for account structure, numbering conventions, and account classification questions
- Invoke `bookkeeping:monthly-close` for close status verification and prerequisite procedures

## Cross-Plugin Consumers

- `firm-operations:quality-compliance` — References this skill for QC standards on compilation/review engagements
- `tax-prep:form-1120-prep` — Uses adjusted trial balance as input for return preparation
