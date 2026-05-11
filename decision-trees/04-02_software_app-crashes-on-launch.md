# Application Crashes on Launch

Application starts then crashes immediately, or gives a "has stopped working" error on launch.

```
User reports: "I open [app] and it crashes right away"

├─ ISOLATE THE CRASH
│   ├─ Event Viewer → Application log → Look for Event ID 1000
│   │   → Note the "Faulting module name" (e.g., ntdll.dll, kernel32.dll)
│   │   → Note the "Exception code" (e.g., 0xc0000005)
│   │   → Search these two things + app name
│   │
│   ├─ Does the app work in Safe Mode?
│   │   → Reboot → Safe Mode → Launch app
│   │   ├─ Works → Startup program, service, or driver is interfering
│   │   │   → Perform clean boot (msconfig → Selective startup → Uncheck all services)
│   │   │   → Re-enable services/apps one by one to find the conflict
│   │   └─ Fails in Safe Mode → App itself is corrupt or incompatible
│   │
│   └─ Has it ever worked on this machine?
│       ├─ YES → What changed? (Windows update, app update, new software?)
│       └─ NO  → May be incompatible with this Windows version
│
├─ COMMON FIXES
│   ├─ Run the app as Administrator (right-click → Run as administrator)
│   ├─ Run in compatibility mode:
│   │   → Right-click app → Properties → Compatibility
│   │   → Try: "Run this program in compatibility mode for:"
│   │   → Try: Windows 8, Windows 7, or the OS it was designed for
│   │
│   ├─ Reinstall the application:
│   │   → Settings → Apps → Uninstall → Re-download fresh copy → Install
│   │
│   └─ Check for and install updates for the application
│
├─ SPECIFIC CRASH PATTERNS
│   ├─ **Faulting module: ntdll.dll**
│   │   → Often indicates system corruption or memory issue
│   │   → Run `sfc /scannow` and `DISM /Online /Cleanup-Image /RestoreHealth`
│   │   → Run Windows Memory Diagnostic
│   │
│   ├─ **Faulting module: nvlddmkm.sys / atikmdag.sys / igdumdim64.dll**
│   │   → Graphics driver crash
│   │   → Update GPU driver from manufacturer
│   │   → Roll back GPU driver if recently updated
│   │   → Check for overheating [see 06-04]
│   │
│   ├─ **Exception code: 0xc0000005 (Access Violation)**
│   │   → App tried to access invalid memory
│   │   → [see cheatsheet-error-codes.md](../cheatsheets/cheatsheet-error-codes.md)
│   │   → Update drivers, run memory diagnostics
│   │
│   └─ **"Side-by-side configuration is incorrect"**
│   │   → Missing or corrupt VC++ Runtime
│   │   → Install/reinstall the correct Visual C++ Redistributable
│   └─
│
├─ DEP / DATA EXECUTION PREVENTION
│   └─ If the crash mentions DEP:
│       → System Properties → Advanced → Performance → Data Execution Prevention
│       → Add the app as an exception ("Turn on DEP for all programs except:")
│
└─ STILL CRASHING?
    └─ Check application vendor's support site for known issues
        → Search: "[app name] crashing on Windows 11 [build]"
        → Escalate to application owner / vendor support
```

**RESULT** → Application launches and runs stably.
