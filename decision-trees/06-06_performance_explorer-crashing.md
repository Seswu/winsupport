# Windows Explorer Crashing / Freezing

File Explorer (formerly Windows Explorer) keeps restarting, freezing, or becoming unresponsive.

```
User reports: "The file window keeps crashing" or "Explorer keeps restarting"

├─ IMMEDIATE FIX
│   ├─ Restart Explorer:
│   │   → Task Manager → Processes → "Windows Explorer"
│   │   → Right-click → Restart
│   │   → (Explorer will restart automatically, including taskbar and desktop)
│   │
│   └─ If Task Manager also freezes:
│       → Ctrl+Alt+Del → Sign out → Sign back in
│       → Or: Hold Shift + Restart (recovery options) → Troubleshoot → Advanced → Restart
│
├─ COMMON CAUSES
│   ├─ **Corrupt thumbnail cache** (crashes when opening folders with images/videos)
│   │   → Run: `cleanmgr` → Check "Thumbnails" → Clean
│   │   → Or delete: `%localappdata%\Microsoft\Windows\Explorer\thumbcache_*.db`
│   │
│   ├─ **Corrupt icon cache** (crashes when browsing folders)
│   │   → Delete icon cache:
│   │   → `%localappdata%\Microsoft\Windows\Explorer\iconcache*`
│   │   → Restart Explorer
│   │
│   ├─ **Shell extension conflicts** (third-party software adds Explorer extensions)
│   │   → Common culprits: Dropbox, Google Drive, 7-Zip, TortoiseGit/SVN
│   │   → Use ShellExView (Nirsoft) to disable third-party shell extensions
│   │   → Or: Uninstall the suspected app → test → reinstall if needed
│   │
│   ├─ **Quick Access / Recent files corruption**
│   │   → File Explorer → Options → General → Privacy → Clear File Explorer history
│   │   → Uncheck "Show recently used files" and "Show frequently used folders"
│   │
│   ├─ **Network drive issues** (Explorer hangs waiting for offline shares)
│   │   → `net use * /delete` → Remove stale network drive mappings
│   │   → [see 03-03](03-03_networking_network-drive-mapping.md)
│   │
│   ├─ **Large folder with thousands of files**
│   │   → Explorer has to generate thumbnails and file info
│   │   → Change view to "Details" (not Thumbnails or Icons)
│   │   → Or: Open the folder in Command Prompt: `start .`
│   │
│   └─ **Video/audio file preview** (Explorer crashing on media files)
│   │   → Disable preview pane: View → Preview pane → OFF
│   │   → Disable details pane: View → Details pane → OFF
│   └─
│
├─ ADVANCED FIXES
│   ├─ Run `sfc /scannow` (system file corruption affects Explorer)
│   ├─ Create a new user profile (current profile may be corrupt)
│   ├─ Run `DISM /Online /Cleanup-Image /RestoreHealth`
│   └─ Check for malware (browser hijackers sometimes hook into Explorer)
│
├─ CHECK EVENT VIEWER
│   └─ Application log → Event ID 1000 → Faulting module name:
│       ├─ `shell32.dll` → Corrupt system file → SFC
│       ├─ `thumbcache.dll` → Corrupt thumbnail cache → Clean thumbnails
│       ├─ `fileexts.dll` → Shell extension issue → Disable extensions
│       └─ `nview.dll` / `nvd3dum.dll` → NVIDIA driver conflict → Update GPU driver
│
└─ STILL CRASHING?
    └─ Known issue with Windows build? → Check for updates
        → Settings → Windows Update → Install latest quality update
        → If recently updated → Roll back update → [see 04-03]
        → Escalate to Tier 2
```

**RESULT** → File Explorer stable and responsive.
