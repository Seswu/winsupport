# Disk at 100% Usage

Task Manager shows disk activity at 100% continuously, even when the user isn't actively doing anything.

```
User reports: "My computer is slow" — Task Manager shows Disk at 100%

├─ CONFIRM
│   └─ Task Manager → Performance tab → Disk → Active time at 100%?
│       ├─ NO  → Slowness is from CPU/RAM/network → refer to those tracks
│       └─ YES → Continue
│
├─ WHICH PROCESS IS CAUSING IT?
│   └─ Processes tab → Sort by Disk (descending)
│       ├─ **"System" or "System Interrupts"**
│       │   → Likely driver or HDD issue
│       │   ├─ Check if the drive is HDD vs SSD:
│       │   │   → HDD: Run `chkdsk C: /scan` for bad sectors
│       │   │   → SSD: Check SATA/AHCI driver is correct
│       │   └─ Update chipset/storage drivers
│       │
│       ├─ **Microsoft Windows Search Indexer** (SearchIndexer.exe)
│       │   → Indexing after large file addition or rebuild
│       │   ├─ Wait for it to finish (10-30 min)
│       │   └─ If persistent → Settings → Search → Searching Windows
│       │       → Advanced → Rebuild index → Or add exclusions
│       │
│       ├─ **Microsoft Antimalware Service** (MsMpEng.exe)
│       │   → Windows Defender scanning
│       │   ├─ Check if a scan is running → Let it complete
│       │   └─ If scanning constantly → Add folder exclusions
│       │       → Windows Security → Virus & threat protection
│       │       → Manage settings → Exclusions → Add exclusion
│       │
│       ├─ **Windows Update** (TiWorker.exe / TrustedInstaller.exe)
│       │   → Updates being installed in background
│       │   ├─ Open Windows Update → Are updates pending?
│       │   └─ Reboot to finish pending updates
│       │
│       ├─ **OneDrive** (OneDrive.exe)
│       │   → Files syncing in the background
│       │   → [see 02-05](02-05_email-collab_onedrive-not-syncing.md)
│       │
│       ├─ **Cortana / Search UI** (SearchApp.exe / Cortana.exe)
│       │   → Known issue on some builds → Disable search indexing
│       │
│       └─ **svchost.exe** (generic host)
│           → Expand in Task Manager → Which service?
│           ├─ wuauserv → Windows Update (see above)
│           ├─ BITS → Background transfers
│           ├─ SysMain (Superfetch) → Disable on HDDs via Services.msc
│           └─ WSearch → Windows Search Indexer (see above)
│
├─ HDD-SPECIFIC FIXES
│   ├─ Disable SysMain (Superfetch) and Windows Search:
│   │   → `services.msc` → Find both services → Right-click → Properties
│   │   → Startup type: Disabled → Stop the service → Reboot
│   │
│   ├─ Increase virtual memory / page file:
│   │   → System Properties → Advanced → Performance → Virtual memory
│   │   → Set to 1.5x the amount of RAM
│   │
│   └─ Run `chkdsk C: /f /r` (schedule at next reboot)
│
├─ SSD-SPECIFIC FIXES
│   ├─ Check if the SSD firmware is up to date
│   ├─ Run Trim manually: `optimize-volume -DriveLetter C -ReTrim -Verbose`
│   ├─ Check if the drive supports TRIM: `fsutil behavior query DisableDeleteNotify`
│   │   → Should be 0 (TRIM enabled)
│   └─ Update SSD driver (NVMe driver from manufacturer)
│
├─ TEMPORARY MITIGATION
│   └─ `fsutil behavior set disablelastaccess 1`
│       → Reduces metadata writes when files are accessed
│       → (Safe to disable on modern systems)
│
└─ STILL AT 100%?
    └─ Run `perfmon /rel` → Check for disk-related failures
        → Check disk health: `wmic diskdrive get status` → "Bad" = failing
        → Escalate: drive replacement or hardware diagnostic
```

**RESULT** → Disk usage normalized. Process causing the bottleneck identified.
