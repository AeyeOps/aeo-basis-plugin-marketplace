# Worker Classification — Employee vs Independent Contractor

**Purpose**: Framework for classifying workers as employees or independent contractors, covering federal and state tests, safe harbor relief, reclassification programs, and documentation requirements.

**Skills**: `bk/1099-compliance` (1099-NEC filing), `tx/payroll-tax-compliance` (employment tax consequences)

**Tier**: 2 (effective date log — rule changes track to specific effective dates)

---

## Why Classification Matters

Misclassifying an employee as an independent contractor triggers:

- **Back employment taxes**: Employer's share of FICA (7.65%), FUTA, plus the employee's share the employer failed to withhold
- **Penalties**: Failure-to-file (Form 941), failure-to-deposit, failure-to-furnish (W-2), plus interest
- **Trust fund recovery penalty**: Personal liability under IRC 6672 for responsible persons
- **State consequences**: Back state unemployment taxes, workers' compensation violations, benefit eligibility disputes
- **Benefits liability**: Misclassified workers may claim retroactive participation in employer benefit plans (health, retirement)

The IRS estimates billions in annual tax gap from misclassification. It is a persistent audit focus.

## IRS Common Law Test — Three Categories

No single factor is determinative. The IRS weighs the totality of the relationship across three categories (Rev. Rul. 87-41, updated in IRS Publication 15-A):

### 1. Behavioral Control

Whether the business has the right to direct and control **how** the work is done:

- **Instructions**: Does the business dictate when, where, and how to work? What tools to use? What order or sequence to follow? Where to purchase supplies?
- **Training**: Does the business provide training on methods and procedures? Employees typically receive training; independent contractors use their own methods.
- **Evaluation systems**: Is the worker evaluated on how the work is performed (employee indicator) or only on the end result (contractor indicator)?

Key: The right to control is what matters, not whether control is actually exercised.

### 2. Financial Control

Whether the business has the right to direct and control the **economic aspects** of the work:

- **Significant investment**: Has the worker made unreimbursed capital investment in equipment, facilities, or tools? Significant investment suggests contractor status.
- **Unreimbursed expenses**: Do workers pay their own business expenses (office, insurance, supplies)? Contractors typically bear their own costs.
- **Opportunity for profit or loss**: Can the worker realize a profit or suffer a loss from the engagement? Employees receive guaranteed pay; contractors bear economic risk.
- **Services available to the market**: Does the worker advertise and offer services to the general public? Contractors typically serve multiple clients.
- **Method of payment**: Guaranteed salary/hourly = employee indicator. Flat fee per project = contractor indicator.

### 3. Type of Relationship

The parties' perception and practice regarding the relationship:

- **Written contracts**: While not determinative, contracts describing the relationship carry weight. A contract calling someone a "contractor" does not override the economic reality.
- **Employee benefits**: Providing health insurance, pension, paid leave, etc. indicates an employment relationship.
- **Permanency**: An indefinite, ongoing relationship suggests employment. A defined engagement for a specific project suggests contractor status.
- **Integral to business**: If the worker's services are a key aspect of the regular business, that suggests employment.

## Form SS-8 — Determination of Worker Status

Either party can file Form SS-8 to request an IRS determination. IRS sends questionnaires to both parties; determination takes 6+ months and is binding on the specific relationship. Filing invites IRS scrutiny — if confident in contractor status, maintaining documentation may be preferable. However, a favorable SS-8 determination provides strong audit defense.

## ABC Test (State-Level)

Many states and the DOL use the ABC test, which is **more restrictive** than the IRS common law test. Under the ABC test, a worker is an employee unless the hiring entity proves ALL THREE prongs:

- **(A) Free from control and direction** — The worker is free from the business's control and direction in performing the work, both under contract and in fact
- **(B) Outside usual course of business** — The work is performed outside the usual course of the hiring entity's business (e.g., a plumber hired by a law firm is outside the firm's usual business; a plumber hired by a plumbing company is not)
- **(C) Independently established trade** — The worker is customarily engaged in an independently established trade, occupation, profession, or business of the same nature as the work performed

### States Using ABC Test (Partial List)

California (AB5, effective 1/1/2020), Massachusetts, New Jersey, Illinois, Connecticut. Each state has its own exemptions and carve-outs. The ABC test creates significantly more employees than the IRS common law test.

## DOL Economic Reality Test

For FLSA (Fair Labor Standards Act) purposes — minimum wage, overtime — the DOL applies a six-factor economic reality test:

1. Opportunity for profit or loss depending on managerial skill
2. Investments by the worker and the potential employer
3. Degree of permanence of the work relationship
4. Nature and degree of control
5. Extent to which the work is integral to the employer's business
6. Skill and initiative

The 2024 DOL final rule (effective March 11, 2024) returned to the totality-of-circumstances economic reality test, replacing the 2021 rule that had prioritized two "core factors."

## Section 530 Safe Harbor — IRC 530

### Requirements (All Three Must Be Met)

A business that misclassified workers may avoid **employment tax liability** (not reclassification itself) if it demonstrates:

