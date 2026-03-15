# QBO Chart of Accounts: Community-Reported Problems and Pitfalls

> Research compiled from QuickBooks Community forums, CPA ProAdvisor blogs, and accounting practitioner resources.
> All findings sourced from real user reports and professional commentary.
> Date compiled: 2026-02-24

---

## 1. Account Type and Detail Type Errors

### The Most Dangerous Technical Mistake

Assigning the wrong account **type** is widely described by ProAdvisors as the single most consequential COA error. Account types determine which financial statement an account appears on. A wrong type means balance sheet accounts appear on the P&L or vice versa — and both statements are then wrong.

**How it happens:** A new transaction category is needed, the user adds an account but doesn't notice the default type is wrong (e.g., defaulting to "Other Current Asset" when the intent was "Expense"). The detail type is also often left at whatever QBO defaulted to, resulting in multiple accounts sharing the same detail type (e.g., all defaulting to "Advertising/Promotional").

**Classic example:** A $50,000 truck purchase coded to "Vehicle Expense" (an expense account) instead of a fixed asset account creates a false $50,000 single-period loss. This corrupts both the P&L and the Balance Sheet, and triggers panic-based business decisions.

**Another example:** If an item code on an invoice points to an account with type "Expense" instead of "Income," a negative balance appears on the revenue side of the P&L.

**The fix problem:** You can change an account type after posting — but only if the account is not a default/system account, and only if subaccounts don't exist under it. The error message "Cannot change the type of an account with subaccounts" blocks the correction path. The workaround is to change each subaccount type first, then the parent — but this is tedious with years of history.

Sources:
- https://outoftheboxtechnology.com/blog/chart-of-accounts-setup-mistakes
- https://www.lesterbahrcpa.com/quickbooks-online---fixing-common-setup-mistakes.html
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-get-this-error-message-when-trying-to-change-an-account-detail/00/194987
- https://quickbooks.intuit.com/learn-support/en-us/account-management/account-type-changes/00/187906

---

## 2. Duplicate Accounts

### Creation Vectors

Duplicates arise from multiple paths:
- Manual creation of an account that already exists under a slightly different name
- Re-importing a CSV chart of accounts that creates new entries alongside existing ones
- Bank reconnection events: reconnecting a bank feed can create a second account in the COA with a parenthetical suffix like "(1234)"
- Multiple users independently adding accounts for the same purpose

**Consequence:** Transactions get split across duplicates. Reports show fragmented data. Bank reconciliations become impossible when transactions from one physical account are distributed across two QBO accounts.

**Example from community:** One user reported QBO downloading approximately 200 transactions to the wrong (duplicate) account. Disconnecting and reconnecting the bank did not fix it.

### Merging Duplicates — The Permanent, Irreversible Fix

QBO's merge mechanism works by renaming the account you want to eliminate to match the name of the account you want to keep, then confirming the merge prompt. **This cannot be undone.**

**What the merge destroys:**
- Reconciliation reports for the merged account are permanently deleted. The reconciled status ("R") remains on individual transactions but the reconciliation report itself is gone.
- After merging reconciled bank accounts, QBO cannot maintain two reconciliation date histories. Users have reported needing to manually unreconcile and re-reconcile years of transactions after a merge.
- One community member described potentially needing to re-reconcile six years of transactions after a merge.

**Merge blockers:**
- Account is connected to online banking: "You can't delete this account because it is part of the Online Banking feature"
- Account is used by a product or service: "This account cannot be deleted because it is used by a product or service"
- Account has subaccounts (must re-home or delete subaccounts first)
- Account is used by a recurring template or scheduled liability payment
- Detail types of the two accounts don't match (merge will be rejected)

**Recommendation from community:** Export reconciliation reports as PDFs before merging. Several users expressed regret at not doing this.

