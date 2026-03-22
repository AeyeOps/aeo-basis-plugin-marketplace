---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-22
jurisdiction: federal
---

# Dividends Received Deduction (DRD) — IRC 243-246A

Complete computation algorithm for the C-corporation dividends received deduction.
DRD is reported on Schedule C of Form 1120 and flows to Line 29b (total special
deductions). Rules are year-dependent — apply the rates for the tax year under
preparation.

## Year-Indexed DRD Rate Variance

DRD rates changed at the TCJA boundary. Determine the tax year first, then apply
the corresponding rates throughout the computation.

**Post-TCJA (tax years beginning after December 31, 2017)**:

- Less than 20% ownership: **50%** deduction (IRC 243(a)(1))
- 20% to less than 80% ownership: **65%** deduction (IRC 243(c))
- 80% or more ownership: **100%** deduction (IRC 243(a)(3), affiliated group)

**Pre-TCJA (tax years beginning before January 1, 2018)**:

- Less than 20% ownership: **70%** deduction (IRC 243(a)(1))
- 20% to less than 80% ownership: **80%** deduction (IRC 243(c))
- 80% or more ownership: **100%** deduction (IRC 243(a)(3))

TCJA Section 13002 reduced rates from 70/80 to 50/65, effective for tax years
beginning after December 31, 2017. The 100% affiliated-group tier was unchanged.

**Other year-dependent rules**:

- **IRC 245A participation exemption**: Post-TCJA only (2018+). 100% DRD for foreign-source dividends from specified 10%-owned foreign corporations
- **AMT DRD adjustment**: Pre-TCJA only (2007-2017). Corporate AMT used a 75% DRD limit when computing AMTI. Repealed by TCJA (corporate AMT eliminated)
- **DPAD interaction**: Pre-TCJA only (2007-2017). Section 199 DPAD was part of the taxable income computation ordering before DRD. Repealed by TCJA
- **Corporate tax rate**: Graduated 15-35% (pre-2018) vs flat 21% (2018+). Affects whether DRD creates a net operating loss, which determines whether the 246(b) limitation applies

## DRD Rate Tiers (IRC 243)

Ownership percentage is measured by vote AND value. For each dividend-paying
corporation, determine the receiving corporation's ownership percentage, then
apply the corresponding tier rate for the tax year.

**Qualifying dividends**: DRD applies only to dividends from domestic C-corporations
(and certain qualifying foreign corporations under IRC 245/245A). Dividends from
the following do NOT qualify for DRD under IRC 243:
- S corporations
- Tax-exempt organizations
- REITs (to the extent the dividend is not from accumulated C-corp earnings)
- Regulated investment companies (mutual funds), except to the extent reported as qualifying dividends
- Foreign corporations (use IRC 245 or 245A instead)

## Taxable Income Limitation (IRC 246(b))

The DRD for the sub-100% tiers is limited to the same tier-specific percentage
of taxable income computed WITHOUT the DRD, NOL deduction, capital loss carryback,
or Section 199A deduction.

**Per-tier limitation formula** (all years):

```
For the 50% tier (post-TCJA) or 70% tier (pre-TCJA):
  Limited DRD = lesser of:
    (a) tier_rate × qualifying dividends received, OR
    (b) tier_rate × taxable income before DRD

For the 65% tier (post-TCJA) or 80% tier (pre-TCJA):
  Limited DRD = lesser of:
    (a) tier_rate × qualifying dividends received, OR
    (b) tier_rate × taxable income before DRD
```

The limitation percentage matches the tier rate. This is NOT a blanket 65% (or
80%) cap applied to total DRD — it is computed separately for each tier.

The **100% tier has no taxable income limitation**. Dividends from 80%+ owned
domestic subsidiaries (affiliated group members) receive a full 100% DRD
regardless of taxable income.

