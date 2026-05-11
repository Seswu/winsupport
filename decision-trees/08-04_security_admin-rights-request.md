# Admin Rights Request

User requests local administrator rights on their machine to install software or change system settings.

```
User requests: "I need admin rights to install [software]"

├─ VERIFY THE REQUEST
│   ├─ Why does the user need admin rights?
│   │   ├─ "To install software" → Are they trying to install it themselves?
│   │   │   → Check if the software is available via Company Portal / Intune
│   │   │   → Check if the software is already in the approved software catalog
│   │   │   → Offer to install it for them with admin rights (Run as admin)
│   │   ├─ "To change a system setting" → Which setting exactly?
│   │   │   → Does the setting require admin (e.g., driver install, service changes)?
│   │   │   → Can IT make the change instead of granting permanent admin?
│   │   └─ "The app needs admin to run" → Compatibility issue
│   │       → Try: Properties → Compatibility → Run as administrator
│   │       → Try: Install for "All users" (per-machine, not per-user)
│   │
│   └─ What application or task specifically requires admin?
│       → Get the exact name, version, and vendor
│       → Check if it's a known application with a documented admin requirement
│       → Some apps falsely claim admin rights when they just need write access to their own folder
│
├─ CONSIDER ALTERNATIVES (try these FIRST before granting admin)
│   ├─ **Install the application for the user**:
│   │   → Run the installer as an administrator on their behalf
│   │   → Most applications install successfully → user doesn't need admin after install
│   │
│   ├─ **Use Microsoft Intune / Company Portal**:
│   │   → Deploy the app through corporate management
│   │   → The app installs with SYSTEM privileges, user doesn't need admin
│   │
│   ├─ **Use a standard user workaround**:
│   │   → For folder-level access: grant modify permissions on the app folder
│   │   → For registry access: pre-configure the registry for the user
│   │
│   └─ **Use Credential Manager or scheduled task**:
│   │   → Some apps can store admin credentials for specific operations
│   │   → Or: Create a scheduled task that runs the app as admin at user request
│   └─
│
├─ EVALUATE THE REQUEST
│   ├─ Is the user's role business justification enough?
│   │   → Developer / Engineer: often legitimately need admin for dev tools
│   │   → General office user: almost never needs permanent admin
│   │
│   └─ Check company policy:
│       → Does the company allow granting admin rights?
│       ├─ NO → Do not grant, explain policy
│       │     → Escalate the app install request to Tier 2
│       └─ YES → Requires manager approval
│
├─ PROCESS (if granting admin rights is approved)
│   ├─ Ensure manager approval is documented in the ticket
│   ├─ Add user to the local Administrators group:
│   │   → Via GPO restricted group (if managed centrally)
│   │   → Or manually: `net localgroup Administrators <domain\username> /add`
│   │   → Or via Intune/Configuration Manager if managed
│   │
│   └─ Set a time limit if using PIM (Privileged Identity Management):
│       → Just-in-time access via Entra ID PIM
│       → User requests elevation → approved → auto-expires after set time
│
├─ RISKS AND REMINDERS
│   └─ Explain to the user (and document in ticket):
│       → Admin rights reduce security: can install malware, change settings
│       → User is responsible for all actions taken with admin rights
│       → Rights can be revoked if policy is violated
│       → Admin rights are not transferable to other users
│
└─ DENIED?
    └─ Help the user find an alternative:
        → Which specific thing are you trying to do?
        → Let me figure out if IT can make that change centrally
        → Many admin-level tasks can be done by IT without granting full admin
```

**RESULT** → Admin rights granted temporarily (if justified) or alternative solution provided.
