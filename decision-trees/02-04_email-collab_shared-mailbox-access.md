# Shared Mailbox Access Issues

User cannot access a shared mailbox or sees an error when trying to open it.

```
User reports: "I can't open the shared mailbox [support@/sales@/etc.]"

├─ HAS THE MAILBOX BEEN ADDED?
│   ├─ User hasn't added it → Add it:
│   │   ├─ Outlook: File → Account Settings → Account Settings → Select email → Change
│   │   │   → More Settings → Advanced → Add → Type shared mailbox name
│   │   └─ Outlook on web: Right-click Folders → Add shared folder
│   │
│   └─ User added it but can't see it → Proceed below
│
├─ PERMISSIONS CHECK
│   ├─ Is the user granted access to the shared mailbox?
│   │   └─ Exchange Admin Center → Recipients → Shared mailboxes
│   │       → Select the mailbox → Mailbox delegation
│   │       ├─ User is listed under "Full Access"?
│   │       │   ├─ YES → Permissions are correct
│   │       │   └─ NO  → Click + → Add user → Save
│   │       │       → The user may need to close/reopen Outlook
│   │       └─ User also needs "Read and manage" (Send As or Send on Behalf)?
│   │           → Also add under "Send As" or "Send on Behalf" if required
│   │
│   └─ How long since the user was added?
│       └─ Permissions can take 15-60 min to propagate
│           → Try `gpupdate /force` if AD permissions are involved
│
├─ OUTLOOK AUTO-MAPPING
│   ├─ Is the mailbox auto-mapping to Outlook automatically?
│   │   (Auto-mapping happens when Full Access is granted)
│   │   ├─ YES → Shared mailbox should appear in Outlook folder list
│   │   └─ NO  → Auto-mapping may be blocked
│   │       → Check: Is `Features\AutoMapping` set to $false on the mailbox?
│   │       → PowerShell: `Set-Mailbox -Identity "Shared" -AutoMapping $true`
│   │
│   └─ If auto-mapping is disabled (by design for large orgs):
│       → User must add manually (see step 1)
│
├─ OUTLOOK CACHE ISSUES
│   ├─ Try: Remove the shared mailbox from Outlook → Restart → Re-add
│   ├─ Try: Create a new Outlook profile
│   └─ Try: Check if the shared mailbox is over the OST size limit
│       → Shared mailboxes > 10 GB can fail to sync
│       → Use: Control Panel → Mail → Show Profiles → New profile for testing
│
└─ STILL BROKEN?
    └─ Test webmail: Sign in to outlook.office.com → Can the user see the shared mailbox?
        ├─ YES → Issue is Outlook client → Create new profile
        └─ NO  → Issue is permissions or server-side
            → Verify permissions applied correctly (try removing and re-adding)
            → Escalate to Exchange admin
```

**RESULT** → Shared mailbox accessible. User can read and send as the shared mailbox.
