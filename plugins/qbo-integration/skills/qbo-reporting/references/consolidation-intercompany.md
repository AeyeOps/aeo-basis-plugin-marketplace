---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Intercompany Transactions

Tracking, reconciling, and eliminating intercompany transactions across multiple QBO companies.

## Contents

- [Overview](#overview)
- [Multi-Entity Client Setup](#multi-entity-client-setup)
- [Intercompany Transaction Recording](#intercompany-transaction-recording)
- [Intercompany Reconciliation](#intercompany-reconciliation)
- [Elimination Entries](#elimination-entries)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

When a parent company owns multiple subsidiaries, each with its own QBO company file:
- Intercompany transactions must be tracked
- Due to/from balances must reconcile
- Eliminations required for consolidated statements

---

## Multi-Entity Client Setup

### Entity Registry

```python
from dataclasses import dataclass
from decimal import Decimal
from typing import Optional
from quickbooks import QuickBooks
from intuitlib.client import AuthClient

@dataclass
class QBOEntity:
    """Represents one QBO company in a consolidated group."""
    entity_id: str  # Internal identifier
    entity_name: str
    realm_id: str  # QBO company ID
    entity_type: str  # "parent", "subsidiary", "affiliate"
    ownership_pct: Decimal = Decimal("100")
    functional_currency: str = "USD"
    elimination_partner: Optional[str] = None  # For reciprocal accounts


@dataclass
class ConsolidatedGroup:
    """Group of related QBO companies for consolidation."""
    group_name: str
    parent_entity_id: str
    entities: list[QBOEntity]
    reporting_currency: str = "USD"


def create_entity_clients(
    group: ConsolidatedGroup,
    auth_client: AuthClient
) -> dict[str, QuickBooks]:
    """
    Create QuickBooks client for each entity in group.

    Returns dict of entity_id -> QuickBooks client.
    """
    clients = {}

    for entity in group.entities:
        # Each entity needs its own client with same auth but different realm_id
        client = QuickBooks(
            auth_client=auth_client,
            refresh_token=auth_client.refresh_token,
            company_id=entity.realm_id
        )
        clients[entity.entity_id] = client

    return clients
```

### Intercompany Account Mapping

```python
@dataclass
class IntercompanyAccountPair:
    """Matched pair of intercompany accounts across entities."""
    entity_a: str
    account_a_id: str
    account_a_name: str
    entity_b: str
    account_b_id: str
    account_b_name: str
    account_type: str  # "due_to_from", "investment", "loan", "revenue_expense"


# Standard intercompany account patterns
IC_ACCOUNT_PATTERNS = {
    "due_to_from": [
        "due to",
        "due from",
        "intercompany receivable",
        "intercompany payable",
        "ic receivable",
        "ic payable"
    ],
    "investment": [
        "investment in",
        "subsidiary",
        "affiliate"
    ],
    "loan": [
        "intercompany loan",
        "loan to",
        "loan from",
        "note receivable.*related",
        "note payable.*related"
    ],
    "revenue_expense": [
        "management fee",
        "intercompany revenue",
        "intercompany expense",
        "royalty"
    ]
}


def identify_intercompany_accounts(
    client,
    entity_id: str
) -> list[dict]:
    """
    Identify intercompany accounts in a QBO company.

    Searches for accounts matching IC patterns.
    """
    import re
    from quickbooks.objects.account import Account

    accounts = Account.all(qb=client)
    ic_accounts = []

    for acct in accounts:
        if not acct.Active:
            continue

        name_lower = acct.Name.lower()

        for ic_type, patterns in IC_ACCOUNT_PATTERNS.items():
            for pattern in patterns:
                if re.search(pattern, name_lower, re.IGNORECASE):
                    ic_accounts.append({
                        "entity_id": entity_id,
                        "account_id": acct.Id,
                        "account_name": acct.Name,
                        "account_type": acct.AccountType,
                        "ic_type": ic_type,
                        "balance": float(acct.CurrentBalance or 0)
                    })
                    break

    return ic_accounts
```

---

## Intercompany Transaction Recording

### Due To/From Entries

```python
from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

def record_intercompany_transaction(
    from_client,
    to_client,
    amount: float,
    description: str,
    transaction_date: str,
    from_ic_payable_id: str,
    from_expense_id: str,
    to_ic_receivable_id: str,
    to_revenue_id: str,
    reference: str = None
) -> tuple[JournalEntry, JournalEntry]:
    """
    Record matching intercompany transaction in both entities.

    Example: Parent pays management fee to Subsidiary
    - In Parent: Debit Management Fee Expense, Credit Due to Subsidiary
    - In Subsidiary: Debit Due from Parent, Credit Management Fee Revenue

    Args:
        from_client: QBO client for paying entity
        to_client: QBO client for receiving entity
        amount: Transaction amount
        description: Transaction description
        transaction_date: Date of transaction
        from_ic_payable_id: Paying entity's IC payable account
        from_expense_id: Paying entity's expense account
        to_ic_receivable_id: Receiving entity's IC receivable account
        to_revenue_id: Receiving entity's revenue account

    Returns:
        Tuple of (from_je, to_je)
    """
    doc_number = reference or f"IC-{transaction_date}"

    # Entry in paying entity
    from_je = JournalEntry()
    from_je.TxnDate = transaction_date
    from_je.DocNumber = doc_number
    from_je.PrivateNote = f"IC transaction: {description}"

    from_je.Line = [
        create_je_line("Debit", from_expense_id, amount, description),
        create_je_line("Credit", from_ic_payable_id, amount, f"Due to related party - {description}")
    ]

    from_je.save(qb=from_client)

    # Entry in receiving entity
    to_je = JournalEntry()
    to_je.TxnDate = transaction_date
    to_je.DocNumber = doc_number
    to_je.PrivateNote = f"IC transaction: {description}"

    to_je.Line = [
        create_je_line("Debit", to_ic_receivable_id, amount, f"Due from related party - {description}"),
        create_je_line("Credit", to_revenue_id, amount, description)
    ]

    to_je.save(qb=to_client)

    return from_je, to_je


def create_je_line(posting_type: str, account_id: str, amount: float, description: str) -> JournalEntryLine:
    """Helper to create journal entry line."""
    line = JournalEntryLine()
    line.Amount = amount
    line.Description = description
    line.JournalEntryLineDetail = {
        "PostingType": posting_type,
        "AccountRef": {"value": account_id}
    }
    return line
```

### Intercompany Loan

```python
def record_intercompany_loan(
    lender_client,
    borrower_client,
    principal: float,
    transaction_date: str,
    lender_loan_receivable_id: str,
    lender_cash_id: str,
    borrower_loan_payable_id: str,
    borrower_cash_id: str,
    reference: str = None
) -> tuple[JournalEntry, JournalEntry]:
    """
    Record intercompany loan origination.

    In Lender:
        Debit: Intercompany Loan Receivable
        Credit: Cash

    In Borrower:
        Debit: Cash
        Credit: Intercompany Loan Payable
    """
    doc_number = reference or f"IC-LOAN-{transaction_date}"

    # Lender entry
    lender_je = JournalEntry()
    lender_je.TxnDate = transaction_date
    lender_je.DocNumber = doc_number
    lender_je.PrivateNote = f"IC loan to related party - principal ${principal:,.2f}"

    lender_je.Line = [
        create_je_line("Debit", lender_loan_receivable_id, principal, "IC loan receivable"),
        create_je_line("Credit", lender_cash_id, principal, "Cash disbursed for IC loan")
    ]

    lender_je.save(qb=lender_client)

    # Borrower entry
    borrower_je = JournalEntry()
    borrower_je.TxnDate = transaction_date
    borrower_je.DocNumber = doc_number
    borrower_je.PrivateNote = f"IC loan from related party - principal ${principal:,.2f}"

    borrower_je.Line = [
        create_je_line("Debit", borrower_cash_id, principal, "Cash received from IC loan"),
        create_je_line("Credit", borrower_loan_payable_id, principal, "IC loan payable")
    ]

    borrower_je.save(qb=borrower_client)

    return lender_je, borrower_je


def record_intercompany_interest(
    lender_client,
    borrower_client,
    interest_amount: float,
    transaction_date: str,
    lender_interest_receivable_id: str,
    lender_interest_income_id: str,
    borrower_interest_payable_id: str,
    borrower_interest_expense_id: str
) -> tuple[JournalEntry, JournalEntry]:
    """
    Record intercompany loan interest accrual.

    In Lender:
        Debit: Interest Receivable
        Credit: Interest Income

    In Borrower:
        Debit: Interest Expense
        Credit: Interest Payable
    """
    # Lender entry
    lender_je = JournalEntry()
    lender_je.TxnDate = transaction_date
    lender_je.DocNumber = f"IC-INT-{transaction_date}"
    lender_je.PrivateNote = "IC loan interest accrual"

    lender_je.Line = [
        create_je_line("Debit", lender_interest_receivable_id, interest_amount, "IC interest receivable"),
        create_je_line("Credit", lender_interest_income_id, interest_amount, "IC interest income")
    ]

    lender_je.save(qb=lender_client)

    # Borrower entry
    borrower_je = JournalEntry()
    borrower_je.TxnDate = transaction_date
    borrower_je.DocNumber = f"IC-INT-{transaction_date}"
    borrower_je.PrivateNote = "IC loan interest accrual"

    borrower_je.Line = [
        create_je_line("Debit", borrower_interest_expense_id, interest_amount, "IC interest expense"),
        create_je_line("Credit", borrower_interest_payable_id, interest_amount, "IC interest payable")
    ]

    borrower_je.save(qb=borrower_client)

    return lender_je, borrower_je
```

---

## Intercompany Reconciliation

### Balance Reconciliation

```python
@dataclass
class ICReconciliationItem:
    """Single intercompany balance pair."""
    entity_a: str
    account_a: str
    balance_a: Decimal  # Positive = receivable
    entity_b: str
    account_b: str
    balance_b: Decimal  # Positive = payable (should offset A)
    difference: Decimal
    is_reconciled: bool


def reconcile_intercompany_balances(
    clients: dict[str, QuickBooks],
    ic_account_pairs: list[IntercompanyAccountPair]
) -> list[ICReconciliationItem]:
    """
    Reconcile intercompany balances across entities.

    For each pair of IC accounts, balances should offset to zero.
    """
    from quickbooks.objects.account import Account

    results = []

    for pair in ic_account_pairs:
        # Get balance from entity A
        client_a = clients[pair.entity_a]
        acct_a = Account.get(pair.account_a_id, qb=client_a)
        balance_a = Decimal(str(acct_a.CurrentBalance or 0))

        # Get balance from entity B
        client_b = clients[pair.entity_b]
        acct_b = Account.get(pair.account_b_id, qb=client_b)
        balance_b = Decimal(str(acct_b.CurrentBalance or 0))

        # For due to/from: receivable should equal payable
        # Sign convention: receivable positive, payable negative in our check
        if pair.account_type == "due_to_from":
            # Entity A has receivable (positive), Entity B has payable (should be negative/credit)
            difference = balance_a + balance_b  # Should net to zero
        else:
            # For investments, loans - need specific logic
            difference = balance_a + balance_b

        results.append(ICReconciliationItem(
            entity_a=pair.entity_a,
            account_a=pair.account_a_name,
            balance_a=balance_a,
            entity_b=pair.entity_b,
            account_b=pair.account_b_name,
            balance_b=balance_b,
            difference=difference,
            is_reconciled=abs(difference) < Decimal("0.01")
        ))

    return results


def generate_ic_reconciliation_report(
    reconciliation: list[ICReconciliationItem],
    as_of_date: str,
    output_path: str = None
) -> dict:
    """
    Generate intercompany reconciliation report.

    Highlights unreconciled items.
    """
    report = {
        "as_of_date": as_of_date,
        "total_pairs": len(reconciliation),
        "reconciled_count": sum(1 for r in reconciliation if r.is_reconciled),
        "unreconciled_count": sum(1 for r in reconciliation if not r.is_reconciled),
        "items": [],
        "exceptions": []
    }

    for item in reconciliation:
        item_data = {
            "entity_a": item.entity_a,
            "account_a": item.account_a,
            "balance_a": float(item.balance_a),
            "entity_b": item.entity_b,
            "account_b": item.account_b,
            "balance_b": float(item.balance_b),
            "difference": float(item.difference),
            "is_reconciled": item.is_reconciled
        }
        report["items"].append(item_data)

        if not item.is_reconciled:
            report["exceptions"].append(item_data)

    if output_path:
        export_ic_reconciliation_excel(report, output_path)

    return report


def export_ic_reconciliation_excel(report: dict, output_path: str):
    """Export IC reconciliation to Excel."""
    from openpyxl import Workbook
    from openpyxl.styles import Font, PatternFill

    wb = Workbook()
    ws = wb.active
    ws.title = "IC Reconciliation"

    # Header
    ws["A1"] = f"Intercompany Reconciliation as of {report['as_of_date']}"
    ws["A1"].font = Font(bold=True, size=14)

    ws["A3"] = f"Total Pairs: {report['total_pairs']}"
    ws["A4"] = f"Reconciled: {report['reconciled_count']}"
    ws["A5"] = f"Unreconciled: {report['unreconciled_count']}"

    # Detail
    headers = ["Entity A", "Account A", "Balance A", "Entity B", "Account B", "Balance B", "Difference", "Status"]
    for col, header in enumerate(headers, 1):
        ws.cell(row=7, column=col, value=header).font = Font(bold=True)

    row = 8
    for item in report["items"]:
        ws.cell(row=row, column=1, value=item["entity_a"])
        ws.cell(row=row, column=2, value=item["account_a"])
        ws.cell(row=row, column=3, value=item["balance_a"])
        ws.cell(row=row, column=4, value=item["entity_b"])
        ws.cell(row=row, column=5, value=item["account_b"])
        ws.cell(row=row, column=6, value=item["balance_b"])
        ws.cell(row=row, column=7, value=item["difference"])

        status_cell = ws.cell(row=row, column=8, value="OK" if item["is_reconciled"] else "EXCEPTION")
        if not item["is_reconciled"]:
            status_cell.fill = PatternFill(start_color="FFCCCC", fill_type="solid")

        row += 1

    wb.save(output_path)
```

---

## Elimination Entries

### Standard Eliminations

```python
@dataclass
class EliminationEntry:
    """Consolidation elimination entry."""
    reference: str
    description: str
    entity_debits: dict[str, list[tuple[str, Decimal]]]  # entity -> [(account_id, amount)]
    entity_credits: dict[str, list[tuple[str, Decimal]]]
    elimination_type: str  # "ic_balance", "ic_revenue_expense", "investment", "equity"


def create_ic_balance_elimination(
    receivable_entity: str,
    receivable_account_id: str,
    payable_entity: str,
    payable_account_id: str,
    amount: Decimal
) -> EliminationEntry:
    """
    Create elimination for intercompany receivable/payable.

    Debit: IC Payable (liability decreases)
    Credit: IC Receivable (asset decreases)
    """
    return EliminationEntry(
        reference="ELIM-IC-BAL",
        description="Eliminate intercompany receivable/payable",
        entity_debits={payable_entity: [(payable_account_id, amount)]},
        entity_credits={receivable_entity: [(receivable_account_id, amount)]},
        elimination_type="ic_balance"
    )


def create_ic_revenue_expense_elimination(
    revenue_entity: str,
    revenue_account_id: str,
    expense_entity: str,
    expense_account_id: str,
    amount: Decimal
) -> EliminationEntry:
    """
    Create elimination for intercompany revenue/expense.

    Debit: IC Revenue (revenue decreases)
    Credit: IC Expense (expense decreases)
    """
    return EliminationEntry(
        reference="ELIM-IC-PL",
        description="Eliminate intercompany revenue/expense",
        entity_debits={revenue_entity: [(revenue_account_id, amount)]},
        entity_credits={expense_entity: [(expense_account_id, amount)]},
        elimination_type="ic_revenue_expense"
    )


def create_investment_elimination(
    parent_entity: str,
    investment_account_id: str,
    sub_entity: str,
    equity_account_ids: list[str],
    investment_amount: Decimal,
    equity_amounts: list[Decimal]
) -> EliminationEntry:
    """
    Create elimination of investment in subsidiary against subsidiary equity.

    Debit: Subsidiary Common Stock
    Debit: Subsidiary Retained Earnings
    Credit: Investment in Subsidiary
    """
    sub_debits = [(acct_id, amt) for acct_id, amt in zip(equity_account_ids, equity_amounts)]

    return EliminationEntry(
        reference="ELIM-INV",
        description="Eliminate investment in subsidiary",
        entity_debits={sub_entity: sub_debits},
        entity_credits={parent_entity: [(investment_account_id, investment_amount)]},
        elimination_type="investment"
    )
```

### Apply Eliminations

```python
def apply_eliminations_to_trial_balance(
    combined_tb: list[dict],
    eliminations: list[EliminationEntry]
) -> list[dict]:
    """
    Apply elimination entries to combined trial balance.

    Returns adjusted trial balance with elimination columns.
    """
    # Index TB by (entity, account_id)
    tb_index = {}
    for row in combined_tb:
        key = (row["entity_id"], row["account_id"])
        tb_index[key] = row
        row["elimination_dr"] = Decimal("0")
        row["elimination_cr"] = Decimal("0")

    for elim in eliminations:
        # Apply debits
        for entity, entries in elim.entity_debits.items():
            for account_id, amount in entries:
                key = (entity, account_id)
                if key in tb_index:
                    tb_index[key]["elimination_dr"] += amount

        # Apply credits
        for entity, entries in elim.entity_credits.items():
            for account_id, amount in entries:
                key = (entity, account_id)
                if key in tb_index:
                    tb_index[key]["elimination_cr"] += amount

    # Calculate consolidated balances
    for row in combined_tb:
        original_balance = Decimal(str(row["debit"])) - Decimal(str(row["credit"]))
        elim_effect = row["elimination_dr"] - row["elimination_cr"]
        row["consolidated_balance"] = float(original_balance + elim_effect)

    return combined_tb
```

---

## Best Practices

1. **Use consistent account naming** - "Due to [Entity]", "Due from [Entity]"
2. **Match DocNumbers** - Same reference in both entities for traceability
3. **Reconcile monthly** - Don't wait until year-end
4. **Document all IC transactions** - Maintain transaction log
5. **Use standardized rates** - Management fees, interest rates documented
6. **Review transfer pricing** - Ensure arm's length transactions
7. **Automate matching** - Script to find unmatched transactions

---

## See Also

- [consolidation-statements.md](consolidation-statements.md) - Combined TB and consolidated statements
- [consolidation-segments.md](consolidation-segments.md) - Segment definitions and reporting
