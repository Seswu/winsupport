# Printer Won't Print

User sent a print job but nothing comes out of the printer — print queue is stuck or jobs disappeared.

```
User reports: "I tried to print but nothing happened"

├─ CHECK THE OBVIOUS FIRST
│   ├─ Is the printer turned on?
│   ├─ Does it have paper?
│   ├─ Does it have toner/ink?
│   └─ Are there any error lights blinking?
│
├─ PRINT QUEUE
│   ├─ Open: Settings → Bluetooth & devices → Printers & scanners
│   │   → Select the printer → Open print queue
│   │   ├─ Queue is empty → App didn't send the job correctly
│   │   │   → Try printing from a different app (Notepad test page)
│   │   ├─ Queue has a "Printing" or "Error" status
│   │   │   → Right-click the document → "Cancel" or "Restart"
│   │   │   → Clear the queue completely
│   │   ├─ Queue has many jobs stuck → Clear all, restart spooler
│   │   │   → `services.msc` → Print Spooler → Right-click → Stop
│   │   │   → Delete: C:\Windows\System32\spool\PRINTERS\* (delete contents)
│   │   │   → Right-click → Start (the service)
│   │   └─ "Printer offline" → Right-click printer → "See what's printing"
│   │       → Printer menu → "Use Printer Offline" → UNCHECK
│   │
│   └─ Print a test page:
│       → Printer Properties → Print Test Page
│       ├─ Prints? → Printer is fine, issue is with the app/document
│       └─ Fails? → Continue below
│
├─ DRIVER / CONNECTION
│   ├─ Is this a network printer?
│   │   ├─ `ping <printer-ip>` → Is it reachable?
│   │   ├─ Check if the printer port is correct:
│   │   │   → Printer Properties → Ports → Check which port
│   │   │   → If "WSD" port → Try switching to "Standard TCP/IP" port
│   │   └─ Re-add the printer: Remove → Add printer → Search again
│   │
│   ├─ Is this a USB printer?
│   │   ├─ Unplug USB → Replug → Test
│   │   ├─ Try a different USB cable
│   │   ├─ Try a different USB port
│   │   └─ Check Device Manager → Is printer listed?
│   │
│   └─ Update or reinstall the printer driver:
│       → Settings → Printers → Printer Properties → Advanced → New Driver
│       → Download latest driver from manufacturer
│
├─ WINDOWS SPECIFIC
│   ├─ Run Print Troubleshooter:
│   │   → Settings → Troubleshoot → Other troubleshooters → Printer
│   │
│   ├─ Reset Print Spooler (alternative method):
│   │   ```cmd
│   │   net stop spooler
│   │   del /Q /F /S "%windir%\System32\spool\PRINTERS\*"
│   │   net start spooler
│   │   ```
│   │
│   └─ Check if the printer is set as default:
│       → Settings → Printers → Select printer → Set as default
│
├─ "ACCESS DENIED" WHEN PRINTING
│   └─ User doesn't have permission to print to this printer
│       → Printer Properties → Security → Add user/group → Allow "Print"
│
└─ STILL NOT PRINTING?
    └─ Test with a different printer (if available)
        ├─ Works on other printer? → Issue is printer-specific
        └─ Fails on all printers? → System-wide print issue → Check spooler
            Escalate to Tier 2
```

**RESULT** → Printer working and print jobs completing.
