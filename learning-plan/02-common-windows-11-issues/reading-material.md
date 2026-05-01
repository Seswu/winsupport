# Reading Material: Common Windows 11 Issues & Troubleshooting

## 1. Login and Lockout Issues

### Authentication Flow in Windows
When a user logs into a domain-joined Windows machine:
1. User enters credentials at the login screen
2. Windows authenticates against Active Directory (on-prem) or Entra ID (cloud)
3. Kerberos tickets are issued for domain resources
4. User profile is loaded (local, roaming, or hybrid)
5. Group Policies are applied
6. Startup programs and mapped drives are initialized

### Common Failure Points

**"The referenced account is currently locked out"**
- Cause: Too many failed password attempts exceeded the account lockout threshold
- Fix: Wait for lockout duration to expire, or an admin unlocks the account in AD/Entra ID
- Root cause: Usually a device or service using old cached credentials repeatedly failing (phone with old mail password, mapped drive, scheduled task)

**"Your Microsoft account or local account password has expired"**
- Cause: Password expiration policy enforced by domain
- Fix: User must change password at next logon

**"The trust relationship between this workstation and the primary domain failed"**
- Cause: Machine account password in AD is out of sync (common when machine was offline for extended periods)
- Fix: Remove from domain and rejoin, or use `Test-ComputerSecureChannel -Repair` in PowerShell

**Windows Hello issues**
- PIN not working, facial recognition not recognized, fingerprint not recognized
- Fix: Reset Windows Hello (Settings > Accounts > Sign-in options > Windows Hello > Remove and re-setup)

### Credential Manager
Location: Control Panel > Credential Manager (or `control /name Microsoft.CredentialManager`)

Two vaults:
- **Web Credentials**: Browser-saved passwords (Edge)
- **Windows Credentials**: Network shares, RDP connections, Outlook, generic app credentials

Clearing a stored credential: Expand the entry > Remove. The next time the app connects, it will prompt for credentials again.

## 2. Slow Performance Troubleshooting

### The Methodical Approach
1. **Identify the bottleneck**: Open Resource Monitor (`resmon`) or Task Manager > Performance. Is it CPU, memory, disk, or network?
2. **Identify the culprit**: Sort processes by the bottleneck resource. What's consuming the most?
3. **Determine if it's expected or abnormal**: Is it a known heavy app (Chrome with 100 tabs) or something unexpected (svchost.exe using 80% CPU)?
4. **Take action**: End the process, update the software, adjust settings, or escalate

### Common Causes of Slowness

**High Disk Usage (100% disk)**
- Often caused by Windows Search indexing, Superfetch (SysMain), or Windows Update
- Check with Resource Monitor > Disk tab > sort by Total (B/sec)
- On HDDs (not SSDs), this is much more common
- Known fix: Disable SysMain service (controversial, test first), limit Windows Search indexing scope

**High CPU from svchost.exe**
- `svchost.exe` hosts Windows services. Multiple instances exist, each hosting different services
- To identify which service: Task Manager > Details > right-click the svchost.exe > Go to Service(s)
- Common culprits: Windows Update (wuauserv), BITS, Windows Defender scan

**Memory pressure**
- Check Task Manager > Performance > Memory. Note "In use" vs. "Available"
- If available memory is low, check for memory leaks (a process whose memory usage keeps growing)
- Non-paged pool memory that keeps growing usually indicates a driver memory leak

**Startup overload**
- Task Manager > Startup apps > sort by "Startup impact"
- Disable unnecessary items. Note: some corporate-mandated items may be grayed out (managed by policy)

## 3. Windows Update Issues

### Update Components
Windows Update relies on several components:
- **Windows Update service** (`wuauserv`): Orchestrates the update process
- **Background Intelligent Transfer Service (BITS)**: Handles the actual download
- **Cryptographic Services** (`CryptSvc`): Validates update signatures
- **Windows Installer** (`msiserver`): Installs MSU/MSI packages

