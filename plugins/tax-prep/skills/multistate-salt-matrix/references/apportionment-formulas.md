---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: multistate
---

# Apportionment Formulas: Multistate Income Tax Allocation and Apportionment

**Purpose**: Cross-state comparison of income apportionment methods — UDITPA framework, single sales factor adoption, sourcing rules, throwback/throwout, Joyce vs Finnigan, and special industry formulas.

**Skills**: multistate-salt-matrix

---

## Apportionment Fundamentals

When a corporation has nexus in multiple states, each state can tax only the portion of income attributable to that state. Apportionment is the mechanism for dividing taxable income among states with jurisdiction.

### UDITPA Three-Factor Formula (Model)

The Uniform Division of Income for Tax Purposes Act (UDITPA), adopted in some form by most states, provides the foundational three-factor formula:

**Apportionment Percentage** = (Property Factor + Payroll Factor + Sales Factor) / 3

Each factor is a fraction: the numerator is the factor attributable to the taxing state; the denominator is the total factor everywhere.

- **Property Factor** = (Average value of real and tangible personal property in-state) / (Average value of real and tangible personal property everywhere). Rented property is included at 8x the annual rental. Property is valued at original cost (not depreciated value under UDITPA).
- **Payroll Factor** = (Compensation paid to employees working in-state) / (Total compensation paid everywhere). Sourced to the state where services are performed. If multi-state, sourced to the state of base of operations, or if none, the state from which services are directed/controlled.
- **Sales Factor** = (Sales sourced to the state) / (Total sales everywhere). Sourcing rules differ by state (see below).

### Evolution Away From Three-Factor

The trend since the early 2000s has been away from equal-weighted three-factor toward sales-weighted and single sales factor formulas. The rationale: a sales-only factor removes tax incentive to relocate property and employees out of state, retaining the tax base for sales into the state.

**Current landscape** (structural, not state-by-state):
- **Single sales factor** — the majority of states now use single sales factor for general business corporations. This means apportioned income = (State sales / Total sales) x Taxable income
- **Double-weighted sales** — a declining number of states weight sales at 50-90% and split the remainder between property and payroll
- **Equal three-factor** — a small number of states retain the original UDITPA equal weighting
- **Modified formulas** — some states use other weightings or additional factors

The specific formula used by a state is set by statute and may change. Always verify the current formula for the applicable tax year.

---

## Sales Factor Sourcing Rules

The sales factor is the most consequential component of modern apportionment because the majority of states now use single sales factor or heavily weight the sales factor.

### Market-Based Sourcing

Sales of services and intangibles are sourced to the state where the **market** for the service is located — typically the state where the customer receives the benefit.

**Majority rule**: Most states have adopted market-based sourcing (either by statute or regulation). Under market-based sourcing:
- Sale of services → sourced where the benefit is received by the customer
- License of intangibles → sourced where the intangible is used
- Sale of intangibles → sourced where the intangible is used

**Benefit-received hierarchy**: When the location of benefit is ambiguous, states may use cascading rules: (1) where the service is delivered, (2) where the customer's billing address is located, (3) where the customer's commercial domicile is located, (4) reasonable approximation.

### Cost-of-Performance Sourcing

The original UDITPA rule: income from services is sourced to the state where the **income-producing activity** is performed. If performed in multiple states, all revenue is sourced to the state where the **greater proportion** of the activity occurs (all-or-nothing approach in many states).

**Declining adoption**: Cost-of-performance sourcing is being replaced by market-based sourcing in the majority of states. However, a meaningful number of states still use cost-of-performance, and some use a proportional (rather than all-or-nothing) version.

**Impact on multistate planning**: The sourcing rule choice can dramatically shift taxable income between states. A service company performing work in State A for customers in State B will have very different apportionment under each method.

### Tangible Personal Property Sourcing

For sales of tangible personal property, the sourcing rule is more uniform: sales are sourced to the state of **destination** (where the property is delivered or shipped to the purchaser). This is the UDITPA default and is followed by virtually all states.

---

## Throwback and Throwout Rules

### Throwback Rule

Under UDITPA Section 16, if a sale of tangible personal property is shipped from a state where the taxpayer has nexus to a state where the taxpayer does NOT have nexus (and therefore is not taxable), the sale is "thrown back" to the origin state and included in that state's sales factor numerator.

**Purpose**: Prevent "nowhere income" — income that escapes taxation in both the destination state (no nexus) and the origin state (not sourced there without throwback).

**State adoption**: Many states have enacted throwback rules. Some states have repealed their throwback rules as part of single sales factor adoption. A corporation must check whether each origin state applies throwback.

**Effect on planning**: Throwback rules increase the effective tax rate in the origin state for sales to customers in non-nexus states. Establishing nexus (and filing) in the destination state can actually reduce the throwback in the origin state.

### Throwout Rule

A less common alternative to throwback: instead of adding the "nowhere sales" to the origin state's numerator, the throwout rule **removes** those sales from the denominator of the sales factor in all states.

**Effect**: Increases the apportionment percentage in every state where the taxpayer files by making the denominator smaller. Unlike throwback (which benefits the origin state), throwout increases tax in all filing states proportionally.

**Adoption**: Only a small number of states apply throwout instead of (or in addition to) throwback.

---

## Joyce vs. Finnigan

These two California court decisions represent opposing approaches to computing the sales factor in a combined/unitary reporting context. The question: when computing a combined group's sales factor, whose nexus determines whether a sale is in the numerator?

### Joyce Rule (Appeal of Joyce Inc., 1966)

