# Microsoft Entra ID Lab — Multifactor Authentication (MFA)

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft Lab:** [Perform basic Multifactor Authentication tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/05-perform-basic-mfa-tasks.html)

---

## Overview

In this lab, I reviewed and configured Microsoft Entra multifactor authentication controls.

The Microsoft exercise uses several **legacy MFA settings**, including per-user MFA and older Service Settings. Because my tenant includes Microsoft Entra ID P1, I completed the legacy exercise for learning purposes but adapted the final configuration toward the modern Microsoft approach using **Authentication Methods and Conditional Access**.

The lab included:

- Testing legacy per-user MFA
- Reviewing MFA Service Settings
- Disabling legacy app passwords
- Reviewing Trusted IP and remembered-MFA settings
- Configuring MFA account lockout
- Enabling Report suspicious activity
- Reviewing system-preferred authentication
- Returning legacy per-user MFA to Disabled
- Preparing the tenant for Conditional Access MFA

---

## Objectives

- Understand legacy per-user MFA.
- Enable and verify MFA for a test user.
- Review legacy MFA Service Settings.
- Disable app passwords.
- Review Trusted IP and trusted-device MFA bypass.
- Configure MFA account lockout.
- Enable suspicious MFA reporting.
- Compare regional security guidance.
- Return the tenant to a modern Conditional Access-ready state.

---

# 1. Review Per-User MFA

Initially, all users had legacy per-user MFA set to:

```text
Disabled
```

![Initial per-user MFA](Per-user%20MFA.png)

I temporarily enabled MFA for my test administrator account.

![Enable per-user MFA](Per-user%20MFA%20Enable.png)

The account then reached:

```text
Status = Enforced
```

![Per-user MFA enforced](Per-user%20MFA%20Enforced.png)

This demonstrated the legacy per-user MFA workflow.

However, Microsoft now recommends enforcing MFA through **Conditional Access** when Conditional Access is available.

---

# 2. Review Microsoft Entra MFA

I also reviewed the current Microsoft Entra MFA area.

![Microsoft Entra MFA overview](Multifactor%20authentication.png)

The portal itself recommends using cloud-based MFA together with **Conditional Access**.

The modern model is therefore:

```text
Authentication Methods
        +
Conditional Access
        +
MFA
        =
Modern identity protection
```

rather than relying primarily on per-user MFA.

---

# 3. Harden Legacy MFA Service Settings

I reviewed the legacy MFA Service Settings and made one important security improvement.

![MFA Service Settings](Per-user%20MFA%20settings%20changed.png)

### Final configuration

| Setting | Configuration |
|---|---|
| App passwords | **Disabled** |
| Trusted IP bypass | **Not configured** |
| Verification methods | Managed by Authentication Methods policy |
| Remember MFA on trusted devices | **Disabled** |

The tenant originally allowed users to create app passwords.

I changed this to:

```text
Do not allow users to create app passwords
```

App passwords exist mainly for older applications that cannot use modern authentication, so I did not want to retain this legacy authentication workaround in a new Microsoft 365 security environment.

I also left Trusted IP bypass disabled and did not enable legacy Remember MFA.

These controls can be managed more securely through Conditional Access.

---

# 4. Configure MFA Account Lockout

The current Microsoft Entra portal no longer displayed Account Lockout in the same location as the older Microsoft training material.

I located the setting through the current portal interface.

![Initial MFA Account Lockout](Account%20lockout.png)

I configured:

```text
MFA denials before lockout: 3
Counter reset: 180 minutes
Automatic unblock: 15 minutes
```

![Configured MFA Account Lockout](Account%20lockout%20fixed.png)

These values match the Microsoft training exercise and demonstrate authentication rate limiting.

---

## Regional Comparison

The `3 / 180 / 15` values are **Microsoft lab values**, not a universal global standard.

| Region | General approach |
|---|---|
| **EU / NIS2** | Requires secure authentication and predefined failed-attempt controls, but does not mandate exact Entra MFA values |
| **Australia / ASD Blueprint** | Microsoft Entra baseline uses approximately `3 / 300 / 1440` |
| **United States / NIST** | Requires effective rate limiting and emphasizes phishing-resistant authentication rather than fixed Entra values |
| **Russia / FSTEC** | Includes failed-authentication limits; some password-authentication guidance uses `5 attempts / 15-minute lockout`, but this is not directly equivalent to Entra MFA lockout |

For a production environment, the exact threshold should depend on:

```text
Risk
Regulatory requirements
User population
Authentication method
Privileged identities
Threat model
Operational impact
```

---

# 5. Enable Report Suspicious Activity

