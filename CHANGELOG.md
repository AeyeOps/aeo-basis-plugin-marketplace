# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
