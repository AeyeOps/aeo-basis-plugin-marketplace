# QBO Special and System Accounts: Behavior, Restrictions, and Implications

**Research date:** 2026-02-24
**Scope:** QuickBooks Online (QBO) — special/system accounts, auto-created accounts, restrictions on modification/deletion, implications of custom alternatives

---

## Overview

When QBO is set up, it auto-creates a set of **default accounts** and **special accounts** that serve internal system functions. Approximately 12 accounts are automatically created, and others are triggered on first use of specific features. Several of these accounts cannot be deleted, renamed, or made inactive, and bypassing them with custom alternatives breaks core QBO workflows.

**Primary reference:**
- [Manage default and special accounts in your QuickBooks Online chart of accounts (AU)](https://quickbooks.intuit.com/learn-support/en-au/help-article/chart-accounts/manage-default-special-accounts-quickbooks-online/L3WvLaIfa_AU_en_AU)
- [Manage default and special accounts (UK)](https://quickbooks.intuit.com/learn-support/en-uk/help-article/chart-accounts/manage-default-special-accounts-quickbooks-online/L3WvLaIfa_GB_en_GB)
- [Manage default and special accounts (CA)](https://quickbooks.intuit.com/learn-support/en-ca/chart-of-accounts/manage-default-and-special-accounts-in-your-chart-of-accounts/01/262191)
- [12 Automatically Created Accounts Explained](https://lendahandaccounting.com/2024/03/10/these-12-accounts-are-automatically-created-in-quickbooks-online/)

---

## The 12 Auto-Created QBO Accounts

| Account | Type | Notes |
|---------|------|-------|
| Accounts Receivable | Current Asset (A/R type) | Created on first invoice |
| Accounts Payable | Current Liability (A/P type) | Created on first bill |
| Undeposited Funds | Current Asset | Permanent, cannot be deleted |
| Opening Balance Equity | Equity | Temporary; created during setup |
| Sales | Income | Default sales income account |
| Cost of Goods Sold | COGS | Created when inventory enabled |
| Expenses | Expense | General expense bucket |
| Other Income | Other Income | Non-operating income |
| Inventory Asset | Current Asset | Created when inventory enabled |
| Payroll Liabilities | Current Liability | Created when payroll enabled |
| Retained Earnings | Equity | System-calculated; no register |
| Uncategorized Income/Expense | Income/Expense | Temporary catch-all; should be reclassified |

Source: [12 Automatically Created Accounts](https://lendahandaccounting.com/2024/03/10/these-12-accounts-are-automatically-created-in-quickbooks-online/)

---

## Accounts Receivable (A/R)

### What it does
QBO auto-creates one A/R account when you create the first invoice. All invoices automatically debit this account; customer payments credit it.

### What breaks if you create a custom A/R account

QBO enforces a single **default A/R account** for all transaction postings. While you can create additional accounts with the "Accounts Receivable" account type, the system:

- **Routes all invoices to the default A/R only** — you cannot select a different A/R account on an invoice form. No dropdown exists on invoices to choose alternative A/R accounts.
- **A/R Aging Report pulls only from the default A/R account.** Balances in custom A/R accounts do not appear in the standard Aging Summary or Aging Detail reports. You must use Transaction Detail by Account to view activity in non-default A/R accounts.
- **Receive Payment cannot target non-default A/R accounts.** The Cash Receipts/Receive Payment workflow is hardwired to the default A/R.
- **Customer-name requirement breaks.** A/R-type accounts require customer names on every transaction. Non-A/R accounts (e.g., "Other Current Asset") cannot be used for invoices at all.

### Creating A/R as "Other Current Asset" instead
If you create a receivables account as an Other Current Asset rather than as an A/R type, invoices will not post to it. Invoice posting is gated on the account having account type = Accounts Receivable.

### Workaround for multiple A/R
Use journal entries to transfer balances between A/R accounts after the fact. This is manual and not integrated into the invoice workflow.

### API behavior
For developers/import pipelines using the QBO API: invoice objects post to whichever A/R account is set as default at the company level. Passing a different A/R account ID via the API may be silently ignored or rejected.

**Sources:**
- [Multiple AR and AP in QBO — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/other-questions/multiple-accounts-payable-and-accounts-receivable-in-qbo/00/958028)
- [QBO Invoicing - Multiple Receivable Accounts](https://quickbooks.intuit.com/learn-support/en-us/payments/qbo-invoicing-multiple-receivable-accounts/00/1509545)
- [Changing default receivable account for QB online](https://quickbooks.intuit.com/learn-support/en-us/account-management/changing-default-receivable-account-for-qb-online/00/1328209)
- [Multiple Receivables GL Accounts & AR Aging Reports](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/multiple-receivables-gl-accounts-can-i-create-multiple-ar-aging/00/1455776)
- [The Subtle Nuances of A/R in QBO — School of Bookkeeping](https://www.schoolofbookkeeping.com/blog/QBOARNuances)
- [Why Invoices Post to AR and AR Other](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/why-are-some-invoices-posting-to-accounts-receivable-and-other/00/1392317)
- [Practical Guide to AR in QBO](https://lendahandaccounting.com/2025/11/05/a-practical-guide-to-accounts-receivable-in-quickbooks-online/)

---

## Accounts Payable (A/P)

### What it does
QBO auto-creates one A/P account when you enter the first vendor bill. All bills automatically credit this account; bill payments debit it.

### What breaks if you create a custom A/P account

Mirrors the A/R situation exactly:

- **All bills post only to the default A/P account.** No dropdown on the bill entry form lets you select a different A/P account.
- **A/P Aging Report pulls only from the default A/P account.** Non-default A/P accounts are invisible to the aging report.
- **Bill Pay function cannot target non-default A/P accounts.**
- **Vendor-name requirement.** A/P accounts require vendor names on every transaction. Using a generic liability account breaks vendor tracking entirely.

### Workaround
Journal entries can transfer balances between A/P accounts, but this is manual and not integrated into the bill entry or payment workflow. If you need vendor-level sub-tracking, use sub-customer or class/location features instead of multiple A/P accounts.

**Sources:**
- [Multiple AR and AP in QBO — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/other-questions/multiple-accounts-payable-and-accounts-receivable-in-qbo/00/958028)
- [BILL.com sync: Multiple or Default AP](https://help.bill.com/direct/s/article/360000021326)

---

## Opening Balance Equity (OBE)

### What it is
A **temporary equity account** that QBO auto-creates during company setup or when adding accounts/balances mid-stream. It acts as the system's balancing offset when it doesn't know the true source of an account's starting balance.

### When QBO auto-creates OBE entries
- You set up a new QBO company and enter asset/liability opening balances
- You link a bank account that already has funds — QBO doesn't know the source, so it credits OBE
- You add a new chart of accounts item with an opening balance
- You perform a QBO conversion from another platform
- You fix errors from prior periods that require balance adjustments
- You merge business entities

### Mechanics
The accounting equation requires Assets = Liabilities + Equity at all times. When QBO enters an asset opening balance (e.g., cash on hand of $10,000), it needs an offsetting credit. Until you tell QBO where that money came from (debt, equity contributions, retained earnings, etc.), it parks the offset in Opening Balance Equity.

### Is it avoidable?
Partially. If you enter all opening balances via properly structured journal entries (debiting assets, crediting specific equity/liability accounts), you bypass OBE entirely. The problem occurs when QBO auto-creates opening balances from account setup wizards or bank feed imports.

### How to clear it
Create a journal entry that reclassifies the OBE balance to appropriate equity accounts:
- Debit: Opening Balance Equity (reduces the balance)
- Credit: Retained Earnings, Common Stock, Additional Paid-in Capital, or Loans Payable — whichever reflects the true origin

### Implications of leaving it open
- The balance sheet shows an unexplained equity figure that confuses auditors, bankers, and investors
- Signals incorrect historical entries per Intuit's own documentation
- Treated as an error indicator in financial reviews

### Can it be deleted?
No. It is a system-created default account. However, the account can be zeroed out via journal entry and then potentially made inactive if the balance is zero.

**Sources:**
- [How to create and adjust Opening Balance Equity — Intuit](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-create-and-adjust-the-opening-balance-equity/00/1460283)
- [Opening Balance Equity Account Issue — Intuit](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/opening-balance-equity-account-issue/00/1008128)
- [What is Opening Balance Equity — Synder](https://synder.com/blog/what-is-opening-balance-equity-in-quickbooks-and-how-to-manage-it/)
- [How to Fix Opening Balance Equity Issues — Juna Financial](https://www.junafinancial.com/fix-opening-balance-equity-quickbooks/)
- [What is Opening Balance Equity — Method.me](https://www.method.me/blog/what-is-opening-balance-equity-in-quickbooks/)
- [Opening Balance Equity in QuickBooks — SaasAnt](https://www.saasant.com/blog/opening-balance-equity-in-quickbooks/)

---

## Retained Earnings

### How QBO handles Retained Earnings

Retained Earnings in QBO is a **calculated equity account** with unusual system behavior that differs from all other accounts.

**Key behaviors:**

1. **No register/no ledger.** The Retained Earnings account does not have a traditional transaction register. If you navigate to it in the Chart of Accounts and try to view its detail, it appears empty. This is expected — it is not a bug.

2. **Auto-calculated at report runtime.** Every time you run a Balance Sheet, QBO calculates Retained Earnings dynamically as the cumulative sum of net income/loss from all prior fiscal years. It is a rollup of P&L history, not a stored balance.

3. **Invisible year-end close.** QBO performs a year-end closing entry automatically on the first day of the new fiscal year, transferring the prior year's net income to Retained Earnings. This transaction is invisible — it does not appear in any journal or transaction list. The year-end close "is not shown on that report or anywhere else for that matter."

4. **Income/expense accounts reset to zero** at the start of each fiscal year due to this invisible close.

5. **Cannot be inactivated.** Retained Earnings is protected — it cannot be made inactive. Attempting to do so will fail silently or return an error.

### Can you post journal entries directly to Retained Earnings?

**Technically yes, practically problematic.** QBO does allow journal entries with Retained Earnings as an account line. However:

- The entry will appear as a manual adjustment on top of the auto-calculated balance
- It does not affect the P&L calculation — only the equity section of the balance sheet
- It does not become part of the invisible year-end close mechanism
- The account has no register to view these JEs; you must run a Quick Report on the account or a General Ledger filtered to that account to see them
- Intuit's guidance is that adjustments should be posted to a separate equity adjustment account rather than directly to Retained Earnings, to preserve the integrity of the auto-calculation

### Recommended approach for prior-period adjustments
Create a dedicated equity account (e.g., "Prior Period Adjustment" or "Opening Retained Earnings") and post your correcting JE there. This keeps the auto-calculated Retained Earnings clean and provides an audit trail.

### What feeds Retained Earnings
All income and expense accounts from all prior fiscal years. If you add historical P&L transactions (even years later), Retained Earnings will recalculate automatically to include them.

**Sources:**
- [Retained Earnings — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/do-more-with-quickbooks/retained-earnings/00/1520226)
- [Manually creating prior year's retained earnings — Intuit](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/manually-creating-prior-year-s-retained-earnings/00/1139782)
- [Why retained earnings is empty but shows in report — Intuit](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/why-retained-earnings-is-empty-but-shows-in-report/00/234283)
- [Retained Earnings Account is Missing — Intuit](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/retained-earnings-account-is-missing/00/488664)
- [How to Adjust Retained Earnings in QuickBooks — Method.me](https://www.method.me/blog/adjust-retained-earnings-in-quickbooks/)
- [How to Adjust, View, and Zero out Retained Earnings — WizXpert](https://www.wizxpert.com/retained-earnings-in-quickbooks/)
- [Bring in previous year's net income from different software](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/bring-in-previous-year-s-net-income-from-different-software/00/695508)
- [Understanding Retained Earnings in QBO — Gentle Frog](https://gentlefrog.com/understanding-retained-earnings-in-quickbooks-online/)

---

## Common Stock and Additional Paid-in Capital (APIC)

### QBO equity account behavior

Unlike Retained Earnings, Common Stock and APIC are **standard equity accounts** with normal ledger behavior. They do not have system-level restrictions analogous to Retained Earnings.

- **Not auto-created.** QBO does not automatically create Common Stock or APIC accounts. You must create them manually under Account Type = Equity.
- **Normal transaction register.** These accounts have full ledger/register views and accept JEs directly.
- **Not calculated.** The balance reflects only what you explicitly post; there is no auto-calculation.
- **Standard Detail Types available:** You can use "Common Stock," "Preferred Stock," "Additional Paid-in Capital," "Other Equity," etc. as the detail type when creating the account.
- **Static by nature.** Common Stock does not change unless you issue or repurchase shares. APIC only changes on equity transactions.

### How to record stock issuances
Journal entry pattern for cash stock issuance:
- Debit: Bank/Cash (amount received)
- Credit: Common Stock (par value of shares issued)
- Credit: Additional Paid-in Capital (proceeds above par value)

### Implications for C-Corp / S-Corp setup
For historical reconstruction contexts, manually creating these accounts and populating them via JE is the correct approach. There is no QBO system dependency on these accounts — they behave like ordinary equity accounts.

**Sources:**
- [Stock (Preferred, Common, Treasury) Entry — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/stock-preferred-common-treasury-entry/00/1121023)
- [Setting up Equity accounts — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/setting-up-equity-accounts/00/975401)
- [Equity Accounts on Your Financial Statements — QuickBooks](https://quickbooks.intuit.com/r/accounting/understanding-the-equity-accounts-on-your-financial-statements/)
- [Understanding Members Equity in QuickBooks — SaasAnt](https://www.saasant.com/articles/understanding-members-equity-in-quickbooks/)
- [S-Corp equity accounts help — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/s-corp-equity-accounts-help/00/287050)

---

## Undeposited Funds

### What it does
Undeposited Funds (UF) is a current asset holding account. When you record a customer payment (check, cash, etc.) without immediately depositing it into a bank account, QBO routes it through UF first. The funds then appear in the Bank Deposit screen, where you batch them into an actual bank deposit transaction. This mimics the physical workflow of batching checks before a bank run.

### System account status
**Undeposited Funds is a permanent system account. It cannot be deleted, and it cannot be made inactive.** This is enforced at the system level regardless of balance.

If you accidentally create a second "Undeposited Funds" account (e.g., "Undeposited Funds-1"), attempting to inactivate it will fail with: "Failed to inactivate some accounts. Unable to make 1 account inactive." The only resolution is to merge the duplicate into the primary UF account.

### Behavior if you try to bypass it
You can configure customer payments to deposit directly to a bank account (bypassing UF) by selecting the bank account in the "Deposit to" field on the Receive Payment screen. This is a valid workflow for single-check deposits. However, UF itself cannot be removed from the COA.

### Implications
- UF balance should normally be zero after all payments have been deposited
- A non-zero UF balance means payments have been recorded but not yet deposited — check for missing Bank Deposit transactions
- JEs to UF are generally not appropriate; the correct workflow is using the Bank Deposit function

**Sources:**
- [How to clear out or deposit undeposited funds — Intuit](https://quickbooks.intuit.com/learn-support/en-us/banking/how-to-clear-out-or-deposit-undeposited-funds/00/1313603)
- [Mistakenly created two undeposited funds accounts — Intuit](https://quickbooks.intuit.com/learn-support/en-us/banking/i-mistakenly-created-two-undeposited-funds-account-now-i-can-t/00/1394966)
- [Undeposited Funds — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/banking/undeposited-funds/00/1183689)

---

## Unapplied Cash Payment Income and Unapplied Cash Bill Payment Expense

These are two additional **system-created special accounts** that many users encounter unexpectedly.

### When they appear
These accounts appear **only on cash-basis P&L reports**, not accrual. They are created and managed entirely by QBO and cannot be deleted.

**Unapplied Cash Payment Income** appears when:
- A customer payment date is earlier than the invoice date it's applied to
- A payment is received and not matched to any invoice (orphaned payment)
- QBO Desktop-to-Online conversions used JEs to adjust A/R (creating timing mismatches)

**Unapplied Cash Bill Payment Expense** appears when:
- A bill payment date is earlier than the vendor bill date
- A bill payment is recorded without a matching bill
- Same migration scenario as above on the A/P side

### System behavior
- QBO creates these accounts automatically to satisfy IRS cash-basis reporting rules: payments received must be recognized as income on the date received, even if no invoice yet exists
- The accounts cannot be deleted
- **Journal entries do not fix these accounts.** JEs affect both cash and accrual simultaneously, but these accounts are cash-basis-only, so a JE will distort accrual-basis reports without fixing the cash-basis issue
- The correct fix is date correction: ensure invoice dates precede payment dates, or create the missing invoice/bill and apply the payment properly

**Sources:**
- [Unapplied Cash Bill Payment Expense — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/unapplied-cash-bill-payment-expense/00/764057)
- [How to clean up Unapplied Cash Payment Income — 5 Minute Bookkeeping](https://5minutebookkeeping.com/unapplied-cash-payment-income-expenses-quickbooks/)
- [Unapplied Cash Payment Income on P&L — Intuit Help](https://quickbooks.intuit.com/learn-support/en-us/help-article/financial-reports/unapplied-cash-payment-income-profit-loss/L35Y3Uvka_US_en_US)
- [Unapplied Cash Payment Accounts in QBO — School of Bookkeeping](https://www.schoolofbookkeeping.com/blog/unapplied-cash-payment-accounts-in-quickbook-online)

---

## Other Feature-Triggered System Accounts

The following accounts are created automatically when specific QBO features are activated:

### Reconciliation Discrepancies
- Created when you finish a bank reconciliation with a non-zero difference and choose to "Add adjusting entry"
- QBO creates this account (under Other Expense) to absorb the discrepancy
- Cannot be deleted while reconciliation data references it

### Sales Tax Payable (per agency)
- Created automatically when sales tax is enabled and configured for a tax agency
- One account per tax agency (e.g., "California Department of Tax and Fee Administration Payable")
- These accounts cannot be deleted and cannot be changed — they are owned by the sales tax engine

### Payroll Liabilities
- Created when payroll is enabled
- Covers federal/state tax withholding, employee benefit deductions
- System-owned; cannot be deleted or meaningfully renamed

### Sales of Product Income
- Created when inventory items are first set up
- If deleted or renamed, QBO will auto-recreate it
- Critical: renaming this account does not prevent QBO from using it — the system tracks by internal ID, not name, but will regenerate if the ID is removed

### Inventory Asset (if applicable)
- Created when the Inventory tracking feature is enabled
- Used exclusively by inventory item valuation; cannot be used for manual entries without affecting COGS calculations

**Sources:**
- [Manage default and special accounts (UK)](https://quickbooks.intuit.com/learn-support/en-uk/help-article/chart-accounts/manage-default-special-accounts-quickbooks-online/L3WvLaIfa_GB_en_GB)
- [What are the default accounts in QBO — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/what-are-the-default-accounts-in-the-chart-of-accounts-in-the/00/1106025)
- [Sales Tax Payable — Intuit Community](https://quickbooks.intuit.com/learn-support/en-us/account-management/how-to-delete-sales-tax-accounts/00/791293)

---

## Summary: Account Restrictions Matrix

| Account | Can Delete? | Can Make Inactive? | Can Post JE? | Auto-Calculated? | What Breaks if Bypassed |
|---------|------------|-------------------|-------------|-----------------|------------------------|
| Accounts Receivable (default) | No | No | Yes | No | All invoices stop routing; AR Aging report breaks |
| Accounts Payable (default) | No | No | Yes | No | All bills stop routing; AP Aging report breaks; Bill Pay breaks |
| Undeposited Funds | No | No | Yes (not recommended) | No | Nothing breaks, but account remains in COA |
| Opening Balance Equity | No | Only if zero balance | Yes | No | Leaves unexplained equity on balance sheet |
| Retained Earnings | No | No | Yes (not recommended) | Yes — cumulative net income | Posts appear as manual adjustments outside auto-calc |
| Common Stock | N/A (must create) | Yes | Yes | No | Nothing — standard equity account |
| Additional Paid-in Capital | N/A (must create) | Yes | Yes | No | Nothing — standard equity account |
| Unapplied Cash Payment Income | No | No | No (JE won't fix) | Yes — cash basis only | Appears on cash P&L when payment/invoice dates misalign |
| Unapplied Cash Bill Pmt Expense | No | No | No (JE won't fix) | Yes — cash basis only | Appears on cash P&L when payment/bill dates misalign |
| Sales Tax Payable (per agency) | No | No | Not recommended | No | Sales tax remittance tracking breaks |
| Reconciliation Discrepancies | No (while referenced) | No | Yes | No | Reconciliation history distorted |

---

## Implications for QBO Reconstruction / Historical JE Import

For historical reconstruction contexts:

1. **All historical JEs should use the QBO default A/R and A/P accounts** — do not create custom A/R or A/P accounts for different years or categories. Use sub-customers and classes/locations for tracking instead.

2. **Retained Earnings does not need to be explicitly set via JE.** If you enter all historical income and expense transactions, QBO auto-calculates Retained Earnings from those. Only create a separate equity account (e.g., "Opening Retained Earnings") if you are entering a single lump-sum prior-year balance rather than rebuilding from individual P&L transactions.

3. **Opening Balance Equity should be zeroed out** as soon as the reconstruction is complete, via a JE reclassifying to appropriate equity accounts (Common Stock, APIC, or a prior-period retained earnings account).

4. **Common Stock and APIC must be created manually** — use Account Type = Equity, and the appropriate Detail Type. These are normal accounts with no system restrictions.

5. **Watch invoice/payment date ordering** when importing historical data — any payment dated before its invoice will generate Unapplied Cash Payment Income on cash-basis reports, which cannot be fixed with a JE.

6. **The A/P Aging and A/R Aging reports will only show the default system accounts** — any additional A/R or A/P accounts created for custom purposes will be invisible to aging reports. Plan the COA accordingly.

---

*All findings sourced from Intuit QuickBooks Community, official QBO help documentation, and third-party QBO expert resources as cited above.*
