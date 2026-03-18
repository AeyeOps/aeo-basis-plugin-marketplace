---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Form Dependency Resolution Patterns

Reference patterns for resolving inter-form dependencies in US individual income tax
returns (Form 1040). While this describes individual returns, the dependency resolution
architecture, threshold parameterization, and tax table structures are transferable
concepts applicable to any multi-form tax computation system.

## Dependency-Solver Architecture

### High-Level Flow

```
Input data --> Input store --> Solver --> Value store --> Solution --> PDF filler --> PDF
```

- **Input**: Taxpayer supplies structured data (form sections with field values)
- **Solve**: Solver builds a dependency graph implicitly by attempting field calculations; dependencies are discovered at runtime when a field requests a value that has not yet been computed
- **Output**: Solution serialized as a structured data file containing all computed values
- **PDF**: Filler reads the solution, maps computed values to PDF form fields, and overlays them onto blank IRS template PDFs

### Implicit Dependency Resolution

No explicit dependency declarations are needed. Dependencies are discovered at runtime:
- When a field computation function requests the value of another field that has not yet been solved, the solver catches the unmet-dependency signal and registers the requesting field as blocked
- When the blocking field is eventually solved, all fields waiting on it are re-attempted
- Conditional access creates conditional dependencies: `if has_hsa: return hsa_deduction` only requires HSA fields when the taxpayer has an HSA
- This approach works because IRS tax form structures are inherently directed acyclic graphs (DAGs) -- no circular dependencies exist

### Solver Loop

1. Add requested top-level forms (e.g., Form 1040), instantiate them, register their inputs and fields
2. Loop while there are unattempted fields or newly-met dependencies:
   a. Pop a field from the unattempted list, attempt to compute it
   b. If computation succeeds, store the value and mark the dependency as met; re-attempt any fields that were waiting on it
   c. If computation raises an unmet-dependency signal (needs another field or form), register the dependency and move on
   d. If computation raises a missing-input signal (needs taxpayer data not yet supplied), register the input dependency
   e. If an unknown form is referenced, dynamically load and instantiate that form, adding its fields to the solver
3. After the loop: verify all dependencies resolved. Report any unresolvable fields (unimplemented tax scenarios, missing inputs, stalled dependencies)

### Diagnostic Outputs (Post-Solve)

- **Unimplemented fields** -- fields where the computation explicitly flagged an unhandled tax scenario
- **Unmet input dependencies** -- missing taxpayer-supplied values and which fields need them
- **Unmet field dependencies** -- fields that could not be solved and which other fields are blocked by them

## Form Model

### Form Structure

Each tax form (1040, schedules, worksheets) is defined with:
- **Name** -- short identifier (e.g., `1040`, `1040_sa`, `w-2`)
- **Tax year** -- the specific year this form definition applies to
- **Description** -- short and long form titles
- **Jurisdiction** -- federal (US) or state
- **Sequence number** -- controls PDF output ordering within a jurisdiction
- **Inputs** -- list of taxpayer-supplied data fields (with type validation)
- **Required fields** -- fields always computed when the form is used
- **Optional fields** -- fields computed only when referenced by other fields
- **Thresholds** -- named constants, often keyed by filing status (see below)
- **PDF field mappings** -- mappings from PDF form field names to computed field names

### Input Forms vs Computed Forms

- **Input forms** (W-2, 1099s, 1098) are data-entry-only: every field is a required input, nothing is computed, and they do not need to be filed (the taxpayer already has physical copies)
- **Computed forms** (1040, schedules, worksheets) have computation logic that derives field values from inputs and other fields

### Form Instances (Multiple Copies)

Forms can have named instances for taxpayers with multiple copies of the same source document:
- W-2 instances: `w-2:0`, `w-2:1` (one per employer)
- HSA instances: `8889:taxpayer`, `8889:spouse`
- Count inputs drive iteration: "number of W-2s" determines how many W-2 instances to sum

## Input Types and Validation

Each input field has a type that validates and parses the taxpayer's entry:
- **String** -- strips whitespace
- **Boolean** -- accepts yes/no variants (case-insensitive)
- **Integer** -- empty defaults to 0
- **Decimal** -- empty defaults to 0.0
- **Enum** -- validates against a defined set of allowed values (e.g., filing status)
- **Pattern** -- validates against a format pattern (e.g., SSN format)

### Validation Exception Flow

Three distinct failure modes, each handled differently by the solver:
- **Missing specification** -- input key not in any known form's input list (triggers dynamic form loading)
- **Missing value** -- input exists in spec but taxpayer hasn't supplied a value (triggers prompting or failure)
- **Invalid value** -- value present but fails type validation

## Field Types and Computation

Computed fields have types that constrain their output:
- **String**, **Boolean**, **Integer**, **Decimal** (with configurable decimal places), **Enum**

### Field Computation Functions

Each field has a computation function that receives:
- **Inputs accessor** -- reads taxpayer-supplied data; bare key resolves to current form, dotted key (e.g., `1040.filing_status`) crosses to another form
- **Values accessor** -- reads computed field values; same resolution rules
- Return `None` to skip output for this field (still counts as 0/empty when other fields access it)
- Signal "not implemented" to flag an unhandled tax scenario
- Access named constants via threshold lookup

