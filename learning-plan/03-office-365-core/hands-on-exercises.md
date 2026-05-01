# Hands-On Exercises: Office 365 Core

Requires a Windows 11 VM with Office installed. A free M365 Developer tenant is recommended for full functionality.

---

## Exercise 1: Outlook Profile Management

**Objective:** Learn to create, manage, and repair Outlook profiles.

1. Open Control Panel > Mail > Show Profiles
2. Note the existing profile(s)
3. Create a new profile:
   - Click "Add"
   - Name it "Test Profile"
   - If you have an M365 account, enter email and let AutoDiscover configure it
   - If not, just complete the wizard to see the process
4. Set "Always use this profile" to prompt for profile at startup
5. Open Outlook. Note the profile selection dialog.
6. Close Outlook. Go back to Mail > Show Profiles.
7. Remove the test profile. Note: this also deletes the associated OST file.
8. Reset "Always use this profile" to the default.

---

## Exercise 2: AutoDiscover Investigation

**Objective:** Understand how Outlook discovers the Exchange server.

1. Open Outlook (if not already open)
2. Hold Ctrl > right-click the Outlook icon in the system tray
3. Click "Test Email AutoConfiguration"
4. Enter your email address (use a real M365 account if available)
5. Check "Use AutoDiscover"
6. Click "Test"
7. Examine the three tabs:
   - **Results**: Summary of what AutoDiscover found
   - **Log**: Step-by-step log of the AutoDiscover process. Look for errors or redirects.
   - **XML**: Raw XML response from the server
8. If you don't have a real account, observe what happens with a fake address and note the error messages.

---

## Exercise 3: OST File Rebuild

**Objective:** Practice rebuilding a corrupted OST file.

1. Open Outlook and note it works normally
2. Close Outlook completely (check Task Manager for lingering Outlook.exe processes)
3. Navigate to `%localappdata%\Microsoft\Outlook\`
4. Find the `.ost` file (note its size)
5. Rename it to `<filename>.ost.old`
6. Open Outlook. Observe:
   - Outlook recreates the OST file
   - It begins downloading mailbox data from the server
   - This may take a while for large mailboxes
7. Check the new OST file size — it will grow as data downloads.
8. Close Outlook. Delete the `.ost.old` file (or keep it for comparison).

---

## Exercise 4: OneDrive Files On-Demand

**Objective:** Understand and test Files On-Demand behavior.

1. Ensure OneDrive is running and synced (check system tray icon)
2. Navigate to your OneDrive folder in File Explorer
3. Observe the status icons on files:
   - Blue cloud = online-only
   - Green checkmark = locally available
   - Solid green = always keep on device
4. Right-click a file with a blue cloud icon > "Always keep on this device"
   - Note the icon changes to solid green
   - Note the file size on disk increases
5. Right-click the same file > "Free up space"
   - Note the icon changes back to blue cloud
   - Note the file size on disk decreases to near-zero
6. Double-click a blue-cloud file. It downloads on-demand and becomes locally available.
7. In OneDrive settings (gear icon > Settings), explore:
   - Sync and backup tab > Manage backup (Known Folder Move settings)
   - Advanced settings > Files On-Demand toggle
   - About tab > version and build info

---

## Exercise 5: OneDrive Known Folder Move

**Objective:** Understand how KFM affects user folders.

1. Check if KFM is active:
   - Open File Explorer
   - Right-click Desktop > Properties > Location tab
   - Note if the path points to OneDrive
   - Do the same for Documents and Pictures
2. If KFM is active, note that:
   - Files saved to Desktop are actually in `OneDrive\Desktop`
   - Files sync automatically to the cloud
   - Other devices with OneDrive can see these files
3. If KFM is not active, understand what would happen if it were enabled:
   - Existing Desktop files would be migrated to OneDrive
   - The original Desktop folder becomes a junction to OneDrive
   - User experience is seamless, but files now live in cloud first
4. Discuss with your team: Is KFM enabled in your environment? Why or why not?

---

## Exercise 6: OneDrive Reset

**Objective:** Practice resetting the OneDrive sync client.

1. Make sure OneDrive is running and syncing
2. Open Command Prompt (not admin needed)
3. Run: `onedrive.exe /reset`
4. Observe:
   - OneDrive icon disappears from system tray
   - After ~30 seconds, it should reappear
   - If it doesn't, manually start: `%localappdata%\Microsoft\OneDrive\OneDrive.exe`
5. After restart, verify sync resumes normally.

---

## Exercise 7: Teams Cache Clearing

**Objective:** Practice clearing Teams cache for troubleshooting.

**For Classic Teams:**
1. Quit Teams completely (right-click system tray icon > Quit)
2. Verify Teams is not running in Task Manager
3. Navigate to `%appdata%\Microsoft\Teams\`
4. Delete all contents of this folder (or move to a backup location)
5. Restart Teams
6. Note: You'll need to sign in again, and settings will reset to defaults

**For New Teams:**
1. Settings > Apps > Installed apps > Microsoft Teams > Advanced options
2. Scroll down and click "Reset"
3. This clears app data while keeping the installation

---

## Exercise 8: Office Safe Mode Diagnosis

**Objective:** Use safe mode to diagnose Office issues.

1. Close all Office applications
2. Open Word in safe mode:
   - `Win + R` > type `winword.exe /safe` > Enter
   - Or hold Ctrl while launching Word, click Yes to safe mode prompt
3. Note the title bar says "[Safe Mode]"
4. In safe mode, check:
   - File > Options > Add-ins — note all add-ins are disabled
   - The ribbon may look different (customizations disabled)
5. Create a new document and type some text. Save it.
6. Close Word.
7. Open Word normally. Does it work?
   - If yes in normal mode too, the issue was transient
   - If no in normal mode but yes in safe mode, an add-in or customization is the problem
8. To identify the problematic add-in:
   - File > Options > Add-ins > COM Add-ins > Go
   - Uncheck all add-ins > OK > restart Word
   - If Word works, re-enable add-ins one by one until the issue returns

---

## Exercise 9: Office Repair

**Objective:** Practice repairing an Office installation.

1. Open Settings > Apps > Installed apps
2. Search for "Microsoft 365" or "Office"
3. Click the three dots (⋯) > Modify
4. UAC prompt appears — accept
5. You're presented with two options:
   - **Quick Repair**: Choose this first. It takes ~5 minutes and fixes most issues.
   - **Online Repair**: Choose if Quick Repair fails. Takes ~30 minutes, re-downloads Office.
6. Run Quick Repair and observe the process.
7. After completion, open an Office app and verify it works.

---

## Exercise 10: Activation Troubleshooting

**Objective:** Diagnose and fix Office activation issues.

1. Open any Office app (Word or Excel)
2. Go to File > Account
3. Under "Product Information", note:
   - Product name (e.g., "Microsoft 365 Apps for enterprise")
   - License status
   - Signed-in user
4. If licensed, note the subscription details.
5. If unlicensed or activation required:
   - Click "Sign in" or "Activate Product"
   - Enter credentials
   - Follow the activation flow
6. If activation fails:
   - Check that the user has a valid M365 license in the admin portal
   - Check that no conflicting licenses are assigned
   - Check internet connectivity
   - Try signing out of all Office apps and signing back in
