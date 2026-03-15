# Practice Management

**Purpose:** Define workflow management principles, engagement lifecycle, and operational cadences for an accounting firm practice.

**Skills that reference this:** engagement-management, capacity-planning, workflow-design

---

## Engagement Lifecycle

### Phase 1: Onboarding
- Client acceptance evaluation complete (see engagement-letters.md)
- Engagement letter signed and filed
- Client added to practice management system with correct entity type, tax year, and service codes
- Prior-year returns and financial data obtained and reviewed
- Access credentials obtained (accounting system login, bank portal, prior preparer authorization)
- Assign engagement team: preparer, reviewer, engagement partner
- Create engagement folder structure (electronic)
- Set internal deadlines (working backward from due date)

### Phase 2: Setup
- Create or verify chart of accounts in accounting software
- Set up engagement checklist/template for the return type
- Import prior-year data where applicable (trial balance, carryforward amounts)
- Identify information gaps and issue initial client request list (PBC list)
- Schedule client kick-off communication (especially for new engagements or complex returns)

### Phase 3: Execution
- Collect and organize source documents as received
- Track PBC list completion; follow up on outstanding items at defined intervals
- Prepare workpapers, lead schedules, and supporting computations
- Draft the return or financial statements
- Document assumptions, judgments, and positions taken
- Flag items for reviewer attention (uncertain positions, unusual transactions, estimates)

### Phase 4: Review
- First-level review (senior/manager): technical accuracy, completeness, tie-out to source data
- Second-level review (partner/engagement quality): overall reasonableness, risk positions, presentation
- Review notes documented and resolved by preparer
- Quality checkpoints: all open items resolved before advancing to delivery
- Tax-specific: verify e-file diagnostics are clear, compare to prior year for anomalies

### Phase 5: Delivery
- Generate final return/deliverable in client-ready format
- Prepare cover letter summarizing key items, tax due, and action items
- Obtain client approval and e-file authorization (Form 8879 for e-file)
- E-file or paper-file the return
- Deliver client copy via secure portal or encrypted transmission
- Issue invoice per engagement letter terms

### Phase 6: Archive
- Finalize and lock workpaper file
- Store signed engagement letter, e-file authorization, and filed return copy
- Update practice management system: mark engagement complete, record actual time and fees
- Post-engagement assessment: note lessons learned, client-specific issues for next year
- Set reminder for next-year engagement start date

## Workflow Design Principles

### Single Source of Truth
- All engagement status tracked in one system (practice management software)
- Avoid parallel tracking in email, spreadsheets, and sticky notes
- Status categories should be consistent firm-wide (e.g., Not Started, In Progress, In Review, Ready to Deliver, Complete)

### Standardized Templates
- Every engagement type has a standard checklist and workpaper template
- Templates enforce completeness: preparer cannot skip steps
- Templates updated annually for law changes, form revisions, and lessons learned from prior season

### Clear Handoff Points
- Defined triggers for status transitions (e.g., "In Review" means all workpapers complete, no open PBC items)
- Handoff includes a brief note to the reviewer on key items and risk areas
- Reviewer acknowledges receipt and sets their own completion target

### Minimize Work-in-Progress
- Focus on completing engagements rather than starting many in parallel
- A return that is 90% done but waiting on one document is less valuable than one that is 100% done and filed
- Set WIP aging targets: escalate engagements that exceed expected duration

## Capacity Planning

- **Inventory:** Count total engagements by type and complexity tier (simple, moderate, complex)
- **Budget hours:** Assign expected hours per engagement tier (e.g., simple 1120 = 8 hrs, complex = 40 hrs)
- **Available capacity:** Calculate available professional hours per period (total hours minus admin, CPE, PTO)
- **Load balancing:** Distribute engagements across team members to equalize utilization
- **Surge planning:** Identify periods of peak demand (Jan 15 - Apr 15 for calendar-year returns) and plan accordingly (overtime, seasonal staff, extended deadlines)
- **Extension strategy:** Proactively extend returns when capacity is insufficient to ensure quality rather than rushing

## Staff Assignment Criteria

