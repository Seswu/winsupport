# Outlook Stuck on "Connecting" / "Disconnected"

Outlook starts but sits on "Connecting" to the server, or shows "Disconnected" in the status bar.

```
User reports: "Outlook is stuck on 'Connecting to server'"

├─ ISOLATE: Is it just Outlook, or email broadly?
│   ├─ Can the user access webmail (outlook.office.com)?
│   │   ├─ NO → Service may be down → Check health dashboard
│   │   │          M365 Admin Center → Health → Service Health
│   │   └─ YES → Issue is specific to Outlook client
│   │
│   └─ Can other users on the same network send/receive email?
│       └─ NO → Network or server issue, not Outlook
│
├─ QUICK FIXES (try in order)
│   ├─ Restart Outlook
│   ├─ Restart the machine (clears stuck MAPI connections)
│   ├─ Run Outlook in safe mode: `outlook.exe /safe`
│   │   └─ Works in safe mode? → Add-in causing it
│   │       → File → Options → Add-ins → Disable all → Re-enable one by one
│   ├─ Run the Microsoft Support and Recovery Assistant (SaRA)
│   │   → Download from https://aka.ms/SaRA → "Outlook won't start"
│   └─ Open: Control Panel → Mail → Show Profiles → Prompt for profile to use
│       → Might reveal a corrupt profile
│
├─ NETWORK / AUTH CHECK
│   ├─ Is the machine connected to the network?
│   │   └─ `ping outlook.office365.com` → Timeout = network blocking
│   ├─ Is the user's password expired?
│   │   └─ Outlook may keep old cached credentials → [see 02-02]
│   ├─ Check if cached credentials are stale
│   │   └─ Control Panel → Credential Manager → Windows Credentials
│   │       Remove any entries for "MicrosoftOffice*" or "OUTLOOK*"
│   │       (Outlook will re-prompt for credentials)
│   └─ Check if modern auth is enabled
│       └─ File → Account Settings → Account Settings → Select account → Change
│           → Check if "Use Microsoft 365 authentication" is selected vs Basic Auth
│
├─ CORRUPT PROFILE (last resort)
│   ├─ Create a new Outlook profile:
│   │   Control Panel → Mail → Show Profiles → Add → Name it → Set as default
│   │   → Enter user's email → AutoDiscover should configure automatically
│   └─ Old profile can be deleted once new one works (keep for a day in case)
│
└─ Still stuck?
    └─ Check if the OST file is corrupt (over 50 GB or oversized)
        → Rename .ost file in `%localappdata%\Microsoft\Outlook`
        → Restart Outlook (recreates the file fresh)
        ├─ Works → Old OST was corrupt
        └─ Still stuck → Escalate to Tier 2 / Exchange admin
```

**RESULT** → Outlook connected and email flowing.
