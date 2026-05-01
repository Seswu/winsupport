# Hands-On Exercises: Pharma Context & Compliance

These exercises are largely conceptual and discussion-based. They don't require a VM but require engagement with your team.

---

## Exercise 1: Map Your Environment

**Objective:** Understand your specific pharma IT environment.

1. Ask your team:
   - Which systems are GxP-validated? Get a list.
   - What is the change control process? Walk through a sample change request.
   - Where are the SOPs stored? Get access and read 2-3 relevant ones.
   - What endpoint security tools are deployed?
   - What is the network topology? Which networks exist?
2. Create a diagram or list of:
   - GxP systems you may support
   - Non-GxP systems you may support
   - The escalation path for each type
   - The change control workflow

---

## Exercise 2: Review Relevant SOPs

**Objective:** Familiarize yourself with the SOPs you'll follow.

1. Find and read the following SOPs (or their equivalents):
   - IT Support / Help Desk SOP
   - Incident Management SOP
   - Change Control SOP
   - Access Management SOP (user provisioning/deprovisioning)
   - Data Backup and Recovery SOP
   - Any SOP related to the specific systems you'll support
2. For each SOP, note:
   - What triggers it?
   - What are the steps?
   - Who is responsible for each step?
   - What documentation is required?
   - What is the escalation path?

---

## Exercise 3: Practice Documentation

**Objective:** Write a support ticket entry that would pass audit scrutiny.

Scenario: A user on a GxP-validated machine reports that they cannot access a shared drive. You investigate and find the drive mapping was broken by a recent policy change. You fix it by running `gpupdate /force` and re-mapping the drive.

Write the ticket documentation including:
- Problem description (as reported by user)
- Investigation steps taken (with timestamps)
- Root cause identified
- Resolution applied
- Verification that the fix worked
- Any follow-up actions needed (e.g., change request to fix the policy)

Have a colleague or your manager review it for adequacy.

---

## Exercise 4: Identify Escalation Scenarios

**Objective:** Practice recognizing when to escalate.

For each scenario, decide: Handle yourself, escalate, or investigate then escalate?

1. User reports their computer is running slowly and you find their antivirus is disabled
2. User on a lab PC can't open a specific application used for data analysis
3. User reports receiving a suspicious email with a link to "reset their password"
4. User's OneDrive is syncing files that appear to contain patient names and medical record numbers
5. A validated QC system shows a different software version than documented in the validation package
6. User can't print to a printer in the manufacturing area
7. User's BitLocker recovery screen appeared and they need the recovery key
8. You notice multiple failed login attempts for a user account from an unusual IP address

Discuss your answers with your team.