Sources:
- https://www.cfosource.net/quickbooks-tips-common-problems-with-your-chart-of-accounts/
- https://quickbooks.intuit.com/learn-support/en-us/banking/i-had-to-merge-two-bank-accounts-but-then-lost-my-bank/00/755810
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/why-does-merger-of-accounts-to-eliminate-duplicate-give-me/00/1450958
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-am-having-trouble-merging-two-income-accounts-within-my-chart/00/811077
- https://quickbooks.intuit.com/learn-support/en-us/banking/while-reconnecting-the-bank-to-qbo-duplicate-bank-accounts-got/00/992126
- https://quickbooks.intuit.com/learn-support/en-us/banking/qb-downloaded-approx-200-transactions-to-the-wrong-account-how/00/217355

---

## 3. Account Renaming — Retroactive and Sometimes Destructive

Renaming an account in QBO is a retroactive change. All historical transactions associated with the account will reflect the new name. This is generally harmless for display purposes, but creates several real problems:

**Bank feed disruption:** After renaming a bank account, transactions may show up referencing the old account name (now non-existent), because QBO "remembered" the old name for auto-categorization rules. Some users report transactions defaulting to the old name in new entries.

**Report display lag:** Users have reported that renaming an account in the COA does not immediately update report display names. Reports can continue showing old names (e.g., "ADCB Athraa AED (Deleted)") even after the rename.

**Accidental merge-by-rename:** If you rename Account A to match Account B's name exactly, QBO triggers the merge prompt. Accepting it permanently merges them. This has caught users off guard — they intended only to rename, not merge.

**Default accounts cannot be renamed in the expected way:** Attempting to change the name of a system-designated default account may be silently rejected or cause unexpected behavior.

Sources:
- https://quickbooks.intuit.com/learn-support/en-us/other-questions/changed-a-name-to-an-account-on-the-chart-of-accounts-that-i/00/1389519
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/changing-a-bank-account-name-without-changing-the-bank-feeds/00/1039433

---

## 4. Accounts Receivable (A/R) — Custom Account Limitations

### The Default-Only Problem

QBO only uses **one A/R account** for all invoice and customer payment activity. You can create multiple accounts with type "Accounts Receivable," but bills and invoices will always post to the default. Community moderators have stated this explicitly: "your invoices will automatically be posted on the default account."

**Consequence:** Secondary A/R accounts accumulate no activity through normal invoice workflows. They can only receive entries via manual journal entries. The A/R Aging Report pulls data exclusively from the default A/R account — meaning a secondary A/R account will never appear in aging.

Community reaction: "If Bills can not be posted in the 2nd accounts payable then what's the use of 2nd payable account?" — this frustration is widespread.

### Journal Entry A/R Constraints

When posting a journal entry to an A/R account, QBO requires a **customer name** in the Name field. Using a vendor name on an A/R line causes an error. You cannot post to A/R without specifying a customer, and you cannot post to A/P without specifying a vendor. This constraint frequently trips up journal entry workflows for inter-company or non-standard transactions.

**The error:** "When you use A/P you must choose a vendor in the name field" — this blocks journal entries where the payee is simultaneously a customer and vendor.

**Workaround and its own problem:** Add the customer as a vendor (or vice versa) with a slightly different name. This creates list bloat and potential confusion.

### Multiple A/R Accounts in One Transaction

You cannot use more than one A/R account in a single transaction. You also cannot mix an A/R line with an A/P line in the same journal entry. This forces clearing account workarounds for intercompany transactions, which adds complexity and audit surface area.

### A/R Aging Mismatches

Common causes of A/R Aging not matching the Balance Sheet A/R figure:
- Date range configuration differs between the two reports
- Cash basis vs. accrual basis mismatch between reports
- Paid invoices appearing as unpaid because payment date falls outside the reporting window
- Statement charges used instead of invoices + payments, creating artificial A/R balances

Sources:
- https://quickbooks.intuit.com/learn-support/en-us/other-questions/multiple-accounts-payable-and-accounts-receivable-in-qbo/00/958028
- https://quickbooks.intuit.com/learn-support/en-us/help-article/accounts-receivable/warning-cannot-use-one-ar-ap-account-transaction/L7xqDWcmE_US_en_US
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/can-i-post-a-journal-entry-to-accounts-receivable-without/00/192409
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/when-i-do-the-journal-entry-it-tells-me-i-have-to-add-the-name/00/1161773
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/multiple-receivables-gl-accounts-can-i-create-multiple-ar-aging/00/1455776
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/why-would-my-a-r-aging-summary-report-not-match-my-balance-sheet/00/1254903

