---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QuickBooks Online Attachments: Upload and Link

Upload files and link documents to transactions.

See also: [attachments-management.md](attachments-management.md) for querying, downloading, and batch operations.

## Contents

- [Overview](#overview)
- [Supported File Types](#supported-file-types)
- [Upload Attachment](#upload-attachment)
- [Link Attachment to Transaction](#link-attachment-to-transaction)
- [Upload and Link in One Step](#upload-and-link-in-one-step)
- [Create Note (Without File)](#create-note-without-file)
- [Best Practices](#best-practices)

---

## Overview

The Attachments API allows you to:
- Upload files (PDF, images, documents) to QBO
- Link attachments to transactions (invoices, bills, journal entries)
- Create notes associated with transactions

---

## Supported File Types

| Category | Extensions | Max Size |
|----------|------------|----------|
| Images | jpg, jpeg, png, gif, bmp, tiff | 20 MB |
| Documents | pdf, doc, docx, xls, xlsx, txt, rtf | 20 MB |
| Other | csv, xml | 20 MB |

---

## Upload Attachment

### Endpoint

```
POST /v3/company/{realmId}/upload
Content-Type: multipart/form-data
```

### Python Example

```python
import requests
from pathlib import Path

def upload_attachment(
    client,
    file_path: Path,
    file_name: str = None,
    content_type: str = None
) -> dict:
    """
    Upload a file to QuickBooks Online.

    Args:
        client: QuickBooks client
        file_path: Path to the file to upload
        file_name: Optional display name (defaults to filename)
        content_type: MIME type (auto-detected if not provided)

    Returns:
        Attachable object with ID
    """
    import mimetypes

    file_path = Path(file_path)
    if not file_path.exists():
        raise FileNotFoundError(f"File not found: {file_path}")

    if content_type is None:
        content_type, _ = mimetypes.guess_type(str(file_path))
        content_type = content_type or "application/octet-stream"

    display_name = file_name or file_path.name

    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/upload"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    with open(file_path, "rb") as f:
        files = {
            "file_content_0": (display_name, f, content_type)
        }
        data = {
            "file_metadata_0": '{"AttachableRef":[],"FileName":"' + display_name + '"}'
        }

        response = requests.post(url, headers=headers, files=files, data=data)

    response.raise_for_status()
    return response.json()
```

### Response

```json
{
  "AttachableResponse": [
    {
      "Attachable": {
        "Id": "5000000000000123456",
        "SyncToken": "0",
        "FileName": "invoice_backup.pdf",
        "FileAccessUri": "/v3/company/1234567890/download/5000000000000123456",
        "TempDownloadUri": "https://...",
        "Size": 102400,
        "ContentType": "application/pdf",
        "Category": "Document"
      }
    }
  ],
  "time": "2024-01-15T10:30:00-08:00"
}
```

---

## Link Attachment to Transaction

### Endpoint

```
POST /v3/company/{realmId}/attachable
```

### Python Example

```python
def link_attachment_to_transaction(
    client,
    attachment_id: str,
    entity_type: str,
    entity_id: str
) -> dict:
    """
    Link an uploaded attachment to a transaction.

    Args:
        client: QuickBooks client
        attachment_id: ID of the uploaded attachment
        entity_type: Transaction type (Invoice, Bill, JournalEntry, etc.)
        entity_id: ID of the transaction

    Returns:
        Updated Attachable object
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/attachable/{attachment_id}"
    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json",
        "Content-Type": "application/json"
    }

    get_response = requests.get(url, headers=headers)
    get_response.raise_for_status()
    attachable = get_response.json()["Attachable"]

    if "AttachableRef" not in attachable:
        attachable["AttachableRef"] = []

    attachable["AttachableRef"].append({
        "EntityRef": {
            "type": entity_type,
            "value": entity_id
        }
    })

    update_url = f"{base_url}/v3/company/{realm_id}/attachable"
    update_response = requests.post(update_url, headers=headers, json=attachable)
    update_response.raise_for_status()

    return update_response.json()


# Supported entity types for linking
ENTITY_TYPES = [
    "Invoice",
    "Bill",
    "JournalEntry",
    "Payment",
    "BillPayment",
    "Deposit",
    "Transfer",
    "SalesReceipt",
    "CreditMemo",
    "RefundReceipt",
    "Estimate",
    "PurchaseOrder",
    "VendorCredit",
    "Customer",
    "Vendor",
    "Employee",
    "Item"
]
```

---

## Upload and Link in One Step

```python
import json

def upload_and_link(
    client,
    file_path: Path,
    entity_type: str,
    entity_id: str,
    note: str = None
) -> dict:
    """
    Upload a file and link it to a transaction in one operation.

    Args:
        client: QuickBooks client
        file_path: Path to file
        entity_type: Transaction type
        entity_id: Transaction ID
        note: Optional note to attach

    Returns:
        Created Attachable object
    """
    import mimetypes

    file_path = Path(file_path)
    content_type, _ = mimetypes.guess_type(str(file_path))
    content_type = content_type or "application/octet-stream"

    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/upload"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    metadata = {
        "AttachableRef": [
            {
                "EntityRef": {
                    "type": entity_type,
                    "value": entity_id
                }
            }
        ],
        "FileName": file_path.name
    }

    if note:
        metadata["Note"] = note

    with open(file_path, "rb") as f:
        files = {
            "file_content_0": (file_path.name, f, content_type)
        }
        data = {
            "file_metadata_0": json.dumps(metadata)
        }

        response = requests.post(url, headers=headers, files=files, data=data)

    response.raise_for_status()
    return response.json()
```

---

## Create Note (Without File)

```python
def create_note(
    client,
    entity_type: str,
    entity_id: str,
    note_text: str
) -> dict:
    """
    Create a text note attached to a transaction.

    Args:
        client: QuickBooks client
        entity_type: Transaction type
        entity_id: Transaction ID
        note_text: Note content

    Returns:
        Created Attachable object
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/attachable"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json",
        "Content-Type": "application/json"
    }

    attachable = {
        "Note": note_text,
        "AttachableRef": [
            {
                "EntityRef": {
                    "type": entity_type,
                    "value": entity_id
                }
            }
        ]
    }

    response = requests.post(url, headers=headers, json=attachable)
    response.raise_for_status()
    return response.json()
```

---

## Best Practices

1. **Always include notes** - Describe what the document represents
2. **Use consistent naming** - Include date and document type in filenames
3. **Compress large files** - Stay under 20MB limit
4. **Validate file types** - Check extensions before uploading
