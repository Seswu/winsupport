# Slow Internet / Intermittent Disconnections

User reports that the internet is slow or drops connection periodically.

```
User reports: "The internet keeps dropping" or "It's really slow today"

├─ ISOLATE THE SCOPE
│   ├─ Are other users on the same network affected?
│   │   ├─ YES → Likely network-wide issue → Notify Infrastructure
│   │   └─ NO  → Issue is specific to this user/machine
│   │
│   └─ Is this a wired or wireless connection?
│       ├─ Wi-Fi → [see 03-01](03-01_networking_wifi-not-connecting.md) — try wired
│       └─ Wired → Continue below
│
├─ GATHER DATA
│   ├─ Run `ping 8.8.8.8 -t` (continuous ping)
│   │   ├─ Request timed out → Intermittent drop
│   │   ├─ High latency (> 100ms consistently) → Congested or poor connection
│   │   └─ Good latency (< 30ms) → Ping is fine, issue may be upper-layer
│   │
│   ├─ Run `pathping 8.8.8.8`
│   │   → Shows loss per hop. Where does loss start?
│   │   └─ Loss at user's subnet → Local issue
│   │       Loss at ISP hop → Carrier issue
│   │
│   └─ Check event logs for network-related errors:
│       → Event Viewer → System → Filter for "e1i65" or "e1g" (Intel NIC events)
│       → Look for "Network link is disconnected" events
│
├─ LOCAL MACHINE FIXES
│   ├─ Reboot the machine (clears network stack, frees ARP cache)
│   ├─ Check for background activity:
│   │   → Task Manager → Network tab → is anything saturating the link?
│   │   ├─ Windows Update → Finish updates during off-hours
│   │   ├─ OneDrive syncing → Pause sync temporarily [see 02-05]
│   │   ├─ Antivirus scanning → Check if scanning causes spikes
│   │   └─ Torrent / file-sharing → Uninstall if present
│   │
│   ├─ Check NIC driver/power:
│   │   → Device Manager → Network adapter → Power Management
│   │   → Uncheck "Allow computer to turn off this device to save power"
│   │   → Update NIC driver from manufacturer
│   │
│   └─ Reset network stack:
│       → `netsh int ip reset` + `netsh winsock reset` + reboot
│
├─ CABLING / HARDWARE
│   ├─ Check Ethernet cable: try a different cable
│   ├─ Check Ethernet port: try a different wall jack or switch port
│   └─ Check if the NIC is faulty: try a USB Ethernet adapter
│
└─ After all steps: Still intermittent?
    └─ Escalate to Network team with:
        - Duration of pings showing packet loss
        - Machine name and MAC address
        - Wall jack/switch port info (if known)
        - Time pattern (if it happens at specific times = congestion)
```

**RESULT** → Connectivity stabilized or escalation data collected.
