# Revenue Recognition — ASC 606

Guidance for recognizing revenue from contracts with customers under ASC 606.

**Tier**: 2 (effective date log — ASC 606 has adoption dates and subsequent amendments)

**Skills**: `bookkeeping-monthly-close`, `bookkeeping-review`, `tax-provision`, `financial-statement-preparation`

## Contents

- [Overview](#overview)
- [Five-Step Model](#five-step-model)
- [Over-Time vs Point-in-Time](#over-time-vs-point-in-time)
- [Variable Consideration](#variable-consideration)
- [Contract Modifications](#contract-modifications)
- [Principal vs Agent](#principal-vs-agent)
- [Licensing](#licensing)
- [Common C-Corp Patterns](#common-c-corp-patterns)
- [GL Account Setup and Recording](#gl-account-setup-and-recording)
- [Decision Points](#decision-points)
- [Effective Date Log](#effective-date-log)
- [References](#references)

---

## Overview

ASC 606 establishes a single, comprehensive framework for recognizing revenue from contracts with customers. The core principle: recognize revenue to depict the transfer of promised goods or services to customers in an amount that reflects the consideration the entity expects to be entitled to in exchange for those goods or services.

ASC 606 replaced industry-specific guidance (ASC 605 and related subtopics) with one model applicable across all industries.

---

## Five-Step Model

### Step 1: Identify the Contract

A contract exists when all of the following criteria are met:

- **Approval**: parties have approved the contract (written, oral, or implied by customary business practices)
- **Rights identified**: each party's rights regarding goods or services are identifiable
- **Payment terms**: payment terms are identifiable
- **Commercial substance**: the contract has commercial substance (future cash flows are expected to change)
- **Probable collection**: it is probable that the entity will collect the consideration it is entitled to

If criteria are not met, consider whether payments received are a deposit (liability) rather than revenue.

### Step 2: Identify Performance Obligations

A performance obligation (PO) is a promise to transfer a distinct good or service (or a bundle). A good or service is **distinct** if both:

- **Capable of being distinct**: the customer can benefit from the good or service on its own or together with other readily available resources
- **Distinct within the contract**: the promise is separately identifiable from other promises (not highly interrelated, does not significantly modify or customize another promised good/service, not highly dependent on other promises)

A series of distinct goods or services that are substantially the same and have the same pattern of transfer (e.g., monthly cleaning service) may be treated as a single PO.

### Step 3: Determine the Transaction Price

The transaction price is the amount the entity expects to be entitled to, including:

- **Fixed consideration**: stated contract price
- **Variable consideration**: discounts, rebates, refunds, credits, price concessions, incentives, performance bonuses, penalties. Estimate using either expected value (probability-weighted) or most likely amount, whichever better predicts the outcome.
- **Significant financing component**: if timing of payments differs significantly from transfer of goods/services (>1 year), adjust for the time value of money. Practical expedient: ignore if period between payment and transfer is one year or less.
- **Non-cash consideration**: measure at fair value at contract inception
- **Consideration payable to a customer**: reduce the transaction price unless the payment is for a distinct good or service from the customer

### Step 4: Allocate the Transaction Price

Allocate the transaction price to each PO based on relative **standalone selling price (SSP)**. Methods to estimate SSP:

- **Adjusted market assessment**: estimate the price customers in the market would pay
- **Expected cost plus margin**: forecast costs to satisfy the PO, add an appropriate margin
- **Residual approach**: permitted only when SSP is highly variable or uncertain; allocate residual amount after allocating observable SSPs to other POs

Discounts are allocated proportionally to all POs unless observable evidence indicates the discount relates entirely to one or more (but not all) POs.

### Step 5: Recognize Revenue

Recognize revenue when (or as) each PO is satisfied by transferring control of the promised good or service to the customer.

---

## Over-Time vs Point-in-Time

### Over-Time Recognition

Revenue is recognized over time if **any one** of these criteria is met:

1. **Simultaneous receipt and consumption**: the customer simultaneously receives and consumes the benefits as the entity performs (e.g., recurring services — cleaning, payroll processing)
2. **Customer-controlled asset**: the entity's performance creates or enhances an asset the customer controls as it is created (e.g., building on customer's land)
3. **No alternative use + right to payment**: the entity's performance creates an asset with no alternative use to the entity, and the entity has an enforceable right to payment for performance completed to date (e.g., custom manufacturing)

### Measuring Progress Over Time

- **Input methods**: measure progress based on inputs (costs incurred, labor hours, machine hours). Cost-to-cost is most common: revenue = (costs to date / total estimated costs) x transaction price.
- **Output methods**: measure progress based on outputs (units produced, milestones reached, surveys of performance). Used when outputs directly measure value transferred.

### Point-in-Time Recognition

If none of the over-time criteria are met, revenue is recognized at the point in time when control transfers. Indicators of control transfer: entity has present right to payment, customer has legal title, customer has physical possession, customer has significant risks and rewards, customer has accepted the asset.

---

## Variable Consideration

### Constraint

Include variable consideration in the transaction price only to the extent it is **highly probable** (IFRS: "highly probable"; US GAAP: "probable") that a significant reversal of cumulative revenue recognized will **not** occur when the uncertainty is resolved.

Factors increasing the risk of reversal: susceptibility to external factors, long resolution period, limited experience with similar contracts, broad range of possible outcomes, practice of offering concessions.

### Reassessment

Reassess the estimated transaction price (including the constraint) at each reporting date. Changes are recognized as revenue adjustments in the period of change (cumulative catch-up).

---

## Contract Modifications

A contract modification is a change in scope, price, or both that is approved by the parties.

- **Separate contract**: treat as a separate contract if the modification adds distinct goods/services AND the price increase reflects standalone selling prices. Account for prospectively.
- **Not a separate contract — distinct remaining goods/services**: treat as termination of the old contract and creation of a new one. Reallocate consideration (including unrecognized amounts from original contract) to remaining POs.
- **Not a separate contract — not distinct remaining goods/services**: treat as part of the original contract. Update progress measurement cumulatively (catch-up adjustment).

---

## Principal vs Agent

Determines whether to recognize revenue **gross** (principal) or **net** (agent/commission):

- **Principal**: controls the good or service before transfer to the customer. Indicators: primary obligation for fulfillment, inventory risk, pricing discretion. Recognize gross revenue.
- **Agent**: arranges for another party to provide the good or service. Does not control the good/service before transfer. Recognize net revenue (fee or commission).

Evaluate on a per-PO basis — an entity can be principal for some POs and agent for others within the same contract.

---

## Licensing

Two types of licenses:

- **Right to access** (dynamic IP — brand, software updated frequently): recognize over time as the entity performs; the customer benefits from ongoing access to the licensor's IP
- **Right to use** (static IP — completed software, music, film): recognize at point in time when the license is transferred; the customer can use the IP as it exists at grant

Factors determining the type: entity undertakes activities that significantly affect the IP, customer is exposed to effects of those activities, activities do not result in transfer of a separate good/service.

---

## Common C-Corp Patterns

- **Product sales (wholesale/retail)**: typically point-in-time at delivery or shipment (depending on shipping terms). Variable consideration for returns — estimate return liability and refund asset.
- **Service contracts (consulting, professional services)**: typically over-time under criterion 1 (simultaneous receipt/consumption). Use input methods (hours) or output methods (deliverables).
- **Long-term construction**: over-time under criterion 3 (no alternative use + right to payment). Cost-to-cost method most common.
- **Software licenses**: right-to-use (point-in-time) if no significant ongoing updates; right-to-access (over-time) if continuous updates are integral.
- **SaaS/subscription**: over-time under criterion 1. Recognize ratably over the subscription period.
- **Bundled arrangements** (hardware + software + support): identify each PO, allocate by SSP, recognize each PO per its satisfaction pattern.
- **Warranties**: assurance-type (included in sale price, not a separate PO — accrue estimated cost) vs service-type (extends beyond assurance — separate PO, defer revenue).

---

## GL Account Setup and Recording

### Account Setup

- **Revenue accounts**: create separate revenue accounts for each significant PO type or revenue stream (product sales, service revenue, license revenue, subscription revenue). Enables disaggregated reporting.
- **Deferred Revenue / Contract Liability**: create as a current liability. Name: "Deferred Revenue" or "Contract Liability". Used when cash is received before PO is satisfied.
- **Unbilled Revenue / Contract Asset**: create as a current asset. Name: "Unbilled Revenue" or "Contract Asset". Used when revenue is recognized before invoicing (over-time recognition with billing in arrears).

### Recording Patterns

**Point-in-time (product sale):**
- Upon delivery: Debit Accounts Receivable, Credit Revenue

**Over-time (service contract, monthly recognition):**
- Billing: Debit Accounts Receivable, Credit Deferred Revenue (if billed in advance)
- Monthly recognition: Debit Deferred Revenue, Credit Service Revenue
- Or if billed in arrears: Debit Unbilled Revenue, Credit Service Revenue; upon billing: Debit Accounts Receivable, Credit Unbilled Revenue

**Bundled arrangement:**
- Split invoice across PO revenue accounts based on SSP allocation. Use journal entry or line-item detail on the invoice.

### Tracking

Most accounting systems do not have native contract-level ASC 606 tracking. Use sub-customers or projects to track revenue by contract, classes or tags to categorize revenue by PO type, custom fields on invoices for contract reference, and external schedules for complex arrangements (multi-year, variable consideration).

For platform-specific execution, invoke `qbo-integration:qbo-bookkeeping`.

---

## Decision Points

- **Contract existence** — if collection is not probable, do not recognize revenue. Reassess when circumstances change.
- **Distinct POs** — when goods/services are highly interrelated or significantly customized together, they are likely a single PO (e.g., design + build in construction).
- **SSP not directly observable** — use adjusted market or expected cost plus margin. Document methodology and rationale. Residual approach is a last resort.
- **Variable consideration** — apply the constraint conservatively. If significant uncertainty exists, exclude from the transaction price until resolved.
- **Bill-and-hold** — revenue can be recognized before delivery only if: customer requested the arrangement, separate identified risk of loss, asset is identified and ready, no ability to use the asset or direct it to another customer.
- **Materiality** — for small C-corps with simple revenue streams, ASC 606 often does not change outcomes vs legacy guidance. Document that the analysis was performed.
- **Returns** — estimate expected returns using historical data. Record refund liability (credit to AR) and asset for right to recover product (debit inventory-like account).

---

## Effective Date Log

- **ASC 606 (ASU 2014-09)**: effective for public business entities for fiscal years beginning after December 15, 2017 (calendar year 2018). Effective for all other entities for fiscal years beginning after December 15, 2018 (calendar year 2019).
- **ASU 2015-14**: deferral of effective date by one year (for both public and private)
- **ASU 2016-08**: principal vs agent considerations (implementation guidance)
- **ASU 2016-10**: identifying performance obligations and licensing (implementation guidance)
- **ASU 2016-12**: narrow-scope improvements and practical expedients (transition)
- **ASU 2016-20**: technical corrections and improvements to ASC 606
- **ASU 2021-08**: contract assets and contract liabilities acquired in business combinations — require acquirer to apply ASC 606 to measure contract assets/liabilities (not fair value); effective for public entities 2023, private entities 2024
- **ASU 2024-13**: disaggregation of income statement expenses — requires disaggregated disclosure of certain expenses (including cost of revenue); effective for public entities beginning after December 15, 2026 (large accelerated filers) and December 15, 2027 (all other public entities)

---

## References

- FASB ASC 606, Revenue from Contracts with Customers
- FASB ASU 2014-09, Revenue from Contracts with Customers (Topic 606)
- FASB ASU 2021-08, Business Combinations — Accounting for Contract Assets and Liabilities
- AICPA Revenue Recognition Guide (industry-specific implementation guidance)
- Deloitte. "A Roadmap to Applying the New Revenue Recognition Standard." Deloitte Accounting Research Tool.
- PwC. "Revenue from Contracts with Customers — Global Edition." PwC Viewpoint.
- EY. "Financial Reporting Developments: Revenue from Contracts with Customers (ASC 606)."
