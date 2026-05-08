# Suggested Template Documents to Create

This document lists potential template documents that a new support hire can create for themselves as they learn the company. Each entry describes the template, why it is useful, what to fill in, and includes a brief example or excerpt.

---

## 1. Team & Escalation Contact Card

**What it is:** A single-page reference card with names, roles, contact information, and escalation triggers for every team and person you may need to contact.

**Why create it:** When a P1 incident hits, you should not be searching through email or Slack to find the right person. A printed or pinned card gives you answers instantly.

**What to fill in:**

| Column | Description |
|---|---|
| Role / Team | Team name or person's title |
| Name | Person's name |
| Contact | Phone, email, Slack, Teams |
| When to escalate | Specific conditions that trigger escalation |

**Example excerpt:**

| Role / Team | Name | Contact | When to escalate |
|---|---|---|---|
| Service Desk Lead | Jane Smith | jane@company.com / ext 210 | SOP questions, scheduling |
| Tier 2 (Windows) | Bob Chen | bob@company.com / ext 211 | Any issue unresolved after 30 min |
| Security On-Call | security@company.com | PagerDuty | Malware, phishing, account compromise |
| Infrastructure On-Call | infra@company.com | PagerDuty | Server down, network outage |
| QA / Compliance | compliance@company.com | Teams channel #qa-support | GxP system changes, change control |

**Format:** Print as a half-sheet (A5) and keep near your monitor.

---

## 2. Common Ticket Types & First-Contact Scripts

**What it is:** A document listing the 5–10 most frequent ticket types at your company, with a structured first-contact script for each.

**Why create it:** Your first response sets the tone. A consistent, professional script ensures you gather the right information, project confidence, and do not miss steps.

**What to fill in:**

- Ticket type name
- Typical user complaint (how the user describes it)
- Information to gather (fields, logs, screenshots)
- First-contact response script (what you say to the user)
- Initial triage steps (what to check before escalating)
- Common resolutions (when applicable)

**Example excerpt:**

```
Ticket Type: "I can't log in / password not working"

User says: "My password stopped working" or "I'm locked out"

Gather:
  - Username (not just display name)
  - Error message (exact wording)
  - Which system (Windows login, Outlook, VPN, specific app)
  - Has self-service password reset been tried?

Script (phone):
  "I'm sorry you're having trouble logging in. Let me start by
  verifying your identity. Can you confirm your full name and
  employee ID? ... Thank you. Can you tell me — which system
  are you trying to log into, and what error message are you seeing?"

Script (ticket / chat):
  "Hi [name], sorry you're running into login issues.
  To help you as quickly as possible, could you let me know:
  1. Which system or application are you trying to access?
  2. What exact error message do you see?
  3. Have you tried the self-service password reset yet?"

Initial triage:
  1. Check if account is locked (Entra ID sign-in logs)
  2. Check if password expired
  3. Check if MFA is working
  4. Escalate if account appears compromised
```

---

## 3. System Inventory Quick Reference

**What it is:** A table of every major system or application you may encounter in tickets, with key attributes.

**Why create it:** When a user says "the LIMS is down" or "Qualio won't load," you need to know in seconds: Is this GxP? Who owns it? Can I touch it?

**What to fill in:**

| Column | Description |
|---|---|
| System name | Common name and full name |
| Purpose | What it does |
| GxP status | Yes / No (and which: GMP, GLP, GCP) |
| Owner / team | Who manages it |
| Support tier | Who handles tickets (Tier 1, Tier 2, vendor) |
| Common issues | Recurring problems |
| Notes | What NOT to do |

**Example excerpt:**

| System | GxP? | Owner | Support tier | Common issues | Notes |
|---|---|---|---|---|---|
| Qualio (QMS) | Yes (GMP) | QA | Tier 2 / QA | "Can't log in," "CAPA entry stuck" | Never modify CAPA entries yourself |
| LabWare (LIMS) | Yes (GLP) | Lab IT | Tier 2 / Vendor | "Instrument not uploading," "Can't print labels" | Lab network only; must be on-site or use jump box |
| Microsoft 365 | No | Infrastructure | Tier 1/2 | "Outlook stuck," "OneDrive not syncing" | Can troubleshoot freely |
| SAP Business One | Yes (GMP) | ERP Admin | Tier 2 / Vendor | "Can't post batch," "Print pick list" | Escalate immediately; no direct access for Tier 1 |

---

## 4. First Week Checklist

**What it is:** A concrete, checkbox-style list of tasks to complete during your first week. Each item has a clear "done" condition.

