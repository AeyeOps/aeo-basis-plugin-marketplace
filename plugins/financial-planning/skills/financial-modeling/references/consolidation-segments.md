---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Segment Reporting and NCI

Segment reporting, non-controlling interest calculations, and consolidation package export.

## Contents

- [Overview](#overview)
- [Segment Definition](#segment-definition)
- [Segment Report Generation](#segment-report-generation)
- [Non-Controlling Interest (NCI)](#non-controlling-interest-nci)
- [Export Consolidation Package](#export-consolidation-package)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

This reference covers:
- Operating segment definitions
- Segment-level financial reporting
- Non-controlling interest (minority interest) calculations
- Complete consolidation package export

---

## Segment Definition

```python
from dataclasses import dataclass
from decimal import Decimal

@dataclass
class ReportingSegment:
    """Operating segment for segment reporting."""
    segment_id: str
    segment_name: str
    entities: list[str]  # Entity IDs in this segment
    revenue_threshold_pct: Decimal = Decimal("10")  # Reportable if > 10%


def define_segments(
    entities: list[dict],
    segment_assignments: dict[str, str]
) -> list[ReportingSegment]:
    """
    Define reporting segments based on entity assignments.

    Args:
        entities: List of entity dicts
        segment_assignments: Dict of entity_id -> segment_id
    """
    segments = {}

    for entity in entities:
        entity_id = entity["id"]
        segment_id = segment_assignments.get(entity_id, "Other")

        if segment_id not in segments:
            segments[segment_id] = ReportingSegment(
                segment_id=segment_id,
                segment_name=segment_id,
                entities=[]
            )

        segments[segment_id].entities.append(entity_id)

    return list(segments.values())
```

---

## Segment Report Generation

```python
def generate_segment_report(
    combined_tb: list,
    segments: list[ReportingSegment],
    period: dict
) -> dict:
    """
    Generate segment financial data.

    Returns segment-level revenue, operating income, and assets.
    """
    segment_data = {}

    for segment in segments:
        segment_data[segment.segment_id] = {
            "name": segment.segment_name,
            "revenue": Decimal("0"),
            "operating_expenses": Decimal("0"),
            "operating_income": Decimal("0"),
            "depreciation": Decimal("0"),
            "capital_expenditures": Decimal("0"),
            "total_assets": Decimal("0"),
            "entities": segment.entities
        }

    # Aggregate by segment
    for row in combined_tb:
        # Find segment for this entity
        segment_id = None
        for segment in segments:
            if row.entity_id in segment.entities:
                segment_id = segment.segment_id
                break

        if not segment_id:
            continue

        data = segment_data[segment_id]
        balance = row.debit - row.credit

        if row.classification == "Asset":
            data["total_assets"] += abs(balance)
        elif row.classification == "Revenue":
            data["revenue"] += row.credit - row.debit
        elif row.classification == "Expense":
            data["operating_expenses"] += balance

            # Track depreciation separately
            if "depreciation" in row.account_name.lower():
                data["depreciation"] += balance

    # Calculate operating income
    for segment_id, data in segment_data.items():
        data["operating_income"] = data["revenue"] - data["operating_expenses"]

    # Convert to float
    for segment_id, data in segment_data.items():
        for key in ["revenue", "operating_expenses", "operating_income",
                    "depreciation", "capital_expenditures", "total_assets"]:
            data[key] = float(data[key])

    # Calculate totals
    totals = {
        "revenue": sum(d["revenue"] for d in segment_data.values()),
        "operating_income": sum(d["operating_income"] for d in segment_data.values()),
        "total_assets": sum(d["total_assets"] for d in segment_data.values())
    }

    return {
        "period": period,
        "segments": segment_data,
        "totals": totals,
        "reconciliation": {}  # Would include corporate/unallocated items
    }
```

---

## Non-Controlling Interest (NCI)

### Calculate NCI

```python
def calculate_non_controlling_interest(
    subsidiary_net_income: Decimal,
    subsidiary_equity: Decimal,
    parent_ownership_pct: Decimal
) -> dict:
    """
    Calculate non-controlling interest (minority interest).

    For less than 100% owned subsidiaries.
    """
    nci_pct = Decimal("1") - parent_ownership_pct

    nci_share_of_income = subsidiary_net_income * nci_pct
    nci_share_of_equity = subsidiary_equity * nci_pct

    return {
        "parent_ownership_pct": float(parent_ownership_pct * 100),
        "nci_pct": float(nci_pct * 100),
        "subsidiary_net_income": float(subsidiary_net_income),
        "nci_share_of_income": float(nci_share_of_income),
        "subsidiary_equity": float(subsidiary_equity),
        "nci_share_of_equity": float(nci_share_of_equity)
    }
```

### Create NCI Entries

```python
def create_nci_entries(
    subsidiary_entity: str,
    parent_entity: str,
    nci_income: Decimal,
    nci_equity: Decimal,
    nci_income_account_id: str,
    nci_equity_account_id: str
) -> list[dict]:
    """
    Create consolidation entries for NCI.

    1. Allocate NCI share of income
    2. Record NCI equity balance
    """
    entries = []

    # NCI share of income
    if nci_income != 0:
        entries.append({
            "type": "nci_income",
            "description": "Allocate NCI share of subsidiary income",
            "debits": [(nci_income_account_id, nci_income)],
            "credits": []  # Offset by reducing parent's share
        })

    # NCI equity
    entries.append({
        "type": "nci_equity",
        "description": "Record NCI share of subsidiary equity",
        "debits": [],
        "credits": [(nci_equity_account_id, nci_equity)]
    })

    return entries
```

---

## Export Consolidation Package

```python
def export_consolidation_package(
    group_name: str,
    as_of_date: str,
    combined_tb: list,
    eliminations: list[dict],
    segments: list[ReportingSegment],
    output_dir: str
) -> dict:
    """
    Export complete consolidation package.

    Creates:
    - Combined trial balance
    - Elimination schedule
    - Consolidated balance sheet
    - Consolidated income statement
    - Segment report
    """
    from pathlib import Path
    from openpyxl import Workbook
    from openpyxl.styles import Font

    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)

    wb = Workbook()

    # Combined TB sheet
    ws_tb = wb.active
    ws_tb.title = "Combined TB"
    ws_tb["A1"] = "Combined Trial Balance"
    ws_tb["A1"].font = Font(bold=True, size=14)

    headers = ["Entity", "Account #", "Account Name", "Debit", "Credit", "Mapped Account"]
    for col, header in enumerate(headers, 1):
        ws_tb.cell(row=3, column=col, value=header).font = Font(bold=True)

    row = 4
    for tb_row in combined_tb:
        ws_tb.cell(row=row, column=1, value=tb_row.entity_name)
        ws_tb.cell(row=row, column=2, value=tb_row.account_number)
        ws_tb.cell(row=row, column=3, value=tb_row.account_name)
        ws_tb.cell(row=row, column=4, value=float(tb_row.debit))
        ws_tb.cell(row=row, column=5, value=float(tb_row.credit))
        ws_tb.cell(row=row, column=6, value=tb_row.mapped_account or "")
        row += 1

    # Eliminations sheet
    ws_elim = wb.create_sheet("Eliminations")
    ws_elim["A1"] = "Elimination Entries"
    ws_elim["A1"].font = Font(bold=True, size=14)

    headers = ["Type", "Description", "Debit Account", "Debit Amount", "Credit Account", "Credit Amount"]
    for col, header in enumerate(headers, 1):
        ws_elim.cell(row=3, column=col, value=header).font = Font(bold=True)

    row = 4
    for elim in eliminations:
        ws_elim.cell(row=row, column=1, value=elim.get("type", ""))
        ws_elim.cell(row=row, column=2, value=elim.get("description", ""))
        debits = elim.get("debits", [])
        credits = elim.get("credits", [])
        if debits:
            ws_elim.cell(row=row, column=3, value=debits[0][0])
            ws_elim.cell(row=row, column=4, value=float(debits[0][1]))
        if credits:
            ws_elim.cell(row=row, column=5, value=credits[0][0])
            ws_elim.cell(row=row, column=6, value=float(credits[0][1]))
        row += 1

    # Segment sheet
    ws_seg = wb.create_sheet("Segment Report")
    ws_seg["A1"] = "Segment Report"
    ws_seg["A1"].font = Font(bold=True, size=14)

    headers = ["Segment", "Revenue", "Operating Expenses", "Operating Income", "Total Assets"]
    for col, header in enumerate(headers, 1):
        ws_seg.cell(row=3, column=col, value=header).font = Font(bold=True)

    row = 4
    for segment in segments:
        ws_seg.cell(row=row, column=1, value=segment.segment_name)
        row += 1

    file_path = output_path / f"Consolidation_{group_name}_{as_of_date}.xlsx"
    wb.save(str(file_path))

    return {
        "group_name": group_name,
        "as_of_date": as_of_date,
        "file_path": str(file_path),
        "sheets": ["Combined TB", "Eliminations", "Segment Report"]
    }
```

---

## Best Practices

1. **Standardize COA** - Map all entities to consolidated chart of accounts
2. **Eliminate consistently** - Same elimination methodology each period
3. **Document NCI calculations** - Ownership percentages and allocations
4. **Reconcile eliminations** - Total eliminations should net to zero
5. **Track intercompany by segment** - For segment elimination reporting
6. **Maintain mapping tables** - Entity account to consolidated account mappings
7. **Version control** - Track consolidation workbook versions
8. **Review minority interest** - Recalculate when ownership changes

---

## See Also

- [consolidation-statements.md](consolidation-statements.md) - Combined TB and consolidated statements
- [consolidation-intercompany.md](consolidation-intercompany.md) - Intercompany tracking and eliminations
