# Learning Plan: Windows 11 & Office 365 Support

## Phase 1: Foundation — Windows Concepts Mapped from Linux (Week 1-2)

Since you know Linux, focus on the conceptual differences:

| Linux Concept | Windows Equivalent |
|---|---|
| Package manager (apt/yum) | Winget, Microsoft Store, MSI/EXE installers |
| systemd services | Windows Services (`services.msc`) |
| Cron jobs | Task Scheduler |
| `/etc/passwd`, PAM | Local Accounts, SAM, Credential Manager |
| File permissions (chmod/chown) | NTFS permissions, ACLs, UAC |
| SSH key auth | Windows Hello, Certificate auth |
| Kernel logs (`dmesg`, journald) | Event Viewer, Reliability Monitor |
| Environment variables | System/User Environment Variables |
| Package repos | WSUS, Intune, SCCM/MECM |

**Key topics to study:**
- Windows registry (concept, common keys, `regedit`)
- Group Policy (what it is, how it overrides local settings)
- User Account Control (UAC) and elevation model
- Windows Update mechanisms
- File system structure and special folders
- NTFS vs. FAT32, encryption (BitLocker, EFS)

**Resources:**
- Microsoft Learn: [Windows 11 fundamentals](https://learn.microsoft.com/en-us/training/paths/describe-microsoft-365-core-services-concepts/)
- Microsoft Docs for IT Pros (not developer docs)

---

## Phase 2: Identity & Device Management (Week 2-3)

Critical for newly imaged machines in a corporate environment:

- **Entra ID (formerly Azure AD)** — cloud identity, SSO, conditional access
- **Active Directory** — on-prem domain join, Group Policy, OU structure
- **Microsoft Intune** — mobile device management (MDM), app deployment, compliance policies
- **Autopilot** — zero-touch provisioning for new machines
- **Domain join vs. Azure AD join vs. Hybrid join** — know the difference

**What to learn:**
- How a new machine gets provisioned end-to-end
- How policies are applied (local → domain → Intune)
- How to check effective policies on a machine (`gpresult /h report.html`)
- Common provisioning failures and how to debug them

---

## Phase 3: Office 365 Suite & Administration (Week 3-4)

Focus on what users actually touch:

- **Outlook** — profiles, OST/PST files, AutoDiscover, cached vs. online mode
- **OneDrive/SharePoint** — sync client, Known Folder Move, sync conflicts
- **Teams** — client issues, meeting problems, policy restrictions
- **Office apps** — activation, shared computer licensing, update channels
- **Microsoft 365 Admin Center** — where to check user licenses, reset passwords, view service health

**Key troubleshooting skills:**
- Office Click-to-Run vs. MSI installations
- Using the Microsoft Support and Recovery Assistant (SaRA)
- Reading Office logs
- Understanding license types (E3, E5, F3, etc.) and what features they include

---

## Phase 4: Pharma-Specific Context (Week 4)

Pharmaceutical companies have unique requirements:

- **GxP / 21 CFR Part 11 compliance** — electronic records, audit trails, validated systems
- **Data classification** — handling PHI, PII, proprietary research data
- **Restricted software installation** — users typically cannot install anything themselves
- **Validated images** — machine images are tested and locked; deviations require change control
- **Audit readiness** — everything must be traceable and documented
- **Network segmentation** — lab networks vs. corporate vs. manufacturing

**Questions to clarify with your team:**
- What DLP/endpoint protection is in place?
- Are machines on-prem domain-joined, cloud-joined, or hybrid?
- What is the ticketing/ITSM tool?
- Are there SOPs for common procedures?

---

## Phase 5: Hands-On Practice (Ongoing)

- Set up a Windows 11 VM (free dev VMs from Microsoft)
- Create a free Microsoft 365 Developer tenant (comes with E5 licenses, auto-renews if active)
- Practice the 15 common issues from your list in a controlled environment
- Learn to use these diagnostic tools:
  - Event Viewer
  - Resource Monitor / Performance Monitor
  - `sfc /scannow`, `DISM /Online /Cleanup-Image /RestoreHealth`
  - `gpresult`, `whoami /all`
  - PowerShell basics for admin tasks
  - Network troubleshooter, `Test-NetConnection`

---

## Phase 6: Where to Find Information

Bookmark these:

| Resource | Purpose |
|---|---|
| [Microsoft Learn](https://learn.microsoft.com) | Official docs, training paths |
| [Microsoft Tech Community](https://techcommunity.microsoft.com) | Forums, known issues |
| [Windows Release Health](https://learn.microsoft.com/en-us/windows/release-health/) | Known issues per build |
| [Microsoft 365 Service Health](https://admin.microsoft.com) | Service outages, advisories |
| [Microsoft Q&A](https://learn.microsoft.com/en-us/answers/) | Community Q&A |
| Event Viewer logs | `Applications and Services Logs` → `Microsoft` → specific product |
| `C:\Windows\Logs` | Windows Update, DISM, CBS logs |
| `%localappdata%\Microsoft\OneDrive\logs` | OneDrive sync logs |

---

## Suggested Questions Before Finalizing

1. **What's the device management stack?** Intune-only, SCCM/MECM, or hybrid?
2. **Are there existing runbooks or SOPs** for common procedures?
3. **What's the onboarding process for new machines?** Autopilot, manual imaging, or third-party tool?
4. **What endpoint security tools** are deployed (CrowdStrike, SentinelOne, Defender for Endpoint)?
5. **Is there a lab or staging environment** you can practice in?
6. **What's the expected timeline** before you need to be support-ready?
