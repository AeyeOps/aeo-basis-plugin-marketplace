---
name: transaction-processing
description: >
  Contains verified bank rule priority logic, auto-matching engine mechanics, and IRC 274
  five-element substantiation checklists that produce more precise answers than reasoning
  from general training alone. Bank feed setup, auto-categorize thresholds, duplicate
  resolution by transaction state, mixed-use accounts, receipt OCR, feed disconnection
  gaps, document retention (3/6/7-year), Cohan rule. Consult when connecting feeds,
  creating bank rules, capturing receipts, or asking "why is this duplicated."
---

# Transaction Processing

Daily transaction ingestion, categorization, and document capture for C-corporation
bookkeeping. This skill covers the full lifecycle from bank feed connection through
categorized, substantiated GL entries ready for reconciliation.

## Bank Feed Connection

Two connection methods, each with distinct use cases:

- **Direct feed** — The accounting platform connects to the bank via a data aggregator
  (e.g., Plaid, Yodlee, MX) or a native bank partnership. Transactions typically sync
  nightly Mon-Fri. Setup: navigate to the banking or bank feeds area, search for the
  institution, authenticate with bank credentials, and select accounts. Provides 90-120
  days of lookback on initial connection.
- **Manual CSV/QFX/OFX import** — Download a transaction file from the bank portal, upload
  it into the accounting platform's import function, and map columns (Date, Description,
  Amount). Required for unsupported banks or historical data beyond the direct feed
  lookback window.

**Multi-account setup**: Connect each account separately. Map each feed to exactly one
COA entry — never map two feeds to the same GL account. Credit cards connect as credit
card type, not bank type. PayPal carries duplicate risk with bank-side PayPal transfers.
Loan accounts typically need a split rule for principal/interest allocation.

## Auto-Matching Engine

The accounting platform matches downloaded transactions against existing GL entries using
three signals:

- **Amount** — Exact match is highest confidence
- **Date proximity** — Typically a 7-day window around the GL transaction date
- **Payee** — Fuzzy match on vendor/customer name in bank description

Confidence tiers:
- **High-confidence** — Pre-selected match; verify before accepting
- **Partial match** — Requires manual confirmation
- **Unmatched** — Lands in the review queue for manual categorization

Transfer detection: when both sides of an inter-account transfer download, the platform
may auto-create a single transfer entry. Verify the linked accounts before accepting.

## Bank Rules

### Creation and Structure

Create rules from the banking rules area of the accounting platform, or directly from a
transaction in the review queue (which pre-populates conditions from that transaction).

**Conditions**: money direction (in/out/both), account scope (specific or all),
description contains/is exactly, payee contains/is exactly, amount (equals/range/greater/less).
Condition logic: All = AND, Any = OR.

**Actions**: transaction type, payee assignment, category (COA account), class (if
enabled), customer (for job costing), split (multi-line categorization).

### Priority and Execution

Rules execute top-to-bottom; first match wins. Drag to reorder. Place most specific
rules at top, broadest catch-alls at bottom.

### Auto-Categorize Decision

- **Auto-categorize** — Skips human review. Use only for high-confidence recurring items
  (rent, payroll debits) after the rule has run correctly for 3+ months with zero errors.
- **Categorize and manually review** — Pre-fills fields but keeps the transaction in the
  review queue. This is the default for all new rules.

### Rule Maintenance

- Keep total rules under 100-200; consolidate similar rules; delete rules for closed vendors
- Audit rules quarterly — verify mappings to active COA entries and current vendor names
- Prefer "Description is exactly" over "contains" to reduce miscategorization
- Name rules clearly by vendor or purpose; document intent for split and class rules
- Test new rules by checking the next transaction batch before trusting them

## Duplicate Detection and Resolution

### Common Causes

- Bank feed + manual entry of the same transaction
- Re-importing CSV/QFX with overlapping date ranges
- Reconnection pulling already-downloaded transactions
- Multiple integrations (e.g., Stripe deposit + bank feed) recording the same item
- Receipt uploaded AND bank feed imported creating two transactions for one expense

### Resolution by State

