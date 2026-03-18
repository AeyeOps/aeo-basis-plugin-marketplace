---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QuickBooks Online GraphQL API Patterns

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Basic Queries](#basic-queries)
4. [Filtering](#filtering)
5. [Pagination](#pagination)
6. [Mutations](#mutations)
7. [Common Patterns](#common-patterns)

---

## Overview

QBO GraphQL API provides flexible querying with:
- Single request for related data
- Field selection (no over-fetching)
- Cursor-based pagination
- Type-safe schema

### Base URL

| Environment | Endpoint |
|-------------|----------|
| Production | `https://public.api.intuit.com/2020-04/graphql` |

### Headers

```http
POST /2020-04/graphql
Authorization: Bearer {access_token}
Content-Type: application/json
```

---

## Authentication

Same OAuth 2.0 tokens as REST API.

```python
import requests

def graphql_query(query: str, variables: dict = None) -> dict:
    """Execute GraphQL query."""
    headers = {
        "Authorization": f"Bearer {access_token}",
        "Content-Type": "application/json"
    }
    payload = {"query": query}
    if variables:
        payload["variables"] = variables

    response = requests.post(
        "https://public.api.intuit.com/2020-04/graphql",
        headers=headers,
        json=payload
    )
    return response.json()
```

---

## Basic Queries

### Company Information

```graphql
query getCompanyInformation {
  company {
    id
    companyName
    companyStartDate
    country
    fiscalYearStartMonth
    primaryPhone {
      freeFormNumber
    }
    primaryAddress {
      line1
      city
      countrySubDivisionCode
      postalCode
    }
  }
}
```

### Query Invoices

```graphql
query getInvoices {
  company {
    invoices {
      nodes {
        id
        docNumber
        txnDate
        dueDate
        totalAmt
        balance
        status
        customer {
          displayName
        }
      }
    }
  }
}
```

### Query Accounts

```graphql
query getAccounts {
  company {
    accounts {
      nodes {
        id
        name
        accountType
        accountSubType
        classification
        currentBalance
        active
      }
    }
  }
}
```

### Query Customers

```graphql
query getCustomers {
  company {
    customers {
      nodes {
        id
        displayName
        companyName
        primaryEmailAddr {
          address
        }
        balance
        active
      }
    }
  }
}
```

---

## Filtering

### Date Range Filter

```graphql
query getInvoicesByDateRange($startDate: String!, $endDate: String!) {
  company {
    invoices(filter: {
      txnDate: { between: [$startDate, $endDate] }
    }) {
      nodes {
        id
        docNumber
        txnDate
        totalAmt
        balance
      }
    }
  }
}
```

Variables:
```json
{
  "startDate": "2024-01-01",
  "endDate": "2024-12-31"
}
```

### Filter by Status

```graphql
query getOpenInvoices {
  company {
    invoices(filter: {
      status: { equals: OPEN }
    }) {
      nodes {
        id
        docNumber
        balance
        customer {
          displayName
        }
      }
    }
  }
}
```

### Filter by Amount

```graphql
query getLargeInvoices {
  company {
    invoices(filter: {
      totalAmt: { greaterThan: 1000 }
    }) {
      nodes {
        id
        docNumber
        totalAmt
      }
    }
  }
}
```

### Combined Filters

```graphql
query getOverdueInvoices($today: String!) {
  company {
    invoices(filter: {
      dueDate: { lessThan: $today }
      balance: { greaterThan: 0 }
    }) {
      nodes {
        id
        docNumber
        dueDate
        balance
        customer {
          displayName
          primaryEmailAddr {
            address
          }
        }
      }
    }
  }
}
```

---

## Pagination

### Cursor-Based Pagination

```graphql
query getTransactions($first: Int!, $after: String) {
  company {
    transactions(
      pagination: { first: $first, after: $after }
    ) {
      pageInfo {
        hasNextPage
        hasPreviousPage
        startCursor
        endCursor
      }
      nodes {
        id
        txnDate
        txnType
        amount
      }
    }
  }
}
```

### Python Pagination Helper

```python
def paginate_graphql(query: str, node_path: str, page_size: int = 100):
    """Paginate through GraphQL results."""
    all_nodes = []
    cursor = None

    while True:
        variables = {"first": page_size, "after": cursor}
        result = graphql_query(query, variables)

        # Navigate to nodes
        data = result.get("data", {})
        for key in node_path.split("."):
            data = data.get(key, {})

        nodes = data.get("nodes", [])
        page_info = data.get("pageInfo", {})

        all_nodes.extend(nodes)

        if not page_info.get("hasNextPage"):
            break

        cursor = page_info.get("endCursor")

    return all_nodes

# Usage
invoices = paginate_graphql(
    INVOICES_QUERY,
    "company.invoices",
    page_size=100
)
```

---

## Mutations

### Create Customer

```graphql
mutation createCustomer($input: CreateCustomerInput!) {
  createCustomer(customer: $input) {
    id
    displayName
    primaryEmailAddr {
      address
    }
  }
}
```

Variables:
```json
{
  "input": {
    "displayName": "ACME Corporation",
    "companyName": "ACME Corporation",
    "primaryEmailAddr": {
      "address": "billing@acme.example.com"
    }
  }
}
```

### Update Invoice

```graphql
mutation updateInvoice($id: ID!, $input: UpdateInvoiceInput!) {
  updateInvoice(id: $id, invoice: $input) {
    id
    docNumber
    dueDate
    balance
  }
}
```

---

## Common Patterns

### Invoice with Payment Details

```graphql
query getInvoiceWithPayments($invoiceId: ID!) {
  company {
    invoice(id: $invoiceId) {
      id
      docNumber
      txnDate
      totalAmt
      balance
      customer {
        displayName
      }
      payments {
        id
        txnDate
        totalAmt
        paymentType
      }
    }
  }
}
```

### Customer with Open Invoices

```graphql
query getCustomerWithOpenInvoices($customerId: ID!) {
  company {
    customer(id: $customerId) {
      id
      displayName
      balance
      invoices(filter: { balance: { greaterThan: 0 } }) {
        nodes {
          id
          docNumber
          dueDate
          balance
        }
      }
    }
  }
}
```

### Account Balances Summary

```graphql
query getAccountBalances {
  company {
    accounts(filter: { active: { equals: true } }) {
      nodes {
        id
        name
        accountType
        classification
        currentBalance
      }
    }
  }
}
```

### Transaction History

```graphql
query getTransactionHistory($startDate: String!, $endDate: String!) {
  company {
    transactions(filter: {
      txnDate: { between: [$startDate, $endDate] }
    }) {
      nodes {
        id
        txnType
        txnDate
        amount
        ... on Invoice {
          docNumber
          customer { displayName }
        }
        ... on Bill {
          docNumber
          vendor { displayName }
        }
        ... on JournalEntry {
          privateNote
        }
      }
    }
  }
}
```

---

## Python GraphQL Client

### Full Example

```python
import requests
from dataclasses import dataclass
from typing import Optional, List, Dict, Any


@dataclass
class QBOGraphQLClient:
    access_token: str
    base_url: str = "https://public.api.intuit.com/2020-04/graphql"

    def execute(self, query: str, variables: Optional[Dict] = None) -> Dict[str, Any]:
        """Execute a GraphQL query."""
        headers = {
            "Authorization": f"Bearer {self.access_token}",
            "Content-Type": "application/json"
        }

        payload = {"query": query}
        if variables:
            payload["variables"] = variables

        response = requests.post(self.base_url, headers=headers, json=payload)
        response.raise_for_status()

        result = response.json()
        if "errors" in result:
            raise Exception(f"GraphQL errors: {result['errors']}")

        return result.get("data", {})

    def get_company_info(self) -> Dict:
        """Get company information."""
        query = """
        query {
          company {
            id
            companyName
            country
            fiscalYearStartMonth
          }
        }
        """
        return self.execute(query)

    def get_invoices(
        self,
        start_date: Optional[str] = None,
        end_date: Optional[str] = None,
        status: Optional[str] = None
    ) -> List[Dict]:
        """Get invoices with optional filters."""
        filters = []
        if start_date and end_date:
            filters.append(f'txnDate: {{ between: ["{start_date}", "{end_date}"] }}')
        if status:
            filters.append(f'status: {{ equals: {status} }}')

        filter_clause = f"filter: {{ {', '.join(filters)} }}" if filters else ""

        query = f"""
        query {{
          company {{
            invoices({filter_clause}) {{
              nodes {{
                id
                docNumber
                txnDate
                dueDate
                totalAmt
                balance
                status
                customer {{
                  displayName
                }}
              }}
            }}
          }}
        }}
        """
        result = self.execute(query)
        return result.get("company", {}).get("invoices", {}).get("nodes", [])

    def get_accounts_by_type(self, account_type: str) -> List[Dict]:
        """Get accounts filtered by type."""
        query = f"""
        query {{
          company {{
            accounts(filter: {{ accountType: {{ equals: {account_type} }} }}) {{
              nodes {{
                id
                name
                currentBalance
                active
              }}
            }}
          }}
        }}
        """
        result = self.execute(query)
        return result.get("company", {}).get("accounts", {}).get("nodes", [])


# Usage
client = QBOGraphQLClient(access_token="your_access_token")
invoices = client.get_invoices(start_date="2024-01-01", end_date="2024-12-31")
```

---

## REST vs GraphQL Comparison

| Scenario | REST | GraphQL |
|----------|------|---------|
| Get invoice with customer | 2 requests | 1 request |
| Get specific fields only | Returns all fields | Returns only requested |
| Complex filtering | Limited operators | Rich filter syntax |
| Pagination | STARTPOSITION/MAXRESULTS | Cursor-based |
| Real-time updates | Webhooks | Webhooks (same) |

### When to Use GraphQL

- Need related data in single request
- Want to minimize data transfer
- Complex filtering requirements
- Building dashboards/reports

### When to Use REST

- Simple CRUD operations
- Batch operations (GraphQL doesn't support batch)
- Already have REST integration
- Need webhooks (both support)
