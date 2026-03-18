---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# 1099 Compliance

Procedures for 1099-NEC and 1099-MISC reporting, W-9 collection, backup withholding, and electronic filing requirements.

**Tier**: 2 (law-indexed — threshold and e-file mandate changes require date tracking)

**Skills**: `bookkeeping-1099`, `bookkeeping-monthly-close`, `bookkeeping-vendor-setup`, `tax-information-returns`

---

## 1099-NEC vs 1099-MISC

**1099-NEC** — nonemployee compensation (contractors, freelancers, attorney service fees). Box 1: payments at or above threshold. Box 4: backup withholding.

**1099-MISC** — everything else: Box 1 rent, Box 2 royalties ($10+), Box 3 other income, Box 6 medical/health care, Box 10 gross proceeds to attorneys (distinct from service fees on NEC).

**Rule**: services = NEC; rent/royalties/other = MISC. Same payee can receive both forms for different payment types.

## Reporting Thresholds

- **TY2024-2025**: $600 general threshold ($10 for royalties)
- **TY2026+**: $2,000 general threshold (One Big Beautiful Bill Act, Sec. 110501, July 2025). Royalty threshold unchanged at $10.

## W-9 Collection and TIN Solicitation

1. Request W-9 **before first payment** — do not pay without one on file
2. Check completeness: name, entity type, TIN (SSN or EIN), address, signature
3. **Entity type determines reportability** — C-Corps and S-Corps generally exempt (exceptions: legal fees, medical payments). Sole props, partnerships, single-member LLCs always reportable.
4. Retain W-9 for 4 years after the last return filed using that TIN
5. Vendor refuses W-9 — initiate backup withholding at 24%

## Backup Withholding

24% rate applies when: vendor fails to furnish TIN, IRS issues B-notice for incorrect TIN, or IRS notifies vendor is subject to withholding. Report in Box 4. Deposit via Form 945 (annual) by January 31.

## Filing Deadlines

- **1099-NEC**: January 31 (paper and e-file). No automatic extension. Recipient copy also January 31.
- **1099-MISC**: February 28 paper / March 31 e-file. 30-day extension via Form 8809. Recipient copy January 31.

## Electronic Filing Mandate

Effective TY2024: filers issuing **10+ information returns** (all types combined — NEC, MISC, INT, W-2, etc.) must e-file. Prior threshold was 250+. E-file via IRS IRIS (free) or third-party services (Tax1099, Track1099).

## B-Notice and TIN Matching

**First B-Notice** (CP2100): send solicitation letter within 15 business days; payee has 30 days to respond with corrected W-9; begin backup withholding if no response. **Second B-Notice** (within 3 years): payee must provide TIN validation from SSA/IRS — W-9 alone insufficient. **Proactive**: use IRS TIN Matching Program before filing.

## Common 1099 Errors

- Wrong box (legal fees on MISC Box 10 vs NEC Box 1)
- Including reimbursed expenses (report fees only under accountable plan)
- Wrong or transposed TIN
- Filing for exempt entities (C-Corps)
- Including credit card payments (processor reports on 1099-K, not payer on NEC)

## Corrected 1099 Filing

**Type 1** (wrong amount/code): file corrected form with "CORRECTED" box checked. **Type 2** (wrong TIN/name/form type): file corrected form with zeroes to void, then file new form with correct info (not marked corrected).

## Platform-Specific Workflows

For platform-specific 1099 vendor setup, payment tracking, and e-filing workflows, invoke `qbo-integration:qbo-bookkeeping`.

## Decision Points

- Vendor W-9 shows C-Corp — generally no 1099 (exception: legal/medical)
- Payment via credit card — do not include on 1099 (processor handles via 1099-K)
- No W-9 returned — begin 24% backup withholding; document solicitation attempts
- Foreign vendor (no US TIN) — no 1099; may require 1042-S
- Contractor paid below threshold — no 1099 required; maintain records

## Effective Date Log

| Change | Effective | Authority |
|--------|-----------|-----------|
| 1099-NEC reintroduced (from MISC Box 7) | TY2020 | IRS Rev. Proc. 2019-29 |
| E-file mandate: 250 lowered to 10 returns | TY2024 | T.D. 9972 (Feb 2023) |
| Threshold raised $600 to $2,000 | TY2026 | One Big Beautiful Bill Act, Sec. 110501 (July 2025) |

## References

- IRS Instructions for Forms 1099-MISC and 1099-NEC (current year)
- IRS Publication 1220 — Specifications for Electronic Filing
- IRS TIN Matching Program — https://www.irs.gov/tax-professionals/taxpayer-identification-number-tin-matching
- T.D. 9972 — Electronic Filing Requirements
- One Big Beautiful Bill Act, Section 110501
