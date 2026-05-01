# Hands-On Exercises: Common Windows 11 Issues

All exercises should be performed in a Windows 11 VM.

---

## Exercise 1: Credential Manager Investigation

**Objective:** Understand where Windows stores cached credentials.

1. Open Credential Manager (search "Credential Manager" or `control /name Microsoft.CredentialManager`)
2. Click "Windows Credentials"
3. Examine the entries. Note entries for:
   - Network shares (MicrosoftAccount:target=...)
   - Remote Desktop connections
   - Outlook/Office connections
   - Generic credentials (apps that use the Windows vault)
4. Add a new Windows credential:
   - Click "Add a Windows credential"
   - Internet/network address: `test-server.company.local`
   - User name: `testuser`
   - Password: `TestPass123!`
5. Open Command Prompt and run: `cmdkey /list`. Note your new credential appears.
6. Remove it: `cmdkey /delete:test-server.company.local`
7. Verify it's gone in Credential Manager.

---

## Exercise 2: Simulate and Diagnose a Slow System

**Objective:** Practice identifying resource bottlenecks.

1. Open Task Manager > Performance tab. Leave it visible.
2. Create CPU load:
   - Open PowerShell and run: `while($true){}` (press Ctrl+C to stop after 30 seconds)
   - Observe the CPU spike in Task Manager
   - Identify the process consuming CPU
3. Create memory pressure:
   - Open PowerShell and run:
     ```powershell
     $array = @()
     1..100000000 | ForEach-Object { $array += $_ }
     ```
   - Observe memory growth in Task Manager
   - Press Ctrl+C to stop. Note: memory may not be fully released immediately
4. Open Resource Monitor (`resmon`). Examine:
   - CPU tab: Average CPU, per-core utilization, processes
   - Memory tab: Hard faults, standby memory, free memory
   - Disk tab: Which processes are reading/writing, queue length
   - Network tab: Per-process network usage
5. Create a startup burden:
   - Press `Win + R`, type `shell:startup`. This opens the user's startup folder.
   - Create a shortcut to Notepad in this folder.
   - Reboot the VM and observe Notepad auto-starting.
   - Remove the shortcut.

---

## Exercise 3: Windows Update Troubleshooting

**Objective:** Practice diagnosing and fixing Windows Update issues.

1. Check current update state:
   - Settings > Windows Update > Check for updates
   - View update history
2. Check update status via PowerShell:
   ```powershell
   Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 10
   Get-Service -Name wuauserv, bits, cryptsvc | Format-Table Name, Status, StartType
   ```
3. Simulate a corrupted update cache:
   - Stop services:
     ```cmd
     net stop wuauserv
     net stop bits
     net stop cryptsvc
     ```
   - Rename the SoftwareDistribution folder:
     ```cmd
     ren C:\Windows\SoftwareDistribution SoftwareDistribution.old
     ```
   - Restart services:
     ```cmd
     net start wuauserv
     net start bits
     net start cryptsvc
     ```
   - Check for updates again
4. Generate readable Windows Update logs:
   ```powershell
   Get-WindowsUpdateLog
   ```
   - This creates `WindowsUpdate.log` on your Desktop
   - Open it and search for errors

---

## Exercise 4: Network Diagnostics

**Objective:** Practice the full network diagnostic workflow.

1. Run the complete diagnostic sequence:
   ```cmd
   ipconfig /all
   ping 127.0.0.1           # Loopback test (tests TCP/IP stack)
   ping <your_ip>           # Own IP test (tests network adapter)
   ping <gateway_ip>        # Gateway test (tests local network)
   ping 8.8.8.8             # Internet test (no DNS)
   ping google.com          # DNS test
   nslookup google.com      # Detailed DNS test
   ```
2. Note what each step tells you. If loopback works but own IP fails, the adapter driver is broken. If gateway works but 8.8.8.8 fails, the router/firewall is the issue.
3. Use PowerShell's `Test-NetConnection`:
   ```powershell
   Test-NetConnection google.com
   Test-NetConnection google.com -TraceRoute
   Test-NetConnection google.com -Port 443
   Test-NetConnection -InformationLevel Detailed
   ```
