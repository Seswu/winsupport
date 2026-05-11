# Account Locked Out

User cannot log in and sees a message that their account is locked or disabled.

```
User reports: "My account is locked — I can't log in anywhere"

├─ CONFIRM: Is the account actually locked?
│   ├─ Entra ID: Sign-in logs → Filter by user → Look for Event ID 50053 (lockout)
│   ├─ On-prem AD: `net user <username> /domain` → check "Account Active" and "Locked"
│   └─ If NOT locked → [see 01-04](01-04_account-auth_cant-login.md)
│
├─ IDENTIFY THE SOURCE of lockout attempts
│   ├─ Check Entra ID sign-in logs → "Sign-in error code" = 50053, 50055, 50056, 50057
│   │   └─ Look at the "Location" and "Application" columns
│   │       Which app/device is hammering the password?
│   │
│   ├─ Check on-prem AD: Look at the PDC emulator's Security log, Event ID 4740
│   │   └─ Note the "Caller Computer Name" — that's the source
│   │
│   └─ Common lockout sources:
│       ├─ Stored credentials in Credential Manager (old password cached)
│       ├─ Mobile device mail profile (wrong password saved)
│       ├─ Mapped network drive with cached credentials
│       ├─ Scheduled task running under user context
│       └─ VPN client trying to auto-connect with old password
│
├─ UNLOCK the account
│   ├─ Entra ID: Admin portal → Users → Select user → "Unlock" or "Enable sign-in"
│   └─ On-prem AD: ADUC → Right-click user → Account tab → Uncheck "Account is locked out"
│       Or: `net user <username> /active:yes /domain`
│
├─ RESOLVE THE ROOT CAUSE (prevent re-lockout)
│   ├─ Source is a mobile device → Update the password in the mail app
│   ├─ Source is Credential Manager → Open `control keymgr.dll` → Remove stale entries
│   ├─ Source is a mapped drive → `net use * /delete` → Remap with new password
│   └─ Source is unknown → Enable lockout logging for more detail
│       Or: Set a longer lockout threshold (if policy allows)
│
└─ User still can't log in after unlock?
    └─ [see 01-01](01-01_account-auth_password-reset.md) — forced password reset + MFA re-enrollment
```

**RESULT** → Account unlocked, source identified, re-lockout prevented.
