---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Schedule D (Form 1120): Capital Gains and Losses

**Purpose**: Schedule D structure, net capital gain computation, capital loss carryback/carryforward mechanics, Section 1231 interaction, and how Schedule D feeds Form 1120 Line 8.

**Skills**: —

---

## Schedule D Structure

### Part I — Short-Term Capital Gains and Losses (Assets Held 1 Year or Less)

| Line | Description |
|------|-------------|
| 1a | Totals from Form 8949 Box A (basis reported to IRS, no adjustments) |
| 1b | Totals from Form 8949 Box B (basis not reported to IRS) |
| 2 | Totals from Form 8949 Box C (no 1099-B received) |
| 3 | Short-term capital gain from installment sales (Form 6252) |
| 4 | Short-term capital gain/loss from like-kind exchanges (Form 8824) |
| 5 | Net short-term capital gain/loss from partnerships, S corporations, estates, trusts |
| 6 | Capital loss carryover from prior years (treated as short-term) |
| 7 | **Net short-term capital gain or loss** (sum of lines 1a through 6) |

### Part II — Long-Term Capital Gains and Losses (Assets Held More Than 1 Year)

| Line | Description |
|------|-------------|
| 8a | Totals from Form 8949 Box D (basis reported to IRS, no adjustments) |
| 8b | Totals from Form 8949 Box E (basis not reported to IRS) |
| 9 | Totals from Form 8949 Box F (no 1099-B received) |
| 10 | Long-term capital gain from installment sales (Form 6252) |
| 11 | Long-term capital gain/loss from like-kind exchanges (Form 8824) |
| 12 | Capital gain distributions (from 1099-DIV Box 2a) |
| 13 | Net long-term capital gain/loss from partnerships, S corporations, estates, trusts |
| 14 | **Net long-term capital gain or loss** (sum of lines 8a through 13) |

### Summary

| Line | Description |
|------|-------------|
| 15 | Net short-term capital gain or loss (from line 7) |
| 16 | Net long-term capital gain or loss (from line 14) |
| 17 | **Total**: Add lines 15 and 16 |

If line 17 is a **gain**, enter on Form 1120 Line 8 as "capital gain net income."
If line 17 is a **loss**, enter zero on Form 1120 Line 8 — the loss cannot offset ordinary income.

## Relationship to Form 8949

Form 8949 provides transaction-level detail; Schedule D summarizes. Totals from 8949 boxes A/B/C flow to Schedule D lines 1a/1b/2 (short-term) and boxes D/E/F flow to lines 8a/8b/9 (long-term). Schedule D then adds installment sales, pass-throughs, and carryovers. The net result flows to Form 1120 Line 8.

**Shortcut**: If all transactions fall in Box A or D with no adjustments, report directly on Schedule D without Form 8949.

## Net Capital Gain Computation

**Capital gain net income** = Total capital gains minus total capital losses (but not below zero). This enters Form 1120 Line 8. Since C-corporations receive no preferential capital gains rate, the short-term/long-term distinction only matters for netting and carryover tracking.

## Capital Loss Limitation

### The Core Rule (IRC Section 1211(a))

A corporation may deduct capital losses **only to the extent of capital gains**. The excess cannot reduce ordinary income — not even by $1 (individuals get a $3,000 annual deduction; corporations get none).

### Capital Loss Carryback and Carryforward (IRC Section 1212(a))

When a corporation has a **net capital loss** (losses exceed gains):

1. **Carry back 3 years** — Apply the loss against capital gains in the 3rd preceding year first, then 2nd preceding, then 1st preceding
2. **Carry forward 5 years** — Any remaining loss after carryback carries forward up to 5 years
3. **Character**: All carryback/carryforward amounts are treated as **short-term capital losses** in the destination year, regardless of their original character
4. **Expiration**: If not used within the 5-year carryforward window, the loss expires permanently

### Carryback Mechanics

- File Form 1120X for carryback years, or Form 1139 for tentative refund within 12 months
- Corporation may elect to **waive carryback** (irrevocable); statement attached to timely-filed return
- Carryback losses applied to earliest available year first (FIFO)
- Capital loss carrybacks tracked **separately** from NOLs — a net capital loss does not create/increase an NOL

## Section 1231 Gains and Losses

Section 1231 covers depreciable/real property used in business held >1 year (equipment, buildings, land, involuntary conversions).

### Recharacterization Rules

- **Net Section 1231 gain**: Treated as long-term capital gain — flows to Schedule D
- **Net Section 1231 loss**: Treated as ordinary loss — fully deductible (bypasses capital loss limitation)
- **5-year lookback** (Section 1231(c)): Net 1231 gains recharacterized as ordinary income to the extent of unrecaptured 1231 losses from the prior 5 years

