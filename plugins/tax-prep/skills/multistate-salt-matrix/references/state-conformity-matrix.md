---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: multistate
---

# State Conformity Matrix: Federal Taxable Income Conformity, Modifications, and Combined Reporting

**Purpose**: Cross-state comparison of federal conformity methods, state-specific modification patterns, combined/separate reporting requirements, unitary business principle, and state tax credit inventory.

**Skills**: multistate-salt-matrix

---

## Federal Taxable Income Conformity

Nearly all states with a corporate income tax use federal taxable income as the starting point for computing state taxable income. The critical question is **which version** of the IRC the state conforms to.

### Rolling Conformity

The state adopts the IRC "as currently in effect" — automatically incorporating all federal tax law changes as they are enacted.

**Advantages**: Simplicity. No state legislative action needed when federal law changes. One set of federal rules applies for both federal and state purposes.

**Risks**: Unplanned revenue impact. A federal tax cut (e.g., TCJA bonus depreciation, CARES Act NOL carryback) automatically reduces state tax revenue unless the state enacts a decoupling provision.

**States**: The majority of states use rolling conformity, though some states that nominally use rolling conformity selectively decouple from specific federal provisions.

### Static (Fixed-Date) Conformity

The state adopts the IRC as of a specific date (e.g., "the IRC as in effect on January 1, 2022"). Federal changes enacted after the conformity date are not incorporated until the state legislature updates the date.

**Advantages**: Revenue predictability. The state controls when federal changes flow through.

**Risks**: Complexity. Taxpayers must compute state taxable income under a historical version of the IRC that differs from the current federal return. This creates permanent and timing differences between the federal and state computation.

**States**: A meaningful minority of states use static conformity. The fixed date is updated periodically (annually or every few years) by the state legislature.

### Selective Conformity

The state adopts certain IRC provisions but not others, on a provision-by-provision basis. The state statute may list specific IRC sections that are adopted and exclude or modify others.

**Advantages**: Maximum state control over tax base.

**Risks**: Highest complexity. Taxpayers must track which IRC provisions apply for state purposes and which do not.

**States**: A few states use selective conformity. This approach is less common but can be found in states that wish to adopt the IRC framework while blocking specific provisions.

---

## State-Specific Modification Patterns

Regardless of the conformity method, most states require taxpayers to make **additions** (increasing state taxable income above the federal starting point) and **subtractions** (decreasing state taxable income). These modifications create the key differences between federal and state tax computations.

### Section 163(j) Business Interest Limitation

**Federal rule**: Business interest expense is limited to 30% of adjusted taxable income (ATI). ATI was computed without regard to depreciation and amortization for 2018-2021, but includes depreciation/amortization for 2022+. Disallowed interest carries forward indefinitely.

**State response**:
- **Conform** — states following rolling conformity generally conform to the federal limitation, including the ATI computation method
- **Decouple (addition required)** — some states do not allow the federal interest deduction and require an addback of all or a portion of the deduction taken on the federal return
- **Decouple (subtraction allowed)** — some states allow interest expense that was disallowed federally
- **Modified conformity** — some states apply their own percentage threshold or ATI computation, different from 30%

The state treatment creates a permanent or timing difference depending on whether the state allows carryforward of disallowed interest.

### GILTI (Global Intangible Low-Taxed Income)

**Federal rule**: Under IRC 951A, U.S. shareholders of CFCs include GILTI in income. IRC 250 allows a 50% deduction (37.5% for tax years after 2025), resulting in an effective federal rate lower than the full corporate rate.

**State response**:
- **Full inclusion, no Section 250 deduction** — state includes 100% of GILTI in the tax base (some states treat GILTI as a dividend eligible for a dividends-received deduction)
- **Full inclusion with Section 250 deduction** — state conforms to both Section 951A and Section 250
- **Partial inclusion** — state includes GILTI but allows a modified deduction
- **Full exclusion** — state subtracts all GILTI from the tax base (treating it as foreign income not subject to state tax)
- **Combined reporting interaction** — in combined reporting states, the treatment of CFC income (including GILTI) within the combined group is particularly complex

### Section 174 R&E Amortization

**Federal rule**: For tax years beginning after December 31, 2021, specified research and experimental expenditures must be capitalized and amortized over 5 years (15 years for foreign research). Prior to 2022, taxpayers could elect to expense R&E costs currently under Section 174(a).

**State response**:
- **Conform to amortization** — states with rolling conformity automatically require amortization, matching the federal treatment
- **Decouple — allow current deduction** — some states enacted legislation allowing taxpayers to continue deducting R&E costs currently for state purposes, even though the federal return amortizes them. This creates a subtraction modification in the first year and addition modifications in subsequent years
- **Static conformity states** — states with a pre-2022 conformity date may automatically allow current deduction because their IRC version predates the amortization requirement

This is one of the most significant current divergences between federal and state treatment, creating substantial state-by-state tracking requirements.

### Bonus Depreciation (IRC 168(k))

