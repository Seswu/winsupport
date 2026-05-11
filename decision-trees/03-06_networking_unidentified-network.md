# Unidentified Network / No Network Access

The network status shows "Unidentified network" or "Ethernet doesn't have a valid IP configuration."

```
User reports: "My network says 'Unidentified network'" or "No network access"

├─ What does `ipconfig` show?
│   └─ IPv4 address starts with:
│       ├─ **169.254.x.x** → APIPA — DHCP failed
│       │   → The NIC self-assigned an IP because it couldn't reach the DHCP server
│       │   ├─ Is the Ethernet cable plugged in?
│       │   ├─ Is the port active? (try a different wall jack)
│       │   ├─ Run: `ipconfig /release` → `ipconfig /renew`
│       │   │   └─ Still 169.254? → DHCP server unreachable
│       │   │       → Check if the user is on VPN (VPN assigns virtual IP)
│       │   │       → Check if a recent update disabled DHCP client service
│       │   │       → `services.msc` → "DHCP Client" → Running? Automatic?
│       │   └─ Try: `netsh interface ip reset` → reboot
│       │
│       ├─ **Valid IP** (e.g., 192.168.x.x, 10.x.x.x)
│       │   ├─ Can you ping the gateway?
│       │   │   → `ipconfig` → Default Gateway → `ping <gateway>`
│       │   │   ├─ Fails → Wrong gateway/subnet → Static IP incorrectly set?
│       │   │   │   → Check: Is "Obtain IP automatically" truly selected?
│       │   │   │   → Set to DHCP → renew
│       │   │   └─ Succeeds → Issue is name resolution or firewall
│       │   │
│       │   └─ Does the network profile say "Public" when it should be "Domain"?
│       │       → Machine may not be properly domain-joined
│       │       → `dsregcmd /status` → Check Azure AD joined / Domain joined
│       │       → Run: `gpupdate /force` → reboot
│       │
│       └─ **0.0.0.0** → Interface disabled or driver issue
│           → `ncpa.cpl` → Right-click Ethernet → Enable
│           → If already enabled → Disable → Enable
│           → Device Manager → Network adapter → Update driver
│
├─ SWITCH / CABLE
│   ├─ Try a different Ethernet cable
│   ├─ Try a different port on the wall/switch
│   └─ Try plugging into a different device (laptop → PC) to isolate
│
├─ NETWORK PROFILE TYPE
│   ├─ Why does it say "Unidentified" instead of a network name?
│   │   → The machine didn't receive a network name via DHCP option or DNS
│   │   → This is cosmetic — but can block file sharing and discovery
│   │   → To fix: Set a static network profile via registry
│   │       HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
│   │       → Find the profile → Change "Category" (1=Public, 2=Private)
│   │
│   └─ If the machine is domain-joined:
│       → Should show "Domain" networks → If not → Check network location awareness
│       → `services.msc` → "Network Location Awareness" → Running? Automatic?
│
└─ STILL UNIDENTIFIED?
    ├─ Disable IPv6 (if not needed by the network)
    │   → `ncpa.cpl` → Right-click Ethernet → Properties → Uncheck IPv6
    ├─ Disable any third-party network filters (antivirus, VPN adapters)
    └─ Escalate to Network/Infrastructure team
```

**RESULT** → Network identified and functional.
