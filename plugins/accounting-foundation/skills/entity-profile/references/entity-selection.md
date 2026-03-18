---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: multistate
---

# Entity Selection and Formation — Business Structure Advisory

**Purpose**: Entity type comparison (C-corp, S-corp, LLC/partnership, sole proprietorship), tax implications of each structure, formation procedures, S-election mechanics, and Florida-specific considerations.

**Skills**: `tx/officer-compensation` (S-corp reasonable compensation), `tx/accumulated-earnings-tax` (C-corp retained earnings risk)

**Tier**: 2 (effective date log — TCJA rate changes and QBI deduction sunset drive entity selection math)

---

## Entity Comparison Overview

### C-Corporation (Form 1120)

- **Taxation**: Entity-level tax at flat 21% (post-TCJA). Distributions taxed again as dividends to shareholders (qualified dividends at 0%/15%/20%). Double taxation is the defining characteristic.
- **Shareholders**: Unlimited number, any type (individuals, entities, foreign persons, trusts)
- **Classes of stock**: Unlimited classes permitted (common, preferred, voting, non-voting)
- **Losses**: Do NOT pass through. Trapped at the entity level. Can offset future C-corp income via NOL carryforward (80% limit post-2017; see `tx/nol-tracking`).
- **Fringe benefits**: Employer-paid health insurance, group-term life ($50K), and other fringe benefits are deductible by the corp and excluded from shareholder-employee income. This is a significant C-corp advantage.
- **Retained earnings**: Retained at entity level. Accumulated earnings tax (AET) risk if earnings exceed reasonable business needs ($250K threshold for most corps). See `tx/accumulated-earnings-tax`.
- **Self-employment tax**: Not applicable. Shareholder-employees pay FICA through payroll only on wages actually paid.

### S-Corporation (Form 1120-S)

