# Application Update Failed

An application update fails to install — the update downloads but then errors out or rolls back.

```
User reports: "I tried to update [app] but it failed"

├─ WHAT TYPE OF UPDATE?
│   ├─ Application's built-in updater (check for updates inside the app)
│   ├─ Microsoft Store update (Microsoft Store → Library → Get updates)
│   ├─ Intune / Company Portal deployment
│   └─ Manual installer (downloaded .exe or .msi from vendor)
│
├─ COMMON FIXES
│   ├─ Close the application completely:
│   │   → Task Manager → End task on the application
│   │   → Some updaters fail if the app process is running
│   │
│   ├─ Run the updater as Administrator
│   │
│   ├─ Check disk space:
│   │   → Application updates may need significant temporary space
│   │   → Clean up temp files, empty recycle bin
│   │
│   ├─ Disable antivirus temporarily:
│   │   → AV may quarantine update files
│   │
│   └─ Check %TEMP% folder for leftover update files:
│   │   → Clear %TEMP% → retry update
│   └─
│
├─ MICROSOFT STORE UPDATES
│   ├─ Reset the Microsoft Store cache:
│   │   → Run: `wsreset.exe`
│   │   → Store will reopen → try update again
│   │
│   ├─ Check for Windows updates:
│   │   → Some Store apps require a specific Windows version
│   │   → Settings → Windows Update → Install pending updates
│   │
│   └─ Sign out of Store and sign back in:
│       → Store → Click profile picture → Sign out → Sign back in → retry
│
├─ THIRD-PARTY APP UPDATES
│   ├─ Download the full installer from the vendor website
│   │   → Some "updates" are only delta patches and require the prior version
│   │   → A full installer can be run as a fresh install
│   │
│   ├─ Uninstall the current version first:
│   │   → Settings → Apps → Uninstall
│   │   → Install the latest version fresh
│   │
│   └─ Check vendor's known issues page:
│       → Search: "[app name] update fails with [error code]"
│
├─ INTRUNE / MANAGED UPDATES
│   ├─ The update may be deployed automatically by IT
│   │   → Check if the app has a pending update in Company Portal
│   │   → Settings → Accounts → Access work or school → Sync → retry
│   │
│   └─ If an Intune deployment failed:
│       → Check: `%ProgramData%\Microsoft\IntuneManagementExtension\Logs`
│       → Look for the error → escalate to Intune admin
│
└─ STILL FAILING?
    └─ Note the error code → cross-reference with cheatsheet-error-codes.md
        → Escalate to Tier 2 / app owner
        → If the app is currently working, "update failed" may not be urgent
        → Some updates can be deferred until the vendor addresses the issue
```

**RESULT** → Application updated successfully, or deferred with known issue noted.