---

## 5. Accounts Payable (A/P) — Custom Account Limitations

The same default-only behavior applies to A/P. QBO uses only the initial default A/P account for all bill activity. Additional A/P accounts created by the user receive no automatic postings from bills. The A/P Aging Report reflects only the default A/P account.

**The result:** Having more than one A/P account makes the system disorganized, resulting in posting to the wrong A/P account. Intuit's own documentation recommends maintaining one A/P account.

**Vendor assignment errors:** Vendors can end up in the wrong A/P account if the COA has multiple A/P accounts. Fixing this requires manual journal entries through clearing accounts.

Sources:
- https://quickbooks.intuit.com/learn-support/en-us/other-questions/multiple-accounts-payable-and-accounts-receivable-in-qbo/00/958028
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/vendors-in-a-wrong-account-payable/00/223028

---

## 6. System-Protected ("Special") Accounts

QBO automatically creates a set of accounts that cannot be deleted, merged, or have their type changed. Attempting to modify them produces errors or silent failures. These accounts exist because QBO's internal transaction engine is hard-coded to use them.

### The Protected Account List

The following accounts are automatically created and have modification restrictions:

- **Accounts Receivable** — auto-created; used by all invoice workflows
- **Accounts Payable** — auto-created; used by all bill workflows
- **Undeposited Funds** — displays a lock icon; cannot be deleted. Holds customer payments until bank deposit is recorded. Misusing this account (bypassing it by recording deposits directly to income accounts) is a very common error that overstates income. One practitioner reported a client who overstated income by $850,000 this way, requiring an amended tax return.
- **Opening Balance Equity** — captures offsetting entries when opening balances are entered. Should be zero'd out to Retained Earnings or other equity accounts after setup. A persistent non-zero balance here indicates incomplete or incorrect opening balance entries.
- **Inventory Asset** — if edited or deactivated, QBO recreates it automatically for any product/service item
- **Cost of Goods Sold** — same auto-recreation behavior as Inventory Asset
- **Retained Earnings** — non-editable; QBO calculates this automatically by rolling prior-year net income. Users cannot make manual entries to this account.
- **Sales of Product Income** — default income account for inventory items. If renamed or deleted, QBO creates a new one. Renaming it causes QBO to create a fresh "Sales of Product Income" account, leaving the renamed version orphaned.
- **Services** — default income account; cannot be deleted
- **Payroll Liabilities** — system-managed if payroll is active
- **Unapplied Cash Payment Income** — auto-generated when a customer pays before an invoice is dated in the system. Indicates backdating problems.
- **Unapplied Cash Bill Payment Expense** — auto-generated when a bill is paid before it is entered. Indicates bill entry workflow problems.
- **Uncategorized Income / Uncategorized Expense** — auto-assigned to bank feed transactions QBO cannot categorize. Leaving balances here corrupts P&L and complicates tax prep.
- **Sales Tax Liability** — created by the QBO sales tax module; detail type cannot be changed

**Error message when trying to inactivate a banking-linked account:** "You can't delete this account because it is part of the Online Banking feature"

**Error message for product/service dependency:** "This account cannot be deleted because it is used by a product or service"

Sources:
- https://lendahandaccounting.com/2024/03/10/these-12-accounts-are-automatically-created-in-quickbooks-online/
- https://reconciledsolutions.net/mysteries-of-the-unknown-undeposited-funds-retained-earnings-unapplied-cash-payment-income-unapplied-cash-bill-payment-expense-in-quickbooks-online/
- https://quickbooks.intuit.com/learn-support/en-us/help-article/chart-accounts/manage-default-special-accounts-chart-accounts/L3WvLaIfa_US_en_US
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/trying-to-inactivate-an-account-but-it-is-giving-me-you-can-t/00/1103650
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/quot-this-account-cannot-be-deleted-because-it-is-used-by-a/00/1239925
- https://quickbooks.intuit.com/learn-support/en-us/taxes/how-do-i-correct-the-mapping-of-the-sales-tax-item-to-the/00/182713