- **Review queue duplicates** — Exclude from the review queue, then delete the excluded item
- **Already categorized** — Undo the categorization, exclude, then delete
- **In the register** — Locate the duplicate in the account register and delete it
- **Post-reconciliation** — Post a correcting JE (preferred) or undo reconciliation first;
  never delete reconciled transactions without understanding downstream impact

## Mixed-Use and Personal Transaction Handling

For accounts with personal and business transactions:

- Review every transaction before accepting — never auto-accept on mixed accounts
- Exclude personal items from the review queue
- Create rules matching known personal payees with Exclude action
- Use "Owner Draw" (sole prop) or "Shareholder Loan" (C-corp) category for personal
  charges with business accounting implications
- If personal transactions exceed 20% of volume, recommend a dedicated business account

## Feed Disconnection Management

**Causes**: credential changes, institution mergers, aggregator token expiration (some
banks require 90-day reauth), bank-side outages.

**Reconnection procedure**:
1. Navigate to the connected account and initiate reconnection with updated credentials
2. If that fails, disconnect fully then reconnect, linking to the same GL account
3. Set the review start date after the last downloaded transaction to prevent duplicates

**Gap handling**: Reconnection pulls 90-120 days (bank-dependent). For gaps beyond the
lookback window, import CSV/QFX manually. Verify continuity by comparing the last
pre-disconnect and first post-reconnect transactions.

## Receipt and Document Capture

### IRS Substantiation (IRC Section 274)

Meals and entertainment expenses require all five elements or the deduction is disallowed:

1. **Amount** — Exact cost
2. **Date** — When incurred
3. **Place** — Name and location of venue
4. **Business purpose** — Specific discussion or activity (not just "business meal")
5. **Business relationship** — Names, titles, and relationship of attendees

A credit card statement alone is insufficient — it proves amount and date but not
purpose, attendees, or relationship. A receipt plus a brief notation satisfies the
requirement.

### Electronic Records (Rev. Proc. 98-25)

The IRS accepts electronic records as originals when they are legible (original
resolution), indexed (retrievable by date/vendor/amount/category), accessible (producible
on demand), and unalterable (audit trail of changes). Paper originals may be destroyed
after compliant digitization.

### Receipt Capture Methods

Most accounting platforms support multiple capture channels:

- **Mobile app** — Photograph receipt directly; triggers OCR immediately
- **Email forwarding** — Forward to a designated receipt inbox provided by the platform
- **Web upload** — Drag or upload image/PDF files through the browser interface
- **Transaction attachment** — Open a matched transaction and attach the receipt manually

OCR extracts vendor name, date, amount, and payment method. Auto-matched receipts
(amount match within date window) require review and confirmation. Unmatched receipts
create draft expenses for manual categorization.

**Limitations**: OCR accuracy degrades on handwritten/faded/crumpled receipts. Non-USD
requires manual conversion. File size limits and supported formats vary by platform but
commonly include JPEG, PNG, GIF, and PDF.

### Third-Party Capture Tools

Use third-party tools when managing 3+ clients or receipt volume exceeds 50/month:

- **Dext** — Bulk capture, multi-currency, line-item extraction, syncs as draft transactions
  to most major accounting platforms
- **Hubdoc** — Auto-fetches recurring documents from suppliers, publishes as bill/expense
  entries to the connected accounting platform
- **Keeper Tax** — SMS-based capture for solo clients; limited accounting platform integration

### Capture-to-Storage Workflow

1. **Capture** — Photograph, scan, or forward at point of receipt (same-day standard;
   beyond 7 days significantly increases loss rates)
2. **Categorize** — Assign expense category, vendor, and GL account
3. **Match** — Link document to corresponding bank feed or GL entry
4. **Store** — Attach to transaction in the accounting platform or document management system
5. **Retrieve** — Locate during month-end review, audit, or tax preparation

### Document Retention

- **3 years** — Standard statute of limitations (IRC 6501(a))
- **6 years** — Gross income understated by >25% (IRC 6501(e))
- **7 years** — Practical recommendation (covers 6-year window plus buffer)
- **Indefinite** — Fraud or failure to file
- **Employment records** — 4 years after tax due or paid
- State periods may extend beyond federal — retain for the longer of the two

## Decision Logic

### Transaction Processing Decisions

- **Direct feed vs. manual import** — Direct feed for all supported accounts; manual only
  for unsupported banks or historical backfill beyond 90 days
