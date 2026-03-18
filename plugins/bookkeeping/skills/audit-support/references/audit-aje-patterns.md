---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Audit AJE Patterns

Adjusting Journal Entry (AJE) patterns for common audit adjustments.

## Contents

- [Overview](#overview)
- [Adjustment Data Structures](#adjustment-data-structures)
- [Revenue Recognition](#revenue-recognition)
- [Accrued Expenses](#accrued-expenses)
- [Bad Debt](#bad-debt)
- [Fixed Assets](#fixed-assets)
- [See Also](#see-also)

---

## Overview

AJEs (Adjusting Journal Entries) correct account balances and affect the books. Common categories:
- Revenue recognition adjustments
- Accrued expense adjustments
- Bad debt and allowance adjustments
- Fixed asset and depreciation adjustments

---

## Adjustment Data Structures

```python
from dataclasses import dataclass, field
from decimal import Decimal
from datetime import date
from typing import Optional
from enum import Enum

class AdjustmentType(Enum):
    AJE = "AJE"  # Adjusting - affects books
    RJE = "RJE"  # Reclassifying - presentation only
    PJE = "PJE"  # Proposed/Passed - not posted


@dataclass
class AdjustmentLine:
    """Single line of an adjustment entry."""
    account_id: str
    account_name: str
    debit: Decimal = Decimal("0")
    credit: Decimal = Decimal("0")
    description: str = ""


@dataclass
class AuditAdjustment:
    """Complete audit adjustment entry."""
    reference: str  # e.g., "AJE-1", "RJE-3"
    type: AdjustmentType
    description: str
    workpaper_ref: str  # e.g., "C-1", "E-2"
    lines: list[AdjustmentLine]
    prepared_by: str = ""
    reviewed_by: str = ""
    date_prepared: Optional[date] = None
    date_posted: Optional[date] = None
    platform_je_id: Optional[str] = None  # Platform JE ID if posted

    @property
    def total_debits(self) -> Decimal:
        return sum(line.debit for line in self.lines)

    @property
    def total_credits(self) -> Decimal:
        return sum(line.credit for line in self.lines)

    @property
    def is_balanced(self) -> bool:
        return self.total_debits == self.total_credits
```

---

## Revenue Recognition

### Defer Unearned Revenue

```python
def create_deferred_revenue_aje(
    unearned_amount: Decimal,
    revenue_account_id: str,
    deferred_revenue_account_id: str,
    description: str = "Defer unearned revenue"
) -> AuditAdjustment:
    """
    AJE to defer revenue recognized prematurely.

    Debit: Revenue (reduce)
    Credit: Deferred Revenue (liability)
    """
    return AuditAdjustment(
        reference="AJE-REV",
        type=AdjustmentType.AJE,
        description=description,
        workpaper_ref="PL-R-1",
        lines=[
            AdjustmentLine(
                account_id=revenue_account_id,
                account_name="Revenue",
                debit=unearned_amount,
                description="Reduce prematurely recognized revenue"
            ),
            AdjustmentLine(
                account_id=deferred_revenue_account_id,
                account_name="Deferred Revenue",
                credit=unearned_amount,
                description="Record deferred revenue liability"
            )
        ]
    )
```

### Accrue Unbilled Revenue

```python
def create_unbilled_revenue_aje(
    unbilled_amount: Decimal,
    revenue_account_id: str,
    unbilled_ar_account_id: str,
    description: str = "Accrue unbilled revenue"
) -> AuditAdjustment:
    """
    AJE to accrue revenue earned but not yet billed.

    Debit: Unbilled Accounts Receivable
    Credit: Revenue
    """
    return AuditAdjustment(
        reference="AJE-REV",
        type=AdjustmentType.AJE,
        description=description,
        workpaper_ref="C-1",
        lines=[
            AdjustmentLine(
                account_id=unbilled_ar_account_id,
                account_name="Unbilled Accounts Receivable",
                debit=unbilled_amount,
                description="Unbilled revenue receivable"
            ),
            AdjustmentLine(
                account_id=revenue_account_id,
                account_name="Revenue",
                credit=unbilled_amount,
                description="Revenue earned not yet billed"
            )
        ]
    )
```

---

## Accrued Expenses

### Accrue Expense

```python
def create_accrued_expense_aje(
    expense_type: str,
    amount: Decimal,
    expense_account_id: str,
    accrued_liability_account_id: str
) -> AuditAdjustment:
    """
    AJE to accrue expenses incurred but not recorded.

    Debit: Expense
    Credit: Accrued Liability
    """
    return AuditAdjustment(
        reference="AJE-EXP",
        type=AdjustmentType.AJE,
        description=f"Accrue {expense_type}",
        workpaper_ref="BB-1",
        lines=[
            AdjustmentLine(
                account_id=expense_account_id,
                account_name=expense_type,
                debit=amount,
                description=f"Accrue {expense_type} expense"
            ),
            AdjustmentLine(
                account_id=accrued_liability_account_id,
                account_name="Accrued Liabilities",
                credit=amount,
                description=f"Accrued {expense_type}"
            )
        ]
    )
```

### Reclassify to Prepaid

```python
def create_prepaid_expense_aje(
    expense_type: str,
    amount: Decimal,
    expense_account_id: str,
    prepaid_account_id: str
) -> AuditAdjustment:
    """
    AJE to reclassify expense to prepaid.

    Debit: Prepaid Expense (asset)
    Credit: Expense (reduce)
    """
    return AuditAdjustment(
        reference="AJE-PRE",
        type=AdjustmentType.AJE,
        description=f"Reclassify {expense_type} to prepaid",
        workpaper_ref="D-1",
        lines=[
            AdjustmentLine(
                account_id=prepaid_account_id,
                account_name="Prepaid Expenses",
                debit=amount,
                description=f"Prepaid {expense_type}"
            ),
            AdjustmentLine(
                account_id=expense_account_id,
                account_name=expense_type,
                credit=amount,
                description=f"Reduce {expense_type} expense"
            )
        ]
    )
```

---

## Bad Debt

```python
def create_bad_debt_aje(
    amount: Decimal,
    bad_debt_expense_account_id: str,
    allowance_account_id: str,
    specific_writeoff: bool = False,
    ar_account_id: str = None
) -> AuditAdjustment:
    """
    AJE for bad debt allowance or write-off.

    Allowance method:
        Debit: Bad Debt Expense
        Credit: Allowance for Doubtful Accounts

    Direct write-off:
        Debit: Bad Debt Expense
        Credit: Accounts Receivable
    """
    if specific_writeoff:
        return AuditAdjustment(
            reference="AJE-BD",
            type=AdjustmentType.AJE,
            description="Write off uncollectible receivable",
            workpaper_ref="C-3",
            lines=[
                AdjustmentLine(
                    account_id=bad_debt_expense_account_id,
                    account_name="Bad Debt Expense",
                    debit=amount,
                    description="Bad debt write-off"
                ),
                AdjustmentLine(
                    account_id=ar_account_id,
                    account_name="Accounts Receivable",
                    credit=amount,
                    description="Write off uncollectible AR"
                )
            ]
        )
    else:
        return AuditAdjustment(
            reference="AJE-BD",
            type=AdjustmentType.AJE,
            description="Adjust allowance for doubtful accounts",
            workpaper_ref="C-3",
            lines=[
                AdjustmentLine(
                    account_id=bad_debt_expense_account_id,
                    account_name="Bad Debt Expense",
                    debit=amount,
                    description="Bad debt provision"
                ),
                AdjustmentLine(
                    account_id=allowance_account_id,
                    account_name="Allowance for Doubtful Accounts",
                    credit=amount,
                    description="Increase allowance"
                )
            ]
        )
```

---

## Fixed Assets

### Depreciation Adjustment

```python
def create_depreciation_aje(
    amount: Decimal,
    depreciation_expense_account_id: str,
    accumulated_depreciation_account_id: str,
    asset_description: str = "Fixed assets"
) -> AuditAdjustment:
    """
    AJE to record or adjust depreciation.

    Debit: Depreciation Expense
    Credit: Accumulated Depreciation
    """
    return AuditAdjustment(
        reference="AJE-DEP",
        type=AdjustmentType.AJE,
        description=f"Adjust depreciation - {asset_description}",
        workpaper_ref="E-3",
        lines=[
            AdjustmentLine(
                account_id=depreciation_expense_account_id,
                account_name="Depreciation Expense",
                debit=amount,
                description=f"Depreciation - {asset_description}"
            ),
            AdjustmentLine(
                account_id=accumulated_depreciation_account_id,
                account_name="Accumulated Depreciation",
                credit=amount,
                description=f"Accumulated depreciation - {asset_description}"
            )
        ]
    )
```

### Capitalize Expense

```python
def create_asset_capitalization_aje(
    amount: Decimal,
    expense_account_id: str,
    fixed_asset_account_id: str,
    description: str
) -> AuditAdjustment:
    """
    AJE to capitalize expense that should be an asset.

    Debit: Fixed Asset
    Credit: Expense (reduce)
    """
    return AuditAdjustment(
        reference="AJE-CAP",
        type=AdjustmentType.AJE,
        description=f"Capitalize {description}",
        workpaper_ref="E-2",
        lines=[
            AdjustmentLine(
                account_id=fixed_asset_account_id,
                account_name="Fixed Assets",
                debit=amount,
                description=f"Capitalize {description}"
            ),
            AdjustmentLine(
                account_id=expense_account_id,
                account_name="Expense",
                credit=amount,
                description=f"Reduce expense - capitalize {description}"
            )
        ]
    )
```

---

## See Also

- [audit-lead-schedules.md](audit-lead-schedules.md) - Lead schedules
