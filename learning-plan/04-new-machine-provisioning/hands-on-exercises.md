# Hands-On Exercises: New Machine Provisioning

---

## Exercise 1: Check Enrollment Status

**Objective:** Learn to verify a device's enrollment status.

1. Open Command Prompt
2. Run: `dsregcmd /status`
3. Examine the output:
   - Is the device Entra ID joined? (`AzureAdJoined`)
   - Is it MDM enrolled? (`MDM enrollment`)
   - Is it domain joined? (`DomainJoined`)
   - What is the device ID?
   - What is the user's UPN?
4. Run: `systeminfo | findstr /B /C:"Domain"` to check traditional domain membership
5. Compare the two outputs and understand the difference between domain join and Entra ID join.

---

## Exercise 2: Group Policy Report

**Objective:** Generate and read a Group Policy report.

1. Open Command Prompt as Administrator
2. Run: `gpupdate /force`
3. Wait for the policy refresh to complete
4. Run: `gpresult /h %USERPROFILE%\Desktop\gp-report.html`
5. Open the HTML file in a browser
6. Examine:
   - Which GPOs are applied (and which are denied)
   - Computer Configuration settings
   - User Configuration settings
   - Security group membership that affects policy
7. Run: `gpresult /r` for a quick text summary

---

## Exercise 3: Intune Management Extension Logs

**Objective:** Learn to read Intune deployment logs.

1. Navigate to `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs`
2. Open `IntuneManagementExtension.log` (you may need admin rights)
3. Use CMTrace (if available) or any text editor
4. Search for "App" or "Policy" to find deployment events
5. Look for any errors (lines containing "error" or "fail")
6. Note the timestamps and what was being processed

---

## Exercise 4: BitLocker Status Check

**Objective:** Check BitLocker encryption status.

1. Open Command Prompt as Administrator
2. Run: `manage-bde -status`
3. Examine:
   - Conversion Status: Fully Encrypted / Encrypting / Not Encrypted
   - Percentage Encrypted
   - Protection Status: Protection On / Off
   - Key Protectors: TPM, PIN, Recovery Key
4. Run: `manage-bde -protectors -get C:` to see the key protectors
5. Note the Recovery Password ID (first 8 chars) — this is what you'd use to look up the key

---

## Exercise 5: Time Synchronization Check

**Objective:** Verify time sync (critical for domain authentication).

1. Run: `w32tm /query /status`
2. Note:
   - Source (where time is coming from)
   - Stratum (distance from authoritative time source)
   - Last successful sync time
   - Poll interval
3. Run: `w32tm /query /source` for just the time source
4. If time is wrong, run: `w32tm /resync` to force a sync

---

## Exercise 6: Simulate a Provisioning Scenario

**Objective:** Walk through the new machine triage checklist.

Perform the full checklist on your VM:
1. [ ] `dsregcmd /status` — enrollment status
2. [ ] `gpresult /h report.html` — policy check
3. [ ] Compare installed apps against a standard list (create a mock list)
4. [ ] `whoami` — verify user context
5. [ ] Check OneDrive status (running? syncing?)
6. [ ] Check Outlook status (configured? connected?)
7. [ ] `manage-bde -status` — BitLocker status
8. [ ] Check Windows Update status
9. [ ] `w32tm /query /status` — time sync
10. [ ] Test network connectivity (`Test-NetConnection`)

Document findings in a mock support ticket format.
