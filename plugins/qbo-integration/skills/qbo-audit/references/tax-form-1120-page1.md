---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Form 1120 Page 1 Mappings

C-Corporation tax form data extraction for Form 1120 Page 1 (Income and Deductions).

## Contents

- [Overview](#overview)
- [Tax Line Mapping Structure](#tax-line-mapping-structure)
- [Income Mappings](#income-mappings)
- [Deduction Mappings](#deduction-mappings)
- [Extract Account Balances](#extract-account-balances)
- [See Also](#see-also)

---

## Overview

Form 1120 Page 1 requires mapping QBO income and expense accounts to specific tax lines. This reference covers:
- Account-to-form-line mapping structures
- Income line mappings (Lines 1-10)
- Deduction line mappings (Lines 12-26)
- Balance extraction functions

See [tax-schedule-l-m.md](tax-schedule-l-m.md) for Schedules L, M-1, M-2, and C.

---

## Tax Line Mapping Structure

```python
from dataclasses import dataclass
from decimal import Decimal
from typing import Optional
import re

@dataclass
class TaxLineMapping:
    """Maps QBO account to Form 1120 line."""
    qbo_account_type: str
    qbo_account_name_pattern: str  # regex or contains
    form_line: str
    schedule: str  # "1120", "L", "M-1", "M-2", "C"
    description: str
    sign_adjustment: int = 1  # 1 or -1 for sign flip
```

---

## Income Mappings

Form 1120 Page 1, Lines 1-10:

```python
INCOME_MAPPINGS = [
    TaxLineMapping("Income", "gross receipts", "1a", "1120", "Gross receipts or sales", 1),
    TaxLineMapping("Income", "returns", "1b", "1120", "Returns and allowances", -1),
    TaxLineMapping("Other Income", "dividend", "4", "1120", "Dividends (Schedule C)", 1),
    TaxLineMapping("Other Income", "interest", "5", "1120", "Interest", 1),
    TaxLineMapping("Other Income", "gross rents", "6", "1120", "Gross rents", 1),
    TaxLineMapping("Other Income", "royalt", "7", "1120", "Gross royalties", 1),
    TaxLineMapping("Other Income", "capital gain", "8", "1120", "Capital gain net income", 1),
    TaxLineMapping("Other Income", "1231", "9", "1120", "Net gain Form 4797", 1),
    TaxLineMapping("Other Income", None, "10", "1120", "Other income", 1),
]
```

---

## Deduction Mappings

Form 1120 Page 1, Lines 12-26:

```python
DEDUCTION_MAPPINGS = [
    TaxLineMapping("Expense", "officer", "12", "1120", "Compensation of officers", 1),
    TaxLineMapping("Expense", "salaries|wages", "13", "1120", "Salaries and wages", 1),
    TaxLineMapping("Expense", "repair", "14", "1120", "Repairs and maintenance", 1),
    TaxLineMapping("Expense", "bad debt", "15", "1120", "Bad debts", 1),
    TaxLineMapping("Expense", "rent", "16", "1120", "Rents", 1),
    TaxLineMapping("Expense", "tax|license", "17", "1120", "Taxes and licenses", 1),
    TaxLineMapping("Expense", "interest", "18", "1120", "Interest", 1),
    TaxLineMapping("Expense", "charitable|contribution", "19", "1120", "Charitable contributions", 1),
    TaxLineMapping("Expense", "depreciation", "20", "1120", "Depreciation (Form 4562)", 1),
    TaxLineMapping("Expense", "depletion", "21", "1120", "Depletion", 1),
    TaxLineMapping("Expense", "advertising", "22", "1120", "Advertising", 1),
    TaxLineMapping("Expense", "pension|profit sharing", "23", "1120", "Pension/profit-sharing plans", 1),
    TaxLineMapping("Expense", "employee benefit", "24", "1120", "Employee benefit programs", 1),
    TaxLineMapping("Expense", None, "26", "1120", "Other deductions", 1),
]
```

---

## Extract Account Balances

### Get Accounts by Pattern

```python
from quickbooks.objects.account import Account

def get_accounts_by_pattern(
    client,
    account_type: str,
    name_pattern: Optional[str] = None
) -> list[dict]:
    """
    Get accounts matching type and optional name pattern.

    Args:
        client: QuickBooks client
        account_type: QBO account type (Income, Expense, etc.)
        name_pattern: Regex pattern for account name

    Returns:
        List of matching accounts with balances
    """
    query = f"SELECT * FROM Account WHERE AccountType = '{account_type}' AND Active = true"
    accounts = Account.query(query, qb=client)

    results = []
    for acct in accounts:
        if name_pattern:
            if not re.search(name_pattern, acct.Name, re.IGNORECASE):
                continue

        results.append({
            "id": acct.Id,
            "name": acct.Name,
            "type": acct.AccountType,
            "balance": float(acct.CurrentBalance or 0)
        })

    return results
```

### Extract Form 1120 Data

```python
def extract_form_1120_data(client, tax_year: int) -> dict:
    """
    Extract all data needed for Form 1120 from QBO.

    Returns dict with form lines and amounts.
    """
    from datetime import date

    start_date = f"{tax_year}-01-01"
    end_date = f"{tax_year}-12-31"

    # Get P&L report for the year
    pnl = get_profit_and_loss(client, start_date, end_date)

    # Get Balance Sheet as of year end
    bs = get_balance_sheet(client, end_date)

    form_data = {
        "tax_year": tax_year,
        "page1": {},
        "schedule_l": {},
        "schedule_m1": {},
        "schedule_m2": {},
        "schedule_c": {}
    }

    # Map income accounts
    for mapping in INCOME_MAPPINGS:
        accounts = get_accounts_by_pattern(
            client,
            mapping.qbo_account_type,
            mapping.qbo_account_name_pattern
        )
        total = sum(a["balance"] * mapping.sign_adjustment for a in accounts)
        form_data["page1"][mapping.form_line] = {
            "description": mapping.description,
            "amount": total,
            "accounts": accounts
        }

    # Map deduction accounts
    for mapping in DEDUCTION_MAPPINGS:
        accounts = get_accounts_by_pattern(
            client,
            mapping.qbo_account_type,
            mapping.qbo_account_name_pattern
        )
        total = sum(a["balance"] * mapping.sign_adjustment for a in accounts)
        form_data["page1"][mapping.form_line] = {
            "description": mapping.description,
            "amount": total,
            "accounts": accounts
        }

    return form_data
```

---

## See Also

- [tax-schedule-l-m.md](tax-schedule-l-m.md) - Schedules L, M-1, M-2, C
- [tax-estimated-payments.md](tax-estimated-payments.md) - Quarterly estimated tax
