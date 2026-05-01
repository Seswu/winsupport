# 11-Day Crash Plan: Windows 11 & Office 365 Support

## Days 1-2: Windows 11 Core Orientation

**Goal:** Navigate the OS, understand the admin tools, map from Linux mental model.

- Windows 11 UI layout, Settings app vs. Control Panel, right-click context menus
- File Explorer: special folders, Quick Access, hidden files, file extensions, path structure
- Task Manager deep dive: processes, startup apps, performance, services tab
- Event Viewer: where logs live, which logs matter (`Application`, `System`, `Setup`), filtering by level/source
- Services (`services.msc`): start/stop, startup types, dependencies — your equivalent of `systemctl`
- UAC and elevation: why "Run as Administrator" matters, how to spot permission issues
- `whoami /all` — understand groups, privileges, integrity levels
- Environment variables: System vs. User, `PATH` gotchas

**Hands-on:** Break things in a VM — deny yourself permissions on a folder, kill a service, observe what breaks and how it shows in logs.

---

## Days 3-4: Common Windows 11 Issues & Troubleshooting

**Goal:** Be able to resolve the most frequent support tickets.

- **Login/lockout issues:** Credential Manager, cached credentials, Windows Hello, smart card login
- **Slow performance:** `perfmon`, Resource Monitor, identifying CPU/disk/memory hogs, disabling startup items
- **Windows Update:** Update history, troubleshooting, `SoftwareDistribution` folder, known bad updates
- **Network issues:** `ipconfig`, `ping`, `nslookup`, `Test-NetConnection` (PowerShell), network reset, DNS flush
- **Driver issues:** Device Manager, driver rollback, reading error codes, manufacturer driver vs. Windows Update driver
- **BSOD:** Reading minidumps with BlueScreenView or WinDbg, safe mode, rollback strategies
- **File/permission issues:** NTFS permissions (effective permissions tool), ownership, inherited vs. explicit, taking ownership
- **Printers:** Print queue stuck, spooler restart, removing stale printer objects

**Key commands to memorize:**
```
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth
gpresult /h report.html
netsh winsock reset
ipconfig /flushdns
```

**Hands-on:** Practice each issue in a VM. Intentionally break drivers, corrupt permissions, break networking, then fix.

---

## Days 5-6: Office 365 Core — What Users Actually Use

**Goal:** Understand and troubleshoot Outlook, OneDrive, Teams, and Office apps.

- **Outlook:**
  - Profile management (`Control Panel > Mail`)
  - OST vs. PST — what they are, where they live, when to rebuild
  - Cached Exchange Mode vs. Online Mode
  - AutoDiscover process (how Outlook finds the server)
  - Common issues: stuck on "connecting," missing folders, send/receive errors, "need password" loop

- **OneDrive:**
  - Sync client behavior, Known Folder Move (Desktop, Documents, Pictures)
  - Files On-Demand (cloud icon vs. green checkmark vs. syncing icon)
  - Sync conflicts, file name/length limits, storage quota
  - Reset OneDrive: `onedrive.exe /reset`
  - Log location: `%localappdata%\Microsoft\OneDrive\logs`

- **Teams:**
  - Common issues: audio/video not working, can't join meetings, presence not updating
  - Clearing Teams cache
  - Web vs. desktop client differences

- **Office apps (Word, Excel, PowerPoint):**
  - Activation/licensing errors
  - Office repair (Quick Repair vs. Online Repair)
  - Safe mode for Office apps: `winword.exe /safe`

**Hands-on:** Create a free M365 Developer tenant. Break Outlook profiles, break OneDrive sync, cause activation errors — then fix them.

---

## Day 7: New Machine Provisioning Issues

**Goal:** Handle the most common "my new PC isn't working right" scenarios.

- **Autopilot / OOBE flow:** What happens during first boot, where it commonly fails
- **Domain join / Entra ID join failures:** Timing issues, DNS misconfiguration, account permissions
- **Missing applications:** Apps not deployed yet, Intune policy not applied, user not in the right group
- **Missing printers/drive mappings:** Group Policy not applied, check with `gpresult /h report.html`
- **Policy not applying:** `gpupdate /force`, understanding policy precedence and processing order
- **BitLocker recovery:** Where to find recovery keys (Entra ID / AD), what triggers recovery
- **User profile issues:** Temp profile loading, profile corruption, roaming profile conflicts

