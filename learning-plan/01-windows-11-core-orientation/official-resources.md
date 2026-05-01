# Official Resources: Windows 11 Core Orientation

## Microsoft Learn Paths

### 1. Install the Windows client
- **URL:** https://learn.microsoft.com/en-us/training/paths/install-windows-client/
- **Topics covered:** Windows client editions, installation methods, upgrade paths, deployment tools
- **Relevance:** High — gives you the foundational overview of Windows client OS
- **Surprise factor for Linux users (1-5):** 2 — Mostly conceptual, nothing surprising
- **Estimated time:** ~2 hours
- **Notes:** Focus on the "Explore the Windows client" and "Examine Windows client editions and requirements" modules

### 2. Explore support and diagnostic tools
- **URL:** https://learn.microsoft.com/en-us/training/modules/explore-support-diagnostic-tools/
- **Topics covered:** Task Manager, Event Viewer, Resource Monitor, Performance Monitor, Reliability Monitor, Process Explorer, Process Monitor, MMC, Windows Registry
- **Relevance:** Very High — directly maps to your daily support tasks
- **Surprise factor for Linux users (1-5):** 3 — The aggregation of tools and the depth of Performance Monitor will be new
- **Estimated time:** ~45 minutes
- **Notes:** Do this one thoroughly

### 3. Support the Windows client environment
- **URL:** https://learn.microsoft.com/en-us/training/paths/support-windows-client-environment/
- **Topics covered:** Windows architecture, troubleshooting methodologies, diagnostic tools, performance monitoring
- **Relevance:** High — covers the configuration surface you'll need to support
- **Surprise factor for Linux users (1-5):** 3 — Diagnostic tools and performance monitoring differ significantly from Linux equivalents
- **Estimated time:** ~1 hour 30 minutes
- **Notes:** Focus on the diagnostic tools and architecture sections

---

## Official Documentation

### 4. Windows IT Pro Docs — Overview
- **URL:** https://learn.microsoft.com/en-us/windows/windows-11/
- **Topics covered:** Everything about Windows 11 for IT professionals
- **Relevance:** Very High — bookmark this as your primary reference
- **Surprise factor for Linux users (1-5):** N/A (reference, not a course)
- **Notes:** The landing page for Windows 11 IT Pro documentation. Use it to navigate to specific topics.

### 5. Windows File Explorer documentation
- **URL:** https://learn.microsoft.com/en-us/windows/client-management/manage-windows-client-with-file-explorer
- **Topics covered:** File Explorer features, known folders, library management
- **Relevance:** Medium — you'll use this daily but it's intuitive
- **Surprise factor for Linux users (1-5):** 2
- **Estimated time:** ~15 minutes read

### 6. Windows Services documentation
- **URL:** https://learn.microsoft.com/en-us/windows/win32/services/services
- **Topics covered:** Service architecture, service control manager, service configuration
- **Relevance:** High — essential for troubleshooting service failures
- **Surprise factor for Linux users (1-5):** 3 — The recovery options and dependency model differ from systemd
- **Estimated time:** ~30 minutes read
- **Notes:** Focus on the overview and configuration sections, not the API/developer sections

### 7. User Account Control technical reference
- **URL:** https://learn.microsoft.com/en-us/windows/security/identity-protection/user-account-control/user-account-control-overview
- **Topics covered:** UAC architecture, security levels, Group Policy settings, application compatibility
- **Relevance:** Medium-High — you'll encounter UAC issues frequently
- **Surprise factor for Linux users (1-5):** 4 — The dual-token model is fundamentally different from sudo
- **Estimated time:** ~20 minutes read

### 8. Windows environment variables
- **URL:** https://learn.microsoft.com/en-us/windows/win32/procthread/environment-variables
- **Topics covered:** System and user environment variables, how they're stored, how they're inherited
- **Relevance:** Medium — useful for troubleshooting app configuration issues
- **Surprise factor for Linux users (1-5):** 3 — The dual scope (user/system) and registry storage are different
- **Estimated time:** ~15 minutes read

### 9. Event Viewer and Windows Event Log
- **URL:** https://learn.microsoft.com/en-us/windows/win32/eventlog/event-logging
- **Topics covered:** Event log architecture, event channels, event structure, querying events
- **Relevance:** Very High — your primary debugging tool
- **Surprise factor for Linux users (1-5):** 3 — The hierarchical channel system and XML event format differ from journald/syslog
- **Estimated time:** ~25 minutes read

### 10. NTFS Permissions and File System
- **URL:** https://learn.microsoft.com/en-us/windows/win32/fileio/ntfs-overview
- **Topics covered:** NTFS features, permissions model, encryption, compression
- **Relevance:** Very High — you will troubleshoot permission issues regularly
- **Surprise factor for Linux users (1-5):** 4 — ACLs with inheritance, effective permissions tool, and the Owner concept work differently than POSIX permissions
- **Estimated time:** ~30 minutes read

---

## Additional Official Resources

### 11. Windows 11 specifications
- **URL:** https://www.microsoft.com/en-us/windows/windows-11-specifications
- **Topics covered:** Hardware requirements, TPM, Secure Boot, processor requirements
- **Relevance:** Medium — useful for understanding why some machines can't upgrade
- **Estimated time:** ~10 minutes read

### 12. Windows keyboard shortcuts reference
- **URL:** https://learn.microsoft.com/en-us/answers/questions/2337008/windows-11-keyboard-shortcuts-article
- **Topics covered:** Complete list of Windows keyboard shortcuts
- **Relevance:** Medium — good for efficiency and helping users
- **Estimated time:** Bookmark for reference

### 13. PowerShell documentation (landing page)
- **URL:** https://learn.microsoft.com/en-us/powershell/
- **Topics covered:** PowerShell language, cmdlets, scripting, administration
- **Relevance:** High — PowerShell is your primary command-line tool
- **Surprise factor for Linux users (1-5):** 3 — Object-based pipeline is very different from text-based bash pipelines
- **Estimated time:** Ongoing — don't try to learn it all at once
- **Notes:** Start with "PowerShell for Linux admins" searches to find comparison guides
