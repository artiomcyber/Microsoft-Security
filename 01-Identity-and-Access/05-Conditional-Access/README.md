# Microsoft Entra ID Lab — Conditional Access

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft Lab:** [Perform basic Conditional Access policy tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/06-perform-basic-conditional-access.html)

---

## Overview

In this lab, I created and tested a Microsoft Entra **Conditional Access** policy.

The goal was to block a specific user from accessing **Microsoft Sway** and then validate the policy with the **What If** analysis tool before enforcement.

I adapted the Microsoft lab to my own fictional tenant and used:

```text
User: Chris Green
Application: Microsoft Sway
Policy: CA-Block-Sway-ChrisGreen
```

The policy was intentionally kept in **Report-only** mode so it could be tested safely before enforcement.

---

## Objectives

- Create a Conditional Access policy.
- Target a specific user.
- Exclude an administrator account.
- Target a specific cloud application.
- Configure **Block access**.
- Use **Report-only** mode for safe testing.
- Validate the result using the **What If** tool.
- Understand how Conditional Access replaces many older identity-security controls.

---

# 1. Review Conditional Access

I opened the Microsoft Entra Conditional Access portal.

![Conditional Access Overview](Conditional%20Access%20%20Overview.png)

Conditional Access allows access decisions to be based on signals such as:

```text
User
Application
Device
Location
Authentication method
Session
```

This makes it much more flexible than legacy per-user security controls.

---

# 2. Create the Policy

I created a new policy named:

```text
CA-Block-Sway-ChrisGreen
```

![New Conditional Access policy](Conditional%20Access%20policy%281%29.png)

The naming convention makes the purpose immediately understandable:

```text
CA
↓
Conditional Access

Block-Sway
↓
Policy action and target

ChrisGreen
↓
Target user
```

---

# 3. Assign the User

I configured the policy to include:

```text
Chris Green
```

![Conditional Access user included](CA%20User%20included%281%29.png)

I also excluded my administrator account:

```text
Artsiom Pankrashkin
```

![Conditional Access administrator excluded](CA%20User%20excluded%281%29.png)

Although the policy already targeted only Chris Green, explicitly reviewing exclusions was useful practice for preventing accidental administrator lockout in broader policies.

---

# 4. Select the Target Application

Under **Target resources**, I selected:

```text
Microsoft Sway
```

![Conditional Access Sway target](CA%20Target%20source%20Sway%281%29.png)

The policy therefore applied only when the selected user attempted to access Sway.

---

# 5. Configure Access Control

Under **Grant**, I selected:

```text
Block access
```

The final policy configuration was:

```text
Policy name:
CA-Block-Sway-ChrisGreen

Include:
Chris Green

Exclude:
Artsiom Pankrashkin

Target:
Microsoft Sway

Grant:
Block access

Network:
Not configured

Conditions:
Not configured

Session:
Not configured

Policy state:
Report-only
```

![Final Conditional Access configuration](CA%20Final%281%29.png)

---

# 6. Create the Policy in Report-Only Mode

After creation, the policy appeared in the Conditional Access policy inventory.

![Conditional Access policies](Conditional%20Access%20%20Policies.png)

The policy state was:

```text
Report-only
```

I intentionally did not immediately switch the policy to **On**.

The tenant still had **Security Defaults** enabled, and I did not want to disable the tenant's existing baseline protection merely to enforce this small test policy.

A safer approach is:

```text
Build policy
     ↓
Report-only
     ↓
Test
     ↓
Review result
     ↓
Implement replacement baseline policies
     ↓
Enable
```

---

# 7. Test with the What If Tool

I used Microsoft Entra's **What If** analysis tool to simulate a sign-in.

The test configuration was:

```text
User: Chris Green
Target resource: Microsoft Sway
Device platform: Windows
Client app: Browser
```

![Conditional Access What If configuration](What%20if%20policies.png)

The modern What If interface required both:

```text
Device platform
Client app
```

so I used a realistic scenario:

```text
Windows + Browser
```

---

# 8. What If Result

The analysis showed that:

```text
CA-Block-Sway-ChrisGreen
```

**would apply** to the simulated sign-in.

The resulting Grant control was:

```text
Block access
```

![Conditional Access What If result](What%20if%20policies%20RUN.png)

This confirmed that the policy logic was working correctly.

The simulated access path was:

```text
Chris Green
      ↓
Windows device
      ↓
Browser
      ↓
Microsoft Sway
      ↓
Conditional Access evaluates policy
      ↓
CA-Block-Sway-ChrisGreen
      ↓
BLOCK ACCESS
```

Because the policy remained in **Report-only**, the block was simulated rather than actually enforced.

---

# Final Configuration

| Control | Configuration |
|---|---|
| Policy | `CA-Block-Sway-ChrisGreen` |
| Included user | Chris Green |
| Excluded administrator | Artsiom Pankrashkin |
| Target resource | Microsoft Sway |
| Grant control | **Block access** |
| Network conditions | Not configured |
| Additional conditions | Not configured |
| Session controls | Not configured |
| Policy state | **Report-only** |
| What If result | **Policy applies / Block access** |

---

# Key Security Lessons

### Conditional Access is policy-based security

Instead of configuring MFA or access controls individually for each user, Conditional Access can evaluate multiple signals before allowing access.

```text
Identity + Context + Resource
            ↓
Conditional Access
            ↓
Allow / Require control / Block
```

---

### Report-only reduces deployment risk

A badly configured Conditional Access policy can potentially lock users or administrators out.

Using Report-only first allows the policy logic to be evaluated before enforcement.

---

### Administrative exclusions require care

Emergency-access and administrator exclusions can prevent lockouts, but excessive exclusions can also create security gaps.

They should therefore be limited and documented.

---

### Security Defaults and Conditional Access require planning

I did not disable Security Defaults simply to activate this test policy.

The better migration path is to first design replacement Conditional Access baseline policies for controls such as MFA before removing the existing tenant-wide baseline.

---

# Final Result

## ✅ Lab Completed Successfully

I successfully:

- Created a Conditional Access policy.
- Used a clear policy naming convention.
- Targeted Chris Green.
- Excluded my administrator account.
- Targeted Microsoft Sway.
- Configured **Block access**.
- Created the policy in **Report-only** mode.
- Used the What If tool.
- Tested a Windows/browser sign-in scenario.
- Verified that the policy would apply.
- Verified that the resulting access decision would be **Block access**.
- Kept the policy safely in Report-only pending a broader Conditional Access baseline design.

---

## Skills Practiced

`Microsoft Entra ID` · `Conditional Access` · `IAM` · `Access Control` · `What If Analysis` · `Report-Only Policies` · `Cloud App Control` · `Policy Testing` · `Zero Trust` · `Microsoft 365 Security`
