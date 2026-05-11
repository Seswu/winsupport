# USB Device Not Recognized

Windows makes a disconnect sound or shows "USB device not recognized" when plugging in a device.

```
User reports: "My USB drive / mouse / keyboard isn't working"

├─ PHYSICAL CHECK
│   ├─ Try a different USB port:
│   │   → Front vs back ports (back ports are direct to motherboard)
│   │   → USB 2.0 vs USB 3.0 (blue = 3.0)
│   │
│   ├─ Try the device on a different computer:
│   │   ├─ Works on other computer → Issue is this machine's USB controller/driver
│   │   └─ Fails on all computers → Device is faulty
│   │
│   └─ Check for physical damage:
│       → USB connector bent or dirty?
│       → Try a different USB cable (for devices with detachable cables)
│
├─ DEVICE MANAGER
│   ├─ Open: `devmgmt.msc`
│   │   └─ Expand "Universal Serial Bus controllers"
│   │       ├─ Look for: "Unknown USB Device" or yellow exclamation
│   │       │   → Right-click → Properties → Device status
│   │       │   → Note the error code (e.g., Code 10, Code 43)
│   │       └─ Look for any greyed-out or ghost devices
│   │           → View → Show hidden devices → Remove ghost entries
│   │
│   ├─ Uninstall and reinstall the device:
│   │   → Right-click the problematic USB entry → Uninstall device
│   │   → Unplug the USB device → Reboot → Plug it back in
│   │
│   └─ Common error codes:
│       ├─ **Code 10** → Device cannot start → Driver issue
│       │   → Update driver → Roll back driver → Uninstall/reinstall
│       ├─ **Code 28** → Driver not installed → Install driver
│       ├─ **Code 43** → Device failed → Windows has stopped it
│       │   → Try a different USB port → Update USB controller driver
│       └─ **Code 45** → Device not connected → Hardware issue
│
├─ POWER MANAGEMENT
│   ├─ USB selective suspend may be cutting power:
│   │   → Control Panel → Power Options → Change plan settings
│   │   → Change advanced power settings → USB settings
│   │   → USB selective suspend setting → Disable
│   │
│   └─ Disable power saving on USB Root Hub:
│       → Device Manager → USB controllers → USB Root Hub (each one)
│       → Properties → Power Management → Uncheck "Allow computer to turn off..."
│       → Do this for ALL USB Root Hub entries
│
├─ DRIVERS
│   ├─ Update the USB controller driver:
│   │   → Device Manager → USB controllers → Intel/Renesa USB controller
│   │   → Update driver → Search automatically
│   │   → Or: download chipset drivers from motherboard/laptop manufacturer
│   │
│   └─ Check Windows Update for USB-related updates:
│       → Settings → Windows Update → Check for updates
│       → Optional updates → Look for USB/driver updates
│
└─ SPECIFIC DEVICE ISSUES
    ├─ **USB drive not showing up in File Explorer**
    │   → Check Disk Management (`diskmgmt.msc`)
    │   ├─ Drive appears but no letter → Right-click → Change drive letter
    │   └─ Drive doesn't appear → Disk may be dead
    │
    ├─ **USB mouse/keyboard not working in Windows but works in BIOS**
    │   → Driver issue → Boot Safe Mode → Test → Update drivers
    │
    └─ **USB device works intermittently**
        → Power management is likely cutting power → See power management section above
```

**RESULT** → USB device recognized and functioning correctly.
