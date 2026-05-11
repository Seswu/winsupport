# VPN Won't Connect

User cannot establish a VPN connection, or the VPN connects but traffic doesn't route correctly.

```
User reports: "VPN won't connect" or "VPN connects but I can't access anything"

├─ ISOLATE THE FAILURE POINT
│   ├─ Is the user on the internet at all?
│   │   └─ Test: `ping 8.8.8.8` from the user's machine
│   │       ├─ Fails → Fix internet first [see 03-01, 03-06]
│   │       └─ Succeeds → Internet is fine, issue is VPN-specific
│   │
│   ├─ Does the VPN client launch and show an error?
│   │   ├─ Error: "Unable to connect to server"
│   │   │   → VPN server address may be wrong → Check the server FQDN/IP
│   │   │   → Firewall may be blocking VPN port (e.g., UDP 500, UDP 4500 for IPsec)
│   │   │   → Try: `telnet <vpn-server> <port>` to test port reachability
│   │   ├─ Error: "Authentication failed"
│   │   │   → [see 01-01](01-01_account-auth_password-reset.md) — password may be wrong
│   │   │   → Certificate issue → Check if user's certificate is valid
│   │   ├─ Error: "No internet, secured" after connecting
│   │   │   → Split tunneling misconfigured → all traffic via VPN may be blocked
│   │   └─ Error: "Connection terminated by server"
│   │       → Server may be at capacity → Try again later
│   │       → Check if user's session already exists (another device)
│   │
│   └─ No error — just doesn't connect / sits spinning
│       → Check if the VPN service is running on the machine
│       → services.msc → Look for the VPN client service
│       → Restart the service → retry
│
├─ CLIENT-SPECIFIC FIXES
│   ├─ Reset the VPN client (varies by client):
│   │   ├─ AnyConnect: `vpnui.exe` → Advanced Statistics → Reset
│   │   ├─ GlobalProtect: Right-click icon → Troubleshoot → Reset
│   │   ├─ Windows built-in: Settings → Network → VPN → Select → Remove → Re-add
│   │   └─ Check for VPN client updates
│   │
│   ├─ Uninstall and reinstall the VPN client
│   │   └─ Check: Is there a newer version compatible with the current Windows build?
│   │
│   └─ Run the vendor's diagnostics tool if available
│
├─ NETWORK / FIREWALL
│   ├─ Disable third-party firewall/antivirus temporarily (conflict with VPN adapter)
│   ├─ Check if the VPN adapter is enabled:
│   │   `ncpa.cpl` → Look for VPN adapter (may be disabled after update)
│   └─ Reset network stack:
│       `netsh int ip reset` + `netsh winsock reset` + reboot
│
├─ VPN CONNECTS BUT NO ACCESS
│   └─ Check routing:
│       → `route print` before and after connecting
│       → Does the VPN add the correct routes?
│       → Try: `tracert <internal-resource>` — does it go through the VPN tunnel?
│
└─ STILL BROKEN?
    ├─ Try from a different network (hotspot, home, office guest)
    │   └─ Works on other network → User's network is blocking VPN
    └─ Escalate to Infrastructure / VPN team
        Provide: VPN server, client version, error message, timestamp
```

**RESULT** → VPN connected and internal resources accessible.
