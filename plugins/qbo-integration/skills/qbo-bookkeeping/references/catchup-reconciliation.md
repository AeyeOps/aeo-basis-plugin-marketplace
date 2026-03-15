# Catch-Up Reconciliation and Validation

Bank reconciliation, year-end closing, and validation checks for catch-up bookkeeping.

## Contents

- [Overview](#overview)
- [Monthly Bank Reconciliation](#monthly-bank-reconciliation)
- [Year-End Closing](#year-end-closing)
- [Validation Checks](#validation-checks)
- [Workflow Summary](#workflow-summary)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

This reference covers verifying and validating catch-up bookkeeping:
- Monthly bank reconciliation
- Year-end closing entries
- Validation checks for completeness
- Audit trail documentation

---

## Monthly Bank Reconciliation

```python
from dataclasses import dataclass
from decimal import Decimal
from datetime import date
import requests

@dataclass
class ReconciliationResult:
    """Result of a bank reconciliation."""
    account_name: str
    period_end: date
    statement_balance: Decimal
    qbo_balance: Decimal
    difference: Decimal
    is_reconciled: bool
    unmatched_items: list[dict]


def reconcile_bank_month(
    client,
    bank_account: str,
    statement_ending_balance: Decimal,
    period_end: date
) -> ReconciliationResult:
    """
    Reconcile QBO bank balance against statement.

    Args:
        client: QuickBooks client
        bank_account: Bank account name
        statement_ending_balance: Balance from bank statement
        period_end: Last day of the period

    Returns:
        ReconciliationResult with any differences
    """
    base_url = client.api_url
    realm_id = client.company_id

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    url = f"{base_url}/v3/company/{realm_id}/reports/TrialBalance"
    params = {
        "date_macro": "Custom",
        "end_date": period_end.isoformat()
    }

    resp = requests.get(url, headers=headers, params=params)
    resp.raise_for_status()

    report = resp.json()
    qbo_balance = Decimal("0")

    rows = report.get("Rows", {}).get("Row", [])
    for row in rows:
        if row.get("type") == "Data":
            cols = row.get("ColData", [])
            if len(cols) >= 2 and cols[0].get("value") == bank_account:
                for col in cols[1:]:
                    val = col.get("value", "0")
                    if val:
                        qbo_balance = Decimal(val.replace(",", ""))
                        break

    difference = statement_ending_balance - qbo_balance

    return ReconciliationResult(
        account_name=bank_account,
        period_end=period_end,
        statement_balance=statement_ending_balance,
        qbo_balance=qbo_balance,
        difference=difference,
        is_reconciled=abs(difference) < Decimal("0.01"),
        unmatched_items=[]
    )
```

---

## Year-End Closing

```python
def create_year_end_closing(
    client,
    fiscal_year_end: date,
    retained_earnings_account: str = "Retained Earnings"
) -> dict:
    """
    Create year-end closing entry.

    Note: QBO handles this automatically for reporting, but
    explicit entries may be needed for specific workflows.
    """
    base_url = client.api_url
    realm_id = client.company_id

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    start_date = date(fiscal_year_end.year, 1, 1)
    url = f"{base_url}/v3/company/{realm_id}/reports/ProfitAndLoss"
    params = {
        "start_date": start_date.isoformat(),
        "end_date": fiscal_year_end.isoformat(),
        "summarize_column_by": "Total"
    }

    resp = requests.get(url, headers=headers, params=params)
    resp.raise_for_status()

    report = resp.json()
    net_income = Decimal("0")

    rows = report.get("Rows", {}).get("Row", [])
    for row in rows:
        summary = row.get("Summary", {})
        if summary:
            cols = summary.get("ColData", [])
            if len(cols) >= 2:
                label = cols[0].get("value", "")
                if "Net Income" in label or "Net Profit" in label:
                    val = cols[1].get("value", "0")
                    net_income = Decimal(val.replace(",", "").replace("$", ""))

    return {
        "fiscal_year_end": fiscal_year_end.isoformat(),
        "net_income": float(net_income),
        "note": "QBO automatically closes to retained earnings for reporting"
    }
```

---

## Validation Checks

### Validation Result

```python
from dataclasses import dataclass

@dataclass
class ValidationResult:
    """Result of a validation check."""
    check_name: str
    passed: bool
    message: str
    details: dict = None
```

### Run All Validations

```python
def validate_catch_up(client, period_start: date, period_end: date) -> list[ValidationResult]:
    """Run validation checks on catch-up bookkeeping."""
    results = []

    obe_check = check_opening_balance_equity(client, period_end)
    results.append(obe_check)

    tb_check = check_trial_balance(client, period_end)
    results.append(tb_check)

    pre_check = check_no_pre_opening_transactions(client, period_start)
    results.append(pre_check)

    return results
```

### Check Opening Balance Equity

```python
def check_opening_balance_equity(client, as_of_date: date) -> ValidationResult:
    """Opening Balance Equity should be zero after catch-up."""
    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/query"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    query = "SELECT * FROM Account WHERE Name = 'Opening Balance Equity'"
    resp = requests.get(url, headers=headers, params={"query": query})

    if resp.ok:
        accounts = resp.json().get("QueryResponse", {}).get("Account", [])
        if accounts:
            balance = Decimal(str(accounts[0].get("CurrentBalance", 0)))
            if abs(balance) < Decimal("0.01"):
                return ValidationResult(
                    "Opening Balance Equity",
                    True,
                    "Opening Balance Equity is zero",
                    {"balance": float(balance)}
                )
            else:
                return ValidationResult(
                    "Opening Balance Equity",
                    False,
                    f"Opening Balance Equity has balance: {balance}",
                    {"balance": float(balance)}
                )

    return ValidationResult(
        "Opening Balance Equity",
        True,
        "No Opening Balance Equity account found"
    )
```

### Check Trial Balance

```python
def check_trial_balance(client, as_of_date: date) -> ValidationResult:
    """Trial balance debits should equal credits."""
    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/reports/TrialBalance"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    params = {"end_date": as_of_date.isoformat()}
    resp = requests.get(url, headers=headers, params=params)

    if resp.ok:
        report = resp.json()
        columns = report.get("Columns", {}).get("Column", [])

        debit_idx = None
        credit_idx = None
        for i, col in enumerate(columns):
            title = col.get("ColTitle", "")
            if "Debit" in title:
                debit_idx = i
            elif "Credit" in title:
                credit_idx = i

        total_debits = Decimal("0")
        total_credits = Decimal("0")

        rows = report.get("Rows", {}).get("Row", [])
        for row in rows:
            if row.get("type") == "Data":
                cols = row.get("ColData", [])
                if debit_idx and len(cols) > debit_idx:
                    val = cols[debit_idx].get("value", "0") or "0"
                    total_debits += Decimal(val.replace(",", ""))
                if credit_idx and len(cols) > credit_idx:
                    val = cols[credit_idx].get("value", "0") or "0"
                    total_credits += Decimal(val.replace(",", ""))

        diff = abs(total_debits - total_credits)
        if diff < Decimal("0.01"):
            return ValidationResult(
                "Trial Balance",
                True,
                "Trial balance is balanced",
                {"debits": float(total_debits), "credits": float(total_credits)}
            )
        else:
            return ValidationResult(
                "Trial Balance",
                False,
                f"Trial balance off by {diff}",
                {"debits": float(total_debits), "credits": float(total_credits), "difference": float(diff)}
            )

    return ValidationResult(
        "Trial Balance",
        False,
        "Could not retrieve trial balance"
    )
```

### Check Pre-Opening Transactions

```python
def check_no_pre_opening_transactions(client, opening_date: date) -> ValidationResult:
    """No transactions should exist before opening date."""
    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/query"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    query = f"SELECT COUNT(*) FROM JournalEntry WHERE TxnDate < '{opening_date.isoformat()}'"
    resp = requests.get(url, headers=headers, params={"query": query})

    pre_count = 0
    if resp.ok:
        pre_count = resp.json().get("QueryResponse", {}).get("totalCount", 0)

    if pre_count == 0:
        return ValidationResult(
            "Pre-Opening Transactions",
            True,
            "No transactions before opening date"
        )
    else:
        return ValidationResult(
            "Pre-Opening Transactions",
            False,
            f"Found {pre_count} transactions before opening date",
            {"count": pre_count, "opening_date": opening_date.isoformat()}
        )
```

---

## Workflow Summary

1. **Enter** - Import transactions by month/year
2. **Reconcile** - Verify bank balances match statements
3. **Validate** - Run checks for completeness and accuracy
4. **Document** - Attach source documents for audit trail

---

## Best Practices

1. **Work chronologically** - Start with earliest year, complete before moving forward
2. **Reconcile monthly** - Don't wait until all transactions are entered
3. **Use batch operations** - Group similar transactions for efficiency
4. **Preserve source documents** - Attach statements to summary journal entries
5. **Document assumptions** - Note any estimates or allocations in private notes
6. **Validate incrementally** - Run checks after each fiscal year
7. **Back up before closing** - Export trial balance before year-end
8. **Maintain consistency** - Use same categorization rules throughout

---

## See Also

- [catchup-planning.md](catchup-planning.md) - Assessment and setup
- [catchup-transaction-entry.md](catchup-transaction-entry.md) - Transaction entry strategies
