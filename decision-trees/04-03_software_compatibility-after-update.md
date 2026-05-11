# App Compatibility After Windows Update

An application that was working stopped working after a Windows Update was applied.

```
User reports: "Ever since Windows updated, [app] doesn't work right"

├─ CHECK WHAT CHANGED
│   ├─ View recent updates:
│   │   Settings → Windows Update → Update history
│   │   → Note the KB number of the most recent update
│   │   → Search: "[KB number] [app name] compatibility"
│   │
│   └─ Is there a known issue with this specific update?
│       → Check Microsoft's known issues list
│       → Check the app vendor's support page
│         (Many vendors proactively publish compatibility notes)
│
├─ COMMON FIXES
│   ├─ Run compatibility troubleshooter:
│   │   → Right-click app → Properties → Compatibility
│   │   → Run compatibility troubleshooter → Let it detect issues
│   │
│   ├─ Run the app as Administrator
│   │
│   ├─ Set compatibility mode manually:
│   │   → Properties → Compatibility → "Run this program in compatibility mode for:"
│   │   → Try the previous Windows version
│   │
│   └─ Check if .NET / VC++ runtime needs repair:
│       → Windows updates sometimes break runtime components
│       → Repair: Settings → Apps → Microsoft .NET / VC++ Redist → Modify → Repair
│
├─ ROLL BACK THE UPDATE (temporary measure)
│   └─ Only if the app is business-critical and there's no fix yet:
│       → Settings → Windows Update → Update history → Uninstall updates
│       → Select the KB that caused the issue → Uninstall → Reboot
│       → PAUSE updates temporarily while waiting for a fix
│
├─ APP VENDOR PATCH
│   └─ Check if the application vendor has released a patch:
│       → Download and install the latest version of the app
│       → Check for: in-app updater, vendor website, LTS branch
│
└─ STILL BROKEN?
    └─ Report to Tier 2 / app owner:
        - App name and version
        - Windows build number (`winver`)
        - KB number that triggered the issue
        - Error message or crash code
```

**RESULT** → Application functional again, or workaround applied pending vendor patch.
