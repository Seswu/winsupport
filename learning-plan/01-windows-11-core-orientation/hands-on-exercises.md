# Hands-On Exercises: Windows 11 Core Orientation

Perform all exercises in a Windows 11 VM. Do not run these on your production machine.

---

## Exercise 1: Navigate Both Settings Interfaces

**Objective:** Understand the split between Settings app and Control Panel.

1. Open Settings (`Win + I`) and browse through each category
2. Open Control Panel (`Win + R` > type `control` > Enter)
3. Find the following settings in **both** interfaces and note where they live:
   - Display resolution
   - Add/remove programs
   - Network adapter properties
   - User accounts
   - Power options
4. Note which settings exist only in one interface or the other

**Expected outcome:** You can confidently navigate both interfaces and know which to use for a given task.

---

## Exercise 2: File Explorer Deep Dive

**Objective:** Master File Explorer and understand the file system.

1. Enable viewing of hidden files and file extensions:
   - Open File Explorer > View > Show > check "Hidden items" and "File name extensions"
2. Navigate to `%APPDATA%`, `%LOCALAPPDATA%`, and `%PROGRAMDATA%`. List the contents of each.
3. Create the following folder structure in your user profile:
   ```
   %USERPROFILE%\test-exercise\
   ├── folder-a\
   │   └── file-a.txt
   ├── folder-b\
   └── file-root.txt
   ```
