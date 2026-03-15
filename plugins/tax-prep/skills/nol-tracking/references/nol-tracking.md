# Net Operating Loss (NOL) Tracking: C-Corporation Rules

**Purpose**: NOL computation, carryback/carryforward rules, utilization limitations, and year-by-year tracking for C-corporation Form 1120 filers.

**Skills**: —

---

## NOL Fundamentals

A **Net Operating Loss** occurs when a corporation's allowable tax deductions exceed its gross income for the tax year. For a C-corporation, the NOL is computed on Form 1120 — it is the negative amount on Line 30 (taxable income before NOL deduction and special deductions, after all other deductions).

### Computing the NOL

1. Start with gross income (Line 11)
2. Subtract all deductions (Lines 12-27) — compensation, rents, taxes, depreciation, etc.
3. The result is taxable income before NOL deduction (Line 28 minus Line 29a/29b adjustments)
4. If negative, the excess is the NOL for the year

### What Cannot Create or Increase an NOL

- **NOL deduction from other years** — Cannot use a prior NOL carryforward to increase the current year's NOL
- **Dividends-received deduction** — May be limited in NOL computation (for non-corporate taxpayers; less relevant for C-corps where DRD is unrestricted in computing NOL)
- **Capital loss in excess of capital gains** — C-corps cannot deduct net capital losses; they carry back 3 years / forward 5 years separately from NOL

## Carryback and Carryforward Rules

### Pre-TCJA (Tax Years Beginning Before January 1, 2018)

- **Carryback**: 2 years (can elect to waive carryback and carry forward only)
- **Carryforward**: 20 years
- **Utilization limit**: 100% of taxable income — NOLs could fully offset income
- **Ordering**: Apply to earliest available year first (FIFO)

### Post-TCJA (Tax Years Beginning After December 31, 2017)

- **Carryback**: **None** (carryback eliminated for most taxpayers)
- **Carryforward**: **Indefinite** (no 20-year expiration)
- **Utilization limit**: **80% of taxable income** (computed without regard to the NOL deduction itself) — the remaining 20% is always taxable
- **Ordering**: Same FIFO ordering. Pre-2018 NOLs applied first (at 100%), then post-2017 NOLs (at 80%)

### CARES Act Exception (Temporary — 2018, 2019, 2020 NOLs)

The CARES Act (March 2020) temporarily restored carryback for NOLs arising in tax years beginning in 2018, 2019, and 2020:

- **5-year carryback** allowed for NOLs arising in these three years
- **100% utilization** (the 80% limit was suspended for tax years beginning before January 1, 2021)
- Taxpayers could elect to waive the carryback
- **Form 1139** (Corporation Application for Tentative Refund) could be used for quick refunds within 12 months of the NOL year-end

**Important**: The CARES Act NOL carryback applied to the NOL *arising* in 2018-2020, not to NOLs *used* in those years. A pre-existing NOL carried into 2020 did not get a new carryback right.

### Farming Loss Exception

Farming losses retain a 2-year carryback right regardless of TCJA changes. A farming loss is the lesser of: (a) the NOL for the year, or (b) the net loss from farming businesses. Applicable only to corporations with farming operations.

## Utilization Ordering Rules

When a corporation has NOLs from multiple years:

1. **Apply oldest NOL first** (FIFO) — the 2-year-carryback / 20-year-forward NOL from an earlier year is used before a newer one
2. **Pre-2018 NOLs offset 100% of taxable income** — no 80% limitation
3. **Post-2017 NOLs offset only 80% of remaining taxable income** (after pre-2018 NOLs are applied)
4. **Ordering within the 80% bucket**: Oldest post-2017 NOL first

### Example

Corporation has:
- 2016 NOL: $50,000 (pre-TCJA, unlimited offset, 20-year carryforward expiring 2036)
- 2019 NOL: $100,000 (post-TCJA, 80% limit, indefinite carryforward)

2025 taxable income (before NOL deduction): $200,000

Step 1: Apply 2016 NOL at 100% — Deduct $50,000. Remaining income: $150,000
Step 2: Apply 2019 NOL at 80% of remaining — 80% x $150,000 = $120,000 max. Deduct $100,000 (full 2019 NOL). Remaining income: $50,000
Result: Taxable income after NOL = $50,000. All NOLs consumed.

If the 2019 NOL were $200,000 instead, only $120,000 would be used (80% of $150,000), and $80,000 would carry forward.

## Refund Procedures for NOL Carrybacks

### Form 1139 — Quick Refund (Tentative)

- Must be filed within 12 months after the close of the NOL year
- IRS has 90 days to process
- No formal examination; refund is tentative (IRS can later audit)
- Available for carrybacks only (pre-TCJA NOLs or CARES Act NOLs)

