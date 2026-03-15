---
name: reconciliation
description: >
  Contains verified tolerance thresholds ($5/$50 write-off rules), brokerage external-vs-internal
  flow isolation procedures, and the Schedule L deposit-derivation prohibition that produce more
  precise answers than general training alone. Bank rec, CC liability matching, stale-check
  escalation, rewards accounting, error heuristics, outstanding check aging. Consult when
  reconciling any account or asking "why doesn't my balance match."
---

# Reconciliation

Operational skill for matching GL balances to external statements across bank, credit
card, and investment accounts. Reconciliation is a prerequisite to monthly close and the
primary control against undetected errors, fraud, and timing mismatches.

**Prerequisite**: All transactions for the period must be recorded before reconciling.
Invoke `bookkeeping:transaction-processing` if the GL is not current.

## Core Principle

Two balances start different and must end identical. The reconciliation classifies every
difference as either a timing item (resolves naturally) or an adjusting item (requires a
journal entry). When both adjusted balances agree, the reconciliation is complete.

```
Adjusted External Balance = Statement Ending Balance +/- Timing Items +/- External Errors
Adjusted Book Balance     = GL Ending Balance +/- Unrecorded Items +/- Book Errors
Both must be equal.
```

## Frequency Guidelines

- **Standard accounts**: monthly, tied to statement date
- **High-volume bank accounts** (>200 txns/month): weekly
- **Fraud-risk accounts**: daily
- **Credit cards**: reconcile to statement date (rarely month-end aligned)
- **Investment accounts**: quarterly minimum; monthly when active trading occurs

---

## Bank Account Reconciliation

### Reconciling Items Taxonomy

**Adjustments to bank balance** (in books, not yet on statement):
- Outstanding checks -- issued and recorded but not cleared. Normal if <90 days.
- Deposits in transit -- recorded near cutoff, not yet credited. Should clear in 1-2 business days.
- Bank errors -- incorrect amounts, duplicates, wrong-account postings by the bank.

**Adjustments to book balance** (on statement, not yet in books):
- Bank service charges, wire fees, NSF charges, overdraft fees -- record as bank charges expense
- Interest earned -- record as interest income
- Direct debits / ACH withdrawals -- auto-payments not yet recorded
- NSF (returned) checks -- reverse original deposit, re-debit customer receivable
- Wire transfers received -- inbound wires not yet in GL
- Book errors -- transpositions, duplicates, omissions, misclassifications

### Procedure

1. Gather bank statement, prior month rec, GL cash detail for the period
2. Start with bank statement ending balance
3. Add deposits in transit; subtract outstanding checks; adjust for bank errors
4. Compute adjusted bank balance
5. Start with GL ending balance
6. Add bank credits not in books (interest, inbound wires); subtract bank debits not in books (fees, NSF, ACH)
7. Correct any book errors discovered
8. Compute adjusted book balance
9. Verify agreement -- if not zero, return to step 3
10. Post adjusting entries for all book-side items (steps 6-7) dated as of statement date
11. Preparer signs; reviewer (different person) signs -- segregation of duties required

### Error Detection Heuristics

- **Divisible by 9** -- transposition error (e.g., $54 recorded as $45)
- **Divisible by 2** -- possible debit/credit reversal
- **Equals a round number** -- likely an omitted transaction
- **Equals an existing transaction** -- check for duplicates or omissions

### Stale-Dated Checks

Outstanding check aging triggers escalating action:

- **1 month**: normal, carry forward
- **2 months**: follow up with payee, document attempt
- **90-180 days**: contact payee; void and reissue if lost. Banks may refuse checks >6 months (UCC 4-404).
- **State escheatment**: payroll checks subject to unclaimed property laws (1-3 year holding periods vary by state). If payee unreachable, void check, credit original expense/AP, and remit to state per reporting schedule.

### Timing Differences

Timing differences are normal and do not require correcting entries. Track month-over-month:
- Outstanding checks carrying forward multiple months warrant investigation
- Deposits in transit >1 week suggest recording error or lost deposit
- ACH initiated on last business day may not settle until next month

---

## Credit Card Reconciliation

### Key Differences from Bank Rec

- **Balance direction**: CC balance is a liability (credit normal). Positive statement balance = company owes.
- **Statement date**: rarely aligns with calendar month-end. Always reconcile to statement date.
- **Timing items**: instead of outstanding checks, the typical item is charges made after statement close but already in the books.
- **Payments**: reduce the liability. Credits on statement, debits in GL.

