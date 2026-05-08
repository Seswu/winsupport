# SOP Categories Relevant to the Support Function

This document categorizes the types of Standard Operating Procedures (SOPs) a support person in a pharmaceutical company will encounter. Use it to understand what SOPs exist, what each covers, and where to find them.

## 1. Core Support SOPs

These govern the daily operation of the service desk and define how support work is managed and documented.

### Incident Management SOP

- **What it covers:** The lifecycle of an incident ticket — logging, categorization, prioritization, initial triage, investigation, resolution, and closure.
- **Typical elements:**
  - Priority matrix (severity + impact → P1/P2/P3/P4)
  - Response and resolution SLAs per priority level
  - When to escalate between tiers
  - Major incident procedure (bridge calls, executive notification)
  - Definition of "resolved" vs. "closed"
- **Required documentation:** Ticket fields (description, impacted user/system, category, priority, steps taken, resolution, closure code).
- **Approval gates:** P1/P2 incidents may require manager notification and post-incident review.

### Request Fulfillment SOP

- **What it covers:** Standard service requests — things that are not incidents (password resets, new account creation, software requests, hardware requests).
- **Typical elements:**
  - List of standard requests and their fulfillment paths
  - Self-service vs. assisted fulfillment
  - Authorization requirements per request type
  - Service catalog (what is offered, what is not)
- **Required documentation:** Request form, approval confirmation, completion verification.

### Service Desk Operations SOP

- **What it covers:** Day-to-day desk operations — shift scheduling, handover procedures, queue management, break coverage, communication standards.
- **Typical elements:**
  - Handover format (what must be communicated at shift change)
  - Queue monitoring cadence
  - Expected response time for ticket updates
  - Professional communication standards (tone, signature blocks, escalation language)

### Knowledge Management SOP

- **What it covers:** How knowledge articles are created, reviewed, approved, and retired.
- **Typical elements:**
  - Article template (symptom, cause, resolution, keywords)
  - Review cycle (who approves new articles, how often they are refreshed)
  - How to identify when a new article is needed
  - Retirement process for obsolete articles

---

## 2. Technical SOPs

These cover specific technical procedures that support staff perform regularly.

### Machine Provisioning / Onboarding SOP

- **What it covers:** End-to-end process for setting up a new user's machine — from the initial request to the user receiving a fully configured computer.
- **Typical elements:**
  - Pre-requisites (license, hardware approval, manager authorization)
  - Step-by-step: Autopilot enrollment, domain/Entra ID join, software deployment, printer mapping, drive mapping
  - Verification checklist (what to confirm before handing the machine to the user)
  - Common failure modes and how to handle them
- **Who performs it:** May be Tier 1, Tier 2, or a dedicated provisioning team.

### Account Management SOP

- **What it covers:** Creating, modifying, disabling, and deleting user accounts across all identity systems (Entra ID, on-prem AD, application-specific accounts).
- **Typical elements:**
  - Account creation process (triggered by HR, not IT)
  - Modification process (name changes, department moves, access changes)
  - Deprovisioning process (offboarding triggers, timeline, verification)
  - Shared account management (if applicable)
  - Service account management
- **Critical pharma note:** Offboarding in pharma must be immediate for terminated employees, not "within 24 hours" — regulatory risk.

### Password Reset SOP

- **What it covers:** The approved process for resetting user passwords.
- **Typical elements:**
  - Identity verification requirements (how to confirm the person is who they say they are)
  - Self-service reset vs. assisted reset
  - Which account types can and cannot be reset by support
  - Password policy requirements (length, complexity, history)
  - MFA re-enrollment after password change (if needed)
- **Required documentation:** Ticket number, verification method used, confirmation of reset.

### Software Deployment SOP

- **What it covers:** How software is installed, updated, and removed on company machines.
- **Typical elements:**
  - Approved software list (what is pre-approved vs. requires a request)
  - Deployment methods (Intune, SCCM, manual installation)
  - Installation verification
  - Handling installation failures (log locations, common error codes)
  - Software removal process
- **Pharma note:** GxP-validated machines have a locked software baseline. No software changes without change control.

### Remote Access Setup SOP

- **What it covers:** Setting up and troubleshooting VPN, remote desktop, and remote support tools.
- **Typical elements:**
  - VPN client installation and configuration
  - MFA enrollment for VPN
  - Troubleshooting common VPN failures (certificate issues, client conflicts, network timeouts)
  - Alternative remote access methods (if VPN fails)

### Backup and Recovery SOP

