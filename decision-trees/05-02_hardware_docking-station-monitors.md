# Docking Station Not Detecting Monitors

External monitors connected via a docking station are not detected, or the display is not extending.

```
User reports: "I plugged in my laptop but the external monitors don't work"

├─ PHYSICAL CHECKS
│   ├─ Is the docking station power light on?
│   │   ├─ NO → Dock not getting power → Check power adapter connection
│   │   │       Try a different power outlet
│   │   └─ YES → Continue
│   │
│   ├─ Is the USB-C / Thunderbolt cable fully inserted?
│   │   → Unplug and re-seat firmly (both ends: laptop and dock)
│   │
│   ├─ Are the monitor cables (HDMI/DP/USB-C) correctly inserted?
│   │   → Unplug and re-seat monitor cables at both ends
│   │
│   └─ Are the monitors turned on?
│       → Check each monitor's power light
│       → Check each monitor's input source (HDMI1, HDMI2, DP, etc.)
│
├─ WINDOWS DISPLAY SETTINGS
│   ├─ Press Win+P → Which mode is selected?
│   │   ├─ "PC screen only" → Only laptop display active
│   │   │   → Change to "Extend" or "Duplicate"
│   │   ├─ "Duplicate" → Same on both (may mask detection)
│   │   └─ "Extend" → Should show external monitors
│   │
│   ├─ Settings → System → Display
│   │   └─ Scroll to "Multiple displays" → "Detect" button
│   │       → Click "Detect" → Windows scans for displays
│   │       ├─ Monitors found? → Arrange them → Apply
│   │       └─ No monitors found → Continue below
│   │
│   └─ Check Display adapter properties:
│       → Advanced display → Display adapter properties
│       → Look for any errors in the adapter status
│
├─ DRIVER / FIRMWARE
│   ├─ Update or reinstall the docking station driver:
│   │   → Device Manager → Expand "Universal Serial Bus devices" or "Docking Station"
│   │   → Look for: yellow exclamation, "Dell Dock," "Lenovo Dock," "Plugable," etc.
│   │   → Right-click → Update driver → Search automatically
│   │   → Or download driver from dock manufacturer
│   │
│   ├─ Update graphics driver (GPU handles display output):
│   │   → Device Manager → Display adapters → Update drivers
│   │   → Download from GPU manufacturer (Intel, NVIDIA, AMD)
│   │
│   ├─ Update Thunderbolt / USB4 firmware:
│   │   → Check manufacturer's support site for firmware updates
│   │   → Some docks need firmware updates to work with new Windows builds
│   │
│   └─ Check if there's a BIOS/UEFI update for the laptop:
│       → Some BIOS updates fix Thunderbolt/USB-C compatibility
│
├─ DOCKING ORDER (important!)
│   ├─ Disconnect laptop from dock
│   ├─ Reboot the laptop
│   ├─ After reboot → Connect the dock
│   │   → Windows should detect and configure monitors
│   └─ If that doesn't work → Try connecting power first, then monitors, then USB devices
│
├─ DEVICE MANAGER
│   ├─ Check for hidden devices:
│   │   → Device Manager → View → Show hidden devices
│   │   → Look for greyed-out monitors → Right-click → Uninstall
│   │   → Then "Detect" again in Display Settings
│   │
│   └─ Look for "Basic Display Adapter" instead of GPU name:
│       → This means GPU driver isn't loading → Update GPU driver
│
└─ STILL NOT WORKING?
    └─ Test: Does a monitor work when plugged directly into the laptop?
        ├─ YES → Dock is the issue → Try a different dock / cable
        └─ NO  → Laptop display output may be faulty → Escalate to hardware team
```

**RESULT** → External monitors detected and extending correctly.