### Procedure

1. Gather CC statement, prior rec, GL detail for the CC liability account
2. Verify opening balance matches prior rec closing
3. Match charges, credits (returns/refunds/rewards), and payments between statement and GL
4. Identify unmatched items: on statement but not in GL (record them); in GL but not on statement (outstanding)
5. Post adjusting entries for interest, fees, rewards, unrecorded transactions
6. Verify: `Adjusted GL Balance = Statement Ending Balance + Outstanding Charges - Outstanding Credits/Payments`
7. Preparer and reviewer sign off

### Multi-Card Setup

Create one GL account per physical card (`VISA - 4455`, `AMEX - 1001`) under a parent "Credit Cards" group account. Combined single-account approaches make clean reconciliation impossible when multiple statements must split one GL balance.

Invoke `accounting-foundation:chart-of-accounts` for account numbering (2200s range for credit cards/short-term notes).

### Cash-Back Rewards Accounting

- **<$500/year**: credit the original expense category (simpler)
- **>=$500/year**: credit Other Income: Credit Card Rewards (preserves gross expenses for budgeting)

Statement credits: DR Credit Card Liability, CR Rewards account.
Direct deposits: DR Bank, CR Rewards account.

### Interest, Fees, and Special Items

- **Interest/finance charges**: Interest Expense (deductible for C-corps)
- **Annual fee**: Bank Fees or Office Expense
- **Late payment fee**: Bank Fees (deductible but avoidable -- flag for review)
- **Foreign transaction fee**: 1-3% of USD amount; Bank Fees or combine with transaction if immaterial
- **Cash advance fee**: Interest Expense (flag -- typically higher rate)
- **Employee personal charges**: record as Employee Advance (receivable); collect repayment within 30 days. Never expense.
- **Disputed charges**: leave at original amount; adjust when issuer resolves. Track in memo.

---

## Investment Account Reconciliation

### The Core Distinction: External vs Internal Activity

A brokerage account has two types of activity. Only external flows generate cash-movement
journal entries. The reconciliation must isolate them.

**External cash flows** (money crossing the bank-brokerage boundary):
- Wire transfers in/out, check deposits, ACH transfers
- These require JEs between the bank and investment GL accounts

**Internal activity** (movement within the brokerage):
- Buying/selling securities (cash to shares and back, inside the brokerage)
- Dividends credited to brokerage cash balance
- Interest earned, foreign tax withheld
- Market appreciation/depreciation

**Critical error to avoid**: When Schedule L shows investments increasing from $X to $Y,
do NOT assume the difference represents cash deposits. Stock portfolios grow through
market appreciation. A $60K increase on Schedule L may represent $0 in actual deposits.

### Reading Brokerage Statements

Every broker formats statements differently, but the task is always the same: find the
section that shows cash movements with the outside world — actual deposits and withdrawals
that crossed the bank-brokerage boundary.

**What to look for**: A section labeled Deposits, Wire Transfers, Journal Entries, Cash
Activity, or similar. Some brokers pre-separate deposits from internal activity on the
summary page; others lump deposits with sale proceeds under labels like "Other Credits."

**Common trap**: Activity summaries that combine external deposits with internal sale
proceeds into a single line. When a summary shows large credits, always drill into the
transaction detail to isolate actual deposits from buy/sell activity.

**Statement types**: Monthly statements with YTD columns give the cleanest data —
one line for annual deposits. Annual-only statements require scanning transaction detail
and filtering by transfer keywords (wires, check deposits, ACH) vs internal keywords
(bought, sold, dividend, interest).

### Self-Reconciliation

Verify the running balance or cash flow equation ties:
`opening_balance + all_transaction_amounts = closing_balance`

If it does not tie, transactions are missing or OCR errors exist. The PDF is authoritative
over OCR metadata.

### Schedule L Comparison

Schedule L Line 6 may carry market value, cost basis, or a hybrid. The CPA may change
methodology between years, creating apparent balance changes that are valuation
reclassifications, not cash movements.

**Correct approach**:
1. Determine external deposits/withdrawals from the brokerage statement
2. Determine portfolio market value from the brokerage statement (opening and closing)
3. Compare Schedule L to brokerage market value
4. If they match: straightforward, Schedule L is at market value
5. If they differ: flag as valuation methodology difference -- investigate, do not plug
6. Never work backwards from Schedule L to infer deposits

