# Account Disabled / Expired

User cannot log in and sees "account disabled," "account expired," or their access was cut off unexpectedly.

```
User reports: "My account stopped working" or "I can't access anything"

├─ CONFIRM: Is the account disabled or expired?
│   ├─ Entra ID: Admin portal → Users → Select user → Check "Account enabled"
│   │   └─ Look at "Sign-in status" in the user detail pane
│   ├─ On-prem AD: ADUC → Right-click → Account tab
│   │   └─ Check "Account is disabled" and "Account expires"
│   └─ If account is active → [see 01-04](01-04_account-auth_cant-login.md)
│
├─ Why was the account disabled?
│   ├─ Expired contractor/temporary account (expiration date reached)
│   │   ├─ Check the user's contract end date
│   │   └─ If contract was extended → Extend the expiration date in AD
│   │       On-prem: ADUC → Account tab → "Account expires" → set new date
│   │       Cloud: Entra ID → Users → Properties → Usage location / Employment details
│   │
│   ├─ Offboarding process was triggered in error
│   │   ├─ Check with HR: was this user supposed to be offboarded?
│   │   │   ├─ YES → Do NOT re-enable. Verify with manager.
│   │   │   └─ NO  → HR confirms error → Re-enable account
│   │   │           Check if groups/licenses were also removed → restore them
│   │   └─
│   ├─ Inactivity / stale account policy
│   │   └─ Check last sign-in date → Re-enable if user is active
│   │
│   ├─ Security incident — account compromised
│   │   └─ Do NOT re-enable. Escalate to Security team.
│   │
│   └─ Unknown / no clear reason
│       └─ Check recent audit logs — who changed the account and why?
│           Entra ID: Audit logs → filter by user → look for "Disable account"
│
├─ RE-ENABLE the account
│   ├─ Entra ID: Admin portal → Users → Select user → Enable sign-in
│   └─ On-prem AD: ADUC → Right-click → Account tab → Uncheck disabled
│       Or: `net user <username> /active:yes /domain`
│
└─ Post-enable checks:
    ├─ Were group memberships removed? → Re-add user to security groups
    ├─ Were licenses removed? → Re-assign (Entra ID → Licenses)
    ├─ Was the mailbox disabled? → Re-enable in Exchange admin center
    └─ Was the user's manager notified that access is restored?
```

**RESULT** → Account re-enabled. Root cause (HR error, stale account, etc.) documented in ticket.