### Amended Return — Form 1120X

- Must be filed within the refund statute of limitations (later of 3 years from filing or 2 years from payment)
- Longer processing time than Form 1139
- Used when Form 1139 deadline has passed or when additional adjustments are needed

## State NOL Conformity Issues

### Florida

- Florida maintains a **separate state NOL** computed on apportioned Florida taxable income
- Florida adopted the TCJA 80% limitation for post-2017 NOLs
- Florida did **NOT** adopt the CARES Act 5-year carryback
- Pre-2018 Florida NOLs: 20-year carryforward
- Post-2017 Florida NOLs: Indefinite carryforward (conforming to federal)
- Florida NOLs are tracked independently from federal NOLs — a federal NOL does not automatically create a Florida NOL if the corporation had positive Florida taxable income

### Other States

State conformity varies widely. Common patterns:
- Some states conform to federal NOL rules entirely (piggyback states)
- Some states have their own carryback/forward periods
- Some states cap NOL utilization at their own percentage
- Very few states adopted the CARES Act carryback provisions

## Year-by-Year Variance Table: C-Corporation NOL Rules

| NOL Arising In | Carryback | Carryforward | Utilization Limit | Special Rules |
|----------------|-----------|--------------|-------------------|---------------|
| 2007 | 2 years | 20 years | 100% | Standard pre-TCJA |
| 2008 | 2 years (or elect 3-5 per ARRA for small biz) | 20 years | 100% | Worker, Homeownership, and Business Act extended carryback to 5 years for small businesses (revoked for most corps) |
| 2009 | 2 years (5 years for eligible small biz) | 20 years | 100% | ARRA extended carryback option |
| 2010-2017 | 2 years | 20 years | 100% | Standard pre-TCJA |
| 2018 | **5 years (CARES Act)** | **Indefinite** | **100% (CARES suspended 80%)** | CARES Act retroactively allowed carryback; 80% limit suspended through 2020 |
| 2019 | **5 years (CARES Act)** | **Indefinite** | **100% (CARES suspended 80%)** | Same CARES Act treatment |
| 2020 | **5 years (CARES Act)** | **Indefinite** | **100% (CARES suspended 80%)** | Last year of CARES Act carryback |
| 2021 | **None** | **Indefinite** | **80%** | TCJA rules fully apply; CARES Act expired |
| 2022 | None | Indefinite | 80% | Standard post-TCJA |
| 2023 | None | Indefinite | 80% | Standard post-TCJA |
| 2024 | None | Indefinite | 80% | Standard post-TCJA |
| 2025 | None | Indefinite | 80% | Standard post-TCJA |
| 2026+ | None | Indefinite | 80% | Unless Congress changes the rules |

### Key Transition: Using NOLs in a Given Tax Year

The utilization limit depends on when the NOL is **used**, not when it arose (except for the CARES Act suspension):

| Tax Year NOL Is Used In | Pre-2018 NOLs | Post-2017 NOLs |
|--------------------------|---------------|----------------|
| 2018-2020 | 100% offset | 100% offset (CARES Act suspended 80% limit) |
| 2021+ | 100% offset | 80% of taxable income (after pre-2018 NOLs) |

## NOL Tracking (C-Corporation Example)

### What to Track Per Year

- **Year the NOL arose**
- **Original NOL amount**
- **Carryback/carryforward regime** (pre-TCJA vs post-TCJA vs CARES Act)
- **Utilization each year** — Amount used, year applied to, remaining balance
- **Expiration date** — 20 years for pre-2018 NOLs; indefinite for post-2017 NOLs
- **Florida NOL** — Separate computation and tracking

### Reconstruction Context

When reconstructing delinquent returns for multiple years:
1. Compute each year's taxable income/loss independently
2. Apply NOL carryforwards in FIFO order
3. For years where carryback was available but the return was never filed — the carryback right may be preserved if the return is filed within the refund statute (but refund statute may have expired for very old years)
4. Document the NOL schedule on a year-by-year waterfall showing: beginning balance, current year NOL (if any), utilization, and ending balance

---

**Authoritative References**:
- IRC 172 — Net Operating Loss deduction
- IRC 172(b)(1)(A) — Carryback and carryforward periods
- IRC 172(a), as amended by TCJA Section 13302 — 80% limitation
- CARES Act Section 2303 — Temporary 5-year carryback for 2018-2020 NOLs
- IRC 172(b)(1)(B) — Farming loss carryback exception
- Form 1139 — Corporation Application for Tentative Refund
- Form 1120, Line 29a — NOL deduction
- IRS Publication 542 — Corporations
- Florida Statute 220.13(1)(b) — Florida NOL rules
