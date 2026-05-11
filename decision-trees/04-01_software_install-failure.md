# Can't Install Software

User cannot install, update, or launch required applications — installer errors, access denied, or the installation wizard crashes.

```
User reports: "I'm trying to install [app] but it won't work"

├─ WHAT TYPE OF INSTALLATION?
│   ├─ MSI installer (.msi)
│   ├─ EXE installer (.exe)
│   ├─ Microsoft Store app
│   ├─ Company portal / Intune deployment
│   └─ Web-based installer (downloader)
│
├─ COMMON FIXES
│   ├─ Run installer as Administrator:
│   │   → Right-click installer → Run as administrator
│   │   → (Many installers require admin rights)
│   │
│   ├─ Check if another version is already installed:
│   │   → Settings → Apps → Installed apps → Search for the app
│   │   → Uninstall the existing version first
│   │
│   ├─ Check disk space:
│   │   → Is there enough free space? → Need at least 2x the installer size
│   │   → [see 06-05](06-05_performance_low-disk-space.md)
│   │
│   ├─ Disable antivirus temporarily:
│   │   → Antivirus may block installer or quarantine files mid-install
│   │   → Add installer folder to exclusion list
│   │
│   └─ Check if the installer is blocked:
│   │   → Right-click the file → Properties
│   │   → If "This file came from another computer" → Check "Unblock"
│   └─
│
├─ SPECIFIC ERROR PATTERNS
│   ├─ **Error 1603** / **Error 1606**
│   │   → Windows Installer issue
│   │   → Run: `msiexec /unregister` → `msiexec /regserver`
│   │   → Restart the Windows Installer service (msiserver)
│   │   → Reboot → try installation again
│   │
│   ├─ **Error 2502** / **2503**
│   │   → Permission issue with the installer
│   │   → Run installer from administrator command prompt
│   │   → Or: Take ownership of %TEMP% folder
│   │
│   ├─ **"Another installation is in progress"**
│   │   → An MSI install is stuck in progress
│   │   → Check: `msiexec /?` for active processes
│   │   → Kill all msiexec processes in Task Manager
│   │   → Reboot → retry
│   │
│   ├─ **"Windows Installer Service could not be accessed"**
│   │   → `services.msc` → Windows Installer → Running? Manual?
│   │   → Set to Manual → Start the service → retry
│   │
│   └─ **"The system cannot find the file specified"**
│       → Installer package is corrupt → Re-download the installer
│
├─ INTRUNE / COMPANY PORTAL
│   ├─ Is the app available in Company Portal?
│   │   → Open Company Portal → Check "Available" or "Required" apps
│   │   → Click Install → wait for status
│   │
│   ├─ Installation stuck at "Downloading" or "Installing"?
│   │   → Sync device: Settings → Accounts → Access work or school
│   │   → Click the work account → Info → Sync
│   │   → Reboot → try again from Company Portal
│   │
│   └─ App installation failed with error code:
│       → Check Intune Management Extension logs:
│       → `%ProgramData%\Microsoft\IntuneManagementExtension\Logs`
│
├─ SYSTEM PRE-REQUISITES
│   ├─ Does the app require a specific Windows feature?
│   │   → Turn Windows features on/off → Enable required feature
│   ├─ Does it require a runtime? (VC++ Redist, .NET, Java)
│   │   → [see 04-05](04-05_software_legacy-runtime-required.md)
│   └─ Does it require a specific Windows version or build?
│       → Check app system requirements → Update Windows if needed
│
└─ STILL FAILING?
    └─ Collect: exact error message, installer filename, OS version/build
        → Search for the error code with the application name
        → Escalate to Tier 2 / application owner
```

**RESULT** → Installed successfully, or escalation with full details.