---

## 7. Parent Account and Subaccount Problems

### Balance Mismatches

Posting transactions directly to a parent account does not update the subaccount balances. Conversely, postings to subaccounts roll up to the parent. This asymmetry causes persistent balance discrepancies that compound over time.

**The core rule:** "Posting to the sub-accounts will impact the parent account balance, but posting to the parent account will not impact the sub-accounts."

**Community example:** A parent bank account showed a balance $4,500 higher than the combined sub-account totals. The cause was transactions posted directly to the parent. The fix required using Edit > Find to locate parent-level transactions and reassign them to the correct subaccounts.

### Bank Feed Restrictions with Subaccounts

QBO does not allow simultaneous bank feed connections to both a parent account and its subaccounts. You must connect either the parent (if the bank downloads one combined feed) or the individual subaccounts (if the bank provides separate feeds per sub-account). Connecting both causes duplicate transaction downloads.

### Detail Type Change Blocked by Subaccounts

If you try to change the detail type of a parent account that has subaccounts, you receive: "Cannot change the type of an account with subaccounts." You must change each subaccount's type individually first, then change the parent.

### Credit Card Parent/Subaccount Reconciliation Problem

When credit card payments post only to the parent account while purchases post to subaccounts, reconciliation fails because subaccounts show no beginning or ending balance. This is a known structural problem with how QBO handles credit card hierarchies.

Sources:
- https://quickbooks.intuit.com/learn-support/en-us/banking/parent-bank-account-and-sub-accounts-the-balances-do-not-match/00/140229
- https://quickbooks.intuit.com/learn-support/en-us/banking/parent-account-preventing-linking-of-subaccounts/00/1076231
- https://quickbooks.intuit.com/learn-support/en-us/banking/cc-payments-only-show-up-on-parent-account-purchases-and-refunds/00/802705
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-get-this-error-message-when-trying-to-change-an-account-detail/00/194987

---

## 8. Tax Line Mapping and Tax Reporting with Non-Standard Accounts

### QBO Has No Tax Line Mapping

Tax line mapping (assigning each account to a specific IRS form line) is available only in QuickBooks Desktop, not QBO. The option simply does not exist in QBO's account settings.

**Implication for Form 1120 filers:** QBO cannot auto-populate tax form lines. All tax prep must be done externally by mapping the QBO trial balance to form lines manually, or by exporting to a tax software that accepts QBO data.

### Tax Form Setting Affects Account Mappings

QBO does have a "tax form" setting in Company Information (1120, 1120S, 1065, 1040 Schedule C, etc.). Changing this setting **unmaps all currently mapped accounts**, requiring every account to be re-mapped. Users who change entity type mid-year or fix an initial wrong selection lose all prior mappings.

### Non-Standard Account Names Do Not Affect QBO Function

Unlike Desktop's tax line feature, account names in QBO have no semantic connection to IRS form lines. Naming an account "Line 11 - Repairs" vs. "Repairs and Maintenance" has no functional difference within QBO. The consequence is that the burden of tax-form-to-COA mapping falls entirely on the preparer.

### 1099 Mapping Problems

For 1099-NEC/MISC reporting, QBO uses account-level mapping to determine which payments to include. If expense accounts are not properly mapped to 1099 categories, contractor payments get excluded from 1099 reports. This is a common first-year mistake when setting up contractor vendor relationships.

Sources:
- https://quickbooks.intuit.com/learn-support/en-us/taxes/tax-line-mapping/00/710571
- https://quickbooks.intuit.com/learn-support/en-us/other-questions/chart-of-accounts-tax-line/00/1180332
- https://quickbooks.intuit.com/learn-support/en-us/account-management/coa-entry-for-tax-line-mapping/00/1212062
- https://quickbooks.intuit.com/learn-support/en-us/account-management/company-information-income-tax-form-used-1120-vs-1120s/00/226050
- https://quickbooks.intuit.com/learn-support/en-us/taxes/1099-account-mapping/00/802535

