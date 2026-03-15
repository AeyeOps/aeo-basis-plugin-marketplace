# Lease Accounting — ASC 842

Guidance for recognizing, measuring, and recording leases under ASC 842 (Leases).

**Tier**: 2 (effective date log — ASC 842 has adoption dates and subsequent amendments)

**Skills**: `bookkeeping-monthly-close`, `bookkeeping-review`, `tax-provision`, `financial-statement-preparation`

## Contents

- [Overview](#overview)
- [Lease Classification](#lease-classification)
- [Initial Measurement](#initial-measurement)
- [Subsequent Measurement](#subsequent-measurement)
- [Short-Term Lease Exemption](#short-term-lease-exemption)
- [Lease Modifications](#lease-modifications)
- [Practical Expedients](#practical-expedients)
- [Disclosure Requirements](#disclosure-requirements)
- [GL Account Setup](#gl-account-setup)
- [Decision Points](#decision-points)
- [Effective Date Log](#effective-date-log)
- [References](#references)

---

## Overview

ASC 842 requires lessees to recognize virtually all leases with terms greater than 12 months on the balance sheet. The lessee records a right-of-use (ROU) asset and a corresponding lease liability at lease commencement. This standard replaced ASC 840, which allowed operating leases to remain off-balance-sheet.

Core principle: a lease conveys the right to control the use of an identified asset for a period of time in exchange for consideration. Control exists when the lessee directs the use of the asset and obtains substantially all its economic benefits.

---

## Lease Classification

Leases are classified as either **finance** or **operating** at commencement. Classification determines the expense pattern, not whether the lease appears on the balance sheet (both types do).

### Finance Lease Criteria

A lease is a finance lease if it meets **any one** of these five criteria:

1. **Ownership transfer** — the lease transfers ownership of the asset to the lessee by the end of the lease term
2. **Purchase option** — the lease grants a purchase option that the lessee is reasonably certain to exercise
3. **Lease term** — the lease term is for the major part of the remaining economic life of the asset (bright-line guidance: typically 75% or more)
4. **Present value** — the present value of lease payments plus any residual value guarantee equals or exceeds substantially all of the fair value of the asset (bright-line guidance: typically 90% or more)
5. **Specialized asset** — the underlying asset is so specialized that it has no alternative use to the lessor at the end of the lease term

If none of these criteria are met, the lease is an **operating lease**.

---

## Initial Measurement

### Lease Liability

Lease liability = present value of remaining lease payments, discounted at:
- **Rate implicit in the lease** — if determinable (rare for lessees)
- **Lessee's incremental borrowing rate (IBR)** — the rate the lessee would pay to borrow on a collateralized basis over a similar term in a similar economic environment

Lease payments included in the measurement: fixed payments (less lease incentives), variable payments based on an index or rate, exercise price of purchase option (if reasonably certain), termination penalties (if lease term reflects exercise), and residual value guarantees.

Variable payments tied to usage or performance (e.g., per-mile charges) are excluded from the liability and expensed as incurred.

### ROU Asset

ROU asset = lease liability + initial direct costs (e.g., commissions, legal fees) + lease payments made at or before commencement (prepaid rent) - lease incentives received.

---

## Subsequent Measurement

### Operating Leases

- **Lease expense**: recognized on a straight-line basis over the lease term as a single line item (typically within operating expenses)
- **Lease liability**: reduced by the cash payment, increased by interest (discount rate applied to opening liability balance)
- **ROU asset**: amortized as a plug — total lease expense minus the interest on the liability equals the ROU amortization for the period
- Result: level expense each period, with front-loaded interest and back-loaded amortization

### Finance Leases

- **Amortization expense**: ROU asset amortized on a straight-line basis over the shorter of the lease term or the useful life of the asset (amortization expense, separate from interest)
- **Interest expense**: computed using the effective interest method on the lease liability balance
- Result: front-loaded total expense (interest is highest in early periods, declining over time)
- Two line items on the income statement: amortization expense and interest expense

---

## Short-Term Lease Exemption

Leases with a term of 12 months or less at commencement (with no purchase option the lessee is reasonably certain to exercise) may be excluded from balance-sheet recognition.

- Election is made by asset class (e.g., exempt all short-term equipment leases, but not short-term vehicle leases)
- If elected, expense lease payments on a straight-line basis (or other systematic basis if more representative)
- Disclose the election and short-term lease cost in the footnotes
- A lease that is short-term at commencement but is later modified to extend beyond 12 months must be reassessed and recognized on the balance sheet

---

## Lease Modifications

A modification is a change in the scope or consideration of a lease not part of the original terms.

- **Separate lease treatment**: if the modification grants an additional right of use that is distinct AND the increase in consideration is commensurate with the standalone price (adjusted for the contract), treat as a separate new lease
- **Not a separate lease**: remeasure the lease liability using a revised discount rate at the modification date, and adjust the ROU asset by the same amount. If the modification decreases scope, reduce the ROU asset proportionally and recognize any gain/loss
- Reassess classification at the modification date

---

## Practical Expedients

### Transition Package (Package of Three)

Entities may elect as a package (all or none) to not reassess at transition:
1. Whether existing contracts are or contain leases
2. Lease classification for existing leases
3. Whether initial direct costs qualify for capitalization

### Hindsight Practical Expedient

Allows use of hindsight when determining lease term (e.g., whether renewal options are reasonably certain) and assessing impairment of ROU assets at transition. Elected separately from the package of three.

### Other Expedients

- **Portfolio approach**: apply to a portfolio of leases with similar characteristics if the result is not materially different from individual application
- **Non-lease components**: lessee may elect, by asset class, to combine lease and non-lease components (e.g., common area maintenance in real estate) and account for as a single lease component

---

## Disclosure Requirements

- Lease cost by type: finance lease (amortization + interest), operating lease cost, short-term lease cost, variable lease cost
- Maturity analysis: undiscounted future lease payments by year for the next five years and thereafter, reconciled to the lease liability
- Weighted-average remaining lease term (separately for operating and finance)
- Weighted-average discount rate (separately for operating and finance)
- Supplemental cash flow information: cash paid for leases, ROU assets obtained in exchange for lease liabilities
- Qualitative disclosures: nature of leases, significant assumptions and judgments, restrictions or covenants

---

## GL Account Setup

Most accounting systems do not have a dedicated lease accounting module. Manual account setup and journal entries are required.

### Required Accounts

- **ROU Asset — Operating Leases**: create as a non-current asset (or current asset if term < 1 year remaining). Name: "Right-of-Use Asset — Operating"
- **ROU Asset — Finance Leases**: create as a non-current asset. Name: "Right-of-Use Asset — Finance"
- **Lease Liability — Current**: create as a current liability. Name: "Lease Liability — Current Portion"
- **Lease Liability — Non-Current**: create as a long-term liability. Name: "Lease Liability — Long-Term"
- **Lease Expense**: use existing rent/lease expense account for operating leases
- **Amortization Expense**: use depreciation/amortization expense account for finance leases
- **Interest Expense — Finance Leases**: use existing interest expense account

### Monthly Journal Entry — Operating Lease

1. Debit Lease Expense (straight-line amount)
2. Credit Cash / AP (actual payment)
3. Debit or Credit ROU Asset (plug to balance — difference between straight-line expense and interest on liability)
4. Debit Lease Liability (principal portion of payment)
5. Credit Lease Liability (interest accrual — discount rate x opening liability)

### Monthly Journal Entry — Finance Lease

1. Debit Amortization Expense (straight-line on ROU asset)
2. Credit Accumulated Amortization / ROU Asset
3. Debit Interest Expense (effective interest on liability)
4. Debit Lease Liability — principal reduction
5. Credit Cash (actual payment)

### Amortization Schedule

Maintain an external amortization schedule (spreadsheet) for each lease tracking: opening liability, interest, payment, closing liability, opening ROU, amortization, closing ROU. Journal entries in the GL are driven from this schedule.

For platform-specific execution, invoke `qbo-integration:qbo-bookkeeping`.

---

## Decision Points

- **Lease vs service contract** — if the customer does not control an identified asset (e.g., vendor can substitute equipment freely), it is a service, not a lease. No ASC 842 treatment.
- **Renewal options** — include in lease term only if reasonably certain to exercise. Factors: economic incentive, business need, leasehold improvements, termination penalties, history of renewals.
- **IBR determination** — for private companies without public debt, estimate based on secured borrowing rate of similar term. Document the methodology.
- **Short-term election** — evaluate by asset class. If most leases in a class are short-term and amounts are immaterial, the exemption reduces complexity significantly.
- **Embedded leases** — review service contracts, outsourcing agreements, and IT hosting arrangements for embedded lease components.
- **Materiality** — private companies may use materiality to limit the number of leases recognized. Document the threshold and rationale.

---

## Effective Date Log

- **ASC 842 (ASU 2016-02)**: effective for public business entities for fiscal years beginning after December 15, 2018 (calendar year 2019). Effective for all other entities for fiscal years beginning after December 15, 2021 (calendar year 2022).
- **ASU 2018-10**: codification improvements to ASC 842 (clarifications, corrections)
- **ASU 2018-11**: targeted improvements — optional transition method allowing application at adoption date rather than earliest comparative period
- **ASU 2019-01**: codification improvements — sales-type lessor transition, cash flow statement classification
- **ASU 2020-05**: effective date deferral for private companies (deferred to 2022)
- **ASU 2021-09**: discount rate for lessees that are not public business entities — allows risk-free rate election by asset class
- **ASU 2023-01**: common control leases — allows private companies to use written terms and conditions (not legally enforceable terms) for common control arrangements; effective for fiscal years beginning after December 15, 2023
- **ASU 2024-02**: profits interests and similar awards — clarification of scope for certain compensation arrangements; effective for fiscal years beginning after December 15, 2024

---

## References

- FASB ASC 842, Leases
- FASB ASU 2016-02, Leases (Topic 842)
- FASB ASU 2023-01, Common Control Arrangements (Topic 842)
- FASB ASU 2024-02, Profits Interests (Topic 842)
- Deloitte. "A Roadmap to Applying the New Leasing Standard." Deloitte Accounting Research Tool.
- PwC. "Leases Guide." PwC Viewpoint.
- AICPA. "Accounting and Auditing Guide: Leases."
