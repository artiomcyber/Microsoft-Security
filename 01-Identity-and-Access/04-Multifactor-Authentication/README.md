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

---

## Objectives

- Test legacy per-user MFA.
- Review MFA Service Settings.
- Disable legacy app passwords.
- Review Trusted IP and remembered-MFA settings.
- Configure MFA account lockout.
- Enable Report suspicious activity.
- Return legacy per-user MFA to Disabled.
- Prepare the tenant for Conditional Access MFA.

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

This allowed me to understand the legacy per-user MFA workflow.

Microsoft now recommends enforcing MFA through **Conditional Access** rather than leaving users in the legacy Enforced state when Conditional Access is available.

---

# 2. Review and Harden MFA Service Settings

I reviewed the legacy MFA Service Settings.

![MFA Service Settings](Per-user%20MFA%20settings%20changed.png)

### Final configuration

| Setting | Configuration |
|---|---|
| App passwords | **Disabled** |
| Trusted IP bypass | **Not configured** |
| Verification methods | Managed by Authentication Methods policy |
| Remember MFA on trusted devices | **Disabled** |

The original tenant allowed app passwords, so I changed this to:

```text
Do not allow users to create app passwords
```

This avoids introducing a legacy authentication workaround into a modern Microsoft 365 environment.

I also left Trusted IP MFA bypass and legacy Remember MFA disabled. These controls can be handled more effectively through Conditional Access.

---

# 3. Configure MFA Account Lockout

The current Entra portal no longer showed Account Lockout in the same location as the older Microsoft lab, so I located it using the portal search.

![Initial MFA Account Lockout](Account%20lockout.png)

I configured the Microsoft lab values:

```text
MFA denials before lockout: 3
Counter reset: 180 minutes
Automatic unblock: 15 minutes
```

![Configured MFA Account Lockout](Account%20lockout%20fixed.png)

These values demonstrate rate limiting against repeated MFA authentication attempts.

---

## Regional Comparison

The `3 / 180 / 15` configuration is a Microsoft lab setting, not a universal international requirement.

| Region | General approach |
|---|---|
| **EU / NIS2** | Requires secure authentication and predefined failed-attempt controls, but does not mandate exact Entra MFA numbers |
| **Australia / ASD Blueprint** | Entra baseline uses approximately `3 / 300 / 1440` |
| **United States / NIST** | Requires effective rate limiting and increasingly emphasizes phishing-resistant MFA rather than fixed Entra values |
| **Russia / FSTEC** | Includes failed-authentication limits; some password-authentication guidance uses `5 attempts / 15-minute lockout`, but this is not directly equivalent to Entra MFA lockout |

For a production environment, the exact values should be selected according to risk, regulatory requirements, user population, and authentication architecture.

---

# 4. Enable Report Suspicious Activity

The Microsoft lab references older controls such as:

- Fraud alert
- Block/unblock users
- Notifications

These have been replaced by the newer:

```text
Report suspicious activity
```

I configured:

```text
State = Enabled
Target = All users
Reporting code = 0
```

![Authentication Methods Settings](Authentication%20methods%20Settings.png)

This allows users to report MFA prompts they did not initiate, helping detect MFA fatigue or push-bombing attacks.

I kept:

```text
System-preferred authentication = Microsoft managed
```

so Microsoft Entra can prefer stronger registered authentication methods.

---

# 5. Return Legacy Per-User MFA to Disabled

After completing the legacy MFA exercise, I returned the test account to:

```text
Per-user MFA = Disabled
```

![MFA disabled](MFA%20disabled.png)

This is intentional.

The next stage will use:

```text
Conditional Access
        ↓
Require MFA
        ↓
Modern Authentication Methods
```

instead of relying on legacy per-user MFA.

---

## Final Configuration

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

## Key Lessons

- MFA should not rely only on legacy per-user settings.
- Conditional Access provides a more flexible modern MFA model.
- Legacy app passwords should be avoided when possible.
- MFA fatigue requires detection and user reporting.
- Authentication methods have different security strengths.
- Phishing-resistant methods such as FIDO2/passkeys are preferable for privileged identities.
- Lockout thresholds should be risk-based rather than treated as universal numbers.

---

## Final Result

### ✅ Lab Completed Successfully

I successfully:

- Tested legacy per-user MFA.
- Verified the Enforced MFA state.
- Hardened legacy Service Settings.
- Disabled app passwords.
- Reviewed Trusted IP and Remember MFA settings.
- Configured MFA account lockout.
- Enabled Report suspicious activity.
- Reviewed international lockout guidance.
- Returned legacy per-user MFA to Disabled.
- Prepared the tenant for modern MFA enforcement using Conditional Access.

---

## Skills Practiced

`Microsoft Entra ID` · `MFA` · `IAM` · `Authentication Methods` · `Conditional Access` · `App Passwords` · `MFA Lockout` · `Report Suspicious Activity` · `MFA Fatigue` · `Zero Trust` · `Identity Security`
