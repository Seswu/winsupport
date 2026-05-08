# Critical Unknowns: Questions to Navigate the Company

This document is organized into three phases: questions to answer before you start, during your first week, and as ongoing discovery. Each entry includes the question, why it matters, who to ask, and what to do with the answer.

## Phase 1: Before Day 1

These are practical blockers. Without these answers, you cannot do your job on day one.

### 1. What ticketing system do we use?

- **Why:** You need access, credentials, and familiarity with the interface before taking tickets.
- **Who:** Service Desk Manager or team lead.
- **What to do:** Request access, install any required agent/plugin, find the knowledge base within the tool, note escalation queues.

### 2. What remote access tools are approved?

- **Why:** You cannot help users remotely without knowing which tool is standard (TeamViewer, BeyondTrust, Quick Assist, native RDP). Some tools require licenses or elevation.
- **Who:** Infrastructure team or Service Desk Manager.
- **What to do:** Install the approved tool, verify it works with your credentials, learn the basic controls.

### 3. Where are existing runbooks and SOPs stored?

- **Why:** You need to find the documented procedures before you need them. Shadowing a colleague is not a substitute for reading the SOPs.
- **Who:** Service Desk Manager or any team member.
- **What to do:** Bookmark the location (SharePoint, Confluence, network share, QMS), check that you have read access, read 2–3 SOPs before your first shift.

### 4. What credentials and accounts do I need before I start receiving tickets?

- **Why:** Many systems require separate accounts — ticketing tool, remote access, admin account (separate from corporate), VPN, MFA enrollment, shared mailboxes.
- **Who:** Service Desk Manager or IT Operations.
- **What to do:** Make a checklist. Confirm each one is provisioned before your first day. Note onboarding lead times (some accounts take 2–5 days).

### 5. What is the shift schedule / on-call rotation?

- **Why:** Know when you are expected to be available and whether there is an on-call rotation. Also note time zones for any remote colleagues or sites.
- **Who:** Service Desk Manager or team lead.
- **What to do:** Add to calendar, set up phone/email forwarding if on-call, confirm after-hours escalation paths.

### 6. What is the handover process between shifts?

- **Why:** Open tickets, ongoing issues, and critical situations need to be handed over clearly. The format (email, ticket notes, verbal) varies by team.
- **Who:** Service Desk Manager or any team member.
- **What to do:** Identify the expected format, observe a handover on your first day, adopt the same structure.

---

## Phase 2: First Week

These build your mental map of the organization and its systems.

### 7. Can I get an organizational chart for IT?

- **Why:** You need to know who to escalate to: Tier 2 team members, the Service Desk Manager, Infrastructure lead, Security lead, QA/Compliance lead. Names and faces matter.
- **Who:** Service Desk Manager or HR / IT Operations.
- **What to do:** Save the org chart somewhere accessible. Note which people handle which domains. Identify your direct escalation POCs for each area.

### 8. Which systems are GxP-validated and which are not?

- **Why:** This is the single most important distinction in pharma IT. A fix on a GxP system requires change control; the same fix on a non-GxP system might be routine. The wrong action can trigger an audit finding.
- **Who:** QA/Compliance team or the System Owner for each application.
- **What to do:** Create a list or spreadsheet: system name → GxP status (Y/N) → GxP category (GMP/GLP/GCP) → System Owner → what to do if it breaks. Save it as your primary reference.

### 9. How do I tell if a specific machine is GxP-validated?

- **Why:** A ticket about "can't print" on a GxP lab workstation is handled differently than the same issue on a corporate laptop. You need a way to determine this at a glance.
- **Who:** Infrastructure team or Computer System Validation group.
- **What to do:** Look for machine naming conventions, AD/Entra ID groups, asset tags, or Intune device categories. Ask if there is a CMDB or asset system to check. Add these heuristics to your reference sheet.

### 10. What is the change control workflow, and when do I trigger it?

- **Why:** If you make an unauthorized change on a validated system, it becomes a compliance incident. You need to know the exact boundary between "fix it" and "open a change request."
- **Who:** QA/Compliance or your manager.
- **What to do:** Get the Change Control SOP. Read it. Walk through a sample request with a colleague. Note the emergency change process (verbal approval path). Keep a quick decision flowchart pinned to your desk (see Reference Material: "Can I Do This?").

### 11. Who are the system owners for the major applications?

- **Why:** When a ticket escalates beyond your scope, you need to know exactly who to hand it to — not "the LIMS team" but a specific person or group.
- **Who:** QA/Compliance or Director of Business Applications.
- **What to do:** Build a contact table: Application → System Owner → Backup Contact → Escalation Email/Distribution List. Keep it updated as people change roles.

### 12. What endpoint security tools are installed on company machines?

- **Why:** Antivirus/EDR (CrowdStrike, Defender, SentinelOne), DLP agents, application whitelisting (Airlock, Avecto), and disk encryption (BitLocker) all interact with troubleshooting. A "can't install app" issue might be a whitelisting block, not a technical failure.
- **Who:** Security team or Infrastructure.
- **What to do:** Learn the agent names and icons. Know which ones can interfere with software installation, USB access, or script execution. Add the security team's contact to your escalation list.

### 13. What are the most common ticket types at this company?

