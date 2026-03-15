---
name: workflow-automation
description: >
  Contains verified event-driven automation architectures (webhook triggers, polling patterns,
  7-stage document collection workflows), deadline escalation sequences with status color
  protocols, and bank feed exception-reporting rules with anomaly thresholds. Covers recurring
  task generation from engagement templates, scheduled report distribution with variance
  flagging, build-vs-buy decision frameworks, ROI analysis models, practice management
  integration patterns, and level-based staff training for automation adoption including
  reverse mentoring. Consult when designing workflow triggers or escalation cadences,
  configuring bank feed auto-categorization rules and exclusions, setting up automated report
  generation and distribution, evaluating automation tools or build-vs-buy tradeoffs,
  planning automation ROI, creating recurring task templates, designing document collection
  sequences, or structuring staff training programs for technology rollouts.
---

# Workflow Automation

Operational skill for designing, implementing, and maintaining process automation across
accounting firm workflows. Synthesized from event-driven automation patterns, practice
management integration architectures, and staff development frameworks for technology
adoption.

## Automation Candidate Identification

Evaluate workflows against three criteria before automating: volume (affects 50+ clients
or fires weekly+), determinism (rules can be stated without human judgment), and error
cost (manual execution has a meaningful error rate). Workflows that score high on all
three are strong automation candidates; workflows requiring professional judgment should
remain manual with automated preparation of inputs.

**High-value automation candidates ranked by typical ROI:**

- Bank feed categorization via accounting system bank rules (recurring, high-volume, rule-based)
- Deadline reminder sequences and escalation (calendar-driven, no judgment required)
- Document collection kickoff and follow-up (date-triggered, template-driven)
- Recurring task generation from engagement templates (predictable, repetitive)
- Scheduled financial report generation and distribution (periodic, deterministic)
- Transaction anomaly flagging for human review (pattern-match, not decision)
- Client portal document classification and routing (filename/OCR-based)

**Keep manual:** Complex tax positions, client advisory conversations, engagement
scoping decisions, exception resolution after anomaly flags.

## Event-Driven Automation Architecture

### Accounting System Webhooks

The accounting system sends real-time POST notifications when entities change. The webhook
payload typically contains entity type, operation (Create/Update/Delete), and entity ID
only -- not the entity data itself. The receiving application must call the accounting
system API to retrieve full details. For platform-specific webhook setup (endpoint
registration, signature verification, retry logic, available entities), invoke
`qbo-integration:qbo-api`.

**Practical trigger patterns:**
- Large transaction alerting -- fetch transaction details on webhook, alert engagement
  manager if amount exceeds threshold (e.g., >$10,000)
- Practice management sync -- invoice creation in the accounting system triggers billing
  entry in the practice management tool
- Anomaly detection -- flag transactions to unusual accounts, round-number amounts, or
  prior-period postings
- Client activity monitoring -- detect when bookkeeping clients make direct entries in
  the accounting system (potential untrained-user problems)

### Polling vs. Webhook Decision

Use webhooks for real-time needs (transaction alerting, status sync). Use scheduled
polling (daily/weekly) for batch reporting and reconciliation where real-time notification
adds no value.

## Document Collection Workflow

A seven-stage automated sequence from kickoff through intake:

1. **Kickoff trigger** -- on configured date (e.g., January 15), initiate collection for
   all active engagements of the relevant type
2. **Auto-generate request list** -- pull template for entity type, customize with
   client-specific items from the prior-year engagement
3. **Portal folder creation + initial request email** -- dedicated upload folder with
   document checklist and deadline
4. **Receipt confirmation** -- auto-acknowledge each upload with progress: "[X] of [Y]
   required items complete"
5. **Completeness dashboard** -- all clients, percentage received, days to deadline
6. **Follow-up triggers** -- auto-send reminders per escalation cadence (see
   `firm-operations:engagement-management` for communication standards)
7. **Email intake fallback** -- auto-forward rules extract attachments, classify by
   sender/subject, route to portal folder, log source as "email"

### Client Portal Options

Evaluate portal platforms on: permissions granularity, practice management integration,
accounting system connectivity, client branding, and data sovereignty requirements. See
`references/workflow-automation.md` for specific platform comparisons.

