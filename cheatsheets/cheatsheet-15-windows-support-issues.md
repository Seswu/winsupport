# Common Windows Support Issues

## 1. Password reset / login issues
Users cannot authenticate into their Windows machine or associated accounts. Symptoms include "incorrect password" errors, account lockout messages, MFA prompts not appearing, or the system rejecting credentials that previously worked.

- Reset via self-service password portal or IT admin tool
- Clear cached credentials (`cmdkey /delete` or Credential Manager)
- Disable/re-enable account lockout after too many failed attempts

## 2. Slow performance
The PC responds sluggishly to user input, takes longer than usual to open applications, or feels unresponsive. Symptoms include high disk/CPU usage in Task Manager, long boot times, lag when typing, or applications taking forever to load.

- Restart the PC (clears memory leaks and stuck processes)
- Disable unnecessary startup programs via Task Manager
- Run Disk Cleanup and check for full disk/low RAM

## 3. Software installation
Users are unable to install, update, or launch required applications. Symptoms include installer errors, "access denied" messages during installation, missing dependencies, or the installation wizard crashing mid-process.

- Run installer as Administrator (right-click > Run as administrator)
- Check if existing version needs to be uninstalled first
- Verify antivirus isn't blocking the installer (add exception)

## 4. Printer/connectivity problems
The printer is not recognized, won't print, or produces garbled output. Symptoms include print jobs stuck in the queue, "printer offline" status, missing printer in the devices list, or error lights blinking on the hardware.

- Remove and re-add the printer in Settings > Devices
- Restart the Print Spooler service (`services.msc`)
- Update or reinstall printer drivers from manufacturer site

## 5. Blue Screen of Death (BSOD)
The system encounters a critical error and displays a blue screen with a stop code, forcing a restart. Symptoms include sudden reboots, specific stop codes like `IRQL_NOT_LESS_OR_EQUAL`, crashes occurring after driver updates or new hardware installation.

- Note the stop code and search Microsoft's BSOD database
- Boot into Safe Mode and uninstall recent drivers/updates
- Run `sfc /scannow` and `chkdsk` to repair system files

## 6. Network/Wi-Fi connectivity
The computer cannot connect to the internet or corporate network. Symptoms include "No internet, secured" warnings, Wi-Fi networks not appearing, intermittent disconnections, or Ethernet showing as "unidentified network."

- Run Windows Network Troubleshooter
- Reset network stack (`ipconfig /release`, `/renew`, `netsh winsock reset`)
- Update or reinstall network adapter drivers

## 7. Email/client configuration
Users cannot send/receive emails or their email client is misbehaving. Symptoms include Outlook stuck on "connecting," missing folders, send/receive errors, calendar not syncing, or repeated password prompts.

- Use AutoDiscover or re-enter server settings manually
- Create a new Outlook profile (Control Panel > Mail)
- Repair Office installation via Settings > Apps > Modify

## 8. Windows update failures
Windows updates fail to download, install, or cause problems after installation. Symptoms include updates stuck at a certain percentage, error codes like `0x80070002`, repeated rollback of updates, or the system reporting "some settings are managed by your organization."

- Run Windows Update Troubleshooter
- Clear SoftwareDistribution folder and restart wuauserv service
- Manually download the update from Microsoft Update Catalog

## 9. File access/permissions
Users cannot open, edit, or save files and folders. Symptoms include "access denied" or "you need permission" errors, files appearing greyed out, inability to access network shares, or files locked by another user/process.

- Take ownership of the file/folder via Properties > Security
- Add user/group to the appropriate NTFS permissions
- Check if file is blocked (Properties > Unblock) or encrypted (EFS)

## 10. Application crashes
Programs close unexpectedly or freeze during use. Symptoms include "program has stopped working" popups, applications becoming unresponsive, error messages referencing faulting modules, or crashes occurring at specific actions.

- Check Event Viewer for crash details and faulting module
- Run the application in compatibility mode
- Repair or reinstall the application

## 11. Driver issues
Hardware devices don't function correctly due to missing, outdated, or corrupted drivers. Symptoms include devices showing with a yellow exclamation mark in Device Manager, hardware not detected, devices working intermittently, or errors after a Windows update.

- Update driver via Device Manager or manufacturer website
- Roll back driver to previous version (Device Manager > Roll Back Driver)
- Uninstall device, reboot, and let Windows reinstall automatically

## 12. Virus/malware concerns
Users suspect their machine is infected with malicious software. Symptoms include unexpected pop-ups, browser homepage changes, new toolbars, slow performance, unfamiliar programs in Task Manager, or files being encrypted/renamed.

- Run a full scan with Windows Defender or enterprise antivirus
- Boot into Safe Mode with Networking and run Malwarebytes
- Isolate the machine from the network if infection is confirmed

## 13. Peripheral/device setup
External devices such as monitors, docking stations, webcams, or USB peripherals aren't working. Symptoms include devices not detected when plugged in, display not extending/mirroring, USB ports not recognizing devices, or devices working intermittently.

- Unplug/replug device and try a different USB port
- Install manufacturer-specific drivers or docking station software
- Update firmware on the peripheral or docking station

## 14. OneDrive/cloud sync issues
Files aren't syncing between the local machine and the cloud. Symptoms include OneDrive stuck on "processing changes," sync conflict notifications, files missing from other devices, red X icons on files, or storage quota exceeded warnings.

- Pause and resume sync or sign out/sign back into OneDrive
- Check for file name conflicts or invalid characters
- Free up OneDrive storage or reset OneDrive (`onedrive.exe /reset`)

## 15. License/activation problems
Windows or Office reports that it isn't activated or the license has expired. Symptoms include activation watermarks on the desktop, "Windows isn't activated" banners, Office features greyed out, or error codes related to invalid product keys.

- Run the Activation troubleshooter (Settings > Update & Security)
- Re-enter the product key or check volume licensing portal
- Contact Microsoft support or IT to verify license assignment
