# Reference Material: Common Windows 11 Issues

## Quick Troubleshooting Decision Trees

### Login Failure
```
User can't log in
├── "Incorrect password" → Verify password, check Caps Lock, reset password
├── "Account locked out" → Unlock in AD/Entra ID, find source of lockout
├── "Trust relationship failed" → `Test-ComputerSecureChannel -Repair` or rejoin domain
├── Windows Hello not working → Reset Windows Hello in Settings
├── Profile won't load → Check for temp profile, verify profile path
└── MFA issue → Check phone/email, verify Authenticator app, check Entra sign-in logs
```

### Slow PC
```
PC is slow
├── Check Task Manager > Performance
│   ├── CPU high → Sort processes by CPU, identify culprit
│   ├── Memory high → Check for leaks, close unnecessary apps
│   ├── Disk 100% → Check Resource Monitor > Disk, consider HDD vs SSD
│   └── Network high → Check for background downloads/updates
├── Check startup apps → Disable unnecessary items
├── Check for pending updates → Install or troubleshoot
└── Check disk space → Free up if >90% full
```

### No Internet
```
No internet
├── Check if other devices work → If no, it's network infrastructure
├── `ping 8.8.8.8` → If no, connectivity issue
│   ├── Check cable/Wi-Fi connection
│   ├── `ipconfig /all` → Check for valid IP (not 169.254.x.x = APIPA)
│   ├── `ipconfig /release && ipconfig /renew`
│   └── `netsh winsock reset` → reboot
├── `ping google.com` → If no but 8.8.8.8 works, DNS issue
│   ├── `ipconfig /flushdns`
│   ├── Check DNS servers in adapter settings
│   └── Check proxy settings
└── Can browse some sites but not others → DNS or proxy filtering
```

## Essential Troubleshooting Commands (Cheat Sheet)

### System Health
| Command | Purpose |
|---|---|
| `sfc /scannow` | Scan and repair protected system files |
| `DISM /Online /Cleanup-Image /RestoreHealth` | Repair Windows image (deeper than SFC) |
| `DISM /Online /Cleanup-Image /CheckHealth` | Check if image is repairable (fast) |
| `chkdsk C: /f` | Check and fix disk errors (requires reboot) |
| `chkdsk C: /r` | Check disk, fix errors, recover bad sectors (slow) |

### Network
| Command | Purpose |
|---|---|
| `ipconfig /all` | Full network configuration |
| `ipconfig /flushdns` | Clear DNS resolver cache |
| `ipconfig /release && ipconfig /renew` | Get new DHCP lease |
| `netsh winsock reset` | Reset Winsock catalog |
| `netsh int ip reset` | Reset TCP/IP stack |
| `netsh interface show interface` | List network interfaces |
| `Test-NetConnection <host>` | Test connectivity (PowerShell) |
| `Test-NetConnection <host> -Port <port>` | Test specific port |
| `Resolve-DnsName <host>` | DNS lookup (PowerShell) |

### Updates
| Command | Purpose |
|---|---|
| `Get-HotFix` | List installed updates (PowerShell) |
| `wmic qfe list` | List installed updates (CMD) |
| `Get-WindowsUpdateLog` | Convert update logs to readable format (PowerShell) |
| `wuauclt /detectnow` | Force Windows Update check (legacy) |
| `USOClient StartScan` | Force Windows Update check (Win10/11) |

### Group Policy
| Command | Purpose |
|---|---|
| `gpupdate /force` | Force Group Policy refresh |
| `gpresult /h report.html` | Generate HTML report of applied policies |
| `gpresult /r` | Text summary of applied policies |
| `rsop.msc` | Resultant Set of Policy (GUI) |

### Domain/Entra
| Command | Purpose |
|---|---|
| `dsregcmd /status` | Device registration status (Entra AD join) |
| `nltest /dsgetdc:<domain>` | Find domain controller |
| `Test-ComputerSecureChannel` | Test domain trust (PowerShell) |
| `Test-ComputerSecureChannel -Repair` | Repair domain trust (PowerShell) |
| `klist` | View Kerberos tickets |