## Deadline Tracking Automation

### Calendar Integration

Maintain a master deadline calendar in the practice management system. Each entry
includes: client name, filing type, jurisdiction, due date, responsible preparer,
responsible reviewer. Sync to Google Calendar or Outlook for staff visibility.

### Reminder Sequences by Filing Type

- **Form 1120** -- reminders at 60, 30, 14, and 7 days before due date
- **Estimated tax payments** -- 14 and 7 days
- **1099 filing** -- November (W-9 collection push) + January (filing deadline)
- **Payroll deposits** -- 3 days before each deposit due date

### Escalation Logic

Reminders route to preparer AND engagement manager. Final-week reminders escalate to
partner if the engagement is not yet in review status.

**Status color protocol:**
- **Green** -- filed or in review with adequate time remaining
- **Yellow** -- in preparation, fewer than 14 days to deadline
- **Red** -- not started or blocked, fewer than 7 days -- auto-escalate to partner

## Recurring Task Generation

### Template-Based Task Creation

Define templates per engagement type. Tasks auto-assign based on engagement roster with
load-balancing by current task count.

- **Monthly close** -- reconciliation, AR/AP review, statement preparation, client delivery
- **Quarterly payroll** -- Form 941, state unemployment, payroll reconciliation
- **Annual 1099** -- W-9 collection, vendor review, generation, e-filing
- **Annual tax prep** -- document request through e-filing (full lifecycle)

### Due Date Derivation

Work backward from filing deadline using standard prep timelines:
- Partner review = filing deadline minus 7 days
- Manager review = filing deadline minus 10 days
- Preparation start = filing deadline minus 14 days (adjust for complexity)

## Bank Feed Monitoring

### Daily Review Queue

Bank feeds in the accounting system import transactions automatically from connected
accounts. Generate a daily review queue prioritized by: amount (largest first), then age
(oldest uncategorized first). Queue items include uncategorized transactions, transactions
requiring approval, and transactions flagged by bank rules for manual review. For
platform-specific bank feed configuration, invoke `qbo-integration:qbo-bookkeeping`.

### Auto-Categorization via Bank Rules

Create rules for recurring transactions (rent, utilities, payroll, loan payments,
subscriptions). Rule structure: match on payee name (contains/exact), amount (range),
and/or memo field. Auto-categorize matched transactions to the correct account and class.

**Exclusions from auto-categorization (require human review):**
- Transactions above a firm-defined dollar threshold
- Transactions from vendors not in the approved vendor list
- Transactions to equity or tax-related accounts
- Transactions to accounts not used in the prior 12 months

### Exception Reporting

Flag and route to engagement manager weekly:
- Amounts >$10K (CTR threshold awareness)
- Round-number amounts (potential estimates, not actuals)
- New vendors not on the approved vendor list
- Transactions to dormant accounts (no activity in prior 12 months)
- Transactions posted to prior periods (backdating)

## Reporting Automation

### Scheduled Report Generation

Use the accounting system's reporting API to pull financial statements on schedule
(monthly close + 5 business days). For platform-specific report API parameters, invoke
`qbo-integration:qbo-reporting`. Standard report set per client:
- Profit & Loss -- current month, YTD, prior year comparative
- Balance Sheet -- current month-end
- Cash Flow Statement -- YTD
- AR Aging, AP Aging

Export to PDF or Excel, upload to client portal automatically. Include a variance
summary flagging line items with >10% variance from budget or prior period.

### Anomaly Flagging Thresholds

Route flagged items to engagement manager for review before client delivery:
- Revenue decline >10% month-over-month
- Expense category increase >15%
- Gross margin compression >5 percentage points
- Negative cash flow in a historically positive month

### Auto-Distribution

Upload reports to client portal on schedule, send notification to designated client
contacts. For clients with board reporting requirements, generate a board-ready summary
(key metrics, charts, narrative) timed to the board meeting calendar.

## Build-vs-Buy Decision Framework

**Use native integrations or Zapier/Make** for standard workflows (task creation,
deadline reminders, document routing, report distribution). These cover the majority
of firm automation needs without custom code.