### Where Updates Live
- Downloaded updates: `C:\Windows\SoftwareDistribution\Download`
- Update history: Settings > Windows Update > Update history
- Installed updates list: `wmic qfe list` or `Get-HotFix` in PowerShell

### Common Update Problems

**Updates stuck downloading or installing**
- Fix 1: Run Windows Update Troubleshooter (Settings > System > Troubleshoot)
- Fix 2: Stop wuauserv and BITS, delete `C:\Windows\SoftwareDistribution\*`, restart services
- Fix 3: Run `DISM /Online /Cleanup-Image /RestoreHealth` then `sfc /scannow`

**Error codes**
- `0x80070002` or `0x80070003`: Corrupted update files. Clear SoftwareDistribution.
- `0x80070005`: Access denied. Run as admin, check permissions.
- `0x80070422`: Service not running. Check that wuauserv and BITS are running.
- `0x800f0922`: Disk space issue. Free up space.
- `0x80244xxx`: Network/connectivity issue. Check proxy, DNS, connectivity to Windows Update.

**Feature updates failing**
- Usually due to incompatible drivers or software
- Check `C:\Windows\$WINDOWS.~BT\Sources\Panther\setupact.log` for details
- Known compatibility issues are listed on Windows Release Health page

## 4. Network Issues

### Diagnostic Commands

**CMD:**
```cmd
ipconfig /all              # Full network configuration
ipconfig /release          # Release DHCP lease
ipconfig /renew            # Request new DHCP lease
ipconfig /flushdns         # Clear DNS cache
ping <host>                # Basic connectivity test
tracert <host>             # Route tracing
nslookup <host>            # DNS lookup
pathping <host>            # Combined ping + tracert with loss stats
netsh winsock reset        # Reset Winsock catalog (fixes many connectivity issues)
netsh int ip reset         # Reset TCP/IP stack
```

**PowerShell:**
```powershell
Test-NetConnection <host>  # Comprehensive connectivity test (replaces ping + more)
Test-NetConnection <host> -TraceRoute  # Traceroute
Test-NetConnection <host> -Port 443    # Test specific port
Resolve-DnsName <host>     # DNS lookup (more detailed than nslookup)
Get-NetAdapter             # List network adapters
Get-NetIPAddress           # List IP addresses
```

### Common Network Problems

**"No internet, secured"**
- Usually DNS or gateway issue. Check with `ipconfig /all`.
- Try `ping 8.8.8.8` (tests internet without DNS). If that works, it's DNS.
- Try `ping google.com`. If that fails but 8.8.8.8 works, it's DNS.

**Intermittent disconnections**
- Often driver-related. Update network adapter driver.
- Check Event Viewer > System for network adapter errors.
- Power management: Device Manager > network adapter > Properties > Power Management > uncheck "Allow the computer to turn off this device to save power"

**Proxy issues**
- Corporate environments often use automatic proxy configuration (PAC files)
- Settings > Network & internet > Proxy
- Check `netsh winhttp show proxy` for system-wide proxy settings

## 5. Driver Issues

### How Windows Drivers Work
Drivers are kernel-mode or user-mode software that allow Windows to communicate with hardware. They come as:
- `.sys` files (kernel-mode drivers, like kernel modules in Linux)
- `.dll` files (user-mode drivers)
- Installed via INF files, or packaged as MSIs/EXEs

### Device Manager (`devmgmt.msc`)
Device symbols:
- **Normal device**: Working fine
- **Yellow triangle**: Problem (click for error code)
- **Down arrow**: Device disabled
- **Blue "i" on white**: Manually configured (not using auto-config)
- **Unknown device**: No driver installed

### Device Problem Error Codes
| Code | Meaning | Typical Fix |
|---|---|---|
| 10 | Device cannot start | Update driver, check hardware |
| 28 | Driver not installed | Install driver |
| 39 | Driver corrupted or missing | Reinstall driver |
| 43 | Device reported problem | Replace hardware or update driver |
| 45 | Device not present (currently disconnected) | Reconnect device |

