# Phishing Link Clicked

User clicked a link in a suspicious email and is worried they may have compromised their account or machine.

```
User reports: "I clicked a link in an email and now I'm worried"

├─ IMMEDIATE QUESTIONS
│   ├─ Did the user enter ANY credentials on the page after clicking?
│   │   ├─ YES → IMMEDIATE: Force password reset
│   │   └─ NO  → Continue
│   │
│   ├─ Did the user download or run any file?
│   │   ├─ YES → IMMEDIATE: Isolate machine (disconnect Wi-Fi + unplug Ethernet)
│   │   └─ NO  → Continue
│   │
│   └─ Has the user shared the link with anyone else?
│       ├─ YES → Notify Security — wider blast radius
│       └─ NO  → Contained to this user
│
├─ CONTAINMENT (do these FIRST)
│   ├─ **Password was entered**:
│   │   → Force password change immediately
│   │   → Revoke all sessions and tokens:
│   │       Entra ID → Users → Select user → Revoke sessions
│   │   → Require MFA re-enrollment
│   │
│   ├─ **File was downloaded/executed**:
│   │   → Disconnect machine from network NOW
│   │   → Do NOT reboot (preserves evidence)
│   │   → Escalate to Security team — possible malware
│   │
│   └─ **No credentials entered, no file downloaded**:
│       → Lower urgency, but still investigate
│       → Continue to investigation step
│
├─ INVESTIGATION
│   ├─ Check Entra ID sign-in logs for the user:
│   │   → admin.microsoft.com → Users → Sign-in logs
│   │   → Filter by user → Look for:
│   │   ├─ Successful sign-ins from unusual locations after the click time
│   │   ├─ Failed sign-ins from unusual locations
│   │   └─ Token refreshes from unexpected IPs
│   │
│   ├─ Check the actual link (do NOT click):
│   │   → Copy the URL → Send to Security for analysis
│   │   → Or: Use VirusTotal URL scanner
│   │
│   ├─ Check user's mailbox rules:
│   │   → Outlook Web → Settings → Rules → Forwarding
│   │   → Any suspicious auto-forwarding rules? → Malicious = compromised
│   │
│   └─ Check if the user has any unusual sign-ins:
│       → Look for: new device, new IP, new location
│
├─ RESOLUTION
│   ├─ **No compromise found**
│   │   → Educate user: always hover before clicking, check sender address
│   │   → Report the email using the company's reporting mechanism
│   │   → Log ticket → Close
│   │
│   ├─ **Credentials compromised but contained**
│   │   → Password reset done, sessions revoked, MFA re-enrolled
│   │   → Monitor for 48 hours for any suspicious activity
│   │   → Educate user → Close
│   │
│   └─ **File downloaded / potential malware**
│       → Machine was isolated
│       → Security team handles remediation
│       → Support role: provide user statement + timestamp → Hand off
│
└─ REMINDERS
    → Do NOT investigate independently beyond the steps above
    → Security incidents are handled by the Security team
    → Your job: recognize, contain, and escalate
    → Document everything in the ticket with exact timestamps
```

**RESULT** → Containment applied. Escalated to Security if needed. User educated.
