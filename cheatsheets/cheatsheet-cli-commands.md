# Windows CLI Commands for Support

Quick-reference of CLI commands by functional area. Each section opens with the kind of problem that calls for these commands. Commands shown in CMD unless marked **PS** (PowerShell).

---

## 1. Networking Diagnostics

Use when: a user can't reach a website, server, or network share; VPN won't connect; "No internet" or "Unidentified network" errors.

| Command | What it does | Typical use |
|---|---|---|
| `ping <host>` | Tests basic reachability and round-trip time | Start here — if ping fails, either the host is down or there's a routing/firewall issue |
| `ipconfig` | Shows IP, subnet mask, default gateway | `ipconfig /all` also shows DNS servers, DHCP server, MAC address |
| `ipconfig /release` then `ipconfig /renew` | Releases and renews the DHCP lease | Fixes "limited connectivity" or wrong IP (169.254.x.x = no DHCP) |
| `ipconfig /flushdns` | Clears the DNS resolver cache | After DNS changes or "site not found" errors |
| `nslookup <host>` | Queries DNS for an IP | Compare against what the user expects — mismatch points to DNS issue |
| `tracert <host>` | Traces the route packets take to a destination | Find where connectivity drops (stops replying = firewall or outage at that hop) |
| `pathping <host>` | Combines tracert with latency/packet-loss stats | More data than tracert — runs for several minutes, shows loss per hop |
| `netstat -ano` | Lists active connections with PIDs | Check if a service is listening on the expected port; find which PID owns a connection |
| `netsh interface ip reset` | Resets TCP/IP stack to default | After malware or corrupt configuration — requires reboot |
| `netsh winsock reset` | Resets the Winsock catalog | Fixes "can't connect" after VPN disconnects or third-party firewall removal |
| `netsh advfirewall show allprofiles` | Shows firewall state per profile | Quickly confirm if Windows Firewall is blocking traffic |
| `netsh wlan show profiles` | Lists saved Wi-Fi networks | `netsh wlan show profile name="SSID" key=clear` shows the password |
| `route print` | Shows the routing table | Useful when VPN routes aren't working or traffic goes the wrong way |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Test-Connection <host>` | Like ping but returns objects — `-Count 1` for a single probe |
| `Test-NetConnection <host> -Port <port>` | Combined ping + port check — the single best network test |
| `Resolve-DnsName <host>` | More detailed than nslookup — shows all DNS records |
| `Get-NetIPAddress` | Structured view of IP config (like `ipconfig` but queryable) |
| `Get-NetAdapter` | Lists physical and virtual adapters with status |
| `Restart-NetAdapter -Name "Ethernet"` | Disables and re-enables an adapter (no need to unplug) |

---

## 2. System Information

Use when: you need to know what hardware is in the machine, which Windows version, or verify compliance with a requirement.

| Command | What it does | Typical use |
|---|---|---|
| `systeminfo` | Dumps OS version, build, install date, RAM, hotfixes, network config | First command for "tell me about this machine" |
| `systeminfo \| findstr /B /C:"OS Name" /C:"OS Version"` | Filters to just OS info | Quick check: is this Windows 11 23H2+, Pro vs Enterprise? |
| `driverquery` | Lists all installed drivers and their dates | Check if a driver is present and when it was installed |
| `msinfo32` | Opens System Information GUI | Browse hardware resources, components, software environment |
| `wmic cpu get name, maxclockspeed, numberofcores` | CPU details | Verify the machine meets minimum hardware requirements |
| `wmic memorychip get capacity, speed, memorytype` | RAM details (per stick) | Sum the capacity to check total RAM |
| `wmic diskdrive get model, size` | Disk model and size | Check if it's an SSD vs HDD (model name tells you) |
| `wmic bios get serialnumber` | Service tag / serial number | Needed for warranty lookup or asset tracking |
| `winver` | Opens the About Windows dialog | Show the user what version they're on |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Get-ComputerInfo` | Massively detailed — OS, hardware, hotfixes, all in one object |
| `Get-CimInstance Win32_OperatingSystem` | OS-level details (last boot, free RAM, OS architecture) |
| `Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3"` | Fixed disks with free/used space |
| `Get-HotFix \| Where-Object InstalledOn -gt (Get-Date).AddDays(-30)` | Recently installed updates — useful if a problem started after a patch |

