# Reading Material: Diagnostic Tools & Reference Building

## 1. Built-in Diagnostic Tools

### Event Viewer Deep Dive

Event Viewer is your most important built-in diagnostic tool. It organizes logs into two main categories:

**Windows Logs** (general OS-level events):
- **Application**: Events from installed applications. Crash events, application errors, install/uninstall events.
- **Security**: Audit events. Logon attempts, account changes, policy changes. (Only populated if auditing is enabled.)
- **System**: Events from Windows components and drivers. Service start/stop, hardware events, update events.
- **Setup**: Installation and setup events. OS upgrades, component installation.
- **Forwarded Events**: Events forwarded from other machines (in domain environments).

**Applications and Services Logs** (product-specific):
- Organized by publisher and product
- Examples:
  - `Microsoft > Windows > WindowsUpdateClient` — Windows Update events
  - `Microsoft > Office` — Office application events
  - `Microsoft > OneDrive` — OneDrive sync events
  - `Microsoft > Teams` — Teams client events
  - `Microsoft > Windows > PrintService` — Print events

**Key Skills:**
- **Filtering**: Right-click log > Filter Current Log. Filter by level, event ID, source, date range.
- **Custom Views**: Create saved filters for quick access.
- **Finding related events**: Right-click an event > "Find Related Events" to see events around the same time.
- **Saving logs**: Right-click log > Save Events As... to export as .evtx file.

### Resource Monitor (`resmon`)

More detailed than Task Manager. Four tabs:
- **CPU**: Per-process CPU, services by process, associated handles (open files), associated modules (loaded DLLs)
- **Memory**: Per-process memory, physical memory map, hard fault information
- **Disk**: Per-process disk activity, which files are being read/written, disk queue length
- **Network**: Per-process network connections, TCP connections, listening ports

**Key use case:** A user says "my computer is slow." Open Resource Monitor, sort by the relevant resource, identify the culprit.

### Performance Monitor (`perfmon`)

For deeper performance analysis:
- **Real-time graphs**: Add counters (CPU, memory, disk, network) and watch in real-time
- **Data Collector Sets**: Record performance data over time for analysis
- **Reports**: Built-in system diagnostics and system performance reports

**Useful counters for troubleshooting:**
- `Processor > % Processor Time` — CPU utilization
- `Memory > Available MBytes` — Free memory
- `PhysicalDisk > % Disk Time` — Disk utilization
- `PhysicalDisk > Current Disk Queue Length` — Disk bottleneck indicator (>2 per spindle = bottleneck)
- `Network Interface > Bytes Total/sec` — Network throughput

### Process Explorer (Sysinternals)

A superior replacement for Task Manager. Download from https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer

**Key features:**
- Process tree view (parent/child relationships)
- Color-coded process types (services, packed processes, suspended)
- Hover over a process to see its command line
- Double-click a process to see:
  - Threads tab: All threads and their CPU usage
  - TCP/IP tab: All network connections
  - Strings tab: Extract strings from process memory
- VirusTotal integration (Options > Check VirusTotal.com) — hashes are checked against 70+ antivirus engines
- Find > Find DLL or Handle — find which process has a file locked

**Linux comparison:** Like `htop` + `lsof` + `strace` combined.

### Process Monitor (Sysinternals)

Real-time monitoring of file system, registry, and process/thread activity. Download from https://learn.microsoft.com/en-us/sysinternals/downloads/procmon

**Key features:**
- Captures every file read/write, registry access, process create/exit
- Filter by process name, path, operation type, result
- "Show Process Tree" — visual tree of all processes
- "Boot Logging" — captures events from system startup
- "Drop Filter" and "Clear Display" — essential keyboard shortcuts (Ctrl+D, Ctrl+X)

**Typical use case:** An app can't find a file it needs. Run ProcMon, filter to the app's process, and see what files it's trying to access and getting "NAME NOT FOUND" or "ACCESS DENIED."

**Linux comparison:** Like `strace -f` + `inotifywait` + `auditd` combined, but with a GUI and filtering.

### Autoruns (Sysinternals)

Shows everything that auto-starts on Windows — far more comprehensive than Task Manager's startup tab. Download from https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns

**Shows:**
- Logon entries (Run/RunOnce registry keys, startup folders)
- Services
- Scheduled tasks
- Drivers
- Browser extensions
- Codecs
- Explorer add-ons (shell extensions, context menu handlers)

**Use case:** A user's computer is slow, and you suspect too many auto-starting programs. Run Autoruns, sort by publisher, and disable unnecessary items.

### TCPView (Sysinternals)

Shows all active TCP and UDP connections with the owning process. Download from https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview

**Shows:**
- Local and remote address/port
- Protocol (TCP/UDP)
- State (ESTABLISHED, LISTENING, TIME_WAIT, etc.)
- Owning process and PID

**Use case:** A user asks "what is my computer talking to?" or you need to verify if a specific connection is active.

## 2. Log File Locations

### Windows System Logs
| Log | Location |
|---|---|
| Windows Update | `C:\Windows\Logs\WindowsUpdate\WindowsUpdate.log` (use `Get-WindowsUpdateLog` in PowerShell) |
| DISM | `C:\Windows\Logs\DISM\dism.log` |
| CBS (Component-Based Servicing) | `C:\Windows\Logs\CBS\CBS.log` |
| Setup | `C:\Windows\Panther\setupact.log` |
| Driver installation | `C:\Windows\INF\setupapi.dev.log` |
| BitLocker | `C:\Windows\Logs\BitLocker\` |

### Application Logs
| Application | Location |
|---|---|
| OneDrive | `%localappdata%\Microsoft\OneDrive\logs\` |
| Teams (Classic) | `%appdata%\Microsoft\Teams\logs.txt` |
| Teams (New) | `%localappdata%\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\logs\` |
| Office | `%localappdata%\Microsoft\Office\Logging\` |
| Outlook | `%localappdata%\Microsoft\Outlook\` (also Event Viewer) |
| Edge | `%localappdata%\Microsoft\Edge\User Data\` |

### Intune / MDM Logs
| Log | Location |
|---|---|
| Intune Management Extension | `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\` |
| Device Management | Event Viewer > Applications and Services Logs > Microsoft > Windows > DeviceManagement-* |
| Autopilot | `C:\Windows\Provisioning\Logs\` |

### Security Logs
| Log | Location |
|---|---|
| Windows Defender | Event Viewer > Applications and Services Logs > Microsoft > Windows > Windows Defender |
| Firewall | Event Viewer > Applications and Services Logs > Microsoft > Windows > Windows Firewall With Advanced Security |
| Audit events | Event Viewer > Windows Logs > Security |

## 3. Common Error Codes

### Windows Error Codes (hex)
| Code | Meaning |
|---|---|
| 0x00000000 | Success |
| 0x80070002 | The system cannot find the file specified |
| 0x80070003 | The system cannot find the path specified |
| 0x80070005 | Access denied |
| 0x80070057 | Invalid parameter |
| 0x80070422 | Service cannot be started (disabled) |
| 0x80070643 | Fatal error during installation |
| 0x80070BC2 | Restart required to complete installation |
| 0x8004de40 | OneDrive network error |
| 0x8007000E | Out of memory |

### Office Activation Error Codes
| Code | Meaning |
|---|---|
| 0x8004DE50 | Network connectivity issue |
| 0x8004DE74 | Proxy/authentication issue |
| 0xC004C003 | Invalid product key |
| 0xC004F074 | KMS activation failed |

## 4. Building Your Reference

### One-Page Cheat Sheet Structure
Create a single document (printed or digital) with:
1. **Top 15 issues** with one-line diagnostic steps
2. **Key commands** grouped by category
3. **Log locations** table
4. **Admin portal URLs**
5. **Escalation contacts**
6. **Common error codes** and their meanings

Update this weekly as you learn new things.

### Browser Bookmark Structure
Organize bookmarks in folders:
```
IT Support/
├── Microsoft/
│   ├── Microsoft Learn
│   ├── M365 Admin Center
│   ├── Entra Admin Center
│   ├── Intune Admin Center
│   ├── Service Health
│   ├── Windows Release Health
│   └── Tech Community
├── Internal/
│   ├── Ticketing System
│   ├── Knowledge Base
│   ├── SOPs
│   └── System Inventory
└── Tools/
    ├── Sysinternals
    ├── NirSoft utilities
    └── Online hash/checksum tools
```
