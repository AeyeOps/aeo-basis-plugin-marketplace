# Extension to File — Form 7004

**Purpose**: Automatic extension of time to file Form 1120 and other business returns; does NOT extend time to pay.

**Skills**: `tax-compliance`, `filing-deadlines`, `extension-management`

---

## Overview

Form 7004 grants an **automatic** extension of time to file certain business income tax, information, and other returns. No reason or justification is required — filing a proper Form 7004 by the original due date automatically grants the extension.

## Extension Periods

- **6 months** — Standard for most entity types including Form 1120 (C-corp)
- **5.5 months** — Estates and trusts (Form 1041)
- **7 months** — C-corporations with June 30 fiscal year-end (tax years beginning before 2026); transitions to 6 months for tax years beginning 2026+

## Form 1120 Filing Deadlines

| Situation | Original Due Date | Extended Due Date |
|-----------|-------------------|-------------------|
| Calendar-year C-corp | April 15 | October 15 |
| June 30 FY C-corp (pre-2026) | September 15 | April 15 (7-month extension) |
| June 30 FY C-corp (2026+) | September 15 | March 15 (6-month extension) |
| Other FY C-corp | 15th of 4th month after FY end | 15th of 10th month after FY end |

## Filing Requirements

### Timing

Form 7004 must be filed **on or before** the original due date of the applicable return. Late-filed extensions are invalid.

### Electronic vs. Paper Filing

Form 7004 can be e-filed for most return types. E-filing is available at IRS.gov/Efile7004.

**Caution**: If Form 7004 is filed on paper but the tax return is filed electronically, the return may be processed before the extension is granted, potentially triggering late-filing notices.

Forms that cannot be e-filed via 7004: Forms 8612, 8613, 8725, 8831, 8876, and 706-GS(D).

### Where to File (Paper)

Filing location depends on total assets and principal business location:
- Under $10M assets — Kansas City or Ogden IRS Service Centers (by state)
- $10M+ assets — Ogden, UT
- Foreign entities — P.O. Box 409101, Ogden, UT 84409

## Payment Requirements

**Form 7004 does NOT extend the time to pay tax.**

Tax payment obligations remain at the **original return due date**. Interest and late-payment penalties accrue from the original due date on any unpaid balance.

### Corporate Safe Harbor for Late Payment Penalty

If the corporation pays at least **90% of the tax shown on the return** by the original due date, the late-payment penalty is waived — provided the remaining balance is paid by the extended due date.

### Tax Estimation (Line 6)

Filers must estimate the total tax liability (including nonrefundable credits) expected for the tax year. Consult Form 1120 instructions for calculation methodology.

## Penalties and Interest

### Late Filing Penalty

Assessed when a return is filed after the due date (including extensions) unless the taxpayer demonstrates reasonable cause. Generally 5% of unpaid tax per month, up to 25%.

### Late Payment Penalty

**0.5% per month** (or part of month) of unpaid tax, capped at 25%. Accrues from the original due date regardless of the extension.

### Interest

Charged on unpaid tax from the original due date until paid. Interest accrues regardless of extensions, reasonable cause, or penalty waivers.

## Decision Points

1. **Can the return be filed by the original due date?** If no, file Form 7004 before the deadline.
2. **Can the full tax liability be paid by the original due date?** If no, pay at least 90% to avoid the late-payment penalty. File the extension regardless.
3. **Is e-filing available for this return type?** Prefer e-filing to avoid processing timing issues.
4. **Is this a June 30 FY corporation?** Check whether 7-month or 6-month extension applies based on tax year.

## Accounting System Integration Notes

- Extension filing does not create a transaction in the accounting system. Document the filing date and confirmation number in the tax notes.
- Any tax payment made with the extension is recorded as a debit to Income Tax Payable and credit to the bank account, dated on the payment date (not the extended filing date). Delegate journal entry execution to `qbo-integration:qbo-bookkeeping`.
- Track the extended due date in the tax calendar to ensure timely filing.

## Year-by-Year Variance Table

| Year | Form 1120 Original Due | Extension Period | Extended Due | Notes |
|------|----------------------|------------------|-------------|-------|
| 2007-2015 | 15th of 3rd month (March 15) | 6 months | September 15 | Old deadline |
| 2016-2025 | 15th of 4th month (April 15) | 6 months | October 15 | PATH Act changed deadline effective for TY 2016+ |
| 2016-2025 (June 30 FY) | September 15 | 7 months | April 15 | Transitional rule |
| 2026+ | 15th of 4th month (April 15) | 6 months | October 15 | Standard |
| 2026+ (June 30 FY) | September 15 | 6 months | March 15 | Transition complete; 7-month rule expires |

The PATH Act of 2015 (P.L. 114-113) moved the Form 1120 due date from the 15th of the 3rd month to the 15th of the 4th month, effective for tax years beginning after December 31, 2015.

## Authoritative References

- IRC Section 6081 — Extension of time for filing returns
- Treas. Reg. 1.6081-3 — Automatic extension for corporation income tax returns
- Treas. Reg. 1.6081-5 — Extensions for certain foreign and domestic entities
- Form 7004 Instructions (Rev. 2025)
- IRC Section 6651 — Failure to file / failure to pay penalties
- IRC Section 6601 — Interest on underpayments
- P.L. 114-113 (PATH Act) — Due date changes for Form 1120
