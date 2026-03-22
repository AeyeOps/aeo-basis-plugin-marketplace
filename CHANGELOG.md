# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.3.1] - 2026-03-22

### Added

- **tax-prep** dividends-received-deduction reference — Complete DRD computation algorithm with year-indexed rate variance table (pre-TCJA 70/80/100, post-TCJA 50/65/100), IRC 246(b) tier-specific taxable income limitation, 246(b)(2) NOL exception, 246(c) holding period, 246A debt-financed stock reduction, 245/245A foreign corporation rules, Schedule C line-by-line walkthrough, and worked examples
- **tax-prep** form-1120-prep Schedule C section — Dedicated DRD section with year-dependent rates and progressive disclosure pointer to the new reference

### Fixed

- **qbo-integration** qbo-audit DRD limitation formula — Replace incorrect blanket "65% of taxable income" with per-tier limitation language and IRC citations
- **qbo-integration** qbo-audit Python calculator — Fix hard-coded `0.65` limitation rate to use year-dependent tier-specific rates, add `tax_year` parameter with pre/post-TCJA rate selection, add IRC 246(b)(2) NOL exception logic

## [0.3.0] - 2026-03-18

### Added

- **tax-prep** accounting-method-changes skill — Form 3115 filing procedures, Section 481(a) adjustment computation, DCN catalog for automatic consent changes (cash-to-accrual, Section 174 R&E, depreciation, UNICAP, bad debts, advance payments), audit protection strategy
- **tax-prep** multistate-salt-matrix skill — Cross-state apportionment formula comparison, federal conformity matrix, multistate nexus evaluation, P.L. 86-272 internet activity analysis, combined/separate reporting, voluntary disclosure programs, state tax credit inventory
- Reference provenance frontmatter on all 130 reference files (authority_level, effective_from, last_verified, jurisdiction)
- Jurisdiction overlay convention documented in CLAUDE.md and README.md

### Changed

- Jurisdiction-labeled section headers on SKILL.md files with dedicated Florida content (state-returns, business-tax-schema, tax-compliance, nol-tracking, tax-planning, entity-profile)
- Scope callout blockquotes added to Florida-containing skills distinguishing national from state-specific content
- Reference files with mixed federal+state content reclassified from `jurisdiction: federal` to `jurisdiction: multistate`

## [0.2.0] - 2026-03-15

Initial release.

### Added

- Marketplace with 6 plugins and 34 skills for C-corporation accounting on US GAAP
- **accounting-foundation** plugin (4 skills) — cross-cutting GAAP knowledge: chart of accounts, categorization rules, entity profile, financial statements
- **qbo-integration** plugin (5 skills) — QuickBooks Online platform integration: API patterns, chart of accounts setup, bookkeeping, reporting, audit data extraction
- **bookkeeping** plugin (7 skills) — full lifecycle: transaction processing, reconciliation, monthly close, payroll recording, compliance reporting, catch-up bookkeeping, audit support
- **tax-prep** plugin (8 skills) — federal Form 1120 and Florida F-1120: form prep, state returns, supporting schedules, compliance, planning, NOL tracking, e-filing
- **financial-planning** plugin (5 skills) — budgeting, variance analysis, financial modeling, strategic advisory, ASC 740 tax provision
- **firm-operations** plugin (5 skills) — engagement management, practice management, quality compliance, data governance, workflow automation
- 124 reference files providing source knowledge for skill synthesis
- Platform-neutral domain content with swappable accounting system integration via qbo-integration plugin
- Cross-plugin skill invocation network enabling runtime knowledge sharing without file dependencies
