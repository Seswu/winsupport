# SharePoint Access / Sync Errors

User cannot access a SharePoint site, or the OneDrive sync client reports errors syncing SharePoint libraries.

```
User reports: "I can't access the SharePoint site" or "SharePoint files won't sync"

├─ ACCESS ISSUES (site won't load)
│   ├─ Check: Does the site load in a web browser?
│   │   ├─ NO → Site may be down or user lacks permissions
│   │   │   ├─ Check service health (admin.microsoft.com → Health)
│   │   │   ├─ Check if the URL is correct (typos, old link)
│   │   │   └─ Check user's access:
│   │   │       → Site owner can verify: Site Settings → Site Permissions
│   │   │       → User not listed → Request access via site owner
│   │   └─ YES → Moves to sync issues below
│   │
│   └─ Check: Is it a timeout error?
│       → Try from a different network (off VPN, home network)
│       → Try clearing browser cache / private browsing
│       → Try a different browser
│
├─ SYNC ISSUES (OneDrive sync client with SharePoint library)
│   ├─ Has the library been added to OneDrive sync?
│   │   → User should have clicked "Sync" on the SharePoint library page
│   │   → Check OneDrive Settings → Account → Which libraries are syncing?
│   │   ├─ Library not listed → Add it: Open SharePoint → Library → Sync
│   │   └─ Library listed but showing errors → Continue below
│   │
│   ├─ Sync limits check:
│   │   ├─ Are there > 5000 items in the library root?
│   │   │   → OneDrive has a 5000-item view limit
│   │   │   → Sync only sub-folders, or organize into fewer items
│   │   ├─ Is the total synced size > 300 GB?
│   │   │   → OneDrive has a 300 GB sync limit (per library)
│   │   └─ Are there file type restrictions?
│   │       → Certain extensions (.pst, .one) don't sync via OneDrive
│   │
│   ├─ Stop sync and re-sync:
│   │   OneDrive Settings → Account → Select library → Stop sync
│   │   → Restart OneDrive → Go to SharePoint → Click Sync again
│   │
│   └─ Check for file conflicts:
│       → OneDrive icon → View sync problems
│       → Common: files checked out, file name too long, invalid characters
│
├─ PERMISSION ISSUES
│   ├─ Does the user have access to the specific document library?
│   │   → SharePoint sites can have unique permissions per library
│   ├─ Does the user have "Contribute" or "Read" level access?
│   │   → "Read" can view but not sync (sync needs at least "Contribute")
│   └─ Is the site part of a Microsoft 365 Group?
│       → Membership controls access → Check Entra ID group membership
│
└─ STILL BROKEN?
    ├─ Clear Office credential cache:
    │   → Control Panel → Credential Manager → Windows Credentials
    │   → Remove all SharePoint/OneDrive entries
    │   → Re-sign into OneDrive
    └─ Reset OneDrive: `%localappdata%\Microsoft\OneDrive\OneDrive.exe /reset`
        → [see 02-05](02-05_email-collab_onedrive-not-syncing.md)
```

**RESULT** → SharePoint accessible and syncing correctly.