**Federal rule**: 100% bonus depreciation was available for qualified property acquired and placed in service after September 27, 2017 through December 31, 2022. Phase-down: 80% for 2023, 60% for 2024, 40% for 2025, 20% for 2026, 0% for 2027+.

**State response**:
- **Full conformity** — state allows the same bonus depreciation percentage as federal
- **Partial conformity** — state caps bonus depreciation at a lower percentage (e.g., state allows 30% when federal allows 100%)
- **Full decoupling** — state does not allow any bonus depreciation; taxpayer must add back the federal bonus amount and compute state depreciation using a standard recovery method
- **Delayed conformity** — static conformity states may allow bonus depreciation at the percentage in effect as of their conformity date, not the current federal percentage

When a state decouples from bonus depreciation, the taxpayer gets a larger depreciation deduction in later years (the state depreciation basis remains the same; only the timing changes). This creates a timing difference tracked on the state modified accelerated cost recovery schedule.

### State Net Operating Loss (NOL) Rules

**Federal rule (post-TCJA)**: NOLs arising in 2021+ cannot be carried back (exception: farming losses); carryforward is indefinite but limited to 80% of taxable income in the carryforward year.

**State response — carryback**:
- Most states conform to the no-carryback rule for post-TCJA losses
- Some states allowed carryback for CARES Act years (2018-2020) and some did not
- A few states maintain their own carryback rules independent of federal

**State response — carryforward period**:
- Many states conform to the indefinite carryforward
- Some states impose a fixed carryforward period (15, 20, or other number of years)
- States that impose a fixed period may have different periods for different loss years

**State response — utilization limitation**:
- Many states conform to the 80% limitation
- Some states impose a lower percentage limitation or a dollar cap on NOL utilization
- Some states suspended or limited NOL utilization during budget shortfalls (temporary provisions)

**State NOL computation**: The state NOL is computed using state taxable income (after state modifications and apportionment), not the federal NOL amount. Changes in apportionment percentages from year to year can affect the state NOL carryforward schedule.

---

## Combined and Separate Reporting

### Separate Return Filing (Default Under UDITPA)

Under the default rule, each legal entity files its own return in each state where it has nexus. Each entity computes its own apportionment factors and applies them to its own federal taxable income (after state modifications).

**Advantages**: Simplicity. Each entity stands alone.

**Disadvantage**: Allows income shifting between related entities (e.g., royalty payments from a high-tax state operating company to a low-tax state holding company).

### Combined Reporting (Mandatory in Some States)

Combined reporting requires all members of a **unitary business** to file a combined return in the state, computing a single combined apportionment factor and reporting combined income.

**Mechanics**:
1. Identify all entities in the unitary group
2. Compute combined federal taxable income (after eliminating intercompany transactions)
3. Apply state modifications to the combined income
4. Compute combined apportionment factors (numerator = all entities' in-state factors; denominator = all entities' everywhere factors)
5. Apply the combined apportionment percentage to combined income
6. Only the entities with nexus in the state are subject to tax, but the combined factor captures all members' activity

**Intercompany eliminations**: In combined reporting, transactions between group members are eliminated — royalties, interest, management fees, and intercompany sales wash out. This is the primary anti-avoidance effect of combined reporting.

### Water's Edge Combined Reporting

Water's edge limits the combined group to domestic entities (and certain foreign entities with substantial U.S. activity). Foreign affiliates that operate entirely outside the United States are excluded.

**Typical water's edge inclusion criteria**:
- All domestic corporations in the unitary group
- Foreign corporations incorporated in tax haven jurisdictions (as defined by the state)
- Foreign corporations with a specified percentage of property, payroll, or sales in the United States
- 80/20 companies (domestic corporations deriving 80%+ of income from foreign sources) may be excluded

### Worldwide Combined Reporting

Worldwide combined reporting includes all entities in the unitary group worldwide, including purely foreign entities. Very few states require worldwide combined. Most states that mandate combined reporting use water's edge as the default, with worldwide as an option or as an override in avoidance situations.

### Unitary Business Test

Determining which entities are part of the "unitary business" is the threshold question for combined reporting.

**Three Unities Test (Butler Bros. / Container Corp.)**:
1. **Unity of ownership** — common ownership, typically greater than 50% direct or indirect voting control (some states use an 80% threshold matching the federal affiliated group definition)
2. **Unity of operation** — shared centralized services such as purchasing, advertising, accounting, HR, legal
3. **Unity of use** — centralized management, executive force, and general system of operations (shared strategic direction)

**Contribution/Dependency Test (Edison California Stores)**:
- The operations of the entities contribute to or are dependent upon each other
- Examined by looking at flows of goods, services, capital, and management between entities

**Practical application**: If entities share management, back-office functions, branding, or capital allocation, they are likely unitary. If they are independently operated with no functional integration, they may not be unitary even under common ownership.

**Holding companies**: A holding company that actively manages subsidiaries (provides capital, sets strategy, provides shared services) is typically part of the unitary group. A passive holding company that merely holds stock may be excluded.

