---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Chart of Accounts for Holding Companies

## Table of Contents

1. [Overview](#overview)
2. [C-Corporation Holding Company COA](#c-corporation-holding-company-coa)
3. [Account Setup in QBO](#account-setup-in-qbo)
4. [Investment Tracking](#investment-tracking)
5. [Inter-Company Transactions](#inter-company-transactions)
6. [Foreign-Owned Corporation Considerations](#foreign-owned-corporation-considerations)

---

## Overview

A holding company's Chart of Accounts focuses on:
- Investment assets (stocks, bonds, real estate)
- Investment income (dividends, interest, capital gains)
- Minimal operating expenses
- Equity tracking for shareholders/parent entities

---

## C-Corporation Holding Company COA

### Assets (1000-1999)

| Account # | Name | Type | SubType | Purpose |
|-----------|------|------|---------|---------|
| 1000 | Checking | Bank | Checking | Operating cash |
| 1010 | Savings | Bank | Savings | Reserve funds |
| 1100 | Money Market | Bank | MoneyMarket | Short-term investments |
| 1200 | Accounts Receivable | Accounts Receivable | AccountsReceivable | Amounts owed to company |
| 1300 | Short-Term Investments | Other Current Asset | Investments | Marketable securities (<1yr) |
| 1310 | Equity Securities | Other Current Asset | Investments | Stocks, ETFs |
| 1320 | Fixed Income Securities | Other Current Asset | Investments | Bonds (<1yr maturity) |
| 1400 | Prepaid Expenses | Other Current Asset | PrepaidExpenses | Prepaid insurance, etc. |
| 1500 | Long-Term Investments | Other Asset | OtherLongTermAssets | Securities held >1yr |
| 1510 | Equity Investments - LT | Other Asset | OtherLongTermAssets | Long-term stock holdings |
| 1520 | Bond Investments - LT | Other Asset | OtherLongTermAssets | Bonds >1yr maturity |
| 1600 | Due from Related Parties | Other Current Asset | LoansToOthers | Intercompany receivables |
| 1700 | Investment in Subsidiaries | Other Asset | OtherLongTermAssets | Subsidiary equity |

### Liabilities (2000-2999)

| Account # | Name | Type | SubType | Purpose |
|-----------|------|------|---------|---------|
| 2000 | Accounts Payable | Accounts Payable | AccountsPayable | Vendor bills |
| 2100 | Credit Card | Credit Card | CreditCard | Corporate card |
| 2200 | Accrued Expenses | Other Current Liability | OtherCurrentLiabilities | Accrued but unpaid |
| 2300 | Taxes Payable | Other Current Liability | OtherCurrentLiabilities | Federal/state taxes |
| 2310 | Federal Income Tax Payable | Other Current Liability | OtherCurrentLiabilities | Federal tax liability |
| 2320 | State Income Tax Payable | Other Current Liability | OtherCurrentLiabilities | State tax liability |
| 2400 | Due to Related Parties | Other Current Liability | OtherCurrentLiabilities | Intercompany payables |
| 2500 | Notes Payable - Current | Other Current Liability | LoanPayable | Current portion of debt |
| 2600 | Notes Payable - Long Term | Long Term Liability | NotesPayable | Long-term debt |

### Equity (3000-3999)

| Account # | Name | Type | SubType | Purpose |
|-----------|------|------|---------|---------|
| 3000 | Common Stock | Equity | CommonStock | Par value of shares |
| 3100 | Additional Paid-In Capital | Equity | PaidInCapitalOrSurplus | Amount above par |
| 3200 | Retained Earnings | Equity | RetainedEarnings | Accumulated profits |
| 3300 | Treasury Stock | Equity | TreasuryStock | Repurchased shares |
| 3900 | Opening Balance Equity | Equity | OpeningBalanceEquity | Migration adjustments |

### Income (4000-4999)

| Account # | Name | Type | SubType | Purpose |
|-----------|------|------|---------|---------|
| 4000 | Dividend Income | Other Income | DividendIncome | Stock dividends |
| 4010 | Qualified Dividends | Other Income | DividendIncome | Qualified div (tax) |
| 4020 | Non-Qualified Dividends | Other Income | DividendIncome | Ordinary div (tax) |
| 4100 | Interest Income | Other Income | InterestEarned | Bond/account interest |
| 4110 | Tax-Exempt Interest | Other Income | TaxExemptInterest | Municipal bond interest |
| 4200 | Capital Gains - Short Term | Other Income | OtherInvestmentIncome | Gains <1yr holding |
| 4210 | Capital Gains - Long Term | Other Income | OtherInvestmentIncome | Gains >1yr holding |
| 4300 | Unrealized Gains | Other Income | OtherInvestmentIncome | Mark-to-market (if used) |
| 4400 | Management Fee Income | Income | ServiceFeeIncome | Fees from subsidiaries |
| 4900 | Other Income | Other Income | OtherMiscellaneousIncome | Miscellaneous |

### Expenses (5000-5999)

| Account # | Name | Type | SubType | Purpose |
|-----------|------|------|---------|---------|
| 5000 | Investment Fees | Expense | CommissionsAndFees | Brokerage fees |
| 5010 | Management Fees | Expense | CommissionsAndFees | Fund management fees |
| 5100 | Professional Fees | Expense | LegalAndProfessionalFees | CPA, attorney |
| 5110 | Accounting Fees | Expense | LegalAndProfessionalFees | Tax prep, bookkeeping |
| 5120 | Legal Fees | Expense | LegalAndProfessionalFees | Legal services |
| 5200 | Bank Charges | Expense | BankCharges | Bank fees |
| 5300 | Insurance | Expense | Insurance | D&O, liability |
| 5400 | Office Expenses | Expense | OfficeExpenses | Registered agent, etc. |
| 5500 | State Fees | Expense | Taxes | Annual report fees |
| 5600 | Interest Expense | Other Expense | OtherMiscellaneousExpense | Loan interest |
| 5700 | Capital Losses - ST | Other Expense | OtherMiscellaneousExpense | Losses <1yr |
| 5710 | Capital Losses - LT | Other Expense | OtherMiscellaneousExpense | Losses >1yr |
| 5800 | Unrealized Losses | Other Expense | OtherMiscellaneousExpense | Mark-to-market |
| 5900 | Other Expenses | Expense | OtherBusinessExpenses | Miscellaneous |

---

## Account Setup in QBO

### Python Script for COA Creation

```python
from quickbooks.objects.account import Account


def create_holding_company_coa(client, accounts: list[dict]):
    """Create Chart of Accounts for holding company."""
    created = []

    for acct in accounts:
        account = Account()
        account.Name = acct["name"]
        account.AccountType = acct["type"]
        account.AccountSubType = acct["subtype"]

        if acct.get("description"):
            account.Description = acct["description"]

        if acct.get("acct_num"):
            account.AcctNum = acct["acct_num"]

        try:
            account.save(qb=client)
            created.append({
                "name": account.Name,
                "id": account.Id,
                "status": "created"
            })
        except Exception as e:
            created.append({
                "name": acct["name"],
                "error": str(e),
                "status": "failed"
            })

    return created


# Example usage
HOLDING_CO_ACCOUNTS = [
    {
        "acct_num": "1000",
        "name": "Checking",
        "type": "Bank",
        "subtype": "Checking",
        "description": "Primary operating account"
    },
    {
        "acct_num": "1300",
        "name": "Short-Term Investments",
        "type": "Other Current Asset",
        "subtype": "Investments",
        "description": "Marketable securities held less than one year"
    },
    {
        "acct_num": "4000",
        "name": "Dividend Income",
        "type": "Other Income",
        "subtype": "DividendIncome",
        "description": "Dividends received from equity investments"
    },
    # ... add all accounts
]

results = create_holding_company_coa(client, HOLDING_CO_ACCOUNTS)
```

### Bulk Creation via Batch API

```python
from quickbooks.batch import batch_create

accounts_to_create = []
for acct in HOLDING_CO_ACCOUNTS:
    account = Account()
    account.Name = acct["name"]
    account.AccountType = acct["type"]
    account.AccountSubType = acct["subtype"]
    if acct.get("description"):
        account.Description = acct["description"]
    accounts_to_create.append(account)

# Create in batches of 30
for i in range(0, len(accounts_to_create), 30):
    batch = accounts_to_create[i:i+30]
    results = batch_create(batch, qb=client)
```

---

## Investment Tracking

### Purchase Investment

```python
def record_investment_purchase(
    client,
    date: str,
    security_name: str,
    shares: float,
    price_per_share: float,
    total_cost: float,
    cash_account_id: str,
    investment_account_id: str
):
    """Record purchase of investment securities."""
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    je = JournalEntry()
    je.TxnDate = date
    je.PrivateNote = f"Purchase {shares} shares of {security_name} @ ${price_per_share}"

    # Debit: Investment account (increase asset)
    debit = JournalEntryLine()
    debit.Amount = total_cost
    debit.Description = f"{security_name} - {shares} shares"
    debit.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": investment_account_id}
    }

    # Credit: Cash account (decrease asset)
    credit = JournalEntryLine()
    credit.Amount = total_cost
    credit.Description = f"Payment for {security_name}"
    credit.JournalEntryLineDetail = {
        "PostingType": "Credit",
        "AccountRef": {"value": cash_account_id}
    }

    je.Line = [debit, credit]
    je.save(qb=client)
    return je
```

### Sell Investment with Gain/Loss

```python
def record_investment_sale(
    client,
    date: str,
    security_name: str,
    shares: float,
    cost_basis: float,
    sale_proceeds: float,
    cash_account_id: str,
    investment_account_id: str,
    gain_account_id: str,
    loss_account_id: str,
    holding_period: str = "long"  # "short" or "long"
):
    """Record sale of investment with gain or loss."""
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    gain_loss = sale_proceeds - cost_basis
    je = JournalEntry()
    je.TxnDate = date
    je.PrivateNote = f"Sell {shares} shares of {security_name}"

    lines = []

    # Debit: Cash (proceeds received)
    cash_line = JournalEntryLine()
    cash_line.Amount = sale_proceeds
    cash_line.Description = f"Sale proceeds - {security_name}"
    cash_line.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": cash_account_id}
    }
    lines.append(cash_line)

    # Credit: Investment account (remove at cost)
    inv_line = JournalEntryLine()
    inv_line.Amount = cost_basis
    inv_line.Description = f"Cost basis - {security_name}"
    inv_line.JournalEntryLineDetail = {
        "PostingType": "Credit",
        "AccountRef": {"value": investment_account_id}
    }
    lines.append(inv_line)

    # Record gain or loss
    if gain_loss > 0:
        # Credit: Capital Gain
        gain_line = JournalEntryLine()
        gain_line.Amount = gain_loss
        gain_line.Description = f"Capital gain ({holding_period}-term) - {security_name}"
        gain_line.JournalEntryLineDetail = {
            "PostingType": "Credit",
            "AccountRef": {"value": gain_account_id}
        }
        lines.append(gain_line)
    elif gain_loss < 0:
        # Debit: Capital Loss
        loss_line = JournalEntryLine()
        loss_line.Amount = abs(gain_loss)
        loss_line.Description = f"Capital loss ({holding_period}-term) - {security_name}"
        loss_line.JournalEntryLineDetail = {
            "PostingType": "Debit",
            "AccountRef": {"value": loss_account_id}
        }
        lines.append(loss_line)

    je.Line = lines
    je.save(qb=client)
    return je
```

### Record Dividend

```python
def record_dividend(
    client,
    date: str,
    payer: str,
    amount: float,
    cash_account_id: str,
    dividend_account_id: str,
    qualified: bool = True
):
    """Record dividend income received."""
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    je = JournalEntry()
    je.TxnDate = date
    div_type = "Qualified" if qualified else "Non-qualified"
    je.PrivateNote = f"{div_type} dividend from {payer}"

    # Debit: Cash
    debit = JournalEntryLine()
    debit.Amount = amount
    debit.Description = f"Dividend - {payer}"
    debit.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": cash_account_id}
    }

    # Credit: Dividend Income
    credit = JournalEntryLine()
    credit.Amount = amount
    credit.Description = f"{div_type} dividend - {payer}"
    credit.JournalEntryLineDetail = {
        "PostingType": "Credit",
        "AccountRef": {"value": dividend_account_id}
    }

    je.Line = [debit, credit]
    je.save(qb=client)
    return je
```

---

## Inter-Company Transactions

### Management Fee from Subsidiary

```python
def record_management_fee(
    client,
    date: str,
    subsidiary_name: str,
    amount: float,
    intercompany_receivable_id: str,
    management_fee_income_id: str
):
    """Record management fee charged to subsidiary."""
    je = JournalEntry()
    je.TxnDate = date
    je.PrivateNote = f"Management fee - {subsidiary_name}"

    je.Line = [
        # Debit: Due from subsidiary
        {
            "DetailType": "JournalEntryLineDetail",
            "Amount": amount,
            "Description": f"Due from {subsidiary_name}",
            "JournalEntryLineDetail": {
                "PostingType": "Debit",
                "AccountRef": {"value": intercompany_receivable_id}
            }
        },
        # Credit: Management fee income
        {
            "DetailType": "JournalEntryLineDetail",
            "Amount": amount,
            "Description": f"Management fee - {subsidiary_name}",
            "JournalEntryLineDetail": {
                "PostingType": "Credit",
                "AccountRef": {"value": management_fee_income_id}
            }
        }
    ]
    je.save(qb=client)
```

---

## Foreign-Owned Corporation Considerations

### Form 5472 Tracking

For 25%+ foreign-owned corporations, track:

1. **Transactions with foreign related parties**
2. **Types of transactions** (capital contributions, loans, services)
3. **Dollar amounts** by category

### Recommended Sub-Accounts

| Account | Purpose |
|---------|---------|
| Due from Foreign Parent | Amounts owed by foreign owner |
| Due to Foreign Parent | Amounts owed to foreign owner |
| Capital Contributions - Foreign | Equity from foreign shareholders |
| Intercompany Interest - Foreign | Interest on related party loans |
| Management Fees - Foreign Related | Fees paid to foreign related parties |

### Transaction Categorization for 5472

```python
# Tag transactions for Form 5472 reporting
FORM_5472_CATEGORIES = {
    "capital_contributions": "Part IV, Line 13",
    "loans_received": "Part IV, Line 15",
    "loans_made": "Part IV, Line 16",
    "interest_received": "Part IV, Line 18",
    "interest_paid": "Part IV, Line 19",
    "services_received": "Part IV, Line 22",
    "services_provided": "Part IV, Line 23",
    "other_amounts_paid": "Part IV, Line 25",
    "other_amounts_received": "Part IV, Line 26"
}
```

See [references/compliance-checklist.md](compliance-checklist.md) for full Form 5472 requirements.

---

## QBO AccountSubType Notes

When creating accounts via QBO API, AccountSubType must be valid for the chosen AccountType.

### Common Pitfalls

| AccountType | Invalid SubType | Valid SubType | Notes |
|-------------|-----------------|---------------|-------|
| Other Current Asset | `Investments` | `OtherCurrentAssets` | "Investments" only valid for "Other Asset" (long-term) |
| Other Current Asset | `Investment` | `OtherCurrentAssets` | Same issue - singular form also invalid |
| Other Asset | `OtherCurrentAssets` | `OtherLongTermAssets` | Current vs long-term distinction matters |

### Brokerage/Investment Account Creation

For brokerage accounts holding securities:
- **Short-term holdings (<1 year)**: Use `Other Current Asset` + `OtherCurrentAssets`
- **Long-term holdings (>1 year)**: Use `Other Asset` + `OtherLongTermAssets`

```python
# Correct pattern for brokerage account
{
    "Name": "1110 - Brokerage Account",
    "AccountType": "Other Current Asset",
    "AccountSubType": "OtherCurrentAssets",  # NOT "Investments"
    "AcctNum": "1110"
}
```

### Error Message

QBO API returns this error for invalid type/subtype combinations:
```
Invalid Enumeration : Invalid Account.AccountType && Account.SubType
```

When you see this error, check the [QBO Account Types Reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account) for valid combinations.
