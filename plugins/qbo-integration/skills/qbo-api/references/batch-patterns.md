---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QuickBooks Online Batch Operations

## Table of Contents

1. [Overview](#overview)
2. [Batch Request Format](#batch-request-format)
3. [Supported Operations](#supported-operations)
4. [Python Implementation](#python-implementation)
5. [Best Practices](#best-practices)
6. [Error Handling](#error-handling)

---

## Overview

Batch operations allow up to 30 requests in a single API call, reducing network overhead and improving performance for bulk operations.

### Limits

| Constraint | Value |
|------------|-------|
| Max requests per batch | 30 |
| Max batch requests per minute | 40 |
| Request timeout | 5 minutes |

---

## Batch Request Format

### Endpoint

```http
POST /v3/company/{realmId}/batch
Content-Type: application/json
```

### Request Structure

```json
{
  "BatchItemRequest": [
    {
      "bId": "unique-id-1",
      "operation": "create",
      "JournalEntry": {
        "TxnDate": "2024-01-15",
        "Line": [...]
      }
    },
    {
      "bId": "unique-id-2",
      "operation": "query",
      "Query": "SELECT * FROM Account WHERE Active = true"
    }
  ]
}
```

### Response Structure

```json
{
  "BatchItemResponse": [
    {
      "bId": "unique-id-1",
      "JournalEntry": {
        "Id": "123",
        "SyncToken": "0",
        ...
      }
    },
    {
      "bId": "unique-id-2",
      "QueryResponse": {
        "Account": [...],
        "startPosition": 1,
        "maxResults": 10
      }
    }
  ]
}
```

---

## Supported Operations

### Create

```json
{
  "bId": "create-1",
  "operation": "create",
  "JournalEntry": {
    "TxnDate": "2024-01-15",
    "PrivateNote": "Monthly dividend",
    "Line": [
      {
        "DetailType": "JournalEntryLineDetail",
        "Amount": 500.00,
        "JournalEntryLineDetail": {
          "PostingType": "Debit",
          "AccountRef": {"value": "35"}
        }
      },
      {
        "DetailType": "JournalEntryLineDetail",
        "Amount": 500.00,
        "JournalEntryLineDetail": {
          "PostingType": "Credit",
          "AccountRef": {"value": "79"}
        }
      }
    ]
  }
}
```

### Update

```json
{
  "bId": "update-1",
  "operation": "update",
  "Customer": {
    "Id": "1",
    "SyncToken": "0",
    "DisplayName": "Updated Name",
    "sparse": true
  }
}
```

### Delete

```json
{
  "bId": "delete-1",
  "operation": "delete",
  "Invoice": {
    "Id": "130",
    "SyncToken": "1"
  }
}
```

### Query

```json
{
  "bId": "query-1",
  "operation": "query",
  "Query": "SELECT * FROM Customer WHERE Active = true MAXRESULTS 100"
}
```

---

## Python Implementation

### Basic Batch Client

```python
import requests
from typing import List, Dict, Any
from dataclasses import dataclass, field


@dataclass
class BatchRequest:
    """Single request within a batch."""
    bId: str
    operation: str  # create, update, delete, query
    entity_type: str = ""
    entity_data: Dict = field(default_factory=dict)
    query: str = ""


@dataclass
class QBOBatchClient:
    """QuickBooks Online Batch API client."""
    access_token: str
    realm_id: str
    base_url: str = "https://quickbooks.api.intuit.com"

    def execute(self, requests: List[BatchRequest]) -> Dict[str, Any]:
        """Execute batch request."""
        if len(requests) > 30:
            raise ValueError("Maximum 30 requests per batch")

        batch_items = []
        for req in requests:
            item = {"bId": req.bId}

            if req.operation == "query":
                item["operation"] = "query"
                item["Query"] = req.query
            else:
                item["operation"] = req.operation
                item[req.entity_type] = req.entity_data

            batch_items.append(item)

        payload = {"BatchItemRequest": batch_items}

        response = requests.post(
            f"{self.base_url}/v3/company/{self.realm_id}/batch",
            headers={
                "Authorization": f"Bearer {self.access_token}",
                "Content-Type": "application/json"
            },
            json=payload
        )
        response.raise_for_status()
        return response.json()


def chunk_list(lst: List, chunk_size: int = 30) -> List[List]:
    """Split list into chunks of specified size."""
    return [lst[i:i + chunk_size] for i in range(0, len(lst), chunk_size)]
```

### Batch Journal Entry Creation

```python
def create_journal_entries_batch(
    client: QBOBatchClient,
    entries: List[Dict]
) -> List[Dict]:
    """Create multiple journal entries in batches."""
    results = []

    # Split into chunks of 30
    for chunk_idx, chunk in enumerate(chunk_list(entries, 30)):
        batch_requests = []

        for idx, entry in enumerate(chunk):
            batch_requests.append(BatchRequest(
                bId=f"je-{chunk_idx}-{idx}",
                operation="create",
                entity_type="JournalEntry",
                entity_data={
                    "TxnDate": entry["date"],
                    "PrivateNote": entry.get("memo", ""),
                    "Line": [
                        {
                            "DetailType": "JournalEntryLineDetail",
                            "Amount": entry["amount"],
                            "Description": entry.get("description", ""),
                            "JournalEntryLineDetail": {
                                "PostingType": "Debit",
                                "AccountRef": {"value": entry["debit_account"]}
                            }
                        },
                        {
                            "DetailType": "JournalEntryLineDetail",
                            "Amount": entry["amount"],
                            "Description": entry.get("description", ""),
                            "JournalEntryLineDetail": {
                                "PostingType": "Credit",
                                "AccountRef": {"value": entry["credit_account"]}
                            }
                        }
                    ]
                }
            ))

        response = client.execute(batch_requests)
        results.extend(response.get("BatchItemResponse", []))

    return results
```

### Batch with python-quickbooks SDK

```python
from quickbooks import QuickBooks
from quickbooks.batch import batch_create, batch_delete

# Batch create
customers = [Customer1, Customer2, Customer3, ...]
results = batch_create(customers, qb=client)

# Process results
for result in results:
    if result.bId:
        print(f"Created: {result.bId}")
    if hasattr(result, 'Fault'):
        print(f"Error: {result.Fault}")
```

---

## Best Practices

### 1. Use Meaningful Batch IDs

```python
# Good: Descriptive IDs that aid debugging
batch_requests = [
    {"bId": "je-2024-01-div-aapl", ...},
    {"bId": "je-2024-01-div-msft", ...},
    {"bId": "je-2024-01-int-bond", ...}
]

# Bad: Generic IDs
batch_requests = [
    {"bId": "1", ...},
    {"bId": "2", ...},
    {"bId": "3", ...}
]
```

### 2. Handle Rate Limits

```python
import time
from functools import wraps


def with_rate_limit(max_per_minute: int = 40):
    """Decorator to enforce rate limits."""
    min_interval = 60.0 / max_per_minute
    last_call = [0.0]

    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            elapsed = time.time() - last_call[0]
            if elapsed < min_interval:
                time.sleep(min_interval - elapsed)
            result = func(*args, **kwargs)
            last_call[0] = time.time()
            return result
        return wrapper
    return decorator


@with_rate_limit(max_per_minute=40)
def execute_batch(client, requests):
    return client.execute(requests)
```

### 3. Validate Before Batching

```python
def validate_journal_entry(entry: Dict) -> List[str]:
    """Validate journal entry before batch submission."""
    errors = []

    if not entry.get("date"):
        errors.append("Missing date")

    if not entry.get("debit_account"):
        errors.append("Missing debit account")

    if not entry.get("credit_account"):
        errors.append("Missing credit account")

    amount = entry.get("amount", 0)
    if amount <= 0:
        errors.append("Amount must be positive")

    return errors


def prepare_batch(entries: List[Dict]) -> tuple[List[Dict], List[Dict]]:
    """Separate valid and invalid entries."""
    valid = []
    invalid = []

    for entry in entries:
        errors = validate_journal_entry(entry)
        if errors:
            entry["_errors"] = errors
            invalid.append(entry)
        else:
            valid.append(entry)

    return valid, invalid
```

### 4. Process Results Properly

```python
def process_batch_results(
    results: List[Dict],
    original_requests: List[Dict]
) -> tuple[List[Dict], List[Dict]]:
    """Process batch results, separating successes and failures."""
    successes = []
    failures = []

    for result in results:
        bid = result.get("bId")

        if "Fault" in result:
            # Find original request for context
            original = next(
                (r for r in original_requests if r.get("bId") == bid),
                {}
            )
            failures.append({
                "bId": bid,
                "fault": result["Fault"],
                "original": original
            })
        else:
            successes.append(result)

    return successes, failures
```

---

## Error Handling

### Common Batch Errors

| Error Code | Description | Resolution |
|------------|-------------|------------|
| 2010 | Object not found | Verify entity ID exists |
| 5010 | Stale object | Refresh SyncToken and retry |
| 6000 | Business validation error | Check field values |
| 6210 | Duplicate name | Use unique name |

### Error Response Format

```json
{
  "BatchItemResponse": [
    {
      "bId": "create-1",
      "Fault": {
        "Error": [
          {
            "Message": "Object Not Found",
            "Detail": "Account with Id 999 not found",
            "code": "2010"
          }
        ],
        "type": "ValidationFault"
      }
    }
  ]
}
```

### Retry Logic

```python
from tenacity import retry, stop_after_attempt, wait_exponential


@retry(
    stop=stop_after_attempt(3),
    wait=wait_exponential(multiplier=1, min=2, max=30)
)
def execute_with_retry(client: QBOBatchClient, requests: List[BatchRequest]):
    """Execute batch with automatic retry on transient failures."""
    try:
        return client.execute(requests)
    except requests.exceptions.HTTPError as e:
        if e.response.status_code in (429, 500, 502, 503, 504):
            raise  # Retry these
        raise  # Don't retry others


def execute_batch_with_fallback(
    client: QBOBatchClient,
    requests: List[BatchRequest]
) -> List[Dict]:
    """Execute batch, falling back to individual requests on failure."""
    try:
        return execute_with_retry(client, requests)
    except Exception as e:
        print(f"Batch failed: {e}, falling back to individual requests")

        results = []
        for req in requests:
            try:
                result = execute_with_retry(client, [req])
                results.extend(result.get("BatchItemResponse", []))
            except Exception as individual_error:
                results.append({
                    "bId": req.bId,
                    "Fault": {"Error": [{"Message": str(individual_error)}]}
                })

        return results
```

---

## Complete Example: Bulk Import

```python
import csv
from pathlib import Path


def import_transactions_from_csv(
    client: QBOBatchClient,
    csv_path: Path
) -> Dict[str, Any]:
    """Import transactions from CSV file using batch operations."""

    # Read CSV
    with open(csv_path) as f:
        reader = csv.DictReader(f)
        entries = list(reader)

    # Validate
    valid_entries, invalid_entries = prepare_batch(entries)

    if invalid_entries:
        print(f"Skipping {len(invalid_entries)} invalid entries")

    # Create batch requests
    all_results = []
    for chunk_idx, chunk in enumerate(chunk_list(valid_entries, 30)):
        requests = [
            BatchRequest(
                bId=f"import-{chunk_idx}-{idx}",
                operation="create",
                entity_type="JournalEntry",
                entity_data=build_journal_entry(entry)
            )
            for idx, entry in enumerate(chunk)
        ]

        results = execute_batch_with_fallback(client, requests)
        all_results.extend(results)

        # Rate limit between chunks
        time.sleep(1.5)

    # Process results
    successes, failures = process_batch_results(all_results, valid_entries)

    return {
        "total": len(entries),
        "valid": len(valid_entries),
        "invalid": len(invalid_entries),
        "created": len(successes),
        "failed": len(failures),
        "failures": failures
    }


def build_journal_entry(entry: Dict) -> Dict:
    """Build JournalEntry from CSV row."""
    return {
        "TxnDate": entry["date"],
        "PrivateNote": entry.get("memo", ""),
        "DocNumber": entry.get("doc_number", ""),
        "Line": [
            {
                "DetailType": "JournalEntryLineDetail",
                "Amount": float(entry["amount"]),
                "Description": entry.get("description", ""),
                "JournalEntryLineDetail": {
                    "PostingType": "Debit",
                    "AccountRef": {"value": entry["debit_account_id"]}
                }
            },
            {
                "DetailType": "JournalEntryLineDetail",
                "Amount": float(entry["amount"]),
                "Description": entry.get("description", ""),
                "JournalEntryLineDetail": {
                    "PostingType": "Credit",
                    "AccountRef": {"value": entry["credit_account_id"]}
                }
            }
        ]
    }
```
