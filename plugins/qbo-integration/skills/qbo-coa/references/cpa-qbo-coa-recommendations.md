---
authority_level: tertiary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# CPA & Accountant Recommendations: QuickBooks Online Chart of Accounts

Research compiled from CPA blogs, accounting firm websites, and professional forums.

---

## 1. General Philosophy: Standard vs. Custom Accounts

### The consensus: Always customize, never rely on QBO defaults

- "While QuickBooks comes with a standard CoA, you can customize it based on a client's industry. If you're familiar with an industry, you can advise your client as to the best chart of accounts."
- "I really don't like the chart of accounts that comes with QuickBooks. It has too many accounts." — CPA blogger recommendation: keep P&L concise enough to fit on one page.
- QBO now applies a standardized COA to new companies based on industry and tax entity type selected during setup, but this is a starting point, not a final answer.
- "No two businesses are ever exactly alike, even within the same industry." Account names require tweaking to reflect actual business usage.
- Account titles become the descriptive line items on financial reports, so precise naming is essential for professional-looking statements.

---

## 2. Numbering Conventions

### Standard ranges used by CPAs and QBO ProAdvisors

The following ranges are the professional standard, aligned with GAAP and consistently recommended across CPA resources:

- **1000–1999** — Assets
  - 1000–1499: Current Assets (cash, receivables, prepaid expenses)
  - 1500–1999: Fixed Assets (equipment, vehicles, accumulated depreciation)
- **2000–2999** — Liabilities
  - 2000–2499: Current Liabilities (accounts payable, credit cards, taxes payable)
  - 2500–2999: Long-Term Liabilities (loans, mortgages)
- **3000–3999** — Equity
- **4000–4999** — Income / Revenue
- **5000–5999** — Cost of Goods Sold (COGS)
- **6000–6999** — Operating Expenses / Overhead
- **7000–7999** — Other Income
- **8000–8999** — Other Expense

Sources: CPA ProAdvisor blogs, accounting firm best-practice guides

### Enabling account numbers in QBO

Account numbering is **off by default** in QBO. CPAs consistently recommend enabling it:

- "Account numbers are a great way to organize accounts for order and readability, and to organize reports used outside of QuickBooks."
- "I prefer to have the account numbering turned on so that I can easily create a professional account listing."
- Include account numbers directly in COA templates for standardized formatting.

---

## 3. Consolidation vs. Granularity

### The dominant CPA recommendation: lean toward consolidation, use sub-accounts for detail

- "Less is best when it comes to your chart of accounts. Reduce the size by reviewing your accounts to identify those with small amounts relative to the size of your business. Unless the account is necessary for managerial reporting, delete the account and roll it into a larger account."
- "Only create accounts you genuinely need. Too many accounts can clutter your reports."
- Small businesses often do well with **50 to 100 accounts**. More than 200 accounts usually means it is time to review and consolidate.
- QBO standard editions have a **250-account limit**; QBO Advanced has no limit.
- "If you have multiple accounts tracking the same type of spend, for example an account called Rent & Utilities, another one called Rent and then another named Utilities, consider merging those accounts together."

### When granularity IS justified

- When operational reporting requires it (e.g., tracking labor by type, separating product lines)
- When a tax preparer specifically requests it for a particular line item
- "Ask your tax accountant what level of detail they need for payroll tracking purposes before you delete sub-accounts."
- "It's recommended to ask your tax accountant what level of detail they need" before removing sub-accounts.
- Starting with sufficient detail enables proper historical comparisons — it is harder to split a consolidated account later than to collapse sub-accounts.

### Parent/child structure: best of both worlds

- "Use the IRS accounts as what is called a Parent Account and to put the detail accounts in as Child (or Sub) Accounts. This will allow you to click one button and collapse the detail accounts into the parent accounts that are needed on your tax returns."
- "Keeping the chart of accounts nice and tidy while providing the ability to track granular details that clients need to manage their business, with client-created sub accounts that can be collapsed into the parent accounts on reports."
- **Critical rule for sub-accounts and tax mapping**: only assign tax lines to sub-accounts, never to parent accounts, to prevent double-counting on tax filings. [QuickBooks Community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/chart-of-accounts/00/1224055)

---

## 4. C-Corporation Specific Setup (Form 1120)

### Equity accounts required for a C-corp

Every C-corporation in QBO should have the following equity accounts:

