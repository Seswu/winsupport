# Common Windows Error Codes

Quick-lookup reference for errors you'll see in dialogs, Event Viewer, and update failures. Each entry gives the code, plain-English meaning, typical cause, and the fix to try first.

---

## Quick Numerical Lookup

| Code | Section | Short description |
|---|---|---|
| `0x0000000A` | BSOD | IRQL_NOT_LESS_OR_EQUAL |
| `0x0000001A` | BSOD | MEMORY_MANAGEMENT |
| `0x0000003B` | BSOD | SYSTEM_SERVICE_EXCEPTION |
| `0x00000050` | BSOD | PAGE_FAULT_IN_NONPAGED_AREA |
| `0x000000EF` | BSOD | CRITICAL_PROCESS_DIED |
| `0xC0000005` | Application | Access violation (crash) |
| `0xC000021A` | BSOD | System process terminated |
| `0x80004005` | Login/General | Unspecified error |
| `0x8000FFFF` | Application | Catastrophic failure |
| `0x80070002` | Windows Update | File not found |
| `0x80070005` | Install/Permissions | Access denied |
| `0x8007000E` | Install/Memory | Out of memory |
| `0x80070020` | Windows Update | Sharing violation |
| `0x80070035` | Network | Bad network path |
| `0x8007007E` | Application | DLL not found |
| `0x80070422` | Windows Update | Service disabled |
| `0x8007052E` | Login | Logon failure |
| `0x80070643` | Install | Installation failure |
| `0x80073712` | Windows Update | Component store corrupt |
| `0x80240017` | Windows Update | Update not applicable |
| `0x8024401c` | Windows Update | Timeout connecting to update server |

---

## Windows Update & Servicing

Use when: updates fail to install, get stuck at a percentage, roll back, or show cryptic hex errors.

### `0x80070002` — ERROR_FILE_NOT_FOUND

**Meaning:** The update component store is missing a file — often because the `SoftwareDistribution` cache is corrupt.

**Fix:**
1. `net stop wuauserv && net stop bits`
2. Delete/rename `C:\Windows\SoftwareDistribution`
3. `net start wuauserv && net start bits`
4. Run Windows Update Troubleshooter (Settings > System > Troubleshoot)

### `0x80070020` — ERROR_SHARING_VIOLATION

**Meaning:** Another process (often antivirus) has locked the update files.

**Fix:**
- Temporarily disable antivirus/antimalware
- Stop Windows Update service, delete `SoftwareDistribution`, restart
- Run `sfc /scannow` if the issue persists

### `0x80070422` — ERROR_SERVICE_DISABLED

**Meaning:** The Windows Update service (`wuauserv`) or BITS service is not running or is disabled.

**Fix:**
1. Open `services.msc`
2. Set **Windows Update** and **Background Intelligent Transfer (BITS)** to **Automatic**
3. Start both services
4. Retry updates

### `0x80240017` — WU_E_NOT_SUPPORTED

**Meaning:** The update is not applicable to this version/edition of Windows.

**Fix:**
- Check the Windows version (`winver`) — is it the correct edition for the update?
- Some updates require a prerequisite or a specific build. Install the latest cumulative update first.
- If manually downloading, confirm you selected the right architecture (x64 vs ARM).

### `0x8024401c` — HTTP_STATUS_REQUEST_TIMEOUT

**Meaning:** The machine couldn't reach Windows Update (or WSUS) within the timeout period.

**Fix:**
- Check internet connectivity (`ping` a public site)
- Reset proxy settings: `netsh winhttp reset`
- Check if the firewall/WVPN is blocking the update endpoint
- If using WSUS, verify the machine is in the correct target group

### `0x80073712` — ERROR_SXS_COMPONENT_STORE_CORRUPT

**Meaning:** The CBS (Component Based Servicing) store is corrupted — system files needed for updates are damaged.

**Fix:**
1. `DISM /Online /Cleanup-Image /RestoreHealth`
2. `sfc /scannow`
3. Reboot and retry updates

### `0x800F0922` — CBS_E_INSTALL_FAILURE (common variant)

**Meaning:** DISM itself couldn't install the payload — often a driver or language pack conflict.

**Fix:**
- Check disk space on `C:` (need at least 10 GB free)
- Disconnect non-essential USB devices
- Check `C:\Windows\Logs\CBS\CBS.log` for the specific failing component

---

## Login & Authentication

Use when: a user can't sign in, gets password prompts repeatedly, or receives access-denied errors on domain resources.

### `0x8007052E` — ERROR_LOGON_FAILURE

**Meaning:** Wrong username or password. The most common error in support.

