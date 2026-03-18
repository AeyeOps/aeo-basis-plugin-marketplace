---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Audit Trial Balance

Trial balance extraction and formatting for CPA audit workpapers.

## Contents

- [Overview](#overview)
- [Standard Trial Balance Format](#standard-trial-balance-format)
- [Excel Export](#excel-export)
- [Comparative Trial Balance](#comparative-trial-balance)
- [See Also](#see-also)

---

## Overview

Trial balance extraction from QuickBooks Online for audit workpapers, including:
- Balance extraction as of any date
- Debit/credit presentation based on normal balance rules
- Excel formatting with standard audit layout
- Comparative (current vs prior year) analysis

---

## Standard Trial Balance Format

```python
from dataclasses import dataclass
from decimal import Decimal
from datetime import date
from typing import Optional
import json

@dataclass
class TrialBalanceRow:
    """Single row in trial balance."""
    account_id: str
    account_number: str
    account_name: str
    account_type: str
    classification: str  # Asset, Liability, Equity, Revenue, Expense
    debit: Decimal
    credit: Decimal
    wp_reference: str = ""  # Workpaper reference (e.g., "A-1", "B-2")


def extract_trial_balance(
    client,
    as_of_date: str,
    include_zero_balances: bool = False
) -> list[TrialBalanceRow]:
    """
    Extract trial balance from QBO as of specified date.

    Args:
        client: QuickBooks client
        as_of_date: Balance date (YYYY-MM-DD)
        include_zero_balances: Include accounts with zero balance

    Returns:
        List of TrialBalanceRow sorted by account number
    """
    from quickbooks.objects.account import Account

    accounts = Account.all(qb=client)
    rows = []

    for acct in accounts:
        if not acct.Active:
            continue

        balance = Decimal(str(acct.CurrentBalance or 0))

        if not include_zero_balances and balance == 0:
            continue

        # Determine debit/credit based on account type and balance
        debit = Decimal("0")
        credit = Decimal("0")

        classification = acct.Classification

        # Normal balance rules
        if classification in ["Asset", "Expense"]:
            # Normal debit balance
            if balance >= 0:
                debit = balance
            else:
                credit = abs(balance)
        else:
            # Liability, Equity, Revenue - normal credit balance
            if balance >= 0:
                credit = balance
            else:
                debit = abs(balance)

        rows.append(TrialBalanceRow(
            account_id=acct.Id,
            account_number=acct.AcctNum or "",
            account_name=acct.Name,
            account_type=acct.AccountType,
            classification=classification,
            debit=debit,
            credit=credit
        ))

    # Sort by account number
    return sorted(rows, key=lambda r: (r.account_number or "zzz", r.account_name))
```

---

## Excel Export

```python
def format_trial_balance_excel(
    rows: list[TrialBalanceRow],
    as_of_date: str,
    company_name: str,
    output_path: str
) -> str:
    """
    Format trial balance for Excel export.

    Standard audit format with:
    - Header with company name and date
    - Account number, name, debit, credit columns
    - Totals row with balance check
    """
    from openpyxl import Workbook
    from openpyxl.styles import Font, Alignment, Border, Side
    from openpyxl.utils import get_column_letter

    wb = Workbook()
    ws = wb.active
    ws.title = "Trial Balance"

    # Header
    ws["A1"] = company_name
    ws["A1"].font = Font(bold=True, size=14)
    ws["A2"] = f"Trial Balance as of {as_of_date}"
    ws["A2"].font = Font(bold=True, size=12)

    # Column headers
    headers = ["Acct #", "Account Name", "Debit", "Credit", "WP Ref"]
    for col, header in enumerate(headers, 1):
        cell = ws.cell(row=4, column=col, value=header)
        cell.font = Font(bold=True)
        cell.border = Border(bottom=Side(style="thin"))

    # Data rows
    total_debits = Decimal("0")
    total_credits = Decimal("0")

    for row_num, row in enumerate(rows, 5):
        ws.cell(row=row_num, column=1, value=row.account_number)
        ws.cell(row=row_num, column=2, value=row.account_name)
        ws.cell(row=row_num, column=3, value=float(row.debit) if row.debit else None)
        ws.cell(row=row_num, column=4, value=float(row.credit) if row.credit else None)
        ws.cell(row=row_num, column=5, value=row.wp_reference)

        total_debits += row.debit
        total_credits += row.credit

    # Totals row
    total_row = len(rows) + 5
    ws.cell(row=total_row, column=2, value="TOTALS").font = Font(bold=True)
    ws.cell(row=total_row, column=3, value=float(total_debits)).font = Font(bold=True)
    ws.cell(row=total_row, column=4, value=float(total_credits)).font = Font(bold=True)

    # Balance check
    diff = total_debits - total_credits
    ws.cell(row=total_row + 1, column=2, value="Difference")
    ws.cell(row=total_row + 1, column=3, value=float(diff))

    if diff != 0:
        ws.cell(row=total_row + 1, column=3).font = Font(color="FF0000", bold=True)

    # Format columns
    ws.column_dimensions["A"].width = 10
    ws.column_dimensions["B"].width = 40
    ws.column_dimensions["C"].width = 15
    ws.column_dimensions["D"].width = 15
    ws.column_dimensions["E"].width = 10

    # Number format for currency columns
    for row in ws.iter_rows(min_row=5, max_row=total_row + 1, min_col=3, max_col=4):
        for cell in row:
            cell.number_format = '#,##0.00'

    wb.save(output_path)
    return output_path
```

---

## Comparative Trial Balance

```python
def extract_comparative_trial_balance(
    client,
    current_date: str,
    prior_date: str
) -> list[dict]:
    """
    Extract comparative trial balance (current vs prior year).

    Returns rows with current, prior, and change columns.
    """
    current_tb = extract_trial_balance(client, current_date, include_zero_balances=True)
    prior_tb = extract_trial_balance(client, prior_date, include_zero_balances=True)

    # Index prior year by account ID
    prior_by_id = {r.account_id: r for r in prior_tb}

    comparative = []
    for current in current_tb:
        prior = prior_by_id.get(current.account_id)

        current_balance = current.debit - current.credit
        prior_balance = (prior.debit - prior.credit) if prior else Decimal("0")
        change = current_balance - prior_balance

        # Calculate percentage change
        pct_change = None
        if prior_balance != 0:
            pct_change = float((change / prior_balance) * 100)

        comparative.append({
            "account_id": current.account_id,
            "account_number": current.account_number,
            "account_name": current.account_name,
            "classification": current.classification,
            "current_balance": float(current_balance),
            "prior_balance": float(prior_balance),
            "change": float(change),
            "pct_change": pct_change,
            "significant": abs(change) > 1000 or (pct_change and abs(pct_change) > 10)
        })

    return comparative
```

---

## See Also

- [audit-pbc-lists.md](audit-pbc-lists.md) - PBC list generation and tracking
- [audit-lead-schedules.md](audit-lead-schedules.md) - Lead schedules with standard references
- [audit-aje-patterns.md](audit-aje-patterns.md) - Adjusting Journal Entry patterns
- [audit-rje-patterns.md](audit-rje-patterns.md) - Reclassifying Journal Entry patterns
