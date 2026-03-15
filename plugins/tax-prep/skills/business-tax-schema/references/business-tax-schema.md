---
name: business-tax-schema
description: |
  Business tax schema: concept vocabulary, form line mappings, and revision topology
  for federal 1120, Florida F-1120, and Form 5472 (2007-2025).

  Use when mapping inventory fields to form lines, checking era-specific rules (pre/post-TCJA),
  validating field completeness, or assembling form-gen packages.
---

# Business Tax Schema

Structured vocabulary for C-corporation tax forms: federal Form 1120, state corporate
return (e.g., Florida F-1120), and Form 5472 (if foreign-owned).

## When to Use This Skill

- **discover-year**: Field naming, completeness checks, cross-reference validation
- **assess-year**: Mapping inventory fields to form lines, era-specific rules
- **assemble-package**: Output field selection for form-gen packages

## File Routing

| Need | File |
|------|------|
| Map an inventory field to a form line | `concept-map.yaml` |
| Federal 1120 line vocabulary (all schedules) | `core/federal-1120.yaml` |
| Florida F-1120 line vocabulary | `core/florida-f1120.yaml` |
| Form 5472 field vocabulary | `core/form-5472.yaml` |
| Signature/preparer fields (shared) | `core/signature-preparer.yaml` |
| FL revision differences by era | `revisions/florida/R-01-{07,13,16,19}.yaml` |
| Federal era differences (TCJA) | `revisions/federal/{pre,post}-tcja.yaml` |
| Per-year filing expectations | `expectations/{year}.yaml` |

## Revision-to-Year Mapping

**Florida F-1120** (4 structural eras):

| Era file | Years | Key features |
|----------|-------|-------------|
| R-01-07 | 2007-2011 | EET present, Schedule VI AMT, taxpayer election (E), $5K exemption |
| R-01-13 | 2012-2014 | EET dropped, line numbers shift, $25K/$50K exemption |
| R-01-16 | 2015-2017 | New credit lines in Schedule V, email added to contact, Schedule VI still present |
| R-01-19 | 2018+ | Schedule VI dropped, taxpayer election dropped, page 2 letters shift |

**Federal 1120** (2 eras):

| Era file | Years | Key features |
|----------|-------|-------------|
| pre-tcja | 2007-2017 | Graduated rates (15-35%), Section 199 on Line 25, corporate AMT, NOL 2yr carryback/20yr forward |
| post-tcja | 2018+ | Flat 21%, no Section 199, no AMT, NOL 80% limit/indefinite carryforward |

## NOL Rules

| Rule | Pre-TCJA (2007-2017) | Post-TCJA (2018+) |
|------|----------------------|-------------------|
| Carryback | 2 years (optional) | Not allowed* |
| Carryforward | 20 years | Indefinite |
| Usage limit | 100% of taxable income | 80% of taxable income |

*CARES Act exception: 2018-2020 NOLs could be carried back 5 years.

NOL ledger: `work-product/nol_ledger.yaml`

## Florida Exemption History

| Years | Exemption |
|-------|-----------|
| 2007-2011 | $5,000 |
| 2012 | $25,000 |
| 2013-2025 | $50,000 |

FL tax rate: 5.5% (stable 2007-2025)

## Form 5472

Required every year for C-corps with 25%+ foreign ownership (IRC 6038A).
Penalty: $25,000 per form for failure to file.
See `core/form-5472.yaml` for Parts I-VI field vocabulary.

## Validation Sources

**External references:**
- [IRS Section 199 Repeal](https://irc.bloombergtax.com/public/uscode/doc/irc/section_199)
- [FL Statutes 220.14](https://www.flsenate.gov/Laws/Statutes/2024/0220.14)
- [FL DOR Corporate Income Tax](https://floridarevenue.com/taxes/taxesfees/Pages/corporate.aspx)

## Historical Form Sourcing

Each tax year requires its own year-specific source. Current-year forms do NOT document historical line structures.

| Source | Years | URL Pattern |
|--------|-------|-------------|
| IRS Prior Archive | 2007-2024 | `irs.gov/pub/irs-prior/i1120--{year}.pdf` |
| TaxFormFinder FL | 2011-2024 | `taxformfinder.org/forms/{year}/{year}-florida-form-f-1120.pdf` |
| eformrs.com FL | 2007-2010 | `eformrs.com/Forms{yy}/States{yy}/FL/FL1120.pdf` |

## Adding a New Year

1. Create `expectations/{year}.yaml` using the prior year as template
2. Verify the FL revision era in `revisions/florida/` — if a new revision stamp appears, update the era file or create a new one
3. Check for federal law changes that would require a revision update (e.g., rate changes, new deductions)
4. Run assess-year against the new expectations to validate

## source_ref Notation

```yaml
source_ref: "extracted_values[field=gross_receipts]"
```
