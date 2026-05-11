# Feature Update Fails

A major Windows version upgrade (e.g., 22H2 → 23H2, Windows 10 → Windows 11) fails to install.

```
User reports: "I tried to update to the new version of Windows but it failed"

├─ CHECK REQUIREMENTS
│   ├─ **Windows 11 system requirements** (if upgrading from Windows 10):
│   │   → CPU: 1 GHz, 2+ cores, compatible 64-bit processor (Intel 8th gen / AMD Zen 2+)
│   │   → RAM: 4 GB minimum (8 GB recommended)
│   │   → Storage: 64 GB minimum
│   │   → TPM: TPM 2.0 enabled in BIOS
│   │   → Secure Boot: Enabled in BIOS
│   │   → Run: `PC Health Check` app → It will tell you what's missing
│   │
│   ├─ Check for TPM 2.0:
│   │   → `tpm.msc` → Status: "The TPM is ready for use"
│   │   → If TPM missing → Reboot → BIOS → Enable TPM / Intel PTT / AMD fTPM
│   │
│   └─ Check Secure Boot:
│   │   → `msinfo32` → System Summary → Secure Boot State → ON
│   │   → If OFF → Reboot → BIOS → Enable Secure Boot (UEFI, not Legacy/CSM)
│   └─
│
├─ PREPARE FOR UPGRADE
│   ├─ **Free up disk space** (feature updates need 20-32 GB):
│   │   → [see 06-05](06-05_performance_low-disk-space.md)
│   │
│   ├─ **Disconnect non-essential USB devices**
│   │   → External drives, USB hubs, card readers — can interfere
│   │
│   ├─ **Disable third-party antivirus** temporarily
│   │
│   ├─ **Update all drivers** (especially storage and network)
│   │
│   └─ **Run Windows Update** — install ALL pending cumulative updates first
│       → Feature updates often require the latest cumulative update
│
├─ COMMON ERROR CODES
│   ├─ `0xC1900101 - 0x20017` → Driver failure
│   │   → Generic driver incompatibility → Update all drivers → retry
│   │   → Disconnect peripherals → retry
│   │
│   ├─ `0x80070070 - 0x50011` / `0x80070070 - 0x50012` → Not enough free space
│   │   → Free up space → [see 06-05]
│   │
│   ├─ `0x800F0923` → Driver incompatible with the new version
│   │   → Update all drivers → If persists, the driver manufacturer needs to update
│   │   → Search the KB article associated with this error
│   │
│   ├─ `0x80200056` → Update was interrupted
│   │   → Machine may have lost power or was restarted during install
│   │   → Ensure stable power → retry
│   │
│   └─ `0x80070522` → Client not running as admin → Run as Administrator
│
├─ CLEAN BOOT APPROACH
│   └─ Perform the upgrade with minimal interference:
│   1. Disable non-Microsoft services: `msconfig` → Services → Hide all MS → Disable all
│   2. Disable startup programs: Task Manager → Startup → Disable all
│   3. Reboot
│   4. Run the update again
│   5. After success → Re-enable services/apps
│
├─ SETUP DIAGNOSTIC
│   └─ If the update fails during the "Copying files" or "Installing features" phase:
│       → Check setup logs: `C:\Windows\Panther\setuperr.log`
│       → Check: `C:\Windows\Logs\MoSetup\*`
│       → The last few lines of these logs usually show the failing operation
│
├─ WINDOWS 11 SPECIFIC
│   └─ If requirements aren't met but the machine needs the update anyway:
│       → There are registry bypasses (not recommended — no future updates)
│       → Better: Replace the hardware or use the current version
│
└─ ALL STEPS FAILED?
    └─ Use the Windows Installation Assistant:
        → https://www.microsoft.com/software-download/windows11
        → Download → Run → "Upgrade this PC now"
        → This installs the full version, bypassing Windows Update client
        → If this also fails → Use Media Creation Tool → ISO → Run setup.exe
        → Last resort: Escalate to Tier 2 for repair install
```

**RESULT** → Feature update installed successfully, or incompatibility documented.
