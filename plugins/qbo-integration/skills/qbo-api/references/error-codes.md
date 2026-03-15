# QuickBooks Online API Error Codes Reference

## Table of Contents

1. [HTTP Status Codes](#http-status-codes)
2. [Authentication Errors](#authentication-errors)
3. [Validation Errors](#validation-errors)
4. [Business Logic Errors](#business-logic-errors)
5. [Rate Limiting](#rate-limiting)
6. [Common Error Patterns](#common-error-patterns)
7. [Troubleshooting Guide](#troubleshooting-guide)

---

## HTTP Status Codes

| Code | Meaning | Action |
|------|---------|--------|
| 200 | Success | Process response |
| 400 | Bad Request | Check request format/data |
| 401 | Unauthorized | Refresh access token |
| 403 | Forbidden | Check permissions/scopes |
| 404 | Not Found | Verify entity ID exists |
| 429 | Too Many Requests | Implement backoff |
| 500 | Internal Server Error | Retry with backoff |
| 503 | Service Unavailable | Retry later |

---

## Authentication Errors

### OAuth Token Errors

| Error | Code | Resolution |
|-------|------|------------|
| `invalid_grant` | - | Refresh token expired; re-authorize |
| `invalid_client` | - | Check client_id and client_secret |
| `invalid_token` | - | Token malformed; re-authorize |
| `access_denied` | - | User denied authorization |
| `expired_token` | 401 | Call refresh token endpoint |

### Token Refresh

```python
from intuitlib.exceptions import AuthClientError

def refresh_token(auth_client, refresh_token):
    """Refresh expired access token."""
    try:
        auth_client.refresh(refresh_token=refresh_token)
        return {
            "access_token": auth_client.access_token,
            "refresh_token": auth_client.refresh_token,
            "expires_in": auth_client.x_refresh_token_expires_in
        }
    except AuthClientError as e:
        if "invalid_grant" in str(e):
            # Refresh token expired - need full re-auth
            raise ReauthorizationRequired("Refresh token expired")
        raise
```

---

## Validation Errors

### Error 6000 - Business Validation

| Detail | Cause | Resolution |
|--------|-------|------------|
| `Business Validation Error` | Invalid field value | Check field constraints |
| `Duplicate Name Exists` | Non-unique name | Use unique name |
| `Required field is missing` | Missing required field | Add required field |

### Error 2010 - Object Not Found

```json
{
  "Fault": {
    "Error": [{
      "Message": "Object Not Found",
      "Detail": "Account with Id 999 not found",
      "code": "2010"
    }],
    "type": "ValidationFault"
  }
}
```

**Resolution:** Verify the entity ID exists before referencing.

```python
def safe_get_account(client, account_id):
    """Safely get account, returning None if not found."""
    try:
        return Account.get(account_id, qb=client)
    except QuickbooksException as e:
        if "2010" in str(e) or "Object Not Found" in str(e):
            return None
        raise
```

### Error 5010 - Stale Object

```json
{
  "Fault": {
    "Error": [{
      "Message": "Stale Object Error",
      "Detail": "Object has been modified since last read",
      "code": "5010"
    }],
    "type": "ValidationFault"
  }
}
```

**Resolution:** Fetch fresh object and retry.

```python
def update_with_retry(client, entity, max_retries=3):
    """Update entity with stale object retry."""
    for attempt in range(max_retries):
        try:
            entity.save(qb=client)
            return entity
        except QuickbooksException as e:
            if "5010" in str(e) or "Stale Object" in str(e):
                # Refresh entity
                entity = type(entity).get(entity.Id, qb=client)
                # Apply changes again...
            else:
                raise
    raise Exception("Max retries exceeded for stale object")
```

### Error 6210 - Duplicate Name

```json
{
  "Fault": {
    "Error": [{
      "Message": "Duplicate Name Exists Error",
      "Detail": "The name supplied already exists",
      "code": "6210"
    }],
    "type": "ValidationFault"
  }
}
```

**Resolution:** Use unique name or find existing entity.

```python
def create_or_get_account(client, name, account_type, subtype):
    """Create account or return existing with same name."""
    # Check if exists
    existing = Account.query(
        f"SELECT * FROM Account WHERE Name = '{name}'",
        qb=client
    )
    if existing:
        return existing[0]

    # Create new
    account = Account()
    account.Name = name
    account.AccountType = account_type
    account.AccountSubType = subtype
    account.save(qb=client)
    return account
```

---

## Business Logic Errors

### Error 6140 - Invalid Reference

Entity reference is invalid or inactive.

```json
{
  "Fault": {
    "Error": [{
      "Message": "Invalid Reference Id",
      "Detail": "Customer Id 123 is invalid",
      "code": "6140"
    }],
    "type": "ValidationFault"
  }
}
```

**Resolution:** Verify referenced entity exists and is active.

### Error 6130 - Required Line Missing

Transaction requires at least one line item.

```python
# Wrong - no lines
invoice = Invoice()
invoice.CustomerRef = {"value": "1"}
invoice.save(qb=client)  # Error!

# Correct - include line
invoice = Invoice()
invoice.CustomerRef = {"value": "1"}
invoice.Line = [{
    "DetailType": "SalesItemLineDetail",
    "Amount": 100,
    "SalesItemLineDetail": {
        "ItemRef": {"value": "1"}
    }
}]
invoice.save(qb=client)
```

### Journal Entry Imbalance

Debits must equal credits.

```python
def validate_journal_entry(lines):
    """Validate journal entry balances."""
    debits = sum(
        line["Amount"] for line in lines
        if line["JournalEntryLineDetail"]["PostingType"] == "Debit"
    )
    credits = sum(
        line["Amount"] for line in lines
        if line["JournalEntryLineDetail"]["PostingType"] == "Credit"
    )

    if abs(debits - credits) > 0.01:  # Allow for rounding
        raise ValueError(f"Unbalanced: Debits={debits}, Credits={credits}")
```

---

## Rate Limiting

### Limits

| Limit Type | Value | Window |
|------------|-------|--------|
| Standard | 500 requests | Per minute |
| Batch | 40 batch requests | Per minute |
| Reports | 10 requests | Per minute |
| Query | 500 requests | Per minute |

### Error 429 Response

```json
{
  "Fault": {
    "Error": [{
      "Message": "message=Rate Limit Exceeded",
      "code": "429"
    }]
  }
}
```

### Rate Limit Handler

```python
import time
from functools import wraps


class RateLimiter:
    """Simple rate limiter for QBO API."""

    def __init__(self, max_requests=500, window_seconds=60):
        self.max_requests = max_requests
        self.window_seconds = window_seconds
        self.requests = []

    def wait_if_needed(self):
        """Wait if rate limit would be exceeded."""
        now = time.time()
        # Remove old requests outside window
        self.requests = [
            r for r in self.requests
            if now - r < self.window_seconds
        ]

        if len(self.requests) >= self.max_requests:
            # Wait for oldest request to expire
            sleep_time = self.requests[0] + self.window_seconds - now
            if sleep_time > 0:
                time.sleep(sleep_time)

        self.requests.append(time.time())


def rate_limited(limiter: RateLimiter):
    """Decorator for rate-limited API calls."""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            limiter.wait_if_needed()
            return func(*args, **kwargs)
        return wrapper
    return decorator


# Usage
limiter = RateLimiter(max_requests=500, window_seconds=60)

@rate_limited(limiter)
def api_call(client, ...):
    ...
```

### Exponential Backoff

```python
import random
from tenacity import (
    retry,
    stop_after_attempt,
    wait_exponential,
    retry_if_exception_type
)


class RateLimitError(Exception):
    pass


def is_rate_limit_error(exception):
    return (
        isinstance(exception, RateLimitError) or
        "429" in str(exception) or
        "Rate Limit" in str(exception)
    )


@retry(
    stop=stop_after_attempt(5),
    wait=wait_exponential(multiplier=1, min=2, max=60),
    retry=retry_if_exception_type((RateLimitError, ConnectionError))
)
def api_call_with_retry(client, operation):
    """API call with exponential backoff retry."""
    try:
        return operation()
    except QuickbooksException as e:
        if "429" in str(e):
            raise RateLimitError(str(e))
        raise
```

---

## Common Error Patterns

### Pattern: Entity Not Found

```python
from quickbooks.exceptions import QuickbooksException

def get_entity_safe(entity_class, entity_id, client):
    """Safely get entity, handling not found."""
    try:
        return entity_class.get(entity_id, qb=client)
    except QuickbooksException as e:
        error_str = str(e)
        if any(x in error_str for x in ["2010", "Not Found", "does not exist"]):
            return None
        raise
```

### Pattern: Handle Token Expiry

```python
class QBOClient:
    """QBO client with automatic token refresh."""

    def __init__(self, auth_client, realm_id):
        self.auth_client = auth_client
        self.realm_id = realm_id
        self._client = None

    @property
    def client(self):
        if self._client is None:
            self._rebuild_client()
        return self._client

    def _rebuild_client(self):
        self._client = QuickBooks(
            auth_client=self.auth_client,
            refresh_token=self.auth_client.refresh_token,
            company_id=self.realm_id
        )

    def execute(self, operation):
        """Execute operation with token refresh on expiry."""
        try:
            return operation(self.client)
        except (AuthorizationException, QuickbooksException) as e:
            if "401" in str(e) or "expired" in str(e).lower():
                self.auth_client.refresh(
                    refresh_token=self.auth_client.refresh_token
                )
                self._rebuild_client()
                return operation(self.client)
            raise
```

### Pattern: Batch Error Handling

```python
def process_batch_response(response):
    """Process batch response, separating successes and failures."""
    successes = []
    failures = []

    for item in response.get("BatchItemResponse", []):
        bid = item.get("bId", "unknown")

        if "Fault" in item:
            fault = item["Fault"]
            errors = fault.get("Error", [])
            failures.append({
                "bId": bid,
                "errors": [
                    {
                        "code": e.get("code"),
                        "message": e.get("Message"),
                        "detail": e.get("Detail")
                    }
                    for e in errors
                ]
            })
        else:
            successes.append(item)

    return successes, failures
```

---

## Troubleshooting Guide

### "Invalid Account Type" Error

**Symptoms:** Creating account fails with validation error.

**Cause:** AccountType and AccountSubType mismatch.

**Solution:** Use valid combinations from the Account Types Reference.

```python
# Valid combinations
ACCOUNT_TYPE_SUBTYPES = {
    "Bank": ["Checking", "Savings", "MoneyMarket", "CashOnHand"],
    "Other Current Asset": ["Investments", "PrepaidExpenses", "UndepositedFunds"],
    "Income": ["ServiceFeeIncome", "SalesOfProductIncome"],
    # ... see api-entities.md for full list
}
```

### "Cannot Find Account" in Journal Entry

**Symptoms:** Journal entry creation fails with account reference error.

**Cause:** Using account name instead of ID, or account is inactive.

**Solution:**
```python
# Get account ID first
account = Account.query(
    "SELECT Id, Name FROM Account WHERE Name = 'Checking'",
    qb=client
)[0]

# Use ID in reference
line.JournalEntryLineDetail = {
    "PostingType": "Debit",
    "AccountRef": {"value": account.Id, "name": account.Name}
}
```

### Slow Query Performance

**Symptoms:** Queries taking too long or timing out.

**Cause:** Querying too many records without pagination.

**Solution:**
```python
# Use pagination
def query_all(entity_class, base_query, client, page_size=1000):
    """Query all records with pagination."""
    all_records = []
    start = 1

    while True:
        query = f"{base_query} STARTPOSITION {start} MAXRESULTS {page_size}"
        batch = entity_class.query(query, qb=client)

        if not batch:
            break

        all_records.extend(batch)
        start += page_size

    return all_records
```

### Webhook Signature Validation Failing

**Symptoms:** Webhook payloads rejected as invalid.

**Cause:** Using wrong verifier token or incorrect signature algorithm.

**Solution:**
```python
import hmac
import hashlib
import base64


def validate_webhook_signature(payload: bytes, signature: str, verifier_token: str) -> bool:
    """Validate Intuit webhook signature."""
    expected = hmac.new(
        verifier_token.encode('utf-8'),
        payload,
        hashlib.sha256
    ).digest()

    expected_b64 = base64.b64encode(expected).decode('utf-8')
    return hmac.compare_digest(expected_b64, signature)
```