---

## 9. Opening Balance Equity — The Setup Time Bomb

Opening Balance Equity (OBE) is QBO's mechanism for balancing the books when only one side of an opening transaction is entered. Every time you enter an opening balance for an asset or liability account without a matching equity entry, QBO credits or debits OBE to keep the balance sheet balanced.

**Why it becomes a problem:**
- OBE should reach zero after initial setup is complete. A non-zero balance at any point after setup means something is wrong.
- Users who enter "today's balance" instead of "the balance as of the start date" create OBE entries that don't correspond to any historical reality.
- When banks are first connected, QBO asks for a start date. The balance on that date posts with an OBE offset. If the user doesn't then enter the corresponding prior transactions, OBE remains.

**How to monitor:** Run the Balance Sheet as of the last day of each month and check OBE. Any non-zero amount is an error signal.

**How to fix:** Journal entry from OBE to the correct equity account (Retained Earnings, Owner's Equity, etc.). Must be done with care — the OBE balance reflects the net of all entry errors, not necessarily a single correctable item.

Sources:
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/opening-balance-equity-account-issue/00/1008128
- https://www.method.me/blog/what-is-opening-balance-equity-in-quickbooks/
- https://optimumresultsbusiness.com/how-to-fix-opening-balance-equity-in-quickbooks-online/
- https://synder.com/blog/what-is-opening-balance-equity-in-quickbooks-and-how-to-manage-it/

---

## 10. Chart of Accounts Size and Structure Problems

### The 250 Account Limit (Plus/Essentials/Simple Start)

QBO Plus and below enforces a hard limit of 250 total accounts including subaccounts. Active accounts count; inactive accounts do not. When the limit is reached, no new accounts can be created.

**Common path to hitting the limit:** Years of adding new accounts for each new vendor or expense type, plus the accounts QBO creates automatically (12 or more at setup), plus payroll accounts if payroll is active.

**Upgrade path:** QBO Advanced has no account limit. Upgrading to Advanced solely to escape the 250 limit is expensive.

**The account numbering complication:** Account numbers are optional but once enabled, they affect sort order and the behavior of account lookups. Restricted characters and maximum digit counts apply. Some users report confusion when account numbers conflict during import.

### Over-Structuring: Too Many Accounts

The most common COA mistake practitioners describe in DIY files is excessive account creation. Symptoms:
- A new account for every vendor instead of grouping (e.g., "AT&T," "Comcast," "Verizon" as separate accounts instead of "Telecommunications")
- 15+ page P&L statements that are unreadable
- Inconsistent categorization: the same type of expense lands in different accounts depending on who entered the transaction
- Trend analysis becomes impossible because the same expense is fragmented

### Under-Structuring: Too Few Accounts

The opposite problem — everything lumped into generic buckets — makes the COA useless for decision-making. A single "Income" account cannot tell you which revenue streams are profitable. A single "Expenses" account cannot support cost control.

### Flat vs. Hierarchical Structure

Construction industry practitioners warn specifically against subaccounts unless necessary — subaccounts lead to users posting to the parent instead of the correct sub, creating the imbalance problem described in Section 7. For general businesses, a flat COA is easier to maintain correctly.

Sources:
- https://www.portebrown.com/newsblog-archive/quickbooks-corner-keep-an-eye-on-your-quickbooks-online-usage-limits
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/new-limits-are-sub-accounts-counted-toward-the-limit-allowed-for/00/289825
- https://www.redhammer.io/blog/best-practices-for-configuring-a-chart-of-accounts-in-quickbooks-online-for-construction-companies
- https://outoftheboxtechnology.com/blog/chart-of-accounts-setup-mistakes
- https://www.accountingdepartment.com/blog/a-small-business-guide-to-building-out-a-chart-of-accounts-in-qbo

---

## 11. Negative Balance Problems

Negative balances on financial statements require investigation. Legitimate cases (accumulated depreciation contra-accounts) should be distinguished from errors:

**Error causes that produce negative balances:**
- Expense account misclassified as Income: the account appears on the revenue side of the P&L, and expenses coded to it drive it negative
- Item code on an invoice pointing to an expense account type: generates negative revenue
- Customer credits or overpayments exceeding outstanding invoices: negative A/R
- Disbursements coded to revenue accounts: negative revenue line
- Payroll corrections that undo categorized transactions: can leave COA balances in an inconsistent state

**The investigation rule:** Every negative balance except contra-accounts (Accumulated Depreciation, Allowance for Doubtful Accounts, Treasury Stock) should be investigated for root cause.

Sources:
- https://www.cfosource.net/quickbooks-tips-common-problems-with-your-chart-of-accounts/
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/negative-accounts-receivable/00/516166

---

## 12. CSV Import Problems

When importing a COA via CSV:
- Accounts with existing balances in QBO may carry over those balances to newly imported accounts, creating duplicates with double the balance
- Subaccount relationships require specific "Main account: subaccount" formatting in the Account Name column; incorrect formatting creates flat accounts instead of a hierarchy
- The import only supports: Account Name, Type, Detail Type (optional), Account Number (optional), Currency (optional) — no description, no notes, no opening balance
- If an imported account name exactly matches an existing account, QBO may silently skip the import or create a duplicate depending on version

Source:
- https://quickbooks.intuit.com/learn-support/en-us/help-article/import-export-data-files/import-chart-accounts-quickbooks-online/L9Res1eb1_US_en_US
- https://quickbooks.intuit.com/learn-support/global/getting-started/importing-chart-of-account-or-create-new-accounts-manually/00/1468553

---

## 13. Practical Rules Distilled from Community Experience

The following rules reflect consensus across the sources researched:

**Before setup:**
- Design the COA on paper first. Decide what reports you need, then work backward to determine what accounts are required to produce them.
- Keep it flat. Avoid subaccounts unless you have a concrete reporting need that requires them.
- One A/R account. One A/P account. QBO's architecture enforces this functionally even if it allows more.
- Never create accounts for individual vendors — use the vendor list for that level of detail.

**During setup:**
- Enter all opening balances from a single, consistent historical date. Do not use "today's balance."
- Verify Opening Balance Equity is zero before processing any live transactions.
- Enable account numbers from the beginning if you intend to use them. Retrofitting numbers later is disruptive.

**After setup:**
- If you discover an account has the wrong type, correct it before transactions accumulate. The correction process grows exponentially harder with transaction volume.
- Before merging any two accounts, export all reconciliation reports as PDFs. The merge is irreversible and reconciliation reports for the absorbed account are deleted.
- Before renaming an account, verify the new name does not exactly match another account — this triggers an irreversible merge.
- Monitor Uncategorized Income, Uncategorized Expense, and Opening Balance Equity on every Balance Sheet. Non-zero balances in these three accounts are reliable indicators of data entry problems.

**Ongoing:**
- Review the COA at least annually. Inactivate unused accounts to stay below the 250 limit.
- Do not create new accounts reactively mid-transaction. Plan first, create second.
- The account name becomes the line item label on every financial report. Name it for the audience of that report.

---

## Source Index

- https://www.cfosource.net/quickbooks-tips-common-problems-with-your-chart-of-accounts/
- https://outoftheboxtechnology.com/blog/chart-of-accounts-setup-mistakes
- https://www.lesterbahrcpa.com/quickbooks-online---fixing-common-setup-mistakes.html
- https://reconciledsolutions.net/mysteries-of-the-unknown-undeposited-funds-retained-earnings-unapplied-cash-payment-income-unapplied-cash-bill-payment-expense-in-quickbooks-online/
- https://lendahandaccounting.com/2024/03/10/these-12-accounts-are-automatically-created-in-quickbooks-online/
- https://5minutebookkeeping.com/uncategorized-income-and-expenses-in-quickbooks-online/
- https://www.accountingdepartment.com/blog/a-small-business-guide-to-building-out-a-chart-of-accounts-in-qbo
- https://www.redhammer.io/blog/best-practices-for-configuring-a-chart-of-accounts-in-quickbooks-online-for-construction-companies
- https://www.saasant.com/blog/ultimate-guide-quickbooks-chart-of-accounts/
- https://www.method.me/blog/what-is-opening-balance-equity-in-quickbooks/
- https://optimumresultsbusiness.com/how-to-fix-opening-balance-equity-in-quickbooks-online/
- https://synder.com/blog/what-is-opening-balance-equity-in-quickbooks-and-how-to-manage-it/
- https://www.portebrown.com/newsblog-archive/quickbooks-corner-keep-an-eye-on-your-quickbooks-online-usage-limits
- https://quickbooks.intuit.com/learn-support/en-us/other-questions/multiple-accounts-payable-and-accounts-receivable-in-qbo/00/958028
- https://quickbooks.intuit.com/learn-support/en-us/help-article/accounts-receivable/warning-cannot-use-one-ar-ap-account-transaction/L7xqDWcmE_US_en_US
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/accounts-receivable-is-all-messed-up/00/1087645
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/multiple-receivables-gl-accounts-can-i-create-multiple-ar-aging/00/1455776
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/can-i-post-a-journal-entry-to-accounts-receivable-without/00/192409
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/when-i-do-the-journal-entry-it-tells-me-i-have-to-add-the-name/00/1161773
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/vendors-in-a-wrong-account-payable/00/223028
- https://quickbooks.intuit.com/learn-support/en-us/banking/i-had-to-merge-two-bank-accounts-but-then-lost-my-bank/00/755810
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/why-does-merger-of-accounts-to-eliminate-duplicate-give-me/00/1450958
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-am-having-trouble-merging-two-income-accounts-within-my-chart/00/811077
- https://quickbooks.intuit.com/learn-support/en-us/banking/parent-bank-account-and-sub-accounts-the-balances-do-not-match/00/140229
- https://quickbooks.intuit.com/learn-support/en-us/banking/parent-account-preventing-linking-of-subaccounts/00/1076231
- https://quickbooks.intuit.com/learn-support/en-us/banking/cc-payments-only-show-up-on-parent-account-purchases-and-refunds/00/802705
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/i-get-this-error-message-when-trying-to-change-an-account-detail/00/194987
- https://quickbooks.intuit.com/learn-support/en-us/taxes/tax-line-mapping/00/710571
- https://quickbooks.intuit.com/learn-support/en-us/account-management/company-information-income-tax-form-used-1120-vs-1120s/00/226050
- https://quickbooks.intuit.com/learn-support/en-us/account-management/coa-entry-for-tax-line-mapping/00/1212062
- https://quickbooks.intuit.com/learn-support/en-us/taxes/1099-account-mapping/00/802535
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/opening-balance-equity-account-issue/00/1008128
- https://quickbooks.intuit.com/learn-support/en-us/help-article/chart-accounts/manage-default-special-accounts-chart-accounts/L3WvLaIfa_US_en_US
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/trying-to-inactivate-an-account-but-it-is-giving-me-you-can-t/00/1103650
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/quot-this-account-cannot-be-deleted-because-it-is-used-by-a/00/1239925
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/new-limits-are-sub-accounts-counted-toward-the-limit-allowed-for/00/289825
- https://quickbooks.intuit.com/learn-support/en-us/help-article/import-export-data-files/import-chart-accounts-quickbooks-online/L9Res1eb1_US_en_US
- https://quickbooks.intuit.com/learn-support/en-us/banking/while-reconnecting-the-bank-to-qbo-duplicate-bank-accounts-got/00/992126
- https://quickbooks.intuit.com/learn-support/en-us/banking/qb-downloaded-approx-200-transactions-to-the-wrong-account-how/00/217355
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/why-would-my-a-r-aging-summary-report-not-match-my-balance-sheet/00/1254903
- https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/negative-accounts-receivable/00/516166
