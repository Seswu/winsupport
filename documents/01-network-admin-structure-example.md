# Network & Administrative Structure — Example: Aegis Therapeutics, Inc.

## Company Profile

| Attribute | Detail |
|---|---|
| **Name** | Aegis Therapeutics, Inc. |
| **Size** | ~900 employees |
| **Sites** | HQ / R&D (Boston, MA), Manufacturing (Bridgewater, NJ), Sales offices (3x regional) |
| **Therapeutic area** | Oncology (small molecule + biologic candidates) |
| **Stage** | 2 marketed products, 4 clinical-stage candidates |
| **Regulatory scope** | FDA (US), EMA (EU), MHRA (UK) — GMP, GLP, GCP across sites |

---

## IT Organizational Hierarchy

```
Chief Information Officer (CIO)
├── Director, IT Operations
│   ├── IT Service Desk Manager
│   │   ├── Tier 1 Support (4 staff)    ← You are here
│   │   └── Tier 2 Support (3 staff)
│   ├── Infrastructure Team (5 staff)
│   │   ├── Server / Storage Admin
│   │   ├── Network Engineer
│   │   └── Cloud Engineer (M365 / Azure)
│   └── Endpoint Engineering (3 staff)
│       ├── Image management & Autopilot
│       └── Intune / SCCM administration
├── Director, IT Security
│   ├── Security Operations (2 staff)
│   └── Identity & Access Management (2 staff)
├── Director, IT Compliance / QA
│   ├── Computer System Validation (3 staff)
│   └── IT Audit Readiness (2 staff)
└── Director, Business Applications
    ├── LIMS / ELN Administrator
    ├── ERP Administrator
    └── QMS / EQMS Administrator
```

**Where support sits:** Tier 1 handles initial triage, password resets, common troubleshooting. Tier 2 handles escalations, software deployment issues, and GxP-adjacent tickets. GxP system changes and security incidents skip to Infrastructure/Compliance/Security directly.

---

## Network Topology

```
Internet
  │
  ├── Site-to-Site VPN (HQ ↔ Manufacturing)
  ├── Remote Access VPN (Cisco AnyConnect + MFA)
  └── M365 / Azure ExpressRoute
       │
 ┌─────┴──────────────────────────────────────────┐
 │                  DMZ                            │
 │  (Web servers, email gateway, reverse proxy)    │
 └──────────────────────┬──────────────────────────┘
                        │
 ┌──────────────────────┴──────────────────────────┐
 │           Corporate Network (HQ)                 │
 │  VLAN 10: Corporate workstations & printers      │
 │  VLAN 20: VoIP phones                            │
 │  VLAN 30: IT infrastructure (servers, DCs)       │
 │  VLAN 40: Guest Wi-Fi (internet only)            │
 │                                                  │
 │  ┌────────────────────────────────────────────┐  │
 │  │      Lab / Research Network (VLAN 50)       │  │
 │  │  - Lab instrumentation & workstations       │  │
 │  │  - LIMS / ELN application servers           │  │
 │  │  - **NO direct internet access**            │  │
 │  │  - Air-gapped from corporate via firewall   │  │
 │  └────────────────────────────────────────────┘  │
 └──────────────────────┬──────────────────────────┘
                        │
 ┌──────────────────────┴──────────────────────────┐
 │          Manufacturing Site (Bridgewater)        │
 │  VLAN 60: Manufacturing execution (MES/SCADA)    │
 │  VLAN 70: Manufacturing workstations (validated)  │
 │  **OT network** — managed by separate team       │
 │  Limited connectivity to corporate (read-only)   │
 └─────────────────────────────────────────────────┘
```

### Key network rules support must know:

- Lab network machines **cannot** be remotely accessed from corporate; must be on-site or use a dedicated jump box
- Manufacturing OT network tickets require **immediate escalation** to the OT team
- Guest Wi-Fi is internet-only; users complaining about no access to internal resources from guest Wi-Fi is a non-issue
- VPN access requires MFA through Microsoft Authenticator or hardware token

---

## Device Management Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Identity** | Entra ID (cloud) + on-prem AD (sync via Microsoft Entra Connect) | Authentication, SSO, device join |
| **Provisioning** | Windows Autopilot (user-driven mode) | New machine setup |
| **MDM** | Microsoft Intune | Device compliance, app deployment, config profiles |
| **Legacy Config** | On-prem Group Policy (via AD) | Drive mappings, printer deployment, legacy settings |
| **Imaging** | SCCM (task sequence, fallback only) | Rare — reimage if Autopilot fails |
| **Patch management** | Windows Update for Business (via Intune rings) | Monthly patching cadence |
| **Endpoint security** | Microsoft Defender for Endpoint + CrowdStrike Falcon | EDR, antivirus, DLP |

### Device join types

| Type | Who | Characteristics |
|---|---|---|
| **Entra ID joined** | Corporate users, sales | Cloud-only, Intune-managed, no on-prem AD |
| **Hybrid Entra ID joined** | Lab, manufacturing, QA | Cloud + on-prem, GPO + Intune, validated images |
| **Entra ID registered** | BYOD, mobile devices | Personal devices with company app access only |

