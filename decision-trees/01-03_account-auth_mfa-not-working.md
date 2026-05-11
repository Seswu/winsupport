# MFA Not Working

User cannot complete multi-factor authentication — no prompt, app not working, or device lost.

```
User reports: "I'm not getting the MFA prompt" or "I got a new phone"

├─ Which MFA method is the user trying?
│   ├─ Authenticator app (Microsoft Authenticator)
│   │   ├─ Is the app installed and set up?
│   │   │   ├─ NO → Guide user through setup:
│   │   │   │   1. Install Microsoft Authenticator (phone app store)
│   │   │   │   2. Go to https://aka.ms/mfasetup on their computer
│   │   │   │   3. Sign in → Security info → Add sign-in method → Authenticator app
│   │   │   │   4. Scan the QR code with the phone app
│   │   │   └─ YES → {
│   │   │       ├─ Are they getting the "approve" notification at all?
│   │   │       │   ├─ NO → Check phone notifications are enabled for Authenticator
│   │   │       │   │       Check that time sync is correct on the phone
│   │   │       │   │       → Resync: Settings → Accounts → "Repair" or re-add
│   │   │       │   └─ YES → They're approving but it fails → {
│   │   │       │       ├─ Is number matching enabled? (number on screen must match app)
│   │   │       │       └─ Try the one-time password code (OTP) from the app instead
│   │   │       │   }
│   │   │       └─ }
│   │   └─
│   ├─ Phone call or SMS
│   │   ├─ Can they receive calls/texts?
│   │   │   ├─ NO → Check if phone number is correct in Security info
│   │   │   │       Check if "Phone call" or "SMS" is even an allowed method
│   │   │   │       → May need to add a different method
│   │   │   └─ YES → Try again — sometimes the provider is slow
│   │   └─
│   ├─ Hardware token (YubiKey, etc.)
│   │   └─ Is the token registered in the user's security info?
│   │       ├─ YES → Try re-inserting, check driver, try a different USB port
│   │       └─ NO  → Must be registered by an admin in the portal
│   │
│   └─ User has lost their phone / device
│       └─ IMMEDIATE: Block sign-in sessions temporarily (security risk)
│           → Admin portal: Users → Select user → Revoke sessions
│           → User must re-enroll MFA from a trusted device or with admin assistance
│           → Require identity verification before issuing temp access
│
├─ User can't access https://aka.ms/mfasetup because they need MFA to log in?
│   └─ Admin-assisted reset: Entra ID → Users → Select user → Authentication methods
│       → Delete existing MFA methods → User will be prompted to re-register on next login
│
└─ MFA works on some apps but not others?
    └─ Check if the app supports modern auth (legacy protocols don't support MFA)
        → [see 02-02](02-02_email-collab_outlook-password-prompt.md)
```

**RESULT** → MFA is functional or user has been guided through re-enrollment.