**Checklist for new machine triage:**
1. Is the machine domain-joined / Entra-joined? (`dsregcmd /status`)
2. Are policies applied? (`gpresult /h report.html`)
3. Are expected apps installed? (Compare against standard image/software list)
4. Is the user logged in with correct account? (`whoami`)
5. Is OneDrive syncing? (Check system tray icon)
6. Is Outlook connected? (Check connection status)

---

## Day 8: Microsoft 365 Admin & Licensing

**Goal:** Know where to look in admin portals and how licensing works.

- **Microsoft 365 Admin Center** (`admin.microsoft.com`):
  - User management: reset passwords, assign licenses, manage groups
  - Service Health dashboard: is this a widespread outage or just one user?
  - Message Center: upcoming changes and advisories

- **Entra Admin Center** (`entra.microsoft.com`):
  - Device status, conditional access policies (why a user might be blocked)
  - Sign-in logs: failed login attempts, MFA failures, conditional access denials

- **Intune Admin Center** (`intune.microsoft.com`):
  - Device compliance status, deployed apps, configuration profiles
  - Sync device, remote actions (wipe, restart, etc.)

- **Licensing basics:**
  - Common license types (E3, E5, Business Premium, F3) and what each includes
  - License assignment delays, license conflicts
  - Shared Computer Activation (SCA) for shared/RDS environments

---

## Day 9: Pharma Context & Compliance Awareness

**Goal:** Understand the constraints and "why" behind pharma IT policies.

- **21 CFR Part 11 / GxP:** Why certain apps require validation, why you can't just "install a workaround"
- **Change control:** Why some fixes require tickets, approval, and documentation before implementation
- **Audit trails:** Everything you do may be reviewed — document your troubleshooting steps
- **Data handling:** Don't access user data without authorization, especially in research/lab contexts
- **Standard images:** Machines are built from validated images — deviations are not allowed without approval
- **Escalation boundaries:** Know when to escalate (security events, GxP system issues, suspected data breaches)

---

## Day 10: Diagnostic Tools & Reference Building

**Goal:** Build your personal quick-reference cheat sheet.

- Build a one-page reference with:
  - Key PowerShell commands for admin tasks
  - Log file locations for Windows, Office, OneDrive, Teams
  - Common error codes and their meanings
  - Quick diagnostic flowcharts for the 15 common issues

- Set up browser bookmarks:
  - Microsoft Learn docs
  - Windows Release Health page
  - M365 Service Health
  - Tech Community forums
  - Internal runbooks/SOPs (ask your team for these)

- Learn where to find info inside Windows:
  - `Settings > System > About` — OS version, build number
  - `winver` — quick version check
  - `msinfo32` — detailed system info
  - `dxdiag` — display/audio driver info

---

## Day 11: Shadow & Scenario Practice

**Goal:** Apply knowledge in realistic scenarios.

- Shadow an experienced supporter for a half-day if possible
- Walk through the 15 common issues with a lab machine, timing yourself
- Practice explaining solutions in non-technical terms (end users don't know what DNS is)
- Draft your first-contact scripts for the top 5 issues:
  - "My PC won't let me log in"
  - "Outlook won't connect"
  - "My new computer is missing [application]"
  - "OneDrive isn't syncing"
  - "My computer is really slow"

---

## Daily Time Allocation (suggested)

| Activity | Time |
|---|---|
| Reading/studying docs | 2 hours |
| Hands-on lab practice | 2 hours |
| Building reference notes | 1 hour |
| **Total** | **~5 hours/day** |

---

## Critical Unknowns to Resolve Immediately

Ask your team on Day 1:
1. What device management stack is used? (Intune, SCCM, hybrid?)
2. Where are the existing runbooks/SOPs?
3. What is the new machine provisioning process?
4. What endpoint security is deployed?
5. What ticketing system is used, and is there a knowledge base?
