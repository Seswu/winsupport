# Reading Material: Microsoft 365 Admin & Licensing

## 1. M365 Admin Center

### URL: https://admin.microsoft.com

This is the primary portal for M365 administration. It serves as a hub linking to specialized admin centers.

### Key Areas

**Users > Active Users**
- List of all users with license assignments
- Create, edit, delete users
- Reset passwords
- Manage aliases and proxy addresses
- Sign-in activity

**Billing > Your products**
- License inventory: how many licenses purchased, assigned, available
- Subscription details and renewal dates

**Health > Service health**
- Current status of all M365 services
- Incidents (active outages) and advisories (planned maintenance)
- Historical health data
- **Always check this first** when multiple users report the same issue

**Health > Message center**
- Upcoming changes, new features, deprecations
- Organized by service (Exchange, Teams, SharePoint, etc.)
- Some changes require admin action before a deadline

**Show all > Settings**
- Org settings: security, privacy, services configuration
- Domains: custom domain management
- Sites: SharePoint site management

### Admin Roles
M365 uses role-based access control. Common roles:
- **Global Admin**: Full access to everything (use sparingly)
- **User Admin**: Can manage users, groups, passwords
- **Helpdesk Admin**: Can reset passwords, manage service requests
- **Exchange Admin**: Full Exchange management
- **Teams Admin**: Full Teams management
- **SharePoint Admin**: Full SharePoint management
- **Intune Admin**: Device management
- **Reports Reader**: Can view reports but not change settings

## 2. Entra Admin Center

### URL: https://entra.microsoft.com

Formerly Azure Active Directory. Manages identity and access.

### Key Areas

**Identity > Users**
- User accounts, group memberships, license assignments
- Sign-in logs: who logged in, from where, with what result
- Audit logs: what changes were made, by whom, when

**Identity > Groups**
- Security groups and Microsoft 365 groups
- Group membership management
- Dynamic groups (membership based on rules)

**Identity > Devices**
- All registered/joined devices
- Device details: OS, compliance status, last check-in
- BitLocker recovery keys per device

**Protection > Conditional Access**
- Policies that control access based on conditions:
  - User/group
  - Cloud app
  - Location (trusted IPs, countries)
  - Device compliance
  - Sign-in risk
- Actions: block access, require MFA, require compliant device

**Monitoring > Sign-in logs**
- Detailed sign-in attempts with:
  - Status (success, failure)
  - Failure reason
  - IP address, location, device
  - MFA details
  - Conditional access evaluation results
- **Essential for troubleshooting login issues**

**Monitoring > Audit logs**
- Record of changes: user created, license assigned, group modified
- Retained for 30 days (or longer with audit premium)

## 3. Intune Admin Center

### URL: https://intune.microsoft.com

Mobile Device Management (MDM) and Mobile Application Management (MAM).

### Key Areas

**Devices > All devices**
- List of managed devices
- Device details: OS, compliance, enrolled apps, last check-in
- Remote actions: sync, restart, wipe, retire, reset password

**Devices > Configuration profiles**
- Settings applied to devices (Wi-Fi, certificates, restrictions, etc.)
- Assigned to groups

**Devices > Compliance policies**
- Rules that determine if a device is "compliant"
- Examples: OS version minimum, encryption required, no jailbreak/root
- Non-compliant devices can be blocked from access via Conditional Access

**Apps > All apps**
- Deployed applications
- App types: Microsoft 365, Win32, LOB, Web, Store
- Assignment: required, available, uninstall

**Reports**
- Device compliance reports
- App installation reports
- Enrollment reports

## 4. Licensing

### Common License Types

| License | Key Features | Typical Users |
|---|---|---|
| **Microsoft 365 E3** | Office apps, Exchange, OneDrive, Teams, SharePoint, Windows Enterprise, basic security | Knowledge workers |
| **Microsoft 365 E5** | Everything in E3 + advanced security, compliance, analytics, Phone System | Power users, executives |
| **Microsoft 365 Business Premium** | E3-like features but for orgs up to 300 users, includes Intune | Small/mid businesses |
| **Microsoft 365 F3** | Frontline workers: web/mobile Office only, limited Exchange, Teams, Shifts | Shift workers, factory |
| **Office 365 E3** | Office apps + Exchange/OneDrive/SharePoint/Teams (no Windows license) | Organizations with separate Windows licensing |
| **Visio Plan 2** | Visio desktop + web | Project managers, engineers |
| **Project Plan 3** | Project desktop + web | Project managers |

### License Assignment
- Licenses are assigned to user accounts in M365 Admin Center or via group-based licensing (Entra ID P1+)
- After assignment, it can take up to 24 hours for all services to activate (usually much faster)
- Each license includes multiple service plans (Exchange, Teams, SharePoint, etc.)
- Individual service plans can be disabled per user

### License Troubleshooting
- User reports "unlicensed" or "activation required": Check license assignment in admin portal
- User has license but app doesn't work: Check if the specific service plan is enabled
- Multiple licenses assigned: May cause conflicts. Remove duplicates.
- License recently assigned: May need time to propagate. Sign out/in of Office apps.

### Shared Computer Activation (SCA)
For shared/RDS/VDI environments where multiple users log into the same machine:
- Office must be configured for SCA
- Each user signs into Office with their own credentials
- License is validated per-session
- Without SCA, only the first user to activate Office can use it

## 5. Service Health Troubleshooting

When multiple users report the same issue, check service health before troubleshooting individual devices.

### Reading Service Health
- **Service operational**: Green checkmark. Issue is likely user-specific.
- **Advisory**: Yellow triangle. Planned maintenance or known issue with workaround.
- **Incident**: Red exclamation. Active outage. Microsoft is working on it.

### Incident Details
- Start time: When the issue began
- Last updated: Most recent update from Microsoft
- User impact: Who is affected and how
- Current status: What Microsoft is doing
- More info: Workarounds if available

### Subscription
- Subscribe to incidents for specific services
- Emails sent when incidents start and update
- Check admin contact email is current
