# Email Delivery Delays / Bounce Backs

User sent an email and it didn't arrive, or they received a delivery failure (NDR) / bounce-back message.

```
User reports: "I sent an email but they didn't get it" or "I got a bounce-back"

├─ READ THE BOUNCE MESSAGE
│   ├─ Look for the error code in the NDR (Non-Delivery Report)
│   │   ├─ **550 5.1.1** → User not found / recipient doesn't exist
│   │   │   → Check recipient email address spelling
│   │   │   → Is the recipient domain correct?
│   │   │   → If internal: check if the recipient exists in the GAL
│   │   ├─ **550 5.1.8** → Access denied (recipient can't receive from this sender)
│   │   │   → Mail flow rule or transport rule blocking
│   │   │   → Check Exchange admin center → Mail flow → Rules
│   │   ├─ **550 5.2.2** → Mailbox full
│   │   │   → Recipient's mailbox is over quota → Inform the recipient
│   │   ├─ **550 5.4.1** → Relay access denied
│   │   │   → Sending from a non-approved server/IP
│   │   │   → Check that the user's client is connecting to the right SMTP server
│   │   ├─ **550 5.7.1** → Message rejected by policy
│   │   │   → Anti-spam, DLP, or transport rule blocked it
│   │   │   → Check Exchange admin: Protection → Spam filter / DLP
│   │   ├─ **554 5.7.1** → Spam filter rejected (e.g., high spam confidence)
│   │   │   → Message was marked as spam → User can release from quarantine
│   │   │   → https://security.microsoft.com/quarantine
│   │   └─ **450 4.7.1** → TLS required but not available
│   │       → Recipient server requires encrypted connection
│   │       → Escalate to Exchange admin
│   │
│   └─ No NDR but recipient says they didn't get it?
│       → Check Message Trace in Exchange admin center
│       → https://admin.exchange.microsoft.com → Mail flow → Message trace
│       → Enter sender and recipient → Look at status:
│           ├─ "Delivered" → Message was delivered. Check recipient's junk/spam.
│           ├─ "Failed" → See failure reason
│           ├─ "Pending" → Still in queue → Check if there's a queue backlog
│           └─ "Filtered as spam" → Marked as spam → Release from quarantine
│
├─ INTERNAL DELIVERY (same organization)
│   ├─ Can sender send to other internal users?
│   │   ├─ NO → Sender's account may be blocked from sending
│   │   │   → Check: Exchange admin → Recipients → Mailboxes → Select user
│   │   │   → Mailbox features → "Recipient limit" or "Prohibit send"
│   │   └─ YES → Issue is between these two specific users
│   │       → Check if recipient has a rule auto-deleting from this sender
│   │       → Check if recipient has inbox forwarding (rule or ForwardingAddress)
│   │
│   └─ Check if a mail flow rule is intercepting:
│       → Exchange admin → Mail flow → Rules
│       → Look for rules that apply to this sender or subject pattern
│
├─ EXTERNAL DELIVERY (to another organization)
│   ├─ Is the sender's domain configured correctly?
│   │   → Check SPF, DKIM, DMARC records (via Exchange admin or DNS team)
│   │   → Without valid SPF, external servers may reject the email
│   ├─ Is the recipient domain rejecting due to reputation?
│   │   → The sending IP may be blacklisted
│   │   → Check MXToolbox IP reputation
│   └─ Is there a size limit issue?
│       → Attachment > 25 MB? → Use OneDrive/SharePoint link instead
│       → Entire message > 35 MB? → Exchange online limit is ~35 MB
│
└─ DELAYS (message was delivered but took a long time)
    ├─ Check message trace → "Deferrals" or "Retries" count
    │   └─ High deferral count → Recipient server was queuing
    ├─ Check if the user sent to a large distribution group
    │   → Large groups take longer to expand and deliver
    └─ Check service health → Exchange online may have a queue backlog
```

**RESULT** → Delivery issue identified and resolved, or escalated with message trace ID.
