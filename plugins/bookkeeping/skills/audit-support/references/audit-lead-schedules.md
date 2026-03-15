# Audit Lead Schedules

Lead schedules, tickmark conventions, and workpaper package generation for CPA audits.

## Contents

- [Overview](#overview)
- [Lead Schedule Structure](#lead-schedule-structure)
- [Standard Lead Schedule Mappings](#standard-lead-schedule-mappings)
- [Generate Lead Schedule](#generate-lead-schedule)
- [Tickmark Conventions](#tickmark-conventions)
- [Complete Workpaper Package](#complete-workpaper-package)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

Lead schedules summarize account groups and link to supporting schedules. This reference covers:
- Standard lead schedule structure and references
- Account-to-lead-schedule mapping
- Tickmark conventions
- Complete workpaper package generation

---

## Lead Schedule Structure

```python
from dataclasses import dataclass
from decimal import Decimal
import json

@dataclass
class LeadSchedule:
    """Lead schedule for an account group."""
    reference: str  # e.g., "A", "B", "C"
    title: str
    accounts: list[str]  # Account IDs
    prior_year_balance: Decimal
    adjustments: list[dict]  # AJEs and RJEs
    current_year_balance: Decimal
    supporting_schedules: list[str]
```

---

## Standard Lead Schedule Mappings

```python
LEAD_SCHEDULE_MAPPINGS = {
    "A": {
        "title": "Cash and Cash Equivalents",
        "account_types": ["Bank"],
        "supporting": ["A-1 Bank Reconciliations", "A-2 Outstanding Checks"]
    },
    "B": {
        "title": "Investments",
        "account_types": ["Other Current Asset", "Other Asset"],
        "account_patterns": ["investment", "securities", "brokerage"],
        "supporting": ["B-1 Investment Schedule", "B-2 Realized Gains/Losses"]
    },
    "C": {
        "title": "Accounts Receivable",
        "account_types": ["Accounts Receivable"],
        "supporting": ["C-1 AR Aging", "C-2 Subsequent Collections", "C-3 Allowance Roll"]
    },
    "D": {
        "title": "Prepaid Expenses",
        "account_types": ["Other Current Asset"],
        "account_patterns": ["prepaid"],
        "supporting": ["D-1 Prepaid Schedule"]
    },
    "E": {
        "title": "Fixed Assets",
        "account_types": ["Fixed Asset"],
        "supporting": ["E-1 Fixed Asset Roll", "E-2 Additions", "E-3 Depreciation"]
    },
    "F": {
        "title": "Intangible Assets",
        "account_types": ["Other Asset"],
        "account_patterns": ["intangible", "goodwill"],
        "supporting": ["F-1 Intangibles Schedule"]
    },
    "AA": {
        "title": "Accounts Payable",
        "account_types": ["Accounts Payable"],
        "supporting": ["AA-1 AP Aging", "AA-2 Subsequent Disbursements"]
    },
    "BB": {
        "title": "Accrued Liabilities",
        "account_types": ["Other Current Liability"],
        "supporting": ["BB-1 Accruals Schedule"]
    },
    "CC": {
        "title": "Debt",
        "account_types": ["Long Term Liability"],
        "supporting": ["CC-1 Debt Schedule", "CC-2 Interest Calculation"]
    },
    "EQ": {
        "title": "Equity",
        "account_types": ["Equity"],
        "supporting": ["EQ-1 Equity Rollforward"]
    },
    "PL-R": {
        "title": "Revenue",
        "account_types": ["Income"],
        "supporting": ["PL-R-1 Revenue Analysis"]
    },
    "PL-E": {
        "title": "Operating Expenses",
        "account_types": ["Expense"],
        "supporting": ["PL-E-1 Expense Analysis"]
    }
}
```

---

## Generate Lead Schedule

```python
def generate_lead_schedule(
    client,
    lead_ref: str,
    current_date: str,
    prior_date: str,
    adjustments: list[dict] = None
) -> dict:
    """
    Generate lead schedule for account group.

    Args:
        client: Accounting platform client (must implement get_accounts()
                and get_account_balance_as_of(account_id, date))
        lead_ref: Lead schedule reference (e.g., "A", "C")
        current_date: Current year-end
        prior_date: Prior year-end
        adjustments: List of AJEs/RJEs affecting these accounts
    """
    if lead_ref not in LEAD_SCHEDULE_MAPPINGS:
        raise ValueError(f"Unknown lead schedule: {lead_ref}")

    config = LEAD_SCHEDULE_MAPPINGS[lead_ref]

    # Get accounts matching this lead schedule
    accounts = get_accounts_for_lead(client, config)

    # Calculate balances
    prior_balance = Decimal("0")
    current_balance = Decimal("0")

    account_details = []
    for acct in accounts:
        prior_bal = client.get_account_balance_as_of(acct.id, prior_date)
        current_bal = float(acct.balance or 0)

        prior_balance += Decimal(str(prior_bal))
        current_balance += Decimal(str(current_bal))

        account_details.append({
            "id": acct.id,
            "number": acct.number,
            "name": acct.name,
            "prior_balance": prior_bal,
            "current_balance": current_bal
        })

    # Apply adjustments
    adj_total = Decimal("0")
    if adjustments:
        for adj in adjustments:
            adj_total += Decimal(str(adj.get("amount", 0)))

    return {
        "reference": lead_ref,
        "title": config["title"],
        "prior_year": float(prior_balance),
        "adjustments": adjustments or [],
        "adjustment_total": float(adj_total),
        "current_year": float(current_balance),
        "per_audit": float(current_balance + adj_total),
        "accounts": account_details,
        "supporting_schedules": config["supporting"]
    }


def get_accounts_for_lead(client, config: dict) -> list:
    """
    Get accounts matching lead schedule criteria.

    The client must implement get_accounts() returning objects with
    id, number, name, active, type, and balance attributes.
    """
    import re

    types = config.get("account_types", [])
    patterns = config.get("account_patterns", [])

    accounts = client.get_accounts()
    matching = []

    for acct in accounts:
        if not acct.active:
            continue

        if acct.type not in types:
            continue

        # If patterns specified, account name must match
        if patterns:
            name_lower = acct.name.lower()
            if not any(re.search(p, name_lower) for p in patterns):
                continue

        matching.append(acct)

    return matching
```

---

## Tickmark Conventions

### Standard Tickmarks

```python
from openpyxl.styles import Font

TICKMARKS = {
    "^": "Footed and cross-footed",
    "T": "Traced to trial balance",
    "S": "Traced to subsidiary ledger",
    "V": "Vouched to supporting documentation",
    "C": "Confirmed",
    "R": "Recalculated",
    "I": "Inspected",
    "O": "Observed",
    "@": "Agreed to prior year workpapers",
    "PY": "Per prior year audit",
    "TB": "Agreed to trial balance",
    "GL": "Traced to general ledger",
    "BS": "Traced to bank statement",
    "BR": "Agreed to bank reconciliation",
    "INV": "Traced to invoice",
    "CTR": "Traced to contract",
    "N/A": "Not applicable",
    "NTE": "See note",
}


def add_tickmark_legend(workbook, sheet_name: str = "Tickmarks"):
    """Add tickmark legend sheet to workbook."""
    ws = workbook.create_sheet(title=sheet_name)

    ws["A1"] = "Tickmark Legend"
    ws["A1"].font = Font(bold=True, size=12)

    row = 3
    for mark, meaning in TICKMARKS.items():
        ws.cell(row=row, column=1, value=mark).font = Font(bold=True)
        ws.cell(row=row, column=2, value=meaning)
        row += 1

    ws.column_dimensions["A"].width = 10
    ws.column_dimensions["B"].width = 40
```

---

## Complete Workpaper Package

```python
def generate_audit_workpaper_package(
    client,
    company_name: str,
    current_year_end: str,
    prior_year_end: str,
    output_dir: str
) -> dict:
    """
    Generate complete audit workpaper package.

    Creates:
    - Trial balance
    - PBC request list
    - Lead schedules for all categories
    - Tickmark legend
    """
    from pathlib import Path

    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)

    package = {
        "company": company_name,
        "current_year_end": current_year_end,
        "prior_year_end": prior_year_end,
        "files": []
    }

    # Trial Balance
    tb_rows = extract_trial_balance(client, current_year_end)
    tb_file = str(output_path / f"TB_{current_year_end}.xlsx")
    format_trial_balance_excel(tb_rows, current_year_end, company_name, tb_file)
    package["files"].append({"type": "trial_balance", "path": tb_file})

    # PBC List
    pbc = generate_pbc_list(company_name, current_year_end)
    pbc_file = str(output_path / f"PBC_{current_year_end}.xlsx")
    export_pbc_to_excel(pbc, pbc_file)
    package["files"].append({"type": "pbc_list", "path": pbc_file})

    # Lead Schedules
    for lead_ref in LEAD_SCHEDULE_MAPPINGS.keys():
        lead = generate_lead_schedule(
            client, lead_ref, current_year_end, prior_year_end
        )
        lead_file = str(output_path / f"Lead_{lead_ref}_{current_year_end}.json")
        with open(lead_file, "w") as f:
            json.dump(lead, f, indent=2, default=str)
        package["files"].append({"type": "lead_schedule", "reference": lead_ref, "path": lead_file})

    return package
```

---

## Best Practices

1. **Use consistent references** - A, B, C for BS; PL-R, PL-E for P&L
2. **Cross-reference everything** - Link supporting to lead schedules
3. **Document procedures** - Note what was done, not just the result
4. **Date all workpapers** - When prepared and reviewed
5. **Initial all work** - Preparer and reviewer initials
6. **Keep tickmarks consistent** - Use standard legend across engagement
7. **Highlight significant items** - Variance analysis, unusual items

---

## See Also

- [audit-pbc-lists.md](audit-pbc-lists.md) - PBC list generation
- [audit-aje-patterns.md](audit-aje-patterns.md) - Adjusting Journal Entry patterns
