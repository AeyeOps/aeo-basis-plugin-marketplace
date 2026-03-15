# Credit Card Reconciliation

Procedures for reconciling credit card accounts, handling rewards, fees, and resolving common discrepancies.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `bookkeeping-bank-rec`, `bookkeeping-monthly-close`, `bookkeeping-review`

---

## Credit Card vs Bank Reconciliation

- **Balance direction**: credit card balance is a liability (credit normal). Positive statement balance = company owes money.
- **Timing items**: instead of outstanding checks, the typical item is charges made after statement close but already in the books.
- **Payments**: payments reduce the liability — credits on the statement, debits in the GL.
- **Statement date**: rarely aligns with calendar month-end. Reconcile to the statement date, not month-end.

## Step-by-Step Procedure

1. **Gather materials**: credit card statement, prior reconciliation, GL detail for the CC liability account
2. **Verify opening balance**: current statement opening must match prior reconciliation closing
3. **Match transactions**: compare charges, credits (returns/refunds/rewards), and payments between statement and GL
4. **Identify unmatched items**: on statement but not in GL (record them); in GL but not on statement (outstanding — carry forward)
5. **Post adjusting entries**: interest, fees, rewards, unrecorded transactions
6. **Verify**: `Adjusted GL Balance = Statement Ending Balance + Outstanding Charges - Outstanding Credits/Payments`
7. **Sign off**: preparer and reviewer sign and date

## Cash-Back Rewards Accounting

**Approach A — Reduce Expense** (simpler): credit the original expense category. Pro: ties benefit to spending. Con: reduces expense below actual cost; complicates budgeting.

**Approach B — Other Income** (recommended for $500+/year): credit Other Income: Credit Card Rewards. Pro: preserves gross expenses for budgeting. Con: inflates both revenue and expense totals.

Statement credits reduce CC liability (debit CC, credit rewards account). Direct deposits hit checking (debit bank, credit rewards account).

## Interest Charges and Fees

- **Interest/finance charges**: Interest Expense — deductible for C-Corps
- **Annual fee**: Bank Fees or Office Expense — record when charged
- **Late payment fee**: Bank Fees (deductible but avoidable)
- **Foreign transaction fee**: 1-3% of USD amount; record as Bank Fees or combine with the transaction if immaterial. For frequent international purchases, consider a no-foreign-fee card.
- **Cash advance fee**: Interest Expense — flag for review (typically higher rate)

## Multi-Card Reconciliation

**One account per card** (recommended): create separate Credit Card type accounts (`VISA - 4455`, `AMEX - 1001`). Clean reconciliation per statement; matches bank feed import; easy to identify cardholder spending. Use a parent account ("Credit Cards") with sub-accounts for presentation.

**Combined account** (not recommended): impossible to reconcile cleanly when multiple statements must split a single GL balance. Bank feeds from different cards intermingle.

## Common Issues

- **Duplicates**: bank feed + manual entry creates two transactions — use "Match" not "Add" for items already in GL
- **Statement cutoff timing**: charges after close appear in GL but not on statement — treat as outstanding
- **Returns at different amounts**: partial refunds require matching to original and adjusting the difference
- **Recurring charges not recorded**: review statement for auto-pay subscriptions; set up recurring transaction templates
- **Disputed charges**: track in a memo; reconcile when issuer resolves
- **Employee personal charges**: record as Employee Advance (receivable); collect repayment within 30 days

## Accounting System Notes

- **Account type**: create as "Credit Card" (not Bank) — ensures liability treatment and correct reconciliation behavior
- **Payment recording**: use a transfer between Checking and Credit Card accounts, not an expense entry. An expense entry creates phantom cost; a transfer correctly reduces both asset and liability.

For platform-specific reconciliation workflows, bank feed setup, and matching rules, invoke the relevant accounting system integration skill.

## Decision Points

- Small difference (<$5) — write off to bank fees; document
- Difference >$5 — must resolve; review statement line-by-line
- Employee personal charge — record as receivable; collect within 30 days; do not expense
- Rewards >$500/year — use Other Income method for budget clarity
- Card has no bank feed — enter manually from statement; consider switching cards
- Disputed charge pending — leave at original amount; adjust when resolved

## References

- Accounting system vendor documentation — Credit card reconciliation procedures
- IRS Publication 535 — Business Expenses (interest deductibility)
- AICPA SSARS — Credit Card Accounting Best Practices