### Services
| Command | Purpose |
|---|---|
| `net start` | List running services |
| `net start <name>` / `net stop <name>` | Start/stop service |
| `sc query` | Detailed service status |
| `Get-Service` | PowerShell service list |
| `Restart-Service <name>` | PowerShell restart |

## Event IDs Quick Reference

### Application Crashes
| Event ID | Log | Source | Meaning |
|---|---|---|---|
| 1000 | Application | Application Error | App crashed (faulting module listed) |
| 1001 | Application | Windows Error Reporting | Crash dump details |
| 1002 | Application | Application Hang | App not responding for N seconds |

### Windows Update
| Event ID | Log | Source | Meaning |
|---|---|---|---|
| 20 | WindowsUpdate | WindowsUpdateClient | Update installation started |
| 21 | WindowsUpdate | WindowsUpdateClient | Update installation completed |
| 24 | WindowsUpdate | WindowsUpdateClient | Update installation failed |
| 31 | WindowsUpdate | WindowsUpdateClient | Update installed successfully (reboot required) |

### Network
| Event ID | Log | Source | Meaning |
|---|---|---|---|
| 10000 | System | Wlansvc | Wi-Fi connected |
| 8003 | System | Wlansvc | Wi-Fi disconnected |
| 4202 | System | Dhcp | IP address lease renewal failed |
| 1014 | System | DNS Client Events | DNS name resolution timed out |

### Print Spooler
| Event ID | Log | Source | Meaning |
|---|---|---|---|
| 307 | System/PrintService | PrintService | Print job spooled |
| 314 | System/PrintService | PrintService | Print job failed |
| 219 | System/PrintService | PrintService | Printer driver issue |

## Print Spooler Reset Procedure

```cmd
:: Stop the spooler
net stop spooler

:: Delete stuck print jobs
del /Q /F /S C:\Windows\System32\spool\PRINTERS\*

:: Start the spooler
net start spooler
```

## OneDrive Reset Command

```cmd
:: Reset OneDrive sync client
onedrive.exe /reset

:: If OneDrive doesn't restart automatically after reset
:: Open Run dialog (Win+R) and paste:
%localappdata%\Microsoft\OneDrive\OneDrive.exe
```

## Safe Mode Access Methods

### Method 1: From Settings
Settings > System > Recovery > Advanced startup > Restart now > Troubleshoot > Advanced options > Startup Settings > Restart > Press 4/5/6

### Method 2: From Login Screen
Hold Shift key > click Power button > Restart > Troubleshoot > Advanced options > Startup Settings > Restart > Press 4/5/6

### Method 3: From Command Prompt (admin)
```cmd
bcdedit /set {current} safeboot minimal
shutdown /r /t 0
:: To exit safe mode:
bcdedit /deletevalue {current} safeboot
```

### Method 4: From System Configuration
`msconfig` > Boot tab > check "Safe boot" > Apply > Restart
**IMPORTANT:** Remember to uncheck it afterward or you'll always boot into Safe Mode.

## BSOD Stop Codes Quick Reference

| Stop Code | Common Cause | First Thing to Try |
|---|---|---|
| `IRQL_NOT_LESS_OR_EQUAL` | Driver accessing wrong memory | Update/rollback recent driver |
| `SYSTEM_THREAD_EXCEPTION_NOT_HANDLED` | Graphics driver | Update GPU driver, boot safe mode |
| `KMODE_EXCEPTION_NOT_HANDLED` | Driver or hardware | Safe mode, check Event Viewer |
| `PAGE_FAULT_IN_NONPAGED_AREA` | Faulty RAM or driver | Run memory diagnostic, update drivers |
| `CRITICAL_PROCESS_DIED` | System file corruption | `sfc /scannow`, `DISM` repair |
| `INACCESSIBLE_BOOT_DEVICE` | Storage driver or disk | Check BIOS storage mode, safe mode |
| `SYSTEM_SERVICE_EXCEPTION` | System file or driver | Update Windows, check for bad update |
| `DRIVER_POWER_STATE_FAILURE` | Driver power management | Update driver, disable fast startup |
| `DPC_WATCHDOG_VIOLATION` | SSD firmware or driver | Update SSD firmware, storage driver |
| `WDF_VIOLATION` | Windows Driver Framework | Update driver for recently added hardware |