Under Joyce, each member entity's nexus is evaluated independently. A sale by Entity A (which has no nexus in the state) is excluded from the combined group's sales factor numerator for that state, even if Entity B (a fellow group member) has nexus.

**Effect**: Reduces the combined group's apportionment factor in the state, because only sales by entities that independently have nexus are included in the numerator.

### Finnigan Rule (Appeal of Finnigan Corp., 1988)

Under Finnigan, the combined group is treated as a single taxpayer. If ANY member of the combined group has nexus in the state, ALL sales by ALL members destined for that state are included in the sales factor numerator.

**Effect**: Increases the combined group's apportionment factor in the state.

### State Adoption

States with combined reporting must choose either Joyce or Finnigan. The choice can significantly affect the combined tax liability:
- **Joyce states** — the combined group's state apportionment is based only on sales by entities with independent nexus
- **Finnigan states** — the combined group's state apportionment captures all sales to the state regardless of which entity made the sale

Some states have switched between Joyce and Finnigan over time. Always verify the current rule for the applicable tax year.

---

## Special Industry Apportionment

Certain industries use modified apportionment formulas because the standard UDITPA factors do not fairly represent their in-state activity.

### Financial Institutions

Many states apply a special formula for banks and financial institutions:
- **Receipts factor** replaces the sales factor and may include interest, dividends, fees, and gains from financial instruments
- Sourcing is based on the location of the borrower, depositor, or the property securing the loan
- Some states use a single receipts factor; others combine it with property and payroll
- The MTC has published a model regulation for financial institutions apportionment

### Airlines

Airline apportionment typically uses:
- **Revenue ton-miles** or **departures** factor instead of or in addition to the standard sales factor
- Property factor includes aircraft, allocated based on time in-state, departures from the state, or revenue generated in the state
- The Airline Deregulation Act (49 USC 40116) preempts certain state taxes; apportionment must comply with federal limitations

### Trucking / Transportation

Motor carrier apportionment may use:
- **Revenue miles** — miles driven in-state / total miles
- Property factor includes vehicles allocated by miles in-state
- Some states use the MTC model for trucking companies

### Publishing / Media

States may apply special sourcing rules for advertising revenue (sourced to location of audience) and subscription revenue (sourced to subscriber location).

### Construction

Long-term construction contracts may be sourced based on the location of the construction project rather than standard sales factor rules.

### Telecommunications

Many states have enacted specific telecom apportionment rules:
- Revenue is sourced based on the location of the customer
- Infrastructure (cell towers, fiber) may be apportioned by mileage or customer count
- Interstate vs. intrastate revenue distinction

---

## MTC Model Allocation and Apportionment Regulation

The MTC has published model regulations interpreting UDITPA, which provide detailed guidance on:

### Business vs. Nonbusiness Income

- **Business income** — income arising from the regular course of the taxpayer's trade or business, or from the acquisition, management, or disposition of property that forms an integral part of the trade or business. Subject to apportionment.
- **Nonbusiness income** — all other income. Allocated (not apportioned) to a specific state (typically the commercial domicile for intangible income, the situs state for real/tangible personal property income).

The distinction matters because nonbusiness income goes entirely to one state, while business income is divided among all nexus states. States vary on the breadth of the "business income" definition — some use a transactional test, some use a functional test, and many use both.

### Allocation of Nonbusiness Income

Under UDITPA/MTC model:
- **Rents and royalties from real property** — allocated to the state where the property is located
- **Rents and royalties from tangible personal property** — allocated to the state where the property is utilized; if in multiple states, apportioned by utilization
- **Capital gains on real property** — allocated to the situs state
- **Capital gains on tangible personal property** — allocated to the state where the property was located at the time of sale
- **Interest and dividends** (nonbusiness) — allocated to the commercial domicile
- **Patent and copyright royalties** (nonbusiness) — allocated to the state where the intangible is used

### Factor Computation Details

**Property factor**:
- Owned property at original cost (not adjusted/depreciated basis)
- Rented property at 8x annual rental (UDITPA standard; some states deviate)
- Average of beginning and end of year values (or monthly averages if property fluctuates)
- Construction in progress is excluded until placed in service (most states)
- Intangible property is excluded from the property factor

**Payroll factor**:
- Total compensation (wages, salaries, commissions, bonuses)
- Sourced where services are performed
- Multi-state employees: base of operations test, then direction/control test, then state of residence
- Leased employees may be included depending on state rules
- Officers' compensation is included

**Sales factor**:
- Gross receipts from all transactions in the ordinary course of business
- Returns and allowances are subtracted
- Intercompany eliminations apply in combined reporting
- Gross receipts vs net gains: most states use gross receipts for tangible property, but some use net gains for financial instruments

---

## Apportionment Planning Considerations

### Factor Management

Corporations can legitimately influence apportionment by managing where factors are located:
- Centralizing property or payroll in low-tax states reduces apportionment to higher-tax states (less effective as more states adopt single sales factor)
- Under single sales factor, the sourcing rule (market-based vs cost-of-performance) is the primary planning lever
- In cost-of-performance states, shifting income-producing activities out of state can reduce the sales factor in that state

### Formula Election

Some states offer an election between formulas (e.g., three-factor or single sales factor). The election may be binding for multiple years. Model the impact before electing.

### Distortion Relief

UDITPA Section 18 (and state equivalents) allows taxpayers or tax authorities to petition for an alternative apportionment method when the standard formula does not "fairly represent" the taxpayer's in-state activity. This is a high bar — the taxpayer must demonstrate that the standard formula produces an unreasonable result AND that the proposed alternative is reasonable.

Section 18 is also used by states to impose alternative apportionment on taxpayers perceived to be using the standard formula to avoid tax.
