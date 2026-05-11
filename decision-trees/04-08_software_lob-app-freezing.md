# LOB App Freezing During Operations

A line-of-business application freezes or becomes unresponsive during normal use — not on launch, but while the user is working in it.

```
User reports: "[App] freezes after I do [operation]"

├─ IS IT ALWAYS THE SAME OPERATION?
│   ├─ YES → Likely the app has a bug in that specific feature
│   │   → Check if other users experience the same freeze
│   │   → Document the exact steps to reproduce
│   │   → Report to application owner / vendor
│   │
│   └─ NO → Random freezes → Likely environmental (machine resources, network, etc.)
│
├─ IMMEDIATE TRIAGE
│   ├─ Try to unfreeze before killing:
│   │   → Wait 30 seconds (some processes are just slow, not frozen)
│   │   → Try clicking in the window (may wake a hung thread)
│   │   → Try minimizing and restoring the window
│   │
│   ├─ If truly frozen → End task:
│   │   → Task Manager → Processes → Select the app → End task
│   │   → Check if any child processes remain → End those too
│   │   → Restart the application
│   │
│   └─ After restart → Can the user continue?
│       → If data was lost → Check auto-save or temp file locations
│       → (Many LOB apps have auto-save; show the user how to find it)
│
├─ RESOURCE CHECK
│   ├─ Open Task Manager → Performance tab:
│   │   ├─ **CPU** → Is it pegged at 100%?
│   │   │   → [see 06-01](06-01_performance_pc-slow.md)
│   │   ├─ **Memory** → Is it near 100%?
│   │   │   → Close other applications → test
│   │   ├─ **Disk** → Is it at 100%?
│   │   │   → [see 06-02](06-02_performance_disk-100-percent.md)
│   │   └─ **Network** → Is the app sending/receiving data?
│   │       → Network spikes during freeze = app waiting for server response
│   │
│   └─ Check while the app is frozen:
│       → Processes tab → Sort by CPU/Memory/Disk → Which process is consuming?
│       → Note the pattern (always uses disk? always uses network?)
│
├─ NETWORK / DATABASE
│   ├─ Does the app communicate with a database or server?
│   │   └─ Freeze may be the app waiting for a network response
│   │       → `ping <app-server>` → high latency or loss?
│   │       → Check VPN status [see 03-02]
│   │       → Check database connectivity
│   │
│   └─ Does the freeze happen during specific actions?
│       → "Save" → Database write timeout
│       → "Search" → Query timeout (large dataset)
│       → "Generate report" → Server-side processing delay
│
├─ COMMON FIXES
│   ├─ Reinstall the application
│   ├─ Check for application updates (vendor may have fixed the issue)
│   ├─ Update critical drivers (especially GPU for rendering apps)
│   └─ Clear application cache (varies by app — check vendor docs)
│
└─ STILL FREEZING?
    └─ Report to app owner / vendor support with:
        - Exact steps to reproduce the freeze
        - Screenshot of Task Manager during the freeze
        - Event Viewer logs from Application log at the time of the freeze
        - Does it always happen at a specific time of day? (server load?)
```

**RESULT** → Application stable, or issue reported to app owner for investigation.