**Custom API development** only when: no existing tool covers the requirement, AND the
volume justifies the investment (>50 clients affected or weekly+ execution frequency).

**Technology stack evaluation criteria:**
- Native accounting system integration available? (preferred over custom API)
- Zapier/Make connector exists? (SaaS-to-SaaS bridge without code)
- API rate limits sufficient for the firm's volume?
- Batch operations supported?
- OAuth token management complexity acceptable?

For platform-specific API rate limits and batch capabilities, invoke
`qbo-integration:qbo-api`.

### Practice Management Tool Capabilities

Evaluate practice management tools on: workflow and task tracking, email integration,
API availability, portal bundling, and Zapier/Make connector support. Prefer native
integrations over custom API work. Use Zapier/Make for SaaS-to-SaaS bridges without
native connectors. See `references/workflow-automation.md` for specific tool comparisons.

## ROI Analysis for Automation Investments

Evaluate each automation candidate on:

1. **Time saved per execution** -- measure current manual process duration, estimate
   automated duration (typically 80-95% reduction for rule-based tasks)
2. **Frequency** -- multiply per-execution savings by annual occurrences
3. **Error reduction** -- estimate current error rate and correction cost; automation
   of deterministic tasks typically eliminates data-entry errors
4. **Implementation cost** -- configuration time for native tools (hours), development
   time for custom integrations (days to weeks), ongoing maintenance
5. **Payback period** -- implementation cost divided by annual time savings valued at
   staff billing rate; target payback under 6 months for Zapier/native, under 12 months
   for custom development

## Staff Development for Automation Adoption

Automation rollouts succeed when staff understand both the tools and the reasoning.
Training must bridge the gap between manual procedures staff already know and the
automated versions replacing them.

### Training by Staff Level

- **Staff Accountant** -- hands-on training: bank rules in the accounting system, portal
  document handling, review queue workflows, time entry in practice management.
  Buddy/mentor assignment for first 90 days covers automation tool orientation alongside
  firm systems.
- **Senior Accountant** -- exception handling: reviewing auto-categorized transactions,
  resolving anomaly flags, managing the document completeness dashboard, running
  automated report packages and reviewing variance summaries before delivery.
- **Manager** -- automation oversight: configuring deadline reminder sequences, designing
  recurring task templates, evaluating bank rule performance, reviewing exception
  reports, managing escalation workflows.
- **Senior Manager/Director** -- strategic evaluation: build-vs-buy decisions, ROI
  analysis, cross-system integration planning, vendor assessment for new automation
  tools.

### Reverse Mentoring for Technology Adoption

Junior staff teach automation and technology skills (bank rule configuration, portal
management, cloud tools) to senior professionals. This accelerates technology adoption
across the firm while building cross-generational respect and giving junior staff
visible leadership opportunities.

### Training Integration with CPE

Align automation training with CPE requirements where qualifying topics overlap (e.g.,
technology in accounting practice). Track credits through the centralized CPE system.
Monthly lunch-and-learn sessions on automation topics can qualify for 1 CPE credit each.

## Security and Compliance for Automated Workflows

### OAuth Token Management

- Store tokens encrypted at rest (AES-256 or equivalent)
- Implement refresh logic with retry and backoff -- many accounting system APIs use
  single-use refresh tokens; a failed refresh invalidates the token
- **Concurrency hazard** -- multiple processes refreshing the same token simultaneously
  cause race conditions. Implement a token lock or serialize refresh operations
- Log all token refresh events for audit trail
- For platform-specific token patterns, invoke `qbo-integration:qbo-api`

### API Rate Limits

- Understand the accounting system's per-company and concurrent request limits
- Implement exponential backoff for HTTP 429 responses
- Batch API calls where possible to reduce request count
- For platform-specific rate limits and batch capabilities, invoke
  `qbo-integration:qbo-api`

### Data Protection Requirements

- TLS 1.2+ for all automated data flows in transit
- Client data at rest encrypted per the firm's Written Information Security Plan (WISP)
- Audit logging: record every automated action (who/what/when/result)
- Principle of least privilege: minimum API permissions per integration; do not share a
  single full-access OAuth token across automations

