# Can't Log In — General

User cannot log in, but the error message is unclear or doesn't match password/lockout/MFA patterns.

```
User reports: "I can't log in" — no clear error or multiple errors across systems

├─ ISOLATE: Which system is failing?
│   ├─ Windows login (local machine)
│   │   ├─ Is the machine connected to the network?
│   │   │   ├─ NO → Cached credentials may work, but domain login won't
│   │   │   │       Try logging in with cached credentials (previous password)
│   │   │   │       If that fails → IT admin must connect machine to network
│   │   │   └─ YES
│   │   ├─ Check: Is the correct username format being used?
│   │   │   ├─ Domain-joined: domain\username or username@domain.com
│   │   │   └─ Local: .\username or machine-name\username
│   │   ├─ Check: Caps Lock, Num Lock, keyboard layout
│   │   └─ Check: Is this the right user account? (screen may show a different user)
│   │
│   ├─ Office 365 / web portal
│   │   ├─ Check Entra ID sign-in logs:
│   │   │   └─ Look for error code:
│   │   │       ├─ 50053 → Account locked [see 01-02]
│   │   │       ├─ 50055 → Password expired [see 01-01]
│   │   │       ├─ 50057 → Account disabled [see 01-05]
│   │   │       ├─ 53003 → Conditional Access block → Check CA policy
│   │   │       ├─ 50126 → Invalid credentials → Password issue [see 01-01]
│   │   │       └─ Other → Search the error code in Microsoft docs
│   │   └─ Is the user trying to access via a blocked location/device?
│   │       └─ Conditional Access may be blocking → Check policy name in sign-in log
│   │
│   ├─ Specific application / VPN
│   │   ├─ Is the application using SSO (Entra ID / SAML)?
│   │   │   ├─ YES → Check if SSO is failing upstream (same as O365 above)
│   │   │   └─ NO  → App has its own credentials → Check app-specific account
│   │   ├─ Does the app require a specific group membership?
│   │   │   └─ [see 01-06](01-06_account-auth_app-access-failure.md)
│   │   └─ Is the app configured for modern auth vs legacy auth?
│   │       └─ If legacy, MFA may not work [see 02-02]
│   │
│   └─ Remote Desktop (RDP)
│       ├─ Is the user allowed to RDP to this machine?
│       │   └─ Check "Remote Desktop Users" local group on the target machine
│       ├─ Is the machine on and reachable? (`ping <hostname>`)
│       └─ Is Network Level Authentication (NLA) causing issues?
│           └─ Try `mstsc /admin` to connect to the console session
│
└─ Unclear error / still failing?
    └─ Collect: exact error message, screenshot, timestamp, affected system
        → Check service health (status.microsoft.com or admin portal)
        → Escalate to Tier 2 with collected information
```

**RESULT** → Login failure diagnosed and resolved, or escalated with detail.
