# Can't Access Specific Application

User can log into Windows and email, but a specific application refuses access.

```
User reports: "I can't access [App Name]" — everything else works

├─ DETERMINE authentication type
│   ├─ Does the app use SSO (Entra ID / SAML)?
│   │   ├─ YES → Check Entra ID sign-in logs for that specific app
│   │   │   └─ Application filter: select the app → look for failures
│   │   │       ├─ Error 53003 → Conditional Access policy blocking
│   │   │       │   → Check: CA policy name in log → What does it require?
│   │   │       │       Compliant device? → Check Intune compliance
│   │   │       │       Trusted location? → Check IP/location
│   │   │       │       Specific app? → Is the user using the right client?
│   │   │       ├─ Error 50126 → Invalid credentials → [see 01-01]
│   │   │       ├─ Error 50076 → MFA required but failed → [see 01-03]
│   │   │       └─ Error 65001 → App not consented → Admin must grant consent
│   │   │
│   │   └─ NO  → App has its own username/password
│   │       ├─ Does the user have an account in that application?
│   │       │   ├─ NO  → Request account creation per company process
│   │       │   └─ YES
│   │       ├─ Can the app administrator verify the account status?
│   │       │   └─ Account disabled/expired → App admin can re-enable
│   │       └─ Try: password reset for app-specific credentials
│   │
│   ├─ Does the app use group-based access?
│   │   ├─ YES → Is the user in the correct AD or Entra ID security group?
│   │   │   ├─ NO → Add user to the group (verify authorization first)
│   │   │   └─ YES → Allow sync time (groups may take 15-60 min to propagate)
│   │   └─ NO
│   │
│   └─ Does the app require a specific license?
│       └─ Check: Entra ID → Licenses → Assigned products
│           ├─ License not assigned → Assign appropriate license
│           └─ License assigned but not showing in app → Check service plan activation
│
├─ Can other users access the app without issue?
│   ├─ NO → Service outage → Check health dashboard or vendor status page
│   │       Escalate to app owner / vendor support
│   └─ YES → Issue is user-specific (continue above)
│
└─ Still blocked?
    └─ Clear browser cache/cookies → try private/incognito window
        → Try a different browser
        → Try from a different network (if possible)
        → Escalate to app owner with: user name, timestamp, error screenshot
```

**RESULT** → Access restored or escalation initiated with collected details.
