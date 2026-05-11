# Calendar Not Syncing

User's calendar shows different data on different devices, or changes aren't appearing for others.

```
User reports: "My calendar isn't updating" or "Other people can't see my availability"

├─ SCOPE CHECK
│   ├─ Is the issue on one device or all?
│   │   ├─ ONE device → Local cache issue
│   │   │   └─ Repair OST: Close Outlook → Rename .ost file
│   │   │       → HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Outlook\Profiles
│   │   │       → Find profile path → rename .ost → restart Outlook
│   │   └─ ALL devices → Server-side issue
│   │
│   ├─ Can other users see the user's calendar?
│   │   ├─ NO → Permissions issue → Check folder permissions
│   │   └─ YES → Issue is likely permissions at the user's end
│   │
│   └─ Is the issue with a shared calendar (someone else's)?
│       ├─ YES → [see 02-04](02-04_email-collab_shared-mailbox-access.md)
│       └─ NO
│
├─ OUTLOOK-SPECIFIC FIXES
│   ├─ Switch between cached and online mode:
│   │   File → Account Settings → Account Settings → Select account → Change
│   │   → "Use Cached Exchange Mode" → Uncheck → Restart → Re-check
│   ├─ Update calendar folder view: Go to Calendar → View → Change View → List
│   │   → See if items exist but aren't rendering
│   ├─ Calendar properties → Permissions → Check + Add "Default" with "Free/Busy time"
│   └─ Run `outlook.exe /resetfoldernames` → resync folder mappings
│
├─ WEBMAIL CHECK
│   └─ Does the calendar work in outlook.office.com?
│       ├─ NO → Server-side issue → Check service health
│       └─ YES → Issue is local to Outlook client
│
├─ EXCHANGE ADMIN CHECKS
│   ├─ Check mailbox size: Is the user near their quota?
│   │   → Mailbox full can prevent sync
│   ├─ Check calendar folder size: `Get-MailboxFolderStatistics` (PowerShell)
│   └─ Check if a retention policy or litigation hold is affecting items
│
└─ ROOT CAUSE
    ├─ Corrupt calendar item (one bad meeting in the series)
    │   → Use webmail → Calendar → View → Change View → List
    │   → Delete suspicious items from the date the problem started
    ├─ Calendar sharing permissions removed inadvertently
    │   → Re-add permissions via Outlook or Exchange Admin Center
    └─ Delegate access issues → Remove and re-add delegate
```

**RESULT** → Calendar syncing across all devices.
