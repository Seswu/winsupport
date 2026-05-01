# Reference Material: Diagnostic Tools & Reference

## Complete Log Location Reference

### System Logs
| Component | Location |
|---|---|
| Windows Update | `Get-WindowsUpdateLog` (PowerShell) |
| DISM | `C:\Windows\Logs\DISM\dism.log` |
| CBS | `C:\Windows\Logs\CBS\CBS.log` |
| Setup/Upgrade | `C:\Windows\Panther\setupact.log` |
| Setup/Upgrade (older) | `C:\Windows\Panther\setuperr.log` |
| Driver installation | `C:\Windows\INF\setupapi.dev.log` |
| BitLocker | `C:\Windows\Logs\BitLocker\BitLocker.log` |
| SFC | `C:\Windows\Logs\CBS\CBS.log` (search for `[SR]`) |
| Chkdsk | Event Viewer > System > Event ID 1001, or `C:\Windows\Logs\ChkDsk\` |
| Fast Startup | `C:\Windows\Prefast\` |
| Provisioning | `C:\Windows\Provisioning\Logs\` |
| TPM | Event Viewer > Applications and Services Logs > Microsoft > Windows > TPM |

### Application Logs
| Application | Location |
|---|---|
| OneDrive sync | `%localappdata%\Microsoft\OneDrive\logs\` |
| OneDrive settings | `%localappdata%\Microsoft\OneDrive\settings\Personal\` |
| Teams (Classic) | `%appdata%\Microsoft\Teams\logs.txt` |
| Teams (Classic) cache | `%appdata%\Microsoft\Teams\` |
| Teams (New) | `%localappdata%\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\logs\` |
| Office diagnostic | `%localappdata%\Microsoft\Office\Logging\` |
| Outlook crash | `%localappdata%\Microsoft\Office\16.0\CrashReports\` |
| Word template | `%appdata%\Microsoft\Templates\Normal.dotm` |
| Edge crash | `edge://crashes` in browser |
| Chrome crash | `chrome://crashes` in browser |

### Enterprise/MDM Logs
| Component | Location |
|---|---|
| Intune Mgmt Extension | `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log` |
| Intune Mgmt Extension (details) | `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneExtensionLogger[timestamp].log` |
| Entra join diagnostics | `dsregcmd /status` (command output) |
| GPO processing | Event Viewer > Applications and Services Logs > Microsoft > Windows > GroupPolicy > Operational |
| Roaming profiles | Event Viewer > Application > User Profile Service events |
| MDM diagnostics | Event Viewer > Applications and Services Logs > Microsoft > Windows > DeviceManagement-* |

### Security Logs
| Component | Location |
|---|---|
| Windows Defender/AV | Event Viewer > Applications and Services Logs > Microsoft > Windows > Windows Defender > Operational |
| Windows Firewall | Event Viewer > Applications and Services Logs > Microsoft > Windows > Windows Firewall With Advanced Security > Firewall |
| Audit logon events | Event Viewer > Windows Logs > Security (Event IDs 4624, 4625) |
| BitLocker audit | Event Viewer > Applications and Services Logs > Microsoft > Windows > BitLocker-API > Management |

## Sysinternals Tools Quick Reference

| Tool | Purpose | Download |
|---|---|---|
| **Process Explorer** | Advanced Task Manager | /downloads/process-explorer |
| **Process Monitor** | Real-time file/registry monitoring | /downloads/procmon |
| **Autoruns** | Comprehensive startup manager | /downloads/autoruns |
| **TCPView** | Network connections viewer | /downloads/tcpview |
| **PsExec** | Remote command execution | /downloads/psexec |
| **PsInfo** | System information | /downloads/psinfo |
| **PsList** | Process listing | /downloads/pslist |
| **PsKill** | Process termination | /downloads/pskill |
| **Sigcheck** | File signature verification | /downloads/sigcheck |
| **Handle** | Find which process has a file open | /downloads/handle |
| **Strings** | Extract strings from binaries | /downloads/strings |
| **BlueScreenView** | Analyze BSOD dumps (NirSoft) | https://nirsoft.net/utils/blue_screen_view.html |
| **WhoCrashed** | BSOD analysis (Resplendence) | https://www.resplendence.com/whocrashed |

## Diagnostic Workflow Quick Reference

### General Troubleshooting Flow
1. **Reproduce** the issue (or get detailed description)
2. **Check service health** — is it a known outage?
3. **Check Event Viewer** — any errors at the time of the issue?
4. **Check the specific application logs** — app-specific errors?
5. **Check recent changes** — updates, policy changes, new software?
6. **Search the error** — error code + product name
7. **Apply fix** — documented procedure or known solution
8. **Verify** — confirm the issue is resolved
9. **Document** — write up in ticket

### First 5 Minutes of Any Ticket
1. Ask: What is the exact error message? (get a screenshot if possible)
2. Ask: When did it start? Was it working before?
3. Ask: Did anything change? (update, new software, password change)
4. Check: Can you reproduce it?
5. Check: Is it affecting other users? (service health)

## CMTrace Log Viewer
- Built into Configuration Manager, available standalone
- Opens log files with color-coding:
  - Red = errors
  - Yellow = warnings
  - Blue = info
- Auto-refreshes as the log is being written
- Essential for reading Intune and SCCM logs

## PowerShell Diagnostic One-Liners

```powershell
# Last 10 application errors
Get-EventLog -LogName Application -EntryType Error -Newest 10

# Last 10 system errors
Get-EventLog -LogName System -EntryType Error -Newest 10

# Services that failed to start
Get-Service | Where-Object { $_.Status -eq 'Stopped' -and $_.StartType -eq 'Automatic' }

# Disk space
Get-PSDrive -PSProvider FileSystem | Format-Table Name, Used, Free

# Installed software
Get-CimInstance Win32_Product | Select-Object Name, Version, Vendor

# Network adapters
Get-NetAdapter | Format-Table Name, Status, LinkSpeed

# Windows Update history
Get-WindowsUpdateLog

# User profile size
Get-ChildItem C:\Users -Directory | ForEach-Object { $_.Name; (Get-ChildItem $_.FullName -Recurse -ErrorAction SilentlyContinue | Measure-Object -Property Length -Sum).Sum }

# Top 10 processes by memory
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 10 Name, @{N='MemoryMB';E={[math]::Round($_.WorkingSet64/1MB,2)}}

# Event log size
Get-WinEvent -ListLog * | Where-Object { $_.RecordCount -gt 0 } | Sort-Object FileSize -Descending | Select-Object LogName, RecordCount, @{N='SizeMB';E={[math]::Round($_.FileSize/1MB,2)}}
```