---

## 3. Disk & Storage

Use when: "my computer is slow" (often disk-related), "disk full" warning, file corruption suspected, or a drive isn't showing up.

| Command | What it does | Typical use |
|---|---|---|
| `chkdsk C:` | Checks the filesystem for errors | Run `chkdsk C: /f` to fix errors (requires dismount or reboot); `chkdsk C: /r` to recover bad sectors (slower, more thorough) |
| `chkdsk C: /scan` | Online scan without dismounting | Safer — no reboot needed; reports issues without fixing |
| `diskpart` | Opens disk partitioning tool | `list disk` → `select disk N` → `list partition` or `clean` (dangerous) |
| `fsutil volume diskfree C:` | Shows free/used space with byte precision | Useful when Explorer shows wrong numbers |
| `vssadmin list shadows` | Lists Volume Shadow Copy snapshots | Tell a user "yes, you can restore that file from yesterday's snapshot" |
| `vssadmin delete shadows /for=C: /oldest` | Deletes oldest shadow copies | Free up space when Previous Versions is eating too much |
| `defrag C: /O` | Optimizes the drive | On SSDs, sends Trim command + rearranges files; on HDDs, defragments |
| `wmic logicaldisk get caption, freespace, size` | All drives with free/total in bytes | Quick overview of disk usage across all volumes |
| `powercfg /batteryreport` | Generates battery health report (HTML) | "Why won't my laptop hold a charge?" — open the generated report in a browser |
| `powercfg /energy` | Runs a 60-second energy analysis (HTML) | Identifies power-draining processes and config issues |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Get-PSDrive -PSProvider FileSystem` | All drives with free/used space (human-readable) |
| `Get-Volume` | Modern view — drive letters, health status, file system type |
| `Optimize-Volume -DriveLetter C -ReTrim -Verbose` | Manually trim an SSD |
| `Get-CimInstance Win32_LogicalDisk -Filter "Size>0" \| Select DeviceID, @{N='FreeGB';E={[math]::Round($_.FreeSpace/1GB,1)}}` | Custom view with GB values |

---

## 4. File & Permissions

Use when: "access denied" on a file or folder, a user can't save to a network share, or a file appears hidden/missing.

| Command | What it does | Typical use |
|---|---|---|
| `icacls <path>` | Shows current NTFS permissions | Check who has what access — compare effective vs. actual |
| `icacls <path> /grant user:(R,W)` | Grants permissions | Give read/write access; common flags: `F` (full), `M` (modify), `RX` (read+execute) |
| `icacls <path> /grant "domain\user":(OI)(CI)R` | Grants read with inheritance | `OI` = object inherit (files), `CI` = container inherit (subfolders) |
| `takeown /f <path>` | Takes ownership of the file | Run first when "you need permission to perform this action" even as admin |
| `takeown /f <path> /r /d y` | Takes ownership recursively | Entire folder — use when a user profile is locked down |
| `attrib` | Shows/sets file attributes | `attrib -r -h -s file.txt` removes Read-only, Hidden, System flags |
| `cipher /u /n` | Shows all encrypted (EFS) files | Identify files the user can't open because their EFS cert was lost |
| `cipher /d <path>` | Decrypts an EFS file (as the owning user) | Only works if the original cert is present |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Get-Acl <path> \| Format-List` | View full ACL with access rules |
| `Set-Acl <path> -AclObject $acl` | Apply ACL (more complex but scriptable) |
| `Get-ChildItem -Force` | Lists files including hidden/system |

---

## 5. Process & Service Management

Use when: an application is frozen, a service won't start, the machine is sluggish from runaway processes, or something is using 100% CPU/memory.

