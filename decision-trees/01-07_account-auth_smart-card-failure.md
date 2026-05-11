# Smart Card / Certificate Authentication Failure

User cannot authenticate using their smart card or certificate, or the card reader isn't recognized.

```
User reports: "My smart card isn't working" or "Certificate not recognized"

├─ PHYSICAL CHECK
│   ├─ Is the smart card inserted correctly?
│   │   └─ Try re-seating the card
│   ├─ Is the card reader detected?
│   │   ├─ Check Device Manager → Smart card readers
│   │   │   ├─ No reader listed → Driver missing
│   │   │   │   → Install driver from manufacturer
│   │   │   │   → Try a different USB port
│   │   │   └─ Yellow exclamation → Driver issue
│   │   │       → Update or reinstall driver
│   │   └─ Is the reader's LED on/blinking?
│   │       └─ No light → Reader may be faulty
│   │           → Try a different reader
│   │
│   ├─ Is the card physically damaged (chip visible, bent)?
│   │   └─ YES → Card needs replacement → Contact certificate authority
│   └─ Does the user have a PIN?
│       ├─ NO  → Provide PIN setup instructions or refer to issuing authority
│       └─ YES → Continue
│
├─ CERTIFICATE CHECK
│   ├─ Open certmgr.msc → Personal → Certificates
│   │   └─ Is the user's certificate present?
│   │       ├─ NO  → Certificate not enrolled
│   │       │   → Check if certificate auto-enrollment is configured
│   │       │   → Run `gpupdate /force` then `certlm -enroll`
│   │       │   → If still missing → Escalate to CA admin for re-issuance
│   │       └─ YES
│   │           ├─ Is the certificate expired?
│   │           │   └─ Check "Valid from" and "Valid to" dates
│   │           │       → Expired → Renew certificate
│   │           └─ Is the certificate trusted by the relying system?
│   │               └─ Check that the issuing CA certificate is in Trusted Root store
│   │
│   └─ Run `certutil -scinfo` to verify card contents
│       └─ Error reading card → Card may be corrupted
│           → Try card on another reader (isolate card vs reader issue)
│
├─ DRIVER / SERVICE CHECK
│   ├─ Is the Windows Smart Card service running?
│   │   └─ `services.msc` → "Smart Card" → should be Running, Startup: Automatic
│   ├─ Is the reader driver up to date?
│   │   └─ Device Manager → Right-click reader → Update driver
│   └─ Has a recent Windows update affected smart card functionality?
│       └─ Check known issues: search "smart card Windows 11 [build number]"
│
└─ Still failing?
    └─ Try the card on a different machine
        ├─ Works on other machine → Reader/driver issue on this machine
        └─ Fails on all machines → Card is faulty → Request replacement
```

**RESULT** → Smart card authentication restored or replacement initiated.
