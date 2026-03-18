---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Tax Computation Provenance Patterns

Reference patterns for provenance-tracked tax computation, deterministic rules engines,
and multi-form/multi-state PDF compilation pipelines. While derived from individual
(Form 1040) tax preparation systems, the architectural patterns -- traced computation,
integer-cents arithmetic, quality gates, conditional form inclusion, and state-downstream-
of-federal design -- are applicable to any tax computation system including corporate
returns.

Distilled: 2026-03-11

## 1. Core Concepts

- Every computed tax value carries **provenance** linking it to source documents and IRS citations
- All tax computation runs deterministically via pure functions -- no LLM math, no stochastic outputs
- All monetary values use **integer cents** throughout to prevent floating-point drift
- Paper filing (PDF package) is the primary output; e-file (MeF XML) requires separate EFIN/ETIN authorization
- Privacy-first: all computation can run client-side with no PII leaving the device

## 2. Architecture

### System Topology

```
Intake (document parsing) -> Interview (guided Q&A) -> Compute (rules engine) -> Review (trace) -> Download (PDF)
```

Each stage operates on a canonical tax return data model stored centrally. The compute
stage is a pure function of that model -- given the same inputs, it always produces the
same outputs.

### Pipeline Layers

- **Intake** -- document parsing (OCR, CSV import) to extract source data from W-2s, 1099s, broker statements
- **Interview** -- guided Q&A flow with conditional step visibility (e.g., spouse info only for joint filers)
- **Compute** -- deterministic rules engine that transforms the tax return model into computed results
- **Review** -- trace-based explainability showing how each number was derived
- **Download** -- PDF compilation pipeline that fills IRS form templates

## 3. Rules Engine

### Deterministic Computation

Rules are pure functions: given a tax return model, produce traced values. Key principles:
- Every monetary value is **integer cents** (e.g., $100.10 = 10,010 cents) to eliminate floating-point rounding errors
- Rules are versioned per tax year under separate module directories
- Adding a new tax year: create year-specific constants + year module, register in the multi-year dispatch table

### Compute Node Pattern

Each computation unit has:
- **ID** -- unique identifier (e.g., `form1040.line9`)
- **Label** -- human-readable description (e.g., "Total Income, Form 1040 Line 9")
- **Compute function** -- pure function that takes the tax return model and produces a traced value
- **Dependencies** -- list of other node IDs this node depends on
- **IRS citation** -- reference to IRS instructions (e.g., "Form 1040, Line 9 Instructions")
- **Tax year** -- version pinning for year-specific rules

The engine calls the top-level computation function, which internally calls each
schedule/form computation in dependency order, then collects all results into a flat
map of node ID to traced value.

### Trace Engine

The top-level compute function produces a result containing:
- All form line values (Form 1040, schedules, credits, state returns)
- A flat trace map: node ID -> traced value, for explainability
- List of executed schedules
- Quality gate results (validation)

The compute trace forms a tree: each node has an output traced value and references
to its input nodes, enabling drill-down from any final number to its source documents.

### Selective Computation

The engine supports skipping schedules that are not needed for a given return (e.g.,
Schedule B only computed if interest/dividends exceed the filing threshold).

## 4. Provenance-Tracked Values

### Traced Value Structure

Every computed value carries:
- **Amount** -- integer cents
- **Source** -- discriminated union:
  - *Document source*: document type, document ID, field name, description
  - *Computed source*: node ID, list of input node IDs, description
  - *User entry source*: field name, timestamp of entry
- **Confidence** -- 0 to 1.0 (1.0 = certain; lower values flag OCR or estimated data)
- **IRS citation** -- optional reference to the specific form line or instruction

### Why Provenance Matters

- **Audit trail**: any number on the return can be traced back to its source document or computation
- **Error detection**: low-confidence values are flagged for manual verification
- **Explainability**: "why this number?" drill-down from any form line to contributing inputs
- **Quality assurance**: reviewers can verify the chain of computation without re-deriving values

## 5. State Computation Architecture

### State-Downstream-of-Federal Pattern

Each state return implementation follows a standard contract:
- **Input**: tax return model + federal Form 1040 result + state return configuration
- **Output**: state computation result (state AGI, taxable income, tax, credits, withholding, refund/owed)

