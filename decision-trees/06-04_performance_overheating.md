# Fan Running Loud / Overheating

The computer fan is loud constantly, the machine feels hot to the touch, or it shuts down under load.

```
User reports: "My laptop fan is really loud" or "it feels hot"

├─ CHECK TEMPERATURES
│   ├─ Use a monitoring tool to check CPU/GPU temps:
│   │   → Task Manager → Performance → CPU/GPU → Look for temperature
│   │   → Or download: HWMonitor, Core Temp, Open Hardware Monitor
│   │   → Normal idle: 30-45°C | Normal under load: 60-85°C
│   │   → Critical: > 95°C (may throttle or shut down)
│   │
│   └─ When does it get hot?
│       ├─ Under load (gaming, video editing, compiling) → Normal
│       ├─ At idle or light use → Issue (dust, thermal paste, or background process)
│       └─ Only when plugged in → Power plan may be forcing high performance
│
├─ PHYSICAL FIXES
│   ├─ Is the laptop on a hard surface?
│   │   → Beds, blankets, pillows block air intakes → Move to desk
│   │   → Consider a laptop cooling pad
│   │
│   ├─ Clean the vents and fans:
│   │   → Use compressed air to blow out dust (from exhaust side)
│   │   → Hold the fan blades still while blowing (to prevent spinning damage)
│   │   → Check: Are the vents physically blocked by dust?
│   │
│   └─ Check the fan itself:
│   │   → Is the fan spinning at all? (hold hand near exhaust)
│   │   → Is the fan making a grinding/clicking noise? → Bearing failing
│   │   → Replace fan if faulty (escalate to hardware team)
│   └─
│
├─ SOFTWARE FIXES
│   ├─ Check for resource hogs (keeping CPU/GPU at high load):
│   │   → Task Manager → Sort by CPU → Identify the culprit
│   │   → [see 06-01](06-01_performance_pc-slow.md)
│   │
│   ├─ Change power plan:
│   │   → Control Panel → Power Options → "Balanced" or "Power Saver"
│   │   → High Performance = maximum heat
│   │
│   ├─ Check if the machine is on a "Performance" or "Turbo" mode:
│   │   → Some OEMs (Lenovo, Dell, ASUS) have performance slider
│   │   → Switch to "Quiet" or "Battery Saver" mode
│   │
│   └─ Update BIOS/UEFI:
│   │   → BIOS updates often include fan curve fixes
│   │   → Check manufacturer support site for the model
│   └─
│
├─ ADVANCED
│   ├─ Undervolt the CPU (if BIOS allows):
│   │   → Use ThrottleStop or Intel XTU
│   │   → Reduces heat at the cost of minor performance
│   │
│   ├─ Re-paste thermal compound:
│   │   → If the computer is > 2 years old, thermal paste may be dried out
│   │   → Requires disassembly → Escalate to hardware team
│   │
│   └─ Check for BIOS fan speed settings:
│   │   → Some BIOS have fan speed controls → "Always on" vs "Adaptive"
│   └─
│
├─ RANDOM SHUTDOWNS (thermal protection)
│   └─ If the machine shuts down without warning:
│       → Almost certainly overheating → Immediate cooling needed
│       → Check vents → clean → let cool before restarting
│       → If it shuts down at low temperatures → Hardware fault
│
└─ AFTER CLEANING — STILL HOT?
    └─ Hardware may be failing (fan, thermal sensor, or chipset)
        → Escalate to hardware team for repair/replacement
```

**RESULT** → Temperature normalized, or repair path identified.
