# Event Viewer Log Map

A problem-first guide: "Where do I look in Event Viewer when the user reports X?" Includes which log, which Event IDs, what to filter for, and what the results mean.

---

## Opening Event Viewer

- **GUI:** `eventvwr.msc` (Win+R, or right-click Start > Event Viewer)
- **PowerShell:** `Get-WinEvent -LogName <Log>` — or the full `wevtutil` set of tools
- **Quick filter:** Right-click a log → **Filter Current Log** → check Critical + Error + Warning

---

## Log-to-Problem Matrix

| If the user says... | Open this log | Filter for... | What to look for |
|---|---|---|---|
| "My app crashed / stopped working" | **Application** | Event ID 1000, 1001, 1026 | Faulting module name, exception code, faulting module path |
| "Blue screen / random reboot" | **System** | Event ID 41, 1001, 6008 | Bugcheck code (e.g., `0x00000050`), bugcheck parameters, timestamp |
| "I can't log in" | **Security** | Event ID 4625, 4776 | Account name, Sub Status code (translates the reason), source IP |
| "My account is locked" | **Security** | Event ID 4740 | Source machine that triggered the lockout, caller computer name |
| "Windows Update is stuck / failing" | **Applications and Services > Microsoft > Windows > WindowsUpdateClient > Operational** | Level: Error | HRESULT code, update GUID, failure detail |
| "My printer won't print" | **Applications and Services > Microsoft > Windows > PrintService > Operational** (and **Admin**) | Level: Error | Driver name, document name, error code |
| "The computer is slow" | **Applications and Services > Microsoft > Windows > Diagnostic-Performance > Operational** | Event ID 100, 200 | Boot duration (100), disk performance (100), CPU responsiveness (200) |
| "A service won't start" | **System** | Event ID 7000, 7001, 7023, 7034 | Service name, error code, dependency name |
| "Something broke but I don't know what" | **System** | Level: Critical + Error | Last 24 hours — scan for repeated errors or clusters around a specific time |
| "VPN keeps disconnecting" | **Applications and Services > Microsoft > Windows > RemoteAccess > Operational** | Level: Error | Connection attempt log, reason for disconnection |
| "BitLocker recovery screen" | **Applications and Services > Microsoft > Windows > BitLocker-API > Management** | Event ID 523, 525, 526 | Recovery key used, mount failure reason |
| "I can't map a network drive" | **Applications and Services > Microsoft > Windows > SMBServer > Operational** | Level: Error | SMB session failures, credential errors |
| "Disk usage is 100%" | **Applications and Services > Microsoft > Windows > Diagnostic-Performance > Operational** | Event ID 100 | Disk service time, process name causing the bottleneck |
| "My Outlook keeps asking for password" | **Applications and Services > Microsoft > Windows > ModernAuth > Operational** | Level: Error | Token acquisition failures, auth protocol errors |

---

## Key Event IDs Reference

The most useful Event IDs sorted numerically with plain-English meaning and action.

### System Log

| ID | Summary | Typical cause | Action |
|---|---|---|---|
| **41** | Kernel-Power — system rebooted without clean shutdown | Power loss, BSOD, or hardware fault | Check preceding events (was there a crash?), inspect reliability history |
| **6008** | Previous shutdown was unexpected | Same as 41 but older-style event | Often appears alongside 41; confirms unclean shutdown |
| **7000** | Service failed to start with specific error | Service dependency missing, corrupt binary, misconfiguration | Note the service name, check dependencies, try manual start |
| **7001** | Service dependency failed — service cannot start | Required service isn't running | Note the dependency name, start it first |
| **7009** | Service did not respond to start request (timeout) | Service hung during start attempt | Increase service timeout via registry, or reinstall the service |
| **7023** | Service terminated with error | Service crashed on start | Look for a more specific error code — reinstall or check the service's own config |
| **7026** | Driver failed to load | Corrupt driver or incompatible driver | Boot Safe Mode, uninstall the failing driver |
| **7030** | Service marked as interactive — not allowed | Misconfigured service | Usually harmless, but can be exploited — escalate if seen in a security context |
| **7031** | Service terminated unexpectedly (with restart count) | Service crashed while running | Check its own logs, Event ID 1000 in Application log for the same process |
| **7034** | Service terminated unexpectedly (no detail) | Service exited without reason | Same as 7031 but less information available |
| **7045** | A new service was installed (audit event) | Software installation / malware | Verify with the user — unknown services should be investigated |

