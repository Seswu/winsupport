# Reading Material: Pharma Context & Compliance Awareness

## 1. Regulatory Framework

### 21 CFR Part 11
FDA regulation governing electronic records and electronic signatures in the pharmaceutical industry.

**Key requirements relevant to IT support:**
- **Audit trails**: Systems must record who did what, when, and any changes to data. As support, your actions may be part of these audit trails.
- **Electronic signatures**: Must be equivalent to handwritten signatures. This affects how authentication is configured.
- **Validation**: Computerized systems used in GxP processes must be validated. You cannot simply "install the latest version" — the software version is locked after validation.
- **Data integrity**: ALCOA+ principle (Attributable, Legible, Contemporaneous, Original, Accurate, Complete, Consistent, Enduring, Available)

### GxP (Good Practice)
Umbrella term for quality guidelines in pharma:
- **GMP** (Good Manufacturing Practice): Manufacturing processes
- **GLP** (Good Laboratory Practice): Laboratory operations
- **GCP** (Good Clinical Practice): Clinical trials
- **GDP** (Good Distribution Practice): Distribution

**What this means for IT support:**
- Changes to systems supporting GxP processes require formal change control
- Testing/validation must occur before changes go to production
- Documentation is mandatory — verbal fixes are not acceptable
- Access to GxP systems is tightly controlled

## 2. Change Control

### What Is Change Control
A formal process for managing changes to validated systems. Every change must be:
1. **Requested**: Documented with reason, impact assessment, risk evaluation
2. **Reviewed**: By quality assurance, system owner, IT
3. **Approved**: Before implementation
4. **Implemented**: In a controlled manner
5. **Tested**: Verification that the change works and didn't break anything
6. **Documented**: Evidence retained for audit

### What This Means for You
- **You cannot make ad-hoc changes** to GxP-related systems or machines
- Even a "simple fix" like installing a printer driver may require change control
- Emergency changes have an expedited process but still require documentation and retroactive approval
- Always check: Is this machine/system in scope for GxP?

### How to Check
- Ask your team which machines, systems, and applications are GxP-validated
- Check the system inventory or CMDB
- Look for labels or tags on machines indicating GxP status
- When in doubt, assume it IS GxP and follow the change control process

## 3. Audit Readiness

### What Auditors Look For
- **Access logs**: Who accessed what, when
- **Change records**: What changes were made, who approved them, were they tested
- **Incident records**: How were issues resolved, was data integrity maintained
- **Training records**: Are support staff trained and qualified
- **Procedure adherence**: Were SOPs followed

### Your Role
- **Document everything**: Every ticket, every action, every resolution
- **Follow SOPs**: Don't deviate from documented procedures
- **Don't delete evidence**: Even failed attempts or troubleshooting steps
- **Be truthful and accurate**: Never falsify records or backdate entries
- **Know your SOPs**: Where they are, how to access them

## 4. Data Classification and Handling

### Typical Data Classifications

| Classification | Description | Examples | Handling Requirements |
|---|---|---|---|
| Public | Information approved for public release | Marketing materials, press releases | Minimal restrictions |
| Internal | Business information not for public release | Internal memos, org charts | Company systems only |
| Confidential | Sensitive business information | Financial data, business plans | Need-to-know, encrypted |
| Restricted | Highly sensitive, regulatory impact | Patient data, clinical trial data, formulas | Strict access control, audit trail, encryption |

### Data You May Encounter as Support
- **User files**: Don't browse user files beyond what's necessary for troubleshooting
- **Research data**: May be proprietary and highly confidential — access only with authorization
- **Patient data**: Subject to HIPAA/privacy regulations — never access without explicit authorization
- **Credentials**: If you see passwords, keys, or tokens — don't record them in tickets

## 5. Standard Machine Images

### What They Are
A pre-configured, tested, and validated Windows image that serves as the baseline for all machines of a given type (e.g., "Standard Corporate Laptop Image v3.2").

### Why They Matter
- The image is validated — every piece of software, every setting, has been tested and approved
- Deviations from the image are not allowed without formal approval
- If a user needs additional software, it goes through a process to be added to the image or deployed separately
- Re-imaging a machine restores it to the validated baseline

### What You Cannot Do
- Install unapproved software on a validated machine
- Change system settings that are part of the validated configuration
- Disable security software (antivirus, DLP, etc.)
- Modify Group Policy settings

### What You Can Do
- Follow documented procedures for troubleshooting
- Re-image a machine if it's corrupted (restores validated state)
- Request a change through change control if a validated fix is needed
- Escalate when something requires deviation from standard

## 6. Network Segmentation

Pharmaceutical companies typically segment their networks:

| Network | Purpose | Access |
|---|---|---|
| Corporate | Office productivity, email, general IT | All corporate users |
| Lab/Research | Lab instruments, research data systems | Lab personnel only |
| Manufacturing | Production systems, SCADA, MES | Manufacturing personnel only |
| DMZ | External-facing services (web, email gateways) | Limited |
| Guest | Visitor Wi-Fi | Visitors only |

**Support implications:**
- You may need separate credentials or VPN for different networks
- Machines on one network cannot directly access machines on another
- Troubleshooting may require coordination across network teams
- Some issues may require an on-site visit (can't remote to lab network)

## 7. Endpoint Security

Common security tools in pharma environments:

### Endpoint Detection and Response (EDR)
- CrowdStrike, SentinelOne, Microsoft Defender for Endpoint
- Monitors for suspicious behavior, not just known malware signatures
- Can isolate machines from the network automatically
- **If EDR alerts**: Escalate immediately — don't investigate on your own

### Data Loss Prevention (DLP)
- Microsoft Purview DLP, Symantec DLP, Forcepoint
- Monitors and blocks unauthorized data transfers
- Can block USB drives, cloud uploads, email attachments with sensitive data
- **If DLP blocks something**: It's working as designed. Don't disable it without authorization.

### Application Whitelisting
- Only approved applications can run
- Unapproved executables are blocked
- Common in GxP environments
- **If an app won't run**: Check if it's whitelisted before troubleshooting the app itself

### Disk Encryption
- BitLocker (Windows built-in)
- Mandatory on all laptops and portable devices
- Recovery keys escrowed to IT

## 8. Escalation Boundaries

Know when to escalate immediately:

### Escalate to Security Team
- Any EDR/antivirus alert indicating possible malware
- Suspected data breach or unauthorized access
- Phishing reports
- Ransomware indicators (encrypted files, ransom notes)
- Any account compromise indicators

### Escalate to System Owner / Quality
- Any issue with a GxP-validated system
- Data integrity concerns (data missing, modified, corrupted)
- Audit trail gaps
- System behavior that doesn't match validated specifications

### Escalate to Network Team
- Network segmentation issues
- Cross-network connectivity problems
- Firewall rule changes needed
- Proxy/PAC file issues

### Escalate to Infrastructure Team
- Domain controller issues
- Certificate authority issues
- Server-related problems
- Storage/SAN issues
