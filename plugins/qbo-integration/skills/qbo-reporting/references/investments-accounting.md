# Investment Accounting in QuickBooks Online

Account structure and journal entry patterns for investment portfolios.

See also: [investments-tax-reporting.md](investments-tax-reporting.md) for 1099 processing and brokerage reconciliation.

## Contents

- [Overview](#overview)
- [Account Structure](#account-structure)
- [Journal Entry Patterns](#journal-entry-patterns)
- [Best Practices](#best-practices)

---

## Overview

For holding companies and investment-focused entities, QBO tracks:
- Investment asset positions (cost basis, market value)
- Investment income (dividends, interest, capital gains)
- Realized and unrealized gains/losses
- Tax lot tracking for Form 1120 reporting

---

## Account Structure

### Investment Asset Accounts

```
1300 - Short-Term Investments (Other Current Asset)
  1310 - Equity Securities
  1320 - Fixed Income Securities
  1330 - Money Market Funds

1500 - Long-Term Investments (Other Asset)
  1510 - Equity Investments - Long Term
  1520 - Bond Investments - Long Term
  1530 - Private Equity/LLC Investments
```

### Investment Income Accounts

```
4000 - Investment Income (Other Income)
  4010 - Dividend Income - Qualified
  4020 - Dividend Income - Ordinary
  4100 - Interest Income - Taxable
  4110 - Interest Income - Tax-Exempt (Muni)
  4200 - Capital Gains - Short Term
  4210 - Capital Gains - Long Term
  4300 - Unrealized Gains (if mark-to-market)
```

### Investment Expense Accounts

```
5000 - Investment Expenses (Expense)
  5010 - Brokerage Fees
  5020 - Management Fees
  5030 - Custodian Fees

5700 - Investment Losses (Other Expense)
  5710 - Capital Losses - Short Term
  5720 - Capital Losses - Long Term
  5800 - Unrealized Losses
```

---

## Journal Entry Patterns

### Record Stock Purchase

```python
def record_stock_purchase(
    client,
    date: str,
    symbol: str,
    shares: float,
    price_per_share: float,
    commission: float,
    bank_account_id: str,
    investment_account_id: str,
    fee_account_id: str
) -> "JournalEntry":
    """
    Record stock purchase.

    Total cost = (shares * price) + commission
    """
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    total_cost = (shares * price_per_share) + commission
    share_cost = shares * price_per_share

    je = JournalEntry()
    je.TxnDate = date
    je.PrivateNote = f"Purchase {shares} shares of {symbol} @ ${price_per_share:.2f}"
    je.DocNumber = f"BUY-{symbol}-{date}"

    # Debit: Investment Account (asset increase)
    investment_line = JournalEntryLine()
    investment_line.Amount = share_cost
    investment_line.Description = f"{symbol} - {shares} shares @ ${price_per_share:.2f}"
    investment_line.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": investment_account_id}
    }

    # Debit: Commission expense
    fee_line = JournalEntryLine()
    fee_line.Amount = commission
    fee_line.Description = f"Brokerage commission - {symbol}"
    fee_line.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": fee_account_id}
    }

    # Credit: Bank account
    bank_line = JournalEntryLine()
    bank_line.Amount = total_cost
    bank_line.Description = f"Payment for {symbol} purchase"
    bank_line.JournalEntryLineDetail = {
        "PostingType": "Credit",
        "AccountRef": {"value": bank_account_id}
    }

    je.Line = [investment_line, fee_line, bank_line]
    je.save(qb=client)
    return je
```

### Record Stock Sale

```python
def record_stock_sale(
    client,
    date: str,
    symbol: str,
    shares: float,
    sale_price: float,
    cost_basis: float,
    commission: float,
    holding_period: str,  # "short" or "long"
    bank_account_id: str,
    investment_account_id: str,
    fee_account_id: str,
    gain_account_id: str,
    loss_account_id: str
) -> "JournalEntry":
    """
    Record stock sale with capital gain/loss.

    holding_period determines account (short-term vs long-term)
    """
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    gross_proceeds = shares * sale_price
    net_proceeds = gross_proceeds - commission
    gain_loss = net_proceeds - cost_basis

    je = JournalEntry()
    je.TxnDate = date
    je.PrivateNote = f"Sell {shares} shares of {symbol} @ ${sale_price:.2f}"
    je.DocNumber = f"SELL-{symbol}-{date}"

    lines = []

    # Debit: Bank account (cash received)
    bank_line = JournalEntryLine()
    bank_line.Amount = net_proceeds
    bank_line.Description = f"Proceeds from {symbol} sale"
    bank_line.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": bank_account_id}
    }
    lines.append(bank_line)

    # Debit: Commission expense
    if commission > 0:
        fee_line = JournalEntryLine()
        fee_line.Amount = commission
        fee_line.Description = f"Brokerage commission - {symbol} sale"
        fee_line.JournalEntryLineDetail = {
            "PostingType": "Debit",
            "AccountRef": {"value": fee_account_id}
        }
        lines.append(fee_line)

    # Credit: Investment account (remove cost basis)
    investment_line = JournalEntryLine()
    investment_line.Amount = cost_basis
    investment_line.Description = f"Cost basis - {shares} shares {symbol}"
    investment_line.JournalEntryLineDetail = {
        "PostingType": "Credit",
        "AccountRef": {"value": investment_account_id}
    }
    lines.append(investment_line)

    # Gain or Loss
    if gain_loss > 0:
        gain_line = JournalEntryLine()
        gain_line.Amount = gain_loss
        gain_line.Description = f"Capital gain - {symbol} ({holding_period}-term)"
        gain_line.JournalEntryLineDetail = {
            "PostingType": "Credit",
            "AccountRef": {"value": gain_account_id}
        }
        lines.append(gain_line)
    elif gain_loss < 0:
        loss_line = JournalEntryLine()
        loss_line.Amount = abs(gain_loss)
        loss_line.Description = f"Capital loss - {symbol} ({holding_period}-term)"
        loss_line.JournalEntryLineDetail = {
            "PostingType": "Debit",
            "AccountRef": {"value": loss_account_id}
        }
        lines.append(loss_line)

    je.Line = lines
    je.save(qb=client)
    return je
```

### Record Dividend Income

```python
def record_dividend(
    client,
    date: str,
    symbol: str,
    total_amount: float,
    qualified_amount: float,
    bank_account_id: str,
    qualified_div_account_id: str,
    ordinary_div_account_id: str,
    withholding: float = 0.0,
    withholding_account_id: str = None
) -> "JournalEntry":
    """
    Record dividend income with qualified/ordinary split.

    IRS requires tracking qualified vs ordinary dividends separately
    for Form 1120 Schedule C.
    """
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    ordinary_amount = total_amount - qualified_amount
    net_received = total_amount - withholding

    je = JournalEntry()
    je.TxnDate = date
    je.PrivateNote = f"Dividend from {symbol}"
    je.DocNumber = f"DIV-{symbol}-{date}"

    lines = []

    # Debit: Bank account (cash received)
    bank_line = JournalEntryLine()
    bank_line.Amount = net_received
    bank_line.Description = f"Dividend received - {symbol}"
    bank_line.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": bank_account_id}
    }
    lines.append(bank_line)

    # Debit: Tax withheld (if any)
    if withholding > 0 and withholding_account_id:
        wh_line = JournalEntryLine()
        wh_line.Amount = withholding
        wh_line.Description = f"Tax withheld - {symbol} dividend"
        wh_line.JournalEntryLineDetail = {
            "PostingType": "Debit",
            "AccountRef": {"value": withholding_account_id}
        }
        lines.append(wh_line)

    # Credit: Qualified dividend income
    if qualified_amount > 0:
        qual_line = JournalEntryLine()
        qual_line.Amount = qualified_amount
        qual_line.Description = f"Qualified dividend - {symbol}"
        qual_line.JournalEntryLineDetail = {
            "PostingType": "Credit",
            "AccountRef": {"value": qualified_div_account_id}
        }
        lines.append(qual_line)

    # Credit: Ordinary dividend income
    if ordinary_amount > 0:
        ord_line = JournalEntryLine()
        ord_line.Amount = ordinary_amount
        ord_line.Description = f"Ordinary dividend - {symbol}"
        ord_line.JournalEntryLineDetail = {
            "PostingType": "Credit",
            "AccountRef": {"value": ordinary_div_account_id}
        }
        lines.append(ord_line)

    je.Line = lines
    je.save(qb=client)
    return je
```

### Record Interest Income

```python
def record_interest(
    client,
    date: str,
    source: str,
    amount: float,
    is_tax_exempt: bool,
    bank_account_id: str,
    taxable_interest_account_id: str,
    tax_exempt_interest_account_id: str
) -> "JournalEntry":
    """Record interest income (taxable or tax-exempt)."""
    from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

    je = JournalEntry()
    je.TxnDate = date
    exempt_str = "tax-exempt" if is_tax_exempt else "taxable"
    je.PrivateNote = f"Interest income from {source} ({exempt_str})"
    je.DocNumber = f"INT-{date}"

    # Debit: Bank
    bank_line = JournalEntryLine()
    bank_line.Amount = amount
    bank_line.Description = f"Interest received - {source}"
    bank_line.JournalEntryLineDetail = {
        "PostingType": "Debit",
        "AccountRef": {"value": bank_account_id}
    }

    # Credit: Interest income
    interest_line = JournalEntryLine()
    interest_line.Amount = amount
    interest_line.Description = f"Interest income - {source}"
    interest_line.JournalEntryLineDetail = {
        "PostingType": "Credit",
        "AccountRef": {
            "value": tax_exempt_interest_account_id if is_tax_exempt
                    else taxable_interest_account_id
        }
    }

    je.Line = [bank_line, interest_line]
    je.save(qb=client)
    return je
```

---

## Best Practices

1. **Track cost basis accurately** - Essential for capital gains calculations
2. **Separate qualified/ordinary dividends** - Required for Form 1120 Schedule C
3. **Distinguish short vs long-term** - Different tax treatment
4. **Use consistent DocNumbers** - Easy audit trail (BUY-AAPL-2024-01-15)
5. **Record fees separately** - Deductible investment expenses
