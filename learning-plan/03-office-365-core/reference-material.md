# Reference Material: Office 365 Core

## Outlook Quick Reference

### Connection Status Window
Hold Ctrl > right-click Outlook system tray icon > Connection Status

| Status | Meaning |
|---|---|
| Connected | Normal operation |
| Connecting | In process of connecting |
| Disconnected | Not connected, will retry |
| Need Password | Authentication required |

The window shows connection details for:
- Exchange server connection
- Address Book (OAB) download
- Free/Busy information
- Public folders (if applicable)

### AutoDiscover Test
In Outlook:
1. Hold Ctrl > right-click system tray icon
2. Click "Test Email AutoConfiguration"
3. Enter email address, check "Use AutoDiscover"
4. Click Test
5. Review the Log tab for errors

### Useful Outlook Switches
| Switch | Purpose |
|---|---|
| `outlook.exe /safe` | Start in safe mode |
| `outlook.exe /resetnavpane` | Reset navigation pane |
| `outlook.exe /cleanrules` | Clear client rules |
| `outlook.exe /cleanserverrules` | Clear server rules |
| `outlook.exe /resetfolders` | Restore default folder names |
| `outlook.exe /importpst` | Import PST file wizard |
| `outlook.exe /profile` | Prompt for profile selection |

### OST File Locations
`%localappdata%\Microsoft\Outlook\`

### Profile Configuration Files
Registry: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Outlook\Profiles`

## OneDrive Quick Reference

### Icon Status Meanings
| Icon | Meaning |
|---|---|
| Blue cloud | Online-only (not on disk) |
| Green checkmark in white circle | Locally available (cached) |
| Solid green checkmark | Always kept on device |
| Blue arrows (syncing) | Currently syncing |
| Red circle with white X | Sync error |
| Gray minus in circle | File excluded from sync |

### Key Locations
| Item | Path |
|---|---|
| Default sync folder | `%USERPROFILE%\OneDrive - <CompanyName>` |
| Log files | `%localappdata%\Microsoft\OneDrive\logs\` |
| Settings database | `%localappdata%\Microsoft\OneDrive\settings\` |
| Personal vault | `%USERPROFILE%\OneDrive - <CompanyName>\Personal Vault` |

### Useful Commands
| Command | Purpose |
|---|---|
| `onedrive.exe` | Launch OneDrive |
| `onedrive.exe /reset` | Reset sync client |
| `onedrive.exe /shutdown` | Shut down OneDrive |

### Common Error Codes
| Code | Meaning | Fix |
|---|---|---|
| 0x8004de40 | Network issue | Check connectivity, proxy |
| 0x8004de45 | Server busy | Wait and retry |
| 0x8004de8a | Login required | Sign in again |
| 0x8004deb4 | Processing changes stuck | Reset OneDrive |
| 0x8007016a | Cloud file provider not running | Restart OneDrive |

## Teams Quick Reference

### Cache Locations
| Version | Path |
|---|---|
| Classic Teams | `%appdata%\Microsoft\Teams\` |
| New Teams | `%localappdata%\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\` |

### Teams Logs
Classic: `%appdata%\Microsoft\Teams\logs.txt`
New Teams: `%localappdata%\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\logs\`

### Teams Diagnostic Collection
In Teams: Ctrl + Shift + Alt + 1 — collects diagnostic logs and saves to Downloads

### Required URLs/Ports (for network troubleshooting)
- `*.teams.microsoft.com`
- `*.skype.com`
- `*.skypeforbusiness.com`
- UDP ports 3478-3481 (media)
- TCP port 443 (signaling)

Full list: https://learn.microsoft.com/en-us/microsoft-365/enterprise/urls-and-ip-address-ranges

## Office Quick Reference

### Office Version Info
In any Office app: File > Account > About [App]. Shows:
- Version number
- Build number
- Architecture (32-bit or 64-bit)
- Update channel

### Update Channels
| Channel | Frequency | Typical Users |
|---|---|---|
| Current | Monthly | Most users |
| Monthly Enterprise | Monthly, more stable | Organizations |
| Semi-Annual Enterprise | Twice a year | Highly regulated orgs |

### License/Activation Troubleshooting
1. File > Account — shows product info and sign-in status
2. Check if user is signed in with correct M365 account
3. Check license assignment in M365 Admin Center
4. Run: `cscript "C:\Program Files\Microsoft Office\Office16\OSPP.VBS" /dstatus` (volume license only)
5. Sign out of all Office apps and sign back in

### Office Repair Commands
```cmd
:: Quick Repair (silent)
"C:\Program Files\Common Files\Microsoft Shared\ClickToRun\OfficeC2RClient.exe" /update user updatetoversion=16.0.12345.67890

:: Online Repair requires GUI: Settings > Apps > Modify
```

### Common Office File Locations
| Item | Path |
|---|---|
| Office templates | `%appdata%\Microsoft\Templates\` |
| Normal.dotm (Word template) | `%appdata%\Microsoft\Templates\Normal.dotm` |
| Excel startup folder | `%appdata%\Microsoft\Excel\XLSTART\` |
| Office add-ins | `%appdata%\Microsoft\AddIns\` |
| Office logs | `%localappdata%\Microsoft\Office\Logging\` |

## M365 Admin Portal Quick Reference

| Portal | URL | Purpose |
|---|---|---|
| M365 Admin Center | https://admin.microsoft.com | User management, licenses, service health |
| Entra Admin Center | https://entra.microsoft.com | Identity, devices, conditional access |
| Intune Admin Center | https://intune.microsoft.com | Device management, app deployment |
| Exchange Admin Center | https://admin.exchange.microsoft.com | Mailbox management, mail flow |
| SharePoint Admin Center | https://admin.microsoft.com/sharepoint | SharePoint, OneDrive settings |
| Teams Admin Center | https://admin.teams.microsoft.com | Teams policies, meeting settings |
| Defender Portal | https://security.microsoft.com | Security alerts, threat protection |
| Purview Portal | https://purview.microsoft.com | Compliance, DLP, retention |
| Windows Release Health | https://learn.microsoft.com/en-us/windows/release-health/ | Known issues, update status |
