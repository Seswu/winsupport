# Antivirus Flagging Legitimate Software

Antivirus (Windows Defender or third-party) is blocking or quarantining a file that the user knows is safe.

```
User reports: "My antivirus is blocking a program I need to use"

├─ CONFIRM IT'S A FALSE POSITIVE
│   ├─ Is this a company-approved application?
│   │   ├─ YES → Likely a false positive
│   │   └─ NO  → User downloaded something → Verify the file source
│   │
│   ├─ Check the file:
│   │   → Right-click the file → Properties
│   │   ├─ Digital signatures tab → Is it signed by a legitimate publisher?
│   │   └─ Details tab → Original filename, file version, product name
│   │
│   ├─ Check the file on VirusTotal:
│   │   → https://www.virustotal.com → Upload the file or hash
│   │   ├─ 0/60 detection → Likely clean (false positive from one AV)
│   │   ├─ 1-3/60 detection → May be a false positive
│   │   └─ > 5/60 detection → Likely actually malicious
│   │
│   └─ Ask: Did you download this from the official vendor website?
│       → If not → Might be a trojan disguised as legitimate software
│
├─ RESTORE THE FILE
│   ├─ **Windows Defender**:
│   │   → Windows Security → Virus & threat protection
│   │   → Protection history → Find the quarantined item
│   │   → Click "Actions" → "Restore"
│   │   → (This also adds an exclusion automatically if you choose that option)
│   │
│   ├─ **Third-party antivirus** (Symantec, McAfee, CrowdStrike, etc.):
│   │   → Open the AV console → Quarantine → Restore
│   │   → Process varies by vendor — check the software
│   │
│   └─ **If file was deleted**:
│       → Re-download from the official source
│       → Add exclusion before downloading again
│
├─ ADD AN EXCLUSION
│   ├─ **Windows Defender**:
│   │   → Windows Security → Virus & threat protection → Manage settings
│   │   → Exclusions → Add or remove exclusions
│   │   → Add exclusion for:
│   │       ├─ **File**: The specific .exe or .dll
│   │       ├─ **Folder**: The installation folder
│   │       ├─ **File type**: .exe, .dll, etc. (use sparingly)
│   │       └─ **Process**: The running process name
│   │
│   ├─ **Third-party AV**: Add the file/folder to the AV's exclusion list
│   │
│   └─ Caution: Only add exclusions for files you're certain are safe
│       → Overly broad exclusions reduce security
│
├─ REPORT FALSE POSITIVE (Windows Defender)
│   └─ Submit the file to Microsoft for analysis:
│       → https://www.microsoft.com/en-us/wdsi/filesubmission
│       → Select: "Microsoft Defender is incorrectly blocking a file"
│       → Upload the file → Submit
│       → This helps prevent the same false positive for others
│
├─ THIRD-PARTY AV ISSUES
│   └─ If corporate-managed AV (CrowdStrike, Defender for Endpoint):
│       → You may NOT be able to add exclusions (managed by policy)
│       → File a request through the proper channel
│       → Contact Security team: "This legitimate app is being blocked"
│
└─ SERVICE DESK NOTE
    → If this is a company-approved app being blocked by corporate AV:
      It may be a new threat detection the Security team needs to know about
      → Escalate: "App X is being flagged by AV, please verify if it's a false positive"
      → Do NOT disable AV for the entire machine
```

**RESULT** → File restored and excluded, or escalated for policy review.
