# Official Resources: Common Windows 11 Issues

## Microsoft Learn Paths

### 1. Troubleshoot Windows client startup
- **URL:** https://learn.microsoft.com/en-us/training/modules/troubleshoot-windows-startup/
- **Topics covered:** Boot process, boot configuration data, startup repair, safe mode, recovery options
- **Relevance:** High — BSOD and boot failures are critical issues
- **Surprise factor for Linux users (1-5):** 4 — BCD (Boot Configuration Data) is very different from GRUB, and the recovery environment is Windows-specific
- **Estimated time:** ~45 minutes

### 2. Troubleshoot the Windows client operating system and apps
- **URL:** https://learn.microsoft.com/en-us/training/paths/troubleshoot-windows-client-operating-system-apps/
- **Topics covered:** Application compatibility, app deployment troubleshooting, startup architecture, service troubleshooting
- **Relevance:** Medium — app compatibility issues come up regularly
- **Surprise factor for Linux users (1-5):** 3 — The compatibility shim system and startup architecture have no Linux equivalent
- **Estimated time:** ~1 hour 30 minutes

### 3. Update Windows client
- **URL:** https://learn.microsoft.com/en-us/training/modules/update-windows-client/
- **Topics covered:** Windows Update configuration, servicing channels, WSUS basics, Group Policy settings for updates
- **Relevance:** Very High — Windows Update issues are among the most common tickets
- **Surprise factor for Linux users (1-5):** 4 — Update channels, feature updates vs. quality updates, and servicing options are all different from apt/yum
- **Estimated time:** ~35 minutes

### 4. Configure networking on Windows clients
- **URL:** https://learn.microsoft.com/en-us/training/paths/configure-networking-windows-clients/
- **Topics covered:** IPv4/IPv6 configuration, DNS, wireless networking, remote access, remote management
- **Relevance:** Very High — network issues are daily occurrences
- **Surprise factor for Linux users (1-5):** 2 — The networking concepts are the same, only the commands differ slightly
- **Estimated time:** ~1 hour 30 minutes

---

## Official Documentation

### 5. Windows Update troubleshooting guide
- **URL:** https://learn.microsoft.com/en-us/windows/deployment/update/windows-update-troubleshooting
- **Topics covered:** Common update errors, fix-it steps, log analysis, reset procedures
- **Relevance:** Very High — keep this bookmarked for update tickets
- **Estimated time:** ~30 minutes read (reference)

### 6. Fix Windows Update errors
- **URL:** https://learn.microsoft.com/en-us/troubleshoot/windows-client/windows-update/fix-windows-update-errors
- **Topics covered:** Specific error codes (0x80070xxx series) and their fixes
- **Relevance:** Very High — error-code-specific troubleshooting
- **Estimated time:** Bookmark for reference

### 7. BSOD troubleshooting guide
- **URL:** https://learn.microsoft.com/en-us/troubleshoot/windows-client/blue-screen/help-stop-blue-screen-screen-errors
- **Topics covered:** Common BSOD causes, safe mode, system restore, driver rollback, memory diagnostics
- **Relevance:** High — BSOD tickets are high-priority
- **Estimated time:** ~20 minutes read

### 8. Analyze minidump files
- **URL:** https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/analyzing-a-minidump-file
- **Topics covered:** Using WinDbg to analyze crash dumps, interpreting results
- **Relevance:** Medium — useful for recurring BSODs
- **Surprise factor for Linux users (1-5):** 4 — WinDbg is a powerful but complex tool; no direct Linux equivalent
- **Estimated time:** ~30 minutes read

### 9. NTFS permissions documentation
- **URL:** https://learn.microsoft.com/en-us/windows/win32/fileio/ntfs-security-overview
- **Topics covered:** ACLs, ACEs, permissions model, inheritance, effective permissions
- **Relevance:** Very High — permission issues are daily tickets
- **Surprise factor for Linux users (1-5):** 4 — The ACL model with inheritance, deny rules, and effective permissions is significantly different from POSIX
- **Estimated time:** ~30 minutes read

### 10. Manage local user and group accounts
- **URL:** https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-localusersmanagergroups
- **Topics covered:** Local account management, Group Policy settings for accounts, password policies
- **Relevance:** Medium — useful for understanding account management
- **Estimated time:** ~20 minutes read
- **Notes:** This URL redirects to the broader MDM policy CSP reference; use it as a starting point

### 11. Windows networking troubleshooting
- **URL:** https://learn.microsoft.com/en-us/troubleshoot/windows-client/networking/networking-troubleshooting-guide
- **Topics covered:** Comprehensive network troubleshooting methodology, tool usage, common scenarios
- **Relevance:** Very High — comprehensive reference
- **Estimated time:** ~45 minutes read

### 12. Print troubleshooting guide
- **URL:** https://learn.microsoft.com/en-us/troubleshoot/windows-client/printing/printing-troubleshooting
- **Topics covered:** Print spooler, driver issues, network printing, common errors
- **Relevance:** High — printer issues are common and frustrating
- **Estimated time:** ~25 minutes read

### 13. Windows Error Reporting
- **URL:** https://learn.microsoft.com/en-us/windows/win32/wer/windows-error-reporting
- **Topics covered:** How Windows collects and reports crash data, where crash reports are stored
- **Relevance:** Medium — useful for understanding crash report locations
- **Estimated time:** ~15 minutes read

---

## Additional Official Resources

### 14. Windows Release Health
- **URL:** https://learn.microsoft.com/en-us/windows/release-health/
- **Topics covered:** Known issues per Windows build, safeguard holds, resolved issues
- **Relevance:** Very High — check this first when users report issues after an update
- **Estimated time:** Bookmark, check weekly

### 15. Microsoft Support and Recovery Assistant (SaRA)
- **URL:** https://aka.ms/SaRA
- **Topics covered:** Automated diagnostic tool for Office and Windows issues
- **Relevance:** High — run this before manual troubleshooting for Office issues
- **Estimated time:** Download and familiarize (~10 minutes)

### 16. Sysinternals Suite
- **URL:** https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite
- **Topics covered:** Advanced troubleshooting tools (Process Explorer, Process Monitor, Autoruns, TCPView, etc.)
- **Relevance:** High — these are the power tools for when basic troubleshooting isn't enough
- **Surprise factor for Linux users (1-5):** 3 — Process Monitor is like strace + ltrace combined, Autoruns is like checking all init scripts
- **Estimated time:** Ongoing — learn tools as needed
- **Key tools to start with:**
  - **Process Explorer**: Better Task Manager (shows process trees, handles, DLLs)
  - **Process Monitor**: Real-time file/registry/network activity monitoring
  - **Autoruns**: Shows everything that auto-starts (much more comprehensive than Task Manager)
  - **TCPView**: Shows all network connections per process
  - **Sigcheck**: Verify file signatures and versions
