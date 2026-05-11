# Cannot Access Specific Website or Application

User can get to some sites but a specific website or web application won't load.

```
User reports: "I can access everything except [specific site/app]"

├─ ISOLATE THE SCOPE
│   ├─ Does anyone else have the same issue?
│   │   ├─ YES → Site may be down → Check with the site owner
│   │   │       Check isitdownrightnow.com or status page of the service
│   │   └─ NO  → Issue is specific to this user/machine
│   │
│   └─ Does it fail in all browsers?
│       ├─ YES → Continue below
│       └─ NO  → Browser-specific issue → Clear cache, disable extensions
│
├─ CHECK ACCESSIBILITY
│   ├─ `ping <site-domain>`
│   │   ├─ Fails → DNS or routing issue
│   │   └─ Succeeds → Host is reachable
│   │
│   ├─ `nslookup <site-domain>`
│   │   ├─ Returns IP? → OK
│   │   └─ DNS resolver timed out / NXDOMAIN → DNS issue
│   │       → Try: `ipconfig /flushdns` → retry
│   │       → Try: use 8.8.8.8 as DNS temporarily to test
│   │
│   ├─ `tracert <site-domain>`
│   │   → Where does the trace stop?
│   │   ├─ Inside network → Firewall or proxy blocking
│   │   └─ Outside network → External routing issue
│   │
│   └─ `Test-NetConnection <site-domain> -Port 443` (PS)
│       → TCP port open? → Website is up, issue may be app-layer
│       → TCP port closed? → Firewall blocking
│
├─ PROXY / VPN CHECK
│   ├─ Is the user behind a web proxy?
│   │   → Settings → Network & Internet → Proxy
│   │   ├─ "Automatically detect settings" = ON
│   │   └─ "Use a proxy server" = check if one is configured
│   │       → If a proxy is set, does the site require bypassing it?
│   │
│   └─ Is the user on VPN?
│       → Some sites are blocked on VPN or require direct internet
│       → Try disconnecting VPN → test → reconnect
│
├─ BROWSER / LOCAL FIXES
│   ├─ Clear browser cache and cookies
│   ├─ Disable all extensions/plugins
│   ├─ Try Incognito/Private mode
│   ├─ Check hosts file: `C:\Windows\System32\drivers\etc\hosts`
│   │   → Look for entries pointing the domain to wrong IP
│   ├─ Reset Winsock: `netsh winsock reset` → reboot
│   └─ Flush DNS: `ipconfig /flushdns` → reboot
│
├─ FIREWALL / SECURITY
│   ├─ Temporarily disable third-party firewall/antivirus
│   ├─ Check Windows Firewall for blocking rules
│   └─ Check Windows Defender Application Guard or Network Protection
│
└─ STILL BLOCKED?
    └─ Provide detailed info to Tier 2/Network team:
        - Site URL
        - Error message (exact text or code)
        - Screenshot of the error
        - Browser developer console output (F12 → Console tab)
        - Is it a company site? → Check with the app owner
        - Is it a public site? → Check proxy logs
```

**RESULT** → Site accessible, or escalation with full diagnostic data.
