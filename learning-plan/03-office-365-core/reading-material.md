# Reading Material: Office 365 Core

## 1. Outlook

### Architecture
Outlook connects to Exchange (on-premises) or Exchange Online (M365 cloud). The connection flow:
1. Outlook needs to find the Exchange server
2. It queries the AutoDiscover service (DNS-based lookup)
3. AutoDiscover returns the correct server URL and configuration
4. Outlook connects and downloads mailbox data

### AutoDiscover Process
This is the most important concept for Outlook connectivity. When Outlook starts:
1. Checks local registry for cached AutoDiscover info
2. Tries SCP (Service Connection Point) lookup in Active Directory (domain-joined)
3. Tries HTTPS to `https://<domain>/autodiscover/autodiscover.xml`
4. Tries HTTPS to `https://autodiscover.<domain>/autodiscover/autodiscover.xml`
5. Uses the response to configure server URLs for Exchange, OAB, EWS, etc.

**Common AutoDiscover failures:**
- DNS not configured correctly (missing autodiscover CNAME)
- Certificate issues (cert doesn't cover autodiscover domain)
- Authentication failures (MFA, conditional access blocking)
- Proxy blocking the autodiscover URLs

### OST vs. PST

| Feature | OST (Offline Storage Table) | PST (Personal Storage Table) |
|---|---|---|
| Purpose | Cached copy of Exchange mailbox | Local archive/import file |
| Tied to | Specific mailbox and profile | Portable, can be opened by any profile |
| Location | `%localappdata%\Microsoft\Outlook\` | Anywhere (user-specified) |
| Can be opened directly? | No, tied to profile | Yes, File > Open > Outlook Data File |
| When to rebuild | Corruption, sync issues, mailbox moved | N/A |

**When to rebuild an OST:**
- Outlook stuck on "connecting"
- Missing folders or emails that exist on the server
- Sync errors
- "Cannot expand folder" errors

**How to rebuild:**
1. Close Outlook
2. Delete or rename the `.ost` file in `%localappdata%\Microsoft\Outlook\`
3. Reopen Outlook — it will recreate the OST from the server
4. The re-download may take significant time for large mailboxes

### Cached Exchange Mode vs. Online Mode
- **Cached Mode** (default): Downloads a local copy of mailbox. Faster, works offline, uses disk space.
- **Online Mode**: All data lives on server. Slower, requires constant connection, no local copy.

Cached Mode slider (Outlook > File > Account Settings > Account Settings > double-click account > Mail to keep offline):
- 1 Day, 3 Days, 1 Week, 2 Weeks, 1 Month, 3 Months, 6 Months, 1 Year, All
- Default is usually 1 Year. Users who need older emails must change to "All" or search online.

### Common Outlook Issues

**"Need Password" loop**
- Cause: Credential not saved, MFA prompt blocked, conditional access blocking
- Fix: Check Credential Manager for saved Outlook credentials, remove and re-add. Check Entra sign-in logs for block reason.

**"Cannot start Microsoft Outlook. Cannot open the Outlook window."**
- Cause: Corrupted profile or navigation pane configuration
- Fix: `outlook.exe /resetnavpane` or create new profile

**Search not working**
- Cause: Windows Search index corrupted
- Fix: Rebuild index (Settings > Search > Searching Windows > Advanced Search Indexer Settings > Advanced > Rebuild)

**Send/receive errors**
- Check Outlook connection status: Hold Ctrl > right-click Outlook icon in system tray > Connection Status
- Shows which server connections are active and any failures

### Profile Management
Location: Control Panel > Mail (search "Mail" in Start menu)

- **Show Profiles**: Lists existing profiles
- **Add**: Creates a new profile (useful when a profile is corrupted)
- **Remove**: Deletes a profile (deletes OST too)
- **Always use this profile**: Sets the default profile

## 2. OneDrive

### How OneDrive Works
OneDrive syncs files between the local machine and SharePoint/OneDrive cloud storage. It uses a differential sync algorithm — only changed blocks are uploaded/downloaded.

### Files On-Demand
This feature allows files to exist in Explorer without taking up local disk space:

| Icon | Status | Description |
|---|---|---|
| Blue cloud | Online-only | File exists in cloud, not on disk. Opens when you double-click (downloads on demand) |
| Green checkmark (white circle) | Locally available | File was opened and is now cached locally |
| Solid green checkmark | Always keep on this device | Pinned to stay local even when not used |

### Known Folder Move (KFM)
When enabled by policy, OneDrive takes over the user's Desktop, Documents, and Pictures folders:
- Files are moved to `C:\Users\<username>\OneDrive - <Company>\Desktop` (and Documents, Pictures)
- The user's original folders become junctions pointing to the OneDrive location
- This provides automatic backup and cross-device sync of important folders

**Support implications:**
- Users may be confused when their Desktop files appear in OneDrive
- If KFM is disabled after being enabled, files remain in OneDrive and the original folders are empty
- Check if KFM is active by looking for OneDrive paths in Desktop/Documents properties

### Sync Conflicts
Occur when the same file is edited on multiple devices before sync completes:
- OneDrive creates a copy: `filename — ComputerName (conflicted copy).ext`
- Both versions are preserved
- User must manually merge and delete the duplicate

### File Name and Path Limits
- Maximum path length: 400 characters (OneDrive business)
- Forbidden characters: `" * : < > ? / \ |`
- Forbidden names: `_vti_inf.html`, `client~1`, `desktop.ini`, etc.
- Leading/trailing spaces and periods in file names

### Storage Quota
- Default: 1 TB (varies by license)
- When full: sync stops, user gets notification
- Check usage: OneDrive icon in system tray > Settings (gear) > Settings > Storage

### Reset OneDrive
```cmd
:: Close OneDrive first
:: Reset the sync client
onedrive.exe /reset

:: If it doesn't restart automatically after ~30 seconds:
%localappdata%\Microsoft\OneDrive\OneDrive.exe
```

### Log Location
`%localappdata%\Microsoft\OneDrive\logs\`

### Common OneDrive Issues

**"Processing changes" stuck**
- File is locked by another application
- File name contains invalid characters
- Path exceeds 400 characters
- OneDrive service hung (reset OneDrive)

**Files not appearing on another device**
- Check sync status (red X = error, blue arrows = syncing)
- Check for conflicts
- Check storage quota

**Known Folder Move confusion**
- User's Desktop files moved to OneDrive
- Check if OneDrive folder is the actual location
- Reconfigure KFM if needed (requires admin policy change)

## 3. Microsoft Teams

### Client Types
- **Desktop client** (recommended): Full features, runs as a user process
- **Web client**: Accessed via browser, some features limited
- **New Teams** (2023+): Rebuilt architecture, better performance, separate from classic Teams

### Cache Location
Classic Teams: `%appdata%\Microsoft\Teams\`
New Teams: `%localappdata%\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\`

### Clearing Teams Cache
**Classic Teams:**
1. Quit Teams (right-click system tray icon > Quit)
2. Delete contents of `%appdata%\Microsoft\Teams\`
3. Restart Teams

**New Teams:**
1. Settings > Apps > Installed apps > Microsoft Teams > Advanced options > Reset

### Common Teams Issues

**Audio/video not working**
- Check device settings in Teams (Settings > Devices)
- Check Windows privacy settings (Settings > Privacy & security > Camera/Microphone > allow desktop apps)
- Check if another app is using the device

**Can't join meeting**
- Check network connectivity (Teams requires specific ports/URLs)
- Check if user has a valid license
- Check time synchronization (Kerberos/SSL requires correct time)

**Presence not updating**
- Sign out and sign back in
- Check Teams service status
- Known delay: presence can take several minutes to update

## 4. Office Applications (Word, Excel, PowerPoint)

### Installation Types
- **Click-to-Run** (current standard): Streamed installation, virtualized, updates automatically
- **MSI** (legacy): Traditional Windows Installer package, no longer the default

### Activation Models
- **User-based**: License tied to user account. Works on up to 5 devices simultaneously.
- **Device-based**: License tied to device. Used for shared computers (Shared Computer Activation).
- **Volume License**: KMS or MAK activation for enterprise deployments.

### Office Repair
Settings > Apps > Installed apps > Microsoft 365 Apps > Modify:
- **Quick Repair**: Fast, fixes most issues, no internet required
- **Online Repair**: Thorough, re-downloads files, requires internet

### Safe Mode
Launch Office apps in safe mode to diagnose add-in or configuration issues:
```
winword.exe /safe    # Word
excel.exe /safe      # Excel
powerpnt.exe /safe   # PowerPoint
outlook.exe /safe    # Outlook
```

Safe mode disables:
- Add-ins
- Custom toolbars/ribbons
- Template changes
- Some settings

If the app works in safe mode, the problem is likely an add-in or customization.

### Common Office Issues

**"This product is unlicensed" or "Activation required"**
- Check license assignment in M365 Admin Center
- Run Office activation troubleshooter
- Check if user is signed into Office with correct account (File > Account)
- Check for license conflicts (multiple licenses assigned)

**Office apps slow or crashing**
- Open in safe mode to check for add-in issues
- Disable hardware graphics acceleration
- Repair Office installation
- Check for pending Office updates

**COM Add-ins causing issues**
- File > Options > Add-ins > COM Add-ins > Go
- Uncheck add-ins one by one to identify the culprit
- Common problematic add-ins: Adobe Acrobat PDFMaker, third-party grammar checkers, enterprise DLP add-ins
