# OneDrive Not Syncing / Red X on Files

Files show a red X, a sync error icon, or OneDrive reports "Changes are being processed."

```
User reports: "My OneDrive isn't syncing" or "files have red X's"

├─ IS ONEDRIVE RUNNING?
│   ├─ Check system tray (notification area) → OneDrive cloud icon
│   │   ├─ Cloud icon not there → OneDrive not running
│   │   │   → Start Menu → OneDrive → Launch
│   │   │   → If it doesn't start → Run `%localappdata%\Microsoft\OneDrive\OneDrive.exe`
│   │   ╰   — If still won't start → Repair OneDrive
│   │       Settings → Apps → Microsoft OneDrive → Modify → Repair
│   │   ├─ Cloud icon with a cross → Syncing paused or stopped
│   │   ├─ Cloud icon with a warning → Errors with specific files
│   │   └─ Cloud icon (solid) → Syncing normally
│   │
│   └─ Check sync icon in File Explorer → Does it show "online-only" vs "available"?
│
├─ PAUSE AND RESUME
│   ├─ Right-click OneDrive icon → Pause syncing → 2 hours
│   ├─ Wait 30 seconds → Right-click → Resume syncing
│   └─ This clears transient sync queue issues
│
├─ CHECK FOR FILE CONFLICTS
│   ├─ Right-click OneDrive icon → View sync problems
│   ├─ OneDrive will list files it can't sync
│   │   └─ Common causes:
│   │       ├─ File name too long (> 255 characters)
│   │       │   → Rename to a shorter path
│   │       ├─ Invalid characters in filename: ~ # % & * : < > ? / \ { }
│   │       │   → Remove these characters
│   │       ├─ File locked by another application (Office document open)
│   │       │   → Close the file → retry
│   │       ├─ File type blocked by policy (e.g., .exe, .ps1)
│   │       │   → Move to a non-OneDrive location
│   │       └─ File is more than 250 GB
│   │           → OneDrive has a per-file size limit
│   │
│   └─ Fix the reported conflict → Resume sync
│
├─ STORAGE / QUOTA CHECK
│   ├─ Is the user over their OneDrive quota?
│   │   → OneDrive web → Settings → Account → "Storage used"
│   │   ├─ Over quota → Free up space or request quota increase
│   │   └─ Under quota → Proceed
│   └─ Check if the user has enough local disk space
│       → OneDrive needs free space to download files
│       → [see 06-05](06-05_performance_low-disk-space.md)
│
├─ RESET ONEDRIVE
│   ├─ Close OneDrive (right-click → Quit)
│   ├─ Run: `%localappdata%\Microsoft\OneDrive\OneDrive.exe /reset`
│   ├─ Wait 30 seconds → OneDrive should auto-restart
│   └─ If it doesn't → Launch OneDrive manually
│
├─ UNLINK AND RELINK
│   └─ OneDrive Settings → Account → Unlink this PC
│       → Re-sign-in → Select folder locations
│
└─ SYNC CLIENT VERSION CHECK
    └─ Is OneDrive up to date?
        → OneDrive Settings → About → version
        → Check that the user is not on the "OneDrive for Business" old client
        → (The old groove.exe client is deprecated; should be modern OneDrive)
```

**RESULT** → OneDrive syncing normally. Files showing correct status icons.