**Why create it:** Onboarding is overwhelming. A checklist prevents you from forgetting critical access requests, introductions, or setup tasks.

**What to fill in:**

- Task description
- Who to contact
- Done condition (how to confirm it is complete)
- Priority (high/medium/low)
- Deadline

**Example excerpt:**

```
[ ] Request ticketing system access
    → Contact: Service Desk Manager
    → Done: Can log in and see the ticket queue
    → Priority: HIGH (needed Day 1)
    → Deadline: Before first shift

[ ] Set up MFA on corporate phone
    → Contact: Security team
    → Done: MFA prompt works on test login
    → Priority: HIGH (needed Day 1)
    → Deadline: Before first shift

[ ] Read Change Control SOP
    → Location: QMS (search "SOP-IT-001 Change Control")
    → Done: Can explain when to open a change request
    → Priority: HIGH
    → Deadline: End of Day 2

[ ] Meet with QA/Compliance lead
    → Contact: Schedule 30-min intro meeting
    → Done: Understand which systems are GxP and what that means for support
    → Priority: MEDIUM
    → Deadline: End of Week 1

[...]
```

---

## 5. Company Acronyms Glossary

**What it is:** A running list of acronyms and abbreviations used internally, with their full forms and a brief explanation.

**Why create it:** Pharmaceutical companies are dense with acronyms. During your first weeks, you will hear terms like "LIMS," "EQMS," "CAPA," "MES," "ELN," "ERP," "CSV," "QMS," "GMP," "GLP," "GCP," "ALCOA," "PIM," "PAM," and many more. A living glossary prevents confusion.

**What to fill in:**

| Acronym | Full form | What it is (in one sentence) |
|---|---|---|

**Example excerpt:**

| Acronym | Full form | What it is |
|---|---|---|
| LIMS | Laboratory Information Management System | System for tracking lab samples, tests, and results |
| ELN | Electronic Lab Notebook | Digital replacement for paper lab notebooks |
| EQMS / QMS | (Electronic) Quality Management System | System for managing quality documents, audits, CAPAs |
| CAPA | Corrective and Preventive Action | Formal process to fix and prevent quality issues |
| MES | Manufacturing Execution System | System that controls and tracks manufacturing on the plant floor |
| ALCOA+ | Attributable, Legible, Contemporaneous, Original, Accurate (+ Complete, Consistent, Enduring, Available) | Data integrity principles that govern all GxP data |
| CSV | Computer System Validation | Process of proving a computer system does what it is supposed to do |
| PIM | Privileged Identity Management | Just-in-time admin access (Microsoft Entra PIM) |

---

## 6. Decision Flowcharts for Common Tickets

**What it is:** Text-based decision trees for the most common or most critical ticket types. Each flowchart helps you quickly determine whether to handle, escalate, or change-request.

**Why create it:** In the moment of a live ticket, you do not want to think through the full decision tree from scratch. A printed flowchart gives you confidence and consistency.

**What to fill in:** For each ticket type, a simple text decision tree:

**Example excerpt (triage for an app crash on a lab machine):**

```
User reports: "App X is crashing"

  ├── Is this machine on the lab network (VLAN 50)?
  │     ├── YES → Ask: Are other lab apps working?
  │     │         ├── YES → Likely app-specific issue
  │     │         │         ├── Is the app on the validated software list?
  │     │         │         │     ├── YES → Escalate to System Owner (no reinstall without change control)
  │     │         │         │     └── NO  → Refer to user's manager; not a standard app
  │     │         └── NO  → Possible network issue → check lab network connectivity
  │     └── NO  → Is this a corporate machine?
  │               ├── YES → Try standard app troubleshooting (reinstall, repair, etc.)
  │               └── NO  → Check machine type → escalate if unsure
  │
  └── Always: Document in ticket. Note that app was crashing. Include error code if available.
```

---

## How to Use These Templates

1. **Start with the Contact Card and Acronyms Glossary** — they are small and immediately useful.
2. **Fill in the System Inventory Quick Reference** during your first week as you discover systems.
3. **Draft the First-Contact Scripts** after you have handled your first 5–10 tickets of each type. Do not try to write them from scratch before taking tickets.
4. **Build the Decision Flowcharts** for the ticket types you find yourself escalating or asking about most often.
5. **Keep all of these documents in a single place** — a personal knowledge base, a document folder, or a pinned note in your ticketing tool. Update them as you learn.
6. **Share them** — if your team does not already have these, your templates could become the starting point for a shared knowledge base.
