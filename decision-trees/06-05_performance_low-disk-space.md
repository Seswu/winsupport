# Low Disk Space Warning

Windows warns that the C: drive is running out of space, or the user reports they can't save files.

```
User reports: "I'm getting a 'low disk space' warning"

├─ CHECK THE CURRENT FREE SPACE
│   └─ File Explorer → This PC → C: drive → Free space
│       ├─ < 10 GB → Urgent: may prevent Windows Update and system function
│       ├─ 10-25 GB → Moderate: will need attention soon
│       └─ > 25 GB → Warning may be for a specific folder (OneDrive, temp)
│
├─ FREE UP SPACE (try in order)
│   ├─ **Run Disk Cleanup**:
│   │   → `cleanmgr` → Select C: drive
│   │   → Click "Clean up system files" → Check everything:
│   │       ├─ Windows Update Cleanup (often biggest — 2-10 GB)
│   │       ├─ Delivery Optimization Files
│   │       ├─ Recycle Bin
│   │       ├─ Temporary Internet Files
│   │       ├─ Temporary files
│   │       ├─ Windows Error Reporting
│   │       └─ Previous Windows installation(s) (Windows.old — can be 10-20 GB)
│   │
│   ├─ **Empty Recycle Bin**:
│   │   → Right-click Recycle Bin → Empty Recycle Bin
│   │
│   ├─ **Clear Temp Folders**:
│   │   → Run: `%temp%` → Select all → Delete (skip files in use)
│   │   → Run: `temp` → Select all → Delete
│   │   → Run: `prefetch` → Select all → Delete
│   │
│   ├─ **Run Storage Sense** (automated cleanup):
│   │   → Settings → System → Storage → Storage Sense → Turn on
│   │   → Or click "Run Storage Sense now"
│   │
│   ├─ **Move personal files** to another drive:
│   │   → Documents, Downloads, Desktop, Pictures
│   │   → Settings → System → Storage → "Change where new content is saved"
│   │   → Set all to D: drive (if available)
│   │
│   ├─ **Uninstall unused applications**:
│   │   → Settings → Apps → Installed apps
│   │   → Sort by size (largest first)
│   │   → Uninstall anything not needed
│   │
│   └─ **Disable Hibernate** (if not used):
│       → `powercfg /hibernate off` → frees up space equal to RAM size
│       → (Don't do this if the user uses sleep/hibernate)
│
├─ BULKY CULPRITS
│   ├─ **Windows.old folder** (after a major Windows update)
│   │   → If user hasn't needed to roll back > 10 days → Safe to remove
│   │   → Disk Cleanup → Clean up system files → Previous Windows installation(s)
│   │
│   ├─ **OneDrive files kept locally**
│   │   → Right-click OneDrive → Settings → Account → Choose folders
│   │   → Uncheck folders that can be "online-only"
│   │   → Files on Demand: Right-click file → "Free up space" (online-only)
│   │
│   ├─ **Windows Component Store (WinSxS)**
│   │   → Run: `DISM /Online /Cleanup-Image /StartComponentCleanup`
│   │   → (This is safe and doesn't break anything)
│   │
│   └─ **Temporary Windows Update files**
│   │   → Disk Cleanup already handles this
│   └─
│
├─ EXPANDING STORAGE
│   ├─ Add an external drive
│   ├─ Use cloud storage (OneDrive files on demand)
│   └─ Upgrade internal SSD (escalate to hardware team)
│
└─ STILL LOW AFTER CLEANUP?
    └─ Check for large files with `dir /s /a | sort /R` or WizTree/Windirstat
        → Look for: pagefile.sys (manage via Virtual Memory settings)
        → hiberfil.sys (manage via powercfg)
        → Large user profile → Investigate Downloads, Desktop, AppData
```

**RESULT** → Free space recovered, C: drive has sufficient room.