---

## State Tax Credit Inventory

States offer tax credits to incentivize economic activity. Credits reduce state tax liability dollar-for-dollar (unlike deductions, which reduce taxable income). Key categories:

### R&D Tax Credits (State Piggyback)

Many states offer an R&D credit that piggybacks on the federal credit structure (IRC 41):
- **Calculation basis** — typically a percentage of qualified research expenses (QREs) as defined for the federal credit
- **Credit rate** — varies by state, often lower than the federal rate
- **Conformity to federal QRE definition** — most states adopt the federal definition, but some modify it (e.g., excluding contract research or limiting to in-state research)
- **Refundable vs. nonrefundable** — some states offer refundable R&D credits (particularly for small businesses); most are nonrefundable with carryforward
- **Carryforward period** — ranges from 5 years to indefinite, depending on the state
- **Apportionment of credit** — states typically limit the credit to QREs performed in-state or apportioned to the state

### Investment Tax Credits

States offer credits for capital investment in the state:
- **Qualified property** — manufacturing equipment, real property construction/renovation, technology infrastructure
- **Credit rate** — typically a percentage of the qualified investment, often with a dollar cap or project cap
- **Clawback provisions** — if the investment is removed from the state or employment levels are not maintained, the credit may be recaptured
- **Carryforward** — generally available; periods vary by state

### Hiring / Employment Credits

Credits for creating new jobs in the state:
- **Qualification criteria** — net new full-time positions above a base level, often with minimum wage or benefit requirements
- **Credit amount** — per-employee dollar amount or percentage of wages, often tiered by region (higher in distressed areas)
- **Duration** — some credits are available for each year the position is maintained, over a multi-year period
- **Clawback** — if employment levels drop below the base, previously claimed credits may be recaptured

### Enterprise Zone / Opportunity Zone Credits

Credits for investment or employment in designated geographic zones:
- **Enterprise zones** — state-designated distressed areas with enhanced credits and exemptions
- **Federal Opportunity Zones** — some states piggyback on federal OZ incentives; others decouple
- **Types of incentives** — property tax abatement, sales tax exemption, income tax credit, enhanced depreciation
- **Certification requirements** — typically requires application and approval from the state economic development agency before investment

### Film / Digital Media Credits

Some states offer significant credits for film, television, and digital media production:
- **Credit rate** — percentage of qualified production expenditures incurred in-state
- **Transferability** — some states allow credits to be sold or transferred to other taxpayers
- **Cap** — many states impose an annual or per-project cap on film credits
- **Audit requirements** — states typically require a CPA audit of qualified expenditures

### Historic Preservation Credits

Credits for rehabilitating certified historic structures:
- Often piggyback on the federal historic rehabilitation credit (IRC 47)
- Additional state percentage on top of the federal credit
- Certified rehabilitation must meet state historic preservation office standards

### Clean Energy / Environmental Credits

Emerging and growing category:
- Credits for renewable energy installation (solar, wind, geothermal)
- Energy efficiency improvement credits
- Electric vehicle credits (manufacturing or purchase)
- Some states offer carbon reduction credits or cap-and-trade allowances

### Credit Utilization Planning

- **Stacking rules** — states may specify the order in which credits are applied (e.g., nonrefundable before refundable, or specific credits before general credits)
- **Limitation on tax** — many nonrefundable credits are limited to a percentage of the tax liability (e.g., credit cannot reduce tax below 50% of the liability before credits)
- **Carryforward tracking** — each credit has its own carryforward schedule; expiring credits should be used first
- **Apportionment interaction** — credits are generally applied after apportionment of income; the credit itself is typically based on in-state activity

---

## State Modification Tracking Across Multiple States

### Computation Flow (Per State)

1. Start with **federal taxable income** (Line 30, Form 1120, before NOL and special deductions — same starting point for all states)
2. Apply **state additions** — items deducted federally but not allowed by the state (e.g., bonus depreciation addback, state tax deduction addback, GILTI exclusion reversal)
3. Apply **state subtractions** — items included federally but excluded by the state (e.g., GILTI exclusion, Section 174 current deduction if the state decouples from amortization, U.S. government interest income)
4. Result is **state modified income before apportionment**
5. Apply **apportionment formula** to arrive at state apportioned income
6. Apply **state NOL deduction** (computed using state rules, not the federal NOL)
7. Apply **state exemption** (if any)
8. Compute **state tax** at the state rate
9. Apply **state tax credits** to reduce the liability
10. Net result: **state tax due** (or refund/overpayment)

### Multistate Modification Matrix

For a corporation filing in multiple states, a modification matrix tracks which adjustments apply in each state. The matrix has:
- Rows: each federal line item requiring state modification (163(j) interest, GILTI, bonus depreciation, Section 174, NOL, state tax deduction, etc.)
- Columns: each filing state
- Cells: addition, subtraction, no modification, or partial modification — with the specific state rule

This matrix is the operational artifact for multistate compliance — it ensures every state's unique treatment is captured and no modification is missed.