"Taxable income before DRD" means taxable income computed on Form 1120 Line 28
(total income minus total deductions), before applying Line 29a (NOL deduction)
and Line 29b (special deductions including DRD).

### NOL Exception (IRC 246(b)(2))

If the FULL (unlimited) DRD would create or increase a net operating loss for
the tax year, the taxable income limitation does NOT apply. The corporation
takes the full DRD.

**Test procedure** (all years):

1. Compute taxable income before DRD and NOL (Line 28 minus Line 29a equivalent)
2. Subtract the full DRD (without applying the limitation)
3. If the result is zero or negative → limitation is waived, take the full DRD
4. If the result is positive → limitation applies, use the per-tier formula above

This exception exists because the purpose of DRD is to eliminate cascading
corporate taxation on intercorporate dividends. Denying the DRD when the
corporation is already in a loss position would defeat that purpose without
generating any tax revenue.

## Holding Period Requirement (IRC 246(c))

All years. The corporation must hold the stock for MORE THAN 45 days during the
91-day period beginning 45 days before the ex-dividend date.

- For preferred stock with dividends attributable to periods totaling more than
  366 days: the holding period is more than 90 days within a 181-day window
- Days count excludes any period where the taxpayer has diminished risk of loss
  through short sales, put options, contractual obligations to sell, or other
  positions (IRC 246(c)(4))
- Failure to meet the holding period for a specific dividend: the entire DRD for
  that dividend is disallowed — the dividend is still taxable income, just without
  the deduction

## Debt-Financed Portfolio Stock (IRC 246A)

All years. If stock is acquired or held with debt ("debt-financed portfolio stock"),
the DRD is reduced proportionally to the extent the stock is debt-financed.

**Reduction formula**:

```
Reduced DRD rate = DRD_tier_rate × (1 - average_indebtedness_percentage)

Where:
  average_indebtedness_percentage = portfolio_indebtedness / adjusted_basis_of_stock
```

Portfolio indebtedness is the average amount of debt allocable to the stock during
the base period (the shorter of the period the stock was held during the tax year
or the period beginning on the ex-dividend date and ending on the date the
corporation disposes of the stock).

**Exceptions — IRC 246A does NOT apply to**:

- Stock in 80%+ owned subsidiaries (the 100% DRD tier)
- Stock qualifying for the Section 245A participation exemption
- Dividends received by a small business investment company (SBIC)

## Foreign Corporation Dividends

### IRC 245 — General Rule (All Years)

Dividends from foreign corporations are partially eligible for DRD to the extent
the foreign corporation has US-source income. The DRD is:

```
DRD = applicable_tier_rate × dividends × (post-1986 undistributed US earnings /
      total post-1986 undistributed earnings)
```

If the foreign corporation has no post-1986 US-source earnings, no DRD is
available under IRC 245. Dividends from a foreign corporation that are not
eligible for DRD under either IRC 245 or 245A are fully taxable with no
deduction (though a foreign tax credit under IRC 901/903 may be available).

### IRC 245A — Participation Exemption (Post-TCJA Only, 2018+)

For tax years beginning after December 31, 2017, a 100% DRD is available for the
**foreign-source portion** of dividends received from "specified 10-percent owned
foreign corporations."

**Requirements**:
- Receiving corporation must be a domestic C-corporation (not an S-corp, RIC, or REIT)
- Must own 10% or more by vote AND value (measured under IRC 958(a), not constructive ownership)
- Dividend must be from a "specified 10-percent owned foreign corporation" — a foreign corporation (other than a PFIC that is not also a CFC) in which the domestic corp owns 10%+
- Must meet a 365-day holding period within a 731-day window (longer than the standard 46-day period under IRC 246(c))

**Exclusions**:
- Hybrid dividends (amounts deductible by the foreign corporation under its local law)
- PFIC stock (unless also a CFC with respect to the shareholder)
- Subpart F inclusions and GILTI inclusions (these are included in income directly under IRC 951/951A, not through DRD)
- The US-source portion of the dividend (only the foreign-source portion qualifies)