- **Taxation**: Pass-through. Income, deductions, and credits flow to shareholders via Schedule K-1. No entity-level federal tax (some states impose entity-level tax).
- **Shareholders**: Maximum 100, must be U.S. individuals, certain trusts, or estates. No corporations, partnerships, or nonresident aliens.
- **Classes of stock**: Single class only (differences in voting rights are permitted).
- **Losses**: Pass through to shareholders, limited by: (1) stock basis, (2) debt basis (direct loans from shareholder to S-corp only), (3) at-risk rules, (4) passive activity rules. Losses in excess of basis are suspended and carry forward.
- **Reasonable compensation**: Officer-shareholders who perform services MUST receive reasonable W-2 compensation. The IRS scrutinizes low salaries because distributions above reasonable compensation avoid payroll tax. See `tx/officer-compensation`.
- **QBI deduction**: S-corp income qualifies for the Section 199A deduction (20% of qualified business income, subject to W-2 wage and UBIA limitations for higher-income taxpayers). Available 2018-2025.
- **Fringe benefits**: >2% shareholder-employees cannot exclude employer-paid health insurance, group-term life, etc. These amounts are included in W-2 income (though deductible on the shareholder's Form 1040).

### LLC Taxed as Partnership (Form 1065)

- **Taxation**: Pass-through via Schedule K-1. No entity-level federal tax.
- **Members**: Unlimited, any type. Maximum flexibility.
- **Allocation flexibility**: Can allocate income, deductions, and credits disproportionately to ownership percentages if allocations have "substantial economic effect" (Treas. Reg. 1.704-1(b)).
- **Self-employment tax**: Active members' distributive share is generally subject to self-employment tax (15.3% up to SS wage base, 2.9% above). Limited partners may be exempt from SE tax on distributive share, but the definition of "limited partner" for SE tax purposes is contested.
- **QBI deduction**: Available for qualifying business income (2018-2025), subject to same limitations as S-corps.
- **Losses**: Pass through, limited by: (1) outside basis, (2) at-risk rules, (3) passive activity rules. Basis includes the member's share of partnership liabilities (recourse and nonrecourse), which is more generous than S-corp basis rules.
- **Guaranteed payments**: Payments to members for services or capital use. Deductible by the partnership, ordinary income to the member, subject to SE tax.

### LLC — Disregarded Entity (Schedule C or Form 1120 depending on owner)

- **Single-member LLC**: Disregarded for federal tax purposes. If owned by an individual, reported on Schedule C (sole proprietorship). If owned by a corporation, reported on the corporation's return.
- **Liability protection**: State law provides limited liability, even though the entity is disregarded for tax purposes.
- **SE tax**: If individual-owned, all net income is subject to SE tax (same as sole proprietorship).

### Sole Proprietorship (Schedule C)

- **Taxation**: All income reported on owner's Form 1040, Schedule C. Subject to SE tax (15.3% up to SS wage base, 2.9% above).
- **No liability protection**: Owner is personally liable for all business debts and obligations.
- **Simplest structure**: No formation filing, no separate return. Lowest compliance cost.
- **QBI deduction**: Available (2018-2025).

## Key Decision Factors

### Tax Rate Differential

- C-corp flat 21% vs individual top rate 37% (2018-2025). The 16-percentage-point spread favors C-corp for retained earnings.
- But distribution triggers additional tax: 21% + 20% qualified dividend + 3.8% NIIT = effective 38.8% combined rate on distributed C-corp earnings vs 37% + 3.8% SE/NIIT = up to 40.8% on pass-through (but pass-through has QBI deduction reducing the effective rate).
- **Breakeven analysis**: If the business retains most earnings, C-corp wins. If the business distributes most earnings, pass-through often wins (especially with QBI deduction).

### Reasonable Compensation (S-Corp)

S-corp payroll tax savings are real but create audit risk. The IRS has dedicated resources to challenging unreasonably low S-corp officer compensation. The savings diminish as compensation increases toward market rate.

### Exit Strategy

- **C-corp sale of stock**: Shareholders pay capital gains tax (0%/15%/20% + 3.8% NIIT). Buyer gets no step-up in asset basis.
- **C-corp sale of assets**: Corporation pays 21% on gain, then liquidating distribution is taxed again to shareholders. Double tax on exit.
- **S-corp / partnership**: Pass-through of gain to owners at capital gains rates. Section 338(h)(10) or Section 754 elections can give buyer a step-up. More tax-efficient exit.
- **QSBS exclusion (IRC 1202)**: C-corp stock held 5+ years may qualify for 100% exclusion of gain (up to $10M or 10x basis). Powerful incentive for C-corp startups.

### Investor Requirements

- Venture capital and institutional investors typically require C-corp structure (preferred stock, unlimited shareholders, no S-corp restrictions)
- Angel investors may accept S-corp or LLC but often prefer C-corp for QSBS eligibility

## Formation Procedures

### Florida-Specific

- **Corporation**: File Articles of Incorporation with FL Division of Corporations (sunbiz.org). Filing fee: $35 (online) + $35 registered agent designation. Annual report: $150, due May 1.
- **LLC**: File Articles of Organization with FL Division of Corporations. Filing fee: $100 (online) + $25 registered agent designation. Annual report: $138.75, due May 1.
- **Late annual report**: $400 late fee. Failure to file results in administrative dissolution.

### Federal Steps (All Entity Types)

1. **EIN application**: Form SS-4, filed online at irs.gov (immediate issuance) or by fax/mail
2. **Organizational documents**: Bylaws (corporation) or Operating Agreement (LLC). Not filed with the state but critical for governance and tax elections.
3. **Initial organizational meeting**: Board resolution adopting bylaws, appointing officers, authorizing bank accounts, setting fiscal year
4. **Bank account**: Open with EIN and organizational documents
5. **State registrations**: Sales tax registration (if applicable), reemployment tax (if employees), professional licenses

### S-Election — Form 2553

- **Deadline**: Within 75 days of formation, OR by March 15 (for calendar-year corps) to be effective for the current tax year
- **Who signs**: All shareholders must consent
- **Effective date**: Beginning of the tax year if filed timely; beginning of the next tax year if filed late
- **Eligibility check**: Verify all S-corp requirements are met (100 shareholders, US individuals/trusts only, single class of stock, calendar or permitted fiscal year)

### Late S-Election Relief — Rev. Proc. 2013-30

If the Form 2553 deadline was missed, relief is available if:
- The entity intended to be classified as S-corp from the effective date
- The entity has been filing as if S-corp (or has not yet filed)
- Filed within 3 years and 75 days of the intended effective date
- All shareholders have reported income consistently with S-corp treatment
- File Form 2553 with "FILED PURSUANT TO REV. PROC. 2013-30" at the top

## State Tax Considerations

### Florida Specifics

- **No personal income tax**: Pass-through income is not taxed at the state level in Florida. This reduces the advantage of pass-through entities compared to states with high personal income tax rates.
- **Corporate income tax**: 5.5% on C-corp taxable income (FL nexus-apportioned federal taxable income, with FL adjustments). $50,000 exemption.
- **S-corps in Florida**: S-corp income passes through and avoids FL corporate income tax entirely (Florida does not impose an entity-level tax on S-corps, unlike some states).
- **LLC/partnership in Florida**: No entity-level FL tax. Members report on their own returns (if FL residents, no FL income tax; if residents of other states, they report FL-source income on their state return).
- **Impact on entity choice**: The lack of FL personal income tax narrows the pass-through advantage. The 5.5% FL corporate tax on C-corps is modest but adds to the federal 21% rate.

## Decision Points

1. **Will the business retain significant earnings?** C-corp at 21% is advantageous for reinvestment. But monitor AET threshold.
2. **Will the business distribute most income?** Pass-through (S-corp or LLC) avoids double taxation. QBI deduction enhances the advantage through 2025.
3. **How many owners, and what type?** More than 100, or foreign/entity shareholders, eliminates S-corp as an option.
4. **Is reasonable compensation a concern?** S-corp savings from low salaries are real but create audit risk. If compensation would be high regardless, the S-corp payroll tax benefit is marginal.
5. **Is exit via stock sale likely?** Evaluate QSBS exclusion (C-corp only) vs pass-through capital gains treatment.
6. **Are investor rounds anticipated?** VC-backed companies almost always need C-corp structure.
7. **Multiple classes of equity needed?** Preferred stock, profit interests, or waterfall allocations require C-corp or LLC. S-corp cannot accommodate.

## Accounting System Notes

- Entity type determines the Chart of Accounts template: C-corp uses equity accounts for Retained Earnings, Common Stock, APIC; S-corp uses Shareholder Equity and AAA tracking; LLC uses Member Capital and Distributions accounts.
- During company setup, select the correct entity type (Corporation, S-Corp, Partnership, Sole Proprietor). This affects tax mapping and default account categories.
- S-corp shareholder basis tracking is NOT handled natively in most accounting systems. Maintain a separate basis schedule outside the accounting system.
- Officer compensation must be processed through payroll (W-2) for S-corps and C-corps with active officer-shareholders.

For platform-specific setup, delegate to `qbo-integration:qbo-coa`.

## Effective Date Log

| Date | Change | Authority |
|------|--------|-----------|
| 1986 | Tax Reform Act: C-corp top rate 34%, individual top rate 28%. S-corp became less attractive. | P.L. 99-514 |
| 1993 | C-corp top rate 35%, individual top rate 39.6%. Pass-through advantage grew. | OBRA 1993 |
| 1997 | Check-the-box regulations: LLCs can elect entity classification. | Treas. Reg. 301.7701-3 |
| 2004 | American Jobs Creation Act: S-corp shareholder limit increased from 75 to 100. | P.L. 108-357 |
| 2013 | Individual top rate restored to 39.6%. 3.8% NIIT enacted (ACA). C-corp at 35% vs individual at 43.4% made C-corp more competitive for retained earnings. | ATRA / ACA |
| 2018 (1/1) | **TCJA**: C-corp rate drops to flat 21%. Individual top rate drops to 37%. QBI deduction (Section 199A) enacted at 20% for pass-through income. Massive shift in entity selection calculus. | P.L. 115-97 |
| 2018 (1/1) | QSBS (IRC 1202) 100% exclusion confirmed for C-corp stock acquired after 9/27/2010 and held 5+ years. | TCJA (no change, but increased C-corp formation for QSBS) |
| 2026 (1/1) | **QBI deduction (Section 199A) scheduled to sunset**. Individual top rate reverts to 39.6%. Pass-through entities lose 20% deduction, significantly changing the entity selection analysis. | TCJA sunset (absent new legislation) |

---

**Authoritative References**:
- IRC 11 — Tax imposed on corporations (21% flat rate)
- IRC 1361-1379 — S corporation provisions
- IRC 199A — Qualified business income deduction (2018-2025)
- IRC 1202 — Qualified small business stock exclusion
- IRC 531-537 — Accumulated earnings tax
- IRC 1402 — Self-employment tax
- Treas. Reg. 301.7701-1 through 301.7701-3 — Entity classification (check-the-box)
- Rev. Proc. 2013-30 — Late S-election relief
- Form 2553 Instructions — Election by a Small Business Corporation
- Form SS-4 Instructions — Application for Employer Identification Number
- Florida Statute 220.02-220.83 — Florida Corporate Income Tax
- Florida Statute 605 — Florida Revised Limited Liability Company Act
- Florida Statute 607 — Florida Business Corporation Act
- FL Division of Corporations (sunbiz.org) — Filing requirements and fees