### Compliance

- Automated client communications must comply with CAN-SPAM and state privacy laws
- Document retention for automated records follows manual record policies
- Client consent for automated processing must be included in the engagement letter

## Decision Points

1. **Webhook or polling?** Webhooks for real-time needs; polling for batch/periodic tasks
2. **Auto-categorize or queue for review?** Auto-categorize only high-confidence recurring
   transactions. Manual review for large, new-vendor, equity, or tax-sensitive items --
   the cost of miscategorization exceeds the time saved
3. **Build or buy?** Native integrations and Zapier/Make for standard workflows. Custom
   API only when volume >50 clients AND no existing tool covers the requirement
4. **Client-facing or internal?** Client-facing automations (portal, emails) must be
   polished, branded, and tested. Internal automations can be more utilitarian
5. **Which portal platform?** Evaluate on permissions, branding, practice management
   bundling, accounting system integration, and data sovereignty requirements
6. **Staff training approach?** Level-appropriate training (operators vs. configurators
   vs. evaluators) with reverse mentoring to accelerate senior adoption

## Supporting References

Read these for deeper detail on specific topics:

- `references/workflow-automation.md` -- Event-driven automation patterns, webhook
  specifications (including QBO-specific details), client portal options, document
  collection workflow stages, deadline tracking sequences, recurring task templates,
  bank feed monitoring rules, reporting automation schedules, practice management tool
  integrations, and security/compliance requirements for automated data flows. Read when
  configuring a specific webhook, designing bank rules, setting up report distribution
  schedules, evaluating API rate limits, or reviewing OAuth token management patterns.
- `references/staff-development.md` -- Career ladder structure (Staff through Partner),
  competency framework by level, performance review cadence, technical training programs
  (new hire onboarding, annual tax update, software training, specialization tracks),
  CPE integration, mentorship models (formal, peer, reverse), busy season management
  (overtime thresholds, comp time, wellness), retention strategies, remote/hybrid
  policies, and succession planning. Read when designing training programs for automation
  rollouts, planning staff assignments by competency level, structuring mentorship for
  technology adoption, or evaluating workload impact of automation changes.

## Cross-Plugin References

For workflow context and engagement triggers:
- Invoke `firm-operations:practice-management` for deadline calendar management, staff
  assignment, workload balancing, and capacity planning that feeds automation scheduling
- Invoke `firm-operations:engagement-management` for engagement lifecycle events that
  trigger automation workflows (onboarding, document requests, billing milestones,
  client communications standards)
- Invoke `firm-operations:quality-compliance` for quality review procedures that interact
  with automated task generation and escalation
- Invoke `firm-operations:data-governance` for data security policies governing automated
  data flows and client portal configuration

For platform-specific automation implementation:
- Invoke `qbo-integration:qbo-api` for accounting system API details, OAuth patterns,
  rate limits, and batch operations used by webhook handlers and report automation
- Invoke `qbo-integration:qbo-bookkeeping` for bank feed processing and bank rule
  configuration in the accounting system
- Invoke `qbo-integration:qbo-reporting` for reporting API parameters and available
  report types used in scheduled report generation

For service-line automation patterns:
- Invoke `bookkeeping:transaction-processing` for transaction categorization workflows
  that bank feed automation supports
- Invoke `bookkeeping:reconciliation` for reconciliation procedures that pair with
  automated bank feed monitoring
- Invoke `bookkeeping:monthly-close` for month-end close tasks driven by recurring
  task templates
- Invoke `tax-prep:e-filing` for e-filing task automation and status tracking workflows
- Invoke `tax-prep:tax-compliance` for tax deadline integration with reminder sequences

## Cross-Plugin Consumers

- `tax-prep:e-filing` -- e-filing task automation and acknowledgment status tracking
- `bookkeeping:transaction-processing` -- bank feed automation patterns and categorization rules
- `bookkeeping:monthly-close` -- recurring task templates for close procedures
- `firm-operations:practice-management` -- automated task generation feeding workflow assignment
- `firm-operations:engagement-management` -- automation triggers from engagement milestones
