# Bank Reconciliation

Procedures for reconciling bank accounts to general ledger balances.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `bookkeeping-bank-rec`, `bookkeeping-monthly-close`, `bookkeeping-review`

## Contents

- [Overview](#overview)
- [Reconciling Items Taxonomy](#reconciling-items-taxonomy)
- [Step-by-Step Procedure](#step-by-step-procedure)
- [Timing Differences](#timing-differences)
- [Stale-Dated Checks](#stale-dated-checks)
- [Documentation Requirements](#documentation-requirements)
- [Common Errors and Detection](#common-errors-and-detection)
- [Decision Points](#decision-points)
- [References](#references)

---

## Overview

Bank reconciliation matches the cash balance per the company's general ledger to the cash balance per the bank statement. The two balances almost never agree initially because of timing differences and recording gaps. The reconciliation identifies and classifies every difference until both adjusted balances match exactly.

Frequency: monthly at minimum. Weekly for high-volume accounts (>200 transactions/month). Daily for accounts with significant fraud risk.

---

## Reconciling Items Taxonomy

### Adjustments to Bank Balance

These items appear in the books but not yet on the bank statement:

- **Outstanding checks** — checks issued and recorded in the GL but not yet cleared the bank. Normal timing difference; becomes a problem only when stale (see below).
- **Deposits in transit** — deposits recorded in the books but not yet credited by the bank. Common when deposits are made near statement cutoff.
- **Bank errors** — incorrect amounts, duplicate postings, or transactions posted to the wrong account by the bank. Rare but must be reported to the bank promptly.

### Adjustments to Book Balance

These items appear on the bank statement but not yet in the books:

- **Bank service charges** — monthly fees, wire fees, NSF charges, overdraft fees. Record as bank charges expense.
- **Interest earned** — interest credited by the bank. Record as interest income.
- **Direct debits/ACH withdrawals** — automatic payments (insurance, loan, subscriptions) not yet recorded.
- **NSF (returned) checks** — deposited checks that bounced. Reverse the original deposit; re-debit the customer's receivable.
- **Wire transfers received** — inbound wires not yet recorded in the books.
- **Book errors** — transposed digits, duplicate entries, omitted transactions, misclassified amounts.

### Error Detection Heuristics

- **Difference divisible by 9** — likely a transposition error (e.g., recording $54 as $45; difference = $9)
- **Difference divisible by 2** — possible debit/credit reversal
- **Difference equals a round number** — likely an omitted transaction
- **Difference equals an existing transaction amount** — check for duplicates or omissions

---

## Step-by-Step Procedure

### 1. Gather Materials

- Bank statement (PDF or online) for the reconciliation period
- Prior month's reconciliation (for beginning outstanding items)
- GL detail for the cash account for the period

### 2. Start with Bank Statement Ending Balance

Begin with the bank's ending balance and adjust for items the bank has not yet processed.

### 3. Add Deposits in Transit

Identify deposits recorded in the GL near month-end that do not appear on the statement. Add these to the bank balance.

### 4. Subtract Outstanding Checks

List all checks issued and recorded in the GL that have not cleared the bank. Subtract from the bank balance. Carry forward any outstanding checks from the prior month's reconciliation that still have not cleared.

### 5. Adjust for Bank Errors

If the bank posted an incorrect amount or duplicate, note the error and adjust the bank balance. Notify the bank in writing.

### 6. Compute Adjusted Bank Balance

`Adjusted Bank Balance = Statement Ending Balance + Deposits in Transit - Outstanding Checks +/- Bank Error Corrections`

### 7. Start with GL Ending Balance

Now work the book side. Begin with the GL cash account ending balance.

### 8. Add Items Credited by Bank but Not in Books

Interest earned, wire transfers received, other credits.

### 9. Subtract Items Debited by Bank but Not in Books

Service charges, NSF checks, wire fees, ACH debits.

### 10. Adjust for Book Errors

Correct any transpositions, duplicates, or omissions discovered during the reconciliation.

### 11. Compute Adjusted Book Balance

`Adjusted Book Balance = GL Ending Balance + Bank Credits Not in Books - Bank Debits Not in Books +/- Book Error Corrections`

### 12. Verify Agreement

Adjusted Bank Balance must equal Adjusted Book Balance. If not, the reconciliation is incomplete — return to step 3 and re-examine.

### 13. Post Adjusting Entries

Record all book-side adjustments (steps 8-10) as journal entries in the GL dated as of the statement date.

### 14. Sign Off

Preparer signs and dates. Reviewer (different person) reviews and signs.

---

## Timing Differences

Timing differences are normal and do not require correcting entries — they resolve naturally as transactions clear.

- **Outstanding checks**: track month-over-month. A check outstanding for one month is normal. Two months warrants follow-up with the payee. Three months or more triggers stale-check procedures.
- **Deposits in transit**: should clear within 1-2 business days. A deposit in transit for more than one week indicates a possible recording error or lost deposit.
- **ACH timing**: ACH transactions initiated on the last business day of the month may not settle until the next month.

---

## Stale-Dated Checks

A check becomes stale when it has been outstanding beyond the issuer's policy or state law:

- **Commercial checks**: typically 90-180 days per company policy; banks may refuse checks >6 months old (UCC 4-404)
- **Payroll checks**: subject to state unclaimed property (escheatment) laws; holding periods vary by state (typically 1-3 years)
- **Government checks**: federal checks are valid for 1 year from issue date

**Procedure for stale checks:**

1. Contact the payee to determine if the check was lost or intentionally not cashed
2. If payee confirms lost — void the original check, re-issue if still owed
3. If payee cannot be located — void the check, credit the original expense account (or Accounts Payable)
4. If escheatment applies — remit to the state unclaimed property division per the state's reporting schedule
5. Document the resolution in the reconciliation workpaper

---

## Documentation Requirements

Every reconciliation must include:

- **Period covered** and statement date
- **Beginning and ending balances** from both bank and GL
- **Complete list of reconciling items** with descriptions, dates, amounts, and reference numbers
- **Adjusted balances** — both sides, showing agreement
- **Preparer name and date**
- **Reviewer name and date** (must be a different person — segregation of duties)
- **Supporting evidence** for adjusting entries (bank notices, error memos)

**Retention**: 7 years minimum (IRS statute of limitations for fraud; 3 years for standard returns).

---

## Common Errors and Detection

| Error | Symptom | Fix |
|-------|---------|-----|
| Duplicate bank feed entry | Two identical transactions in GL; bank rec shows overstated book balance | Delete the duplicate; verify bank feed rules aren't double-importing |
| Check recorded twice | Outstanding check list has duplicate; adjusted bank balance too low | Remove duplicate from GL |
| Deposit recorded in wrong period | Deposit in transit carries forward indefinitely | Move to correct period |
| Transposed digits | Difference divisible by 9 | Find the transposed entry and correct |
| ACH not recorded | Bank shows debit with no GL match | Post the transaction; review auto-pay schedules |
| Wrong account | Bank rec balances but another account is misstated | Reclassify between accounts |

---

## Decision Points

| Situation | Decision | Guidance |
|-----------|----------|----------|
| Small unexplained difference (<$5) | Write off to bank charges or misc income | Document in rec workpaper |
| Unexplained difference $5-$50 | Investigate one pass; write off with preparer + reviewer approval | |
| Unexplained difference >$50 | Must resolve | Do not close until explained |
| Check outstanding >90 days | Contact payee | Document attempt |
| Check outstanding >180 days | Void and review escheatment | Follow state-specific rules |
| Bank error identified | Adjust bank side of rec; notify bank | Keep written confirmation from bank |
| Prior-month outstanding item cleared at different amount | Adjust the book entry for the difference | Common with estimated accruals |

---

## References

- Numeric. "Bank Reconciliation." https://numeric.io/blog/bank-reconciliation
- UCC Article 4-404 — Bank's right to refuse stale checks
- IRS Publication 583 — Recordkeeping for small business
- AICPA AU-C Section 501 — Audit evidence for specific items (bank confirmations)