| Command | What it does | Typical use |
|---|---|---|
| `tasklist` | Lists all running processes with PID and memory | Find the PID for a process you need to kill |
| `tasklist /v` | Verbose — shows window title, status, user | Find which user is running a process |
| `tasklist /m <dll>` | Shows processes that loaded a specific DLL | Identify which app is holding a file open |
| `taskkill /PID <pid> /F` | Force-kills a process by PID | Last resort for frozen app; `taskkill /IM <name>.exe /F` by image name |
| `taskkill /IM <name>.exe /T` | Kills a process and all its children | Kill a launcher and its child processes in one shot |
| `sc query` | Lists all services with status | See which services are running, stopped, or stuck in START_PENDING |
| `sc query <service>` | Status of a specific service | `sc query wuauserv` — is Windows Update actually running? |
| `sc start <service>` | Starts a service | `sc start spooler` — start the print spooler after clearing the queue |
| `sc stop <service>` | Stops a service | `sc stop wuauserv` before clearing SoftwareDistribution |
| `sc config <service> start= auto` | Sets service startup type | `start= auto`, `demand` (manual), `disabled` |
| `net start` | Shows started services (simpler list) | Quick check |
| `net start <service>` | Starts a service | Older-style but works; same as `sc start` |
| `net stop <service>` | Stops a service | Same as `sc stop` — use whichever you remember |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Get-Process \| Sort-Object CPU -Descending \| Select -First 10` | Top 10 CPU-hungry processes |
| `Get-Process \| Where-Object WorkingSet -gt 500MB` | Processes using more than 500 MB RAM |
| `Stop-Process -Id <pid> -Force` | PowerShell version of taskkill |
| `Get-Service \| Where-Object Status -eq 'Stopped' \| Where-Object StartType -eq 'Automatic'` | Services that should be running but aren't |
| `Restart-Service <name> -Force` | Restart a service (stops dependencies too) |
| `Set-Service -Name <name> -StartupType Automatic` | Set a service to auto-start |

---

## 6. User & Account Management

Use when: investigating login failures, verifying group memberships, resetting local passwords, or checking account lockout state.

| Command | What it does | Typical use |
|---|---|---|
| `whoami` | Current username | "What account am I running as right now?" |
| `whoami /user` | Current user's SID | Useful for correlating with registry/SID entries |
| `whoami /groups` | All group memberships including built-in | "Does this user actually have admin rights?" Check for `BUILTIN\Administrators` |
| `whoami /all` | Everything — user, groups, privileges, SIDs | One command to fully identify the user's security context |
| `net user <name>` | Shows user account details (local) | Password last set, expiry, lockout status, group membership |
| `net user <name> *` | Sets password for a local user (prompts) | Reset a local account password |
| `net user <name> /active:yes` | Enables a disabled account | Re-enable an account that was disabled |
| `net localgroup` | Lists all local groups | See what groups exist on the machine |
| `net localgroup Administrators` | Members of Administrators group | "Who can run as admin on this machine?" |
| `net localgroup Administrators <domain\user> /add` | Adds a user to a group | Grant admin rights (do this sparingly) |
| `net localgroup "Remote Desktop Users" <user> /add` | Adds RDP access | Allow a user to remote into this machine |
| `net accounts` | Password/local account policy | Lockout threshold, lockout duration, password age |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Get-LocalUser` | List all local users with status, enabled/disabled |
| `Get-LocalGroupMember -Group "Administrators"` | Members of Administrators group |
| `Set-LocalUser -Name <name> -Password (Read-Host -AsSecureString)` | Set local user password |
| `Enable-LocalUser / Disable-LocalUser` | Toggle account state |
| `Get-WinEvent -FilterHashtable @{LogName='Security';ID=4625} \| Select -First 10` | Recent failed logon attempts |

---

## 7. System Health & Repair

Use when: Windows itself seems corrupt, files are missing, updates won't install, or the machine is behaving erratically with no clear cause.

