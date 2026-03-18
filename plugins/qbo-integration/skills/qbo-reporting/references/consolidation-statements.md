---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Consolidated Financial Statements

Combined trial balance, account mapping, and consolidated balance sheet/income statement generation.

## Contents

- [Overview](#overview)
- [Combined Trial Balance](#combined-trial-balance)
- [Account Mapping](#account-mapping)
- [Consolidated Balance Sheet](#consolidated-balance-sheet)
- [Consolidated Income Statement](#consolidated-income-statement)
- [See Also](#see-also)

---

## Overview

This reference covers generating consolidated financial statements from multiple QBO companies:
- Combined trial balance extraction
- Standardized chart of accounts mapping
- Consolidated balance sheet generation
- Consolidated income statement generation

See [consolidation-intercompany.md](consolidation-intercompany.md) for elimination entries.

---

## Combined Trial Balance

### Data Structure

```python
from dataclasses import dataclass
from decimal import Decimal
from typing import Optional
from quickbooks import QuickBooks
from quickbooks.objects.account import Account

@dataclass
class CombinedTBRow:
    """Row in combined trial balance."""
    entity_id: str
    entity_name: str
    account_id: str
    account_number: str
    account_name: str
    account_type: str
    classification: str
    debit: Decimal
    credit: Decimal
    # For consolidation
    mapped_account: Optional[str] = None  # Standardized account for consolidation
    segment: Optional[str] = None
```

### Extract from Multiple Entities

```python
def extract_combined_trial_balance(
    clients: dict[str, QuickBooks],
    entities: list[dict],
    as_of_date: str
) -> list[CombinedTBRow]:
    """
    Extract trial balance from all entities and combine.

    Args:
        clients: Dict of entity_id -> QuickBooks client
        entities: List of entity info dicts with id, name
        as_of_date: Balance date

    Returns:
        Combined list of all TB rows across entities
    """
    combined = []

    for entity in entities:
        entity_id = entity["id"]
        entity_name = entity["name"]
        client = clients[entity_id]

        accounts = Account.all(qb=client)

        for acct in accounts:
            if not acct.Active:
                continue

            balance = Decimal(str(acct.CurrentBalance or 0))
            if balance == 0:
                continue

            # Determine debit/credit
            debit = Decimal("0")
            credit = Decimal("0")

            if acct.Classification in ["Asset", "Expense"]:
                if balance >= 0:
                    debit = balance
                else:
                    credit = abs(balance)
            else:
                if balance >= 0:
                    credit = balance
                else:
                    debit = abs(balance)

            combined.append(CombinedTBRow(
                entity_id=entity_id,
                entity_name=entity_name,
                account_id=acct.Id,
                account_number=acct.AcctNum or "",
                account_name=acct.Name,
                account_type=acct.AccountType,
                classification=acct.Classification,
                debit=debit,
                credit=credit
            ))

    return combined
```

---

## Account Mapping

### Consolidated Chart of Accounts

```python
@dataclass
class ConsolidatedAccount:
    """Standardized account for consolidation."""
    consolidated_id: str
    consolidated_name: str
    classification: str
    display_order: int
    fs_line: str  # Financial statement line item


CONSOLIDATED_COA = {
    # Assets
    "1000": ConsolidatedAccount("1000", "Cash and Cash Equivalents", "Asset", 100, "Current Assets"),
    "1100": ConsolidatedAccount("1100", "Short-term Investments", "Asset", 110, "Current Assets"),
    "1200": ConsolidatedAccount("1200", "Accounts Receivable, net", "Asset", 120, "Current Assets"),
    "1300": ConsolidatedAccount("1300", "Inventory", "Asset", 130, "Current Assets"),
    "1400": ConsolidatedAccount("1400", "Prepaid Expenses", "Asset", 140, "Current Assets"),
    "1500": ConsolidatedAccount("1500", "Other Current Assets", "Asset", 150, "Current Assets"),
    "1600": ConsolidatedAccount("1600", "Property, Plant & Equipment, net", "Asset", 160, "Non-current Assets"),
    "1700": ConsolidatedAccount("1700", "Intangible Assets, net", "Asset", 170, "Non-current Assets"),
    "1800": ConsolidatedAccount("1800", "Long-term Investments", "Asset", 180, "Non-current Assets"),
    "1900": ConsolidatedAccount("1900", "Other Non-current Assets", "Asset", 190, "Non-current Assets"),

    # Liabilities
    "2000": ConsolidatedAccount("2000", "Accounts Payable", "Liability", 200, "Current Liabilities"),
    "2100": ConsolidatedAccount("2100", "Accrued Liabilities", "Liability", 210, "Current Liabilities"),
    "2200": ConsolidatedAccount("2200", "Short-term Debt", "Liability", 220, "Current Liabilities"),
    "2300": ConsolidatedAccount("2300", "Deferred Revenue", "Liability", 230, "Current Liabilities"),
    "2400": ConsolidatedAccount("2400", "Other Current Liabilities", "Liability", 240, "Current Liabilities"),
    "2500": ConsolidatedAccount("2500", "Long-term Debt", "Liability", 250, "Non-current Liabilities"),
    "2600": ConsolidatedAccount("2600", "Deferred Tax Liabilities", "Liability", 260, "Non-current Liabilities"),
    "2700": ConsolidatedAccount("2700", "Other Non-current Liabilities", "Liability", 270, "Non-current Liabilities"),

    # Equity
    "3000": ConsolidatedAccount("3000", "Common Stock", "Equity", 300, "Shareholders' Equity"),
    "3100": ConsolidatedAccount("3100", "Additional Paid-in Capital", "Equity", 310, "Shareholders' Equity"),
    "3200": ConsolidatedAccount("3200", "Retained Earnings", "Equity", 320, "Shareholders' Equity"),
    "3300": ConsolidatedAccount("3300", "Accumulated Other Comprehensive Income", "Equity", 330, "Shareholders' Equity"),
    "3400": ConsolidatedAccount("3400", "Treasury Stock", "Equity", 340, "Shareholders' Equity"),
    "3500": ConsolidatedAccount("3500", "Non-controlling Interest", "Equity", 350, "Shareholders' Equity"),

    # Revenue
    "4000": ConsolidatedAccount("4000", "Revenue", "Revenue", 400, "Revenue"),
    "4100": ConsolidatedAccount("4100", "Other Income", "Revenue", 410, "Other Income"),

    # Expenses
    "5000": ConsolidatedAccount("5000", "Cost of Revenue", "Expense", 500, "Cost of Revenue"),
    "6000": ConsolidatedAccount("6000", "Operating Expenses", "Expense", 600, "Operating Expenses"),
    "7000": ConsolidatedAccount("7000", "Interest Expense", "Expense", 700, "Interest Expense"),
    "8000": ConsolidatedAccount("8000", "Income Tax Expense", "Expense", 800, "Income Tax Expense"),
}
```

### Create Account Mapping

```python
import re

def create_account_mapping(
    entity_accounts: list[dict],
    mapping_rules: dict = None
) -> dict[tuple[str, str], str]:
    """
    Create mapping from (entity_id, account_id) to consolidated account.

    Uses name matching and optional explicit rules.
    """
    # Default mapping rules based on account name patterns
    default_rules = [
        (r"cash|checking|savings|money market", "1000"),
        (r"investment.*short|marketable", "1100"),
        (r"accounts receivable|a/r|trade receivable", "1200"),
        (r"inventory|merchandise", "1300"),
        (r"prepaid", "1400"),
        (r"property|equipment|furniture|vehicle", "1600"),
        (r"goodwill|intangible|patent|trademark", "1700"),
        (r"investment.*long|equity investment", "1800"),
        (r"accounts payable|a/p|trade payable", "2000"),
        (r"accrued|accrual", "2100"),
        (r"short.*term.*debt|line of credit|current.*loan", "2200"),
        (r"deferred revenue|unearned", "2300"),
        (r"long.*term.*debt|note payable|mortgage", "2500"),
        (r"common stock|capital stock", "3000"),
        (r"paid.*in|apic|additional", "3100"),
        (r"retained earnings", "3200"),
        (r"treasury", "3400"),
        (r"revenue|sales|income", "4000"),
        (r"cost of (goods |revenue|sales)", "5000"),
        (r"expense|salary|wage|rent|utility|insurance", "6000"),
        (r"interest expense", "7000"),
        (r"income tax|tax expense", "8000"),
    ]

    mapping = {}

    for acct in entity_accounts:
        key = (acct["entity_id"], acct["account_id"])
        name_lower = acct["account_name"].lower()

        # Check explicit mapping rules first
        if mapping_rules and key in mapping_rules:
            mapping[key] = mapping_rules[key]
            continue

        # Apply default pattern matching
        for pattern, consolidated_id in default_rules:
            if re.search(pattern, name_lower, re.IGNORECASE):
                mapping[key] = consolidated_id
                break
        else:
            # Default based on classification
            classification = acct.get("classification", "")
            if classification == "Asset":
                mapping[key] = "1900"
            elif classification == "Liability":
                mapping[key] = "2700"
            elif classification == "Equity":
                mapping[key] = "3200"
            elif classification == "Revenue":
                mapping[key] = "4100"
            else:
                mapping[key] = "6000"

    return mapping
```

---

## Consolidated Balance Sheet

```python
def generate_consolidated_balance_sheet(
    combined_tb: list[CombinedTBRow],
    eliminations: list[dict],
    as_of_date: str,
    group_name: str
) -> dict:
    """
    Generate consolidated balance sheet.

    Aggregates by consolidated account, applies eliminations.
    """
    # Apply eliminations and aggregate
    consolidated = {}

    for row in combined_tb:
        cons_acct = row.mapped_account
        if not cons_acct:
            continue

        if cons_acct not in consolidated:
            consolidated[cons_acct] = {
                "account": CONSOLIDATED_COA.get(cons_acct),
                "entity_balances": {},
                "eliminations": Decimal("0"),
                "consolidated": Decimal("0")
            }

        # Add entity balance
        balance = row.debit - row.credit
        entity = row.entity_id
        if entity not in consolidated[cons_acct]["entity_balances"]:
            consolidated[cons_acct]["entity_balances"][entity] = Decimal("0")
        consolidated[cons_acct]["entity_balances"][entity] += balance

    # Apply eliminations
    for elim in eliminations:
        for acct_id, amount in elim.get("debits", []):
            if acct_id in consolidated:
                consolidated[acct_id]["eliminations"] += Decimal(str(amount))
        for acct_id, amount in elim.get("credits", []):
            if acct_id in consolidated:
                consolidated[acct_id]["eliminations"] -= Decimal(str(amount))

    # Calculate consolidated balances
    for acct_id, data in consolidated.items():
        entity_total = sum(data["entity_balances"].values())
        data["consolidated"] = entity_total + data["eliminations"]

    # Build balance sheet structure
    balance_sheet = {
        "title": f"{group_name} - Consolidated Balance Sheet",
        "as_of_date": as_of_date,
        "assets": {"current": [], "non_current": [], "total": Decimal("0")},
        "liabilities": {"current": [], "non_current": [], "total": Decimal("0")},
        "equity": {"items": [], "total": Decimal("0")}
    }

    for acct_id, data in sorted(consolidated.items(), key=lambda x: x[0]):
        acct_info = data["account"]
        if not acct_info:
            continue

        item = {"account": acct_info.consolidated_name, "amount": float(data["consolidated"])}

        if acct_info.classification == "Asset":
            if "Current" in acct_info.fs_line:
                balance_sheet["assets"]["current"].append(item)
            else:
                balance_sheet["assets"]["non_current"].append(item)
            balance_sheet["assets"]["total"] += data["consolidated"]
        elif acct_info.classification == "Liability":
            if "Current" in acct_info.fs_line:
                balance_sheet["liabilities"]["current"].append(item)
            else:
                balance_sheet["liabilities"]["non_current"].append(item)
            balance_sheet["liabilities"]["total"] += data["consolidated"]
        elif acct_info.classification == "Equity":
            balance_sheet["equity"]["items"].append(item)
            balance_sheet["equity"]["total"] += data["consolidated"]

    # Convert totals to float
    for section in ["assets", "liabilities", "equity"]:
        balance_sheet[section]["total"] = float(balance_sheet[section]["total"])

    return balance_sheet
```

---

## Consolidated Income Statement

```python
def generate_consolidated_income_statement(
    combined_tb: list[CombinedTBRow],
    eliminations: list[dict],
    start_date: str,
    end_date: str,
    group_name: str
) -> dict:
    """
    Generate consolidated income statement.
    """
    consolidated = {}

    for row in combined_tb:
        if row.classification not in ["Revenue", "Expense"]:
            continue

        cons_acct = row.mapped_account
        if not cons_acct:
            continue

        if cons_acct not in consolidated:
            consolidated[cons_acct] = {
                "account": CONSOLIDATED_COA.get(cons_acct),
                "entity_balances": {},
                "eliminations": Decimal("0"),
                "consolidated": Decimal("0")
            }

        # For P&L: credit is positive for revenue, debit is positive for expense
        if row.classification == "Revenue":
            balance = row.credit - row.debit
        else:
            balance = row.debit - row.credit

        entity = row.entity_id
        if entity not in consolidated[cons_acct]["entity_balances"]:
            consolidated[cons_acct]["entity_balances"][entity] = Decimal("0")
        consolidated[cons_acct]["entity_balances"][entity] += balance

    # Apply eliminations
    for elim in eliminations:
        if elim.get("type") != "ic_revenue_expense":
            continue
        for acct_id, amount in elim.get("debits", []):
            if acct_id in consolidated:
                consolidated[acct_id]["eliminations"] -= Decimal(str(amount))
        for acct_id, amount in elim.get("credits", []):
            if acct_id in consolidated:
                consolidated[acct_id]["eliminations"] += Decimal(str(amount))

    # Calculate consolidated
    for acct_id, data in consolidated.items():
        entity_total = sum(data["entity_balances"].values())
        data["consolidated"] = entity_total + data["eliminations"]

    # Build income statement
    income_statement = {
        "title": f"{group_name} - Consolidated Income Statement",
        "period": {"start": start_date, "end": end_date},
        "revenue": Decimal("0"),
        "cost_of_revenue": Decimal("0"),
        "gross_profit": Decimal("0"),
        "operating_expenses": Decimal("0"),
        "operating_income": Decimal("0"),
        "other_income_expense": Decimal("0"),
        "income_before_tax": Decimal("0"),
        "income_tax": Decimal("0"),
        "net_income": Decimal("0"),
        "line_items": []
    }

    for acct_id, data in sorted(consolidated.items(), key=lambda x: x[0]):
        acct_info = data["account"]
        if not acct_info:
            continue

        amount = data["consolidated"]
        income_statement["line_items"].append({"account": acct_info.consolidated_name, "amount": float(amount)})

        if acct_id == "4000":
            income_statement["revenue"] = amount
        elif acct_id == "4100":
            income_statement["other_income_expense"] += amount
        elif acct_id == "5000":
            income_statement["cost_of_revenue"] = amount
        elif acct_id == "6000":
            income_statement["operating_expenses"] = amount
        elif acct_id == "7000":
            income_statement["other_income_expense"] -= amount
        elif acct_id == "8000":
            income_statement["income_tax"] = amount

    # Calculate subtotals
    income_statement["gross_profit"] = income_statement["revenue"] - income_statement["cost_of_revenue"]
    income_statement["operating_income"] = income_statement["gross_profit"] - income_statement["operating_expenses"]
    income_statement["income_before_tax"] = income_statement["operating_income"] + income_statement["other_income_expense"]
    income_statement["net_income"] = income_statement["income_before_tax"] - income_statement["income_tax"]

    # Convert to float
    for key in income_statement:
        if isinstance(income_statement[key], Decimal):
            income_statement[key] = float(income_statement[key])

    return income_statement
```

---

## See Also

- [consolidation-intercompany.md](consolidation-intercompany.md) - Intercompany tracking and eliminations
- [consolidation-segments.md](consolidation-segments.md) - Segment definitions and reporting
