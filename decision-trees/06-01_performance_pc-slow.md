# PC Is Slow / Takes Forever to Boot

User reports that their computer is sluggish, takes a long time to start, or feels unresponsive.

```
User reports: "My computer is so slow today"

├─ QUICK WINS
│   ├─ Restart the computer (clears memory leaks, stuck processes, cached temp data)
│   ├─ Check if it's a specific user or all users on the machine
│   │   → If one user → Corrupt user profile → Create new profile
│   └─ Ask: "When did this start?"
│       → "After an update" → [see 04-03](04-03_software_compatibility-after-update.md)
│       → "After installing [app]" → Uninstall that app → test
│       → "This morning" → Check for overnight Windows Update
│
├─ TASK MANAGER (Ctrl+Shift+Esc)
│   ├─ Performance tab → Which resource is maxed out?
│   │   ├─ **Disk 100%** → [see 06-02](06-02_performance_disk-100-percent.md)
│   │   ├─ **Memory > 90%** → Close apps, check for memory leaks
│   │   │   → `tasklist /fi "memusage gt 500000"` → Find high consumers
│   │   │   → Consider adding more RAM if machine allows
│   │   ├─ **CPU 100%** → Check which process is using CPU
│   │   │   → Malware scan? Windows Update? Search indexing?
│   │   │   → End the process if it's stuck → Reboot if needed
│   │   └─ **Network > 50%** → Something is downloading
│   │       → Check: Windows Update, OneDrive sync, antivirus update
│   │
│   └─ Startup tab → Check startup impact:
│       → Disable high-impact startup programs that aren't needed
│       → (Right-click → Disable does NOT uninstall the app)
│
├─ BOOT TIME
│   ├─ If slow boot:
│   │   → `perfmon /rel` → Reliability Monitor → Check boot-related failures
│   │   → Task Manager → Startup → Disable unnecessary startup apps
│   │   → Settings → Apps → Startup → Turn off unused apps
│   │   → Check if Fast Startup is enabled:
│   │       Settings → System → Power → Additional power settings
│   │       → Choose what power buttons do → Change unavailable settings
│   │       → Fast Startup → ON (recommended for HDD, can cause issues on SSD)
│   │
│   └── Check Event Viewer for boot performance events:
│       → Event ID 100 (Diagnostic-Performance) → Shows boot duration in ms
│
├─ DISK SPACE
│   ├─ Is the C: drive almost full?
│   │   → [see 06-05](06-05_performance_low-disk-space.md)
│   └─ Run Disk Cleanup:
│   │   → `cleanmgr` → Select C: drive → Clean up system files
│   │   → Check: Windows Update Cleanup, Temporary files, Recycle Bin
│   └─
│
├─ VISUAL EFFECTS
│   └─ System Properties → Advanced → Performance → Settings
│       → "Adjust for best performance" (disables animations, transparency, etc.)
│       → Or: Custom → Uncheck individual effects
│
├─ MALWARE CHECK
│   └─ Run a full antivirus scan:
│       → If Windows Defender: Settings → Privacy & Security → Windows Security
│       → Virus & threat protection → Scan options → Full scan
│       → [see 08-02](08-02_security_antivirus-false-positive.md)
│
└─ STILL SLOW?
    └─ Check if the machine has an SSD vs HDD:
        → Task Manager → Performance → Disk → Model name
        ├─ HDD → Upgrade to SSD (dramatic improvement)
        └─ SSD → Check drive health with manufacturer tool
            → Escalate to hardware team if drive health is failing
```

**RESULT** → Performance improved, root cause identified.
