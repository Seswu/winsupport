# Update Rolled Back Repeatedly

Windows Update installs, then during reboot it says "Undoing changes" and rolls back — repeatedly.

```
User reports: "Windows keeps trying to update but then rolls back"

├─ CHECK THE LOOP
│   ├─ This means the update installs but fails during the boot phase (driver, hardware, or compatibility)
│   ├─ Windows detects the failure automatically and rolls back to the previous build
│   ├─ Next time it tries again → same failure → same rollback → infinite loop
│
│   └─ BREAK THE LOOP:
│       → Settings → Windows Update → Pause updates (for 7 days)
│       → This gives time to investigate without the machine trying again
│
├─ IDENTIFY THE FAILING UPDATE
│   ├─ Settings → Windows Update → Update history
│   │   → Which update failed?
│   │   └─ Is it:
│   │       ├─ **A regular monthly cumulative update** (KB5xxxxxx)
│   │       │   → Known issue with this specific KB? Search the KB number
│   │       ├─ **A .NET Framework update**
│   │       │   → Run .NET Framework repair tool
│   │       │   → dotnet.microsoft.com/download/dotnet-framework-repair
│   │       ├─ **A driver update**
│   │       │   → Likely incompatible driver
│   │       │   → Hide this specific update: `wushowhide.diagcab` (Microsoft tool)
│   │       └─ **A feature update** (22H2 → 23H2)
│   │           → [see 07-03](07-03_updates_feature-update-fails.md)
│   │
│   └─ Check the CBS log for the failing component:
│       → `C:\Windows\Logs\CBS\CBS.log` → Search for "Error" near the end
│
├─ RESOLVE
│   ├─ **Roll back the problematic update manually**:
│   │   → Settings → Windows Update → Update history → Uninstall updates
│   │   → Select the KB → Uninstall → Reboot
│   │   → Hide it with `wushowhide.diagcab` so it doesn't reinstall
│   │
│   ├─ **Run the update troubleshooter**:
│   │   → [see 07-01](07-01_updates_update-stuck.md) — especially DISM and SFC
│   │
│   ├─ **Clean the component store**:
│   │   → `DISM /Online /Cleanup-Image /StartComponentCleanup`
│   │   → `DISM /Online /Cleanup-Image /RestoreHealth`
│   │   → `sfc /scannow`
│   │
│   ├─ **Check for incompatible software**:
│   │   → Boot into Safe Mode → Does the update succeed there?
│   │   ├─ YES → Third-party software/driver is blocking
│   │   │   → Clean boot (msconfig) → Isolate the cause
│   │   └─ NO  → System-level issue (DISM/SFC needed)
│   │
│   └─ **Check disk space**:
│   │   → Updates need room for the rollback image too
│   │   → Need > 20 GB free for feature update rollback protection
│   └─
│
├─ MANUAL INSTALL
│   └─ Download the specific KB from Microsoft Update Catalog
│       → Manually install the .msu → Observe the exact error code
│       → That error code tells you the real problem
│
└─ STILL LOOPING?
    └─ Escalate to Tier 2:
        → Note: KB number, Windows build, DISM/SFC results
        → Possible solutions: Repair install via media, or in-place upgrade
```

**RESULT** → Update loop broken, issue identified, update either installed or hidden.
