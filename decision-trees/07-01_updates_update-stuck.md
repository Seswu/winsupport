# Windows Update Stuck / Won't Install

Windows Update is stuck at a certain percentage, downloads indefinitely, or fails to install.

```
User reports: "Windows Update is stuck at X%" or "Updates won't install"

├─ CHECK WHAT'S HAPPENING
│   ├─ Open: Settings → Windows Update → View update history
│   │   ├─ Are there any failed updates? → Note the KB number and error code
│   │   └─ Is there a pending restart? → Reboot now
│   │
│   └─ Run the Windows Update Troubleshooter:
│       → Settings → System → Troubleshoot → Other troubleshooters
│       → Windows Update → Run → Let it attempt fixes
│
├─ STOP AND RESET THE UPDATE SERVICE
│   ├─ Run as Administrator:
│   │   ```cmd
│   │   net stop wuauserv
│   │   net stop bits
│   │   net stop dosvc
│   │   ren C:\Windows\SoftwareDistribution SoftwareDistribution.old
│   │   ren C:\Windows\System32\catroot2 catroot2.old
│   │   net start wuauserv
│   │   net start bits
│   │   net start dosvc
│   │   ```
│   │   → Open Windows Update again → "Check for updates"
│   │   → This resets the update cache without losing installed updates
│   │
│   └─ Alternative (PowerShell):
│       `Install-Module -Name PSWindowsUpdate` → `Get-WUInstall` (more control)
│
├─ SYSTEM FILE CORRUPTION
│   ├─ Run `sfc /scannow` → Repair system files
│   └─ Run `DISM /Online /Cleanup-Image /RestoreHealth`
│       → This may take 15-30 minutes → Do NOT interrupt
│
├─ FREE UP SPACE
│   └─ Windows Update needs disk space to download and install
│       → [see 06-05](06-05_performance_low-disk-space.md)
│       → Need at least 10-20 GB free for feature updates
│
├─ COMMON ERROR CODES
│   ├─ `0x80070002` → File missing → Reset SoftwareDistribution (see above)
│   ├─ `0x80070020` → Sharing violation → Disable antivirus → retry
│   ├─ `0x80070422` → Service disabled → `services.msc` → wuauserv + BITS → Automatic
│   ├─ `0x80073712` → Component store corrupt → DISM /RestoreHealth
│   ├─ `0x80240017` → Update not applicable → Check Windows version
│   └─ `0x8024401c` → Timeout → Check internet connection / WSUS access
│
├─ STUCK AT "DOWNLOADING 0%" OR "PENDING DOWNLOAD"
│   ├─ Check disk space temporarily used for downloads:
│   │   → `C:\Windows\SoftwareDistribution\Download` → Clear contents
│   │   → Retry
│   └─ Check if Windows Update is blocked by proxy/firewall:
│       → `netsh winhttp show proxy`
│       → If proxy is set incorrectly → `netsh winhttp reset proxy`
│
├─ STUCK AT "INSTALLING" FOR HOURS
│   ├─ Let it run — some updates take 2-3 hours (especially feature updates)
│   ├─ Check `C:\Windows\Panther` logs for progress
│   └─ If genuinely stuck (> 6 hours same percentage):
│       → Hard reboot → Run troubleshooter → Retry
│
└─ ALL STEPS FAILED?
    └─ Download and install the update manually:
        → https://www.catalog.update.microsoft.com
        → Search for the KB number → Download → Run the .msu
        → This bypasses Windows Update client entirely
        → If manual install also fails → Repair install Windows (escalate)
```

**RESULT** → Windows Update functioning and updates installed.
