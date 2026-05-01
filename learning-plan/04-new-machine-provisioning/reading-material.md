# Reading Material: New Machine Provisioning Issues

## 1. The New Machine Lifecycle

When a new Windows machine arrives at a pharmaceutical company, it goes through this general flow:
1. **Procurement**: IT orders machine from approved vendor (Dell, HP, Lenovo)
2. **Image preparation**: Machine image is built from a validated, tested baseline
3. **Provisioning**: Machine is configured for the corporate environment
4. **Handoff**: Machine is delivered to the user
5. **First login**: User signs in, policies apply, apps install, OneDrive syncs

### Provisioning Methods

**Windows Autopilot (modern, cloud-first)**
- Machine ships directly to user
- User turns it on, connects to Wi-Fi
- Enters corporate email address
- Entra ID authenticates, Intune applies policies
- Apps install automatically in background
- Zero IT touch required

**Traditional Imaging (legacy, on-prem)**
- IT receives machine
- Boots from PXE or USB
- Applies a captured image (via SCCM/MECM)
- Joins to domain
- IT manually configures and delivers

**Hybrid**
- Autopilot for enrollment + domain join for on-prem resources
- Most common in organizations transitioning from on-prem to cloud

## 2. Autopilot Deep Dive

### How Autopilot Works
1. Device hardware hash is registered in Intune (by vendor, reseller, or IT)
2. When user turns on the new device and connects to internet:
   - OOBE (Out of Box Experience) runs
   - User enters email address
   - Windows checks if hardware hash is registered in Autopilot
   - If yes: Autopilot profile is applied (determines OOBE experience)
   - Device enrolls in Entra ID and Intune
   - Intune pushes apps, policies, and configuration
3. User lands at desktop with a managed machine

### Where Autopilot Fails
- **Hardware hash not registered**: Device doesn't recognize as corporate
- **No internet**: Autopilot requires internet during OOBE
- **DNS/Proxy blocking**: Can't reach Intune/Entra endpoints
- **User not licensed**: Needs Intune and Entra ID P1/P2 license
- **Profile assignment wrong**: Wrong Autopilot profile applied
- **App installation delays**: Apps queued but not yet installed — user thinks they're "missing"

## 3. Domain Join / Entra ID Join

### Domain Join (on-prem Active Directory)
- Machine authenticates to a domain controller
- Creates computer account in AD
- Group Policies apply from domain
- User logs in with domain credentials
- Requires network connectivity to domain controller

### Entra ID Join (cloud)
- Machine registers with Microsoft Entra ID (formerly Azure AD)
- User logs in with M365/cloud credentials
- Intune manages device configuration
- No domain controller required
- Requires internet connectivity

### Hybrid Entra ID Join
- Machine is both domain-joined and registered with Entra ID
- Best of both worlds: on-prem resources + cloud management
- Requires AD Connect sync between on-prem AD and Entra ID

### Checking Join Status
```cmd
:: Entra ID join status
dsregcmd /status

:: Domain join status
systeminfo | findstr /B /C:"Domain"
```

### Common Join Failures
- **DNS misconfigured**: Machine can't find domain controller or Entra endpoints
- **Time sync off**: Kerberos requires time within 5 minutes of DC
- **Account permissions**: User doesn't have permission to join machines to domain
- **Network blocks**: Firewall blocks required ports
- **SID conflicts**: Cloned images without proper sysprep

## 4. Missing Applications

### Why Apps Don't Appear Immediately
- **Intune app deployment is async**: Apps are queued and install in the background
- **Assignment delay**: User/device needs to be in the right Azure AD group
- **Group membership sync**: Azure AD group membership can take up to 24 hours to propagate
- **Installation dependencies**: Some apps require prerequisites
- **Reboot required**: Some apps won't install until after a reboot
- **User not licensed**: Some apps require specific M365 licenses

### Checking App Deployment Status
- Intune Admin Center > Devices > select device > check "Apps" tab
- On the device: Settings > Accounts > Access work or school > click account > Info > scroll to see deployed apps
- Event Viewer > Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider

### Company Portal App
Users can install available apps themselves via the Company Portal app (if enabled by IT):
- Open Company Portal > browse available apps > Install
- Shows installation status and progress

## 5. Group Policy and Configuration