- **Common Stock** — par value of issued shares; Account Type: Equity, Detail Type: Common Stock
- **Preferred Stock** — if applicable; Detail Type: Preferred Stock
- **Additional Paid-in Capital** (or Paid-in Capital) — amounts paid above par value; Detail Type: Additional Paid-in Capital
- **Retained Earnings** — automatically created by QBO; Detail Type: Retained Earnings; do not delete
- **Dividends Paid** — distributions to shareholders; Detail Type: Partner Distributions or Owner's Draw (rename to "Dividends Paid")
- **Treasury Stock** — if the company has repurchased shares

Sources: QuickBooks Community forums, accounting software guides

### Corporate income tax accounts

QBO does not have a dedicated detail type for federal/state income taxes. Standard professional approach:

- **Federal Income Tax Payable** — Account Type: Other Current Liabilities; tracks estimated tax payments and year-end balance
- **Federal Income Tax Expense** — Account Type: Other Expense, Detail Type: Other Miscellaneous Expense (rename clearly)
- **State Income Tax Payable** — Account Type: Other Current Liabilities
- **State Income Tax Expense** — Account Type: Other Expense

Note: Corporate income taxes are NOT a deductible business expense for tax purposes and will appear as an M-1 adjustment on the return, but they still need a home in the books.

### Loans to/from shareholders and officers

- **Loan from Shareholder/Officer** — Account Type: Other Current Liabilities (if < 1 year) or Long-Term Liabilities (if > 1 year)
- **Loan to Shareholder/Officer** — Account Type: Other Current Assets or Long-Term Assets (a receivable, not an expense)

Source: QuickBooks Community forums

### Form 1120 income and expense line mapping

The following expense categories map directly to Form 1120 lines and should exist as distinct accounts or parent accounts:

| Form 1120 Line | Account to Create |
|---|---|
| Line 1 | Sales / Gross Receipts |
| Line 2 | Returns and Allowances |
| Line 4 | Cost of Goods Sold |
| Line 8 | Dividends Received |
| Line 9 | Interest Income |
| Line 10 | Gross Rents Received |
| Line 12 | Compensation of Officers |
| Line 13 | Salaries and Wages |
| Line 14 | Repairs and Maintenance |
| Line 15 | Bad Debts |
| Line 16 | Rents (paid) |
| Line 17 | Taxes and Licenses |
| Line 18 | Interest Expense |
| Line 19 | Charitable Contributions |
| Line 20 | Depreciation / Amortization |
| Line 26 | Other Deductions (advertising, legal, etc.) |

Sources: IRS Form 1120, CPA tax preparation guides

---

## 5. QBO Prep for Taxes Feature and Tax Mapping

### What Prep for Taxes does (available in QBO Accountant)

- Automatically maps most QBO account balances to lines on Form 1120 (and 1065, 990, 1040)
- Unmapped accounts are surfaced at the top for manual assignment
- Allows switching between cash and accrual basis for review without affecting client settings
- Can export directly to Intuit ProConnect Tax Online, or as CSV for ProSeries/Lacerte

Sources: Intuit Prep for Taxes documentation, QBO Accountant help articles

### Important limitation

**QBO does not have tax line mapping in the same way QuickBooks Desktop does.** The "Tax Line" field visible in QB Desktop's chart of accounts is not available in QBO's standard COA interface. Prep for Taxes (in QBO Accountant only) is the replacement mechanism.

### Practical workflow CPAs use

1. Set company tax form to 1120 in Company Settings
2. Build COA with accounts named to align with 1120 line items (see table above)
3. Use Prep for Taxes annually to verify auto-mapping and fix unmapped accounts
4. Run the Income Tax Summary report (Reports > Accountant & Taxes) to verify all mappings before handing off to tax software

Sources: CPA tax preparation workflow guides

---

## 6. Account Naming Best Practices

- Rename generic accounts to match actual use: "Cash" → "Business Checking ×4521", "Loan Payable" → "Bank Loan - Equipment"
- Use actual bank names and last four digits for bank and credit card accounts — makes reconciliation faster
- Use straightforward titles like "Bank Fees" or "Office Equipment" rather than codes or abbreviations
- "Account names should be broad categories to encompass many similar transactions" for P&L line items
- Keep a consistent format across all accounts (e.g., always "Expense - [Type]" or just plain noun phrases — pick one and stick to it)

---

## 7. Common COA Mistakes CPAs Fix in Client Files

From professional blog posts and CPA practice experience:

