# Outlook Constantly Prompting for Password

Outlook repeatedly asks for the user's password even after entering it correctly.

```
User reports: "Outlook keeps asking for my password over and over"

├─ MODERN AUTH CHECK
│   ├─ Is modern authentication enabled for this tenant?
│   │   └─ Check: tenant likely has modern auth on by default
│   │       → Old Outlook versions (2013, 2016 pre-update) don't support it
│   │       → Minimum: Outlook 2016 with Click-to-Run or Outlook 2019/365
│   ├─ Is modern auth enabled in Outlook's account settings?
│   │   └─ File → Account Settings → Select account → Change
│   │       → Check "Use Microsoft 365 authentication" (ON = modern auth)
│   └─ If modern auth is OFF → Turn it on
│       → Or: The registry key `EnableADAL` = 1
│       HKCU\SOFTWARE\Microsoft\Office\16.0\Common\Identity\EnableADAL = 1
│
├─ CREDENTIAL MANAGER
│   └─ Open `control keymgr.dll` → Windows Credentials
│       → Remove ALL entries that contain "MicrosoftOffice", "OUTLOOK", "ADAL"
│       → Restart Outlook → it will prompt for credentials fresh
│
├─ STALE TOKENS
│   ├─ Delete the token cache folder:
│   │   `%LOCALAPPDATA%\Microsoft\OneAuth\` (close Outlook first)
│   ├─ Remove saved credentials via `cmdkey /list` → `cmdkey /delete:<target>`
│   └─ Run: `signout` from Windows credential prompt → sign back in
│
├─ VERSION / UPDATE CHECK
│   ├─ Is Outlook up to date?
│   │   └─ File → Office Account → Update Options → Update Now
│   ├─ Is the build old enough that it lacks modern auth support?
│   │   └─ If on an old build → Update or reinstall
│   └─ Check if a recent update broke auth
│       → Known issues: search "Outlook password prompt [KB number]"
│
├─ MULTIPLE ACCOUNTS
│   └─ Does the user have multiple mailboxes added?
│       → Mixed auth types (modern + basic) can cause conflicts
│       → Remove the non-primary account → test → re-add
│
└─ AD FS / FEDERATION ISSUES (if applicable)
    └─ Check: Are other federated apps prompting for password?
        ├─ YES → Federation server may be unreachable
        │   → Check ADFS endpoints
        → Notify identity team
        └─ NO  → Outlook-specific → Try full Office repair
            → Settings → Apps → Microsoft 365 → Modify → Quick Repair
            → If that fails → Online Repair
```

**RESULT** → Password prompt resolved, or known federation issue escalated.
