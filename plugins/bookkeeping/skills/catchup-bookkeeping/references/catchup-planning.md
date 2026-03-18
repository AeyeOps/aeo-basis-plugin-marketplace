---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Multi-Year Catch-Up: Planning and Setup

Assessment, chart of accounts setup, and opening balances for catch-up bookkeeping.

See also: [catchup-transaction-entry.md](catchup-transaction-entry.md) for transaction entry.

## Contents

- [Overview](#overview)
- [Phase 1: Assessment and Planning](#phase-1-assessment-and-planning)
- [Phase 2: Chart of Accounts Setup](#phase-2-chart-of-accounts-setup)
- [Phase 3: Opening Balances](#phase-3-opening-balances)
- [Workflow Summary](#workflow-summary)

---

## Overview

Multi-year catch-up applies when:
- A business has fallen behind on bookkeeping
- Migrating from another system with incomplete data
- Reconstructing records from bank/brokerage statements
- Preparing for audit or tax filing of prior years

---

## Phase 1: Assessment and Planning

### Gather Source Documents

**Bank and Financial Statements**
- All bank statements (checking, savings, money market)
- Credit card statements
- Loan statements and amortization schedules
- Brokerage/investment account statements

**Income Documentation**
- Invoices issued
- Payment receipts
- 1099s received (for prior years)
- Sales reports

**Expense Documentation**
- Bills and receipts
- Payroll records
- Tax filings (if any exist)
- Contracts and agreements

### Determine Starting Point

Assess the current state of the company file before entering any catch-up transactions.

**Steps**:

1. **Count existing accounts** — Determine whether the company file has accounts beyond system defaults. A minimal default chart indicates a fresh start; a populated chart may indicate partial setup.

2. **Check Opening Balance Equity** — Query the Opening Balance Equity account balance. A non-zero balance means someone previously attempted to set up opening balances. Record this value as context for later OBE cleanup.

3. **Find earliest existing transaction** — Search journal entries, deposits, expenses, and invoices ordered by date ascending. The earliest transaction date establishes what period coverage already exists in the system.

4. **Build gap analysis** — Compare existing transaction dates against bank statement periods. Identify months with no recorded activity. These gaps define the scope of the catch-up work.

**Assessment output**:
- `has_opening_balances` — Whether an OBE balance exists (true/false)
- `earliest_transaction` — Date of the first transaction found, or null if the file is empty
- `account_count` — Number of accounts in the chart of accounts
- `gaps_detected` — List of month/year periods with no transaction coverage

### Define Catch-Up Period

Establish the catch-up period boundaries based on the assessment:

- **Start date** — Day after the last reliable closing, or incorporation date if no books exist
- **End date** — Current month, or the last period needed for an upcoming filing deadline
- **Fiscal year end month** — Calendar year-end (month 12) unless the entity uses a non-calendar fiscal year

**Enumerate work units**:
1. List every calendar year (or fiscal year) in the range
2. For non-calendar fiscal years, calculate each fiscal year's start and end dates (e.g., fiscal year ending June = July 1 through June 30)
3. Plan work sequentially, oldest fiscal year first
4. Each fiscal year should balance independently before moving to the next

---

## Phase 2: Chart of Accounts Setup

### Minimum Account Structure

Create accounts organized by GAAP classification. Only create accounts that will actually receive postings during catch-up — add others as needed during transaction entry.

**Assets**
- Bank — Operating Checking, Savings (one account per actual bank account)
- Other Current Asset — Undeposited Funds
- Accounts Receivable — Accounts Receivable
- Fixed Asset — Fixed Assets, Accumulated Depreciation
- Other Asset — Security Deposits

**Liabilities**
- Accounts Payable — Accounts Payable
- Credit Card — Company Credit Card (one account per actual card)
- Long-Term Liability — Notes Payable

**Equity**
- Owner's Equity (or Common Stock / Retained Earnings for C-corps)
- Owner's Draw (or Shareholder Distributions for C-corps)
- Opening Balance Equity (temporary, cleared at end of catch-up)

**Income**
- Sales Revenue
- Service Revenue
- Interest Income
- Other Income

**Cost of Goods Sold**
- Cost of Goods Sold (expand if the business has distinct cost categories)

**Expenses**
- Advertising
- Bank Fees
- Insurance
- Office Supplies
- Professional Services
- Rent
- Utilities
- Payroll Expenses
- Travel
- Meals
- Miscellaneous

**Notes**: Duplicate account names are silently skipped by most accounting systems. Account subtypes and numbering conventions should follow the firm's standard chart; invoke `accounting-foundation:chart-of-accounts` for the canonical structure.

---

## Phase 3: Opening Balances

Opening balances represent account values on the day before your catch-up period starts.

### When Opening Balances Are Needed

- Starting from a clean company file with no prior data
- Known asset/liability balances exist from prior tax returns or financial statements
- Bank accounts had balances on the catch-up start date

### How to Record

Create a single journal entry dated one day before the catch-up start date.

**Construct the journal entry**:

1. **Gather known balances** — For each account, collect the balance as of the opening date from bank statements, prior tax returns, or financial statements. Record each balance with its account name, account type (Asset, Liability, Equity, etc.), and dollar amount.

2. **Determine debit vs. credit for each line** — Accounts with normal debit balances (Asset, Expense, COGS) are debited for positive balances and credited for negative balances. Accounts with normal credit balances (Liability, Equity, Revenue) are credited for positive balances and debited for negative balances.

3. **Build the journal entry lines** — Each account with a non-zero balance becomes one line in the journal entry. Skip zero-balance accounts.

4. **Calculate the OBE balancing line** — Sum all debits and all credits. The difference between total debits and total credits becomes the Opening Balance Equity line. If debits exceed credits, OBE is credited for the difference. If credits exceed debits, OBE is debited.

5. **Set the journal entry date and memo** — Date the entry one day before the catch-up start date. Add a private note: "Opening balances for catch-up bookkeeping."

### Opening Balance Equity Cleanup

After entering opening balances, the OBE account holds the net difference. As catch-up progresses:

- OBE should trend toward zero as retained earnings from catch-up periods accumulate
- Any residual balance after all catch-up periods are closed indicates a data gap
- Final cleanup: reclassify remaining OBE balance to Retained Earnings with a journal entry and memo explaining the source

---

## Workflow Summary

1. **Assess** — Evaluate current company file state and gather documents
2. **Plan** — Define catch-up period and approach (summary vs detailed)
3. **Setup** — Create chart of accounts and opening balances

Continue to [catchup-transaction-entry.md](catchup-transaction-entry.md) for transaction entry.
