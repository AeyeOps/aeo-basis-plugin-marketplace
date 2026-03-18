---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QuickBooks Online Analysis Reports

Trial Balance, Account List, comparative reports, and Excel export.

See also: [reports-financial.md](reports-financial.md) for P&L, Balance Sheet, Cash Flow, and GL.

## Contents

- [Available Reports](#available-reports)
- [Trial Balance Report](#trial-balance-report)
- [Account List Detail Report](#account-list-detail-report)
- [Multi-Period Comparative Reports](#multi-period-comparative-reports)
- [Export to Excel](#export-to-excel)
- [Error Handling](#error-handling)
- [Best Practices](#best-practices)

---

## Available Reports

| Report | Endpoint | Description |
|--------|----------|-------------|
| Trial Balance | `/reports/TrialBalance` | Account balances verification |
| Account List Detail | `/reports/AccountListDetail` | Chart of accounts with metadata |
| A/R Aging Summary | `/reports/AgedReceivables` | Customer receivables aging |
| A/P Aging Summary | `/reports/AgedPayables` | Vendor payables aging |

---

## Trial Balance Report

### Endpoint

```
GET /v3/company/{realmId}/reports/TrialBalance
```

### Python Example

```python
import requests

def get_trial_balance(
    client,
    as_of_date: str,
    accounting_method: str = "Accrual"
) -> dict:
    """
    Fetch Trial Balance report.

    Args:
        client: QuickBooks client
        as_of_date: Report date (YYYY-MM-DD)
        accounting_method: 'Cash' or 'Accrual'

    Returns:
        Trial balance data with debit/credit columns
    """
    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/reports/TrialBalance"

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


def verify_trial_balance(report_data: dict) -> tuple[float, float, bool]:
    """
    Verify trial balance totals.

    Returns:
        Tuple of (total_debits, total_credits, is_balanced)
    """
    total_debits = 0.0
    total_credits = 0.0

    for row in report_data.get("Rows", {}).get("Row", []):
        if row.get("group") == "GrandTotal":
            col_data = row.get("Summary", {}).get("ColData", [])
            if len(col_data) >= 3:
                total_debits = float(col_data[1].get("value", 0) or 0)
                total_credits = float(col_data[2].get("value", 0) or 0)
            break

    is_balanced = abs(total_debits - total_credits) < 0.01
    return total_debits, total_credits, is_balanced
```

---

## Account List Detail Report

### Endpoint

```
GET /v3/company/{realmId}/reports/AccountListDetail
```

### Python Example

```python
def get_account_list_detail(
    client,
    account_type: str = None,
    account_status: str = "Not_Deleted"
) -> dict:
    """
    Fetch detailed Chart of Accounts.

    Args:
        client: QuickBooks client
        account_type: Filter by type (Bank, Income, Expense, etc.)
        account_status: 'Deleted' or 'Not_Deleted'

    Returns:
        Account list with full metadata
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/reports/AccountListDetail"

    params = {
        "account_status": account_status,
        "columns": "account_name,account_type,detail_acc_type,account_bal,account_desc",
        "minorversion": "65"
    }

    if account_type:
        params["account_type"] = account_type

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, params=params, headers=headers)
    response.raise_for_status()
    return response.json()
```

---

## Multi-Period Comparative Reports

### Monthly Comparison

```python
def get_monthly_pl_comparison(client, year: int) -> dict:
    """Get P&L with monthly columns for year."""
    from reports_financial import get_profit_and_loss

    return get_profit_and_loss(
        client,
        start_date=f"{year}-01-01",
        end_date=f"{year}-12-31",
        summarize_by="Month"
    )


def get_quarterly_pl_comparison(client, year: int) -> dict:
    """Get P&L with quarterly columns for year."""
    from reports_financial import get_profit_and_loss

    return get_profit_and_loss(
        client,
        start_date=f"{year}-01-01",
        end_date=f"{year}-12-31",
        summarize_by="Quarter"
    )
```

### Year-Over-Year Comparison

```python
def compare_years(client, years: list[int]) -> dict:
    """Compare P&L across multiple years."""
    from reports_financial import get_profit_and_loss, parse_profit_and_loss

    results = {}

    for year in years:
        report = get_profit_and_loss(
            client,
            start_date=f"{year}-01-01",
            end_date=f"{year}-12-31"
        )
        results[year] = parse_profit_and_loss(report)

    return results
```

---

## Export to Excel

```python
import openpyxl
from openpyxl.styles import Font, Alignment
from datetime import datetime

def export_report_to_excel(
    report_data: dict,
    output_path: str,
    report_name: str
) -> None:
    """Export QBO report to formatted Excel file."""
    wb = openpyxl.Workbook()
    ws = wb.active
    ws.title = report_name[:31]  # Excel sheet name limit

    # Header
    header = report_data.get("Header", {})
    ws["A1"] = header.get("ReportName", report_name)
    ws["A1"].font = Font(bold=True, size=14)
    ws["A2"] = f"Period: {header.get('StartPeriod', '')} to {header.get('EndPeriod', '')}"
    ws["A3"] = f"Generated: {datetime.now().strftime('%Y-%m-%d %H:%M')}"

    # Column headers
    columns = report_data.get("Columns", {}).get("Column", [])
    col_titles = [c.get("ColTitle", "") or c.get("ColType", "") for c in columns]
    for col_idx, title in enumerate(col_titles, start=1):
        cell = ws.cell(row=5, column=col_idx, value=title)
        cell.font = Font(bold=True)

    # Data rows
    row_num = 6
    for row in report_data.get("Rows", {}).get("Row", []):
        row_num = write_report_row(ws, row, row_num, 0)

    # Auto-adjust column widths
    for column in ws.columns:
        max_length = 0
        column_letter = column[0].column_letter
        for cell in column:
            if cell.value:
                max_length = max(max_length, len(str(cell.value)))
        ws.column_dimensions[column_letter].width = min(max_length + 2, 50)

    wb.save(output_path)


def write_report_row(ws, row_data: dict, row_num: int, indent: int) -> int:
    """Recursively write report rows with indentation."""
    # Write header if present
    header = row_data.get("Header", {}).get("ColData", [])
    if header:
        for col_idx, col_data in enumerate(header, start=1):
            value = col_data.get("value", "")
            cell = ws.cell(row=row_num, column=col_idx, value=value)
            if col_idx == 1:
                cell.alignment = Alignment(indent=indent)
                cell.font = Font(bold=True)
        row_num += 1

    # Write nested rows
    for sub_row in row_data.get("Rows", {}).get("Row", []):
        if sub_row.get("type") == "Data":
            col_data = sub_row.get("ColData", [])
            for col_idx, col in enumerate(col_data, start=1):
                value = col.get("value", "")
                cell = ws.cell(row=row_num, column=col_idx, value=value)
                if col_idx == 1:
                    cell.alignment = Alignment(indent=indent + 1)
            row_num += 1
        elif sub_row.get("type") == "Section":
            row_num = write_report_row(ws, sub_row, row_num, indent + 1)

    # Write summary if present
    summary = row_data.get("Summary", {}).get("ColData", [])
    if summary:
        for col_idx, col_data in enumerate(summary, start=1):
            value = col_data.get("value", "")
            cell = ws.cell(row=row_num, column=col_idx, value=value)
            if col_idx == 1:
                cell.alignment = Alignment(indent=indent)
            cell.font = Font(bold=True)
        row_num += 1

    return row_num
```

---

## Error Handling

```python
import requests
from requests.exceptions import HTTPError

class ReportError(Exception):
    """Report generation error."""
    pass


def fetch_report_safely(url: str, params: dict, headers: dict) -> dict:
    """Fetch report with error handling."""
    try:
        response = requests.get(url, params=params, headers=headers, timeout=60)
        response.raise_for_status()

        data = response.json()

        # Check for empty report
        for option in data.get("Header", {}).get("Option", []):
            if option.get("Name") == "NoReportData" and option.get("Value") == "true":
                raise ReportError("No data available for the specified period")

        return data

    except HTTPError as e:
        if e.response.status_code == 401:
            raise ReportError("Authentication failed - refresh token")
        elif e.response.status_code == 429:
            raise ReportError("Rate limit exceeded - retry after delay")
        else:
            raise ReportError(f"HTTP {e.response.status_code}: {e.response.text}")
    except requests.exceptions.Timeout:
        raise ReportError("Request timed out - report may be too large")
```

---

## Best Practices

1. **Export for audit** - Always export reports to Excel for records
2. **Compare periods** - Use summarize_column_by for trend analysis
3. **Verify trial balance** - Always check that debits equal credits
4. **Handle timeouts** - Large reports may take time; use appropriate timeouts
5. **Cache results** - Store reports to reduce API calls
