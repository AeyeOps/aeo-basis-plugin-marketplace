---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QuickBooks Online Financial Reports

Core financial reports: Profit & Loss, Balance Sheet, Cash Flow, General Ledger.

See also: [reports-analysis.md](reports-analysis.md) for Trial Balance, comparative reports, and Excel export.

## Contents

- [Available Reports](#available-reports)
- [Common Query Parameters](#common-query-parameters)
- [Profit and Loss Report](#profit-and-loss-report)
- [Balance Sheet Report](#balance-sheet-report)
- [General Ledger Report](#general-ledger-report)
- [Cash Flow Report](#cash-flow-report)
- [Best Practices](#best-practices)

---

## Available Reports

| Report | Endpoint | Description |
|--------|----------|-------------|
| Profit and Loss | `/reports/ProfitAndLoss` | Income statement (revenue, expenses, net income) |
| Balance Sheet | `/reports/BalanceSheet` | Assets, liabilities, equity at point in time |
| Cash Flow | `/reports/CashFlow` | Operating, investing, financing activities |
| General Ledger | `/reports/GeneralLedger` | All transactions by account |

---

## Common Query Parameters

All reports support these parameters:

| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| `start_date` | string | Report start (YYYY-MM-DD) | `2024-01-01` |
| `end_date` | string | Report end (YYYY-MM-DD) | `2024-12-31` |
| `date_macro` | string | Predefined date range | `This Fiscal Year` |
| `accounting_method` | string | `Cash` or `Accrual` | `Accrual` |
| `summarize_column_by` | string | Grouping method | `Month`, `Quarter`, `Year` |
| `customer` | string | Customer ID filter | `1,2,3` |
| `vendor` | string | Vendor ID filter | `5,10` |
| `department` | string | Department ID filter | `1` |
| `class` | string | Class ID filter | `2` |

### Date Macros

```
Today, Yesterday, This Week, Last Week, This Week-to-date, Last Week-to-date,
Next Week, Next 4 Weeks, This Month, Last Month, This Month-to-date,
Last Month-to-date, Next Month, This Fiscal Quarter, Last Fiscal Quarter,
This Fiscal Quarter-to-date, Last Fiscal Quarter-to-date, Next Fiscal Quarter,
This Fiscal Year, Last Fiscal Year, This Fiscal Year-to-date,
Last Fiscal Year-to-date, Next Fiscal Year
```

---

## Profit and Loss Report

### Endpoint

```
GET /v3/company/{realmId}/reports/ProfitAndLoss
```

### Python Example

```python
import requests

def get_profit_and_loss(
    client,
    start_date: str,
    end_date: str,
    accounting_method: str = "Accrual",
    summarize_by: str = "Total"
) -> dict:
    """
    Fetch Profit and Loss report.

    Args:
        client: QuickBooks client
        start_date: Start date (YYYY-MM-DD)
        end_date: End date (YYYY-MM-DD)
        accounting_method: 'Cash' or 'Accrual'
        summarize_by: 'Total', 'Month', 'Quarter', 'Year'

    Returns:
        Report data dict with Header, Rows, Columns
    """
    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/reports/ProfitAndLoss"

    params = {
        "start_date": start_date,
        "end_date": end_date,
        "accounting_method": accounting_method,
        "summarize_column_by": summarize_by,
        "minorversion": "65"
    }

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, params=params, headers=headers)
    response.raise_for_status()
    return response.json()
```

### Response Structure

```json
{
  "Header": {
    "ReportName": "ProfitAndLoss",
    "ReportBasis": "Accrual",
    "StartPeriod": "2024-01-01",
    "EndPeriod": "2024-12-31",
    "Currency": "USD",
    "SummarizeColumnsBy": "Total"
  },
  "Rows": {
    "Row": [
      {
        "Header": {"ColData": [{"value": "Income"}, {"value": ""}]},
        "Rows": {
          "Row": [
            {
              "ColData": [
                {"id": "79", "value": "Dividend Income"},
                {"value": "15000.00"}
              ],
              "type": "Data"
            }
          ]
        },
        "type": "Section",
        "group": "Income",
        "Summary": {
          "ColData": [{"value": "Total Income"}, {"value": "15000.00"}]
        }
      }
    ]
  },
  "Columns": {
    "Column": [
      {"ColType": "Account", "ColTitle": ""},
      {"ColType": "Money", "ColTitle": "Total"}
    ]
  }
}
```

### Parsing P&L Data

```python
def parse_profit_and_loss(report_data: dict) -> dict:
    """Extract key metrics from P&L report."""
    result = {
        "total_income": 0.0,
        "total_expenses": 0.0,
        "gross_profit": 0.0,
        "net_income": 0.0,
        "income_accounts": [],
        "expense_accounts": []
    }

    for row in report_data.get("Rows", {}).get("Row", []):
        group = row.get("group", "")

        if group == "Income":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["total_income"] = float(summary[1].get("value", 0) or 0)
            for sub_row in row.get("Rows", {}).get("Row", []):
                if sub_row.get("type") == "Data":
                    col_data = sub_row.get("ColData", [])
                    if len(col_data) >= 2:
                        result["income_accounts"].append({
                            "name": col_data[0].get("value"),
                            "id": col_data[0].get("id"),
                            "amount": float(col_data[1].get("value", 0) or 0)
                        })

        elif group == "Expenses":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["total_expenses"] = float(summary[1].get("value", 0) or 0)

        elif group == "GrossProfit":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["gross_profit"] = float(summary[1].get("value", 0) or 0)

        elif group == "NetIncome":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["net_income"] = float(summary[1].get("value", 0) or 0)

    return result
```

---

## Balance Sheet Report

### Endpoint

```
GET /v3/company/{realmId}/reports/BalanceSheet
```

### Python Example

```python
def get_balance_sheet(
    client,
    as_of_date: str,
    accounting_method: str = "Accrual"
) -> dict:
    """
    Fetch Balance Sheet report.

    Args:
        client: QuickBooks client
        as_of_date: Report date (YYYY-MM-DD)
        accounting_method: 'Cash' or 'Accrual'

    Returns:
        Report data dict
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/reports/BalanceSheet"

    params = {
        "end_date": as_of_date,
        "accounting_method": accounting_method,
        "minorversion": "65"
    }

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, params=params, headers=headers)
    response.raise_for_status()
    return response.json()


def parse_balance_sheet(report_data: dict) -> dict:
    """Extract key metrics from Balance Sheet."""
    result = {
        "total_assets": 0.0,
        "total_liabilities": 0.0,
        "total_equity": 0.0,
        "assets": {"current": [], "fixed": [], "other": []},
        "liabilities": {"current": [], "long_term": []},
        "equity": []
    }

    for row in report_data.get("Rows", {}).get("Row", []):
        group = row.get("group", "")

        if group == "TotalAssets":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["total_assets"] = float(summary[1].get("value", 0) or 0)

        elif group == "TotalLiabilities":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["total_liabilities"] = float(summary[1].get("value", 0) or 0)

        elif group == "TotalEquity":
            summary = row.get("Summary", {}).get("ColData", [])
            if len(summary) >= 2:
                result["total_equity"] = float(summary[1].get("value", 0) or 0)

    # Verify accounting equation
    calculated_equity = result["total_assets"] - result["total_liabilities"]
    if abs(calculated_equity - result["total_equity"]) > 0.01:
        print(f"Warning: Balance sheet imbalance detected")

    return result
```

---

## General Ledger Report

### Endpoint

```
GET /v3/company/{realmId}/reports/GeneralLedger
```

### Python Example

```python
def get_general_ledger(
    client,
    start_date: str,
    end_date: str,
    account_id: str = None,
    source_account_type: str = None
) -> dict:
    """
    Fetch General Ledger report.

    Args:
        client: QuickBooks client
        start_date: Start date (YYYY-MM-DD)
        end_date: End date (YYYY-MM-DD)
        account_id: Optional account ID filter
        source_account_type: Filter by account type (e.g., 'Bank')

    Returns:
        Report data dict with all transactions
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/reports/GeneralLedger"

    params = {
        "start_date": start_date,
        "end_date": end_date,
        "minorversion": "65",
        "columns": "account_name,tx_date,txn_type,doc_num,name,memo,subt_nat_amount,rbal_nat_amount"
    }

    if account_id:
        params["account"] = account_id
    if source_account_type:
        params["source_account_type"] = source_account_type

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, params=params, headers=headers)
    response.raise_for_status()
    return response.json()


def parse_general_ledger_transactions(report_data: dict) -> list[dict]:
    """Extract transactions from General Ledger report."""
    transactions = []

    for row in report_data.get("Rows", {}).get("Row", []):
        if row.get("type") == "Section":
            account_header = row.get("Header", {}).get("ColData", [])
            account_name = account_header[0].get("value") if account_header else "Unknown"
            account_id = account_header[0].get("id") if account_header else None

            for txn_row in row.get("Rows", {}).get("Row", []):
                if txn_row.get("type") == "Data":
                    col_data = txn_row.get("ColData", [])
                    transactions.append({
                        "account_name": account_name,
                        "account_id": account_id,
                        "date": col_data[0].get("value") if len(col_data) > 0 else None,
                        "type": col_data[1].get("value") if len(col_data) > 1 else None,
                        "doc_num": col_data[2].get("value") if len(col_data) > 2 else None,
                        "name": col_data[3].get("value") if len(col_data) > 3 else None,
                        "memo": col_data[4].get("value") if len(col_data) > 4 else None,
                        "amount": float(col_data[5].get("value") or 0) if len(col_data) > 5 else 0,
                        "balance": float(col_data[6].get("value") or 0) if len(col_data) > 6 else 0
                    })

    return transactions
```

---

## Cash Flow Report

### Endpoint

```
GET /v3/company/{realmId}/reports/CashFlow
```

### Python Example

```python
def get_cash_flow(
    client,
    start_date: str,
    end_date: str,
    summarize_by: str = "Total"
) -> dict:
    """
    Fetch Cash Flow statement.

    Args:
        client: QuickBooks client
        start_date: Start date (YYYY-MM-DD)
        end_date: End date (YYYY-MM-DD)
        summarize_by: 'Total', 'Month', 'Quarter', 'Year'

    Returns:
        Cash flow report data
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/reports/CashFlow"

    params = {
        "start_date": start_date,
        "end_date": end_date,
        "summarize_column_by": summarize_by,
        "minorversion": "65"
    }

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, params=params, headers=headers)
    response.raise_for_status()
    return response.json()


def parse_cash_flow(report_data: dict) -> dict:
    """Extract cash flow categories."""
    result = {
        "net_income": 0.0,
        "operating_activities": 0.0,
        "investing_activities": 0.0,
        "financing_activities": 0.0,
        "net_cash_increase": 0.0,
        "ending_cash": 0.0
    }

    for row in report_data.get("Rows", {}).get("Row", []):
        group = row.get("group", "")
        summary = row.get("Summary", {}).get("ColData", [])

        if not summary or len(summary) < 2:
            continue

        value = float(summary[1].get("value", 0) or 0)

        if group == "NetIncome":
            result["net_income"] = value
        elif group == "OperatingActivities":
            result["operating_activities"] = value
        elif group == "InvestingActivities":
            result["investing_activities"] = value
        elif group == "FinancingActivities":
            result["financing_activities"] = value
        elif group == "CashIncrease":
            result["net_cash_increase"] = value
        elif group == "EndingCash":
            result["ending_cash"] = value

    return result
```

---

## Best Practices

1. **Use minor versions** - Include `?minorversion=65` for latest features
2. **Specify columns** - Request only needed columns to reduce payload
3. **Cache reports** - Store generated reports to reduce API calls
4. **Handle empty data** - Check `NoReportData` option in response
5. **Use date macros** - For standard periods, use predefined ranges
6. **Verify totals** - Cross-check report totals for accuracy
