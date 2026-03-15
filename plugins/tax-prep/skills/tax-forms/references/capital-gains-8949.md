# Form 8949: Sales and Other Dispositions of Capital Assets (Corporate)

**Purpose**: Form 8949 reporting categories, box selection, basis adjustment codes, wash sale rules, and cost basis methods for C-corporation capital gain/loss transactions.

**Skills**: —

---

## Form 8949 Structure

### Part I — Short-Term Capital Gains and Losses (Held 1 Year or Less)

- **Box A**: Transactions reported on Form 1099-B showing basis **was reported to the IRS**. No adjustments needed if basis is correct.
- **Box B**: Transactions reported on Form 1099-B showing basis **was NOT reported to the IRS**. Taxpayer must provide cost basis.
- **Box C**: Transactions **not reported on any Form 1099-B** (private sales, crypto pre-2024, etc.).

### Part II — Long-Term Capital Gains and Losses (Held More Than 1 Year)

- **Box D**: 1099-B with basis reported to IRS (long-term counterpart of Box A).
- **Box E**: 1099-B without basis reported (long-term counterpart of Box B).
- **Box F**: No 1099-B received (long-term counterpart of Box C).

### Columns

(a) Description of property, (b) Date acquired, (c) Date sold, (d) Proceeds, (e) Cost basis, (f) Adjustment code(s), (g) Adjustment amount, (h) Gain or loss = (d) - (e) +/- (g).

## Basis Adjustment Codes (Column f)

| Code | Meaning | When to Use |
|------|---------|-------------|
| **W** | Wash sale loss disallowed | Substantially identical security repurchased within 30 days before/after sale |
| **B** | Basis reported to IRS is incorrect | 1099-B cost basis does not match taxpayer records (e.g., missing reinvested dividends, corporate actions) |
| **T** | Short/long-term reported incorrectly | 1099-B holding period classification is wrong |
| **O** | Other | Adjustments not covered by other codes (e.g., Section 1202 exclusion, worthless securities) |
| **S** | Proceeds reported as gross, not net | 1099-B Box 1e shows sale expense separately; adjust proceeds to net |
| **N** | Noncovered security | Security acquired before broker was required to track basis |
| **H** | Short sale — holding period related | Adjustment for short sales affecting holding period |

Multiple codes can be combined on a single line (e.g., "BW" for incorrect basis AND wash sale).

## Corporate-Specific Rules

### No Preferential Capital Gains Rate

C-corporations do **not** receive a preferential tax rate on long-term capital gains. All capital gains are taxed at the same rate as ordinary income:
- **Pre-2018**: Graduated rates (15%/25%/34%/35%)
- **Post-2017**: Flat 21% (TCJA)

This is fundamentally different from individuals, who pay 0%/15%/20% on long-term capital gains. For C-corps, the short-term vs. long-term distinction matters only for:
1. Netting gains against losses within each category
2. Capital loss carryback/carryforward tracking
3. Compliance reporting (Schedule D still requires the split)

### Capital Loss Limitation

- Corporations can **only deduct capital losses against capital gains** — no deduction against ordinary income
- Individuals get a $3,000 annual deduction against ordinary income; corporations get **none**
- Net capital loss is carried **back 3 years, then forward 5 years** (as short-term capital loss)
- Carried-back/forward losses retain their character but are always treated as short-term in the carryover year

## Wash Sale Rule (Section 1091)

### Application to Corporations

The wash sale rule applies equally to C-corporations. A loss is disallowed when:

1. A security is sold at a loss, AND
2. A **substantially identical** security is acquired within 30 days before or after the sale date (61-day window)

### Disallowed Loss Treatment

- The disallowed loss is added to the cost basis of the replacement security
- The holding period of the original security tacks onto the replacement
- Report the sale on Form 8949 with code **W** in column (f) and the disallowed amount in column (g) as a positive adjustment

### "Substantially Identical" — Key Rulings