### Policy Processing Order
1. Local Group Policy
2. Site-level GPO
3. Domain-level GPO
4. OU-level GPO (closest OU wins if conflicts)

### Policy Application Timing
- **Computer policies**: Applied at startup and every 90 minutes (±30 min random offset)
- **User policies**: Applied at logon and every 90 minutes (±30 min random offset)
- Force immediate application: `gpupdate /force`

### Checking Applied Policies
```cmd
:: Generate HTML report
gpresult /h C:\temp\gp-report.html

:: Text summary
gpresult /r

:: GUI view
rsop.msc
```

### Common Policy Issues
- **Policy not applying**: Check `gpresult` to verify. Run `gpupdate /force`.
- **Wrong policy applying**: Check OU membership, group filtering, WMI filters
- **Policy conflict**: Higher-priority GPO overrides lower. Check precedence in `gpresult`.
- **Slow policy processing**: Check Event Viewer for Group Policy operational logs

## 6. BitLocker

### What Is BitLocker
Full-disk encryption built into Windows Pro and Enterprise editions. Encrypts the entire drive so data is protected if the device is stolen.

### How It Works in Corporate Environments
- BitLocker is enabled automatically via Intune policy or GPO
- Recovery key is escrowed to Entra ID or Active Directory
- User never sees the key under normal operation
- Recovery is triggered by hardware changes, TPM issues, or firmware updates

### When Recovery Is Triggered
- TPM cleared or firmware updated
- Boot configuration changed
- Motherboard replaced
- PIN or USB key required but not provided
- Too many failed boot attempts

### Finding Recovery Keys
- **Entra ID**: Entra Admin Center > Devices > select device > BitLocker keys
- **Active Directory**: AD Users and Computers > computer object > Properties > BitLocker Recovery tab
- **M365 Admin Center**: admin.microsoft.com > Users > select user > BitLocker recovery keys

### User Experience During Recovery
1. Blue screen appears: "BitLocker Recovery"
2. Shows Recovery Key ID (first 8 characters)
3. User must enter the 48-digit recovery key
4. IT looks up the key using the Key ID and provides it to the user
5. After entering the key, the system boots normally

## 7. User Profile Issues

### Temporary Profile
When Windows can't load the user's profile, it creates a temporary profile:
- Desktop appears empty (default Windows desktop)
- Settings reset to defaults
- Files saved to temporary profile are deleted at logoff
- Event Viewer > Application log > Event ID 1511 or 1515 (profile load failure)

**Causes:**
- Corrupted profile on disk
- Registry hive corruption (NTUSER.DAT)
- Antivirus locking the profile files
- Profile path on network share unavailable

**Fix:**
1. Log off and log back in (sometimes resolves transient lock)
2. Reboot and try again
3. If persistent: Delete the corrupted profile (Settings > Accounts > Other users > or via System Properties > Advanced > User Profiles > Settings)
4. Log in again — a fresh profile is created
5. User data may need to be recovered from backup or OneDrive

### Roaming Profiles
In some environments, user profiles roam between machines:
- Profile is stored on a network share
- Downloaded at login, uploaded at logoff
- Can cause slow logons if profile is large
- Can cause conflicts if user logs into multiple machines simultaneously

### Known Folder Move During Provisioning
If OneDrive KFM is configured via Intune:
- During first login, OneDrive starts syncing
- Desktop, Documents, and Pictures are redirected to OneDrive
- This happens in the background — user may not notice immediately
- If the user puts files on Desktop before KFM finishes, they may briefly appear in two places

## 8. New Machine Triage Checklist

When a user reports "my new computer isn't right":

1. **Check enrollment**: `dsregcmd /status` — is it Entra-joined and Intune-managed?
2. **Check policies**: `gpresult /h report.html` — are expected policies applied?
3. **Check apps**: Compare installed apps against standard software list. Check Intune for app deployment status.
4. **Check user context**: `whoami` — correct user logged in?
5. **Check OneDrive**: Is it running? Is it syncing? Are folders in the right place?
6. **Check Outlook**: Is it configured? Connected? Downloading mail?
7. **Check BitLocker**: Is it enabled and healthy? (`manage-bde -status`)
8. **Check Windows Update**: Is it up to date? Any pending updates?
9. **Check time sync**: Correct date/time? (`w32tm /query /status`)
10. **Check network**: Can reach internal resources? Internet?
