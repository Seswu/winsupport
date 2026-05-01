# Reference Material: Pharma Context & Compliance

## Quick Decision Guide: Can I Do This?

| Action | GxP Machine | Non-GxP Machine |
|---|---|---|
| Reset user password | Yes (follow SOP) | Yes |
| Restart a service | Yes (if documented in SOP) | Yes |
| Install approved software | Yes (via standard deployment) | Yes |
| Install unapproved software | No — requires change control | Maybe — check with manager |
| Disable antivirus | No — requires security approval | No — requires security approval |
| Modify registry | Only per documented procedure | Only if necessary for fix |
| Re-image machine | Yes (restores validated state) | Yes |
| Access user files | Only as needed for ticket | Only as needed for ticket |
| Change system settings | Only if part of validated config | Yes, within policy |
| Disable Windows Update | No — may break compliance | Only with approval |

## Key Contacts Template

Fill in with your team's information:

| Role | Contact | When to Escalate |
|---|---|---|
| IT Service Desk Lead | | General support guidance |
| Security Team | | Malware, breaches, phishing |
| Quality Assurance | | GxP system issues, change control |
| System Owner: [System Name] | | Issues with that system |
| Network Team | | Network/connectivity issues |
| Infrastructure Team | | Server/DC/storage issues |
| Compliance Officer | | Regulatory questions |
| Change Control Board | | Submit change requests |

## GxP System Identification

Questions to ask when determining if a system is GxP:
1. Is this system used in manufacturing? (GMP)
2. Is this system used in laboratory testing? (GLP)
3. Is this system used in clinical trials? (GCP)
4. Does this system store or process data that supports regulatory submissions?
5. Is this system listed in the validated systems inventory?
6. Does this system have an SOP associated with it?

If YES to any of these, treat as GxP and follow change control.

## Audit Trail Considerations

Your actions that may appear in audit trails:
- Logins to systems
- File access and modifications
- Configuration changes
- User account changes
- Software installations
- Security setting changes

Always:
- Work through official channels (ticketing system, remote tools)
- Document your actions in the ticket
- Use your own credentials (never share accounts)
- Only do what's necessary for the resolution

## Data Handling Rules

- **Minimize access**: Only open files necessary to resolve the issue
- **Don't copy data**: Never copy user data to USB, personal cloud, or email
- **Don't photograph screens**: Screenshots in tickets are okay, but don't capture sensitive data
- **Report exposures**: If you accidentally see data you shouldn't have, report it
- **Use approved tools**: Only use company-approved remote access and diagnostic tools

## Regulatory Reference

| Regulation | Scope | Relevance to IT |
|---|---|---|
| 21 CFR Part 11 | Electronic records/signatures | System access, audit trails, e-signatures |
| 21 CFR Part 210/211 | GMP for drugs | Manufacturing systems validation |
| 21 CFR Part 58 | GLP for nonclinical studies | Lab system validation |
| 21 CFR Part 312 | Clinical investigations | Clinical trial system validation |
| HIPAA | Patient data privacy | Access controls, audit trails |
| GDPR | EU personal data | Data protection, right to erasure |
| EU Annex 11 | Computerized systems (EU) | Similar to 21 CFR Part 11 for EU |

## SOP Structure (Typical)

Standard Operating Procedures in pharma typically include:
1. Title and SOP number
2. Purpose
3. Scope
4. Responsibilities
5. Procedure (step-by-step instructions)
6. References
7. Revision history
8. Approval signatures

When following an SOP:
- Use the current version (check revision number)
- Follow steps in order
- Document any deviations
- Record completion with date and signature (electronic or physical)
