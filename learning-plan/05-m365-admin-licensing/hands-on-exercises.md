# Hands-On Exercises: Microsoft 365 Admin & Licensing

Requires access to an M365 tenant (Developer tenant recommended).

---

## Exercise 1: Navigate the Admin Centers

**Objective:** Familiarize yourself with all admin portals.

1. Go to https://admin.microsoft.com and sign in with your admin account
2. Explore the left navigation. Click "Show all" to see the full list
3. Visit each specialized admin center:
   - Entra Admin Center
   - Intune Admin Center
   - Exchange Admin Center
   - Teams Admin Center
   - SharePoint Admin Center
4. In each portal, identify the key sections and what they manage
5. Bookmark all portals in your browser

---

## Exercise 2: Service Health Check

**Objective:** Learn to check if an issue is a service-wide outage.

1. In M365 Admin Center, go to Health > Service health
2. Review the status of each service
3. Click into any service with an advisory or incident
4. Read the incident details:
   - When did it start?
   - What is the user impact?
   - What is Microsoft doing?
5. Subscribe to updates for an incident (if any are active)
6. Check Message Center: Health > Message center
7. Filter by service and note any upcoming changes

---

## Exercise 3: User and License Management

**Objective:** Practice managing users and assigning licenses.

1. Go to Users > Active users
2. Click on a user account
3. Examine the user details pane:
   - Account info (name, email, UPN)
   - Licenses and apps
   - Groups
   - Sign-in status
4. Click "Licenses and apps"
   - Note which licenses are assigned
   - Note which service plans are enabled within each license
   - Toggle a service plan off and on (in a test account)
5. Create a new test user:
   - Click "Add a user"
   - Fill in details
   - Assign a license
   - Complete the wizard
6. Delete the test user when done (or keep for other exercises)

---

## Exercise 4: Sign-in Log Investigation

**Objective:** Use sign-in logs to diagnose authentication issues.

1. Go to Entra Admin Center > Identity > Users > All users
2. Select a user > Sign-in logs
3. Review recent sign-in attempts:
   - Successes and failures
   - Date, time, IP address, location
   - Application used (Office, Teams, browser, etc.)
4. Click on a failed sign-in to see details:
   - Error code and description
   - Conditional Access evaluation
   - MFA details
   - Device information
5. Filter by date range, status (failure), or application

---

## Exercise 5: Conditional Access Exploration

**Objective:** Understand how Conditional Access policies affect users.

1. Go to Entra Admin Center > Protection > Conditional Access > Policies
2. Review existing policies (if any)
3. Note the structure of a policy:
   - Assignments (who it applies to, which apps, which conditions)
   - Access controls (grant: allow/block, require MFA, require compliant device)
   - Session controls (sign-in frequency, app enforcement)
4. Use the "What If" tool:
   - Click "What If" in the Conditional Access menu
   - Enter a user and sign-in properties
   - See which policies would apply and what the result would be
5. This is critical for understanding why a user might be blocked from accessing something

---

## Exercise 6: Intune Device Check

**Objective:** Learn to check device status in Intune.

1. Go to Intune Admin Center > Devices > All devices
2. Find a device (or your VM if enrolled)
3. Click the device to see details:
   - Device properties (OS, version, serial)
   - Compliance status
   - Detected apps
   - Configuration profiles applied
   - Last check-in time
4. Check compliance: click "Check compliance status"
5. Look at deployed apps and their installation status
6. Note available remote actions (sync, restart, etc.)
