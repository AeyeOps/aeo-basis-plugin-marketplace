# Audit PBC Lists

PBC (Prepared by Client) list generation and tracking for CPA audits.

## Contents

- [Overview](#overview)
- [Standard PBC Categories](#standard-pbc-categories)
- [Generate PBC List](#generate-pbc-list)
- [Excel Export](#excel-export)
- [See Also](#see-also)

---

## Overview

PBC lists document items the client must provide to auditors. This reference covers:
- Standard PBC categories and items
- Generating PBC request lists
- Status tracking
- Excel export for client distribution

---

## Standard PBC Categories

```python
from dataclasses import dataclass
from datetime import date

PBC_CATEGORIES = {
    "general": {
        "title": "General",
        "items": [
            "Trial balance as of year-end",
            "Bank reconciliations for all accounts (year-end)",
            "Bank statements (full year + subsequent month)",
            "General ledger detail for all accounts",
            "Chart of accounts with account numbers",
            "Minutes of board meetings",
            "Articles of incorporation and bylaws",
            "Organization chart",
        ]
    },
    "cash": {
        "title": "Cash and Investments",
        "items": [
            "Bank reconciliations for all accounts",
            "Bank statements (12 months + January)",
            "Investment statements (all accounts)",
            "Brokerage confirmations for purchases/sales",
            "Outstanding check list",
            "Deposit in transit detail",
            "Wire transfer documentation",
        ]
    },
    "receivables": {
        "title": "Accounts Receivable",
        "items": [
            "Aged accounts receivable listing",
            "Subsequent collections report",
            "Bad debt write-offs (detail and authorization)",
            "Credit memo listing",
            "Revenue recognition policy",
            "Major customer contracts",
        ]
    },
    "inventory": {
        "title": "Inventory (if applicable)",
        "items": [
            "Inventory listing with quantities and costs",
            "Inventory count procedures",
            "Obsolete inventory analysis",
            "Cost flow assumption documentation (FIFO, LIFO, etc.)",
        ]
    },
    "fixed_assets": {
        "title": "Fixed Assets",
        "items": [
            "Fixed asset register/schedule",
            "Additions and disposals detail",
            "Depreciation schedule",
            "Invoices for major additions (>$5,000)",
            "Capital vs expense policy",
        ]
    },
    "payables": {
        "title": "Accounts Payable",
        "items": [
            "Aged accounts payable listing",
            "Vendor reconciliations (major vendors)",
            "Subsequent disbursements report",
            "Unrecorded liabilities search results",
        ]
    },
    "debt": {
        "title": "Debt and Equity",
        "items": [
            "Loan agreements and amendments",
            "Debt amortization schedules",
            "Interest rate documentation",
            "Stock ledger / cap table",
            "Shareholder agreement",
            "Dividend declarations",
        ]
    },
    "payroll": {
        "title": "Payroll",
        "items": [
            "Payroll registers (all periods)",
            "W-2 reconciliation to GL",
            "401(k) contribution reconciliation",
            "Payroll tax returns (941s)",
            "Workers compensation policy",
        ]
    },
    "tax": {
        "title": "Taxes",
        "items": [
            "Prior year tax returns (federal and state)",
            "Tax provision calculation",
            "Estimated tax payments",
            "Sales tax returns",
            "Property tax bills",
        ]
    },
    "related_party": {
        "title": "Related Party",
        "items": [
            "Related party transaction listing",
            "Intercompany account reconciliations",
            "Officer/shareholder loan agreements",
            "Lease agreements with related parties",
        ]
    },
}
```

---

## Generate PBC List

```python
def generate_pbc_list(
    company_name: str,
    fiscal_year_end: str,
    categories: list[str] = None,
    custom_items: dict = None,
    output_path: str = None
) -> dict:
    """
    Generate PBC request list.

    Args:
        company_name: Client company name
        fiscal_year_end: Year-end date (YYYY-MM-DD)
        categories: List of category keys to include (None = all)
        custom_items: Additional items by category
        output_path: Optional Excel output path

    Returns:
        PBC list as dict
    """
    pbc = {
        "company": company_name,
        "year_end": fiscal_year_end,
        "generated": date.today().isoformat(),
        "categories": {}
    }

    cats_to_include = categories or list(PBC_CATEGORIES.keys())

    for cat_key in cats_to_include:
        if cat_key not in PBC_CATEGORIES:
            continue

        cat = PBC_CATEGORIES[cat_key]
        items = [{"item": i, "status": "pending", "notes": ""} for i in cat["items"]]

        # Add custom items
        if custom_items and cat_key in custom_items:
            for custom in custom_items[cat_key]:
                items.append({"item": custom, "status": "pending", "notes": ""})

        pbc["categories"][cat_key] = {
            "title": cat["title"],
            "items": items
        }

    if output_path:
        export_pbc_to_excel(pbc, output_path)

    return pbc
```

---

## Excel Export

```python
def export_pbc_to_excel(pbc: dict, output_path: str):
    """Export PBC list to Excel with status tracking columns."""
    from openpyxl import Workbook
    from openpyxl.styles import Font, PatternFill, Border, Side

    wb = Workbook()
    ws = wb.active
    ws.title = "PBC Request List"

    # Header
    ws["A1"] = f"PBC Request List - {pbc['company']}"
    ws["A1"].font = Font(bold=True, size=14)
    ws["A2"] = f"Fiscal Year End: {pbc['year_end']}"

    row = 4
    for cat_key, cat in pbc["categories"].items():
        # Category header
        ws.cell(row=row, column=1, value=cat["title"]).font = Font(bold=True, size=12)
        ws.merge_cells(start_row=row, start_column=1, end_row=row, end_column=5)
        row += 1

        # Column headers
        headers = ["#", "Item", "Status", "Received Date", "Notes"]
        for col, header in enumerate(headers, 1):
            cell = ws.cell(row=row, column=col, value=header)
            cell.font = Font(bold=True)
            cell.fill = PatternFill(start_color="DDDDDD", fill_type="solid")
        row += 1

        # Items
        for idx, item in enumerate(cat["items"], 1):
            ws.cell(row=row, column=1, value=idx)
            ws.cell(row=row, column=2, value=item["item"])
            ws.cell(row=row, column=3, value=item["status"])
            ws.cell(row=row, column=4, value="")  # Received date
            ws.cell(row=row, column=5, value=item["notes"])
            row += 1

        row += 1  # Blank row between categories

    # Column widths
    ws.column_dimensions["A"].width = 5
    ws.column_dimensions["B"].width = 50
    ws.column_dimensions["C"].width = 12
    ws.column_dimensions["D"].width = 15
    ws.column_dimensions["E"].width = 30

    wb.save(output_path)
```

---

## Status Tracking

PBC item statuses:

| Status | Meaning |
|--------|---------|
| `pending` | Not yet received |
| `requested` | Follow-up sent |
| `received` | Document received, not yet reviewed |
| `reviewed` | Reviewed, no issues |
| `issue` | Issues identified, follow-up needed |
| `n/a` | Not applicable to this engagement |

---

## See Also

- [audit-lead-schedules.md](audit-lead-schedules.md) - Lead schedules with standard references
- [audit-aje-patterns.md](audit-aje-patterns.md) - Adjusting Journal Entry patterns