- **What it covers:** How user data is backed up, and how to restore it when requested.
- **Typical elements:**
  - What is backed up (OneDrive, network shares, local machine = user's responsibility)
  - Self-service restore methods (OneDrive recycle bin, Windows File History)
  - Assisted restore process (escalation to Infrastructure for server-level restores)
  - Backup retention periods per data type

---

## 3. Pharma / Compliance SOPs

These are specific to the regulated pharmaceutical environment. They govern how changes are made, how incidents are handled, and how audits are supported.

### Change Control SOP

- **What it covers:** The formal process for making changes to GxP-validated systems, infrastructure, or processes.
- **Typical elements:**
  - Change classification (standard, minor, major, emergency)
  - Change request form requirements
  - Risk assessment and impact analysis
  - Approval workflow (IT, QA, System Owner, sometimes Quality Assurance)
  - Implementation in test environment before production
  - Post-implementation review
  - Emergency change process (verbal approval, retrospective documentation)
- **Why it matters for support:** Even a "simple" change on a GxP machine may require a formal change request. Know which systems are in scope before touching them.

### Deviation / Incident Handling SOP (IT-specific)

- **What it covers:** How to handle an unplanned event that deviates from standard procedures — especially if data integrity may have been affected.
- **Typical elements:**
  - Definition of a deviation
  - Immediate containment steps
  - Notification requirements (QA, System Owner)
  - Investigation and root cause analysis
  - Corrective and preventive actions (CAPA)
  - Documentation requirements
- **Why it matters for support:** If your troubleshooting accidentally causes a deviation (e.g., you restart a service that was not supposed to be restarted, causing a data collection gap), you need to follow this process.

### Access Control SOP

- **What it covers:** How access to systems and data is granted, reviewed, and revoked.
- **Typical elements:**
  - Access request process (who can request, who must approve)
  - Periodic access review requirements (quarterly or annual)
  - Segregation of duties requirements (same person cannot have conflicting roles)
  - Privileged access management (admin accounts, just-in-time access)
- **Why it matters for support:** You should not have more access than your role requires. Requesting access you do not need can create a compliance finding. Know your access boundaries.

### Data Integrity SOP

- **What it covers:** ALCOA+ principles and how they apply to electronic data in the IT environment.
- **Typical elements:**
  - What constitutes a data integrity issue (missing data, modified data, untimely entries)
  - How to identify data integrity issues during support
  - Escalation path for suspected data integrity issues
  - Data integrity in backup/restore processes
- **Why it matters for support:** If a user reports missing data and you find a database inconsistency, do not quietly fix it. Escalate — data integrity issues must be formally investigated.

### Audit Support SOP

- **What it covers:** How to prepare for and respond during an internal or regulatory audit.
- **Typical elements:**
  - Who interacts with auditors (usually not support staff, but know who)
  - What documents to have ready (SOPs, training records, ticket samples, change records)
  - How to behave if an auditor approaches you directly (redirect to the audit lead)
  - How to handle auditor requests for data or system access

---

## 4. Security SOPs

These govern how the company responds to security threats and incidents.

### Security Incident Response SOP

- **What it covers:** How to identify, report, contain, and remediate security incidents.
- **Typical elements:**
  - Definition of a security incident (malware, phishing, unauthorized access, data breach)
  - Reporting chain (support → Security team, do not investigate independently)
  - Immediate containment actions (disconnect from network, power off)
  - Evidence preservation (do not reboot, do not delete files)
  - Forensics and investigation (handled by Security, not support)
- **Why it matters for support:** You are likely the first point of contact for a user reporting suspicious behavior. Your response in the first minutes matters. Do not investigate — escalate.

### Malware / Phishing Response SOP

- **What it covers:** How to handle user reports of malware, ransomware, or phishing emails.
- **Typical elements:**
  - How to identify a phishing email (common indicators, reporting button in Outlook)
  - What to do: user reports a phishing email (instruct user to use the reporting tool, do not click links)
  - What to do: user clicked a phishing link (escalate to Security)
  - What to do: user sees ransomware message (disconnect from network, call Security immediately)
- **Required documentation:** Ticket with user's account, time of report, actions taken.

### Data Loss Prevention (DLP) SOP

- **What it covers:** What DLP policies are in place, how to respond to DLP alerts, and how to handle user inquiries about blocked actions.
- **Typical elements:**
  - Types of data protected (PHI, PII, trade secrets, financial data)
  - Common DLP scenarios (USB block, email block, cloud upload block)
  - How to handle a DLP alert (escalate to Security, do not override)
  - Legitimate data transfer process (if user needs to transfer data that DLP blocks)

### Acceptable Use Policy (AUP)

- **What it covers:** What users can and cannot do with company devices, networks, and data. Not strictly an SOP, but closely related.
- **Typical elements:**
  - Personal use of company devices
  - Software installation restrictions
  - Data handling requirements
  - Consequences of policy violations
- **Why it matters for support:** You will encounter users who violate AUP. Know how to handle it tactfully (remind them of policy, escalate if persistent or severe).

---

## How to Find and Use SOPs

### Locating SOPs

| SOP Type | Likely Location | Access |
|---|---|---|
| Support/IT SOPs | IT SharePoint / Confluence / Jira Knowledge Base | IT team access |
| Pharma/Compliance SOPs | QMS (e.g., Qualio, Veeva Vault, MasterControl) | May require separate access request |
| Security SOPs | Security team's SharePoint or Confluence | Security team access |
| HR/Administrative | Corporate intranet or HR system | All employees |

### Using SOPs Correctly

1. **Always check the version number** — using an outdated procedure is a compliance risk.
2. **Follow the steps in order** — SOPs are written in a specific sequence for a reason (often tied to validation).
3. **Document any deviations** — if you cannot follow an SOP exactly, document what you did differently and why.
4. **Do not assume** — "I knew what to do from experience" is not a defense in an audit. If the SOP says to do it differently, the SOP wins.
5. **Suggest improvements** — if you find an SOP is wrong or outdated, notify the owner. Do not just work around it.