- **Auto-categorize threshold** — Enable only after a rule runs correctly for 3+ months
  with zero errors; default all new rules to manual review
- **Rule conflict** — Reorder so the more specific rule is higher; never rely on creation order
- **Duplicate post-reconciliation** — Correcting JE preferred over delete-and-re-reconcile
- **Disconnection gap >90 days** — Manual CSV import required; feed reconnection cannot
  reach back far enough

### Document Capture Decisions

- **Receipt captured, no bank match** — Hold in unmatched queue for 7 days; if no match,
  create expense manually and investigate whether charge hit a different account
- **Missing receipt over $75** — Attempt duplicate from vendor; if unavailable, document
  the attempt and business purpose in a contemporaneous log (Cohan rule allows partial
  substantiation for non-274 expenses)
- **Multiple receipts match same amount** — Verify by date and vendor; if ambiguous,
  confirm with client before matching
- **Built-in vs. third-party capture** — Third-party when managing 3+ clients or
  volume exceeds 50 receipts/month; the platform's native capture is sufficient below
  those thresholds

## Platform-Specific Notes

The procedures in this skill are platform-neutral. For platform-specific implementation
details (API behavior, UI navigation, feature toggles), consult the appropriate
integration plugin — e.g., `qbo-integration` for QuickBooks Online.

## Client Training Essentials

- Set up email forwarding rules to auto-forward vendor receipts to the platform's receipt inbox
- Walk through mobile app receipt capture during onboarding
- Establish weekly reminder cadence for unfiled receipts
- For digital-resistant clients, provide a labeled envelope; collect and scan monthly
- Standardize an expense report template for employees: date, vendor, amount, purpose,
  attendees (if meal)

## Common Pitfalls

- **Year-end receipt dump** — Prevention: monthly or weekly capture cadence
- **Missing meals substantiation** — Credit card charge exists but no attendee/purpose
  record; deduction disallowed on exam. Prevention: require brief note at capture time
- **Thermal paper fading** — Gas station/restaurant receipts go blank within 1-2 years.
  Prevention: photograph immediately; never rely on paper originals
- **Personal expenses mixed in** — Prevention: separate cards; flag and exclude during
  categorization
- **Simultaneous direct feed + manual import** — Never connect the same bank account
  via both methods; guaranteed duplicates

## Supporting References

Read these for deeper detail on specific topics:

- `references/bank-feed-automation.md` — Complete bank feed operations reference. Covers
  connection methods, auto-matching engine mechanics, bank rule creation and priority,
  duplicate handling procedures, disconnection troubleshooting, multi-account setup, and
  reconciliation impact. Read when troubleshooting a specific feed issue, designing a rule
  strategy, or handling a complex reconnection scenario.

- `references/receipt-document-capture.md` — Full document capture and retention reference.
  Covers IRC 274 substantiation requirements, Rev. Proc. 98-25 electronic records
  standards, receipt capture methods and OCR details, third-party tool comparisons
  (Dext, Hubdoc, Keeper Tax), federal and state retention periods, client training
  workflows, and common pitfalls. Read when advising on substantiation requirements,
  evaluating capture tools, or setting up a client's document workflow.

## Cross-Plugin References

For account classification and categorization logic:
- Invoke `accounting-foundation:categorization-rules` for transaction-to-account routing
  rules, category selection logic, and classification edge cases
- Invoke `accounting-foundation:chart-of-accounts` for COA structure, numbering ranges,
  account type taxonomy, and tax-line mapping

For QBO platform operations:
- Invoke `qbo-integration:qbo-api` for programmatic transaction creation, API-based
  imports, and timing coordination with bank feeds
- Invoke `qbo-integration:qbo-coa` for QBO-specific account types, detail types, and
  platform-level COA configuration

## Cross-Plugin Consumers

- `bookkeeping:reconciliation` — Transactions must be categorized before reconciliation
- `bookkeeping:monthly-close` — Transaction completeness is a close prerequisite
- `bookkeeping:catchup-bookkeeping` — Bulk historical transaction processing uses these
  same workflows at scale
- `bookkeeping:audit-support` — Document capture and retention directly supports audit
  readiness
- `tax-prep:form-1120-prep` — Substantiated, correctly categorized transactions feed
  return preparation
