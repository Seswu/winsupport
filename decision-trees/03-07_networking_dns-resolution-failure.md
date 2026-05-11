# DNS Resolution Failures

User sees "server not found" or "DNS address could not be found" for websites they know exist.

```
User reports: "I get 'server not found' for sites that should work"

├─ CONFIRM IT'S DNS
│   ├─ `ping <site-domain>` → "Ping request could not find host"
│   ├─ `ping 8.8.8.8` → succeeds (confirms internet is up, DNS is the issue)
│   └─ `nslookup <site-domain>` → times out or returns wrong IP
│
├─ DOES THE SITE RESOLVE ON A DIFFERENT DNS?
│   └─ `nslookup <site-domain> 8.8.8.8` (query Google DNS directly)
│       ├─ Returns correct IP → DNS server is the problem
│       │   → Corporate DNS may not have the record → Check internal DNS zone
│       │   → Flush cache: `ipconfig /flushdns` → retry
│       │
│       └─ Also fails → Issue is at the domain registrar or NS servers
│           → Check: isitdownrightnow.com → may be a public outage
│
├─ COMMON CAUSES
│   ├─ **Stale local DNS cache**
│   │   → `ipconfig /flushdns` → `ipconfig /registerdns`
│   │   → Reboot
│   │
│   ├─ **Wrong DNS server configured**
│   │   → `ipconfig /all` → check DNS server IPs
│   │   │   ├─ Points to a server that's down? → Switch to alternate
│   │   │   ├─ Points to the wrong server? → Correct via DHCP or static config
│   │   │   └─ Points to "8.8.8.8" but corporate needs internal DNS? →
│   │   │       → May not resolve internal hostnames
│   │   └─ Reset: `ipconfig /release` → `renew`
│   │
│   ├─ **Hosts file hijacking**
│   │   → `notepad C:\Windows\System32\drivers\etc\hosts`
│   │   → Look for entries mapping the domain to 127.0.0.1 or wrong IP
│   │   → Remove or comment out (#) the bad entry
│   │
│   ├─ **VPN DNS settings**
│   │   → VPN may push its own DNS servers → Check after disconnecting VPN
│   │   → Some VPN clients don't restore original DNS on disconnect
│   │   → Reset: `netsh interface ip set dns "Ethernet" dhcp`
│   │
│   ├─ **Network adapter DNS registration**
│   │   → `ipconfig /registerdns` → forces the machine to re-register
│   │
│   └─ **DNSSEC validation failure**
│       → If the domain has broken DNSSEC records, some resolvers will NXDOMAIN
│       → Try resolving without DNSSEC (change to 8.8.8.8 temporarily)
│
├─ INTERNAL HOSTS (internal company resources)
│   ├─ Can you resolve by FQDN but not short name?
│   │   → DNS suffix search order issue → Check adapter settings or GPO
│   │   → `ipconfig /all` → DNS Suffix Search List
│   │
│   └─ Can't resolve at all?
│       → Internal DNS zone may not be available on this network
│       → Check if the user needs VPN to access internal DNS
│
└─ AFTER ALL STEPS — still failing?
    └─ Check if it's all sites or one site
        ├─ All sites → Escalate to Network/Infrastructure (DNS server may be down)
        └─ One site → Escalate to web/app owner (DNS record may be wrong)
```

**RESULT** → DNS resolution functioning correctly.
