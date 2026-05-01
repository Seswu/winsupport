# Reading Material: Shadow & Scenario Practice

## 1. Why Shadowing Matters

Reading documentation and practicing in a VM gives you theoretical knowledge. Shadowing an experienced supporter gives you:
- **Context**: How the ticketing system works, how tickets are prioritized
- **Communication style**: How to talk to non-technical users
- **Triage judgment**: What to investigate first, what to skip, when to escalate
- **Tool usage**: What tools experienced supporters actually use vs. what docs say
- **Escalation paths**: Who to contact for what, how to escalate effectively
- **Unwritten rules**: Things that aren't documented but everyone knows

## 2. What to Observe During Shadowing

### Communication
- How do they greet users?
- How do they translate technical terms into plain language?
- How do they handle frustrated users?
- How do they confirm the issue is resolved before closing the ticket?

### Triage
- What questions do they ask first?
- How do they determine priority?
- What do they check first? (Service health? Event Viewer? Ask the user?)
- When do they decide to escalate?

### Tools
- What tools are on their desktop/bookmarks?
- What do they use the ticketing system for beyond logging?
- Do they use remote access tools? Which ones?
- What diagnostic commands do they run from memory?

### Documentation
- How detailed are their ticket notes?
- What information do they capture?
- How do they categorize and tag tickets?
- What templates or macros do they use?

## 3. Scenario Practice Methodology

### Why Practice Scenarios
Scenarios prepare you for the reality of support work:
- Multiple issues at once
- Vague user descriptions
- Time pressure
- The need to communicate clearly

### How to Practice

**Step 1: Set up the scenario**
- Configure your VM to exhibit a specific problem
- Or use a checklist to walk through a known issue

**Step 2: Time yourself**
- Goal: Diagnose and resolve the top 15 issues within 15 minutes each
- First attempts will take longer — that's expected

**Step 3: Write up the ticket**
- Document the issue, investigation, resolution, and any follow-up
- Write as if the user and your manager will both read it

**Step 4: Review**
- Did you follow the right diagnostic steps?
- Could you have been faster?
- Was your documentation adequate?

## 4. First-Contact Scripts

These are templates for responding to common user reports. Adapt them to your company's style.

### "My PC won't let me log in"
```
1. "What happens when you try? Do you see an error message?"
2. "Has this happened before, or is this the first time?"
3. "Did you recently change your password?"
4. "Are other people able to log in?"
5. If error says "account locked": "I'll unlock your account. Please try again in 2 minutes."
6. If error says "incorrect password": "Let me reset your password. I'll send you the temporary one."
7. If error says "trust relationship failed": "I need to repair the connection. This will take about 5 minutes."
```

### "Outlook won't connect"
```
1. "What do you see? Is there an error message?"
2. "Is Teams working? Can you access the web version of Outlook?"
3. If web works but desktop doesn't: "Let me check your Outlook profile."
4. Hold Ctrl > right-click Outlook tray icon > Connection Status. Check if connected.
5. If not connected: Check credentials, check AutoDiscover, rebuild profile if needed.
6. If everyone is affected: Check M365 Service Health.
```

### "My new computer is missing [application]"
```
1. "Which application is missing? Let me check if it's supposed to be on your machine."
2. Compare against the standard software list for their role.
3. If it should be there: Check Intune deployment status. Apps may still be installing.
4. If it's installing: "It's on the way. It can take up to 30 minutes to appear."
5. If it's not deployed: "Let me check your access. You may need to be added to the group."
6. If it needs manual install: "I'll deploy it remotely. You'll see it appear shortly."
```

### "OneDrive isn't syncing"
```
1. "What do you see on the OneDrive icon? Is there a red X?"
2. Click the OneDrive icon in the system tray. Check sync status.
3. If red X: Click it to see the error. Common: file in use, name conflict, storage full.
4. If stuck on "Processing changes": Reset OneDrive (onedrive.exe /reset).
5. If not running at all: Launch OneDrive from Start menu. Check if it signs in.
6. If sync is slow: Check file size, network speed, any large pending uploads.
```

### "My computer is really slow"
```
1. "When is it slow? All the time, or at specific times?"
2. "What were you doing when you noticed it?"
3. "When was the last time you restarted?"
4. Open Task Manager. Check CPU, Memory, Disk. Identify top consumer.
5. If disk at 100%: Check what's using it (Resource Monitor). Often Windows Update or indexing.
6. If memory high: Check for memory leaks. Recommend restart.
7. If startup is slow: Check startup apps. Disable unnecessary items.
8. Quick fix: "Let me restart your computer. This often resolves slowness."
```

## 5. Explaining Technical Concepts to Non-Technical Users

### Translation Guide

| Technical Term | User-Friendly Explanation |
|---|---|
| DNS | "The phonebook that translates website names to addresses" |
| Cache | "Temporary storage that speeds things up" |
| Driver | "Software that tells Windows how to talk to your hardware" |
| Reboot | "Turn it off and on again — this clears out stuck processes" |
| Profile | "Your personal settings and files on this computer" |
| Sync | "Making sure the files on your computer match the files in the cloud" |
| Authentication | "Proving who you are so the system lets you in" |
| Group Policy | "Settings that the IT department applies to all computers" |
| License | "Permission to use the software" |
| Server | "A computer that provides services to other computers" |

### Communication Best Practices
- Avoid jargon. Use analogies when technical terms are unavoidable.
- Confirm understanding: "Does that make sense?"
- Set expectations: "This will take about 10 minutes."
- Follow up: "Is everything working now?"
- Don't blame the user: "Sometimes this happens" instead of "You did X wrong."
- Be patient: What's obvious to you is not obvious to them.