State computation is strictly downstream of federal:
- States consume the federal result but never modify it
- No circular dependency between federal and state
- States reuse the same bracket computation logic with state-specific bracket tables

### State Computation Result

Each state produces:
- State code, form label, residency type
- State AGI, state taxable income, state tax, state credits, tax after credits
- State withholding, overpaid amount, amount owed
- Optional: apportionment ratio (part-year/nonresident), disclosures
- State-specific detail (varies by state)

### Common State Patterns

- All states start from federal AGI, then apply state-specific additions/subtractions
- Most states use their own bracket tables (progressive rates)
- Part-year residents use apportionment ratios based on days/income in state
- State withholding sourced from W-2 state boxes
- Each state module provides a config-driven review layout for consistent UI

### Residency Types

- Full-year resident (default)
- Part-year resident (requires move-in/move-out dates for apportionment)
- Nonresident (income sourced to state)

## 6. Constants and Parameters

### Federal Constants (Example: Tax Year 2025)

- Standard deduction by filing status (5 statuses)
- Additional standard deduction (age 65+ / blind)
- Ordinary income tax brackets (7 brackets x 5 filing statuses)
- Long-term capital gains thresholds (0% / 15% / 20%)
- NIIT rate (3.8%) and thresholds by filing status
- Additional Medicare tax rate (0.9%) and thresholds
- Social Security wage base
- Capital loss deduction limit ($3,000 / $1,500 MFS)
- Schedule B filing threshold ($1,500)
- All amounts sourced from IRS Revenue Procedures

### Multi-Year Framework

- Constants are versioned per tax year in separate modules
- New year: create year-specific constants (delta from prior year), register in dispatch table
- State modules, form compilers, and templates are all year-aware

## 7. Data Model

### Central Tax Return Type

Top-level fields:
- Tax year, filing status, dependent-claimable flag
- Taxpayer and optional spouse information
- Income source selections (controls which interview steps appear)

Source documents (arrays):
- W-2s (all 20 boxes, Box 12 coded entries, Box 13 checkboxes, state/local boxes)
- 1099-B, 1099-INT, 1099-DIV, 1099-MISC, 1099-NEC, 1099-G, 1099-R, SSA-1099, 1095-A
- RSU vest events, ISO exercises
- Schedule C businesses, Schedule E properties, Schedule K-1s

Derived data:
- Capital transactions (processed from 1099-B + equity compensation linkage)
- Adjustments, deductions (standard/itemized with age/blind flags), credits
- Prior year info (AGI, capital loss carryforwards, suspended passive activity losses)
- Estimated tax payments (quarterly)
- State return configurations

### Key Type Flow

```
TaxReturn -> computeFederal() -> FederalResult
                                      |
                                      v
TaxReturn + FederalResult + StateConfig -> stateModule.compute() -> StateResult
                                                                        |
                                                                        v
TaxReturn + StateResult + StateFormTemplates -> stateCompiler.compile() -> StatePDF
```

## 8. PDF Compilation Pipeline

### Pipeline Steps

1. Load blank IRS PDF template bytes for each form
2. Run the rules engine to compute all values
3. Determine which forms are needed (conditional inclusion based on computed values)
4. Fill each form using its filler function:
   a. Load the blank template
   b. Set text fields with computed values
   c. Set checkboxes for applicable flags
   d. Flatten the form (makes fields read-only)
5. Fill state forms via state-specific compilers
6. Generate a cover sheet (summary with key numbers, forms list, mailing checklist)
7. Assemble all filled forms into a single PDF in IRS attachment sequence order

### Assembly Order (IRS Attachment Sequence Numbers)

Forms are ordered by IRS attachment sequence numbers:
Form 1040 (00) -> Schedule 1 (02) -> Schedule 2 (05) -> Schedule 3 (06) -> Schedule A (07) -> Schedule B (08) -> Schedule C (09) -> Schedule D (12) -> Form 8949 (12A) -> Schedule E (13) -> Schedule SE (17) -> ... continuing through all applicable forms

### Conditional Form Inclusion

The compiler determines which forms are needed based on computed values:
- Schedule B only included if interest/dividends exceed the filing threshold
- Form 8949 only if capital transactions exist
- State forms only if that state appears in the return's state configuration
- Each form's inclusion is driven by the rules engine output, not user selection

### Field Name Discovery

