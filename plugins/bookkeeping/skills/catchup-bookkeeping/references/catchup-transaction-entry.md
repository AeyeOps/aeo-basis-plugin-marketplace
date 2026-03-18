---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Catch-Up Transaction Entry

Transaction entry strategies and bank statement classification for multi-year catch-up bookkeeping.

## Contents

- [Overview](#overview)
- [Monthly Summary vs Detailed Transactions](#monthly-summary-vs-detailed-transactions)
- [Monthly Summary Journal Entry](#monthly-summary-journal-entry)
- [Bank Statement Import](#bank-statement-import)
- [See Also](#see-also)

---

## Overview

This reference covers entering historical transactions for catch-up bookkeeping:
- Choosing between summary and detailed approaches
- Creating monthly summary journal entries
- Classifying bank statement transactions by type
- Creating deposit, expense, and transfer entries

See [catchup-planning.md](catchup-planning.md) for assessment and setup.

---

## Monthly Summary vs Detailed Transactions

### Monthly Summary Approach (Faster, less granular)
- One journal entry per month per category
- Best for: Catch-up periods > 2 years, limited documentation
- Audit risk: Higher — less transaction-level detail

### Detailed Transaction Approach (Slower, more accurate)
- Enter individual transactions from statements
- Best for: Recent periods, audit preparation, tax planning
- Audit risk: Lower — full trail available

---

## Monthly Summary Journal Entry

### Data Model

Each monthly summary captures one account category's total for a single month:

- **account_name** — GL account receiving the summary posting (e.g., "Sales Revenue", "Rent")
- **year** — Calendar year
- **month** — Month number (1-12)
- **total** — Net amount for this category during the month (positive = debit, negative = credit)
- **description** — Optional memo for the line item

### Workflow

**For each month in the catch-up period**:

1. **Collect summaries** — Categorize every bank/credit card statement line item into an account. Sum each account's transactions for the month into a single total.

2. **Determine the journal entry date** — Use the last calendar day of the month (e.g., January = Jan 31, February = Feb 28 or 29).

3. **Build journal entry lines** — For each account category with a non-zero total:
   - Positive totals (expenses, cost of goods) become debit lines
   - Negative totals (income received, credits) become credit lines
   - Each line references the GL account and the absolute dollar amount

4. **Add the bank offset line** — Calculate the net of all category lines. The bank account receives the balancing entry:
   - If net category amount is positive (net outflow), credit the bank account
   - If net category amount is negative (net inflow), debit the bank account

5. **Set the private note** — Tag the journal entry with "Monthly summary for MM/YYYY" for later identification and reconciliation.

6. **Post the journal entry** — Record in the general ledger dated the last day of the month.

### Validation

After posting each monthly summary:
- Verify the bank account GL balance matches the ending bank statement balance for that month
- Check for negative expense account balances (indicates a categorization error)
- Confirm revenue totals are plausible relative to known business activity

---

## Bank Statement Import

### Transaction Classification

Each bank statement line item is classified into one of five types based on its characteristics:

- **Deposit** — Positive amount, represents incoming funds. Record as a deposit entry coded to the appropriate income account (Sales Revenue, Service Revenue, Interest Income, or Other Income).

- **Withdrawal** — Negative amount, represents a payment or expense. Record as an expense entry coded to the appropriate expense account based on vendor or description pattern matching.

- **Transfer** — Movement between the entity's own accounts (e.g., checking to savings, checking to credit card payment). Record as a transfer entry specifying the source and destination accounts. Do not code transfers to income or expense accounts.

- **Check** — Negative amount with a check number. Record as a check payment entry with the check number stored as a reference. Code to the appropriate expense account.

- **Fee** — Bank or card fee (service charge, overdraft fee, monthly maintenance). Record as an expense entry coded to Bank Fees.

### Import Workflow

**For each bank/credit card statement in the catch-up period**:

1. **Identify the bank account** — Match the statement to the corresponding GL bank or credit card account. If the account does not exist in the chart of accounts, create it first.

2. **Classify each line item** — Assign a transaction type (deposit, withdrawal, transfer, check, fee) based on amount sign, description, and check number presence.

3. **Assign account coding** — Map each transaction to a GL account:
   - Deposits → Income account based on source (customer payment, interest, refund, etc.)
   - Withdrawals → Expense account based on vendor/description
   - Transfers → Destination account (another bank account, credit card, loan)
   - Checks → Expense account, with check number as document reference
   - Fees → Bank Fees expense account

4. **Handle unknowns** — When the expense category cannot be determined, code to Miscellaneous. When the income category cannot be determined, code to Other Income. Flag both for client review. Never skip a transaction — skipped transactions break reconciliation.

5. **Enter transactions** — Record each classified transaction in the general ledger with:
   - Transaction date (from statement)
   - Amount (absolute value)
   - Source account (the bank/credit card account)
   - Destination account (income, expense, or transfer target)
   - Description (from statement line item)
   - Document reference (check number, if applicable)
   - Private note: "Imported: [original description]"

### Credit Card Statements

Process credit card statements using the same classification workflow, but with the credit card liability account as the source:

- Each charge → Expense entry against the credit card account
- Payments received from checking → Transfer entry from bank account to credit card account
- Credits/refunds → Negative expense entry or income entry, depending on nature

---

## See Also

- [catchup-planning.md](catchup-planning.md) — Assessment and setup