4. Right-click `file-a.txt` > Properties. Note the attributes tab. Check "Hidden" and observe what happens in Explorer.
5. Right-click `file-root.txt` > Properties > Security tab. Examine the permission entries.
6. Open a Command Prompt and navigate to `C:\`. Try to `dir` inside `C:\Windows\System32\config`. Note the access denied error.
7. In PowerShell, run `Get-ChildItem $env:LOCALAPPDATA -Force | Select-Object Name, Length` and observe the output.

---

## Exercise 3: Task Investigation via Task Manager

**Objective:** Learn to identify and manage processes.

1. Open Task Manager (`Ctrl + Shift + Esc`)
2. Click "More details" if in compact mode
3. On the Processes tab, sort by CPU, then by Memory. Identify the top 3 resource consumers.
4. Right-click any process > "Go to details" and note the PID.
5. Right-click the same process in Details tab > "Open file location". Note where the binary lives.
6. Go to the Startup apps tab. Identify 3 enabled startup items and what they do.
7. Go to the Performance tab. Note your CPU, memory, disk, and network utilization.
8. Find a non-critical background process and "End task" it. Observe what happens (pick something like OneDrive or a browser updater — not a Windows system process).

**Expected outcome:** You can quickly identify resource-hogging processes and understand what processes do.

---

## Exercise 4: Event Viewer Investigation

**Objective:** Find and interpret log entries.

1. Open Event Viewer (`eventvwr.msc`)
2. Navigate to Windows Logs > Application
3. Filter the log to show only "Error" and "Critical" level events from the last 7 days
4. Double-click an error event and examine:
   - Event ID
   - Source
   - Date and time
   - General description
   - Details tab (XML view)
5. Navigate to Windows Logs > System. Find Event ID 7036 (service state changes). Notice how noisy it is.
6. Create a Custom View:
   - Right-click "Custom Views" > Create Custom View
   - Filter for Event ID 1000 (Application Error) and 1002 (Application Hang)
   - Save it as "App Crashes and Hangs"
7. Navigate to Applications and Services Logs > Microsoft > Windows. Expand and find at least 3 product-specific log channels.

---

## Exercise 5: Service Manipulation

**Objective:** Understand and manage Windows services.

1. Open Services (`services.msc`)
2. Find the "Print Spooler" service. Note its status, startup type, and Log On As account.
3. Right-click Print Spooler > Properties. Examine the Dependencies tab. Note which services it depends on and which depend on it.
4. Stop the Print Spooler service. Verify it stopped.
5. Try to add a printer. Observe the error.
6. Start the Print Spooler again.
7. Change the startup type to "Manual" (don't leave it this way — change it back to Automatic afterward).
8. From an elevated Command Prompt, run:
   ```cmd
   sc query spooler
   net stop spooler
   net start spooler
   ```
9. From an elevated PowerShell, run:
   ```powershell
   Get-Service -Name spooler | Format-List *
   Restart-Service -Name spooler
   ```

---

## Exercise 6: UAC and Permission Experimentation

**Objective:** Experience how UAC and permissions work.

1. Create a file on your Desktop: `test-file.txt`
2. Right-click > Properties > Security > Advanced
3. Click "Disable inheritance" > "Convert inherited permissions into explicit permissions"
4. Remove your user account from the permission list. Apply and close.
5. Try to open the file. Note the error.
6. Right-click > Properties > Security > Advanced > Owner > Change > type your username > OK
7. Grant yourself Full Control again.
8. Create a text file called `test-script.bat` on Desktop with content: `echo Hello World`. Try to run it. Note the UAC behavior.
9. Right-click the same file > "Run as administrator". Note the UAC prompt.

---

## Exercise 7: User Context Exploration

**Objective:** Understand your security context.

1. Open Command Prompt (not elevated). Run:
   ```cmd
   whoami /all
   ```
2. Examine the output:
   - What is your SID?
   - What groups are you in?
   - What privileges do you have?
   - What is your Mandatory Label (integrity level)?
3. Open an elevated Command Prompt (right-click > Run as administrator). Run `whoami /all` again.
4. Compare the two outputs. Note the differences in:
   - Mandatory Label (should change from Medium to High)
   - Enabled privileges (more privileges in elevated context)
5. Run `whoami /user` in both contexts. Note the SID is the same — same user, different token.
6. In PowerShell, run:
   ```powershell
   [Security.Principal.WindowsIdentity]::GetCurrent() | Format-List *
   ```

---

## Exercise 8: Environment Variable Investigation

**Objective:** Understand environment variable scope and behavior.

1. Open Command Prompt. Run `echo %PATH%`. Note the output.
2. Open Settings > System > About > Advanced system settings > Environment Variables.
3. In the User variables section, find or create a variable called `MYTEST` with value `hello-from-user`.
4. In the System variables section, find or create a variable called `MYTEST` with value `hello-from-system`.
5. Open a **new** Command Prompt. Run `echo %MYTEST%`. Note which value wins (user scope wins for CMD).
6. Now check PATH: observe how it combines both user and system PATH entries.
7. In CMD, run `setx MYTEST "new-value"`. Close CMD. Open a new CMD and check `echo %MYTEST%`.
8. Try to set a PATH value longer than 1024 characters with `setx`. Observe the truncation warning.

---

## Exercise 9: Registry Exploration

**Objective:** Familiarize yourself with the Registry Editor.

1. Open Registry Editor (`regedit`). Accept UAC prompt.
2. Navigate to `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`. This lists programs that start with your user session.
3. Navigate to `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`. This lists installed programs (some may be under `WOW6432Node` for 32-bit).
4. Navigate to `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`. Find the `spooler` subkey. Examine its values, especially `Start` (2 = Automatic, 3 = Manual, 4 = Disabled).
5. Navigate to `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion`. Note the `ProductName`, `CurrentBuild`, and `ReleaseId`.
6. **Do not modify any registry values.** The purpose is to understand the structure.

---

## Exercise 10: System Information Gathering

**Objective:** Learn to quickly gather system details.

Run each of these commands and note what information they provide:

1. `winver` — Quick OS version dialog
2. `msinfo32` — Full system information (hardware, drivers, conflicts)
3. `dxdiag` — Display and audio diagnostics
4. `systeminfo` (CMD) — Detailed system summary
5. PowerShell: `Get-ComputerInfo` — Comprehensive system info
6. PowerShell: `Get-CimInstance Win32_OperatingSystem | Select-Object *` — OS details
7. PowerShell: `Get-LocalUser` — Local user accounts
8. PowerShell: `Get-LocalGroup` — Local groups

Save the output of at least three of these commands to files for future reference.
