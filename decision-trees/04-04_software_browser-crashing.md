# Browser Keeps Crashing

The web browser (Chrome, Edge, Firefox) freezes, crashes, or becomes unresponsive.

```
User reports: "My browser keeps crashing" or "tabs keep freezing"

├─ IS IT ONE BROWSER OR ALL?
│   ├─ One browser → Issue is browser-specific
│   └─ All browsers → Issue is system-wide (driver, memory, or network)
│       → [see 06-03](06-03_performance_system-freezing.md) for system-level issues
│       → Run memory diagnostic if crashes occur in all browsers
│
├─ BROWSER-SPECIFIC FIXES (try in order)
│   ├─ Clear browsing data:
│   │   → Settings → Privacy and security → Clear browsing data
│   │   → Clear: cache, cookies, site data (keep passwords)
│   │   → Focus on cache (corrupt cache is a common crash cause)
│   │
│   ├─ Disable extensions:
│   │   → Settings → Extensions → Disable all
│   │   → Restart the browser
│   │   ├─ Stops crashing? → Re-enable extensions one by one
│   │   │   → Find the culprit → Remove it
│   │   └─ Still crashing → Continue
│   │
│   ├─ Create a new browser profile:
│   │   → Chrome: Settings → You and Google → Your profile → Sign out
│   │   → Create a new local profile (no sync)
│   │   → If stable → Import data from old profile gradually
│   │
│   ├─ Reset browser settings:
│   │   → Chrome: Settings → Reset settings → Restore to original defaults
│   │   → Edge: Settings → Reset settings → Restore settings to default
│   │   → (This disables extensions, resets startup page, clears temporary data)
│   │
│   ├─ Disable hardware acceleration:
│   │   → Settings → System and performance → "Use hardware acceleration when available"
│   │   → Turn OFF → Restart browser
│   │   → (Graphics driver issues often cause browser crashes with HW accel on)
│   │
│   └─ Repair / reinstall:
│   │   → Chrome: Reinstall from google.com/chrome (settings may be preserved)
│   │   → Edge: Settings → Apps → Microsoft Edge → Modify → Repair
│   └─
│
├─ COMMON CAUSES
│   ├─ **Outdated GPU driver**
│   │   → Update GPU driver → test
│   │
│   ├─ **Browser profile corruption**
│   │   → Create new profile (see above)
│   │
│   ├─ **Out of memory (RAM)**
│   │   → Close other applications → test
│   │   → Check Task Manager → Memory usage
│   │
│   ├─ **Malware / browser hijacker**
│   │   → Run antivirus scan [see 08-02]
│   │   → Check browser shortcuts: right-click → Properties → Target
│   │   → Should end with `chrome.exe"` (not extra URL appended)
│   │
│   └─ **Too many tabs/windows open**
│   │   → Memory pressure from too many open tabs
│   │   → Use tab sleeping features
│   └─
│
└─ STILL CRASHING?
    └─ Check browser crash logs:
        → Chrome: chrome://crashes
        → Edge: edge://crashes
        → Note the crash ID → search for known issues
        → Escalate if severe
```

**RESULT** → Browser stable and responsive.
