# Internal Controls — COSO 2013 Framework

Guidance for designing, implementing, and evaluating internal controls using the COSO 2013 framework, with focus on small/mid C-corporations.

**Tier**: 3 (evergreen framework — COSO 2013 is the current authoritative version)

**Skills**: `bookkeeping-review`, `financial-statement-preparation`, `engagement-management`, `audit-support`

## Contents

- [Overview](#overview)
- [Five Components and 17 Principles](#five-components-and-17-principles)
- [Small Entity Considerations](#small-entity-considerations)
- [Common Controls for C-Corps](#common-controls-for-c-corps)
- [IT General Controls](#it-general-controls)
- [Segregation of Duties](#segregation-of-duties)
- [Documenting Controls](#documenting-controls)
- [Testing Effectiveness](#testing-effectiveness)
- [Deficiency Classification](#deficiency-classification)
- [Accounting System Controls](#accounting-system-controls)
- [Decision Points](#decision-points)
- [References](#references)

---

## Overview

The Committee of Sponsoring Organizations of the Treadway Commission (COSO) published the updated Internal Control — Integrated Framework in 2013, superseding the original 1992 framework. It is the most widely accepted framework for designing and evaluating internal controls over financial reporting (ICFR) in the United States.

Internal control is defined as a process, effected by an entity's board of directors, management, and other personnel, designed to provide reasonable assurance regarding the achievement of objectives in three categories:

- **Operations**: effectiveness and efficiency of operations
- **Reporting**: reliability of financial reporting
- **Compliance**: compliance with applicable laws and regulations

The framework consists of five interrelated components supported by 17 principles. All five components and all relevant principles must be present and functioning for the system of internal control to be effective.

---

## Five Components and 17 Principles

### Control Environment

The foundation — sets the tone of the organization and establishes the basis for all other components.

1. **Integrity and ethical values**: management demonstrates commitment to integrity and ethical values through actions, policies, and culture
2. **Board oversight**: the board of directors (or equivalent oversight body) demonstrates independence from management and exercises oversight of internal control development and performance
3. **Organizational structure**: management establishes reporting lines, authority, and responsibility in pursuit of objectives — clear organizational chart, defined roles
4. **Competence commitment**: the organization demonstrates commitment to attract, develop, and retain competent individuals aligned with objectives — hiring, training, performance evaluation
5. **Accountability**: the organization holds individuals accountable for their internal control responsibilities — performance measures, incentives, consequences

### Risk Assessment

The process of identifying and analyzing risks to achieving objectives, forming the basis for how risks should be managed.

6. **Specify objectives**: the organization specifies objectives with sufficient clarity to enable identification and assessment of risks — financial reporting objectives must be consistent with the applicable framework (GAAP, tax basis, etc.)
7. **Identify and analyze risks**: the organization identifies risks to achieving objectives across the entity and analyzes risks to determine how they should be managed — consider both internal and external factors
8. **Assess fraud risk**: the organization considers the potential for fraud in assessing risks — fraudulent reporting, misappropriation of assets, corruption. Addresses incentive/pressure, opportunity, and rationalization (fraud triangle)
9. **Identify and analyze significant changes**: the organization identifies and assesses changes that could significantly impact the system of internal control — new business lines, acquisitions, IT system changes, regulatory changes, key personnel turnover

### Control Activities

The actions established by policies and procedures that help ensure management directives to mitigate risks are carried out.

10. **Select and develop control activities**: the organization selects and develops control activities that contribute to mitigating risks to acceptable levels — preventive and detective controls addressing identified risks
11. **Select and develop IT general controls**: the organization selects and develops general control activities over technology to support the achievement of objectives — access controls, change management, operations, program development
12. **Deploy through policies and procedures**: the organization deploys control activities through policies that establish expectations and procedures that put policies into action — documented, communicated, consistently applied

### Information and Communication

The information necessary to carry out internal control responsibilities and the communication channels to support it.

13. **Use relevant, quality information**: the organization obtains or generates and uses relevant, quality information to support the functioning of internal control — timely, accurate, complete, accessible
14. **Communicate internally**: the organization internally communicates information, including objectives and responsibilities for internal control, necessary to support the functioning of internal control — upward, downward, and across the organization
15. **Communicate externally**: the organization communicates with external parties regarding matters affecting the functioning of internal control — regulators, auditors, customers, vendors, shareholders

### Monitoring Activities

Assessments to ascertain whether the components of internal control are present and functioning over time.

16. **Select, develop, and perform ongoing and/or separate evaluations**: the organization selects, develops, and performs evaluations to ascertain whether the components of internal control are present and functioning — ongoing monitoring (embedded in processes) and separate evaluations (periodic assessments)
17. **Evaluate and communicate deficiencies**: the organization evaluates and communicates internal control deficiencies in a timely manner to parties responsible for taking corrective action, including senior management and the board as appropriate

---

## Small Entity Considerations

The COSO framework applies to entities of all sizes, but implementation looks different in smaller organizations:

- **Less formal documentation**: policies may be communicated verbally or through demonstrated behavior rather than written manuals. The key is that controls exist and function, not that they are formally documented.
- **Owner-manager involvement**: in small entities, the owner-manager often directly performs or closely oversees key controls. This involvement can be an effective control itself but creates concentration risk if the owner-manager is the source of fraud risk.
- **Fewer personnel**: makes traditional segregation of duties difficult or impossible. Compensating controls (owner review, independent reconciliation, external oversight) become critical.
- **Simpler IT environment**: may use a single cloud-based accounting system with limited customization. IT general controls focus on access management, backup, and data integrity rather than complex change management processes.
- **Proportionate documentation**: documentation requirements scale with entity size. A small entity does not need the same level of formal documentation as a public company, but must document enough to demonstrate controls are designed and operating.
- **Board oversight**: many small C-corps lack a formal independent board. Owner oversight, advisory boards, or engagement of external advisors (CPA, attorney) can serve as compensating mechanisms.
- **Culture over documentation**: in small entities, the control environment often depends more on the tone set by ownership than on written policies. Ethical culture, demonstrated integrity, and clear expectations are the primary control environment elements.

---

## Common Controls for C-Corps

### Detective Controls (Identify errors or irregularities after they occur)

- **Bank reconciliation**: monthly reconciliation of all bank accounts to the GL by someone other than the person who records transactions or handles cash (see `bk/bank-reconciliation.md`)
- **Monthly financial review by management**: owner or manager reviews P&L and balance sheet, investigates variances to budget or prior period (see `bk/monthly-close-sop.md`)
- **Accounts receivable aging review**: periodic review of outstanding receivables to identify collection issues, unauthorized write-offs, or lapping
- **Physical inventory counts**: periodic (at least annual) physical counts compared to perpetual records; investigate and adjust variances
- **Vendor statement reconciliation**: compare vendor statements to AP records to detect missed invoices or duplicate payments
- **Payroll review**: review of payroll register by someone not involved in payroll processing — verify employees, rates, hours, deductions

### Preventive Controls (Prevent errors or irregularities before they occur)

- **Approval limits for expenditures**: defined thresholds requiring management approval (e.g., purchases >$500 require owner sign-off, >$5,000 require two approvals)
- **Dual signatures on checks**: require two signatures on checks above a threshold (e.g., $2,500 or $5,000)
- **Authorized signer list**: maintain a current list of persons authorized to sign checks, approve expenditures, and execute contracts
- **Purchase order system**: require POs for purchases above a threshold to create an audit trail and authorization record
- **New vendor approval**: require approval before adding new vendors to the system to prevent fictitious vendor fraud
- **Access controls to accounting system**: limit system access by role; restrict journal entry posting to authorized personnel
- **Backup procedures**: regular automated backups of accounting data (cloud systems typically handle backup automatically; for desktop systems, verify backup schedule)

---

## IT General Controls

IT general controls (ITGCs) support the reliability of information produced by IT systems. For small C-corps using cloud-based accounting systems, the scope is narrower than for entities with on-premises systems.

### Access Management

- **User provisioning**: grant access based on job responsibilities; assign appropriate system roles (administrator, accountant, standard user, reports-only)
- **Termination procedures**: revoke access promptly when employees leave or change roles
- **Password policies**: enforce strong passwords; enable two-factor authentication (2FA) on the accounting system and banking portals
- **Periodic access reviews**: review user lists quarterly to verify appropriateness of access levels

### Change Management

- **Chart of accounts changes**: require approval before adding, modifying, or deactivating accounts to prevent unauthorized GL structure changes
- **System configuration changes**: document changes to accounting system settings (company info, accounting preferences, tax settings, automation rules)
- **Integration changes**: review and approve connections to third-party apps (payment processors, payroll, bank feeds)

### Operations

- **Data backup**: cloud-based accounting systems typically provide continuous backup. For desktop installations, verify backup is performed and tested regularly.
- **Disaster recovery**: document access procedures if primary systems are unavailable (accounting system login credentials stored securely, backup contacts for IT support)

### Audit Log

The accounting system should maintain an audit log tracking: user, date/time, action (created, modified, deleted), entity affected. Use for monitoring and investigation.

---

## Segregation of Duties

The principle of segregation requires that no single person should control all aspects of a transaction. Four functions should be separated where possible:

1. **Authorization**: approving transactions (e.g., approving a purchase order, authorizing a wire transfer)
2. **Custody**: physical control of assets (e.g., handling cash, holding inventory, signing checks)
3. **Recording**: entering transactions into the accounting system (e.g., posting journal entries, recording deposits)
4. **Reconciliation**: verifying recorded transactions against independent records (e.g., bank reconciliation, inventory counts)

### Small Entity Compensating Controls

When segregation is not possible due to limited staff:

- **Owner review**: the owner-manager reviews all bank statements, canceled checks, and monthly financial statements independently. This is the single most important compensating control in a small entity.
- **Bank statement review**: owner receives bank statements (paper or electronic) directly and reviews for unusual transactions before handing to the bookkeeper
- **Surprise audits**: periodic unannounced reviews of cash, inventory, or expense reports
- **External reconciliation**: engage an outside accountant to perform monthly or quarterly bank reconciliations
- **Dual control over cash**: require two people for cash counts, bank deposits, and safe access
- **Rotation of duties**: periodically rotate responsibilities to prevent concealment of irregularities and to cross-train staff
- **Mandatory vacations**: require employees in sensitive positions to take consecutive time off, allowing others to perform their duties and potentially uncover irregularities

---

## Documenting Controls

### Methods

- **Narrative descriptions**: written descriptions of processes, identifying who does what, when, and what controls exist at each step. Most common for small entities.
- **Flowcharts**: visual representation of transaction flow from initiation to recording, with control points marked. Useful for complex processes.
- **Control matrices**: structured format mapping financial statement assertions to risks to controls to test procedures. Used in formal assessments.

### Control Matrix Structure

For each significant process (revenue, purchasing, payroll, treasury):

- **Assertion**: what the financial statement claims (existence, completeness, accuracy, cutoff, classification, rights/obligations, presentation)
- **Risk**: what could go wrong (fictitious revenue, unrecorded liabilities, misclassified expenses)
- **Control**: the activity that mitigates the risk (approval required, reconciliation performed, segregation of duties)
- **Frequency**: how often the control operates (per transaction, daily, weekly, monthly, quarterly, annually)
- **Control owner**: who performs the control
- **Evidence**: what documentation the control produces (signed approval, reconciliation report, exception log)

---

## Testing Effectiveness

Four methods for evaluating whether controls are operating as designed:

1. **Inquiry**: ask personnel who perform the control about their procedures, how they handle exceptions, and what they do if the control identifies an issue. Inquiry alone is generally insufficient.
2. **Observation**: watch the control being performed in real time. Provides evidence for a single point in time only.
3. **Inspection of documentation**: examine the evidence produced by the control — signed approvals, completed reconciliations, exception reports, audit logs. Most common method for detective controls.
4. **Reperformance**: independently re-execute the control to verify it produces the expected result. Most reliable method but also most resource-intensive.

Effective testing typically combines methods: inquiry + inspection (most common), inquiry + observation, or inquiry + reperformance for high-risk areas.

---

## Deficiency Classification

Internal control deficiencies are classified by severity:

- **Deficiency**: a control is designed, implemented, or operated in a way that does not allow management or employees to prevent or detect misstatements on a timely basis. Exists when a control is missing or not operating effectively, but the risk of material misstatement is low.
- **Significant deficiency**: a deficiency, or combination of deficiencies, that is less severe than a material weakness yet important enough to merit attention by those charged with governance. Indicates a reasonable possibility that a more-than-inconsequential misstatement will not be prevented or detected.
- **Material weakness**: a deficiency, or combination of deficiencies, such that there is a reasonable possibility that a material misstatement of the financial statements will not be prevented or detected on a timely basis. Requires disclosure and remediation.

### Communication Requirements

- **Audit engagements (AU-C 265)**: significant deficiencies and material weaknesses must be communicated in writing to management and those charged with governance
- **Review engagements (AR-C 90)**: no specific requirement to evaluate or report on internal controls, but the accountant should communicate any material weaknesses discovered during the review
- **Compilation and preparation**: no internal control evaluation or communication requirement, but the accountant should consider communicating deficiencies discovered in the course of the engagement

---

## Accounting System Controls

Cloud-based accounting systems typically provide built-in features that support internal controls:

- **User roles**: assign role-based access with the minimum permissions necessary for each user's responsibilities (e.g., administrator, standard user, reports-only)
- **Audit log**: review the system audit log that tracks all user activity — who created, modified, or deleted transactions. The log should be immutable (not editable or deletable by any user). Use for monitoring and investigation.
- **Period lock**: set a closing date and password to prevent unauthorized changes to prior periods. Configure separate lock dates for accountant vs. client access if the system supports it.
- **Two-factor authentication**: enable for all users, especially those with administrative or accountant-level roles
- **Bank feed rules**: automate categorization but review for accuracy. Overly broad rules can miscategorize transactions. Periodically audit rules for continued appropriateness.
- **Approval workflows**: use system-provided approval workflows for purchase orders and expenses where available; otherwise use manual approval processes with documented evidence
- **Granular permissions**: where the system supports it, configure granular control over what users can view, create, edit, and delete in each functional area (sales, expenses, banking, payroll, reports)
- **Activity feed**: review recent activity across all users as a quick visual check for unexpected entries

For platform-specific execution, invoke `qbo-integration:qbo-bookkeeping`.

---

## Decision Points

- **Framework selection** — COSO 2013 is the default for US entities. Smaller entities may use the AICPA's "Internal Control — A Tool for the Audit Committee" for a simplified approach. COBIT is used for IT-focused assessments.
- **Scope of assessment** — for small C-corps not subject to SOX, focus on high-risk areas: cash and treasury, revenue recognition, significant estimates, related party transactions, and payroll. Full-scope assessment is typically not cost-justified.
- **Formality of documentation** — scale to entity size and risk. A small single-location C-corp with simple operations may document controls through brief narratives. A multi-location entity with complex transactions needs control matrices and flowcharts.
- **Segregation of duties infeasible** — when staff size prevents proper segregation, document the compensating controls in place (owner review, external reconciliation). Acknowledge the residual risk to governance.
- **IT controls scope** — for cloud accounting system users, the cloud provider manages infrastructure, application, and database controls. Entity-level IT controls focus on access management, configuration, and integration management. No need to test data center controls.
- **Frequency of monitoring** — ongoing monitoring (embedded in daily operations) is preferred over separate evaluations (periodic assessments). For small entities, monthly owner review of financial statements combined with accounting system audit log review provides adequate monitoring.
- **Remediation priority** — material weaknesses require immediate remediation. Significant deficiencies should be addressed within a reasonable timeframe (one to two quarters). Deficiencies should be tracked and addressed as resources allow.
- **Cost-benefit** — controls must be cost-justified. A control that costs more to implement than the risk it mitigates is not reasonable. Small entities should focus on high-impact, low-cost controls (bank reconciliation, owner review, access restrictions).

---

## References

- COSO. "Internal Control — Integrated Framework." Committee of Sponsoring Organizations of the Treadway Commission, 2013.
- COSO. "Internal Control over Financial Reporting — Guidance for Smaller Public Companies." 2006 (supplemental guidance applicable to smaller entities generally).
- AICPA. AU-C Section 265, Communicating Internal Control Related Matters Identified in an Audit.
- AICPA. AU-C Section 315, Understanding the Entity and Its Environment and Assessing the Risks of Material Misstatement.
- AICPA. "Audit Guide: Assessing and Responding to Audit Risk in a Financial Statement Audit."
- ISACA. COBIT 2019 Framework (IT governance and controls).