### Journal Entries

**External cash transfers** (evidence-based deposit total > 0):
```
DR  Brokerage Investments    {deposit amount}
  CR  Checking               {deposit amount}
```

**Zero deposits** (evidence-based): no cash transfer JE. Do not create one.

**Dividends, capital gains, interest, foreign tax**: standard income/expense JEs within
brokerage accounts -- do not touch operating cash.

**Market value adjustment**: DR/CR Investments and AOCI for unrealized gain/loss changes.

### Red Flags

- Schedule L investment grew but brokerage shows zero deposits: normal (market appreciation)
- Activity Summary "Other Credits" is large: normal (includes sale proceeds)
- Schedule L BOY does not equal prior year brokerage EOY market value: valuation methodology difference
- 1099-DIV shows $0 but brokerage shows dividends: check Income Summary Statement vs reported-to-IRS version
- Only annual Account Summary available (no monthly statements): cannot determine deposits -- flag as blocking gap

---

## Decision Logic

### Tolerance Thresholds

- **Difference <$5**: write off to bank charges (debit) or miscellaneous income (credit). Document in rec workpaper.
- **Difference $5-$50**: investigate one pass. Write off with preparer + reviewer approval if unresolved.
- **Difference >$50**: must resolve. Do not close the reconciliation until explained.

### Situational Decisions

- **Prior-month outstanding item cleared at different amount**: adjust the book entry for the difference. Common with estimated accruals.
- **Bank error identified**: adjust bank side of rec; notify bank in writing; retain written confirmation.
- **CC card has no bank feed**: enter manually from statement; consider switching cards.
- **Brokerage data unavailable**: flag as blocking gap. Do not derive deposits from Schedule L arithmetic.

---

## Documentation Requirements

Every reconciliation must include:

- Period covered and statement date
- Beginning and ending balances from both external statement and GL
- Complete list of reconciling items with descriptions, dates, amounts, and reference numbers
- Adjusted balances showing agreement
- Preparer name and date
- Reviewer name and date (must be a different person)
- Supporting evidence for adjusting entries

**Retention**: 7 years minimum (IRS fraud statute of limitations; 3 years for standard returns).

---

## Platform Execution

For platform-specific reconciliation workflows, bank feed management, matching rules,
and UI navigation, invoke the relevant accounting system integration skill:
- `qbo-integration:qbo-bookkeeping` for QuickBooks Online
- `qbo-integration:qbo-api` for programmatic reconciliation and bank feed API patterns

---

## Supporting References

Read these for deeper detail on specific topics. Each file is the authoritative source
for its domain; the synthesized content above covers the operational essentials.

- `references/bank-reconciliation.md` -- Full bank reconciliation procedure with
  step-by-step walkthrough, reconciling items taxonomy with examples, error detection
  heuristics, stale-check rules with UCC/escheatment citations, and decision matrix for
  unexplained differences. Read when handling edge cases in bank rec or when you need the
  complete procedure checklist.

- `references/credit-card-reconciliation.md` -- CC-specific reconciliation procedure,
  rewards accounting methods with dollar thresholds, interest/fee classification,
  multi-card GL setup, employee personal charge handling, and disputed charge tracking.
  Read when setting up CC reconciliation for a new card or resolving CC-specific
  discrepancies.

- `references/investment-account-reconciliation.md` -- Brokerage statement reading
  patterns, external-vs-internal flow isolation method, Schedule L comparison logic,
  valuation methodology detection, discovery/assessment/reconstruction stage guidance,
  and red flag patterns. Read when reconciling investment accounts or when Schedule L
  balance changes need investigation.

## Cross-Plugin References

- Invoke `accounting-foundation:chart-of-accounts` for account classification, numbering conventions, and GL account setup for cash, CC liability, and investment accounts
- Invoke `qbo-integration:qbo-bookkeeping` for QBO-specific reconciliation workflows, bank feed management, and matching rules
- Invoke `qbo-integration:qbo-api` for programmatic reconciliation and bank feed API patterns
- Invoke `bookkeeping:transaction-processing` as a prerequisite -- all transactions must be recorded before reconciling

## Cross-Plugin Consumers

- `bookkeeping:monthly-close` -- reconciliation is a close prerequisite; feeds the close checklist
- `bookkeeping:review` -- reconciliation workpapers are a primary review artifact
- `tax-prep:form-1120-prep` -- investment account reconciliation informs Schedule L reporting
