# QuickBooks Online REST API Entity Reference

## Table of Contents

1. [Account](#account)
2. [JournalEntry](#journalentry)
3. [Invoice](#invoice)
4. [Bill](#bill)
5. [Payment](#payment)
6. [Customer](#customer)
7. [Vendor](#vendor)
8. [Deposit](#deposit)
9. [Transfer](#transfer)
10. [Query Operations](#query-operations)

---

## Account

Represents a financial account (asset, liability, equity, income, expense).

### Create Account

```http
POST /v3/company/{realmId}/account
Content-Type: application/json

{
  "Name": "Investment Account",
  "AccountType": "Other Current Asset",
  "AccountSubType": "Investments",
  "Description": "Short-term equity investments"
}
```

### Response

```json
{
  "Account": {
    "Id": "94",
    "Name": "Investment Account",
    "SyncToken": "0",
    "AccountType": "Other Current Asset",
    "AccountSubType": "Investments",
    "Classification": "Asset",
    "CurrentBalance": 0,
    "Active": true,
    "FullyQualifiedName": "Investment Account",
    "MetaData": {
      "CreateTime": "2024-01-15T10:30:00-08:00",
      "LastUpdatedTime": "2024-01-15T10:30:00-08:00"
    }
  }
}
```

### Python SDK

```python
from quickbooks.objects.account import Account

account = Account()
account.Name = "Investment Account"
account.AccountType = "Other Current Asset"
account.AccountSubType = "Investments"
account.Description = "Short-term equity investments"
account.save(qb=client)
```

### Account Types Reference

| AccountType | Valid AccountSubTypes |
|-------------|----------------------|
| Bank | CashOnHand, Checking, MoneyMarket, RentsHeldInTrust, Savings, TrustAccounts |
| Other Current Asset | AllowanceForBadDebts, DevelopmentCosts, EmployeeCashAdvances, Investments, LoansToOfficers, LoansToOthers, LoansToStockholders, PrepaidExpenses, Retainage, UndepositedFunds |
| Fixed Asset | AccumulatedDepletion, AccumulatedDepreciation, Buildings, DepletableAssets, FurnitureAndFixtures, Land, LeaseholdImprovements, MachineryAndEquipment, OtherFixedAssets, Vehicles |
| Other Asset | AccumulatedAmortization, Goodwill, IntangibleAssets, Licenses, OrganizationalCosts, OtherLongTermAssets |
| Accounts Receivable | AccountsReceivable |
| Accounts Payable | AccountsPayable |
| Credit Card | CreditCard |
| Other Current Liability | DirectDepositPayable, LineOfCredit, LoanPayable, OtherCurrentLiabilities, PayrollClearing, PayrollTaxPayable, PrepaidExpensesPayable, SalesTaxPayable, TrustAccountsLiabilities |
| Long Term Liability | NotesPayable, OtherLongTermLiabilities, ShareholderNotesPayable |
| Equity | AccumulatedAdjustment, CommonStock, OpeningBalanceEquity, OwnersEquity, PaidInCapitalOrSurplus, PartnerContributions, PartnerDistributions, PreferredStock, RetainedEarnings, TreasuryStock |
| Income | DiscountsRefundsGiven, NonProfitIncome, OtherPrimaryIncome, SalesOfProductIncome, ServiceFeeIncome, UnappliedCashPaymentIncome |
| Other Income | DividendIncome, InterestEarned, OtherInvestmentIncome, OtherMiscellaneousIncome, TaxExemptInterest |
| Expense | AdvertisingPromotional, Auto, BadDebts, BankCharges, CharitableContributions, CommissionsAndFees, CostOfLabor, DuesAndSubscriptions, Entertainment, EntertainmentMeals, Equipment, EquipmentRental, FinanceCosts, GlobalTaxExpense, Insurance, InterestPaid, LegalAndProfessionalFees, OfficeExpenses, OfficeSupplies, OtherBusinessExpenses, OtherMiscellaneousServiceCost, PayrollExpenses, PromotionalMeals, RentOrLeaseOfBuildings, RepairAndMaintenance, ShippingFreightAndDelivery, Stationery, Supplies, Taxes, Travel, TravelMeals, Utilities |
| Other Expense | Amortization, Depreciation, ExchangeGainOrLoss, OtherMiscellaneousExpense, PenaltiesAndSettlements |
| Cost of Goods Sold | CostOfLaborCos, EquipmentRentalCos, OtherCostsOfServiceCos, ShippingFreightDeliveryCos, SuppliesMaterialsCogs |

---

## JournalEntry

Manual adjustments to the general ledger. Lines must balance (debits = credits).

### Create Journal Entry

```http
POST /v3/company/{realmId}/journalentry
Content-Type: application/json

{
  "TxnDate": "2024-01-15",
  "PrivateNote": "Dividend income from AAPL",
  "Line": [
    {
      "DetailType": "JournalEntryLineDetail",
      "Amount": 500.00,
      "Description": "Dividend received",
      "JournalEntryLineDetail": {
        "PostingType": "Debit",
        "AccountRef": {
          "value": "35",
          "name": "Checking"
        }
      }
    },
    {
      "DetailType": "JournalEntryLineDetail",
      "Amount": 500.00,
      "Description": "Dividend income",
      "JournalEntryLineDetail": {
        "PostingType": "Credit",
        "AccountRef": {
          "value": "79",
          "name": "Dividend Income"
        }
      }
    }
  ]
}
```

### Response

```json
{
  "JournalEntry": {
    "Id": "123",
    "SyncToken": "0",
    "TxnDate": "2024-01-15",
    "PrivateNote": "Dividend income from AAPL",
    "Line": [
      {
        "Id": "0",
        "DetailType": "JournalEntryLineDetail",
        "Amount": 500.00,
        "Description": "Dividend received",
        "JournalEntryLineDetail": {
          "PostingType": "Debit",
          "AccountRef": {
            "value": "35",
            "name": "Checking"
          }
        }
      },
      {
        "Id": "1",
        "DetailType": "JournalEntryLineDetail",
        "Amount": 500.00,
        "Description": "Dividend income",
        "JournalEntryLineDetail": {
          "PostingType": "Credit",
          "AccountRef": {
            "value": "79",
            "name": "Dividend Income"
          }
        }
      }
    ],
    "TotalAmt": 500.00
  }
}
```

### Python SDK

```python
from quickbooks.objects.journalentry import JournalEntry, JournalEntryLine

je = JournalEntry()
je.TxnDate = "2024-01-15"
je.PrivateNote = "Dividend income from AAPL"

debit_line = JournalEntryLine()
debit_line.Amount = 500.00
debit_line.Description = "Dividend received"
debit_line.JournalEntryLineDetail = {
    "PostingType": "Debit",
    "AccountRef": {"value": "35", "name": "Checking"}
}

credit_line = JournalEntryLine()
credit_line.Amount = 500.00
credit_line.Description = "Dividend income"
credit_line.JournalEntryLineDetail = {
    "PostingType": "Credit",
    "AccountRef": {"value": "79", "name": "Dividend Income"}
}

je.Line = [debit_line, credit_line]
je.save(qb=client)
```

### Common Journal Entry Patterns

#### Investment Purchase

```python
# Buy $10,000 of stock
je.Line = [
    # Debit: Increase investment asset
    {"PostingType": "Debit", "AccountRef": {"value": "INV_ACCT"}, "Amount": 10000},
    # Credit: Decrease cash
    {"PostingType": "Credit", "AccountRef": {"value": "CHECKING"}, "Amount": 10000}
]
```

#### Investment Sale with Gain

```python
# Sell stock: cost basis $10,000, sale price $12,000
je.Line = [
    # Debit: Cash received
    {"PostingType": "Debit", "AccountRef": {"value": "CHECKING"}, "Amount": 12000},
    # Credit: Remove investment at cost
    {"PostingType": "Credit", "AccountRef": {"value": "INV_ACCT"}, "Amount": 10000},
    # Credit: Record gain
    {"PostingType": "Credit", "AccountRef": {"value": "CAPITAL_GAINS"}, "Amount": 2000}
]
```

#### Dividend Received

```python
je.Line = [
    # Debit: Cash received
    {"PostingType": "Debit", "AccountRef": {"value": "CHECKING"}, "Amount": 500},
    # Credit: Dividend income
    {"PostingType": "Credit", "AccountRef": {"value": "DIV_INCOME"}, "Amount": 500}
]
```

---

## Invoice

Customer billing document.

### Create Invoice

```http
POST /v3/company/{realmId}/invoice
Content-Type: application/json

{
  "CustomerRef": {"value": "1"},
  "TxnDate": "2024-01-15",
  "DueDate": "2024-02-15",
  "Line": [
    {
      "DetailType": "SalesItemLineDetail",
      "Amount": 100.00,
      "Description": "Consulting services",
      "SalesItemLineDetail": {
        "ItemRef": {"value": "1", "name": "Services"},
        "Qty": 1,
        "UnitPrice": 100
      }
    }
  ]
}
```

### Python SDK

```python
from quickbooks.objects.invoice import Invoice
from quickbooks.objects.detailline import SalesItemLine, SalesItemLineDetail

invoice = Invoice()
invoice.CustomerRef = {"value": customer.Id}
invoice.TxnDate = "2024-01-15"
invoice.DueDate = "2024-02-15"

line = SalesItemLine()
line.Amount = 100.00
line.Description = "Consulting services"
line.SalesItemLineDetail = SalesItemLineDetail()
line.SalesItemLineDetail.ItemRef = {"value": "1", "name": "Services"}
line.SalesItemLineDetail.Qty = 1
line.SalesItemLineDetail.UnitPrice = 100

invoice.Line = [line]
invoice.save(qb=client)

# Send invoice email
invoice.send(qb=client, send_to="customer@example.com")
```

---

## Bill

Vendor invoice (accounts payable).

### Create Bill

```http
POST /v3/company/{realmId}/bill
Content-Type: application/json

{
  "VendorRef": {"value": "56"},
  "TxnDate": "2024-01-15",
  "DueDate": "2024-02-15",
  "Line": [
    {
      "DetailType": "AccountBasedExpenseLineDetail",
      "Amount": 200.00,
      "Description": "Office supplies",
      "AccountBasedExpenseLineDetail": {
        "AccountRef": {"value": "7", "name": "Office Supplies"}
      }
    }
  ]
}
```

### Python SDK

```python
from quickbooks.objects.bill import Bill
from quickbooks.objects.detailline import AccountBasedExpenseLine, AccountBasedExpenseLineDetail

bill = Bill()
bill.VendorRef = {"value": vendor.Id}
bill.TxnDate = "2024-01-15"
bill.DueDate = "2024-02-15"

line = AccountBasedExpenseLine()
line.Amount = 200.00
line.Description = "Office supplies"
line.AccountBasedExpenseLineDetail = AccountBasedExpenseLineDetail()
line.AccountBasedExpenseLineDetail.AccountRef = {"value": "7", "name": "Office Supplies"}

bill.Line = [line]
bill.save(qb=client)
```

---

## Payment

Customer payment received.

### Create Payment

```http
POST /v3/company/{realmId}/payment
Content-Type: application/json

{
  "CustomerRef": {"value": "1"},
  "TotalAmt": 100.00,
  "Line": [
    {
      "Amount": 100.00,
      "LinkedTxn": [
        {
          "TxnId": "130",
          "TxnType": "Invoice"
        }
      ]
    }
  ]
}
```

### Python SDK

```python
from quickbooks.objects.payment import Payment

payment = Payment()
payment.CustomerRef = {"value": customer.Id}
payment.TotalAmt = 100.00
payment.Line = [
    {
        "Amount": 100.00,
        "LinkedTxn": [{"TxnId": invoice.Id, "TxnType": "Invoice"}]
    }
]
payment.save(qb=client)
```

---

## Customer

### Create Customer

```python
from quickbooks.objects.customer import Customer

customer = Customer()
customer.DisplayName = "ACME Corporation"
customer.CompanyName = "ACME Corporation"
customer.GivenName = "John"
customer.FamilyName = "Doe"
customer.PrimaryEmailAddr = {"Address": "john@acme.example.com"}
customer.PrimaryPhone = {"FreeFormNumber": "555-123-4567"}
customer.BillAddr = {
    "Line1": "123 Main St",
    "City": "San Francisco",
    "CountrySubDivisionCode": "CA",
    "PostalCode": "94105"
}
customer.save(qb=client)
```

---

## Vendor

### Create Vendor

```python
from quickbooks.objects.vendor import Vendor

vendor = Vendor()
vendor.DisplayName = "Office Depot"
vendor.CompanyName = "Office Depot"
vendor.PrimaryEmailAddr = {"Address": "ap@officedepot.example.com"}
vendor.save(qb=client)
```

---

## Deposit

Bank deposit combining multiple payments.

### Create Deposit

```http
POST /v3/company/{realmId}/deposit
Content-Type: application/json

{
  "DepositToAccountRef": {"value": "35", "name": "Checking"},
  "TxnDate": "2024-01-15",
  "Line": [
    {
      "Amount": 100.00,
      "LinkedTxn": [
        {"TxnId": "120", "TxnType": "Payment"}
      ]
    }
  ]
}
```

---

## Transfer

Move funds between accounts.

### Create Transfer

```http
POST /v3/company/{realmId}/transfer
Content-Type: application/json

{
  "FromAccountRef": {"value": "35", "name": "Checking"},
  "ToAccountRef": {"value": "36", "name": "Savings"},
  "Amount": 1000.00,
  "TxnDate": "2024-01-15"
}
```

### Python SDK

```python
from quickbooks.objects.transfer import Transfer

transfer = Transfer()
transfer.FromAccountRef = {"value": "35", "name": "Checking"}
transfer.ToAccountRef = {"value": "36", "name": "Savings"}
transfer.Amount = 1000.00
transfer.TxnDate = "2024-01-15"
transfer.save(qb=client)
```

---

## Query Operations

### Basic Query Syntax

```http
GET /v3/company/{realmId}/query?query=SELECT * FROM Account WHERE Active = true
```

### Python SDK Queries

```python
# Query all customers
customers = Customer.all(qb=client)

# Query with filter
invoices = Invoice.query(
    "SELECT * FROM Invoice WHERE TxnDate >= '2024-01-01'",
    qb=client
)

# Query with pagination
accounts = Account.query(
    "SELECT * FROM Account STARTPOSITION 1 MAXRESULTS 100",
    qb=client
)

# Count query
count = Invoice.count("SELECT COUNT(*) FROM Invoice", qb=client)
```

### Query Operators

| Operator | Example |
|----------|---------|
| = | `WHERE Name = 'Checking'` |
| != | `WHERE Active != false` |
| < | `WHERE Balance < 1000` |
| > | `WHERE TxnDate > '2024-01-01'` |
| <= | `WHERE Amount <= 500` |
| >= | `WHERE DueDate >= '2024-01-01'` |
| IN | `WHERE AccountType IN ('Bank', 'Credit Card')` |
| LIKE | `WHERE DisplayName LIKE 'John%'` |

### Query Pagination

```python
def get_all_invoices(client, start_date):
    """Paginate through all invoices."""
    all_invoices = []
    start = 1
    page_size = 1000

    while True:
        query = f"""
            SELECT * FROM Invoice
            WHERE TxnDate >= '{start_date}'
            STARTPOSITION {start} MAXRESULTS {page_size}
        """
        batch = Invoice.query(query, qb=client)

        if not batch:
            break

        all_invoices.extend(batch)
        start += page_size

    return all_invoices
```
