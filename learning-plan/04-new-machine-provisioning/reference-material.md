# Reference Material: New Machine Provisioning

## Quick Commands

### Enrollment & Identity
| Command | Purpose |
|---|---|
| `dsregcmd /status` | Check Entra ID and MDM enrollment status |
| `systeminfo \| findstr /B /C:"Domain"` | Check domain membership |
| `whoami /upn` | Show user principal name |
| `klist` | Show Kerberos tickets |
| `nltest /dsgetdc:<domain>` | Find domain controller |

### Policy & Configuration
| Command | Purpose |
|---|---|
| `gpupdate /force` | Force Group Policy refresh |
| `gpresult /h report.html` | Generate HTML report of applied policies |
| `gpresult /r` | Text summary of applied policies |
| `rsop.msc` | GUI view of applied policies |

### BitLocker
| Command | Purpose |
|---|---|
| `manage-bde -status` | Show BitLocker status for all drives |
| `manage-bde -status C:` | Show BitLocker status for C: drive |
| `manage-bde -protectors -get C:` | Show key protectors |

### Time Sync
| Command | Purpose |
|---|---|
| `w32tm /query /status` | Show time sync status |
| `w32tm /resync` | Force time resync |
| `w32tm /query /source` | Show time source |

### Provisioning Logs
| Log Location | Content |
|---|---|
| `C:\Windows\Provisioning\Logs` | Provisioning operations |
| Event Viewer > Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider | Intune/MDM events |
| Event Viewer > Applications and Services Logs > Microsoft > Windows > DeviceManagement-Admin | Intune admin events |
| `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs` | Intune Management Extension logs (app/policy deployment) |
| `C:\Windows\Temp\AutopilotDiagnostics` | Autopilot diagnostics (if enabled) |

## Autopilot Troubleshooting

### Key Autopilot Endpoints (must be reachable)
- `https://login.microsoftonline.com`
- `https://login.live.com`
- `https://enterpriseenrollment.manage.microsoft.com`
- `https://enterpriseregistration.windows.net`
- `https://ztd.dds.microsoft.com`
- `https://autopilot.manage.microsoft.com`

### dsregcmd /status Output Interpretation
Look for these key fields:
```
AzureAdJoined: YES/NO
EnterpriseJoined: YES/NO
DomainJoined: YES/NO
MDM enrollment: YES/NO (should be "Microsoft Intune" or similar)
```

## Intune App Deployment Status

### On Device
- Settings > Accounts > Access work or school > click account > Info
- Lists all apps deployed via Intune and their status

### In Intune Portal
- Devices > All devices > select device > Apps tab
- Shows deployed apps, installation status, last check-in

### Event IDs (DeviceManagement-Enterprise-Diagnostics-Provider)
| Event ID | Meaning |
|---|---|
| 1901 | MDM policy applied successfully |
| 1902 | MDM policy application failed |
| 1905 | App installation started |
| 1906 | App installation completed |
| 1907 | App installation failed |

## Standard Software Checklist Template

For a new machine, verify these categories:
- [ ] OS is up to date
- [ ] Endpoint protection installed and active
- [ ] Office 365 installed and activated
- [ ] Outlook configured
- [ ] OneDrive syncing
- [ ] Teams installed and signed in
- [ ] Browser configured (Edge/Chrome with corporate policies)
- [ ] Printer connections working
- [ ] Network drives mapped (if applicable)
- [ ] Required third-party apps installed (per role)
- [ ] BitLocker enabled
- [ ] User can access internal resources (intranet, file shares)
