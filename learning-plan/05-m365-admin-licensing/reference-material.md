# Reference Material: Microsoft 365 Admin & Licensing

## Admin Portal URLs Quick Reference

| Portal | URL | Purpose |
|---|---|---|
| M365 Admin Center | https://admin.microsoft.com | Hub for all admin tasks |
| Entra Admin Center | https://entra.microsoft.com | Identity, devices, access |
| Intune Admin Center | https://intune.microsoft.com | Device and app management |
| Exchange Admin Center | https://admin.exchange.microsoft.com | Mailbox, mail flow |
| SharePoint Admin Center | https://admin.microsoft.com/sharepoint | SharePoint, OneDrive |
| Teams Admin Center | https://admin.teams.microsoft.com | Teams policies |
| Defender Portal | https://security.microsoft.com | Security |
| Purview Portal | https://purview.microsoft.com | Compliance |
| Power Platform Admin | https://admin.powerplatform.microsoft.com | Power Apps, Power Automate |
| Viva Goals Admin | https://goals.viva.com | Viva |

## License Comparison Quick Reference

| Service | E3 | E5 | Business Premium | F3 |
|---|---|---|---|---|
| Office desktop apps | Yes | Yes | Yes | Web/mobile only |
| Exchange Online | 100 GB | 100 GB | 50 GB | 2 GB |
| OneDrive | 1 TB | 1 TB | 1 TB | 2 GB |
| SharePoint | Yes | Yes | Yes | Yes |
| Teams | Yes | Yes | Yes | Yes |
| Intune | Via add-on | Via add-on | Yes | Yes |
| Windows Enterprise | Yes | Yes | No | No |
| Defender for Office 365 | Plan 1 | Plan 2 | No | No |
| Advanced eDiscovery | No | Yes | No | No |
| Power BI Pro | No | Yes | No | No |
| Phone System | No | Yes | No | No |

## Sign-in Log Interpretation

### Common Failure Reasons
| Error | Meaning | Fix |
|---|---|---|
| Invalid username or password | Wrong credentials | Reset password |
| Account locked out | Too many failed attempts | Unlock account, find source |
| MFA denied/rejected | User rejected MFA prompt | Re-prompt, check Authenticator |
| MFA timed out | User didn't respond in time | Re-prompt |
| Conditional Access block | Policy blocked access | Review CA policy, check conditions |
| Device not compliant | Device doesn't meet compliance | Check Intune compliance |
| Sign-in risk too high | Risky sign-in detected | Verify with user, reset password |
| Account disabled | Account is disabled | Enable account |
| Password expired | Password past expiration | Force password change |

## Entra Device Status Fields

| Field | Values | Meaning |
|---|---|---|
| Join Type | Entra ID Joined, Hybrid, Registered | How the device is connected |
| MDM | Intune, None | Which MDM manages the device |
| Compliance | Compliant, Non-compliant, Pending | Meets Intune compliance policies |
| Operating System | Windows 11, etc. | OS of the device |
| Last sign-in | Date/time | Most recent user sign-in |

## Group-Based Licensing

Prerequisites: Entra ID P1 or higher

How it works:
1. Create a security group
2. Assign a license to the group (not individual users)
3. Add users to the group — they automatically get the license
4. Remove users from the group — license is removed

Benefits:
- Easier management
- Automated onboarding/offboarding
- Clear visibility of who should have what

## Admin Role Permissions

| Role | Can Reset Passwords | Manage Licenses | Manage Devices | View Sign-in Logs |
|---|---|---|---|---|
| Global Admin | Yes | Yes | Yes | Yes |
| User Admin | Yes | Yes | Limited | Yes |
| Helpdesk Admin | Yes | No | No | Limited |
| Intune Admin | No | No | Yes | No |
| Exchange Admin | Yes | No | No | No |
| Reports Reader | No | No | No | Yes (reports only) |

## PowerShell Admin Modules

| Module | Purpose | Install Command |
|---|---|---|
| Microsoft.Graph | Entra ID / M365 management | `Install-Module Microsoft.Graph` |
| ExchangeOnlineManagement | Exchange Online | `Install-Module ExchangeOnlineManagement` |
| MicrosoftTeams | Teams management | `Install-Module MicrosoftTeams` |
| MSOnline (legacy) | Legacy Entra management | `Install-Module MSOnline` |
| AzureAD (deprecated) | Deprecated, use Microsoft.Graph | N/A |
