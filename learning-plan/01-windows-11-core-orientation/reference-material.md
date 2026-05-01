# Reference Material: Windows 11 Core Orientation

## Quick Reference: Essential GUI Tools

| Tool | How to Open | Purpose |
|---|---|---|
| Settings | `Win + I` | Modern settings interface |
| Control Panel | `control` (Run dialog) | Legacy settings |
| Task Manager | `Ctrl + Shift + Esc` | Process management, performance |
| Event Viewer | `eventvwr.msc` | System and application logs |
| Services | `services.msc` | Manage background services |
| Registry Editor | `regedit` | Windows registry (use with caution) |
| Local Users and Groups | `lusrmgr.msc` | Manage local accounts/groups |
| Computer Management | `compmgmt.msc` | Aggregate admin console |
| Group Policy Editor | `gpedit.msc` | Local policy management (Pro+ only) |
| Disk Management | `diskmgmt.msc` | Partition and volume management |
| Device Manager | `devmgmt.msc` | Hardware and drivers |
| System Configuration | `msconfig` | Boot options, services, startup |
| System Information | `msinfo32` | Detailed system hardware/software info |
| DirectX Diagnostic Tool | `dxdiag` | Display and audio driver info |
| Resource Monitor | `resmon` (from Task Manager > Performance) | Detailed resource usage |
| Performance Monitor | `perfmon` | Performance counters and logging |

## File System Quick Reference

### Known Folders and Paths

| Folder | Path | Environment Variable |
|---|---|---|
| User Profile | `C:\Users\<username>` | `%USERPROFILE%` |
| Desktop | `C:\Users\<username>\Desktop` | `%USERPROFILE%\Desktop` |
| Documents | `C:\Users\<username>\Documents` | `%USERPROFILE%\Documents` |
| Downloads | `C:\Users\<username>\Downloads` | `%USERPROFILE%\Downloads` |
| AppData Roaming | `C:\Users\<username>\AppData\Roaming` | `%APPDATA%` |
| AppData Local | `C:\Users\<username>\AppData\Local` | `%LOCALAPPDATA%` |
| Program Files (64-bit) | `C:\Program Files` | `%PROGRAMFILES%` |
| Program Files (32-bit) | `C:\Program Files (x86)` | `%PROGRAMFILES(x86)%` |
| ProgramData | `C:\ProgramData` | `%PROGRAMDATA%` |
| Windows | `C:\Windows` | `%WINDIR%`, `%SYSTEMROOT%` |
| Temp (User) | `C:\Users\<username>\AppData\Local\Temp` | `%TEMP%` |
| Temp (System) | `C:\Windows\Temp` | N/A |

### File Attributes (vs. Linux permissions)

| Windows Attribute | Effect | Linux Rough Equivalent |
|---|---|---|
| Hidden | Not shown in Explorer by default | Dot-prefix (.) |
| Read-only | Cannot be modified (not enforced by all apps) | `chmod -w` |
| System | OS file, extra protection | N/A |
| Archive | File has been modified since backup | N/A |
| Encrypted (EFS) | Encrypted with user's certificate | LUKS/ecryptfs |
| Compressed | NTFS transparent compression | gzip/btrfs compression |

## Event Viewer: Critical Event IDs

### System Log
| Event ID | Source | Meaning |
|---|---|---|
| 1074 | User32 | System shutdown/restart (shows who initiated and why) |
| 41 | Kernel-Power | Unexpected shutdown (power loss, crash) |
| 1001 | BugCheck | BSOD occurred (includes bugcheck code) |
| 7036 | Service Control Manager | Service state changed |
| 7000 | Service Control Manager | Service failed to start |
| 7023 | Service Control Manager | Service terminated with error |
| 10010 | DCOM | DCOM server did not register |
| 37 | Kernel-Processor-Power | CPU throttling due to thermal/power |

### Application Log
| Event ID | Source | Meaning |
|---|---|---|
| 1000 | Application Error | Application crash |
| 1001 | Application Crash | Application crash details |
| 1002 | Application Hang | Application not responding |
| 11707 / 11724 | MsiInstaller | MSI install success/failure |
| 10 | WinMgmt | WMI event |

### Security Log (if auditing enabled)
| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Logoff |
| 4720 | User account created |
| 4722 | User account enabled |
| 4725 | User account disabled |
| 4740 | Account lockout |

## Services Quick Reference

### Common Startup Types and What They Mean

| Startup Type | When It Starts | Equivalent Concept |
|---|---|---|
| Automatic | At boot, before user login | `systemctl enable --now` |
| Automatic (Delayed) | ~2 min after boot | `systemd.timer` with delay |
| Manual | When requested by app/service | Not enabled, but available |
| Disabled | Never | `systemctl mask` |
| Trigger Start | On specific event (USB insert, network change) | `systemd.path` or `.device` |

### Service Recovery Options
Services can be configured to take actions on failure:
1. **First failure**: Restart service, run program, restart computer, or take no action
2. **Second failure**: Same options
3. **Subsequent failures**: Same options
4. **Reset fail count after**: Time period before the failure counter resets

Configure: `services.msc` > right-click service > Recovery tab

## UAC: What Triggers a Prompt

| Action | UAC Prompt? |
|---|---|
| Installing software | Yes |
| Changing system settings | Yes |
| Modifying files in Program Files or Windows directories | Yes |
| Running a .exe as Administrator | Yes |
| Changing UAC settings itself | Yes |
| Installing a driver | Yes |
| Adding/removing user accounts | Yes |
| Changing file associations (system-wide) | Yes |

## Environment Variables: Useful Commands

### CMD
```cmd
:: Show all variables
set

:: Show specific variable
echo %PATH%

:: Set for current session only
set MYVAR=value

:: Set persistently (user scope)
setx MYVAR "value"

:: Set persistently (system scope, requires admin)
setx MYVAR "value" /M
```

### PowerShell
```powershell
# Show all variables
Get-ChildItem Env:

# Show specific variable
$env:PATH

# Set for current session
$env:MYVAR = "value"

# Set persistently (user scope)
[Environment]::SetEnvironmentVariable("MYVAR", "value", "User")

# Set persistently (system scope)
[Environment]::SetEnvironmentVariable("MYVAR", "value", "Machine")
```

## Keyboard Shortcuts (Support-Relevant)

| Shortcut | Action |
|---|---|
| `Win + I` | Open Settings |
| `Win + R` | Open Run dialog |
| `Win + E` | Open File Explorer |
| `Win + X` | Open Quick Link menu (power user menu) |
| `Win + L` | Lock computer |
| `Win + D` | Show desktop (minimize all) |
| `Win + M` | Minimize all windows |
| `Ctrl + Shift + Esc` | Open Task Manager directly |
| `Ctrl + Alt + Del` | Open security screen (lock, switch user, sign out, Task Manager) |
| `Win + Ctrl + Shift + B` | Restart graphics driver (useful for display issues) |
| `Win + V` | Open clipboard history |
| `Win + .` (period) | Open emoji/symbol picker |
| `Shift + right-click` | Show full context menu (legacy options) |

## Registry: Key Locations for Support

| Hive | Path | Purpose |
|---|---|---|
| HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run | System-wide startup programs |
| HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run | Per-user startup programs |
| HKLM\SYSTEM\CurrentControlSet\Services | Service configuration |
| HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall | Installed programs (64-bit) |
| HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall | Installed programs (32-bit) |
| HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders | User folder paths |
| HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion | OS version and build info |