The Microsoft lab references older MFA features such as:

- Fraud alert
- Block/unblock users
- Notifications

The modern Microsoft Entra replacement is:

```text
Report suspicious activity
```

I configured:

```text
State = Enabled
Target = All users
Reporting code = 0
```

![Authentication Methods Settings](Authentication%20methods%20%20Settings.png)

This allows users to report MFA prompts they did not initiate.

This is particularly useful against attacks such as:

```text
MFA fatigue
Push bombing
Repeated authentication prompts
Stolen-password sign-in attempts
```

---

## System-Preferred Authentication

On the same page, I reviewed:

```text
System-preferred authentication
```

The tenant was configured as:

```text
State = Microsoft managed
Target = All users
```

I retained this configuration.

This allows Microsoft Entra to prefer stronger authentication methods available to the user.

---

# 6. Return Legacy Per-User MFA to Disabled

After completing the legacy MFA exercise, I returned the test account to:

```text
Per-user MFA = Disabled
```

![Legacy per-user MFA disabled](MFA%20disabled.png)

All users were again shown as Disabled in the legacy per-user MFA interface.

This is intentional.

The next implementation stage will use:

```text
Conditional Access
        ↓
Require MFA
        ↓
Modern Authentication Methods
```

instead of legacy per-user MFA.

---

# Final Configuration

| Control | Final State |
|---|---|
| Legacy per-user MFA | **Disabled** |
| App passwords | **Disabled** |
| Trusted IP bypass | **Not configured** |
| Remember MFA | **Disabled** |
| MFA lockout | **3 / 180 / 15** |
| Report suspicious activity | **Enabled for all users** |
| System-preferred authentication | **Microsoft managed** |
| Modern MFA enforcement | **Conditional Access planned next** |

---

# Legacy vs Modern MFA

### Legacy model

```text
User
  ↓
Per-user MFA
  ↓
Enabled / Enforced
```

### Modern model

```text
User
  ↓
Conditional Access
  ↓
Evaluate user, application, device and context
  ↓
Require MFA
  ↓
Authentication Methods Policy
  ↓
Strong authentication
```

Conditional Access provides much more flexibility than legacy per-user MFA.

---

# Key Security Lessons

### MFA is not one single technology

Different MFA methods provide different security levels.

Examples include:

```text
Microsoft Authenticator
TOTP
FIDO2 security keys
Passkeys
Certificates
```

---

### Not all MFA is phishing resistant

OTP codes and standard push notifications can still be targeted by phishing and social engineering.

Phishing-resistant methods include technologies such as:

```text
FIDO2
Passkeys
Certificate-based authentication
```

These are especially important for privileged accounts.

---

### MFA fatigue needs specific protection

Attackers may repeatedly send MFA requests hoping that a user eventually approves one.

Controls such as:

- Report suspicious activity
- Number matching
- Conditional Access
- Stronger authentication methods
- Monitoring

help reduce this risk.

---

### Legacy compatibility can weaken security

App passwords and authentication bypasses may be necessary in older environments, but I would avoid introducing them into a modern Microsoft 365 environment unless there is a documented business requirement.

---

# Final Result

## ✅ Lab Completed Successfully

I successfully:

- Reviewed Microsoft Entra MFA.
- Tested legacy per-user MFA.
- Verified the Enforced state.
- Reviewed legacy MFA Service Settings.
- Disabled app-password creation.
- Left Trusted IP MFA bypass disabled.
- Left legacy Remember MFA disabled.
- Configured MFA account lockout to `3 / 180 / 15`.
- Compared the configuration with EU, Australian, US, and Russian security guidance.
- Enabled Report suspicious activity for all users.
- Reviewed system-preferred authentication.
- Returned legacy per-user MFA to Disabled.
- Prepared the tenant for modern MFA enforcement using Conditional Access.

The key lesson from this lab was that modern MFA administration is broader than simply setting a user to **Enforced**.

A stronger architecture combines:

```text
Authentication Methods
        +
Conditional Access
        +
Strong MFA
        +
Monitoring
        +
User reporting
```

---

## Skills Practiced

`Microsoft Entra ID` · `IAM` · `MFA` · `Multifactor Authentication` · `Authentication Methods` · `Conditional Access` · `App Passwords` · `MFA Lockout` · `Report Suspicious Activity` · `MFA Fatigue` · `Phishing-Resistant MFA` · `Zero Trust` · `Identity Security`

---

## Frameworks Reviewed

- Microsoft Entra security guidance
- EU NIS2 / ENISA guidance
- Australian Signals Directorate Blueprint for Secure Cloud
- NIST SP 800-63B
- FSTEC authentication guidance
