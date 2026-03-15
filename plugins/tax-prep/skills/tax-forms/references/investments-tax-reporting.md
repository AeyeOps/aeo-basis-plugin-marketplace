# Investment Tax Reporting

Brokerage reconciliation, 1099 processing, and investment income reporting.

See also: [investments-accounting.md](investments-accounting.md) for account structure and journal entries.

## Contents

- [Brokerage Reconciliation](#brokerage-reconciliation)
- [1099 Processing](#1099-processing)
- [Investment Income Summary](#investment-income-summary)
- [Best Practices](#best-practices)

---

## Brokerage Reconciliation

### Parse Brokerage Statement

```python
from dataclasses import dataclass
from decimal import Decimal
from datetime import date
from typing import Optional

@dataclass
class Position:
    """Investment position from brokerage."""
    symbol: str
    shares: Decimal
    cost_basis: Decimal
    market_value: Decimal
    unrealized_gain: Decimal
    account_type: str  # "equity", "fixed_income", "cash"


@dataclass
class Transaction:
    """Investment transaction from brokerage."""
    date: date
    type: str  # "buy", "sell", "dividend", "interest", "fee"
    symbol: Optional[str]
    shares: Optional[Decimal]
    amount: Decimal
    description: str


def parse_brokerage_csv(file_path: str) -> tuple[list[Position], list[Transaction]]:
    """
    Parse brokerage statement CSV.

    Adjust column mappings based on broker format
    (column names vary by custodian).
    """
    import csv

    positions = []
    transactions = []

    # Template - adjust for your broker's format
    with open(file_path, newline="", encoding="utf-8") as f:
        reader = csv.DictReader(f)

        for row in reader:
            if "Symbol" in row and "Shares" in row:
                positions.append(Position(
                    symbol=row["Symbol"],
                    shares=Decimal(row.get("Shares", "0").replace(",", "")),
                    cost_basis=Decimal(row.get("Cost Basis", "0").replace(",", "").replace("$", "")),
                    market_value=Decimal(row.get("Market Value", "0").replace(",", "").replace("$", "")),
                    unrealized_gain=Decimal(row.get("Gain/Loss", "0").replace(",", "").replace("$", "")),
                    account_type="equity"
                ))

    return positions, transactions
```

### Reconcile Positions

```python
def reconcile_brokerage_to_gl(
    brokerage_positions: list[Position],
    gl_investment_balance: float,
    as_of_date: str
) -> dict:
    """
    Compare brokerage positions to GL investment account balance.

    Returns reconciliation report. For platform-specific GL balance
    retrieval, invoke the appropriate qbo-integration skill.
    """
    brokerage_cost_basis = sum(float(p.cost_basis) for p in brokerage_positions)
    brokerage_market_value = sum(float(p.market_value) for p in brokerage_positions)

    difference = gl_investment_balance - brokerage_cost_basis

    return {
        "as_of_date": as_of_date,
        "gl_balance": gl_investment_balance,
        "brokerage_cost_basis": brokerage_cost_basis,
        "brokerage_market_value": brokerage_market_value,
        "difference": difference,
        "is_reconciled": abs(difference) < 0.01,
        "unrealized_gain": brokerage_market_value - brokerage_cost_basis,
        "positions": [
            {
                "symbol": p.symbol,
                "shares": float(p.shares),
                "cost_basis": float(p.cost_basis),
                "market_value": float(p.market_value)
            }
            for p in brokerage_positions
        ]
    }
```

---

## 1099 Processing

### Process 1099-DIV

```python
def process_1099_div(
    data: dict,
    tax_year: int,
    account_mapping: dict
) -> list:
    """
    Process 1099-DIV and produce journal entry specifications.

    Args:
        data: Dict with 1099-DIV box values
        tax_year: Tax year
        account_mapping: Map of box numbers to GL account names

    1099-DIV Boxes:
        1a: Total ordinary dividends
        1b: Qualified dividends (subset of 1a)
        2a: Capital gain distributions
        3:  Nondividend distributions (return of capital)
        4:  Federal tax withheld
        11: Tax-exempt interest dividends

    For platform-specific journal entry creation, invoke the
    appropriate qbo-integration skill.
    """
    entries = []

    # Box 1a: Total ordinary dividends
    if data.get("1a", 0) > 0:
        qualified = data.get("1b", 0)
        ordinary = data["1a"] - qualified

        if qualified > 0:
            entries.append({
                "date": f"{tax_year}-12-31",
                "amount": qualified,
                "account": account_mapping["qualified_dividends"],
                "description": "1099-DIV Box 1b - Qualified Dividends"
            })

        if ordinary > 0:
            entries.append({
                "date": f"{tax_year}-12-31",
                "amount": ordinary,
                "account": account_mapping["ordinary_dividends"],
                "description": "1099-DIV Box 1a - Ordinary Dividends (non-qualified)"
            })

    # Box 2a: Capital gain distributions
    if data.get("2a", 0) > 0:
        entries.append({
            "date": f"{tax_year}-12-31",
            "amount": data["2a"],
            "account": account_mapping["capital_gains_dist"],
            "description": "1099-DIV Box 2a - Capital Gain Distributions"
        })

    # Box 3: Nondividend distributions (return of capital)
    if data.get("3", 0) > 0:
        entries.append({
            "date": f"{tax_year}-12-31",
            "amount": data["3"],
            "account": account_mapping["investment_account"],
            "description": "1099-DIV Box 3 - Return of Capital (reduces basis)"
        })

    # Box 4: Federal tax withheld
    if data.get("4", 0) > 0:
        entries.append({
            "date": f"{tax_year}-12-31",
            "amount": data["4"],
            "account": account_mapping["fed_tax_withheld"],
            "description": "1099-DIV Box 4 - Federal Tax Withheld"
        })

    # Box 11: Tax-exempt interest dividends
    if data.get("11", 0) > 0:
        entries.append({
            "date": f"{tax_year}-12-31",
            "amount": data["11"],
            "account": account_mapping["tax_exempt_interest"],
            "description": "1099-DIV Box 11 - Exempt Interest Dividends"
        })

    return entries
```

### Process 1099-B (Sales)

```python
def process_1099_b(
    client,
    sales: list[dict],
    tax_year: int,
    account_mapping: dict
) -> list:
    """
    Process 1099-B sales and create capital gain/loss entries.

    Args:
        sales: List of sale transactions with:
            - symbol, shares, proceeds, cost_basis, date_sold,
            - date_acquired, gain_loss, holding_period
    """
    entries = []

    # Aggregate by holding period
    short_term_gains = 0.0
    short_term_losses = 0.0
    long_term_gains = 0.0
    long_term_losses = 0.0

    for sale in sales:
        gain_loss = sale["gain_loss"]
        is_long_term = sale.get("holding_period", "short") == "long"

        if gain_loss >= 0:
            if is_long_term:
                long_term_gains += gain_loss
            else:
                short_term_gains += gain_loss
        else:
            if is_long_term:
                long_term_losses += abs(gain_loss)
            else:
                short_term_losses += abs(gain_loss)

    # Create aggregate entries
    if short_term_gains > 0:
        entries.append(create_capital_gain_entry(
            client, f"{tax_year}-12-31", short_term_gains,
            account_mapping["st_gains"], "Short-term capital gains"
        ))

    if short_term_losses > 0:
        entries.append(create_capital_loss_entry(
            client, f"{tax_year}-12-31", short_term_losses,
            account_mapping["st_losses"], "Short-term capital losses"
        ))

    if long_term_gains > 0:
        entries.append(create_capital_gain_entry(
            client, f"{tax_year}-12-31", long_term_gains,
            account_mapping["lt_gains"], "Long-term capital gains"
        ))

    if long_term_losses > 0:
        entries.append(create_capital_loss_entry(
            client, f"{tax_year}-12-31", long_term_losses,
            account_mapping["lt_losses"], "Long-term capital losses"
        ))

    return entries
```

---

## Investment Income Summary

```python
def generate_investment_income_report(
    account_balances: list[dict],
    start_date: str,
    end_date: str
) -> dict:
    """
    Generate investment income summary for tax reporting.

    Args:
        account_balances: List of dicts with 'name' and 'balance' keys,
            extracted from the GL. For platform-specific extraction,
            invoke the appropriate qbo-integration skill.
        start_date: Period start (YYYY-MM-DD)
        end_date: Period end (YYYY-MM-DD)
    """
    income_summary = {
        "period": {"start": start_date, "end": end_date},
        "qualified_dividends": 0.0,
        "ordinary_dividends": 0.0,
        "interest_taxable": 0.0,
        "interest_tax_exempt": 0.0,
        "capital_gains_st": 0.0,
        "capital_gains_lt": 0.0,
        "capital_losses_st": 0.0,
        "capital_losses_lt": 0.0,
        "total_investment_income": 0.0
    }

    for account in account_balances:
        balance = float(account.get("balance", 0))
        name_lower = account.get("name", "").lower()

        if "qualified" in name_lower and "dividend" in name_lower:
            income_summary["qualified_dividends"] += balance
        elif "dividend" in name_lower:
            income_summary["ordinary_dividends"] += balance
        elif "tax-exempt" in name_lower or "tax exempt" in name_lower:
            income_summary["interest_tax_exempt"] += balance
        elif "interest" in name_lower:
            income_summary["interest_taxable"] += balance
        elif "short" in name_lower and "gain" in name_lower:
            income_summary["capital_gains_st"] += balance
        elif "long" in name_lower and "gain" in name_lower:
            income_summary["capital_gains_lt"] += balance

    income_summary["total_investment_income"] = (
        income_summary["qualified_dividends"] +
        income_summary["ordinary_dividends"] +
        income_summary["interest_taxable"] +
        income_summary["interest_tax_exempt"] +
        income_summary["capital_gains_st"] +
        income_summary["capital_gains_lt"]
    )

    return income_summary
```

---

## Best Practices

1. **Track tax-exempt interest** - Separate reporting for muni bonds
2. **Reconcile monthly** - Match brokerage statements to the GL
3. **Attach 1099s** - Link tax documents to year-end entries
4. **Aggregate by holding period** - Separate short vs long-term for Schedule D
5. **Document cost basis** - Maintain records for audit support