**Fix:**
- Reset the password (self-service or admin portal)
- Check Caps Lock / Num Lock
- Verify the user is typing the correct domain\username format
- Check if the account is locked: look in Entra ID sign-in logs or AD for Event ID 4740

### `0x80070035` — BAD_NETWORK_PATH

**Meaning:** Windows can't find the network path (UNC, mapped drive, or SMB share).

**Fix:**
- `ping <server>` — does the machine resolve the server name?
- `nslookup <server>` — confirm DNS is returning the correct IP
- Check SMB is enabled (Settings > Optional Features > SMB 1.0/CIFS — though this should be off for security)
- Run `net use \\server\share` to re-establish the connection

### `0x80004005` — E_FAIL (unspecified)

**Meaning:** Generic failure — often credentials-related when accessing network resources or databases.

**Fix:**
- Open Credential Manager (Control Panel > Credential Manager) and remove stale credentials
- Re-add the target resource with fresh credentials
- Run the application as Administrator
- Check Event Viewer for a more specific inner error

### `0x80070005` — E_ACCESSDENIED

**Meaning:** The user does not have permission, or UAC is blocking the operation.

**Fix:**
- Run the action as Administrator (right-click > Run as administrator)
- Check NTFS permissions with `icacls <path>`
- Check if the user is in the correct security group
- For COM/DCOM errors: check DCOM permissions (`dcomcnfg`)

### `0x8007139F` — ERROR_LOGON_SESSION_COEXISTENCE (RDP)

**Meaning:** Another user session is already logged in with conflicting credentials on a remote machine.

**Fix:**
- Disconnect the existing session (or reboot the remote machine)
- Use `mstsc /admin` to connect to the console session
- Wait for the session timeout to expire

---

## Application Crashes & System Errors

Use when: a program crashes with an error dialog, or you see these codes in Event Viewer Application log.

### `0xC0000005` — STATUS_ACCESS_VIOLATION

**Meaning:** The program tried to access a memory address it didn't have permission to read/write. Often causes the "faulting module" crash pattern.

**Fix:**
- Update the application to the latest version
- Update graphics/audio drivers (common trigger for games and CAD software)
- Run Windows Memory Diagnostic (`mdsched.exe`) to rule out faulty RAM
- Disable Data Execution Prevention (DEP) for the specific app — not recommended as first step
- Reinstall the application

### `0x8007007E` — ERROR_MOD_NOT_FOUND

**Meaning:** A required DLL is missing. The app can't load one of its dependencies.

**Fix:**
- Reinstall the application (restores missing DLLs)
- Install the latest Microsoft Visual C++ Redistributable (often the missing dependency)
- Run `sfc /scannow` — a system DLL may be corrupt
- Look in Event Viewer for the exact DLL name; search for it on the system

### `0x8000FFFF` — E_UNEXPECTED

**Meaning:** "Catastrophic failure" — one of Windows' most generic error messages. Usually means a COM component failed unexpectedly.

**Fix:**
- Check Event Viewer Application log for a preceding error with more detail
- Re-register the failing DLL: `regsvr32 <dllname>` (if you can identify it)
- Repair or reinstall the application
- Check for corrupt user profile if the error is tied to a specific user

### `0x80131500` — COR_E_EXCEPTION (.NET)

**Meaning:** A .NET application threw an unhandled exception.

**Fix:**
- Look in Event Viewer for the faulting .NET runtime version and stack trace
- Repair the .NET runtime (Settings > Apps > Microsoft .NET > Modify)
- Update .NET to the latest version
- Check if a recent Windows Update broke .NET (common with monthly patches)

### `0x8007023E` — ERROR_CRC

**Meaning:** Cyclic Redundancy Check error — data read from disk doesn't match its checksum.

**Fix:**
- This is often a **hardware issue** — the disk is failing
- Run `chkdsk C: /f` to identify bad sectors
- Back up data immediately if the drive is still accessible
- Replace the drive

---

## Installation & Permission Errors

Use when: installing or uninstalling software fails, with a generic error code.

### `0x80070643` — ERROR_INSTALL_FAILURE

**Meaning:** The Windows Installer (MSI) encountered a fatal error during installation.

**Fix:**
- Check disk space on the target drive
- Run `msiexec /unregister` then `msiexec /regserver` to re-register Windows Installer
- Re-download the installer (the original may be corrupt)
- Check `%TEMP%` for installer logs with more detail
- Disable antivirus temporarily

### `0x8007000E` — E_OUTOFMEMORY

**Meaning:** The system ran out of memory (RAM or virtual memory/page file).

**Fix:**
- Close unnecessary applications
- Check available RAM in Task Manager
- Increase virtual memory (page file): System Properties > Advanced > Performance > Virtual Memory
- Check for memory leaks — `tasklist` sorted by memory use

