# Battery Not Charging / Drains Quickly

Laptop battery isn't charging, charges slowly, or drains faster than expected.

```
User reports: "My laptop battery isn't charging" or "it dies really fast"

├─ NOT CHARGING
│   ├─ Is the power adapter plugged in at both ends?
│   │   → Wall outlet → Power brick → Laptop charging port
│   │
│   ├─ Is the charging LED on (on the laptop, power brick, or both)?
│   │   ├─ LED off → No power reaching the laptop
│   │   │   → Try a different wall outlet
│   │   │   → Check the power cable for damage
│   │   │   → Check if the power brick has a removable cable (may be loose)
│   │   │   → Try a different power adapter (same wattage)
│   │   ├─ LED on but battery % drops → Adapter wattage too low
│   │   │   → Using a phone charger instead of laptop charger?
│   │   │   → Using a lower-wattage adapter than the laptop requires?
│   │   │   → Laptop may charge but slowly or not under load
│   │   └─ LED on but battery % stays the same → Battery health check
│   │
│   ├─ Check battery status in Settings:
│   │   → Settings → System → Power & battery → Battery percentage
│   │   → Look for "Plugged in, not charging" message
│   │   ├─ Normal if battery is near 100% (stops charging to preserve battery)
│   │   ├─ Not normal at < 80% → Possible:
│   │   │   ├─ Battery is too hot → Let it cool → Retry
│   │   │   ├─ Battery threshold set by manufacturer (Lenovo/Samsung battery conservation mode)
│   │   │   └─ Battery driver issue → Device Manager → Batteries → Uninstall "Microsoft ACPI Compliant Control Method Battery" → Reboot
│   │   └─
│   │
│   ├─ Check power port for debris/damage:
│   │   → USB-C ports can accumulate lint → Gently clean with toothpick
│   │   → Barrel connectors can bend → Inspect for damage
│   │
│   └─ Generate battery report:
│       → `powercfg /batteryreport` → Open the HTML report
│       → Look at "Full Charge Capacity" vs "Design Capacity"
│       → If full charge capacity is < 50% of design → Battery worn out
│
├─ DRAINS QUICKLY
│   ├─ Generate energy report:
│   │   → `powercfg /energy` (run as admin, wait 60 seconds)
│   │   → Open the HTML report → Look for high-power-consumption processes
│   │
│   ├─ Check what's draining the battery:
│   │   → `powercfg /sleepstudy` (If the machine goes to sleep)
│   │   → Task Manager → See which apps use the most power
│   │   → Settings → System → Power & battery → Battery usage
│   │   → Note which apps are "High" battery usage
│   │
│   ├─ Common battery drain causes:
│   │   ├─ Screen brightness too high → Lower brightness
│   │   ├─ Too many background apps → Settings → Apps → Startup → Disable unnecessary
│   │   ├─ Bluetooth is on but unused → Turn off
│   │   ├─ Keyboard backlight → Turn off
│   │   ├─ High-performance power plan → Switch to Balanced or Power saver
│   │   ├─ Antivirus scanning → Check scan schedule
│   │   └─ Outdated drivers (especially GPU) → Update drivers
│   │
│   └─ Change power plan:
│       → Control Panel → Power Options
│       → Choose "Balanced" or "Power Saver" (not "High Performance")
│       → Or: Settings → System → Power & battery → Power mode → Best power efficiency
│
├─ BATTERY HEALTH
│   ├─ Battery report shows "Worn out" or "Replace"?
│   │   → If the battery can't hold a charge → Needs replacement
│   │   → Check warranty status → Request replacement battery
│   │
│   └─ Calibrate battery (for removable batteries):
│   │   → Charge to 100% → Use until the laptop shuts off
│   │   → Charge to 100% again → Battery gauge is recalibrated
│   └─
│
└─ STILL NOT CHARGING?
    └─ Try a different power adapter (borrow from identical laptop)
        ├─ Charges? → Original adapter is faulty → Replace adapter
        └─ Still won't charge? → Laptop charging circuit may be faulty
            → Escalate to hardware team / warranty repair
```

**RESULT** → Battery charging and lasting as expected, or replacement identified.
