# Form 1120 Schedules L, M-1, M-2, C

Balance sheet mapping, book-tax reconciliation, retained earnings analysis, and dividends received deduction.

## Contents

- [Overview](#overview)
- [Schedule L - Balance Sheet](#schedule-l---balance-sheet)
- [Schedule M-1 - Book-Tax Reconciliation](#schedule-m-1---book-tax-reconciliation)
- [Schedule M-2 - Retained Earnings Analysis](#schedule-m-2---retained-earnings-analysis)
- [Schedule C - Dividends Received Deduction](#schedule-c---dividends-received-deduction)
- [Complete Tax Data Export](#complete-tax-data-export)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

This reference covers Form 1120 supporting schedules:
- **Schedule L**: Balance Sheet per Books
- **Schedule M-1**: Reconciliation of Income per Books with Income per Return
- **Schedule M-2**: Analysis of Unappropriated Retained Earnings
- **Schedule C**: Dividends Received Deduction

---

## Schedule L - Balance Sheet

### Asset Mappings

```python
import re
from quickbooks.objects.account import Account

SCHEDULE_L_ASSETS = [
    # Line, Description, QBO Account Types, Pattern
    ("1", "Cash", ["Bank"], None),
    ("2a", "Trade notes and accounts receivable", ["Accounts Receivable"], None),
    ("2b", "Less allowance for bad debts", ["Other Current Asset"], "allowance"),
    ("3", "Inventories", ["Other Current Asset"], "inventor"),
    ("4", "U.S. government obligations", ["Other Current Asset"], "treasury|t-bill"),
    ("5", "Tax-exempt securities", ["Other Current Asset"], "muni|tax.exempt"),
    ("6", "Other current assets", ["Other Current Asset"], None),
    ("7", "Loans to shareholders", ["Other Current Asset"], "loan.*shareholder|due.*shareholder"),
    ("8", "Mortgage and real estate loans", ["Other Current Asset"], "mortgage|real estate loan"),
    ("9", "Other investments", ["Other Asset"], "investment"),
    ("10a", "Buildings and other depreciable assets", ["Fixed Asset"], "building|equipment|furniture"),
    ("10b", "Less accumulated depreciation", ["Fixed Asset"], "accumulated depreciation"),
    ("11a", "Depletable assets", ["Fixed Asset"], "depletable|mineral"),
    ("11b", "Less accumulated depletion", ["Fixed Asset"], "accumulated depletion"),
    ("12", "Land", ["Fixed Asset"], "land"),
    ("13a", "Intangible assets", ["Other Asset"], "intangible|goodwill|patent"),
    ("13b", "Less accumulated amortization", ["Other Asset"], "accumulated amortization"),
    ("14", "Other assets", ["Other Asset"], None),
]

SCHEDULE_L_LIABILITIES = [
    ("15", "Accounts payable", ["Accounts Payable"], None),
    ("16", "Mortgages, notes, bonds payable < 1 year", ["Other Current Liability"], "note|loan|mortgage"),
    ("17", "Other current liabilities", ["Other Current Liability"], None),
    ("18", "Loans from shareholders", ["Long Term Liability"], "loan.*shareholder|due.*shareholder"),
    ("19", "Mortgages, notes, bonds payable >= 1 year", ["Long Term Liability"], "note|loan|mortgage"),
    ("20", "Other liabilities", ["Long Term Liability"], None),
    ("21", "Capital stock - preferred", ["Equity"], "preferred"),
    ("22a", "Capital stock - common", ["Equity"], "common stock|capital stock"),
    ("23", "Additional paid-in capital", ["Equity"], "paid.in|apic|additional"),
    ("24", "Retained earnings - appropriated", ["Equity"], "appropriated"),
    ("25", "Retained earnings - unappropriated", ["Equity"], "retained earnings"),
    ("26", "Adjustments to shareholders' equity", ["Equity"], "adjustment|oci"),
    ("27", "Less cost of treasury stock", ["Equity"], "treasury"),
]
```

### Extract Schedule L

```python
def extract_schedule_l(client, tax_year: int) -> dict:
    """
    Extract Schedule L (Balance Sheet) data.

    Returns beginning and ending balances for each line.
    """
    boy_date = f"{tax_year}-01-01"
    eoy_date = f"{tax_year}-12-31"

    schedule_l = {"assets": {}, "liabilities_equity": {}}

    bs_accounts = Account.query(
        "SELECT * FROM Account WHERE Classification IN ('Asset', 'Liability', 'Equity')",
        qb=client
    )

    for line, desc, types, pattern in SCHEDULE_L_ASSETS:
        matching = filter_accounts(bs_accounts, types, pattern)
        boy_balance = sum(get_account_balance_as_of(client, a.Id, boy_date) for a in matching)
        eoy_balance = sum(float(a.CurrentBalance or 0) for a in matching)

        schedule_l["assets"][line] = {
            "description": desc,
            "beginning": boy_balance,
            "ending": eoy_balance,
            "accounts": [a.Name for a in matching]
        }

    for line, desc, types, pattern in SCHEDULE_L_LIABILITIES:
        matching = filter_accounts(bs_accounts, types, pattern)
        boy_balance = sum(get_account_balance_as_of(client, a.Id, boy_date) for a in matching)
        eoy_balance = sum(float(a.CurrentBalance or 0) for a in matching)

        schedule_l["liabilities_equity"][line] = {
            "description": desc,
            "beginning": boy_balance,
            "ending": eoy_balance,
            "accounts": [a.Name for a in matching]
        }

    return schedule_l


def filter_accounts(accounts: list, types: list[str], pattern: str) -> list:
    """Filter accounts by type and name pattern."""
    result = []
    for acct in accounts:
        if acct.AccountType not in types:
            continue
        if pattern and not re.search(pattern, acct.Name, re.IGNORECASE):
            continue
        result.append(acct)
    return result
```

---

## Schedule M-1 - Book-Tax Reconciliation

### Common Adjustments

```python
from dataclasses import dataclass
from decimal import Decimal

@dataclass
class BookTaxDifference:
    """Represents a book-tax reconciliation item."""
    description: str
    book_amount: Decimal
    tax_amount: Decimal
    m1_line: str
    permanent: bool  # True = permanent, False = temporary


M1_ADJUSTMENTS = {
    "2": {"description": "Federal income tax per books", "qbo_pattern": "federal.*tax|income tax expense"},
    "3": {"description": "Excess of capital losses over capital gains", "calculation": "net_capital_loss"},
    "4": {"description": "Income subject to tax not recorded on books", "examples": ["prepaid income"]},
    "5": {
        "description": "Expenses recorded on books not deducted on return",
        "sub_items": {"a": "Depreciation", "b": "Charitable", "c": "T&E", "d": "Other"}
    },
    "7": {"description": "Income recorded on books not on return", "examples": ["tax-exempt interest"]},
    "8": {"description": "Deductions on return not on books", "examples": ["bonus depreciation"]},
}
```

### Extract Schedule M-1

```python
def extract_schedule_m1(client, tax_year: int, tax_adjustments: dict) -> dict:
    """
    Build Schedule M-1 reconciliation.

    Args:
        client: QuickBooks client
        tax_year: Tax year
        tax_adjustments: Dict of known book-tax differences
    """
    pnl = get_profit_and_loss(client, f"{tax_year}-01-01", f"{tax_year}-12-31")
    net_income_books = pnl.get("net_income", 0)

    m1 = {
        "1": {"description": "Net income (loss) per books", "amount": net_income_books},
        "2": {"description": "Federal income tax per books", "amount": 0},
        "3": {"description": "Excess capital losses", "amount": 0},
        "4": {"description": "Income subject to tax not on books", "amount": 0},
        "5a": {"description": "Depreciation", "amount": 0},
        "5b": {"description": "Charitable contributions", "amount": 0},
        "5c": {"description": "Travel and entertainment", "amount": 0},
        "5d": {"description": "Other", "amount": 0},
        "6": {"description": "Total additions (lines 1-5)", "amount": 0},
        "7": {"description": "Income on books not on return", "amount": 0},
        "8": {"description": "Deductions on return not on books", "amount": 0},
        "9": {"description": "Total subtractions (lines 7-8)", "amount": 0},
        "10": {"description": "Taxable income (line 6 - line 9)", "amount": 0},
    }

    # Line 2: Federal income tax
    fed_tax = get_accounts_by_pattern(client, "Expense", "federal.*tax|income tax")
    m1["2"]["amount"] = sum(a["balance"] for a in fed_tax)

    # Line 5c: Meals (50% disallowed)
    meals = get_accounts_by_pattern(client, "Expense", "meal|entertainment")
    m1["5c"]["amount"] = sum(a["balance"] for a in meals) * 0.50

    # Line 5d: Penalties (100% disallowed)
    penalties = get_accounts_by_pattern(client, "Expense", "penalty|fine")
    m1["5d"]["amount"] = sum(a["balance"] for a in penalties)

    # Line 7: Tax-exempt interest
    exempt = get_accounts_by_pattern(client, "Other Income", "tax.exempt|muni")
    m1["7"]["amount"] = sum(a["balance"] for a in exempt)

    # Apply user adjustments
    for line, adj in tax_adjustments.items():
        if line in m1:
            m1[line]["amount"] = adj.get("amount", 0)

    # Calculate totals
    m1["6"]["amount"] = sum(m1[k]["amount"] for k in ["1", "2", "3", "4", "5a", "5b", "5c", "5d"])
    m1["9"]["amount"] = m1["7"]["amount"] + m1["8"]["amount"]
    m1["10"]["amount"] = m1["6"]["amount"] - m1["9"]["amount"]

    return m1
```

---

## Schedule M-2 - Retained Earnings Analysis

```python
def extract_schedule_m2(client, tax_year: int, dividends_paid: Decimal = 0) -> dict:
    """
    Build Schedule M-2 (Analysis of Unappropriated Retained Earnings).
    """
    re_accounts = Account.query(
        "SELECT * FROM Account WHERE AccountSubType = 'RetainedEarnings'",
        qb=client
    )

    if not re_accounts:
        raise ValueError("No Retained Earnings account found")

    re_account = re_accounts[0]
    boy_balance = get_account_balance_as_of(client, re_account.Id, f"{tax_year}-01-01")

    pnl = get_profit_and_loss(client, f"{tax_year}-01-01", f"{tax_year}-12-31")
    net_income = pnl.get("net_income", 0)

    eoy_balance = float(re_account.CurrentBalance or 0)

    m2 = {
        "1": {"description": "Balance at beginning of year", "amount": boy_balance},
        "2": {"description": "Net income (loss) per books", "amount": net_income},
        "3": {"description": "Other increases", "amount": 0},
        "4": {"description": "Total (lines 1-3)", "amount": boy_balance + net_income},
        "5a": {"description": "Distributions - Cash", "amount": float(dividends_paid)},
        "5b": {"description": "Distributions - Stock", "amount": 0},
        "5c": {"description": "Distributions - Property", "amount": 0},
        "6": {"description": "Other decreases", "amount": 0},
        "7": {"description": "Total distributions and decreases", "amount": float(dividends_paid)},
        "8": {"description": "Balance at end of year", "amount": 0}
    }

    m2["8"]["amount"] = m2["4"]["amount"] - m2["7"]["amount"]

    # Verify against actual
    variance = eoy_balance - m2["8"]["amount"]
    if abs(variance) > 0.01:
        m2["variance"] = {
            "calculated": m2["8"]["amount"],
            "actual": eoy_balance,
            "difference": variance
        }

    return m2
```

---

## Schedule C - Dividends Received Deduction

```python
@dataclass
class DividendReceived:
    """Dividend received from another corporation."""
    payer_name: str
    payer_ein: str
    ownership_pct: Decimal
    amount: Decimal
    qualified: bool


def calculate_dividends_received_deduction(
    dividends: list[DividendReceived],
    taxable_income_before_drd: Decimal
) -> dict:
    """
    Calculate Dividends Received Deduction for Schedule C.

    DRD rates:
    - Less than 20% ownership: 50% deduction
    - 20% to less than 80% ownership: 65% deduction
    - 80% or more ownership: 100% deduction
    """
    schedule_c = {
        "column_a": [],
        "column_b": [],
        "column_c": [],
        "totals": {
            "line_1": 0, "line_2": 0, "line_3": 0,
            "total_dividends": 0, "total_deduction": 0
        }
    }

    for div in dividends:
        if not div.qualified:
            continue

        if div.ownership_pct >= 80:
            rate = Decimal("1.00")
            line = "line_3"
        elif div.ownership_pct >= 20:
            rate = Decimal("0.65")
            line = "line_2"
        else:
            rate = Decimal("0.50")
            line = "line_1"

        deduction = div.amount * rate

        schedule_c["column_a"].append({"payer": div.payer_name, "amount": float(div.amount)})
        schedule_c["column_b"].append(float(rate * 100))
        schedule_c["column_c"].append(float(deduction))

        schedule_c["totals"][line] += float(div.amount)
        schedule_c["totals"]["total_dividends"] += float(div.amount)
        schedule_c["totals"]["total_deduction"] += float(deduction)

    # Taxable income limitation
    ti_limit = float(taxable_income_before_drd) * 0.65
    if schedule_c["totals"]["total_deduction"] > ti_limit:
        schedule_c["totals"]["limited_deduction"] = ti_limit
        schedule_c["totals"]["limitation_applied"] = True

    return schedule_c
```

---

## Complete Tax Data Export

```python
from datetime import datetime
from pathlib import Path
import json

def export_form_1120_package(
    client,
    tax_year: int,
    output_dir: str,
    tax_adjustments: dict = None,
    dividends_paid: Decimal = 0,
    dividends_received: list[DividendReceived] = None
) -> dict:
    """Export complete Form 1120 data package."""
    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)

    package = {
        "tax_year": tax_year,
        "company_id": client.company_id,
        "generated": datetime.now().isoformat()
    }

    package["page1"] = extract_form_1120_data(client, tax_year)
    package["schedule_l"] = extract_schedule_l(client, tax_year)
    package["schedule_m1"] = extract_schedule_m1(client, tax_year, tax_adjustments or {})
    package["schedule_m2"] = extract_schedule_m2(client, tax_year, dividends_paid)

    if dividends_received:
        taxable_income = package["schedule_m1"]["10"]["amount"]
        package["schedule_c"] = calculate_dividends_received_deduction(
            dividends_received, Decimal(str(taxable_income))
        )

    with open(output_path / f"form_1120_{tax_year}.json", "w") as f:
        json.dump(package, f, indent=2, default=str)

    return package
```

---

## Best Practices

1. **Run reports for fiscal year** - Use consistent date ranges
2. **Verify M-2 ties to Balance Sheet** - Retained earnings must reconcile
3. **Document all M-1 adjustments** - Keep workpapers for each book-tax difference
4. **Track permanent vs temporary** - Affects deferred tax calculations
5. **Review account classifications** - Correct QBO account types simplify mapping

---

## See Also

- [tax-form-1120-page1.md](tax-form-1120-page1.md) - Form 1120 Page 1 mappings
- [tax-estimated-payments.md](tax-estimated-payments.md) - Quarterly estimated tax
