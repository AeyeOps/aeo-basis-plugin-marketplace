---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Foreign Tax Credit — Form 1118 (Corporations)

**Purpose**: Corporate foreign tax credit computation, separate limitation categories, carryback/carryforward rules, and GILTI/branch income treatment.

**Skills**: `tax-compliance`, `foreign-tax-credit`, `international-tax`

---

## Overview

Form 1118 is the corporate equivalent of individual Form 1116. A **separate Form 1118** must be completed for each applicable separate limitation category. The credit is computed on a category-by-category basis — excess credits in one category cannot offset U.S. tax on income in another category.

## Separate Limitation Categories

A separate Form 1118 is required for each:

1. **Section 951A (GILTI)** — Global intangible low-taxed income under Section 951A, excluding passive income
2. **Foreign Branch Income** — Business profits attributable to qualified business units (QBUs) in foreign countries; excludes passive income
3. **Passive Category Income** — Foreign personal holding company income (dividends, interest, rents, royalties, gains) with exceptions
4. **Section 901(j) Income** — Income from sanctioned countries (Iran, North Korea, Sudan, Syria)
5. **General Category Income** — Residual category for income not fitting elsewhere; includes high-taxed income kicked out of passive
6. **Treaty-Resourced Income** (4 variants) — U.S.-source income re-sourced as foreign under an applicable tax treaty

## Income Classification Rules

### Passive Category Income

Income that would be foreign personal holding company income if the corporation were a CFC, **except**:
- Financial services income
- Export financing interest (unless related-person factoring income)
- High-taxed income (reclassified to general category via HTKO)
- Active rents or royalties derived in the active conduct of a trade or business

### High-Tax Kickout (HTKO)

Passive income is reclassified to **general category** when the effective foreign tax rate on that income exceeds the **highest U.S. corporate rate** (currently 21% under Section 11). Code "HTKO" is used on Schedule A.

### Look-Through Rules (CFCs)

Dividends, interest, rents, and royalties received by a **10% U.S. shareholder** from a **CFC** are not automatically passive. They are assigned to the category of the underlying earnings of the CFC that generated the payment. This also applies to:
- Dividends from noncontrolled 10%-owned foreign corporations
- Intercompany payments between related domestic corporations

## Credit Computation

### Eligible Taxes

A domestic corporation may claim credits for:
- Income, war profits, and excess profits taxes paid or accrued to foreign countries or U.S. possessions
- Taxes in lieu of income taxes under Section 903
- Taxes deemed paid under Section 960 (with respect to CFC inclusions)

### Foreign Tax Credit Limitation

The credit for each category cannot exceed:

```
(Foreign-source taxable income in category / Worldwide taxable income) x U.S. tax
```

Excess credits beyond this limitation are carried back or forward.

### Section 78 Gross-Up

Taxes deemed paid under Section 960 (for Section 951(a)(1) and 951A inclusions) are included as a **gross-up** — added to gross income. This ensures the credit limitation numerator includes the deemed-paid taxes.

## Carryback and Carryforward

Excess foreign tax credits that exceed the limitation for a category:

1. **Carry back 1 year** to offset taxes in the same category
2. Then **carry forward 10 years** to offset taxes in the same category

**Critical exception**: Carryback/carryforward provisions do **NOT** apply to taxes assigned to **Section 951A (GILTI) category** income.

## Overall Foreign Loss (OFL) and Separate Limitation Loss (SLL) Accounts

Form 1118 must be filed (even without electing the credit) if the corporation has any additions to, reductions of, or recapture from:
- Overall foreign loss accounts
- Overall domestic loss accounts
- Separate limitation loss accounts

These accounts track situations where foreign losses offset U.S.-source income (OFL) or vice versa, requiring future income recapture.

## Foreign Tax Redeterminations

The corporation must redetermine its foreign tax credit and U.S. tax liability when:

- Accrued taxes, when actually paid, differ from amounts claimed
- Accrued taxes are not paid within **24 months** after the tax year-end
- Foreign taxes are refunded
- CFC inclusions under Sections 951, 951A, or 1293 change
- The corporation switches between crediting and deducting foreign taxes

## Section 960(d)(4) — New for 2025

Disallows a foreign tax credit under Section 901 for **10%** of any foreign income taxes paid or accrued with respect to Section 959(a) distributions, to the extent the previously taxed E&P were excluded by reason of a Section 951A inclusion.

## Decision Points

1. **Credit vs. deduction?** The credit is almost always more beneficial. Deducting foreign taxes only reduces taxable income; crediting offsets tax dollar-for-dollar (subject to limitation).
2. **Which category?** Classify each item of foreign-source income into the correct category before computing the limitation.
3. **Is HTKO applicable?** Check if the effective foreign rate on passive income exceeds 21%.
4. **Are there CFC payments?** Apply look-through rules to dividends, interest, rents, royalties from CFCs.
5. **Excess credits?** Carry back 1 year first, then forward 10 years (except GILTI category).
6. **Any OFL/SLL recapture required?** Check loss accounts for recapture obligations.

## Accounting System Integration Notes

- Foreign taxes paid are typically recorded as a debit to Foreign Tax Paid (expense account or sub-account of Income Tax Expense). At year-end, the credit election reclassifies these as credits on Form 1118.
- Track foreign-source income by category using class or location tracking if the corporation has multiple categories of foreign income.
- The Section 78 gross-up is a tax return adjustment only — it does not create a journal entry in the accounting system.

For platform-specific account setup and class/location configuration, invoke the appropriate `qbo-integration` skill.

## Year-by-Year Variance Table

| Year | Max Corp Rate | HTKO Threshold | Carryback | Carryforward | Key Changes |
|------|-------------|---------------|-----------|-------------|-------------|
| 2007-2017 | 35% | >35% | 1 year | 10 years | Pre-TCJA; basket system with general and passive categories |
| 2018+ | 21% | >21% | 1 year | 10 years | TCJA: new categories (GILTI, foreign branch); 21% rate lowers limitation ceiling |
| 2018+ (GILTI) | 21% | N/A | None | None | No carryback/carryforward for Section 951A taxes |
| 2022+ | 21% | >21% | 1 year | 10 years | GILTI high-tax exclusion regulations finalized |
| 2025+ | 21% | >21% | 1 year | 10 years | Section 960(d)(4) — 10% disallowance on PTEP distributions from 951A inclusions |

### Pre-TCJA vs. Post-TCJA Category Structure

| Pre-2018 Categories | Post-2017 Categories |
|---------------------|---------------------|
| General limitation | General category |
| Passive | Passive category |
| — | Foreign branch category (new) |
| — | Section 951A / GILTI category (new) |
| Section 901(j) | Section 901(j) (unchanged) |
| Treaty-resourced | Treaty-resourced (unchanged) |

## Authoritative References

- IRC Section 901 — Credit for taxes of foreign countries and U.S. possessions
- IRC Section 904 — Limitation on credit
- IRC Section 951A — GILTI
- IRC Section 960 — Deemed paid credit for subpart F and GILTI inclusions
- IRC Section 78 — Gross-up for deemed paid taxes
- IRC Section 905(c) — Foreign tax redeterminations
- Treas. Reg. 1.904-4 — Separate limitation categories
- Treas. Reg. 1.861-8 through 1.861-17 — Allocation and apportionment of expenses
- Form 1118 Instructions (current revision)