PDF form field names are discovered by enumerating the AcroForm fields in IRS PDF
templates. Each form has a mapping file documenting the field name -> computed value
correspondence.

## 9. Form Coverage

### Federal Forms (Comprehensive)

Core: Form 1040 (Lines 1-37)

Schedules: 1 (Additional Income), 2 (Additional Taxes), 3 (Additional Credits), A (Itemized Deductions with SALT cap, mortgage, charitable, investment interest), B (Interest & Dividends), C (Business Income), D (Capital Gains/Losses with carryforward), E Parts I & II (Rental + K-1 passthrough), SE (Self-Employment Tax)

Credits: Child Tax Credit (8812), EITC, Education Credits (8863), Dependent Care (2441), Saver's Credit (8880), Energy Credits (5695), Foreign Tax Credit (1116), Premium Tax Credit (8962)

Other: AMT (6251), HSA (8889), Capital Gains Detail (8949), QBI Deduction (8995/8995-A), Additional Medicare Tax (8959), NIIT (8960), Passive Activity Loss (8582), Nondeductible IRAs (8606), Home Office (8829), Investment Interest (4952)

### Income Types Supported

W-2, 1099-INT, 1099-DIV, 1099-B, 1099-MISC, 1099-NEC, 1099-G, 1099-R, 1099-SA, 1095-A, SSA-1099, K-1 (partnership/S-corp/trust), rental (Schedule E), self-employment (Schedule C), RSU vest events, ISO exercises

## 10. Interview Flow Design

### Conditional Step Visibility

Each interview step has:
- **Visibility predicate**: controls whether the step appears (e.g., spouse info only for MFJ/MFS)
- **Completion predicate**: drives progress indicators

### Section Structure (36 Steps in 5 Sections)

1. **Getting Started**: filing status, personal info, dependents, income source selection
2. **Income**: conditional steps per income type (W-2, 1099 variants, Schedule C/E/K-1)
3. **Deductions & Credits**: prior year info, standard vs itemized, credit inputs
4. **Review**: federal review with trace explainability, per-state reviews
5. **Download**: PDF generation and filing package assembly

### Gap Analysis

Automated completeness checking:
- Personal info (name, SSN, address)
- Filing status presence
- Spouse info for joint filers
- Income document presence
- Withholding information
- Returns: completion percentage, missing items, readiness assessment, next action suggestion

## 11. Quality Gates

Four categories of validation, with violations collected (not thrown) to surface all issues at once:

- **Registry gates**: every rules module must have a matching form compiler
- **Computation gates**: computed results satisfy numeric and structural invariants
- **Cross-state gates**: multi-state returns are internally consistent (no double-counting, withholding allocations sum correctly)
- **Compiler gates**: PDF output validation (all required fields populated, values within expected ranges)

## 12. Key Design Decisions

### Integer Cents Everywhere
All monetary values stored as integer cents to eliminate floating-point drift. Helper
functions convert between dollars and cents at I/O boundaries only.

### Deterministic Rules, No LLM Math
AI may be used for OCR extraction (with confidence scores) and conversational guidance,
but never for final tax calculations, form field values, or filing decisions. Rules are
pure functions, fully testable, version-pinned per tax year.

### Provenance on Every Value
Every value carries source, confidence, and IRS citation. Enables "why this number?"
trace drill-down from any form line. Supports interactive explainability.

### State Computation is Downstream
No circular dependency between federal and state. States consume the federal result and
produce state-specific outputs. Reuses federal bracket computation with state bracket tables.

### Conditional Form Inclusion
Compiler determines which forms are needed based on computed values, not user selection.
Reduces unnecessary form clutter and ensures consistency.

### Privacy-First
All computation can run client-side. No PII needs to be transmitted for core tax
computation. SSNs masked in review interfaces, only written to final PDF output.

## 13. Equity Compensation Basis Adjustment

### Problem
Brokers report RSU sales on 1099-B with $0 cost basis, causing double taxation
(income already taxed on W-2 at vest).

### Solution Pipeline
1. Capture RSU vest events (date, shares, FMV at vest, linked W-2)
2. Match RSU vests to 1099-B sales by security identifier, acquisition date, and share quantity
3. Detect basis discrepancy (1099-B basis vs FMV-at-vest basis)
4. Auto-generate Form 8949 adjustment with appropriate category and adjustment code
5. Calculate tax impact of the correction