- **Competency match:** Assign staff with relevant experience to engagement type (e.g., corporate tax experience for Form 1120)
- **Development opportunity:** Pair less experienced staff with mentors on moderate-complexity engagements
- **Continuity:** Same team on recurring engagements year-over-year for efficiency and institutional knowledge
- **Independence:** Verify no conflicts of interest exist between staff and client
- **Workload balance:** No single person should be the bottleneck for more than a defined percentage of engagements

## Quality Checkpoints

Embedded at each lifecycle phase transition:

- **Onboarding to Setup:** Engagement letter signed? Prior data obtained? Team assigned?
- **Setup to Execution:** PBC list issued? System setup verified? Carryforwards imported?
- **Execution to Review:** All workpapers complete? PBC items resolved? Diagnostics clear?
- **Review to Delivery:** Review notes resolved? Partner sign-off? E-file authorization obtained?
- **Delivery to Archive:** Return filed? Invoice sent? File locked?

## Client Communication Cadence

- **Engagement start:** Kick-off email with timeline, PBC list, and key deadlines
- **Document collection:** Follow-up at 2 weeks, 4 weeks, and 6 weeks if items outstanding
- **Status update:** Notify client when return enters review phase (sets expectation for delivery timeline)
- **Delivery:** Cover letter with action items and payment instructions
- **Post-filing:** Confirmation of acceptance (e-file acknowledgment) shared with client
- **Off-season:** Annual check-in for continuance, planning opportunities, fee discussion

## Deadline Tracking

- Maintain a master deadline calendar covering all clients and all jurisdictions
- Deadlines include: original due date, extended due date, estimated payment dates, state-specific dates
- Set internal deadlines 7-14 days before external deadlines
- Automated reminders at defined intervals (30 days, 14 days, 7 days, 3 days before internal deadline)
- Missed-deadline protocol: immediate escalation to engagement partner, assess filing options, document

## Workload Balancing Across Tax Seasons

- **January - March:** Focus on data collection, W-2/1099 processing, entity return preparation (Form 1120 due April 15)
- **April 1-15:** Final push for original filings; extension filing for returns not ready
- **May - September:** Extended return preparation, tax planning, bookkeeping catch-up, CPE
- **October 1-15:** Final push for extended returns
- **November - December:** Year-end planning, engagement letter renewals, system updates, internal training

## Technology Stack Considerations

- **Practice management:** Tracks engagements, deadlines, staff assignment, time/billing
- **Document management:** Secure storage, version control, client portal integration
- **Tax preparation software:** Return preparation, e-filing, diagnostics
- **Accounting software:** Cloud or desktop accounting platform for bookkeeping engagements (for QBO-specific procedures, see qbo-integration plugin)
- **Communication:** Encrypted email, client portal for document exchange
- **Workflow automation:** Automated status updates, reminders, PBC tracking
- Integration between systems minimizes double-entry and manual tracking errors

## Accounting System Integration Notes

- The accounting system serves as both the platform and a workflow artifact: engagement execution produces journal entries, reconciliations, and reports
- Accounting system subscription ownership should be documented in the engagement letter (client-owned vs. firm-owned)
- Closing date and password controls enforce the review-to-archive handoff (lock completed periods)
- Export trial balance and financial statements from the accounting system as part of the archive phase
- For multi-year reconstruction projects, period-locking sequence must follow the pipeline stage order
- For QBO-specific procedures (Accountant tools, multi-client management, closing date controls), see qbo-integration plugin

## Decision Points

- **Single vs. multiple workflow tools:** Consolidate to one practice management system rather than fragmenting across email, spreadsheets, and project boards. The cost of context-switching exceeds the cost of a purpose-built tool.
- **Extension-first vs. on-time-first:** For firms with more engagements than capacity, an extension-first strategy (extend all, then file in order of complexity) often produces higher quality than rushing returns before April 15.
- **Fixed team vs. rotating assignments:** Fixed teams build institutional knowledge and efficiency; rotating assignments build bench strength and reduce key-person risk. Balance based on firm size.
- **Client portal adoption:** Require clients to use a secure portal for document exchange. Email-based document collection is a security risk and an efficiency drag.

## Authoritative References

- AICPA SQMS No. 1 (engagement performance component)
- AICPA Practice Management resources
- IRS Circular 230 (duties and restrictions relating to practice before the IRS)
- Florida Board of Accountancy, FAC 61H1 (supervision requirements)