### Application Log

| ID | Summary | Typical cause | Action |
|---|---|---|---|
| **1000** | Application Error — faulting module / exception code | App bug, corrupt install, or environmental issue | Note faulting module (e.g., `ntdll.dll` = often corruption; `nvlddmkm.sys` = GPU driver). Search the code |
| **1001** | Windows Error Reporting — crash dump generated | BSOD or application crash | Contains bucket ID and crash details — useful for searching Microsoft's database |
| **1026** | .NET Runtime error | .NET application exception | Note the exception type and stack trace; check if a specific .NET version is needed |
| **1034** | Application Hang — program stopped responding | App frozen / not processing messages | Note hung module name, check for resource exhaustion, update or reinstall the app |
| **11708** | MSI installer error | Software installation/uninstall failure | Contains the installer error code — cross-reference with error codes cheatsheet |

### Security Log

| ID | Summary | Typical cause | Action |
|---|---|---|---|
| **4624** | Successful logon (baseline event) | Normal user logon | Correlate with 4625 for failed-then-succeeded patterns (user tried wrong password then got it right) |
| **4625** | Failed logon attempt | Wrong password, expired password, disabled account | Check Sub Status code: `0xC000006D` = bad password, `0xC000006E` = unknown user, `0xC0000072` = account disabled, `0xC0000071` = expired password |
| **4634** + **4647** | Logoff initiated | Normal user logoff | Useful for session duration tracking |
| **4648** | Logon with explicit credentials — "Run as" used | User or process ran something as a different user | Useful for detecting lateral movement (security context) |
| **4672** | Special privileges assigned to new logon | Admin logon (includes all admin rights) | Expected for administrators — concerning if a non-admin user triggers this |
| **4720** | User account created | New employee, or potentially unauthorized | Verify against HR/IT onboarding records |
| **4724** | Password reset attempt recorded | Password was reset by an administrator | Confirm the admin was authorized — tracks who reset whose password |
| **4740** | Account was locked out | Too many bad password attempts | Note the Caller Computer Name — that's the source of the lockout attempts |
| **4768** | Kerberos TGT requested | Domain authentication event | Contains client IP and account; used for troubleshooting domain auth failures |
| **4769** | Kerberos service ticket requested | Accessing a domain resource | Contains service name and target — useful for "can't access share" issues |
| **4776** | Credential validation (domain controller) | Domain logon attempt (failed or succeeded) | Contains the workstation name and error code — more detail than 4625 in some cases |

### Diagnostic-Performance Log (System Slowdowns)

| ID | Summary | Typical cause | Action |
|---|---|---|---|
| **100** | Disk service time exceeded threshold | Process is hammering the disk — 100% usage in Task Manager | Note the process name; if it's `System`, check for driver issues; if it's an app, update or replace it |
| **101** | Boot total time too long | Too many startup processes, slow driver, fast boot issues | Compare boot time over several boots; check startup impact in Task Manager > Startup |
| **200** | CPU service time exceeded threshold | Process is using excessive CPU time | Similar to 100 — note the process, check for resource leaks |
| **301** | Application responsiveness degraded | An app took > 2 seconds to process a message | Find the process name — often a sign of low-memory condition |

### Windows Update Log (under Applications and Services)

| ID | Summary | Typical cause | Action |
|---|---|---|---|
| **20** | Installation failure | Update install failed | Contains the HResult code — cross-reference with error codes cheatsheet |
| **25** | Installation successful | Update applied | Confirmation — no action needed |
| **43** | Update downloaded | Update ready to install | Informational — no action needed |
| **44** | Update installed (requires reboot) | Update waiting for restart | Inform the user: "Your machine needs to restart to finish applying updates" |

### TerminalServices (RDP) Log (under Applications and Services)

