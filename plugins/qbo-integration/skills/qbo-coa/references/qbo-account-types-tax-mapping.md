---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QBO Account Types and Tax Mapping for C-Corps (Form 1120)

**Research date:** 2026-02-24
**Scope:** How QuickBooks Online account types and detail types affect Form 1120 tax reporting

---

## 1. Overview: Two Separate Systems

QBO uses two distinct mechanisms that relate to tax reporting. Conflating them causes confusion:

1. **Account Type + Detail Type** — controls where a transaction appears on the P&L and balance sheet *inside QBO*. This is a bookkeeping classification that determines report structure and financial statement presentation.

2. **Tax Line Mapping** (QB Desktop only) / **Prep for Taxes** (QBO via QBOA) — explicitly links chart of accounts entries to specific tax form lines for export to ProConnect Tax, TurboTax Business, Lacerte, or ProSeries.

Key insight from Intuit community: *"The option to set up tax lines in QBO is unavailable — setting up tax-line mapping is only available in QuickBooks Desktop."* For QBO, tax software integration is handled via the **Prep for Taxes** feature in QuickBooks Online Accountant (QBOA), which auto-maps accounts to tax lines and exports to ProConnect.

Sources:
- [Tax-Line Mapping thread](https://quickbooks.intuit.com/learn-support/en-us/taxes/tax-line-mapping/00/710571)
- [Company Information: 1120 vs 1120S](https://quickbooks.intuit.com/learn-support/en-us/account-management/company-information-income-tax-form-used-1120-vs-1120s/00/226050)
- [Prep for Taxes — Firm of the Future](https://www.firmofthefuture.com/accounting/quickbooks-can-do-what-prep-for-taxes/)

---

## 2. QBO Account Types: Complete List (15 Types)

QBO has exactly **15 account types** and approximately **154 detail types** (also called "subtypes" in the API, or "Tax form section" in some UI contexts). Detail types are fixed — they cannot be created, edited, or deleted by users.

### Balance Sheet Account Types

| Account Type | Classification | Where It Appears |
|---|---|---|
| Bank | Asset | Balance Sheet — Current Assets |
| Accounts Receivable (A/R) | Asset | Balance Sheet — Current Assets |
| Other Current Assets | Asset | Balance Sheet — Current Assets |
| Fixed Assets | Asset | Balance Sheet — Long-Term Assets |
| Other Assets | Asset | Balance Sheet — Long-Term Assets |
| Accounts Payable (A/P) | Liability | Balance Sheet — Current Liabilities |
| Credit Card | Liability | Balance Sheet — Current Liabilities |
| Other Current Liabilities | Liability | Balance Sheet — Current Liabilities |
| Long Term Liabilities | Liability | Balance Sheet — Long-Term Liabilities |
| Equity | Equity | Balance Sheet — Equity |

### Profit & Loss Account Types

| Account Type | Classification | Where It Appears |
|---|---|---|
| Income | Revenue | P&L — Income section (above gross profit) |
| Cost of Goods Sold | Expense | P&L — COGS section (reduces gross profit) |
| Expenses | Expense | P&L — Operating Expenses section |
| Other Income | Revenue | P&L — Other Income section (below operating income) |
| Other Expense | Expense | P&L — Other Expenses section (below operating income) |

Sources:
- [Complete list community thread](https://quickbooks.intuit.com/learn-support/en-us/banking/how-many-types-of-accounts-does-quickbooks-have-how-many-detail/00/790394)
- [QBO Account Types PDF](https://quickbooks.intuit.com/learn-support/s/ooxbu36397/attachments/ooxbu36397/reports-and-accounting/157399/1/QBO%20full%20COA%20list.pdf)
- Standard Chart of Accounts — CPA ProAdvisor reference

---

## 3. Detail Types (AccountSubType) — Complete Enumeration

The QBO API exposes these as `AccountSubType`. There are **125 values** in the SDK enumeration (some UI versions show 154, with regional and product-tier differences).

### Bank Account Detail Types
- Cash on Hand
- Checking
- Money Market
- Savings
- Rents Held in Trust
- Trust Account

### Accounts Receivable Detail Types
- Accounts Receivable

### Other Current Assets Detail Types
- Allowance for Bad Debts
- Development Costs
- Employee Cash Advances
- Inventory
- Investment — Mortgage/Real Estate Loans
- Investment — Tax-Exempt Securities
- Investment — U.S. Government Obligations
- Investments — Other
- Loans to Officers
- Loans to Others
- Loans to Stockholders
- Other Current Assets
- Prepaid Expenses
- Retainage
- Undeposited Funds

### Fixed Assets Detail Types
- Accumulated Depletion
- Accumulated Depreciation
- Buildings
- Depletable Assets
- Fixed Asset Computers
- Fixed Asset Copiers
- Fixed Asset Furniture
- Fixed Asset Other Tools/Equipment
- Fixed Asset Phone/Communications
- Fixed Asset Software
- Furniture and Fixtures
- Land
- Leasehold Improvements
- Machinery and Equipment
- Other Fixed Assets
- Vehicles

### Other Assets Detail Types
- Accumulated Amortization of Other Assets
- Goodwill
- Intangible Assets
- Lease Buyout
- Organizational Costs
- Other Long-Term Assets
- Security Deposits

### Accounts Payable Detail Types
- Accounts Payable

### Credit Card Detail Types
- Credit Card

### Other Current Liabilities Detail Types
- Direct Deposit Payable
- Federal Income Tax Payable
- Global Tax Payable
- Global Tax Suspense
- Insurance Payable
- Line of Credit
- Loan Payable
- Other Current Liabilities
- Payroll Clearing
- Payroll Tax Payable
- Prepaid Expenses Payable
- Rents in Trust — Liability
- Sales Tax Payable
- State/Local Income Tax Payable
- Trust Accounts — Liabilities
- Unapplied Cash Bill Payment Expense

### Long Term Liabilities Detail Types
- Notes Payable
- Other Long-Term Liabilities
- Shareholder Notes Payable

### Equity Detail Types
- Accumulated Adjustment (S-Corp only)
- Common Stock (C-Corp)
- Opening Balance Equity
- Owner's Equity
- Paid-in Capital or Surplus (C-Corp)
- Partner Contributions (Partnership)
- Partner Distributions (Partnership)
- Partners' Equity (Partnership)
- Preferred Stock (C-Corp)
- Retained Earnings
- Treasury Stock (C-Corp)

### Income Detail Types
- Discounts/Refunds Given
- Non-Profit Income
- Other Primary Income
- Sales of Product Income
- Service/Fee Income
- Unapplied Cash Payment Income

### Cost of Goods Sold Detail Types
- Cost of Labor — COS
- Equipment Rental — COS
- Other Costs of Services — COS
- Shipping, Freight & Delivery — COS
- Supplies & Materials — COGS

### Expenses Detail Types
- Advertising/Promotional
- Auto
- Bad Debts
- Bank Charges
- Charitable Contributions
- Commissions & Fees
- Cost of Labor
- Dues & Subscriptions
- Entertainment
- Entertainment Meals
- Equipment Rental
- Finance Costs
- Global Tax Expense
- Insurance
- Interest Paid
- Legal & Professional Fees
- Licenses
- Office/General Administrative Expenses
- Other Miscellaneous Expense
- Other Miscellaneous Service Cost
- Payroll Expenses
- Penalties & Settlements
- Promotional Meals
- Rent or Lease of Buildings
- Repair & Maintenance
- Shipping, Freight & Delivery
- Supplies & Materials
- Taxes Paid
- Travel
- Travel Meals
- Unapplied Cash Bill Payment Expense
- Utilities
- Vehicles

### Other Income Detail Types
- Dividend Income
- Exchange Gain or Loss
- Interest Earned
- Other Investment Income
- Other Miscellaneous Income
- Tax-Exempt Interest
- Unapplied Cash Payment Income

### Other Expense Detail Types
- Amortization
- Depreciation
- Exchange Gain or Loss
- Other Miscellaneous Expense
- Penalties & Settlements

Sources:
- [AccountSubTypeEnum — Intuit SDK docs](https://static.developer.intuit.com/sdkdocs/qbv3doc/ippdotnetdevkitv3/html/50796272-b70e-0c9b-77d9-00c18758396c.htm)
- [QBO Account Type API reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)
- [Default Account Types & Detail Types PDF](https://quickbooks.intuit.com/learn-support/s/ooxbu36397/attachments/ooxbu36397/reports-and-accounting/226910/1/QuickBooks%20Default%20Account%20Types%20&%20Detail%20Types.pdf)

---

## 4. How QBO Maps to Form 1120

### 4a. The Company Tax Form Setting

In QBO: **Gear → Account and Settings → Advanced → Accounting → Income tax form used**

Setting this to "Form 1120 (C Corporation)" does two things:
1. Tells QBO which predefined chart of accounts to suggest at setup (equity accounts like Common Stock, Preferred Stock, Treasury Stock appear for C-Corps)
2. Determines which tax form the "Prep for Taxes" feature targets when auto-mapping accounts to tax lines

**Critical limitation:** Changing this setting *after initial setup* unmaps any previously assigned tax line associations. Your transaction data is not affected, but all tax line linkages are cleared.

Source: [1120 vs 1120S thread](https://quickbooks.intuit.com/learn-support/en-us/account-management/company-information-income-tax-form-used-1120-vs-1120s/00/226050)

### 4b. Prep for Taxes (QBO → ProConnect)

The "Prep for Taxes" feature (available in QuickBooks Online Accountant) automatically assigns accounts to tax form lines and exports directly to ProConnect Tax Online. Workflow:

1. Open QBOA → client's QBO file
2. Open Prep for Taxes dashboard
3. Review auto-mapped assignments (income, expense, balance sheet)
4. Manually correct any unmapped or incorrectly mapped accounts
5. Export to ProConnect (creates new return or updates existing)

The system auto-maps based on the account's **account type** and **detail type**. Accounts with unusual or ambiguous detail types may require manual assignment.

Source: [Prep for Taxes documentation](https://accountants.intuit.com/support/en-us/help-article/manage-integrations/prep-taxes-quickbooks-accountant-proconnect-tax/L7w6k3VxH_US_en_US)
Source: [Use Prep for Taxes](https://quickbooks.intuit.com/learn-support/en-us/help-article/map-forms-accounts/use-prep-taxes-map-export-clients-tax-info/L4EUJdqX3_US_en_US)

### 4c. Manage Account Mapping to Tax Categories

QBO also provides a direct "Manage tax categories" interface for corporations. This allows per-account assignment to tax form boxes without leaving QBO.

Source: [Manage account mapping to tax categories](https://quickbooks.intuit.com/learn-support/en-us/help-article/corporation/manage-account-mapping-tax-categories-tax-forms/L54GSV2pl_US_en_US)

---

## 5. Form 1120 Line-by-Line QBO Account Mapping

The following table shows how QBO account categories flow to Form 1120 lines, based on the QuickBooks Tax-Line Mapping table used for Desktop/TurboTax Business imports. The same logic applies to Prep for Taxes auto-mapping in QBO.

### Page 1: Income (Lines 1–11)

| Form 1120 Line | Description | QBO Account Type | Recommended Detail Type |
|---|---|---|---|
| Line 1a | Gross receipts or sales | Income | Sales of Product Income, Service/Fee Income, Other Primary Income |
| Line 1b | Returns and allowances | Income | Discounts/Refunds Given |
| Line 4 | Dividends and inclusions | Other Income | Dividend Income |
| Line 5 | Interest | Other Income | Interest Earned, Tax-Exempt Interest |
| Line 6 | Gross rents | Income or Other Income | Other Primary Income |
| Line 7 | Gross royalties | Income or Other Income | Other Primary Income |
| Line 8 | Capital gain net income | Other Income | Other Miscellaneous Income |
| Line 10 | Other income | Other Income | Other Miscellaneous Income |

### Schedule A: Cost of Goods Sold (Lines 2/COGS)

| Form 1120 Reference | Description | QBO Account Type | Recommended Detail Type |
|---|---|---|---|
| COGS — Purchases | Inventory purchases | Cost of Goods Sold | Supplies & Materials — COGS |
| COGS — Labor | Direct labor costs | Cost of Goods Sold | Cost of Labor — COS |
| COGS — Shipping | Freight/delivery | Cost of Goods Sold | Shipping, Freight & Delivery — COS |
| COGS — Other | Other direct costs | Cost of Goods Sold | Other Costs of Services — COS |

### Page 1: Deductions (Lines 12–29)

| Form 1120 Line | Description | QBO Account Type | Recommended Detail Type |
|---|---|---|---|
| Line 12 | Compensation of officers | Expenses | Payroll Expenses |
| Line 13 | Salaries and wages | Expenses | Payroll Expenses |
| Line 14 | Repairs and maintenance | Expenses | Repair & Maintenance |
| Line 15 | Bad debts | Expenses | Bad Debts |
| Line 16 | Rents | Expenses | Rent or Lease of Buildings |
| Line 17 | Taxes and licenses | Expenses | Taxes Paid, Licenses |
| Line 18 | Interest | Expenses | Interest Paid, Finance Costs |
| Line 19 | Charitable contributions | Expenses | Charitable Contributions |
| Line 20 | Depreciation (Form 4562) | Other Expense | Depreciation |
| Line 21 | Depletion | Other Expense | Amortization |
| Line 22 | Advertising | Expenses | Advertising/Promotional |
| Line 23 | Pension, profit-sharing | Expenses | Payroll Expenses |
| Line 24 | Employee benefit programs | Expenses | Insurance (or Payroll Expenses) |
| Line 26 | Other deductions | Expenses | Various |

### Schedule L: Balance Sheet

| Schedule L Line | Description | QBO Account Type | Recommended Detail Type |
|---|---|---|---|
| Line 1 | Cash | Bank | Checking, Savings, Cash on Hand |
| Line 2a | Accounts receivable | Accounts Receivable (A/R) | Accounts Receivable |
| Line 4 | U.S. govt obligations | Other Current Assets | Investment — U.S. Government Obligations |
| Line 5 | Tax-exempt securities | Other Current Assets | Investment — Tax-Exempt Securities |
| Line 6 | Loans to stockholders | Other Current Assets | Loans to Stockholders |
| Line 7 | Mortgage/real estate loans | Other Current Assets | Investment — Mortgage/Real Estate Loans |
| Line 8a | Depreciable assets | Fixed Assets | Buildings, Machinery & Equipment, Vehicles, etc. |
| Line 8b | Accumulated depreciation | Fixed Assets | Accumulated Depreciation |
| Line 9a | Depletable assets | Fixed Assets | Depletable Assets |
| Line 9b | Accumulated depletion | Fixed Assets | Accumulated Depletion |
| Line 10 | Land | Fixed Assets | Land |
| Line 11a | Intangible assets | Other Assets | Intangible Assets, Goodwill |
| Line 11b | Accumulated amortization | Other Assets | Accumulated Amortization of Other Assets |
| Line 12 | Other assets | Other Current Assets / Other Assets | Various |
| Line 14 | Accounts payable | Accounts Payable (A/P) | Accounts Payable |
| Line 15 | Short-term notes payable | Other Current Liabilities | Loan Payable, Line of Credit |
| Line 17 | Loans from stockholders | Long Term Liabilities | Shareholder Notes Payable |
| Line 18 | Long-term mortgage/notes | Long Term Liabilities | Notes Payable |
| Line 19 | Other liabilities | Other Current Liabilities / Long Term Liabilities | Other Current Liabilities |
| Line 21a | Preferred stock | Equity | Preferred Stock |
| Line 21b | Common stock | Equity | Common Stock |
| Line 22 | Paid-in capital surplus | Equity | Paid-in Capital or Surplus |
| Line 24 | Retained earnings | Equity | Retained Earnings |
| Line 28 | Treasury stock | Equity | Treasury Stock |

Source: [QuickBooks Tax-Line Mapping Table — ERO Support](https://kb.erosupport.com/print-12.html)
Source: [IRS Form 1120 Instructions (2025)](https://www.irs.gov/instructions/i1120)

---

## 6. What Happens with Wrong Account Types

### 6a. P&L Structure Corruption

The account type directly controls which section of the P&L report the account appears in. Wrong type = wrong section:

- **Expense classified as Income** — inflates gross revenue, understates costs; dramatically misrepresents profitability
- **Income classified as Other Income** — moves revenue below the operating income line; reduces reported gross profit and operating income; may look like non-operating revenue to tax software
- **COGS classified as Expenses** — does not affect taxable income directly, but breaks the gross profit calculation on Form 1120 Schedule A; COGS items should reduce gross receipts before deductions
- **Expenses classified as COGS** — same P&L misrepresentation in the other direction; gross profit appears lower than actual; deduction amounts on Form 1120 will be misallocated across lines

Source: QuickBooks Community forums, CPA categorization guides

### 6b. Balance Sheet Misclassification

- **Long-term liability classified as Other Current Liabilities** — makes the company appear less solvent (higher current ratio impact); misrepresents working capital; Schedule L on Form 1120 will show wrong line totals
- **Fixed asset classified as Other Current Assets** — asset will not depreciate properly; depreciation deduction on Form 1120 Line 20 will be incorrect; balance sheet presentation wrong
- **Asset expensed instead of capitalized** — immediate over-deduction in one period; missing depreciation deductions in future years; potential Schedule M-1 reconciliation issues

Source: CPA categorization guides, QuickBooks error correction resources

### 6c. Tax Export Failures

When using Prep for Taxes or TurboTax import:

- Accounts with wrong types will auto-map to incorrect tax lines
- Unmapped accounts appear in a flagged list requiring manual reassignment
- If tax form setting is changed after accounts are mapped, **all existing tax line assignments are cleared** and must be redone
- TurboTax Business "may question some tax line assignments if they conflict with normal mapping, or commonly have multiple mapping possibilities"

Source: [Importing QB data into TurboTax — Insightful Accountant](https://insightfulaccountant.com/tax-and-practice-management/quickbooks-at-year-end:-importing-quickbooks-data-into-turbo/)

### 6d. IRS Audit Risk

The IRS may view persistent account misclassifications (especially inventory/COGS inconsistencies) as:
- Signs of unreported income
- Inflated deductions
- Material misstatement of taxable income

Source: [Will the IRS Look Through My QuickBooks?](https://www.taxandaccounting.com/post/will-the-irs-look-through-my-quickbooks)
Source: [QuickBooks Financial Reports Wrong — Verotus](https://www.verotusllp.com/2026/02/why-your-quickbooks-financial-reports.html)

---

## 7. C-Corp Specific Account Considerations

### 7a. Equity Structure

C-Corps require specific equity account types not available (or named differently) for other entity types:

| Account Name | Detail Type | Notes |
|---|---|---|
| Common Stock | Common Stock | Par value of issued shares |
| Preferred Stock | Preferred Stock | Par value of preferred shares |
| Additional Paid-in Capital | Paid-in Capital or Surplus | Capital above par value |
| Retained Earnings | Retained Earnings | System-managed; auto-updated by net income |
| Treasury Stock | Treasury Stock | Recorded as negative equity (contra-equity) |
| Opening Balance Equity | Opening Balance Equity | Temporary; should be zeroed out after setup |

These map to Schedule L, Lines 21–28 on Form 1120.

Source: [Stock account thread](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/stock-preferred-common-treasury-entry/00/1121023)
Source: QBO Categories — CPA ProAdvisor reference

### 7b. Federal Income Tax Payments

This is one of the most commonly miscategorized items for C-Corps. The correct treatment:

**C-Corp corporate income tax is NOT a deductible business expense on Form 1120.** It should NOT be placed in the Expenses section as "Taxes Paid."

Correct approach (two-step):
1. Record estimated tax payments as a reduction of the tax liability account (Other Current Liabilities — Federal Income Tax Payable), not as an expense
2. Record the income tax expense as "Other Expense" — this appears below the operating income line on the P&L, consistent with GAAP treatment for income taxes on corporate financial statements

If recorded as a regular Expense, it will incorrectly inflate deductions when tax data is exported to ProConnect/TurboTax and may cause Schedule M-1 reconciliation errors.

Source: [Corporate Federal Income Tax Deposits](https://quickbooks.intuit.com/learn-support/en-us/other-questions/corporate-federal-income-tax-deposits/00/1066560)
Source: [Federal tax payments thread](https://quickbooks.intuit.com/learn-support/en-us/employees-and-payroll/federal-tax-payments-what-account-do-these-go-under/00/201125)
Source: [C-Corp income tax expense — JustAnswer](https://www.justanswer.com/tax/f8vz8-paid-income-tax-c-corp-business-account.html)

### 7c. Income vs. Other Income

QBO separates these into distinct P&L sections:

| Type | Appears On | Form 1120 Destination |
|---|---|---|
| Income | P&L — main income section | Lines 1–3 (gross receipts, dividends, interest as primary income) |
| Other Income | P&L — below operating income | Lines 4–10 (rents, royalties, capital gains, other income) |

While tax preparers "worth their salt" can work with either classification, using the wrong type:
- Misrepresents gross profit and operating income
- May cause Prep for Taxes to auto-map to the wrong Form 1120 line
- Complicates Schedule M-1 book-to-tax reconciliation

Source: [Confused with Other Income thread](https://quickbooks.intuit.com/learn-support/en-us/taxes/confused-with-other-income/00/1531458)

### 7d. COGS vs. Expense — Tax Significance for Corporations

For C-Corps with inventory-based revenue, COGS has distinct tax significance:

- **COGS** flows to Form 1120 Schedule A (Form 1125-A for larger corporations), then the net result appears on Line 2 of the main form
- **Expenses** flow to Lines 12–26 of Form 1120 as specific deduction line items
- COGS reduces *gross profit* before operating deductions; regular expenses reduce *taxable income* after gross profit is established

Only manufacturing, merchandising, or mining businesses should report direct costs as COGS. Service companies should use Expenses. Misclassification distorts gross margin, affects Section 263A UNICAP calculations if applicable, and creates COGS/deduction allocation errors on Form 1120.

Source: [COGS vs. Expenses — TurboTax community](https://ttlc.intuit.com/community/tax-credits-deductions/discussion/expenses-vs-cost-of-goods-sold/00/787592)
Source: [Including COGS on business tax forms — The Balance Money](https://www.thebalancemoney.com/including-cost-of-goods-sold-on-business-tax-forms-397851)

---

## 8. Detail Type Does (and Does Not) Affect Financials

There is conflicting official guidance on whether detail types have direct financial impact. The nuanced truth:

**Detail types do NOT affect:**
- Transaction posting or journal entry behavior
- Arithmetic calculations in QBO
- Which account type category the account belongs to (that is set by Account Type, which is immutable once set)

**Detail types DO affect:**
- The initial suggested mapping when using Prep for Taxes auto-assign
- How Intuit tax products (ProConnect, TurboTax Business) categorize the account during import
- Report filtering by detail type (Account List Report)
- QBO's suggested default account during transaction entry in some contexts

Source: [Chart of Accounts Detail Types thread](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/chart-of-accounts-detail-types/00/724748)
Source: [How to utilize account detail type for reporting](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-utilize-account-detail-type-for-reporting/00/219166)

---

## 9. API Field Reference (AccountType and AccountSubType)

The QBO API Account object uses these fields:

| API Field | Purpose | Required |
|---|---|---|
| `AccountType` | One of the 15 account type strings | Yes |
| `AccountSubType` | One of 125 detail type strings | No (defaults applied) |
| `Classification` | Asset / Equity / Expense / Liability / Revenue | Required, filterable |
| `TaxAccount` | Boolean — marks account as taxable | Optional |
| `TaxCodeRef` | References applicable tax code (Desktop only) | Optional |

Valid `AccountType` strings for the API:
`Bank`, `AccountsReceivable`, `OtherCurrentAsset`, `FixedAsset`, `OtherAsset`, `AccountsPayable`, `CreditCard`, `OtherCurrentLiability`, `LongTermLiability`, `Equity`, `Income`, `CostOfGoodsSold`, `Expense`, `OtherIncome`, `OtherExpense`

Source: [QBO Account API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)
Source: [Account Properties — Intuit SDK](https://static.developer.intuit.com/sdkdocs/qbv3doc/ippdotnetdevkitv3/html/4c8da9d0-06b2-44eb-4996-97416f5999c7.htm)
Source: [Understanding Account Type & SubTypes — Intuit Developer](https://help.developer.intuit.com/s/question/0D54R00009Jch41SAB/understanding-account-type-account-sub-types)

---

## 10. TurboTax / Tax Prep Software: How It Reads QBO

When QBO data is imported into TurboTax Business or via Prep for Taxes into ProConnect:

1. **Company entity type** (from QBO settings) determines which tax form to target (1120, 1120S, 1065, etc.)
2. **Account Type** provides primary classification: which section of the tax form (income, deductions, balance sheet)
3. **Detail Type** provides secondary disambiguation: which *specific line* within that section
4. Unmapped accounts are surfaced for manual assignment during import
5. Tax software allows the user to confirm or override any auto-mapped assignment
6. Corrected mappings can optionally be written back to QBO ("Save these Categories in QuickBooks")

**TurboTax import requires QB Desktop** (QBO files cannot be directly imported into TurboTax Business — they must go through Prep for Taxes → ProConnect, or be exported from QBO to a QB Desktop file first).

Source: [Importing QB data into TurboTax — Insightful Accountant](https://insightfulaccountant.com/tax-and-practice-management/quickbooks-at-year-end:-importing-quickbooks-data-into-turbo/)

---

## 11. Key Tax Line Mapping: Form 1120 Reference Table (Complete)

This mapping is based on the QuickBooks Tax-Line Mapping table used in QB Desktop and TurboTax Business. The same logic drives Prep for Taxes auto-mapping in QBO.

### Income Lines

| QB Tax Line Label | Form 1120 Location |
|---|---|
| Gross receipts or sales | Line 1a |
| Returns and allowances | Line 1b |
| Interest Income | Line 5 |
| Gross rents | Line 6 |
| Gross royalties | Line 7 |
| State tax refund | Line 10 (Other income) |
| Other income | Line 10 |
| Dividends (less than 20% owned corps) | Schedule C |
| Dividends (20%+ owned corps) | Schedule C |
| Dividends (debt-financed stock) | Schedule C |
| Dividends (preferred stock) | Schedule C |
| Dividends (foreign corporations) | Schedule C |
| Dividends (domestic corporations) | Schedule C |
| Tax-exempt interest income | Schedule M-1 |

### Cost of Goods Sold (Form 1125-A / Schedule A)

| QB Tax Line Label | Form 1120 Location |
|---|---|
| Purchases | Schedule A, Line 2 |
| Cost of labor | Schedule A, Line 3 |
| Section 263A depreciation | Schedule A, Line 4 |
| Other costs | Schedule A, Line 5 |

### Deduction Lines

| QB Tax Line Label | Form 1120 Location |
|---|---|
| Accounting | Line 12 |
| Advertising | Line 22 |
| Auto and truck | Line 26 (Other deductions) |
| Bad debts | Line 15 |
| Bank charges | Line 26 |
| Business gifts | Line 26 |
| Charitable contributions | Line 19 |
| Commissions | Line 26 |
| Comp. of Officers | Line 12 |
| Continuing/professional education | Line 26 |
| Credit & collection costs | Line 26 |
| Depletion | Line 21 |
| Depreciation | Line 20 |
| Discounts given | Line 26 |
| Dues and subscriptions | Line 26 |
| Employee benefit programs | Line 24 |
| Independent contractors | Line 26 |
| Insurance | Line 26 |
| Interest expense | Line 18 |
| Janitorial & cleaning | Line 26 |
| Legal & professional fees | Line 26 |
| Licenses | Line 26 |
| Meals and entertainment (50% limit) | Line 26 |
| Meals and entertainment (100% deductible) | Line 26 |
| Office expenses | Line 26 |
| Other deductions | Line 26 |
| Other miscellaneous taxes | Line 17 |
| Parking | Line 26 |
| Payroll taxes | Line 17 |
| Pension/profit-sharing | Line 23 |
| Postage and Delivery | Line 26 |
| Printing & copying | Line 26 |
| Rents | Line 16 |
| Repairs and maintenance | Line 14 |
| Salaries and wages | Line 13 |
| Security | Line 26 |
| State taxes | Line 17 |
| Supplies | Line 26 |
| Telephone/Internet | Line 26 |
| Tools | Line 26 |
| Travel | Line 26 |
| Utilities | Line 26 |
| Local property taxes | Line 17 |

### Balance Sheet Lines (Schedule L)

| QB Tax Line Label | Form 1120 Schedule L |
|---|---|
| Cash | Line 1 |
| Accounts Receivable | Line 2a |
| U.S. government obligations | Line 4 |
| Tax-exempt securities | Line 5 |
| Loans to stockholders | Line 6 |
| Mortgage/real estate loans | Line 7 |
| Buildings/other depreciable assets | Line 8a |
| Accumulated depreciation | Line 8b |
| Depletable assets | Line 9a |
| Accumulated depletion | Line 9b |
| Land | Line 10 |
| Intangible assets | Line 11a |
| Accumulated amortization | Line 11b |
| Other current assets | Line 14 |
| Other investments | Line 14 |
| Other assets | Line 14 |
| Accounts payable | Line 15 |
| Short-term mortgage/note/bonds payable | Line 17 |
| Loans from stockholders | Line 19 |
| Long-term mortgage/note/bonds payable | Line 20 |
| Other current liabilities | Line 17 |
| Other liabilities | Line 17 |
| Capital stock — Preferred | Line 22a |
| Capital stock — Common | Line 22b |
| Paid-in or capital surplus | Line 23 |
| Cost of Treasury stock | Line 27b |

Source: [QuickBooks Tax-Line Mapping Table — ERO Support](https://kb.erosupport.com/print-12.html)
Source: [QuickBooks Tax-Line Mapping Table (alternate URL)](https://kb.erosupport.com/article.php?id=12&id=12)

---

## 12. Official Intuit Guidance Summary

- **No tax line mapping in QBO** (Desktop-only feature). QBO relies on "Prep for Taxes" or the "Manage tax categories" interface.
- **Detail types are Intuit's invention** to simplify bookkeeper account selection; they are fixed and cannot be customized.
- **Setting the tax form type** (1120 vs 1120S vs 1065) is a cross-platform integration setting, not an operational accounting setting. If never integrating with ProConnect/TurboTax, it has no effect on QBO operations.
- **Prep for Taxes auto-maps** based on account type and detail type; always verify auto-assignments before exporting to ProConnect.
- **Consult an accountant** before altering tax line mappings — the official Intuit guidance on every related help thread consistently advises this.

Key official Intuit resources:
- [Learn where detail types appear on your tax forms](https://quickbooks.intuit.com/learn-support/en-us/help-article/tax-forms/learn-quickbooks-online-detail-types-appear-tax/L8cmkg8i9_US_en_US)
- [Manage account mapping to tax categories](https://quickbooks.intuit.com/learn-support/en-us/help-article/corporation/manage-account-mapping-tax-categories-tax-forms/L54GSV2pl_US_en_US)
- [Learn about account types and detail types](https://quickbooks.intuit.com/learn-support/en-us/help-article/chart-accounts/learn-account-detail-types-chart-accounts/L2gCy0rfy_US_en_US)
- [Learn about the chart of accounts in QBO](https://quickbooks.intuit.com/learn-support/en-us/help-article/chart-accounts/learn-chart-accounts-quickbooks-online/L2yc6KBob_US_en_US)
- [QBO Account API Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)