### Driver Rollback
If an update causes problems:
- Device Manager > right-click device > Properties > Driver tab > Roll Back Driver
- Only available if a previous version was installed
- Grayed out if no previous version exists

### Where to Get Drivers
1. Windows Update (often has basic/generic drivers)
2. Device manufacturer (Dell, HP, Lenovo support pages)
3. Component manufacturer (Intel, NVIDIA, RealDirect)
4. Never use "driver updater" third-party tools

## 6. Blue Screen of Death (BSOD)

### What Causes BSOD
A BSOD (bug check) occurs when the kernel encounters an error it cannot safely recover from. Common causes:
- Faulty drivers (most common)
- Failing hardware (RAM, disk)
- Corrupted system files
- Overclocking

### Reading a BSOD
The screen shows:
- A sad face emoticon (Windows 8+)
- A stop code (e.g., `IRQL_NOT_LESS_OR_EQUAL`, `SYSTEM_THREAD_EXCEPTION_NOT_HANDLED`)
- Sometimes the failing driver filename (e.g., `nvlddmkm.sys`)
- A QR code linking to Microsoft's BSOD lookup

### Minidump Files
Location: `C:\Windows\Minidump\`
These files contain diagnostic information about the crash. Analyze them with:
- **BlueScreenView** (NirSoft): Simple GUI, shows likely culprit driver
- **WinDbg** (from Windows SDK): Full-featured, steep learning curve
- **WhoCrashed**: User-friendly analysis with explanations

### Safe Mode
Boot options:
- Settings > System > Recovery > Advanced startup > Restart now > Troubleshoot > Advanced options > Startup Settings > Restart > Press 4, 5, or 6
- Or hold Shift while clicking Restart from Start menu
- Options:
  - 4: Safe Mode (minimal drivers)
  - 5: Safe Mode with Networking (adds network drivers)
  - 6: Safe Mode with Command Prompt

## 7. File and Permission Issues

### NTFS Permissions Explained
NTFS uses Access Control Lists (ACLs) with the following permission types:
- **Full control**: Everything + change permissions + take ownership
- **Modify**: Read, write, delete, execute
- **Read & execute**: Read files, run programs
- **Read**: View files and folders
- **Write**: Create files and folders

### Inheritance
Permissions flow from parent folder to child by default. A file inherits permissions from its containing folder.

To see inheritance: Properties > Security > Advanced > check "Enable inheritance"

### Taking Ownership
When you can't access a file:
1. Properties > Security > Advanced > Owner > Change
2. Enter your username > Check Names > OK
3. Check "Replace owner on subcontainers and objects"
4. Apply, then grant yourself Full Control

### Effective Permissions
Properties > Security > Advanced > Effective Access tab > Select a user > View effective permissions. This shows the actual permissions after all group memberships and deny rules are resolved.

**Important:** Deny always overrides Allow. If a user is in a group with Allow Read but also in a group with Deny Read, they cannot read.

## 8. Printer Issues

### Print Spooler Architecture
1. Application sends print job to spooler
2. Spooler converts job to EMF/XPS format
3. Spooler sends job to port driver (USB, network, etc.)
4. Port driver sends to printer

### Common Problems

**Print jobs stuck in queue**
- Cancel all jobs: Right-click printer > See what's printing > Printer > Cancel All Documents
- If stuck: Restart Print Spooler service
- Nuclear option: Stop spooler, delete `C:\Windows\System32\spool\PRINTERS\*`, start spooler

**"Printer offline"**
- Check physical connection (USB cable, network connectivity)
- Printer properties > Ports tab > verify correct port
- SNMP status can falsely report offline: Ports tab > Configure Port > uncheck SNMP Status Enabled

**Driver mismatch**
- Universal Print Driver vs. manufacturer-specific driver
- v3 vs. v4 driver model (v4 is the newer, simplified model)
- Remove old driver: Print Management console > Drivers > right-click > Remove > Remove driver and driver package
