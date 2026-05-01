# Reference Material: Shadow & Scenario Practice

## Shadowing Checklist

Print this and bring it with you when shadowing.

### Before Shadowing
- [ ] Have your VM set up and ready for note-taking
- [ ] Review the 15 common issues and their solutions
- [ ] Prepare a notebook or digital document for notes
- [ ] Set expectations with the person you're shadowing

### During Shadowing — Track Each Ticket
For each ticket observed, note:
- **Issue category**: Which of the 15 common issues does it fall under?
- **First question asked**: What did they ask first?
- **Diagnostic steps**: What did they check, in what order?
- **Tools used**: What tools or commands did they run?
- **Root cause**: What was actually wrong?
- **Resolution**: How was it fixed?
- **Time to resolve**: How long did it take?
- **User communication**: How did they explain things to the user?
- **Escalation?**: Was it escalated? To whom? Why?

### After Shadowing — Debrief Questions
- What was the most common issue you saw today?
- What surprised you the most?
- What do you wish someone had told you before you started?
- Which tools do you use most often?
- What are the top 3 things that take the most time?
- When do you escalate? What's your threshold?
- What knowledge gaps do new supporters usually have?
- What's the one thing I should learn first?

## Scenario Practice Scenarios

Use these to practice on your VM. Time yourself. Document your process.

### Scenario 1: Login Failure (Difficulty: Easy)
**Setup:** Lock the user account by entering wrong passwords 5+ times (if local policy allows), or simulate by renaming the user's Credential Manager entry for a network share.
**Expected steps:** Verify lockout, unlock account, check for source of repeated failures, test login.

### Scenario 2: Slow Computer (Difficulty: Easy)
**Setup:** Create multiple startup items, run a background process consuming CPU, fill the disk to 95%.
**Expected steps:** Open Task Manager, identify bottleneck, take corrective action, recommend restart.

### Scenario 3: Outlook Not Connecting (Difficulty: Medium)
**Setup:** Remove Outlook's saved credentials from Credential Manager, or change the mailbox server to an invalid one.
**Expected steps:** Check connection status, check credentials, test AutoDiscover, rebuild profile if needed.

### Scenario 4: OneDrive Not Syncing (Difficulty: Medium)
**Setup:** Create a file with an invalid name in OneDrive (e.g., `file:with:colons.txt`), or run `onedrive.exe /reset` and don't restart it.
**Expected steps:** Check sync status, identify the error, fix the problematic file, reset OneDrive if needed.

### Scenario 5: Missing Application on New PC (Difficulty: Medium)
**Setup:** Simulate by uninstalling a standard app from the VM.
**Expected steps:** Verify against standard software list, check deployment status, reinstall via appropriate method.

### Scenario 6: Windows Update Stuck (Difficulty: Medium)
**Setup:** Stop the Windows Update service, or corrupt the SoftwareDistribution folder.
**Expected steps:** Check update status, run troubleshooter, clear SoftwareDistribution, restart services.

### Scenario 7: Printer Not Working (Difficulty: Easy)
**Setup:** Stop the Print Spooler service.
**Expected steps:** Check printer status, restart spooler, clear stuck jobs, test print.

### Scenario 8: Network Connectivity Issue (Difficulty: Medium)
**Setup:** Set an invalid DNS server, or disable the network adapter.
**Expected steps:** Check connection status, run diagnostic commands, fix configuration.

### Scenario 9: Application Crash (Difficulty: Hard)
**Setup:** Install an incompatible application, or corrupt its configuration.
**Expected steps:** Check Event Viewer for crash details, check compatibility, repair/reinstall, check for updates.

### Scenario 10: Blue Screen After Update (Difficulty: Hard)
**Setup:** Use the CrashOnCtrlScroll registry key to trigger a BSOD (in a VM only).
**Expected steps:** Boot to Safe Mode, check Event Viewer for bugcheck, analyze minidump, rollback driver/update.

### Scenario 11: File Access Denied (Difficulty: Medium)
**Setup:** Remove user's permissions from a folder, change ownership to SYSTEM.
**Expected steps:** Check permissions, take ownership, restore appropriate permissions.

### Scenario 12: Office Activation Error (Difficulty: Medium)
**Setup:** Sign out of Office, or remove the license.
**Expected steps:** Check license in admin portal, sign back in, run activation troubleshooter, repair Office.

### Scenario 13: Teams Audio/Video Not Working (Difficulty: Medium)
**Setup:** Disable camera/microphone in Windows Privacy settings.
**Expected steps:** Check Teams device settings, check Windows privacy settings, test with another app.

### Scenario 14: BitLocker Recovery (Difficulty: Easy)
**Setup:** Trigger BitLocker recovery mode (in VM, if BitLocker is enabled).
**Expected steps:** Get Recovery Key ID from user, look up key in Entra ID/AD, provide to user, verify boot.

### Scenario 15: User Profile Issues — Temporary Profile (Difficulty: Hard)
**Setup:** Corrupt the NTUSER.DAT file or deny access to the profile folder.
**Expected steps:** Identify temp profile (Event ID 1511), delete corrupted profile, have user log in fresh, recover data.

## Scenario Scoring Rubric

Score yourself on each scenario:

| Criteria | 1 (Poor) | 2 (Fair) | 3 (Good) | 4 (Excellent) |
|---|---|---|---|---|
| **Diagnosis speed** | >30 min | 15-30 min | 5-15 min | <5 min |
| **Correct steps** | Wrong approach | Missed steps | Good approach, minor gaps | Efficient, complete |
| **Root cause identified** | Did not find | Found after hints | Found independently | Found immediately |
| **Resolution** | Did not resolve | Workaround only | Proper fix applied | Proper fix + prevention |
| **Communication** | Technical jargon | Some jargon | Clear, professional | Clear, empathetic |
| **Documentation** | No notes | Minimal notes | Adequate notes | Complete, audit-ready |

Target: Average score of 3+ across all 15 scenarios before going live.
