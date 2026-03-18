---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Data Security

**Purpose:** Document IRS requirements and FTC Safeguards Rule obligations for protecting taxpayer data in tax and accounting practices, including WISP requirements.

**Skills that reference this:** wisp-management, security-compliance, data-protection

---

## Legal Framework

### FTC Safeguards Rule (Gramm-Leach-Bliley Act)
- The Gramm-Leach-Bliley Act (GLBA) classifies tax return preparers as "financial institutions"
- The FTC Safeguards Rule requires financial institutions to develop, implement, and maintain a written information security plan
- Applies to all tax preparers regardless of firm size -- sole practitioners are not exempt
- Failure to comply may result in an FTC investigation and enforcement action
- Security events affecting 500 or more people must be reported to the FTC within 30 days of discovery

### FTC Safeguards Rule Requirements
- Designate a qualified individual to oversee the information security program
- Identify and assess risks to customer information in every area of operations
- Design and implement a safeguards program; regularly monitor and test it
- Implement multi-factor authentication for anyone accessing customer information on the system (required for all companies regardless of size)
- Select service providers that can maintain appropriate safeguards; contractually require them to do so
- Evaluate and adjust the program for changes in business, operations, or security testing results
- Provide security awareness training and schedule regular refreshers

### IRC Section 7216
- Criminal penalties for unauthorized disclosure or use of tax return information
- Tax preparers may not disclose or use tax return information for any purpose other than preparing the return, unless the taxpayer consents in writing
- Penalties: up to $1,000 fine and/or up to 1 year imprisonment per violation

### IRS Publication 1345
- Handbook for Authorized IRS e-File Providers
- Establishes six security and privacy standards for online providers
- E-file providers must adhere to these in addition to the FTC Safeguards Rule

## Written Information Security Plan (WISP)

### Requirement
- The law requires every tax preparer to create and maintain a WISP
- The WISP must be written and accessible
- It must be appropriate to the firm's size, scope, complexity, and sensitivity of data handled
- IRS Publication 5708 provides a template specifically for tax and accounting practices

### WISP Outline (per IRS Publication 5708)

**I. Objectives, Purpose, and Scope**
- Objective statement: legal obligations under GLBA and FTC Safeguards Rule
- Purpose statement: what and how taxpayer information is being protected
- Scope statement: sets the limits and intent of the plan

**II. Designated Qualified Individual**
- Data Security Coordinator (DSC): responsible for implementing, supervising, and maintaining the WISP
- Public Information Officer (PIO): designated spokesperson for breach communications
- In small firms, the owner may serve in both roles
- Document responsibilities of each role

**III. Risk Assessment**
- List all types of information the practice handles (taxpayer PII, employee data, business financial data)
- Identify potential sources of loss: internal (employee error, theft) and external (hacking, phishing, physical theft, natural disaster)
- Assess potential damage from each risk
- Outline procedures to monitor and test for new risks

**IV. Hardware Inventory**
- Catalog all devices that store or process taxpayer data
- Include: computers, laptops, phones, tablets, printers, routers, external drives, USB devices
- Record description, physical location, and types of information stored/processed by each device

**V. Safety Measures (Policies)**
- Data collection and retention policy
- Data disclosure policy
- Network protection policy
- User access control policy
- Electronic data exchange policy
- Wi-Fi access policy
- Remote access policy
- Connected devices policy
- Reportable incidents policy
- Employee code of conduct

**VI. Implementation Clause**
- Date of implementation, firm name
- Compliance statement referencing GLBA and FTC Safeguards Rule
- Signatures of principal and DSC

**VII. Attachments**
- Attachment A: Record Retention Policies
- Attachment B: Rules of Behavior and Conduct Safeguarding Client PII
- Attachment C: Security Breach Procedures and Notifications
- Attachment D: Employee/Contractor Acknowledgement of Understanding
- Attachment E: Firm Hardware Inventory containing PII Data
- Attachment F: Firm Employees Authorized to Access PII

## Protect Your Clients; Protect Yourself (IRS Pub 4557)

### Basic Security Steps
- Install anti-malware/anti-virus on all devices; set to auto-update
- Use strong passwords: minimum 8 characters (16 for admin accounts), mix of letters/numbers/symbols, unique per account
- Implement multi-factor authentication for all system access
- Encrypt all sensitive files and emails containing PII
- Back up data to a secure external source not connected full-time to the network
- Review return information (especially direct deposit) before e-filing
- Wipe or destroy old devices before disposal
- Limit data access to need-to-know personnel
- Monitor EFIN/PTIN accounts weekly for unauthorized filings
- Withdraw POA/CAF for former clients
- Implement audit trails recording who performed what activity and when
- Maintain a clean desk policy

