# Microsoft Entra ID Lab — Conditional Access

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft Lab:** [Perform basic Conditional Access policy tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/06-perform-basic-conditional-access.html)

---

## Overview

In this lab, I created and tested a Microsoft Entra **Conditional Access** policy.

The objective was to block a selected user from accessing **Microsoft Sway** and validate the expected result using the **What If** analysis tool before enforcement.

For my fictional `ExposedCybersecurity` tenant, I used:

```text
Policy: CA-Block-Sway-ChrisGreen
User: Chris Green
Application: Microsoft Sway
Action: Block access
State: Report-only
```

The policy remained in **Report-only** mode so I could safely validate its behavior before enforcement.

---

## Objectives

- Create a Conditional Access policy.
- Target a specific user.
- Exclude an administrator.
- Target a specific cloud application.
- Configure **Block access**.
- Test the policy in **Report-only** mode.
- Validate the result with the **What If** tool.
- Practice safe Conditional Access deployment.

---

# 1. Review Conditional Access

I opened the Microsoft Entra Conditional Access portal.

![Conditional Access Overview](Conditional%20Access%20%20Overview.png)

Conditional Access can evaluate signals such as:

```text
User
Application
Device
Location
Authentication context
Session
```

and then make an access decision.

---

# 2. Create the Policy

I created a new Conditional Access policy named:

```text
CA-Block-Sway-ChrisGreen
```

![Conditional Access policy](Conditional%20Access%20policy.png)

The naming convention clearly describes the purpose:

```text
CA
↓
Conditional Access

Block-Sway
↓
Purpose

ChrisGreen
↓
Target user
```

---

# 3. Include the Target User

I configured the policy to include:

```text
Chris Green
```

![Conditional Access included user](CA%20User%20included.png)

The policy therefore evaluates access attempts made by this user.

---

# 4. Exclude the Administrator

I excluded my administrator account:

```text
Artsiom Pankrashkin
```

![Conditional Access excluded administrator](CA%20User%20excluded.png)

Although this policy targets only Chris Green, practicing administrator exclusions is useful because broader Conditional Access policies can cause accidental administrator lockout if configured incorrectly.

---

# 5. Select Microsoft Sway

Under **Target resources**, I selected:

```text
Microsoft Sway
```

![Microsoft Sway target](CA%20Target%20source%20Sway.png)

The policy therefore applies when the targeted user attempts to access Microsoft Sway.

---

# 6. Configure Block Access

Under **Grant**, I selected:

```text
Block access
```

The final policy configuration was:

```text
Policy:
CA-Block-Sway-ChrisGreen

Include:
Chris Green

Exclude:
Artsiom Pankrashkin

Target resource:
Microsoft Sway

Grant:
Block access

Network:
Not configured

Conditions:
Not configured

Session:
Not configured

State:
Report-only
```

![Final Conditional Access configuration](CA%20Final.png)

---

# 7. Create the Policy in Report-Only Mode

After creation, the policy appeared in the Conditional Access policy inventory.

![Conditional Access Policies](Conditional%20Access%20%20Policies.png)

The policy state was:

```text
Report-only
```

I intentionally did not enable the policy immediately.

A safer deployment process is:

```text
Create policy
     ↓
Report-only
     ↓
Test
     ↓
Review
     ↓
Enable when ready
```

This reduces the risk of accidental access disruption or administrator lockout.

---

# 8. Test the Policy with What If

I used the Conditional Access **What If** tool to simulate a sign-in.

The test scenario was:

```text
User: Chris Green
Target resource: Microsoft Sway
Device platform: Windows
Client app: Browser
```

![Conditional Access What If configuration](What%20if%20policies.png)

The current What If interface required both a device platform and client application, so I used a realistic:

```text
Windows + Browser
```

scenario.

---

# 9. Validate the Result

The What If analysis showed that:

```text
CA-Block-Sway-ChrisGreen
```

**would apply**.

The Grant control was:

```text
Block access
```

![Conditional Access What If result](What%20if%20policies%20RUN.png)

The resulting access logic was:

```text
Chris Green
      ↓
Windows device
      ↓
Browser
      ↓
Microsoft Sway
      ↓
Conditional Access evaluation
      ↓
CA-Block-Sway-ChrisGreen
      ↓
BLOCK ACCESS
```

Because the policy remained in **Report-only** mode, the result was simulated rather than actively enforced.

---

## Why I Kept the Policy in Report-Only

My tenant still had **Security Defaults** providing baseline protection.

I did not disable Security Defaults simply to activate this single test policy.

Before replacing Security Defaults with Conditional Access, I would first create appropriate baseline policies such as:

- Require MFA.
- Protect administrator accounts.
- Block legacy authentication.
- Require stronger authentication for privileged identities.

This avoids weakening the tenant while moving from Security Defaults to Conditional Access.

---

# Final Configuration

| Control | Configuration |
|---|---|
| Policy | `CA-Block-Sway-ChrisGreen` |
| Included user | Chris Green |
| Excluded administrator | Artsiom Pankrashkin |
| Target application | Microsoft Sway |
| Grant control | **Block access** |
| Network | Not configured |
| Additional conditions | Not configured |
| Session controls | Not configured |
| Policy state | **Report-only** |
| What If result | **Policy applies** |
| Expected result | **Block access** |

---

# Key Security Lessons

### Conditional Access is policy-based security

Instead of configuring security individually for every account, Conditional Access evaluates identity and contextual information before making an access decision.

```text
Identity
   +
Resource
   +
Context
   ↓
Conditional Access
   ↓
Allow / Require controls / Block
```

### Report-only reduces deployment risk

Conditional Access policies can have significant impact. Testing them before enforcement helps prevent unexpected access problems.

### Administrator lockout must be considered

Incorrect Conditional Access policies can affect privileged accounts, so administrator and emergency-access scenarios should always be considered.

### What If helps validate policy logic

The What If tool allows administrators to simulate whether Conditional Access policies would apply without requiring a real sign-in attempt.

---

# Final Result

## ✅ Lab Completed Successfully

I successfully:

- Created a Conditional Access policy.
- Used a clear policy naming convention.
- Included Chris Green.
- Excluded my administrator account.
- Targeted Microsoft Sway.
- Configured **Block access**.
- Created the policy in **Report-only** mode.
- Used the What If analysis tool.
- Tested a Windows + Browser access scenario.
- Verified that the policy would apply.
- Verified that the resulting Grant control would be **Block access**.
- Kept the policy safely in Report-only while Security Defaults remained active.

---

## Skills Practiced

`Microsoft Entra ID` · `Conditional Access` · `IAM` · `Access Control` · `What If Analysis` · `Report-Only Policies` · `Cloud Applications` · `Policy Testing` · `Administrator Protection` · `Zero Trust` · `Microsoft 365 Security`
