# Monitor Flickering or Losing Signal

External monitor flickers, blinks, or periodically loses signal.

```
User reports: "My monitor keeps flickering" or "screen goes black for a second"

├─ ISOLATE THE CAUSE
│   ├─ Does the flicker happen on all monitors or just one?
│   │   ├─ All monitors → Likely laptop/GPU/docking station issue
│   │   └─ One monitor → Likely cable, monitor, or port issue
│   │
│   ├─ Does the flicker happen on the laptop's built-in screen?
│   │   ├─ YES → GPU driver or hardware issue
│   │   └─ NO  → External display / cable issue
│   │
│   └─ Does the flicker happen at all times or intermittently?
│       → Intermittent → Loose cable, interference, or heat issue
│       → Constant → Refresh rate, resolution, or driver issue
│
├─ CABLE CHECK
│   ├─ Try a different cable (HDMI, DP, USB-C)
│   ├─ Try a different cable type (e.g., HDMI instead of DP)
│   ├─ Ensure the cable is fully inserted at both ends
│   ├─ Check if the cable is damaged (bent pins, kinked)
│   └─ Try a shorter cable (long cables can degrade signal)
│
├─ MONITOR CHECK
│   ├─ Try the monitor with a different computer
│   │   ├─ Flickers on different computer too → Monitor is faulty
│   │   └─ Works on different computer → Issue is the laptop/dock
│   │
│   ├─ Check the monitor's own menu:
│   │   → Look for "DisplayPort version" or "HDMI version" settings
│   │   → Try different versions (1.2 vs 1.4, etc.)
│   │
│   └─ Check refresh rate:
│   │   → Windows: Settings → System → Display → Advanced display
│   │   → Try lowering the refresh rate (e.g., 144 Hz → 60 Hz)
│   │   → Some monitors flicker at certain refresh rates
│   └─
│
├─ DOCKING STATION
│   ├─ [see 05-02](05-02_hardware_docking-station-monitors.md)
│   ├─ Try plugging the monitor directly into the laptop (bypass dock)
│   │   └─ No flicker? → Dock is the issue → Update dock firmware
│   └─ Try a different port on the dock
│
├─ GPU / DRIVER
│   ├─ Update graphics driver:
│   │   → Device Manager → Display adapters → Update driver
│   │   → Download latest from Intel/NVIDIA/AMD directly
│   │
│   ├─ Roll back graphics driver if recently updated
│   │
│   ├─ Disable GPU hardware acceleration in affected apps:
│   │   → Browsers: Settings → System → Use hardware acceleration → OFF
│   │   → Teams: Settings → General → Disable GPU hardware acceleration
│   │
│   └─ Change GPU power management:
│   │   → NVIDIA Control Panel → Manage 3D settings → Power management → Prefer maximum performance
│   │   → Intel Graphics Command Center → Power → Use AC power when plugged in
│   └─
│
├─ ELECTRICAL / INTERFERENCE
│   ├─ Move power cables away from monitor cables
│   ├─ Try a different power outlet (ground loop can cause flickering)
│   └─ Check if fluorescent lights nearby are causing interference
│
└─ STILL FLICKERING?
    ├─ Disable Adaptive Sync / FreeSync / G-Sync:
    │   → These can cause flickering on some monitor/cable combinations
    │   → Disable in monitor menu and/or GPU control panel
    └─ Escalate to hardware team (monitor or laptop GPU may be faulty)
```

**RESULT** → Monitor stable with no flickering or signal loss.