---

## Identity & Access Architecture

```
User authenticates → Entra ID
  ├── MFA: Microsoft Authenticator push (default) or hardware token (manufacturing)
  ├── Conditional Access:
  │   ├── Block access from non-compliant devices (Intune compliance check)
  │   ├── Require MFA for all external access
  │   ├── Block access from unexpected geographic locations
  │   └── Require managed device for access to GxP systems
  └── Privileged Identity Management (PIM)
      └── Admin roles require justification + time-limited approval
```

### Account types

| Type | Description | Who has it |
|---|---|---|
| **Corporate account** | Standard Entra ID user | All employees |
| **Service account** | Used by applications, no interactive login | Managed by Infrastructure |
| **Admin account** | Separate account with admin roles | IT staff only (activated via PIM) |
| **Shared account** | Lab equipment, cleanroom terminals | Lab/manufacturing (strictly controlled, audited) |

---

## Application Landscape

| Application | Category | GxP? | Owner | Support tier |
|---|---|---|---|---|
| M365 (Exchange, Teams, SharePoint, OneDrive) | Productivity | No | Cloud Engineer | Tier 1/2 |
| LIMS (LabWare) | Lab data management | **Yes** (GLP) | LIMS Admin | Tier 2 / LIMS Admin |
| ELN (Benchling) | Electronic lab notebooks | **Yes** (GLP) | LIMS Admin | Tier 2 / Vendor |
| QMS (Qualio) | Quality management | **Yes** (GMP/GCP) | QMS Admin | Tier 2 / QMS Admin |
| ERP (SAP Business One) | Enterprise resource planning | **Yes** (GMP) | ERP Admin | Tier 2 / ERP Admin |
| MES (POMS) | Manufacturing execution | **Yes** (GMP) | Manufacturing IT | OT Team only |
| Document management (Veeva Vault) | Regulatory documents | **Yes** (GxP) | QA | Tier 2 / Veeva Admin |
| Confluence / Jira | Internal IT documentation | No | IT Operations | Tier 1/2 |
| Slack | Internal communication | No | IT Operations | Tier 1 |

### Golden rule for support

> If a ticket involves any application marked **GxP**, do not make changes. **Escalate to the system owner.** If you're unsure, treat it as GxP and escalate.

---

## Data Flow & Compliance Boundaries

```
User creates document
  ├── Local machine: Encrypted with BitLocker
  ├── If corporate user:
  │   └── OneDrive sync (Known Folder Move) → SharePoint Online
  │       └── Retention policy applied (6yr default, 25yr for GxP records)
  ├── If lab user:
  │   └── Saved to lab network share (backed up nightly)
  │       └── LIMS/ELN stores data in validated database
  └── If manufacturing user:
      └── MES/SCADA system (no local storage permitted)
          └── Data archived to validated storage
```

### DLP rules (data loss prevention)

- USB removable media: **blocked** on all GxP machines and lab machines
- Corporate machines: USB allowed but **monitored** (copy events logged)
- OneDrive/SharePoint sharing with external users: **requires approval**
- Email with credit card numbers, SSN, or clinical trial IDs: **blocked automatically**

### Backup schedule

| Data type | Frequency | Retention | Restore SLA |
|---|---|---|---|
| User OneDrive/SharePoint | Continuous (cloud) | 30-day recycle bin + 93-day legal hold | Self-service |
| Lab network shares | Nightly + hourly snapshots | 30 days | 4 hours |
| GxP application databases | Nightly + transaction log every 15 min | 7 years (regulatory) | 8 hours |
| Manufacturing systems | Nightly | 2 years | 24 hours (requires vendor) |

---

## Change Control Process (abbreviated)

```
User request / IT need
  → Ticket created in Jira Service Management
  → Assess: Is this a GxP system?
    ├── No → Standard change (peer review, implement)
    └── Yes → Formal change control:
        1. Change request form (description, risk assessment, rollback plan)
        2. Reviewed by IT + QA + System Owner
        3. Approved at Change Control Board (meets weekly; emergency path available)
        4. Implemented in test environment → testing/validation
        5. Implemented in production
        6. Post-implementation review & documentation closed
```

> **Emergency changes** (e.g., production outage on a validated system): Verbal approval from Director + QA, implement immediately, retrospective documentation within 5 business days.

---

## Key Escalation Contacts (fill with real names)

| Domain | Team | When to escalate |
|---|---|---|
| **Security** | IT Security | Malware, phishing, account compromise, DLP alerts |
| **Infrastructure** | Infrastructure Team | Server down, network outage, domain issues, VPN problems |
| **QA / Compliance** | IT Compliance | GxP system issues, change control, audit questions |
| **Manufacturing IT** | OT Team | Any issue on the manufacturing OT network |
| **LIMS / Lab Systems** | LIMS Admin | Lab applications, instrument connectivity |
| **Quality System** | QMS Admin | Qualio issues, CAPA entries |