### Dependencies Are Implicit

- No explicit dependency declarations needed
- Dependencies discovered at runtime by accessing another field's value
- Accessing a cross-form value triggers that form to be loaded and that field to be solved
- Conditional access means conditional dependencies

## Threshold Dictionaries

Named constants passed to forms, used instead of magic numbers in computation logic.

### Simple Thresholds
```
minimum_tax: 500.00
```

### Filing-Status-Keyed Thresholds
```
standard_deduction:
  Single, MarriedFilingSeparately: 13,850
  MarriedFilingJointly, QualifyingSurvivingSpouse: 27,700
  HeadOfHousehold: 20,800
```

Keys can be single filing status values or groups of statuses sharing the same threshold.
Accessed in field logic by name and optional filing status key.

Heavily used in Form 1040 computation (standard deduction, EIC limits, Medicare thresholds, QBI thresholds).

## Tax Tables and Brackets

### Tax Table Structure (Income Under $100,000)

For taxable income below $100,000, the IRS publishes a lookup table with rows in $25 or $50 increments:
```
(income_min, income_max, single_tax, mfj_tax, mfs_tax, hoh_tax)
```
- ~2,000 rows covering $0-$99,999
- Tax amount looked up by income range and filing status column

### Marginal Rate Brackets (Income $100,000+)

For taxable income at or above $100,000, marginal rate brackets apply:
```
(income_min, income_max, rate, subtraction_amount)
```
- Tax = taxable_income x rate - subtraction_amount
- Grouped by filing status
- Example (Single): (100,000, 164,925, 0.24, 5,979.00)

### Qualified Dividends and Capital Gains

Preferential tax rates on qualified dividends and long-term capital gains:
- 0% / 15% / 20% rate tiers with filing-status-dependent thresholds
- Computed via a worksheet that applies preferential rates to the qualified portion and ordinary rates to the remainder

## Supported Form Dependencies (Federal)

### Output Forms (Filed)
- Form 1040 -- U.S. Individual Income Tax Return
- Schedule 1 -- Additional Income and Adjustments to Income
- Schedule 3 -- Additional Credits and Payments
- Schedule A -- Itemized Deductions
- Schedule B -- Interest and Ordinary Dividends
- Schedule 8812 -- Credits for Qualifying Children
- Form 8606 -- Nondeductible IRAs
- Form 8889 -- Health Savings Accounts
- Form 8959 -- Additional Medicare Tax
- Form 8995 -- Qualified Business Income Deduction

### Worksheets (Not Filed)
- Qualified Dividends and Capital Gain Tax Worksheet
- Schedule 2 Need Form 6251 Check
- Recovery Rebate Credit Worksheet
- Figure Tax (tax table / bracket lookup)

### Input Forms (Data Entry Only)
- W-2, 1098, 1099-DIV, 1099-G, 1099-INT, 1099-R

### State Forms
State returns depend on federal results (e.g., North Carolina D-400 series depends on Form 1040 AGI and taxable income).

## Filing Status Enumerations

- Single
- Married Filing Jointly
- Married Filing Separately
- Head of Household
- Qualifying Surviving Spouse (called Qualifying Widow/Widower in tax years before 2022)

## Field Naming Conventions

- Input names: descriptive snake_case (e.g., `filing_status`, `medical_dental_expenses`)
- Source document inputs: match IRS box numbers (e.g., `box_1`, `box_12a_code`, `box_13_retirement`)
- Computed field names: match IRS line numbers (e.g., `1`, `2a`, `5a`, `12c`)
- Cross-form references: `form_name.field_name` or `form_name:instance.field_name`
- Count inputs for multi-instance: `number_w-2`, `number_1099-div`

## PDF Output Pipeline

### Form Filling

For each form that needs filing:
1. Read the computed solution values for that form
2. Map solution field names to PDF template field names (text fields, checkboxes, dropdowns)
3. Fill the blank IRS PDF template with the mapped values
4. Flatten the form (makes fields read-only)

### Assembly

- All filled form PDFs concatenated into a single output document
- Forms sorted by (jurisdiction, sequence number) for correct IRS attachment ordering

## Key Design Patterns

- **Input-only forms** -- source documents (W-2, 1099) are pure data entry; no computation logic
- **Conditional dependencies** -- gate expensive or optional computations behind boolean checks to avoid requiring unnecessary inputs
- **Multi-instance iteration** -- use count inputs to drive iteration over multiple copies of the same form type
- **Not-implemented guards** -- explicitly flag unhandled tax scenarios rather than silently producing incorrect results
- **Thresholds over magic numbers** -- define named constants in threshold dictionaries, never hard-code dollar amounts in computation logic
- **DAG assumption** -- IRS form structures are inherently acyclic; no circular dependency handling is needed

## Limitations

- Individual (Form 1040) only; patterns are transferable to corporate returns but form coverage is individual-focused
- No Schedule C (business income), Schedule D (capital gains), Schedule E (rental/partnership), Schedule F (farm income) in the dependency resolution reference
- No AMT (Form 6251), Form 4797, Form 8863 (education credits), Form 8949
- Single-year forms only; no multi-year carryforward tracking