**Interaction with IRC 245**: For dividends from a 10%+ owned foreign corporation
in tax years 2018+, IRC 245A generally supersedes IRC 245 for the foreign-source
portion. The US-source portion may still be eligible for DRD under IRC 245 to the
extent of US earnings.

## Computation Ordering on Form 1120

DRD interacts with charitable contributions and NOL in a circular dependency.
Resolution sequence (all years, with noted differences):

1. Compute total income (Lines 1-11)
2. Compute all deductions except: charitable contribution, DRD, NOL, and DPAD (Lines 12-18, 20-26)
3. Compute preliminary taxable income = total income − deductions from step 2
4. Charitable contribution limit = 10% of preliminary TI (before charitable deduction, DRD, NOL, capital loss carryback) per IRC 170(b)(2)
5. Deductible charitable = lesser of actual contributions and the 10% limit
6. Taxable income before special deductions = preliminary TI − deductible charitable − DPAD (pre-2018 only)
7. **Compute DRD** per tier, applying IRC 246(b) limitation unless the NOL exception applies
8. Apply NOL deduction (Line 29a) — post-TCJA: limited to 80% of taxable income after DRD; pre-TCJA: no percentage limitation
9. Final taxable income (Line 30)

The circular dependency between charitable contribution and taxable income is
resolved by computing the charitable limit before DRD (step 4 uses preliminary TI
that excludes DRD). DRD does not affect the charitable limit calculation.

## Schedule C Line-by-Line Walkthrough

Schedule C reports dividends received and computes the DRD. It has three columns
across approximately 20 lines (line numbers may vary slightly by form revision year):

**Column (a)**: Dividends received — the gross amount of dividends from each category
**Column (b)**: Percentage — the applicable DRD rate for the tier
**Column (c)**: Special deductions — the computed DRD amount (column a × column b)

### Line groups (post-TCJA form structure)

**Lines 1-2** — Domestic corporations by ownership tier:
- Line 1: Less than 20% owned domestic corporations (50% rate post-TCJA, 70% pre-TCJA)
- Line 2: 20% or more owned domestic corporations (excluding 80%+ affiliates) (65% rate post-TCJA, 80% pre-TCJA)

**Line 3** — Debt-financed stock dividends (IRC 246A applies — the rate in column b is the reduced rate after applying the debt-finance reduction)

**Line 4** — Affiliated group members (80%+ owned, 100% rate, all years)

**Lines 5-8** — Special categories:
- Line 5: Certain preferred stock of less-than-20% owned public utilities
- Line 6: Certain preferred stock of 20%+ owned public utilities
- Line 7: Other qualifying dividends (e.g., from tax-exempt organizations to the extent of qualifying amounts)
- Line 8: Foreign corporation dividends qualifying under IRC 245(b) (100% DRD for wholly-owned foreign subsidiary dividends out of US earnings)

**Lines 9-10** — Foreign-source dividends:
- Line 9: Foreign corporation dividends not qualifying for special rate under Line 8 (IRC 245(a) partial DRD based on US-source earnings ratio)
- Line 10: IRC 245A dividends (post-TCJA only, 100% participation exemption)

**Lines 11-13** — Subtotals:
- Line 11: IC-DISC and former DISC dividends
- Line 12: Other dividends
- Line 13: Total dividends — sum of all lines above (column a only)

**Lines 14-19** — Taxable income limitation computation:
- Line 14: Total of column (a) amounts subject to the 50% (or 70% pre-TCJA) tier
- Line 15: Total of column (c) amounts for these lines (the tentative DRD before limitation)
- Line 16: Taxable income computed without DRD, NOL deduction, and capital loss carryback
- Line 17: Line 16 × 50% (or 70% pre-TCJA)
- Line 18: The lesser of Line 15 or Line 17 — UNLESS the amount on Line 15 creates an NOL (in which case Line 15 applies regardless)
- Line 19: Total deductions — Line 18 + amounts from lines not subject to limitation (100% tier, 245A amounts)

