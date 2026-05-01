# Official Resources: New Machine Provisioning

## Microsoft Learn Paths

### 1. Deploy Windows using Windows Autopilot
- **URL:** https://learn.microsoft.com/en-us/training/modules/deploy-windows-using-autopilot/
- **Topics covered:** Autopilot concepts, deployment types, user-driven vs. self-deploying mode, prerequisites
- **Relevance:** Very High — Autopilot is the modern standard for new machine deployment
- **Surprise factor for Linux users (1-5):** 5 — Zero-touch provisioning via hardware hash registration is a completely new concept
- **Estimated time:** ~40 minutes

### 2. Enroll devices in Microsoft Intune
- **URL:** https://learn.microsoft.com/en-us/training/modules/enroll-devices-in-intune/
- **Topics covered:** Enrollment methods, automatic enrollment, GPO-based enrollment, troubleshooting enrollment
- **Relevance:** Very High — enrollment is the foundation of device management
- **Estimated time:** ~35 minutes

### 3. Deploy and manage apps with Intune
- **URL:** https://learn.microsoft.com/en-us/training/modules/deploy-manage-apps-intune/
- **Topics covered:** App types, assignment, installation behavior, Win32 app packaging, troubleshooting
- **Relevance:** High — essential for understanding why apps do/don't appear on new machines
- **Estimated time:** ~45 minutes

---

## Official Documentation

### 4. Windows Autopilot documentation
- **URL:** https://learn.microsoft.com/en-us/mem/autopilot/windows-autopilot
- **Topics covered:** Complete Autopilot documentation, deployment guides, troubleshooting
- **Relevance:** Very High — primary reference
- **Estimated time:** Bookmark and navigate as needed

### 5. Autopilot troubleshooting guide
- **URL:** https://learn.microsoft.com/en-us/mem/autopilot/troubleshooting
- **Topics covered:** Common Autopilot errors, enrollment failures, diagnostic collection
- **Relevance:** High — keep bookmarked for provisioning tickets
- **Estimated time:** ~25 minutes read

### 6. Hybrid Entra ID join
- **URL:** https://learn.microsoft.com/en-us/entra/identity/devices/how-to-hybrid-join
- **Topics covered:** Prerequisites, configuration, troubleshooting, AD Connect requirements
- **Relevance:** Medium-High — common in transitional environments
- **Estimated time:** ~25 minutes read

### 7. Intune app deployment troubleshooting
- **URL:** https://learn.microsoft.com/en-us/mem/intune/apps/troubleshoot-app-deployment
- **Topics covered:** App installation failures, detection method issues, assignment problems, log locations
- **Relevance:** Very High — practical troubleshooting guide
- **Estimated time:** ~30 minutes read

### 8. Group Policy troubleshooting
- **URL:** https://learn.microsoft.com/en-us/troubleshoot/windows-client/group-policy/gpo-troubleshooting
- **Topics covered:** GPO not applying, slow processing, filtering issues, WMI filter problems
- **Relevance:** High — essential for policy-related issues
- **Estimated time:** ~25 minutes read

### 9. BitLocker deployment and management
- **URL:** https://learn.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview
- **Topics covered:** BitLocker architecture, TPM integration, recovery key escrow, management via Intune/GPO
- **Relevance:** High — BitLocker recovery calls are common for new machines
- **Estimated time:** ~25 minutes read

### 10. Windows user profile troubleshooting
- **URL:** https://learn.microsoft.com/en-us/troubleshoot/windows-client/shell-experience/temporary-profile-log-on
- **Topics covered:** Temporary profile causes, resolution steps, profile recreation
- **Relevance:** High — temp profile issues are disruptive for users
- **Estimated time:** ~20 minutes read

---

## Additional Official Resources

### 11. Intune device management overview
- **URL:** https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-device-management
- **Topics covered:** What Intune manages, compliance policies, configuration profiles, remote actions
- **Relevance:** High — foundational Intune knowledge
- **Estimated time:** ~15 minutes read

### 12. Windows provisioning package reference
- **URL:** https://learn.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-packages
- **Topics covered:** PPKG files, Windows Configuration Designer, offline provisioning
- **Relevance:** Medium — used in some deployment scenarios
- **Estimated time:** ~15 minutes read
