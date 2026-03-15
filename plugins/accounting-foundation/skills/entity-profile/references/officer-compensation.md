# Officer Compensation: Reasonable Compensation Rules

**Purpose**: IRS reasonable compensation requirements for corporate officers, documentation standards, and audit defense strategies.

**Skills**: —

---

## Why Reasonable Compensation Matters

### C-Corporation Context

For C-corps, officer compensation is a **deductible expense** that reduces taxable income. The IRS scrutinizes compensation to ensure it is not:

- **Excessive** — Disguised dividends (corporation deducts more than arm's-length amount, avoiding double taxation)
- **Inadequate** — Less common for C-corps, but relevant when corporate income is retained to avoid personal tax

**C-corp incentive**: Higher compensation = larger deduction = lower corporate tax. The IRS may recharacterize excessive compensation as a non-deductible dividend.

### S-Corporation Context (for reference)

S-corps have the opposite incentive: lower compensation = lower payroll tax. The IRS scrutinizes S-corp officer pay to ensure it is not unreasonably low. This is NOT the the client entity situation (C-corp) but comes up in cross-entity advisory work.

## IRS Multi-Factor Analysis

The IRS uses a facts-and-circumstances test based on these factors (from the Reasonable Compensation Job Aid and case law):

### Primary Factors

1. **Training and experience** — Education, certifications, years of industry experience
2. **Duties and responsibilities** — Scope of role, decision-making authority, hours worked
3. **Time and effort devoted** — Full-time vs part-time, hours per week, multiple roles
4. **Dividend history** — C-corps that never pay dividends while paying high salaries attract scrutiny
5. **Compensation paid by comparable businesses** — Industry, geography, company size benchmarks
6. **Compensation compared to gross and net income** — Compensation as a percentage of revenue and profit
7. **Character and condition of the company** — Startup vs mature, profitable vs struggling
8. **Compensation formula or policy** — Consistent, documented methodology carries weight
9. **Conflict of interest** — Officer who sets their own pay, especially when also a controlling shareholder

### The Independent Investor Test

Would an independent investor (hypothetical outside shareholder) be satisfied with the return on their investment after officer compensation is paid? If compensation consumes most or all of corporate profits, leaving no reasonable return for shareholders, the compensation may be excessive.

This test is particularly relevant for closely-held C-corps like the client entity where the officer-shareholder controls both sides of the transaction.

### Additional Factors from Case Law

- **Bonus and contingent compensation** — Must be pursuant to a free bargain between the corporation and officer, ideally established before services are rendered (Exacto Spring Corp v. Commissioner)
- **Catch-up compensation** — Prior years of underpayment can justify current-year above-market pay, but must be documented contemporaneously
- **Comparison to employee compensation** — If officers earn 10x what their next-highest employee earns, IRS may scrutinize

## Schedule E Officer Compensation Reporting

Form 1120, Schedule E requires disclosure of:

- Name of each officer
- Social Security Number
- Percent of time devoted to business
- Percent of stock owned (common and preferred)
- Amount of compensation

This data feeds IRS automated screening. Common audit triggers:

- **Zero officer compensation** when the corporation has revenue (especially for S-corps, but C-corps too)
- **Compensation exceeding 50% of gross receipts** for a small corporation
- **Dramatic year-to-year swings** without documented business justification
- **Compensation far above industry medians** for the company's size and revenue

## Documentation Best Practices

### What to Maintain

- **Written employment agreement** — Signed before the tax year, specifying salary, bonus formula, benefits
- **Board resolution** — Minutes documenting the board's determination of officer compensation, ideally referencing comparable data
- **Compensation study or benchmarking** — Even an informal analysis using publicly available data
- **Time records** — Especially if the officer performs multiple roles or works part-time
- **Job description** — Written description of duties, responsibilities, and authority

### Benchmarking Sources

- **BLS Occupational Employment Statistics** — Median wages by occupation and geography
- **RMA Annual Statement Studies** — Financial ratios by industry (SIC/NAICS)
- **Robert Half Salary Guide** — Professional/executive compensation benchmarks
- **IRS Statistics of Income (SOI)** — Corporate return data by asset size and industry
- **Salary.com, Glassdoor, Payscale** — Market compensation data (less authoritative but useful as corroboration)
- **Trade association surveys** — Industry-specific compensation data

### Documentation Timing

Document the compensation decision **before** the tax year begins or at the start of the year. Retroactive justification is far less persuasive to the IRS.

## Audit Defense Strategy

### If Compensation Is Challenged

1. **Produce the employment agreement and board resolution** — Shows arm's-length negotiation
2. **Present comparable data** — Multiple sources showing market range
3. **Demonstrate the independent investor test** — Show that shareholders received reasonable returns (dividends or retained earnings growth) after compensation
4. **Document officer qualifications** — Resume, certifications, unique skills that justify premium pay
5. **Show consistency** — Stable compensation policy applied year-over-year, adjusted for inflation and business growth

### Common Defenses

- **Multiple hats** — Officer serves as CEO, CFO, sales manager, and technician in a small company. Each role has a market value; aggregate is the benchmark
- **Industry expertise** — Specialized technical knowledge commands premium
- **Revenue generation** — Officer is the primary revenue generator; compensation tied to results
- **Catch-up** — Prior years of below-market pay (document this contemporaneously, not after the fact)

## Zero Compensation Years (Delinquent Filing Context)

For years where the client entity had no payroll but the officer was active:

- **No compensation was paid** — Report $0 on Schedule E with a note that the corporation was inactive or pre-revenue
- **If the officer was active and generating revenue** — The IRS may impute reasonable compensation. This is primarily an S-corp risk (constructive distribution), but for C-corps the risk is that the IRS denies other deductions or recharacterizes transactions
- **Reconstructed returns** — If adding officer compensation to delinquent returns, establish the amount using contemporaneous benchmarking data for the relevant year, not current-year data

## Accounting System Notes

- Officer compensation posts to a payroll expense account (typically "Officers' Compensation" or "Officer Salaries")
- Must track separately from regular employee wages for Schedule E reporting
- The payroll module handles W-2 generation and tax deposit scheduling
- For reconstructed years without actual payroll: Journal entries to Compensation expense with memo noting the basis for the amount

For platform-specific payroll configuration, delegate to `qbo-integration:qbo-coa`.

---

**Authoritative References**:
- IRC 162(a)(1) — Deduction for reasonable compensation
- IRC 162(m) — $1M deduction limit for publicly traded corporations (not applicable to private corps)
- Exacto Spring Corp v. Commissioner, 196 F.3d 833 (7th Cir. 1999) — Independent investor test
- Elliotts, Inc. v. Commissioner, 716 F.2d 1241 (9th Cir. 1983) — Multi-factor analysis
- IRS Reasonable Compensation Job Aid (LB&I)
- Treas. Reg. 1.162-7 — Compensation for personal services
- Treas. Reg. 1.162-9 — Bonuses to employees