| ID | Summary | Typical cause | Action |
|---|---|---|---|
| **21** | Remote Desktop session logon success | User connected via RDP | Audit — confirm authorized |
| **22** | Remote Desktop shell started | Session fully established | Informational |
| **23** | Remote Desktop session logoff | User disconnected | Informational |
| **24** | Remote Desktop session disconnected (not clean) | Network issue or timeout | Check network stability; correlate with VPN disconnects |
| **25** | Remote Desktop session reconnection | User reconnected | Informational |
| **1149** | RDP connection attempt (with IP) | User-initiated RDP | Correlate with 4624/4625 for authentication status |

---

## Custom Views Worth Creating

Save time by pre-building these views. In Event Viewer: right-click **Custom Views** → **Create Custom View**.

### "Crashes & Failures — Last Hour"
- **Log:** Application, System
- **Level:** Critical + Error
- **Time:** Last hour
- **Use:** Live troubleshooting — check this when a user says "it broke just now"

### "BSOD History"
- **Log:** System
- **Event IDs:** 41, 1001
- **Time:** Last 30 days
- **Use:** See all unexpected shutdowns and memory.dmp reports in one place

### "Service Failures"
- **Log:** System
- **Event IDs:** 7000, 7001, 7023, 7031, 7034
- **Time:** Last 7 days
- **Use:** Find services that are failing repeatedly

### "Failed Logons (Security)"
- **Log:** Security
- **Event IDs:** 4625
- **Time:** Last 24 hours
- **Use:** Identify brute force attempts or users stuck in a login loop

### "Print Troubleshooting"
- **Log:** Applications and Services > PrintService > Operational
- **Level:** Error
- **Time:** Last 24 hours
- **Use:** Isolate print failures without digging through spooler logs

### "Application Crashes"
- **Log:** Application
- **Event IDs:** 1000, 1026
- **Time:** Last 7 days
- **Use:** Trend analysis — is a specific app crashing more often after an update?

---

## Filtering & Power Tips

### Filter by Event ID
Right-click the log → **Filter Current Log** → XML tab:
```xml
<Select Path="System">
*[System[EventID=4625]]
</Select>
```
Multiple IDs on one line:
```xml
<Select Path="Security">
*[System[(EventID=4624 or EventID=4625)]]
</Select>
```

### Filter by time
Use the **Logged** dropdown instead of scrolling. Select a narrow window first (last hour), then widen if you don't find enough.

### Copy events quickly
Select an event → Ctrl+C copies the full detail to clipboard. Paste into your ticket for reference.

### Export custom views
After creating a useful view: right-click it → **Export Custom View** → save as `.xml`. Share with teammates or keep as a backup.

### PowerShell equivalents
```powershell
# Last 10 failed logons
Get-WinEvent -LogName Security -FilterXPath "*[System[EventID=4625]]" -MaxEvents 10

# Application crashes in the last 24 hours
Get-WinEvent -LogName Application -FilterXPath "*[System[(EventID=1000 or EventID=1026)]]" |
  Where-Object { $_.TimeCreated -gt (Get-Date).AddDays(-1) }

# All critical+error from System log (last 20)
Get-WinEvent -LogName System -FilterXPath "*[System[(Level=1 or Level=2)]]" -MaxEvents 20

# Specific service failure
Get-WinEvent -LogName System -FilterXPath "*[System[EventID=7000]]"
```

### Reliability Monitor (when Event Viewer is too much)
Run `perfmon /rel`. This opens Reliability Monitor — a timeline of failures grouped by type (application, Windows, hardware, misc). For end-user communication, the plain-English descriptions here are better than raw Event Viewer output. Use this first, Event Viewer for the details.

---

## Decision Flow: When Event Viewer Says Nothing

```
Event Viewer shows no relevant errors for the reported problem?

├── Is the problem still happening?
│     ├── YES → Open Resource Monitor (perfmon /res) while reproducing the issue
│     │         Look for: disk queue length, network drops, hard faults
│     ├── NO (intermittent) → Enable verbose logging for the component
│     │    Examples: Enable NETSH trace for network, turn on .NET assembly binding logging
│     └── NO (can't reproduce) → Close the ticket with "unable to reproduce" + note
│
├── Are you looking in the right log?
│     └── Check the Log-to-Problem matrix above
│
└── Is the problem caused by a specific application?
      └── Check the application's own logs instead:
           C:\ProgramData\ vendor folders, %APPDATA% logs, application event sinks
```
