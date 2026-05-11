# BitLocker Recovery Screen

The computer boots to a blue screen asking for the BitLocker recovery key instead of booting to Windows.

```
User reports: "My computer is asking for a BitLocker recovery key"

├─ WHY DID THIS HAPPEN?
│   ├─ Common triggers:
│   │   ├─ BIOS/UEFI change (Secure Boot disabled, boot order changed)
│   │   ├─ TPM firmware update (TPM measured different values)
│   │   ├─ Hardware change (new motherboard, RAM, SSD)
│   │   ├─ Boot configuration change (booted from USB, changed BCD)
│   │   └─ TPM was cleared/reset
│   │
│   └─ Ask the user: "Did you make any hardware or BIOS changes recently?"
│       → This helps prevent a recurrence
│
├─ ENTER THE RECOVERY KEY
│   ├─ Where to find the recovery key:
│   │   ├─ **Microsoft account** (if the user's device is registered):
│   │   │   → https://account.microsoft.com/devices → Select device → BitLocker recovery key
│   │   ├─ **Entra ID / Azure AD** (company-managed device):
│   │   │   → https://myaccount.microsoft.com → Devices → View recovery key
│   │   │   → Or admin: admin.microsoft.com → Devices → BitLocker recovery keys
│   │   ├─ **Saved to Microsoft account during setup** → Check the user's email
│   │   │   → Search inbox for "BitLocker recovery key"
│   │   ├─ **Active Directory** (on-prem domain):
│   │   │   → ADUC → Computer object → Properties → BitLocker Recovery tab
│   │   └─ **Printed or saved to file** → Did the user save it when BitLocker was enabled?
│   │       → Check Documents folder for "BitLocker Recovery Key.txt"
│   │
│   ├─ If you as admin need to retrieve it:
│   │   → Entra ID: admin.microsoft.com → Devices → Select device → "BitLocker recovery keys"
│   │   → On-prem: ADUC → Find computer → Properties → BitLocker Recovery tab
│   │   → Or PowerShell:
│   │       `Get-ADBitLockerRecoveryKey -ComputerName <name>` (requires RSAT)
│   │
│   └─ Enter the 48-digit recovery key (numeric, hyphenated)
│       → Type carefully — wrong key = wrong screen
│       → If the key doesn't work → You have the wrong key → Check again
│       → The key is unique per device
│
├─ AFTER SUCCESSFUL BOOT
│   ├─ **Suspend BitLocker before making future hardware changes**:
│   │   → Control Panel → BitLocker Drive Encryption → Suspend protection
│   │   → (Suspends means it resumes on next boot — use "Disable" for permanent)
│   │
│   ├─ **If a hardware change was planned**:
│   │   → Suspend BitLocker → Make the hardware change → Resume BitLocker
│   │
│   └─ **If the trigger is unknown**:
│       → The TPM may need to be re-initialized
│       → Run: `manage-bde -protectors -enable C:` (if disabled)
│       → Check Event Viewer → BitLocker-API → Management log
│
├─ BITLOCKER ADMIN CHECK
│   └─ If you can't find the recovery key:
│       → **Critical issue**: The machine may need to be reset/reimaged
│       → Check: Was the key escrowed to Entra ID / AD?
│           If not → The device was not properly configured
│           → User may lose data if the key is gone
│       → Escalate urgently
│
└─ KEY RECOVERY FOUND?
    ├─ YES → Enter key → Machine boots normally
    │   → Document the trigger (what caused the recovery screen)
    │   → Resume normal encryption → Confirm with `manage-bde -status`
    └─ NO  → Machine cannot boot without recovery key
        → Escalate to Tier 2 / Security team
        → Options: data recovery, reimage, or hardware decryption
```

**RESULT** → Machine booted normally. BitLocker protection intact.