4. Simulate a DNS issue:
   - Open `C:\Windows\System32\drivers\etc\hosts` as Administrator
   - Add a line: `127.0.0.1 example.com`
   - Run `ping example.com`. Note it resolves to 127.0.0.1
   - Remove the line. Run `ipconfig /flushdns`
   - Run `ping example.com` again. Normal resolution returns.
5. Simulate Winsock corruption and fix:
   - This is dangerous to actually simulate, but practice the fix:
     ```cmd
     netsh winsock reset
     netsh int ip reset
     ```
   - Note: requires reboot to take effect

---

## Exercise 5: Driver Troubleshooting

**Objective:** Practice managing drivers in Device Manager.

1. Open Device Manager (`devmgmt.msc`)
2. Find your display adapter. Right-click > Properties > Driver tab. Note:
   - Driver provider
   - Driver date
   - Driver version
   - Whether "Roll Back Driver" is available
3. View driver details: Driver tab > Driver Details. Note the `.sys` file paths.
4. For a non-critical device (e.g., a USB controller or HID device):
   - Right-click > Disable device. Observe what happens.
   - Right-click > Enable device.
5. Check for hidden devices:
   - View > Show hidden devices
   - Note grayed-out entries (disconnected devices with drivers still installed)
6. View driver event history:
   - Right-click device > Properties > Events tab
   - Note the timeline of driver installation, updates, and errors
7. Export driver information:
   ```powershell
   Get-WindowsDriver -Online | Format-Table ClassName, ProviderName, Driver, Date, Version
   ```

---

## Exercise 6: Crash Simulation and Analysis

**Objective:** Experience a BSOD and analyze the dump.

**WARNING:** This will crash your VM. Only do this in a disposable VM.

1. Enable manual crash trigger via registry (this is a documented Microsoft debugging feature):
   ```
   reg add "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
   ```
2. Reboot the VM.
3. Hold right Ctrl and press Scroll Lock twice. This will trigger a BSOD (BugCheck 0xE2 = MANUALLY_INITIATED_CRASH).
4. The VM will reboot. After boot, check `C:\Windows\Minidump\` for a `.dmp` file.
5. Download NirSoft BlueScreenView (https://www.nirsoft.net/utils/blue_screen_view.html) or use WinDbg.
6. Open the minidump file and analyze:
   - What was the bug check code?
   - What driver was involved (if any)?
   - What was the crash address?
7. Check Event Viewer > System for Event ID 1001 (BugCheck) for details.

---

## Exercise 7: NTFS Permission Troubleshooting

**Objective:** Practice diagnosing and fixing permission issues.

1. Create a test folder: `C:\test-perms`
2. Create a file inside: `C:\test-perms\secret.txt`
3. Right-click the folder > Properties > Security > Advanced
4. Click "Disable inheritance" > "Convert inherited permissions into explicit permissions"
5. Remove "Users" group from the permission list
6. Add a specific user (your account) with Read-only permission:
   - Add > Select a principal > type your username > OK
   - Basic permissions: Read
7. Try to create a new file in the folder. You should get "access denied"
8. Now take ownership:
   - Properties > Security > Advanced > Owner > Change
   - Type your username > Check Names > OK
   - Check "Replace owner on subcontainers and objects"
   - Apply
9. Now grant yourself Full Control and verify you can write to the folder.
10. Practice viewing effective permissions:
    - Properties > Security > Advanced > Effective Access tab
    - Select a user > View effective permissions
    - Note the difference between what groups allow and what the user actually has

---

## Exercise 8: Print Spooler Troubleshooting

**Objective:** Practice managing the print spooler and clearing stuck jobs.

1. Install a virtual printer (e.g., "Microsoft Print to PDF" should already be installed)
2. Send a test print to it
3. Open the print queue (double-click the printer icon in the system tray or Settings > Bluetooth & devices > Printers & scanners > click printer > Open print queue)
4. Simulate a stuck job:
   - Stop the Print Spooler: `net stop spooler`
   - Try to print something. It will queue but not process.
   - Note the job status shows "Spooling" or "Error"
5. Clear stuck jobs:
   ```cmd
   net stop spooler
   del /Q /F /S C:\Windows\System32\spool\PRINTERS\*
   net start spooler
   ```
6. Verify the queue is empty
7. Check Print Service logs:
   - Event Viewer > Applications and Services Logs > Microsoft > Windows > PrintService
   - Right-click > Operational > Enable Log (if disabled)
   - Print something and watch the events populate