- Same stock/security of same issuer — YES
- Options/contracts to acquire substantially identical stock — YES
- Different classes (common vs. preferred) of same issuer — Generally NO, unless convertible
- Related party purchases (>50% ownership) can trigger wash sale for corporations
- Corporations have no tax-deferred accounts (no IRA "reset" available)

## Cost Basis Methods

- **Specific Identification**: Designate which lots are sold at time of sale. Most flexibility but must be documented contemporaneously.
- **FIFO** (default): Oldest shares sold first. Higher gains in rising markets.
- **Average Cost**: Available only for mutual fund / DRIP shares (not individual stocks or ETFs). Irrevocable once elected for a fund.

## Decision Points

1. **Which box (A-F)?** Check the 1099-B: Does it report basis? Was one issued at all? This determines the box.
2. **Any adjustment codes needed?** Compare 1099-B basis to taxpayer records. Check for wash sales.
3. **Specific ID vs. FIFO?** If the client has multiple lots, specific identification can optimize the outcome (though for C-corps the rate is the same, loss timing still matters for carryback/carryforward).
4. **Aggregate or detail reporting?** Box A and Box D transactions can be reported in aggregate on Schedule D (skipping Form 8949) if no adjustments are needed. All others require detail on Form 8949.
5. **Net capital loss?** If total losses exceed total gains, no deduction against ordinary income — compute carryback eligibility.

## Accounting System Integration Notes

- Brokerage transactions should post to capital gains/loss income accounts segregated by holding period (short-term vs. long-term)
- Use separate income accounts: "Short-Term Capital Gains", "Long-Term Capital Gains", "Short-Term Capital Losses", "Long-Term Capital Losses"
- Year-end journal entries should reconcile to Form 1099-B totals
- Wash sale adjustments are tax-only entries — book basis tracks the original cost; tax basis tracks the adjusted cost
- Attach 1099-B PDFs to the year-end journal entries for audit trail
- Most accounting systems do not natively track individual security lots — maintain a separate lot-level schedule

For platform-specific account setup and journal entry creation, invoke the appropriate `qbo-integration` skill.

## Year-by-Year Variance Table: Corporate Capital Gains Tax Treatment

| Tax Year | Corporate Tax Rate on Capital Gains | Loss Carryback | Loss Carryforward | Key Changes |
|----------|-------------------------------------|----------------|-------------------|-------------|
| 2007 | Graduated: 15/25/34/35% (same as ordinary) | 3 years | 5 years | Standard pre-TCJA |
| 2008-2017 | Graduated: 15/25/34/35% | 3 years | 5 years | No change from 2007 |
| 2018 | **Flat 21%** | 3 years | 5 years | TCJA flat rate; no preferential CG rate for corps |
| 2019 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2020 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2021 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2022 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2023 | Flat 21% | 3 years | 5 years | 1099-B digital asset reporting requirements expanded |
| 2024 | Flat 21% | 3 years | 5 years | Brokers required to report cost basis for digital assets |
| 2025 | Flat 21% | 3 years | 5 years | Standard post-TCJA |
| 2026+ | Flat 21% | 3 years | 5 years | Unless Congress changes; individual rates sunset but corp rate stays at 21% |

**Note**: Unlike NOL rules, capital loss carryback/carryforward periods (3 years back / 5 years forward) have remained constant and were **not** changed by TCJA or CARES Act. The only variance is the tax rate applied when gains are recognized.

---

**Authoritative References**:
- IRC Section 1001 — Determination of gain or loss
- IRC Section 1091 — Wash sale rule
- IRC Section 1211(a) — Capital loss limitation for corporations
- IRC Section 1212(a) — Capital loss carryback and carryforward for corporations
- IRC Section 1222 — Short-term and long-term capital gain/loss definitions
- IRC Section 1012 — Basis of property (cost basis)
- Treasury Reg. 1.1012-1(e) — Average basis for mutual fund shares
- Form 8949 Instructions (current revision)
- IRS Publication 542 — Corporations (capital gains section)
- IRS Publication 550 — Investment Income and Expenses (wash sale rules)
