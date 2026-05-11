# Wi-Fi Not Connecting / "No Internet, Secured"

User can see the Wi-Fi network but can't connect, or connects but has no internet access.

```
User reports: "Wi-Fi connected but no internet" or "can't see any networks"

├─ Is Wi-Fi turned on?
│   ├─ Check physical switch, Fn key, or F-key (common on laptops)
│   ├─ Check: Settings → Network & Internet → Wi-Fi → ON
│   └─ Check: Airplane mode is OFF
│
├─ Can the user see their SSID in the network list?
│   ├─ NO
│   │   ├─ Wi-Fi adapter disabled in Network Connections?
│   │   │   → `ncpa.cpl` → Right-click Wi-Fi → Enable
│   │   ├─ Wi-Fi adapter driver missing?
│   │   │   → Device Manager → Network adapters → Look for yellow exclamation
│   │   │   → Update or reinstall driver
│   │   └─ SSID broadcast disabled? → Must connect manually:
│   │       → Network & Internet → Wi-Fi → Manage known networks → Add manually
│   │       → Enter SSID, security type, password
│   │
│   └─ YES → Continue
│
├─ What does the connection status show?
│   ├─ "Connected, no internet" or "No internet, secured"
│   │   ├─ Can other devices connect on the same SSID?
│   │   │   ├─ NO → Router/AP is down → Escalate to Infrastructure
│   │   │   └─ YES → Problem is device-specific
│   │   │
│   │   ├─ Run: `ipconfig /release` then `ipconfig /renew`
│   │   │   └─ If IP starts with 169.254.x.x → DHCP failure
│   │   │       → `ipconfig /flushdns` + `netsh winsock reset` + reboot
│   │   │
│   │   ├─ Run: `ping 8.8.8.8`
│   │   │   ├─ Fails → Check if static IP/DNS is set (wrong gateway)
│   │   │   └─ Succeeds → DNS issue → `ipconfig /flushdns`
│   │   │
│   │   └─ Check proxy settings:
│   │       → Settings → Network & Internet → Proxy
│   │       → "Automatically detect settings" ON, everything else OFF
│   │
│   ├─ "Can't connect to this network"
│   │   └─ Forget network → Reconnect with correct password
│   │       → `netsh wlan show profile name="SSID" key=clear` (verify password)
│   │
│   └─ "No networks found" / adapter not showing
│       → Check if the Wi-Fi adapter is in power saving mode:
│           Device Manager → Network adapters → Wi-Fi adapter → Power Management
│           Uncheck "Allow computer to turn off this device to save power"
│
├─ Wi-Fi keeps disconnecting?
│   ├─ Check driver power saving (same as above)
│   ├─ Check for interference: move closer to AP
│   └─ Update Wi-Fi adapter driver from manufacturer
│
└─ After all steps: Still broken?
    ├─ Run Windows Network Troubleshooter
    ├─ Try USB Wi-Fi adapter (isolates internal adapter failure)
    └─ Escalate to Tier 2
```

**RESULT** → Wi-Fi connected and internet accessible.
