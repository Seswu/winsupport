# Hands-On Exercises: Diagnostic Tools & Reference

---

## Exercise 1: Event Viewer Mastery

**Objective:** Become proficient at finding and interpreting events.

1. Open Event Viewer. Create the following Custom Views:
   - **Application Crashes**: Application log, Event ID 1000, 1001, 1002
   - **Service Failures**: System log, Event ID 7000, 7023
   - **Update Events**: WindowsUpdateClient log, all levels
   - **Security Logons**: Security log, Event ID 4624, 4625 (if auditing enabled)
2. For each custom view, filter to the last 7 days
3. Open one event from each view and examine:
   - Event ID
   - Source
   - General description
   - Details tab (XML) — find specific data fields
4. Practice the "Find Related Events" feature on an event
5. Export a log: Right-click a log > Save Events As > save as .evtx

---

## Exercise 2: Resource Monitor Investigation

**Objective:** Use Resource Monitor to identify resource bottlenecks.

1. Open Resource Monitor (`resmon`)
2. On the CPU tab:
   - Sort processes by CPU
   - Expand a process to see its threads
   - Right-click a process > Search Online (for unfamiliar processes)
3. On the Disk tab:
   - Sort by Total (B/sec)
   - Note which files are being read/written most
   - Note which processes have the most disk activity
4. On the Network tab:
   - Note which processes have network connections
   - Note the remote addresses and ports
5. On the Memory tab:
   - Note the Hard Faults/sec — high numbers indicate memory pressure
   - Note the standby vs. free memory

---

## Exercise 3: Process Explorer Deep Dive

**Objective:** Learn to use Process Explorer for advanced diagnostics.

1. Download and run Process Explorer as Administrator
2. View > Show Lower Pane. View > Lower Pane View > DLLs
3. Find a running process (e.g., your browser):
   - Note its parent process
   - Note the command line
   - In the lower pane, note loaded DLLs
4. Find > Find DLL — search for a DLL name. Which processes load it?
5. Find > Find Handle — search for a file name. Which process has it open?
6. Double-click a process > Strings tab — note what strings are embedded
7. Options > Replace Task Manager — make Process Explorer your default

---

## Exercise 4: Process Monitor Investigation

**Objective:** Use ProcMon to trace application behavior.

1. Download and run Process Monitor as Administrator
2. It immediately starts capturing. Let it run for 10 seconds, then pause (Ctrl+E)
3. Clear the display (Ctrl+X)
4. Set a filter: Process Name is `notepad.exe` then Include
5. Launch Notepad. Observe the captured events.
6. Note the file system activity (reading configuration, fonts, etc.)
7. Note the registry activity (reading user settings)
8. In Notepad, create a new file and save it. Observe the file creation events.
9. Set a filter: Result is `NAME NOT FOUND` then Include. This shows files the process is looking for but can't find — useful for diagnosing "missing file" issues.
10. Save the capture: File > Save. Format: Native Process Monitor Format (PML).

---

## Exercise 5: Autoruns Investigation

**Objective:** Use Autoruns to identify auto-starting programs.

1. Download and run Autoruns as Administrator
2. Examine the Logon tab — these are programs that start at user login
3. Compare with Task Manager > Startup apps — Autoruns shows much more
4. Examine the Services tab — all services, including disabled ones
5. Examine the Scheduled Tasks tab — all scheduled tasks
6. Examine the Everything tab — all auto-start entries
7. Options > Filter Options > check "Hide Microsoft Entries" — now only third-party entries show
8. Options > Verify Code Signatures — unsigned entries are highlighted in pink
9. Right-click any entry > Search Online to research what it is
10. **Do not disable anything** in this exercise — just observe

---

## Exercise 6: Build Your Reference Document

**Objective:** Create your personal cheat sheet.

1. Create a new document (OneNote, text file, or Markdown)
2. Add the following sections:
   - Essential commands (from reference material)
   - Log file locations (from reference material)
   - Admin portal URLs
   - Common error codes
   - Escalation contacts (fill in with your team's info)
   - Personal notes (add as you learn new things)
3. Format it for quick scanning (use tables, bullet points, bold headings)
4. Keep it accessible — pin to taskbar, save to desktop, or print it
5. Commit to updating it weekly

---

## Exercise 7: PowerShell Diagnostic Script

**Objective:** Create a reusable diagnostic script.

Write a PowerShell script that gathers the following and outputs to a text file:

1. System info (OS version, build, uptime)
2. Top 5 processes by CPU and memory
3. Disk space for all drives
4. Network adapter status
5. Services that are stopped but should be running (Automatic startup type)
6. Last 5 application errors from Event Viewer
7. Last 5 system errors from Event Viewer
8. Windows Update status (pending updates)
9. BitLocker status
10. Time sync status

Save this script and run it on your VM. Use it as a starting point for future diagnostic needs.
