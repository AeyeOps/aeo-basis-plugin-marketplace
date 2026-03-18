---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Receipt and Document Capture

Workflows for capturing, categorizing, matching, and retaining receipts and source documents to substantiate business transactions.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `bookkeeping-document-capture`, `bookkeeping-expense-management`, `bookkeeping-audit-prep`

## Contents

- [IRS Substantiation Requirements](#irs-substantiation-requirements)
- [Document Types](#document-types)
- [Capture-to-Storage Workflow](#capture-to-storage-workflow)
- [Receipt Capture Methods](#receipt-capture-methods)
- [Third-Party Capture Tools](#third-party-capture-tools)
- [Retention Requirements](#retention-requirements)
- [Client Training](#client-training)
- [Common Pitfalls](#common-pitfalls)
- [Decision Points](#decision-points)
- [References](#references)

---

## IRS Substantiation Requirements

### IRC Section 274 — Meals and Entertainment

Expenses under IRC 274 require heightened substantiation. Each expense must document all five elements or it is disallowed:

- **Amount** — exact cost of the meal or entertainment
- **Date** — when the expense was incurred
- **Place** — name and location of the restaurant or venue
- **Business purpose** — specific business discussion or activity (not just "business meal")
- **Business relationship** — names and titles of attendees; relationship to taxpayer

A credit card statement alone is insufficient — it proves amount and date but not purpose, attendees, or business relationship. A receipt plus a brief notation satisfies the requirement.

### General Recordkeeping — Rev. Proc. 98-25

The IRS accepts electronic records as original documents provided they are:

- **Legible** — readable at original resolution; no degraded scans
- **Indexed** — retrievable by date, vendor, amount, or category
- **Accessible** — producible on demand during examination (not locked in a decommissioned system)
- **Unalterable** — system must prevent after-the-fact modification or maintain an audit trail of changes

Paper originals may be destroyed after digitization if these conditions are met.

---

## Document Types

Beyond receipts, a complete document capture workflow covers:

- **Vendor invoices** — AP substantiation, especially for accrual-basis entries
- **Contracts and agreements** — lease terms, loan covenants, service agreements
- **Bank and credit card statements** — monthly statements as secondary evidence
- **Loan documents** — amortization schedules, promissory notes
- **Tax forms received** — 1099s, W-2s, K-1s, property tax bills
- **Insurance policies** — coverage summaries, premium notices
- **Vehicle mileage logs** — if claiming standard mileage rate deduction

---

## Capture-to-Storage Workflow

1. **Capture** — photograph, scan, or forward the document at point of receipt
2. **Categorize** — assign expense category, vendor, and GL account
3. **Match to transaction** — link the document to the corresponding bank feed or GL entry
4. **Store** — attach to the transaction in the accounting system or document management platform
5. **Retrieve** — locate and produce during month-end review, audit, or tax preparation

The goal is zero lag between incurring an expense and capturing its documentation. Same-day capture is the standard; anything beyond 7 days significantly increases loss rates.

---

## Receipt Capture Methods

### Upload Methods

Most accounting platforms support multiple capture channels:

- **Mobile app** — Photograph receipt directly in the platform's mobile app; triggers OCR immediately
- **Email forwarding** — Forward receipt emails to a designated receipt inbox provided by the platform
- **Web upload** — Drag or upload image/PDF files through the browser interface
- **Transaction attachment** — Open a matched transaction and attach the receipt file manually

### OCR and Matching

The platform's built-in OCR extracts vendor name, date, amount, and payment method from the receipt image. It then suggests a match against existing bank feed transactions using amount and date proximity.

- **Auto-matched**: OCR amount matches a bank feed transaction within a date window — review and confirm
- **Unmatched**: no bank feed match found — the platform creates a draft expense that the bookkeeper categorizes manually
- **Attached to existing**: receipt is linked as supporting documentation on an already-categorized transaction

### Limitations

- OCR accuracy degrades on handwritten, faded, or crumpled receipts
- Receipts in non-USD currencies require manual entry of the converted amount
- File size limits and supported formats vary by platform but commonly include JPEG, PNG, GIF, and PDF

---

## Third-Party Capture Tools

### Dext (formerly Receipt Bank)

- Bulk capture via mobile, email forwarding, or fetch from supplier portals
- Multi-currency support with automatic conversion
- Syncs categorized expenses as draft transactions to most major accounting platforms
- Line-item extraction for itemized receipts
- Best for: firms managing multiple clients with high receipt volume

### Hubdoc

- Auto-fetches recurring documents from banks, utilities, and suppliers
- Email forwarding and mobile upload
- Publishes as bill or expense with attached source document to the connected platform
- Integrates with multiple accounting platforms
- Best for: practices managing clients across multiple accounting platforms

### Keeper Tax

- Targets freelancers and sole proprietors
- Monitors bank transactions and flags potential deductions
- Receipt capture via text message (SMS photo)
- Limited accounting platform integration — primarily an expense-finding tool, not a full capture platform
- Best for: solo clients who need nudging to capture anything at all

---

## Retention Requirements

### Federal (IRS)

- **3 years** — standard statute of limitations from the filing date (IRC 6501(a))
- **6 years** — if gross income is understated by more than 25% (IRC 6501(e))
- **Indefinite** — fraud or failure to file (no statute of limitations)
- **Employment tax records** — 4 years after the tax becomes due or is paid, whichever is later

### State Considerations

- State statutes of limitations may extend beyond federal (e.g., California: 4 years; Montana: 5 years)
- Sales tax audit periods vary: Florida 3 years, New York 3 years, California 4 years
- When federal and state periods differ, retain for the longer of the two

### Practical Recommendation

Retain all source documents for **7 years** from filing date. This covers the 6-year substantial understatement window plus a buffer for late-filed returns. Storage costs for digital documents are negligible compared to the risk of missing records during examination.

---

## Client Training

Consistent capture depends on making it effortless for the client:

- **Set up email forwarding** — create a rule in the client's email to auto-forward receipts from common vendors to the platform's receipt inbox
- **Mobile app onboarding** — walk the client through photographing a receipt and verifying OCR results during the first meeting
- **Weekly reminder cadence** — automated reminder (email or text) asking the client to upload any unfiled receipts
- **Physical receipt box** — for clients who resist digital workflows, provide a labeled envelope or box; collect and scan monthly
- **Expense report template** — for clients with employees, standardize a simple form: date, vendor, amount, purpose, attendees (if meal)

---

## Common Pitfalls

- **Year-end receipt dump** — clients deliver a shoebox in January; half the receipts are faded and many are missing. Prevention: monthly or weekly capture cadence.
- **Missing meals substantiation** — credit card charge exists but no record of attendees or business purpose. The deduction is disallowed on examination. Prevention: require a brief note at time of capture.
- **Personal expenses mixed in** — client's personal and business purchases on the same card. Prevention: separate cards; flag and exclude personal charges during categorization.
- **Duplicate entry** — receipt uploaded AND bank feed imported, creating two transactions for one expense. Prevention: always match before adding; review the unmatched receipt queue regularly.
- **Thermal paper fading** — gas station and restaurant receipts printed on thermal paper become blank within 1-2 years. Prevention: photograph immediately; do not rely on keeping the paper original.

---

## Decision Points

- Receipt captured but no bank feed match found — hold in unmatched queue for 7 days; if no match appears, create the expense manually and investigate whether the charge hit a different account
- Client cannot produce a receipt for an expense over $75 — attempt to obtain a duplicate from the vendor; if unavailable, document the attempt and the business purpose in a contemporaneous log (per Cohan rule, partial substantiation may be allowed for non-274 expenses)
- Multiple receipts match the same bank transaction amount — verify by date and vendor; if ambiguous, confirm with the client before matching
- Client wants to keep paper only — explain Rev. Proc. 98-25 benefits; if they insist, scan copies for the firm's workpapers and return originals
- Third-party tool vs. built-in capture — use third-party tools when managing 3+ clients or when receipt volume exceeds 50/month per client; the platform's native capture is sufficient for lower volumes

---

## References

- IRC Section 274 — Disallowance of certain entertainment, amusement, and recreation expenses
- IRC Section 6501 — Limitations on assessment and collection
- Rev. Proc. 98-25 — Automatic consent for electronic storage systems
- IRS Publication 463 — Travel, Gift, and Car Expenses (substantiation requirements)
- IRS Publication 583 — Starting a Business and Keeping Records
- UCC Article 3 — Negotiable instruments (check retention context)
