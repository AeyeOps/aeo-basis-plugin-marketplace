# Audit RJE Patterns

Reclassifying Journal Entry (RJE) patterns, posting adjustments to QBO, and summary generation.

## Contents

- [Overview](#overview)
- [Reclassifying Entries (RJEs)](#reclassifying-entries-rjes)
- [Post Adjustments to QBO](#post-adjustments-to-qbo)
- [Adjustment Summary](#adjustment-summary)
- [Materiality and Passed Adjustments](#materiality-and-passed-adjustments)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

This reference covers:
- **RJE (Reclassifying Journal Entry)**: Presentation-only adjustments
- **Posting AJEs to QBO**: Creating journal entries from audit adjustments
- **Adjustment summaries**: Tracking and reporting all adjustments
- **Passed adjustments**: Materiality evaluation for management representation

---

## Reclassifying Entries (RJEs)

RJEs affect presentation only, not the underlying books.

### Current Portion of Long-Term Debt

```python
from audit_aje_patterns import AuditAdjustment, AdjustmentType, AdjustmentLine
from decimal import Decimal

def create_current_lt_debt_rje(
    current_portion: Decimal,
    lt_debt_account_id: str,
    current_portion_account_id: str
) -> AuditAdjustment:
    """
    RJE to reclassify current portion of long-term debt.

    Presentation only - shows proper current/long-term split.
    """
    return AuditAdjustment(
        reference="RJE-DEBT",
        type=AdjustmentType.RJE,
        description="Reclassify current portion of LT debt",
        workpaper_ref="CC-1",
        lines=[
            AdjustmentLine(
                account_id=lt_debt_account_id,
                account_name="Long-Term Debt",
                debit=current_portion,
                description="Reduce LT debt"
            ),
            AdjustmentLine(
                account_id=current_portion_account_id,
                account_name="Current Portion of LT Debt",
                credit=current_portion,
                description="Record current portion"
            )
        ]
    )
```

### Gross vs Net Presentation

```python
def create_contra_reclassification_rje(
    amount: Decimal,
    gross_account_id: str,
    contra_account_id: str,
    net_account_id: str,
    description: str
) -> AuditAdjustment:
    """
    RJE to present gross/contra vs net.

    Example: Show gross AR and Allowance separately instead of net AR.
    """
    return AuditAdjustment(
        reference="RJE-CONTRA",
        type=AdjustmentType.RJE,
        description=description,
        workpaper_ref="",
        lines=[
            AdjustmentLine(
                account_id=gross_account_id,
                account_name="Gross Amount",
                debit=amount,
                description="Gross presentation"
            ),
            AdjustmentLine(
                account_id=contra_account_id,
                account_name="Contra Account",
                credit=amount,
                description="Contra presentation"
            )
        ]
    )
```

---

## Post Adjustments to QBO

```python
from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine
from datetime import date

def post_adjustment_to_qbo(
    client,
    adjustment: AuditAdjustment,
    post_date: str
) -> JournalEntry:
    """
    Post audit adjustment to QBO.

    Only AJEs should be posted - RJEs are for presentation only.
    PJEs are passed adjustments and should not be posted.

    Args:
        client: QuickBooks client
        adjustment: The adjustment to post
        post_date: Date for the journal entry

    Returns:
        Created JournalEntry
    """
    if adjustment.type != AdjustmentType.AJE:
        raise ValueError(f"Only AJEs should be posted. Got {adjustment.type.value}")

    if not adjustment.is_balanced:
        raise ValueError(
            f"Adjustment {adjustment.reference} is not balanced. "
            f"Debits: {adjustment.total_debits}, Credits: {adjustment.total_credits}"
        )

    je = JournalEntry()
    je.TxnDate = post_date
    je.DocNumber = adjustment.reference
    je.PrivateNote = f"Audit adjustment: {adjustment.description} (WP: {adjustment.workpaper_ref})"

    lines = []
    for adj_line in adjustment.lines:
        je_line = JournalEntryLine()
        je_line.Description = adj_line.description

        if adj_line.debit > 0:
            je_line.Amount = float(adj_line.debit)
            je_line.JournalEntryLineDetail = {
                "PostingType": "Debit",
                "AccountRef": {"value": adj_line.account_id}
            }
        else:
            je_line.Amount = float(adj_line.credit)
            je_line.JournalEntryLineDetail = {
                "PostingType": "Credit",
                "AccountRef": {"value": adj_line.account_id}
            }

        lines.append(je_line)

    je.Line = lines
    je.save(qb=client)

    # Update adjustment with QBO reference
    adjustment.qbo_je_id = je.Id
    adjustment.date_posted = date.today()

    return je


def post_all_ajes(
    client,
    adjustments: list[AuditAdjustment],
    post_date: str
) -> list[dict]:
    """
    Post all AJEs to QBO.

    Returns list of results with success/failure status.
    """
    results = []

    for adj in adjustments:
        if adj.type != AdjustmentType.AJE:
            results.append({
                "reference": adj.reference,
                "status": "skipped",
                "reason": f"Not an AJE ({adj.type.value})"
            })
            continue

        try:
            je = post_adjustment_to_qbo(client, adj, post_date)
            results.append({
                "reference": adj.reference,
                "status": "posted",
                "qbo_je_id": je.Id
            })
        except Exception as e:
            results.append({
                "reference": adj.reference,
                "status": "failed",
                "error": str(e)
            })

    return results
```

---

## Adjustment Summary

```python
def generate_adjustment_summary(
    adjustments: list[AuditAdjustment],
    output_path: str = None
) -> dict:
    """
    Generate summary of all audit adjustments.

    Groups by type and calculates totals by account.
    """
    summary = {
        "aje_count": 0,
        "rje_count": 0,
        "pje_count": 0,
        "total_aje_debits": Decimal("0"),
        "total_aje_credits": Decimal("0"),
        "adjustments": [],
        "by_account": {},
        "passed_adjustments": []
    }

    for adj in adjustments:
        if adj.type == AdjustmentType.AJE:
            summary["aje_count"] += 1
            summary["total_aje_debits"] += adj.total_debits
            summary["total_aje_credits"] += adj.total_credits
        elif adj.type == AdjustmentType.RJE:
            summary["rje_count"] += 1
        else:
            summary["pje_count"] += 1
            summary["passed_adjustments"].append({
                "reference": adj.reference,
                "description": adj.description,
                "amount": float(adj.total_debits)
            })

        # Track by account
        for line in adj.lines:
            acct = line.account_name
            if acct not in summary["by_account"]:
                summary["by_account"][acct] = {
                    "debits": Decimal("0"),
                    "credits": Decimal("0")
                }
            summary["by_account"][acct]["debits"] += line.debit
            summary["by_account"][acct]["credits"] += line.credit

        summary["adjustments"].append({
            "reference": adj.reference,
            "type": adj.type.value,
            "description": adj.description,
            "workpaper": adj.workpaper_ref,
            "debits": float(adj.total_debits),
            "credits": float(adj.total_credits),
            "posted": adj.qbo_je_id is not None
        })

    if output_path:
        export_adjustment_summary_excel(summary, adjustments, output_path)

    return summary


def export_adjustment_summary_excel(
    summary: dict,
    adjustments: list[AuditAdjustment],
    output_path: str
):
    """Export adjustments to Excel with detail and summary sheets."""
    from openpyxl import Workbook
    from openpyxl.styles import Font, PatternFill

    wb = Workbook()

    # Summary sheet
    ws_sum = wb.active
    ws_sum.title = "Summary"
    ws_sum["A1"] = "Audit Adjustment Summary"
    ws_sum["A1"].font = Font(bold=True, size=14)

    ws_sum["A3"] = "AJE Count:"
    ws_sum["B3"] = summary["aje_count"]
    ws_sum["A4"] = "RJE Count:"
    ws_sum["B4"] = summary["rje_count"]
    ws_sum["A5"] = "Passed (PJE) Count:"
    ws_sum["B5"] = summary["pje_count"]

    # Detail sheet
    ws_det = wb.create_sheet("Adjustments")
    headers = ["Ref", "Type", "Description", "WP Ref", "Debits", "Credits", "Posted"]
    for col, header in enumerate(headers, 1):
        ws_det.cell(row=1, column=col, value=header).font = Font(bold=True)

    for row, adj in enumerate(summary["adjustments"], 2):
        ws_det.cell(row=row, column=1, value=adj["reference"])
        ws_det.cell(row=row, column=2, value=adj["type"])
        ws_det.cell(row=row, column=3, value=adj["description"])
        ws_det.cell(row=row, column=4, value=adj["workpaper"])
        ws_det.cell(row=row, column=5, value=adj["debits"])
        ws_det.cell(row=row, column=6, value=adj["credits"])
        ws_det.cell(row=row, column=7, value="Yes" if adj["posted"] else "No")

    # By Account sheet
    ws_acct = wb.create_sheet("By Account")
    ws_acct["A1"] = "Account"
    ws_acct["B1"] = "Total Debits"
    ws_acct["C1"] = "Total Credits"
    ws_acct["D1"] = "Net Effect"

    row = 2
    for acct, vals in summary["by_account"].items():
        ws_acct.cell(row=row, column=1, value=acct)
        ws_acct.cell(row=row, column=2, value=float(vals["debits"]))
        ws_acct.cell(row=row, column=3, value=float(vals["credits"]))
        ws_acct.cell(row=row, column=4, value=float(vals["debits"] - vals["credits"]))
        row += 1

    wb.save(output_path)
```

---

## Materiality and Passed Adjustments

```python
from dataclasses import dataclass

@dataclass
class MaterialityThresholds:
    """Audit materiality thresholds."""
    overall_materiality: Decimal
    performance_materiality: Decimal
    trivial_threshold: Decimal  # Below this = clearly trivial


def evaluate_passed_adjustments(
    passed: list[AuditAdjustment],
    materiality: MaterialityThresholds
) -> dict:
    """
    Evaluate passed adjustments against materiality.

    Returns summary for management representation letter.
    """
    total_passed = sum(adj.total_debits for adj in passed)

    return {
        "passed_count": len(passed),
        "total_amount": float(total_passed),
        "overall_materiality": float(materiality.overall_materiality),
        "percent_of_materiality": float((total_passed / materiality.overall_materiality) * 100)
            if materiality.overall_materiality > 0 else 0,
        "exceeds_materiality": total_passed > materiality.overall_materiality,
        "adjustments": [
            {
                "reference": adj.reference,
                "description": adj.description,
                "amount": float(adj.total_debits)
            }
            for adj in passed
        ],
        "management_rep_required": total_passed > materiality.trivial_threshold
    }
```

---

## Best Practices

1. **Number sequentially** - AJE-1, AJE-2, etc. within each engagement
2. **Document workpaper reference** - Link every adjustment to supporting workpaper
3. **Balance before posting** - Verify debits = credits
4. **Only post AJEs** - RJEs and PJEs don't affect books
5. **Track passed adjustments** - Required for management representation letter
6. **Review all entries** - Second review before posting
7. **Maintain adjustment log** - Keep master list across all years

---

## See Also

- [audit-aje-patterns.md](audit-aje-patterns.md) - Adjusting Journal Entry patterns
- [audit-trial-balance.md](audit-trial-balance.md) - Trial balance extraction
- [audit-lead-schedules.md](audit-lead-schedules.md) - Lead schedules