1. **Wrong account type** — Balance sheet accounts appearing on P&L or vice versa. "Probably one of the most common mistakes." Fix by editing the account type.
2. **Default detail types left unchanged** — Multiple accounts with identical detail type classifications regardless of purpose.
3. **Posting to parent accounts instead of sub-accounts** — Defeats the purpose of the hierarchy; causes double-counting on rolled-up reports.
4. **Duplicate accounts** — Two accounts for the same thing under slightly different names. Creates split reporting and confuses tax prep.
5. **Too many granular accounts** — "Miscellaneous" proliferation, separate accounts for every vendor, etc. Roll into broader categories.
6. **Using Opening Balance Equity incorrectly** — Should be cleared to zero after initial setup by reallocating to proper equity accounts.
7. **Not enabling account numbers** — Makes reports harder to sort and review outside QBO.
8. **Failing to mark accounts inactive** — Old unused accounts inflate the list and can confuse staff.

---

## 8. Classes vs. Sub-Accounts: When CPAs Use Each

A common mistake is creating sub-accounts where Classes would be more appropriate. CPAs follow this rule:

- **Accounts** answer "what type of transaction" (what)
- **Classes** answer "which department/project/location" (what for)

> "If you buy Office Supplies for multiple departments and want to track each department's use, the 'what' is office supplies; the 'what for' is the various departments — so office supplies should be an account and each department should be set up as a class."

- Classes are available in QBO Plus and Advanced (not Simple Start or Essentials)
- Using Classes instead of sub-accounts keeps the COA shorter while adding dimensional analysis
- Classes add "columns" to the income statement without proliferating accounts

Sources: CPA practice guides on QBO Classes vs. sub-accounts

---

## 9. ProAdvisor COA Templates (Intuit's Accountant Feature)

Launched November 2023 for QBO Accountant users:

- Templates can be created from scratch, customized from pre-made service/product templates, or imported via CSV/Excel
- Templates are firm-wide — any team member can apply them to new or existing client files
- **Applying a template to an existing file does not change existing accounts** — it only adds new ones. Duplicates get a "-1" suffix.
- Editing a template after deployment does NOT retroactively update clients already set up with it
- Available for 32 industry types as a starting point
- Standardized templates reduce cleanup time and make COA-to-tax-project mapping faster in ProConnect

Sources: Intuit ProAdvisor documentation, CPA firm template guides

---

## 10. Ongoing COA Maintenance Recommendations

- **Review quarterly or annually** — run a P&L with 2-3 year date range to see which accounts have activity; deactivate zero-activity accounts
- **Never delete accounts with transaction history** — mark inactive instead
- **Reconcile before deactivating** any bank, credit card, or liability account
- **Lock prior periods** after year-end close to prevent accidental edits to historical data
- **Create an "Ask My Accountant" account** — expense account, detail type "Other Business Expenses" — as a holding pen for uncertain transactions; review with accountant at check-ins
- **Consistency over time is paramount** — resist the urge to restructure mid-year; changes make year-over-year comparisons harder

---

## 11. Practitioner COA Philosophy — Key Positions

Curated positions from experienced QuickBooks ProAdvisors and CPA practitioners:

**On industry-specific templates:**
- Industry-specific COA kits with customized setups by entity type and industry are available as ProAdvisor resources
- Downloadable sample charts of accounts exist for financial services and other verticals
- Standard ranges: 1000 Assets, 2000 Liabilities, 3000 Equity, 4000 Income, 5000 COGS, 6000 Expenses, 7000 Other Income, 8000 Other Expense

**On common mistakes:**
- Most common client mistake: wrong account type assignment
- "The chart of accounts serves as your core foundation in your company accounting system" — errors cascade through all reports
- Enable account numbering; verify opening balances when converting

**On tax alignment:**
- "Having a properly set up chart of accounts will save you time and money when it comes to tax season"
- Use IRS tax form line items as parent accounts, detailed tracking as child accounts
- Passing a properly structured P&L to your tax preparer "saves significant professional fees"

**On account hygiene:**
- Enable account numbers for readability; conciseness is better for reporting
- Avoid overly specific account creation — complicates reporting
- Rename accounts using actual bank names + last 4 digits

**On simplicity:**
- "I really don't like the chart of accounts that comes with QuickBooks. It has too many accounts."
- P&L should fit on one page when practical
- Equipment purchases: use de minimis safe harbor ($2,500 threshold) to expense rather than capitalize

---

## 12. Default / Protected Accounts in QBO

These accounts are created automatically and **cannot be deleted or renamed** in QBO:

- Undeposited Funds
- Opening Balance Equity
- Retained Earnings
- Accounts Receivable (A/R)
- Accounts Payable (A/P)

CPAs note: "Opening Balance Equity" should be cleared to zero after initial setup by moving balances to proper equity accounts. Leaving a balance here indicates incomplete setup.

---

## Source Index

Content synthesized from Intuit official documentation, QuickBooks Community forums, IRS publications, AICPA guidance, and CPA ProAdvisor blog posts on QBO chart of accounts best practices.