### Security Software Required
- Anti-virus (prevents malware)
- Anti-spyware (prevents unauthorized data collection)
- Firewall (blocks unwanted connections)
- Drive encryption (protects data on lost/stolen devices)
- Set all security software to update automatically
- Keep browsers up to date

### Wireless Network Security
- Change default router admin password
- Reduce wireless broadcast power to minimum needed
- Change SSID to non-identifying name; disable SSID broadcast
- Use WPA-3 encryption (never WEP)
- Do not use public Wi-Fi for business activities
- Require VPN for remote access
- Consider firm-issued laptops for remote workers

### Protecting Stored Client Data
- Back up encrypted copies to external drives or cloud storage; encrypt before uploading
- Use drive encryption on all devices
- Do not attach external drives with client data to public computers
- Do not install unauthorized software
- Inventory all devices storing client data and all software processing client data
- Limit/disable internet access on devices storing taxpayer data
- Delete data from devices/drives before disposal: use software shredders or physically destroy

## Detecting and Responding to Data Theft

### Signs of Data Theft
- Client e-filed returns rejected because SSN was already filed
- Clients receiving IRS authentication letters (5071C, 4883C, 5747C) without filing
- Clients who haven't filed receiving refunds or transcripts
- IRS online account notices about unauthorized access
- EFIN/PTIN filing counts exceeding actual filings
- Clients responding to emails the firm didn't send
- Slow/unusual computer behavior, unexpected lock-outs

### Reporting a Data Loss
- **IRS:** Report to local IRS Stakeholder Liaison immediately
- **FBI:** Contact local field office if directed by IRS
- **Local police:** File a police report
- **State tax agencies:** Visit Federation of Tax Administrators for state breach contacts
- **FTC:** Report at IdentityTheft.gov; required notification for events affecting 500+ people within 30 days
- **Security expert:** Engage to determine cause, scope, and remediation
- **Insurance carrier:** Report breach to professional liability/cyber insurance carrier

### Recovery Steps
- Update IRS Stakeholder Liaison with developments
- Review FTC's "Data Breach Response: A Guide for Business"
- Determine how intrusion occurred; fix vulnerabilities before resuming operations
- May need new EFIN before resuming e-filing
- Develop a business continuity plan
- Make full backups of all business data; encrypt backup files
- Consider monthly backups (more frequent during filing season)

## Annual Review Requirements

- Review and update the WISP at least annually
- Review when any material business change occurs (new office, new staff, new technology)
- Update hardware inventory
- Verify all employees have completed annual security training
- Update Attachment F (authorized access list) for personnel changes
- Test incident response procedures
- Review security software and update as needed
- Document the review and any changes made

## PII Definition (per IRS Pub 5708)

PII means information containing first name and last name (or first initial and last name) of a taxpayer, spouse, dependent, or legal guardianship person in combination with any of:
- Social Security number, date of birth, or employment data
- Driver's license or state-issued ID number
- Income data, tax filing data, retirement plan data, asset ownership data, investment data
- Financial account numbers, credit/debit card numbers, access codes, or passwords
- Email addresses, non-listed phone numbers, residential or mobile contact information

PII does NOT include information obtained from publicly available sources (phone directories, public government records).

## Decision Points

- **WISP template vs. custom:** Small firms should start with the IRS Pub 5708 template and customize. Larger firms may need a more comprehensive plan.
- **Cloud vs. on-premise security:** Cloud solutions (cloud accounting platforms, cloud document management) shift some security responsibility to the vendor, but the firm must verify vendor compliance and maintain its own access controls.
- **Cyber insurance:** Consider separate cyber liability insurance in addition to professional liability E&O. Cyber policies cover breach response costs, notification expenses, and regulatory fines.
- **Employee training frequency:** IRS recommends at least annual training; quarterly security reminders during filing season are best practice.

## Authoritative References

- IRS Publication 4557: Safeguarding Taxpayer Data -- A Guide for Your Business
- IRS Publication 5708: Creating a Written Information Security Plan for Your Tax & Accounting Practice (Rev. 8-2024)
- IRS Publication 1345: Handbook for Authorized IRS e-File Providers
- FTC Safeguards Rule (16 CFR Part 314)
- Gramm-Leach-Bliley Act (15 U.S.C. 6801-6809)
- IRC Section 7216 (Disclosure or Use of Information by Preparers of Returns)
- NIST SP-800-18, NISTIR 7621 (Small Business Information Security)
- FTC: Data Breach Response -- A Guide for Business
- IRS Data Theft Information for Tax Professionals: irs.gov/newsroom/data-theft-information-for-tax-professionals
