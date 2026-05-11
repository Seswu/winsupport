# Network Drive Mapping Fails

User cannot map a network drive, or a mapped drive shows a red X or "disconnected" status.

```
User reports: "My mapped drives aren't working" or "Can't map a drive"

├─ CAN THE USER REACH THE SERVER?
│   └─ `ping <server-name>` (e.g., `ping fileserver.company.com`)
│       ├─ Fails → Name resolution issue → `nslookup <server-name>`
│       │   ├─ Resolves? → `ping <IP>` → If IP pings but name doesn't, check DNS
│       │   └─ Doesn't resolve? → DNS issue
│       │       → `ipconfig /flushdns` → retry
│       │       → Check if the user is on VPN (internal shares need VPN)
│       └─ Succeeds → Connectivity is fine
│
├─ CAN THE USER ACCESS THE SHARE MANUALLY?
│   └─ `\\server-name\share-name` in File Explorer or Run dialog
│       ├─ Works → Mapping command probably has wrong path
│       │   → Verify the share path and drive letter
│       └─ Fails with...
│           ├─ "Access denied" → Permission issue
│           │   → Check: Does the user have NTFS/SMB permissions?
│           │   → [see 06-01](01-06_account-auth_app-access-failure.md)
│           ├─ "Network path not found" → Wrong server/share name
│           │   → Verify with the user (typo in path)
│           │   → Check if the share name has changed
│           └─ "This program is blocked" → Group Policy restriction
│               → Check GP: "Remove 'Map network drive' and 'Disconnect network drive'"
│
├─ CREDENTIAL ISSUES
│   ├─ Check: Is the user's domain account working?
│   │   → `whoami` should show domain\username
│   │   → If local account → use `net use Z: \\server\share /user:domain\username *`
│   │
│   ├─ Check for stale credentials:
│   │   → `net use` (shows all current connections)
│   │   → `net use * /delete` (removes all stale connections)
│   │   → Re-map fresh
│   │
│   └─ Check Credential Manager:
│       → `control keymgr.dll` → Windows Credentials
│       → Remove any entries for the file server
│       → Re-map (will prompt for fresh credentials)
│
├─ MAPPING METHODS
│   ├─ Via File Explorer:
│   │   → Right-click "This PC" → Map network drive
│   │   → Or: `net use Z: \\server\share /persistent:yes`
│   │
│   ├─ Via Group Policy (check if it should auto-map):
│   │   → Run `gpresult /h c:\temp\gpreport.html` → Check Drive Maps policy
│   │   → If policy exists but drive isn't mapping → `gpupdate /force` → reboot
│   │
│   └─ Via logon script:
│       → Check if script exists → Run manually → look for errors
│
├─ DRIVE KEEPS DISCONNECTING
│   ├─ Check power settings: "Turn off hard disk after"
│   │   → Network drives can disconnect when the adapter sleeps
│   │   → Power Options → Change plan settings → Change advanced power settings
│   │   → Hard disk → Turn off after → Set to 0 (never)
│   │
│   └─ Check if the user is connected via VPN with short timeout
│       → VPN idle disconnect may kill SMB sessions
│
└─ STILL BROKEN?
    └─ Test: Does it work on a different machine?
        ├─ YES → Issue is user's machine
        └─ NO  → Issue is server/permissions → Escalate
```

**RESULT** → Network drive mapped and accessible.
