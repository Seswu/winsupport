# System Freezing Intermittently

The computer becomes unresponsive for seconds at a time — mouse moves then stops, audio stutters, everything pauses.

```
User reports: "My computer keeps freezing for a few seconds"

├─ ISOLATE THE PATTERN
│   ├─ Does the freeze happen at regular intervals?
│   │   └─ Every X minutes? → Likely a scheduled task or scan
│   │       → Check Task Scheduler → Look for recurring tasks at freeze times
│   │       → Check Defender scan schedule
│   │
│   ├─ Does the freeze happen during specific actions?
│   │   └─ Opening a file? Saving? Switching apps?
│   │       → Likely disk-related or application-specific
│   │
│   └─ Does the freeze happen even at idle?
│   │   └─ When the user leaves the computer then comes back?
│   │       → Power saving or sleep mode issue
│   └─
│
├─ CHECK RESOURCE MONITOR
│   ├─ Open: `perfmon /res` (Resource Monitor)
│   │   → Watch during a freeze:
│   │   ├─ **Disk** → Queue Length spikes? → [see 06-02]
│   │   ├─ **CPU** → One core pinned at 100%?
│   │   ├─ **Memory** → Hard faults/second > 10? → Not enough RAM
│   │   └─ **Network** → Activity spikes? → App phoning home
│   │
│   └─ After a freeze, check Event Viewer:
│       → Application log → Any errors at the freeze timestamp?
│       → System log → Look for disk errors or driver failures
│
├─ COMMON CAUSES
│   ├─ **Drive issue** (HDD going bad, SSD firmware bug)
│   │   → Check drive health: `wmic diskdrive get status`
│   │   → Check Event Viewer for "disk" errors in System log
│   │
│   ├─ **Driver issue** (especially network, storage, or chipset)
│   │   → Update all critical drivers
│   │   → Check for yellow exclamation marks in Device Manager
│   │
│   ├─ **Thermal throttling** (CPU/GPU overheating)
│   │   → Check if fans are running → Clean dust from vents
│   │   → [see 06-04](06-04_performance_overheating.md)
│   │   → Use HWMonitor or similar to check temperatures
│   │
│   ├─ **Memory pressure** (not enough RAM, system is paging heavily)
│   │   → Hard fault rate in Resource Monitor > 10/s
│   │   → Close Chrome tabs, Office documents → test
│   │   → Consider adding more RAM
│   │
│   ├─ **Background updates**
│   │   → Windows Update, Store updates, app updates
│   │   → Check Windows Update → Install pending → Reschedule
│   │
│   └─ **Antivirus scanning**
│   │   → Check if Defender or third-party AV is scanning
│   │   → Exclude common folders from real-time scanning
│   └─
│
├─ QUICK TESTS
│   ├─ Reboot → Does the freeze come back immediately?
│   │   ├─ NO / takes hours → Maybe a memory leak that builds over time
│   │   └─ YES immediately → Likely a driver or startup program
│   │
│   ├─ Boot Safe Mode → Does the freeze happen there?
│   │   ├─ NO → Third-party driver or service is the cause
│   │   │   → Clean boot (msconfig) to isolate
│   │   └─ YES → Possibly hardware fault or core driver issue
│   │
│   └─ Run hardware diagnostics:
│       → Windows Memory Diagnostic (`mdsched.exe`) → Check for RAM errors
│       → `chkdsk C: /scan` → Check for disk errors
│
└─ STILL FREEZING?
    └─ Check for known issues with this specific Windows build:
        → `winver` → Note the build number
        → Search: "Windows 11 [build] freezing issues"
        → Escalate to Tier 2 with freeze pattern and diagnostic data
```

**RESULT** → Freezing identified and resolved, or escalated with diagnostic data.
