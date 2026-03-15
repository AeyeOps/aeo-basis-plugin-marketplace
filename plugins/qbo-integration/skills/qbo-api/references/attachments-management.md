# QuickBooks Online Attachments: Management

Query, download, delete, and batch operations for attachments.

See also: [attachments-upload.md](attachments-upload.md) for uploading and linking files.

## Contents

- [Query Attachments](#query-attachments)
- [Download Attachment](#download-attachment)
- [Delete Attachment](#delete-attachment)
- [Batch Attachment Upload](#batch-attachment-upload)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)

---

## Query Attachments

### Get Attachments for Transaction

```python
import requests

def get_attachments_for_entity(
    client,
    entity_type: str,
    entity_id: str
) -> list[dict]:
    """
    Get all attachments linked to a transaction.

    Args:
        client: QuickBooks client
        entity_type: Transaction type
        entity_id: Transaction ID

    Returns:
        List of Attachable objects
    """
    base_url = client.api_url
    realm_id = client.company_id

    query = f"SELECT * FROM Attachable WHERE AttachableRef.EntityRef.Type = '{entity_type}' AND AttachableRef.EntityRef.value = '{entity_id}'"

    url = f"{base_url}/v3/company/{realm_id}/query"
    params = {"query": query, "minorversion": "65"}

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, headers=headers, params=params)
    response.raise_for_status()

    data = response.json()
    return data.get("QueryResponse", {}).get("Attachable", [])
```

### Get All Attachments

```python
def get_all_attachments(client, max_results: int = 1000) -> list[dict]:
    """Get all attachments in the company."""
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    query = f"SELECT * FROM Attachable MAXRESULTS {max_results}"

    url = f"{base_url}/v3/company/{realm_id}/query"
    params = {"query": query, "minorversion": "65"}

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    response = requests.get(url, headers=headers, params=params)
    response.raise_for_status()

    data = response.json()
    return data.get("QueryResponse", {}).get("Attachable", [])
```

---

## Download Attachment

```python
from pathlib import Path

def download_attachment(
    client,
    attachment_id: str,
    output_path: Path
) -> Path:
    """
    Download an attachment file.

    Args:
        client: QuickBooks client
        attachment_id: Attachment ID
        output_path: Where to save the file

    Returns:
        Path to downloaded file
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id

    url = f"{base_url}/v3/company/{realm_id}/attachable/{attachment_id}"
    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json"
    }

    meta_response = requests.get(url, headers=headers)
    meta_response.raise_for_status()

    attachable = meta_response.json()["Attachable"]

    download_url = attachable.get("TempDownloadUri")
    if not download_url:
        raise ValueError("No download URL available")

    file_response = requests.get(download_url, stream=True)
    file_response.raise_for_status()

    output_path = Path(output_path)
    with open(output_path, "wb") as f:
        for chunk in file_response.iter_content(chunk_size=8192):
            f.write(chunk)

    return output_path
```

---

## Delete Attachment

```python
def delete_attachment(client, attachment_id: str, sync_token: str) -> bool:
    """
    Delete an attachment.

    Args:
        client: QuickBooks client
        attachment_id: Attachment ID
        sync_token: Current SyncToken

    Returns:
        True if deleted successfully
    """
    import requests

    base_url = client.api_url
    realm_id = client.company_id
    url = f"{base_url}/v3/company/{realm_id}/attachable?operation=delete"

    headers = {
        "Authorization": f"Bearer {client.auth_client.access_token}",
        "Accept": "application/json",
        "Content-Type": "application/json"
    }

    payload = {
        "Id": attachment_id,
        "SyncToken": sync_token
    }

    response = requests.post(url, headers=headers, json=payload)
    response.raise_for_status()
    return True
```

---

## Batch Attachment Upload

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
from pathlib import Path

def upload_attachments_batch(
    client,
    files: list[tuple[Path, str, str]],
    max_workers: int = 5
) -> list[dict]:
    """
    Upload multiple attachments concurrently.

    Args:
        client: QuickBooks client
        files: List of (file_path, entity_type, entity_id) tuples
        max_workers: Concurrent upload threads

    Returns:
        List of upload results
    """
    from attachments_upload import upload_and_link

    results = []

    with ThreadPoolExecutor(max_workers=max_workers) as executor:
        futures = {
            executor.submit(
                upload_and_link,
                client,
                file_path,
                entity_type,
                entity_id
            ): (file_path, entity_type, entity_id)
            for file_path, entity_type, entity_id in files
        }

        for future in as_completed(futures):
            file_info = futures[future]
            try:
                result = future.result()
                results.append({
                    "file": str(file_info[0]),
                    "success": True,
                    "data": result
                })
            except Exception as e:
                results.append({
                    "file": str(file_info[0]),
                    "success": False,
                    "error": str(e)
                })

    return results
```

---

## Use Cases

### Attach Brokerage Statements to Journal Entries

```python
def attach_brokerage_statement(
    client,
    statement_path: Path,
    journal_entry_id: str,
    period: str
) -> dict:
    """Attach monthly brokerage statement to corresponding JE."""
    from attachments_upload import upload_and_link

    return upload_and_link(
        client,
        file_path=statement_path,
        entity_type="JournalEntry",
        entity_id=journal_entry_id,
        note=f"Brokerage statement for {period}"
    )
```

### Attach Tax Documents

```python
def attach_tax_document(
    client,
    document_path: Path,
    document_type: str,
    tax_year: int,
    entity_type: str = None,
    entity_id: str = None
) -> dict:
    """
    Attach tax-related document.

    Args:
        document_type: '1099-DIV', '1099-INT', '1099-B', 'K-1', etc.
    """
    from attachments_upload import upload_and_link, upload_attachment, create_note

    note = f"{document_type} for tax year {tax_year}"

    if entity_type and entity_id:
        return upload_and_link(
            client,
            file_path=document_path,
            entity_type=entity_type,
            entity_id=entity_id,
            note=note
        )
    else:
        result = upload_attachment(client, document_path)
        attachment_id = result["AttachableResponse"][0]["Attachable"]["Id"]
        create_note(client, "Company", client.company_id, note)
        return result
```

### Attach Vendor Invoice to Bill

```python
def attach_vendor_invoice(
    client,
    invoice_path: Path,
    bill_id: str,
    vendor_name: str,
    invoice_number: str
) -> dict:
    """Attach vendor's PDF invoice to QBO bill."""
    from attachments_upload import upload_and_link

    return upload_and_link(
        client,
        file_path=invoice_path,
        entity_type="Bill",
        entity_id=bill_id,
        note=f"Vendor invoice {invoice_number} from {vendor_name}"
    )
```

---

## Best Practices

1. **Batch uploads** - Use concurrent uploads for multiple files
2. **Store attachment IDs** - Keep mapping for future reference
3. **Download for backup** - Export attachments before account changes
4. **Clean up orphans** - Remove unlinked attachments periodically
5. **Retry on failure** - Handle transient errors gracefully
