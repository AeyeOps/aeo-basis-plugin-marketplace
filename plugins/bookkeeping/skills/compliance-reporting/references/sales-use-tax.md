---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: multistate
---

# Sales and Use Tax Compliance

Procedures for managing sales tax nexus, use tax obligations, exemption certificates, and state filings for bookkeeping clients.

**Tier**: 2 (law-indexed — nexus thresholds and marketplace rules change by state and year)

**Skills**: `bookkeeping-sales-tax`, `bookkeeping-monthly-close`, `tax-state-compliance`, `client-onboarding`

---

## Sales Tax Nexus

### Physical Presence Nexus
Business has nexus via: office/warehouse/store, employees (including remote), inventory stored (including FBA), equipment, or sales representatives in the state.

### Economic Nexus (post-Wayfair 2018)
States can require collection based on economic activity alone — typically **$100K in sales OR 200 transactions** (some states sales-only, e.g., Texas $500K). Measured over current or prior calendar year. Once exceeded, registration is mandatory even with no physical presence.

## Use Tax Obligation

When a business buys taxable items and the seller does not charge sales tax, the buyer owes **use tax** to their home state at the same rate. Common triggers: online out-of-state purchases, trade show equipment, supplies from non-collecting sellers. Record as liability when purchased; remit on the regular sales tax return (separate line). Review expense transactions monthly for untaxed purchases of taxable goods.

## Marketplace Facilitator Laws

All 45 sales-tax states + DC have marketplace facilitator laws. Amazon, eBay, Etsy, Walmart Marketplace collect and remit on behalf of third-party sellers. Direct sales (own website, phone, in-person) remain the seller's responsibility. Track marketplace vs direct sales separately.

## Exemption Certificates

1. Collect valid certificate **before or at time of exempt sale** (multi-state: Streamlined or Multi-Jurisdiction form accepted)
2. Validate: buyer name, address, state, exemption reason, signature, active permit number
3. Retain blanket certificates for ongoing relationships; single-purchase certificates for one-time sales
4. Retention: generally 3–4 years after last covered sale (state-dependent; e.g., Florida: 3 years, California: 4 years)
5. Common types: resale, manufacturing, agriculture, government, nonprofit (not all states exempt nonprofits)

## State-Specific Rules

State-specific rules vary. Key items to verify per state:

- **Base rate and local surtax structure** — combined rates differ by jurisdiction (state base + county/city/district add-ons)
- **Taxable service definitions** — most states tax tangible goods by default; service taxability varies widely
- **Dealer collection allowances** — some states offer a discount for timely filing and remittance
- **Filing frequency thresholds** — assigned by the state based on annual tax liability (monthly, quarterly, semi-annually, or annually)
- **Key exemptions and certificates** — common categories include resale, manufacturing, agriculture, government, and nonprofit (not all states exempt all categories)
- **Form numbers and due dates** — each state has its own return form, due date schedule, and EFT requirements

Common state forms include DR-15 (Florida), ST-3 (New Jersey), ST-100 (New York), and ST-1 (Illinois). Consult the relevant state department of revenue for current forms and instructions.

## Filing and Remittance

1. Calculate tax collected (review Sales Tax Liability report)
2. Calculate use tax owed on untaxed purchases
3. Apply credits (overpayments, bad debt deductions)
4. Subtract collection allowance if applicable (where state offers one; timely filing typically required)
5. File return by due date per state schedule
6. Remit payment (EFT required above state-specific thresholds)
7. Record in GL: debit Sales Tax Payable, credit Cash/Checking

## Audit Preparation

Maintain at all times: exemption certificate file organized by customer, copies of all returns with payment proof, nexus analysis documentation, taxability research memos, and monthly reconciliation of sales per books to sales per returns. Audit triggers: consistently late filings, large refund claims, industry-targeted audits, nexus questionnaire responses.

## Platform-Specific Workflows

For platform-specific sales tax center configuration, rate calculation, auto-filing, and payment recording workflows, invoke `qbo-integration:qbo-bookkeeping`.

## Decision Points

- Client sells in 1–2 states — manage manually in accounting platform
- Client sells in 5+ states — recommend third-party sales tax automation services (e.g., Avalara, TaxJar)
- Economic nexus threshold exceeded — register immediately; back-tax exposure accrues
- Customer claims verbal exemption — require written certificate before exempting
- Nexus questionnaire received — do not ignore; consult CPA before responding
- Zero-tax-due period — file the return anyway; non-filing triggers delinquency notices
- Sales only on marketplaces — marketplace collects; still file if registered

## Effective Date Log

| Change | Effective | Authority |
|--------|-----------|-----------|
| Wayfair — economic nexus authorized | June 21, 2018 | *South Dakota v. Wayfair*, 585 U.S. 162 |
| All 45 sales-tax states + DC enacted marketplace facilitator laws | 2018–2021 | Various state legislation |

## References

- *South Dakota v. Wayfair, Inc.*, 585 U.S. 162 (2018)
- Streamlined Sales Tax Governing Board — https://www.streamlinedsalestax.org
- Sales Tax Institute — https://www.salestaxinstitute.com