### Depreciation Recapture (Applied Before 1231 Netting)

- **Section 1245** (personal property): All gain attributable to depreciation is ordinary income
- **Section 1250** (real property): Excess depreciation over straight-line is ordinary; for corporations, Section 291 recaptures an additional 20% of remaining 1250 gain

## Form 1120 Line 8: Capital Gain Net Income

Enter Schedule D line 17 if positive; enter zero if a loss (the loss carries back/forward separately). If no capital transactions, leave blank. Line 8 is part of the Income section (Lines 1-11) summing to Total Income on Line 11.

## Installment Sale Interaction (Form 6252)

- Installment sale gains flow to Schedule D lines 3 (short-term) or 10 (long-term)
- Gain character (short/long-term) determined at original sale date, not payment date
- Not available for publicly traded securities or dealer property
- Related party resale within 2 years triggers immediate gain recognition (Section 453(e))

## Decision Points

1. **Does the corporation have a net capital loss?** If yes, no deduction on Form 1120 — compute carryback eligibility (3 years) and track carryforward (5 years).
2. **Should the corporation waive carryback?** Only beneficial if the carryforward years will have higher capital gains to offset. The election is irrevocable — analyze carefully.
3. **Any Section 1231 transactions?** Separate these from pure capital asset sales. Apply depreciation recapture first, then net under Section 1231 rules before flowing to Schedule D.
4. **5-year lookback**: Check for unrecaptured Section 1231 losses from the prior 5 years that would recharacterize current-year Section 1231 gains as ordinary.
5. **Capital loss carryforward expiring?** With only a 5-year window, plan asset sales to generate offsetting capital gains before carryforward expires.

## Accounting System Integration Notes

- Schedule D line 17 (if positive) maps to Form 1120 Line 8 via "Other Income:Capital Gains" account
- Maintain separate accounts for short-term and long-term gains/losses
- Section 1231 gains recharacterizing as capital gains post to capital gains account (recharacterization is tax-level)
- Capital loss carrybacks/carryforwards tracked off-book in a supporting schedule, not in the accounting system
- Year-end: capital gain/loss totals should tie to Schedule D line 17 before carryover adjustments

For platform-specific account setup, invoke the appropriate `qbo-integration` skill.

## Year-by-Year Variance Table: Schedule D / Corporate Capital Gains

| Tax Year | Corp Tax Rate Applied to Capital Gains | Capital Loss Carryback | Capital Loss Carryforward | Schedule D Notable Changes |
|----------|----------------------------------------|------------------------|---------------------------|---------------------------|
| 2007 | Graduated: 15/25/34/35% | 3 years | 5 years | Standard rules; alternative tax computation available if net CG taxed lower at 35% cap |
| 2008-2017 | Graduated: 15/25/34/35% | 3 years | 5 years | No change; NIIT (3.8%, 2013+) does not apply to C-corps |
| 2018 | **Flat 21%** | 3 years | 5 years | TCJA flat rate eliminates graduated brackets; alternative tax on capital gains (old Section 1201) repealed — no longer needed since top rate dropped from 35% to 21% |
| 2019 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2020 | Flat 21% | 3 years | 5 years | CARES Act did NOT change capital loss rules (only NOLs) |
| 2021-2022 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2023-2024 | Flat 21% | 3 years | 5 years | Form 8949 / 1099-DA updated for digital asset reporting |
| 2025 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2026+ | Flat 21% | 3 years | 5 years | Corp rate stays 21%; individual TCJA sunset does not affect C-corp Schedule D |

### Post-TCJA Planning Focus

With the flat 21% rate, bracket arbitrage is gone. Corporate capital gains planning now centers on: (1) timing gains to use expiring carryforwards, (2) ensuring Section 1231 gains get capital vs. ordinary treatment, (3) generating capital gains to utilize capital losses (no ordinary income offset).

---

**Authoritative References**:
- IRC 1201 — Alternative tax for corporations (repealed by TCJA post-2017)
- IRC 1211(a) / 1212(a) — Capital loss limitation, carryback (3yr), carryforward (5yr)
- IRC 1221 / 1222 — Capital asset definition, short/long-term definitions
- IRC 1231, 1245, 1250, 291 — Section 1231 property, depreciation recapture, corporate 1250 recapture
- IRC 453 — Installment method
- Schedule D (Form 1120) Instructions, Form 1120 Line 8
- IRS Publication 542 (Corporations), Publication 544 (Sales and Dispositions)
