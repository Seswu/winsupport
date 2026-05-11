# External Monitor Not Extending

A monitor is detected but won't extend — it only mirrors the laptop screen or shows no signal.

```
User reports: "I connected a monitor but it's just showing the same thing as my laptop"

├─ Win+P → Check display mode
│   ├─ Currently "Duplicate" → Change to "Extend"
│   ├─ Currently "PC screen only" → Change to "Extend"
│   └─ Currently "Extend" but still not working → Continue
│
├─ SETTINGS → SYSTEM → DISPLAY
│   ├─ Are both monitors detected (1, 2, etc.)?
│   │   ├─ Only one monitor shown → [see 05-02](05-02_hardware_docking-station-monitors.md)
│   │   └─ Both shown → Continue
│   │
│   ├─ Is the second monitor greyed out or dim?
│   │   → Click "Detect" → If still dim, connection issue
│   │
│   └─ Scroll to "Multiple displays" → dropdown
│       ├─ "Extend these displays" → Selected?
│       └─ No → Select it → "Keep changes"
│
├─ MONITOR SPECIFIC
│   ├─ Check the monitor's on-screen input menu:
│   │   → Press the physical button on the monitor
│   │   → Navigate to Input/Source → Select the correct port (HDMI, DP, USB-C)
│   │
│   ├─ Try a different cable:
│   │   → HDMI vs DP vs USB-C → different cables may behave differently
│   │   → Some laptops only output to certain ports (e.g., HDMI works, DP doesn't)
│   │
│   └─ Try a different monitor:
│       → Does a different monitor extend?
│       ├─ YES → Issue is with the original monitor
│       └─ NO  → Issue is with the laptop/dock
│
├─ RESOLUTION / SCALING
│   ├─ Sometimes an "extend" fails if the monitor resolution is not supported:
│   │   → Settings → Display → Select the external monitor
│   │   → Display resolution → Try a lower resolution
│   │   → Display orientation → Landscape
│   │
│   └─ Try changing the refresh rate:
│       → Advanced display → Refresh rate → 60 Hz (most compatible)
│
├─ GRAPHICS DRIVER
│   ├─ Open the GPU control panel (Intel Graphics, NVIDIA Control Panel, AMD Adrenalin)
│   │   → Look for display/multiple display settings
│   │   → Force detection or re-configure
│   │
│   └─ Update or roll back graphics driver:
│       → Some drivers have known issues with multi-monitor
│
└─ STILL MIRRORING OR NO SIGNAL?
    └─ Reboot with monitor connected:
        → Sometimes Windows detects monitors only at boot
        → Reboot → After login, check Win+P → Extend
```

**RESULT** → External monitor extending properly with laptop display.