| Command | What it does | Typical use |
|---|---|---|
| `sfc /scannow` | Scans and repairs protected system files | Start here for corruption. If it finds issues it can't fix, proceed to DISM |
| `sfc /verifyonly` | Checks without repairing | Faster — just tells you if there's a problem |
| `DISM /Online /Cleanup-Image /CheckHealth` | Quick check of image health | Is the component store healthy? Returns good/bad/corrupt |
| `DISM /Online /Cleanup-Image /ScanHealth` | In-depth scan of image health | Reports corruption without fixing — no risk, good first step |
| `DISM /Online /Cleanup-Image /RestoreHealth` | Repairs component store corruption | Run after `sfc` fails; sources files from Windows Update (or specify `/Source` with a mounted ISO) |
| `DISM /Online /Cleanup-Image /StartComponentCleanup` | Cleans superseded component files | Free up disk space from WinSxS after updates |
| `shutdown /r /t 0` | Immediate reboot | Use when the Start menu won't work or GUI is unresponsive |
| `shutdown /r /o /t 0` | Reboot into Advanced Boot Options | Access Safe Mode, Startup Repair, or recovery environment |
| `shutdown /s /t 0` | Immediate shutdown | Same — bypasses Start menu |
| `bcdedit` | Boot Configuration Data editor | View/edit boot settings without booting into recovery |
| `bcdedit /enum` | Enumerate boot entries | See all OS entries, boot loaders, and parameters |
| `reagentc /info` | Shows Windows Recovery Environment status | Is WinRE enabled? Does the recovery image exist? |
| `reagentc /enable` | Re-enables WinRE | If recovery tools won't load |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Repair-WindowsImage -Online -RestoreHealth` | PS version of DISM |
| `Get-WindowsEdition -Online` | Current edition, SKU, and version |
| `Checkpoint-Computer -Description "Before fix" -RestorePointType MODIFY_SETTINGS` | Create a System Restore point before making changes |

---

## 8. Registry

Use when: a Windows setting can't be changed through the GUI, a policy is stuck, a program needs to be removed from startup, or last-resort troubleshooting where GUI options fail.

| Command | What it does | Typical use |
|---|---|---|
| `reg query <key>` | Lists values under a key | `reg query HKCU\Control Panel\Desktop` to see current wallpaper, screen saver settings |
| `reg query <key> /v <value>` | Gets a specific value | `reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` to check installed programs |
| `reg add <key> /v <name> /t <type> /d <data>` | Adds or modifies a registry value | Example: `reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 0 /f` |
| `reg delete <key> /v <name> /f` | Deletes a registry value | Remove a lingering entry — double-check before deleting |
| `reg delete <key> /f` | Deletes an entire key | **Dangerous** — only if you're sure |
| `reg compare <key1> <key2>` | Compares two registry keys | Useful after imaging — "what did this installer change?" |
| `reg export <key> <file.reg>` | Exports a key to a `.reg` file | Backup before making changes |
| `reg import <file.reg>` | Imports from `.reg` file | Apply settings from a known-good backup |
| `regini <script>` | Apply registry changes from a script | Advanced — for mass changes with page-level settings |

**PowerShell additions:**

| PS Command | What it does |
|---|---|
| `Get-ItemProperty -Path <regpath>` | Equivalent to `reg query` |
| `Set-ItemProperty -Path <regpath> -Name <value>` | Equivalent to `reg add` |
| `New-Item -Path <regpath>` | Create a new key |
| `Remove-ItemProperty -Path <regpath> -Name <value>` | Delete a value |

---

## Quick Reference: When to Run What

| Symptom | First command | Second command |
|---|---|---|
| "Can't connect to anything" | `ipconfig /all` / `ping 8.8.8.8` | `netsh interface ip reset` |
| "This specific site won't load" | `nslookup <site>` | `tracert <site>` |
| "My app crashed" | `eventvwr` → Application log | Look for Event ID 1000 |
| "Windows Update is stuck" | `net stop wuauserv` | Delete `C:\Windows\SoftwareDistribution` |
| "100% disk usage" | `tasklist` (find the culprit) | `chkdsk` / check for Windows Update running |
| "Access denied on a folder" | `icacls <path>` | `takeown /f <path> /r /d y` |
| "Service won't start" | `sc query <service>` | Check Event Viewer System log for Event ID 7000 |
| "Blue screens randomly" | Note the stop code | Run `chkdsk /f` and `sfc /scannow` |
| "Forgot my password" | `net user <name> *` (local only) | For domain accounts: use admin portal or self-service |
| "Machine feels corrupt" | `sfc /scannow` | `DISM /Online /Cleanup-Image /RestoreHealth` |

---

> **Tip:** Append ` \| clip` (CMD) or ` \| Set-Clipboard` (PS) to any command to pipe the output directly to your clipboard — paste into a ticket note instantly.
