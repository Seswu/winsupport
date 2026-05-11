# Password Reset / Forgotten Password

User cannot log in because they forgot their password or their password expired.

```
User reports: "I can't log in — my password doesn't work"

├─ Has the user tried the self-service password reset (SSPR) portal?
│   ├─ NO → Direct them to the SSPR URL
│   │        If SSPR is not available → Proceed below
│   └─ YES
│
├─ Verify the user's identity per company policy
│   ├─ Ask for: employee ID, manager name, or other approved identifier
│   ├─ Do NOT reset based on "it's me" alone — social engineering risk
│   └─ If identity cannot be verified → Escalate to manager verification
│
├─ Reset the password in the identity admin portal
│   ├─ Entra ID (admin.microsoft.com) → Users → Select user → Reset password
│   │   ├─ Option A: Auto-generate → User must change on next login
│   │   ├─ Option B: Set temporary password → Communicate securely (not email)
│   │   └─ Check: "Require password change on next sign-in" = YES (always)
│   └─ On-prem AD → Active Directory Users and Computers → Right-click → Reset Password
│
├─ After reset, does the user still can't log in?
│   ├─ Check if MFA is blocking (user may need MFA re-enrollment)
│   │   └─ [see 01-03](01-03_account-auth_mfa-not-working.md)
│   ├─ Check if the account is actually locked
│   │   └─ [see 01-02](01-02_account-auth_account-locked.md)
│   └─ Check if the user is typing the right domain\username format
│       └─ Try: domain\username or user@domain.com depending on setup
│
└─ All steps tried and still failing?
    └─ Check Entra ID sign-in logs for error details
        Escalate to Tier 2 with the sign-in log ID
```

**RESULT** → Password reset applied and communicated. User can log in.

**If password change isn't sticking** → Check if password writeback is enabled (hybrid environments). The on-prem password may be overriding the cloud change.