1. **Reasonable basis** for treating workers as independent contractors, based on one of:
   - Judicial precedent, published IRS ruling, or technical advice
   - Prior IRS audit that did not reclassify substantially similar workers
   - Long-standing recognized practice of a significant segment of the industry
2. **Substantive consistency** — The business (and any predecessor) has not treated any worker holding a substantially similar position as an employee for any period after 1977
3. **Reporting consistency** — The business filed all required federal tax returns (including Forms 1099) consistent with treating the workers as non-employees

### Key Limitations

- Section 530 relief applies only to **federal employment tax liability**, not to the worker's status for benefits, state taxes, or other purposes
- Cannot be used prospectively to justify new classifications — it is a defense against past misclassification
- The burden of proof is on the IRS, not the taxpayer (IRC 530(e)(4))

## Voluntary Classification Settlement Program (VCSP)

### What It Is

An IRS program allowing businesses to prospectively reclassify workers from independent contractors to employees with significantly reduced penalties:

- Pay 10% of the employment tax liability for the most recent tax year (calculated at reduced rates under IRC 3509)
- No interest or penalties
- No employment tax audits for prior years on the reclassified workers
- Must treat workers as employees going forward

### Eligibility

- Must have consistently treated the workers as independent contractors
- Must have filed all required 1099s for the workers for the previous three years
- Cannot be under IRS or DOL audit regarding the workers' classification
- File Form 8952 and enter into a closing agreement with the IRS

## Documentation Best Practices

For every independent contractor engagement, maintain:

- **Written independent contractor agreement**: Scope, payment terms, no benefits, control limited to result
- **Invoices from the contractor** (not timesheets)
- **Proof of contractor's business**: Business license, insurance certificates, other clients
- **1099-NEC**: Filed timely for payments of $600+ (replaced 1099-MISC Box 7 starting 2020)
- **No exclusivity clause**: Agreement should not prohibit working for others

## Decision Points

1. **Common law test or ABC test?** Determine which test applies based on the jurisdiction and context (federal taxes vs state unemployment vs FLSA).
2. **Borderline case?** Consider filing Form SS-8 for certainty, or strengthen documentation and maintain consistent treatment.
3. **Existing misclassification discovered?** Evaluate Section 530 safe harbor eligibility. If not eligible, consider VCSP for prospective correction with reduced penalties.
4. **Multi-state operations?** Evaluate classification under each state's test. A worker may be a contractor federally but an employee under state ABC test.
5. **Converting contractor to employee?** Stop 1099 treatment, begin payroll, issue W-2. Consider VCSP if prior classification was incorrect.

## Accounting System Integration Notes

For platform-specific vendor setup and 1099 tracking, invoke `qbo-integration:qbo-bookkeeping` and `bookkeeping:compliance-reporting`.

- Independent contractors are set up as **Vendors**, not Employees, in the accounting system
- Payments post to expense accounts (e.g., Contract Labor, Professional Fees) via Check or Bill Payment
- Track 1099-eligible payments by vendor — flag vendors for 1099 reporting in the vendor record
- 1099-NEC generation: most accounting systems can prepare and e-file 1099s for eligible vendors at year-end
- If reclassifying a contractor to employee: create a new Employee record, begin payroll, stop vendor payments. Do NOT merge vendor and employee records.

## Effective Date Log

| Date | Change | Authority |
|------|--------|-----------|
| 1978 | Section 530 safe harbor enacted | Revenue Act of 1978 |
| 1987 | IRS 20-factor test published | Rev. Rul. 87-41 |
| 2000s | IRS consolidates to 3-category common law test | IRS restructuring / Pub 15-A |
| 2015 | 1099-MISC filing deadline moved to January 31 for NEC amounts | PATH Act |
| 2020 (1/1) | California AB5 applies ABC test broadly | Cal. Labor Code 2775 |
| 2020 (tax year) | Form 1099-NEC replaces 1099-MISC Box 7 for nonemployee compensation | IRS / Treasury |
| 2020 (11/3) | California Prop 22 exempts app-based drivers from AB5 | Ballot initiative |
| 2021 (3/8) | DOL final rule: two "core factors" (control + opportunity for profit/loss) | 86 FR 1168 |
| 2024 (3/11) | DOL final rule: returns to totality-of-circumstances, rescinds 2021 rule | 89 FR 1638 |

---

**Authoritative References**:
- IRC 3121(d) — Definition of employee
- IRC 3509 — Determination of employer's liability for certain employment taxes (reduced rates for good-faith misclassification)
- IRC 530 (Revenue Act of 1978) — Safe harbor for worker classification
- Rev. Rul. 87-41 — IRS 20-factor common law test
- IRS Publication 15-A — Employer's Supplemental Tax Guide (worker classification guidance)
- IRS Form SS-8 — Determination of Worker Status
- IRS Form 8952 — Application for VCSP
- DOL Final Rule, 89 FR 1638 (Jan. 10, 2024) — Employee or Independent Contractor Classification Under the FLSA
- California Labor Code 2775 (AB5) — ABC test
- Nationwide Mut. Ins. Co. v. Darden, 503 U.S. 318 (1992) — Common law agency test for ERISA