### `0x80070057` — E_INVALIDARG

**Meaning:** An invalid parameter was passed — often seen when copying files with long paths, or in installer arguments.

**Fix:**
- If copying files: check path length (< 260 characters for traditional paths)
- If installing: run the installer from a shorter path (e.g., `C:\temp\` instead of Desktop)
- Enable long path support via Group Policy if this is a recurring issue

### `0x800F081F` — CBS_E_NOT_APPLICABLE / CBS_E_INSTALL_FAILURE

**Meaning:** The .NET Framework or language pack installation failed — usually because a prerequisite is missing.

**Fix:**
- Run Windows Update first (install all pending updates)
- Use the offline installer with `/q` and `/norestart` flags
- Confirm the installer targets the correct OS version

---

## BSOD Stop Codes

Use when: the machine blue-screened. Note the stop code from the screen or find it in Event Viewer (System log, Event ID 1001).

### `0x00000050` — PAGE_FAULT_IN_NONPAGED_AREA

**Meaning:** The system tried to access memory that doesn't exist or isn't available. Usually a faulty driver, failing hardware, or corrupt system file.

**Fix:**
- Run `chkdsk /f` to check for disk errors
- Run `sfc /scannow` to check system files
- Test RAM with Windows Memory Diagnostic (`mdsched.exe`)
- Boot into Safe Mode and uninstall recent drivers (especially network, video, or storage drivers)
- Check for recent hardware changes (new RAM, new SSD)

### `0x0000000A` — IRQL_NOT_LESS_OR_EQUAL

**Meaning:** A driver or system service accessed a memory address at an incorrect interrupt request level (IRQL). Almost always a driver issue.

**Fix:**
- Boot into Safe Mode
- Roll back or uninstall recently installed drivers (focus on: network, video, chipset, storage)
- Disable recently added devices
- If the stop code mentions a specific file (e.g., `nvlddmkm.sys`), that's your culprit — update/reinstall that component

### `0x000000EF` — CRITICAL_PROCESS_DIED

**Meaning:** A critical system process (like `csrss.exe`, `wininit.exe`, `smss.exe`) terminated unexpectedly.

**Fix:**
- Boot into Safe Mode
- Run `sfc /scannow` and `DISM /Online /Cleanup-Image /RestoreHealth`
- Run `chkdsk C: /r`
- If neither works, try Startup Repair from the recovery environment
- Last resort: Reset this PC (keep files)

### `0x0000001A` — MEMORY_MANAGEMENT

**Meaning:** A severe memory management error — usually faulty physical RAM.

**Fix:**
- Run Windows Memory Diagnostic (`mdsched.exe`) — let it complete the full pass
- Test with one RAM stick at a time if you have multiple
- Check for overheating (RAM errors increase at high temperatures)
- Update BIOS/chipset drivers (sometimes fixes memory compatibility issues)

### `0x0000003B` — SYSTEM_SERVICE_EXCEPTION

**Meaning:** An exception occurred while executing a system service routine. Very commonly caused by a misbehaving driver.

**Fix:**
- Update all drivers — especially GPU (the most common trigger)
- Run `sfc /scannow`
- Uninstall recent software or updates
- Run `chkdsk /f` to rule out disk corruption
- Check for a specific driver name in the blue screen text — that tells you the culprit

### `0xC000021A` — STATUS_SYSTEM_PROCESS_TERMINATED

**Meaning:** A critical user-mode process (like `Winlogon` or `Client Server Runtime`) crashed. Windows halts to protect system integrity.

**Fix:**
- Boot into Safe Mode
- Roll back recently installed Windows Updates (Settings > Windows Update > Update history > Uninstall updates)
- Restore from a system restore point
- `sfc /scannow` and `DISM`
- If caused by a third-party driver, boot to Safe Mode and uninstall

### `0x00000133` — DPC_WATCHDOG_VIOLATION

**Meaning:** A driver didn't complete its Deferred Procedure Call within the watchdog timeout — the system detected an unacceptably long response. Common with SSD/NVMe drivers and older storage drivers.

**Fix:**
- Update storage drivers (NVMe, SATA, AHCI)
- Update BIOS/firmware
- Disable USB selective suspend in power settings
- Check for incompatible virtualization software

---

## When the Code Isn't Listed Here

1. **Search the exact code** in the Microsoft Win32 Error Codes documentation
2. **Search the code + a brief description** of what you were doing — someone has hit this before
3. **Look in Event Viewer** for a more specific error or inner exception
4. **Run `net helpmsg <decimal>`** — convert the hex to decimal and run `net helpmsg <number>` for Windows' own description
