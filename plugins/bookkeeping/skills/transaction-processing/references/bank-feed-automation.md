---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Bank Feed Automation

Bank feed connection, auto-matching, rules engine, and ongoing maintenance for automated transaction ingestion.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `bookkeeping-bank-feeds`, `bookkeeping-bank-rec`, `bookkeeping-monthly-close`

---

## Connection Methods

- **Direct feed** — The accounting platform connects to the bank via a data aggregator (e.g., Plaid, Yodlee, MX) or a native bank partnership. Transactions typically sync nightly Mon-Fri. Setup: navigate to the banking or bank feeds area, search for the institution, authenticate with bank credentials, and select accounts. Aggregator coverage varies; some banks offer native partnerships with faster sync.
- **Manual CSV/QFX/OFX import** — Download a transaction file from the bank portal, upload it into the accounting platform's import function, and map columns (Date, Description, Amount). Works with any bank. Essential for historical data beyond the direct feed lookback window.
- Direct feeds provide only 90-120 days of lookback on initial connection. Manual import covers any date range the bank offers.

---

## Auto-Matching Engine

The accounting platform compares downloaded transactions against existing GL entries on three signals:

- **Amount** — exact match is highest confidence
- **Date proximity** — typically a 7-day window around the GL transaction date
- **Payee** — fuzzy match on vendor/customer name in bank description

Confidence tiers: high-confidence matches are pre-selected (verify before accepting); partial matches require manual confirmation; unmatched items land in the review queue. Transfer detection: when both sides of an inter-account transfer download, the platform may auto-create a single transfer entry.

---

## Bank Rules

**Creation**: Navigate to the banking rules area of the accounting platform, or create a rule directly from a transaction in the review queue (which pre-populates conditions).

**Conditions**: money direction (in/out/both), account scope (specific or all), description contains/is exactly, payee contains/is exactly, amount (equals/range/greater/less), condition logic (All = AND, Any = OR).

**Actions**: transaction type, payee assignment, category (COA account), class (if enabled), customer (for job costing), split (multi-line categorization).

**Priority**: rules execute top-to-bottom; first match wins. Drag to reorder. Most specific rules at top, broadest catch-alls at bottom.

**Auto-categorize vs manual review**: auto-categorize skips human review (use only for high-confidence recurring items like rent, payroll debits). "Categorize and manually review" pre-fills fields but keeps the transaction in the review queue. Default to manual review.

---

## Handling Duplicates

**Causes**: bank feed + manual entry of the same transaction; re-importing CSV/QFX with overlapping dates; reconnection pulling already-downloaded transactions; multiple integrations (e.g., Stripe + bank feed) recording the same deposit.

**Detection**: identical date/amount/description in the review queue or categorized transactions; reconciliation discrepancies (doubled amounts); audit log showing duplicate uploads.

**Resolution**: Review queue duplicates — exclude, then delete the excluded item. Already categorized — undo categorization, exclude, then delete. In the register — locate the duplicate in the account register and delete. Reconciled duplicates — post a correcting JE or undo reconciliation first.

---

## Excluding Personal Transactions

For mixed-use accounts: review every transaction before accepting (never auto-accept). Exclude personal items from the review queue. Create rules matching known personal payees with an exclude action. Use "Owner Draw" or "Shareholder Loan" category for personal charges with business accounting implications. Long-term fix: separate business and personal banking.

---

## Managing Disconnections

**Causes**: credential changes, institution mergers, aggregator token expiration (some banks require 90-day reauth), bank-side outages.

**Reconnection**: Navigate to the connected account and initiate reconnection with updated credentials. If that fails, disconnect fully then reconnect, linking to the same GL account.

**Gap handling**: reconnection pulls 90-120 days (bank-dependent). Set the review start date after the last downloaded transaction to avoid duplicates. For gaps beyond the lookback window, import CSV/QFX manually. Verify continuity by comparing last pre-disconnect and first post-reconnect transactions.

---

## Multi-Account Setup

Connect each account separately: checking (highest volume, most benefit from automation), savings, credit cards (connect as credit card type, not bank), PayPal (watch for duplicate risk with bank-side PayPal transfers), loan accounts (principal/interest split often needs a split rule). Each feed maps to exactly one COA entry. Never map two feeds to the same GL account.

---

## Reconciliation Impact

Bank feed transactions are **not reconciled by default**. Categorizing only records in the GL. Formal reconciliation through the platform's reconciliation function is still required monthly. Feed timing may differ from statement cutoff dates.

---

## Best Practices

- Review before accepting -- reserve auto-categorize for a small set of predictable recurring items
- Keep rules under 100-200; consolidate similar rules; delete rules for closed vendors
- Audit rules quarterly; verify mappings to active COA entries and current vendor names
- Prefer "Description is exactly" over "contains" to avoid miscategorization
- Name rules clearly (vendor or purpose); document intent for split and class rules
- Test new rules by checking the next transaction batch
- Never connect the same bank account via both direct feed and manual import simultaneously

---

## Decision Points

- **Direct feed vs manual import** -- direct feed for all supported accounts; manual only for unsupported banks or historical backfill
- **Auto-categorize threshold** -- enable only after a rule runs correctly for 3+ months with zero errors
- **Mixed-use accounts** -- if personal transactions exceed 20% of volume, recommend a dedicated business account
- **Disconnection gap >90 days** -- manual CSV import required; feed reconnection cannot reach back far enough
- **Duplicate post-reconciliation** -- correcting JE preferred over delete-and-re-reconcile
- **Rule conflict** -- reorder so the more specific rule is higher; never rely on creation order

---

## Platform Integration Notes

- Bank feed connections are typically per-company file; each company file has independent connections
- Programmatic transaction creation (API JEs or imports) can trigger duplicate detection against feed items — coordinate timing
- Some platforms offer AI-assisted auto-categorization of feed transactions; disable or supervise if manual rule control is preferred

---

## References

- Plaid. "How Plaid works." https://plaid.com/how-it-works/
- AICPA. "Accounting Technology Best Practices." https://www.aicpa.org/topic/technology