- **Why:** The frequency distribution of tickets determines where you should focus your learning first. A company where password resets are 40% of tickets needs a different approach than one where new machine provisioning dominates.
- **Who:** Service Desk Manager or run a report from the ticketing tool.
- **What to do:** Get the top 10 ticket types and their frequency. Compare against the 15 common issues you already know. Identify the top 3 that are specific to this company and prioritize learning those workflows.

### 14. Is there a lab or test environment for practice?

- **Why:** Hands-on practice without breaking production systems is essential. A test environment for provisioning, software deployment, and policy testing lets you learn safely.
- **Who:** Infrastructure or Endpoint Engineering.
- **What to do:** Request access. Find out if it is always available or needs to be scheduled. Ask if there are pre-built test scenarios.

---

## Phase 3: Ongoing Discovery

These questions do not need answers on day one, but should be discovered over the first 30–60 days.

### 15. What is the informal escalation path?

- **Why:** The org chart shows one structure. The reality is often different — certain people have deeper expertise despite their title, others prefer written tickets over verbal pings, and some managers want to be CC'd on everything.
- **Who:** Observation and colleagues.
- **What to do:** Pay attention to how senior support staff route tickets. Which people get looped in for certain issues? Who is the "go-to" for each domain? Map the informal structure alongside the formal one.

### 16. What audits have happened recently, and what findings were raised?

- **Why:** Audit findings reveal systemic weaknesses and areas where support processes failed. They also show what regulators and QA care about most at this company.
- **Who:** QA/Compliance, or review past audit documentation if accessible.
- **What to do:** Read the findings. Note any that relate to IT support (e.g., incomplete ticket documentation, delayed changes, access control gaps). Use these as case studies for your own behavior.

### 17. What is the history of the company's IT infrastructure? What major migrations or changes are coming?

- **Why:** Context explains current pain points. If the company recently migrated from on-prem Exchange to Exchange Online, Outlook issues may be related to lingering profile problems. If a manufacturing site is being added, new network segments will appear.
- **Who:** Infrastructure team or Director of IT Operations.
- **What to do:** Ask about the last 12 months of major IT changes and the next 6 months of planned changes. Note these on a timeline to understand recurring ticket patterns.

### 18. How is success measured for support staff?

- **Why:** Different teams value different metrics: ticket volume, first-call resolution, customer satisfaction scores, adherence to SOPs, escalation rate. Knowing what is measured helps you prioritize.
- **Who:** Service Desk Manager.
- **What to do:** Understand the metrics. Ask what the team's current performance is against those targets. If the target is "95% of tickets resolved within 4 hours" vs. "100% of tickets documented per SOP," the daily focus is different.

### 19. What communication channels are used for urgent issues?

- **Why:** A major outage or security incident may not go through the ticketing system. Know the channels: Slack channel, Teams group, email distribution list, phone tree, or SMS.
- **Who:** Service Desk Manager or Infrastructure.
- **What to do:** Join the relevant channels. Configure notifications. Know the protocol: for a manufacturing outage, who says what to whom and in what order.

### 20. What is the relationship between IT and the business units?

- **Why:** In pharma, every business unit (R&D, Manufacturing, Quality, Clinical) has different priorities, regulatory pressures, and tolerance for downtime. Understanding these relationships helps you communicate appropriately.
- **Who:** Observation, your manager, and the business unit IT liaisons if they exist.
- **What to do:** Identify which business units feel well-served by IT and which do not. Know which sites or departments have the most critical uptime requirements. Tailor your communication style accordingly (e.g., a lab manager during an audit needs different language than a sales rep during quarterly close).

### 21. Who holds institutional knowledge that is not documented?

- **Why:** Every company has unwritten knowledge: the person who knows why a certain server is named oddly, the one who remembers why a specific policy was created, the admin who knows every lab instrument's quirks. These people are invaluable but may leave without warning.
- **Who:** Observation and colleagues.
- **What to do:** Identify these people. Ask them questions respectfully. If they share undocumented knowledge, write it down and offer to turn it into a documented SOP. Protect their time — they are often the most overloaded.

### 22. What does the onboarding process look like for new employees?

- **Why:** You may be expected to support new hires during onboarding. Understanding the process end-to-end (HR trigger → account creation → hardware provisioning → application access → training) helps you troubleshoot when something goes wrong.
- **Who:** Service Desk Manager or HR IT.
- **What to do:** Walk through the onboarding process step-by-step. Identify common failure points (e.g., "users often don't get their ERP access until day 3"). Create a quick-reference checklist for diagnosing new hire issues.

### 23. What are the most frequent self-inflicted wounds?

- **Why:** Every company has recurring issues caused by internal processes, not external factors: a provisioning script that breaks monthly, a group policy that conflicts with a critical app, a known compatibility issue after every Patch Tuesday.
- **Who:** Tier 2 team members, historical ticket analysis.
- **What to do:** Ask the senior support staff: "What breaks regularly that we already know the fix for?" Document these in your personal runbook. If they are not already in the knowledge base, consider adding them after consulting your manager.

### 24. What are the boundaries of my authority?

- **Why:** Knowing what you are allowed to do without approval prevents both overstepping and unnecessary delays. Some teams allow Tier 1 to reset any password except admin accounts; others restrict password resets to Tier 2.
- **Who:** Service Desk Manager or SOPs.
- **What to do:** Get explicit boundaries for common actions: password resets, group membership changes, software installations, registry modifications, service restarts, remote access sessions, and file access.