**Line 20** — Total special deductions. This amount flows to Form 1120, Page 1, Line 29b.

Note: The limitation computation on Lines 14-19 as shown on the current form
applies the limitation in aggregate by tier group. For corporations with dividends
in both the 50% and 65% tiers, the limitation is computed separately for each
tier. The form handles this through the line groupings.

## Worked Examples

### Example 1: Basic DRD, No Limitation Binding (Post-TCJA)

Corporation receives $10,000 in dividends from a domestic corporation in which
it owns 5% (less than 20% tier). Tax year 2022.

- DRD rate: 50% (post-TCJA, <20% ownership)
- Tentative DRD: $10,000 × 50% = $5,000
- Taxable income before DRD: $100,000
- Limitation: 50% × $100,000 = $50,000
- $5,000 < $50,000 → limitation does not bind
- **DRD claimed: $5,000**

### Example 2: Limitation Applies, Reduces DRD (Post-TCJA)

Corporation receives $50,000 in dividends from a domestic corporation in which
it owns 10% (<20% tier). Tax year 2023. Other income and deductions yield taxable
income before DRD of $8,000.

- DRD rate: 50%
- Tentative DRD: $50,000 × 50% = $25,000
- Taxable income before DRD: $8,000
- Limitation: 50% × $8,000 = $4,000
- $25,000 > $4,000 → limitation applies
- Test NOL exception: $8,000 − $25,000 = −$17,000 (negative → NOL would be created)
- **246(b)(2) exception applies** → limitation is waived
- **DRD claimed: $25,000** (full amount, because it creates an NOL)

### Example 3: Limitation Applies and Binds (Post-TCJA)

Corporation receives $6,000 in dividends from a domestic corporation (<20% tier).
Tax year 2022. Taxable income before DRD is $4,500.

- DRD rate: 50%
- Tentative DRD: $6,000 × 50% = $3,000
- Limitation: 50% × $4,500 = $2,250
- $3,000 > $2,250 → limitation applies
- Test NOL exception: $4,500 − $3,000 = $1,500 (positive → no NOL created)
- 246(b)(2) exception does NOT apply
- **DRD claimed: $2,250** (capped by limitation)

### Example 4: Pre-TCJA Rates (2015 Tax Year)

Same facts as Example 1 but tax year is 2015.

- DRD rate: 70% (pre-TCJA, <20% ownership)
- Tentative DRD: $10,000 × 70% = $7,000
- Taxable income before DRD: $100,000
- Limitation: 70% × $100,000 = $70,000
- $7,000 < $70,000 → limitation does not bind
- **DRD claimed: $7,000** (vs $5,000 post-TCJA — the $2,000 difference is entirely due to the rate change)

## Authoritative References

- IRC 243 — Dividends received by corporations (rate tiers)
- IRC 243(c) — Special rule for 20%-owned corporations (65% post-TCJA, 80% pre-TCJA)
- IRC 245 — Dividends received from certain foreign corporations (US-source earnings fraction)
- IRC 245A — Deduction for foreign-source portion of dividends from specified 10%-owned foreign corporations (post-TCJA)
- IRC 246(b) — Limitation on aggregate amount of deductions (taxable income limitation)
- IRC 246(b)(2) — Exception when DRD creates or increases NOL
- IRC 246(c) — Holding period requirement (46 days in 91-day window)
- IRC 246A — Dividends received deduction reduced where portfolio stock is debt-financed
- TCJA Section 13002 — DRD rate reduction (70/80 to 50/65), effective 2018+
- TCJA Section 14101 — IRC 245A participation exemption, effective 2018+
- Form 1120, Schedule C Instructions (current and prior year revisions)
