# Microsoft Entra ID Lab — Configure Self-Service Password Reset (SSPR)

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft Lab:** [Perform basic Self-Service Password Reset (SSPR) tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/04-perform-basic-sspr-tasks.html)

---

## Overview

In this lab, I configured **Self-Service Password Reset (SSPR)** in Microsoft Entra ID.

SSPR allows users to reset their own passwords after verifying their identity, reducing dependency on the IT help desk while maintaining identity-verification controls.

The original Microsoft training exercise uses an older Microsoft Entra configuration experience. My tenant uses the current authentication-method management model, so I adapted the exercise to the modern Microsoft Entra interface while preserving the original security objectives.

The implementation included:

- Reviewing the original SSPR state
- Enabling SSPR for a controlled pilot group
- Reusing the existing `Project23` group
- Reviewing SSPR authentication requirements
- Reviewing the modern centralized Authentication Methods policy
- Avoiding Security Questions because Microsoft is retiring them
- Requiring authentication-method registration
- Changing authentication-information reconfirmation from 180 to 90 days
- Enabling user password-reset notifications
- Enabling administrator password-reset notifications

---

# Objectives

The objectives of this lab were to:

- Enable Self-Service Password Reset.
- Scope SSPR to a selected group.
- Review authentication methods available for password recovery.
- Understand the difference between the legacy SSPR configuration and the current Authentication Methods policy.
- Configure user registration requirements.
- Configure authentication-information reconfirmation.
- Configure password-reset notifications.
- Apply the Microsoft lab requirements using the current Entra interface.
- Verify the completed configuration.

---

# Task 1 — Review the Existing SSPR Configuration

I first navigated to:

```text
Microsoft Entra admin center
        ↓
Entra ID
        ↓
Password reset
        ↓
Properties
```

The original configuration showed:

```text
Self service password reset enabled = None
```

![Initial SSPR configuration](Password%20reset.png)

This meant that regular end users were not currently enabled for Self-Service Password Reset.

This provided a baseline before making any changes.

---

# Task 2 — Enable SSPR for a Pilot Group

Rather than immediately enabling SSPR for every user in the tenant, I selected:

```text
Self service password reset enabled = Selected
```

I then chose:

```text
Project23
```

as the pilot group.

![SSPR enabled for Project23](Password%20reset%20assigned%20group.png)

The `Project23` group had already been created during my previous Microsoft Entra Group Management lab.

Reusing this group allowed me to connect the labs together and simulate an incremental identity-management implementation.

---

## Why Use a Pilot Group?

Deploying a security capability to a limited group before enabling it tenant-wide is a useful operational practice.

A controlled rollout can follow this model:

```text
New security capability
        ↓
Pilot group
        ↓
Testing
        ↓
Troubleshooting
        ↓
User feedback
        ↓
Broader deployment
```

This reduces the risk of an incorrectly configured policy affecting the entire organization.

For this lab:

```text
Pilot group = Project23
```

---

# Task 3 — Review Password Reset Authentication Methods

I opened:

```text
Password reset
        ↓
Authentication methods
```

The current Entra interface showed:

```text
Number of methods required to reset = 1
```

It also showed that **Security Questions** were the only authentication method still directly configurable from this legacy SSPR page.

![Legacy SSPR authentication methods page](PR%20Authentication%20methods.png)

The portal also displayed an important notice:

```text
Security Questions are retiring in March 2027.
```

The page additionally stated:

```text
Use the auth methods policy to manage other authentication methods.
```

This demonstrated that the Microsoft training instructions no longer fully match the current Entra administration experience.

---

# Number of Methods Required to Reset

I kept:

```text
Number of methods required to reset = 1
```

This matched the requirement in the Microsoft lab.

Conceptually:

```text
User forgets password
        ↓
Starts SSPR
        ↓
Provides one approved verification method
        ↓
Identity verified
        ↓
Password reset permitted
```

For more sensitive or higher-risk environments, organizations may choose stronger recovery requirements depending on their security architecture.

---

# Why I Did Not Enable Security Questions

I intentionally left:

```text
Security Questions = Disabled
```

The Microsoft Entra portal itself indicated that Security Questions are being retired.

Additionally, security questions have several weaknesses because answers may be:

- Guessable
- Publicly available
- Discoverable through social media
- Reused across services
- Based on information known by other people

Rather than enabling a method approaching retirement simply to reproduce an older training lab, I kept the modern authentication-method configuration already available in the tenant.

---

# Task 4 — Review the Modern Authentication Methods Policy

I followed the link from the SSPR page to the centralized:

```text
Authentication Methods Policy
```

The current tenant configuration showed several available authentication methods.

![Modern Authentication Methods policy](Auth%20Methods.png)

The configuration included:

| Authentication Method | Status |
|---|---|
| Passkey (FIDO2) | Enabled |
| Microsoft Authenticator | Enabled |
| SMS | Disabled |
| Temporary Access Pass | Enabled |
| Hardware OATH tokens | Disabled |
| Software OATH tokens | Enabled |
| Voice call | Disabled |
| Email OTP | Enabled |
| Certificate-based authentication | Disabled |
| Verified ID | Disabled |
| QR code | Disabled |

---

# Legacy Lab vs Modern Microsoft Entra

The Microsoft training exercise referenced older SSPR controls such as:

```text
Email
Mobile phone
Mobile app code
```

In the current environment, most authentication-method administration has moved to the centralized:

```text
Authentication Methods Policy
```

The older model was approximately:

```text
Password Reset
      ↓
Authentication Methods
      ↓
Individual SSPR method checkboxes
```

The current model is more centralized:

```text
Microsoft Entra
      ↓
Authentication Methods
      ↓
Policies
      ↓
Central authentication-method management
```

This was one of the most useful lessons in the exercise because it required understanding the **purpose of the configuration** rather than simply following old screenshots.

---

# Current Authentication Methods in My Tenant

Several modern authentication capabilities were already enabled.

## Microsoft Authenticator

```text
Status = Enabled
Target = All users
```

Microsoft Authenticator provides a stronger modern authentication mechanism than relying only on passwords.

---

## Software OATH Tokens

```text
Status = Enabled
Target = All users
```

Software OATH supports time-based one-time passwords generated by compatible authenticator applications.

---

## Temporary Access Pass

```text
Status = Enabled
Target = All users
```

Temporary Access Pass can support controlled onboarding and authentication-method registration scenarios.

---

## SMS

```text
Status = Disabled
```

The original training exercise referenced mobile-phone verification.

However, I did not enable SMS simply to reproduce an older lab configuration because stronger modern authentication methods were already available in the tenant.

---

## Email OTP

The Authentication Methods policy also showed:

```text
Email OTP = Enabled
```

I documented this as part of the authentication-method inventory.

I did not assume that every method displayed in the Authentication Methods policy is automatically usable in every SSPR scenario, because authentication-method applicability depends on the authentication scenario and identity type.

This distinction is important when administering Microsoft Entra.

---

# Task 5 — Configure User Registration

I next opened:

```text
Password reset
        ↓
Registration
```

The existing configuration showed:

```text
Require users to register when signing in = Yes
```

and:

```text
Reconfirm authentication information = 180 days
```

![Original SSPR registration settings](PR%20Registration.png)

The Microsoft lab required:

```text
90 days
```

so I changed:

```text
180
```

to:

```text
90
```

and saved the configuration.

![Updated SSPR registration settings](PR%20Registration%20fixed.png)

The portal confirmed:

```text
Password reset policy saved
```

---

# Why Authentication Registration Matters

SSPR depends on users having valid recovery information registered before they lose access.

Without registration:

```text
User forgets password
        ↓
No registered verification method
        ↓
Identity cannot be verified
        ↓
SSPR cannot complete
```

With proper registration:

```text
User signs in
        ↓
Registers authentication method
        ↓
Authentication information stored
        ↓
Password forgotten later
        ↓
User verifies identity
        ↓
Password reset completed
```

Requiring registration during sign-in helps prepare users before an account-recovery event occurs.

---

# Why Reconfirmation Matters

Authentication information can become outdated.

Examples include:

```text
Employee changes phone
Employee replaces mobile device
Authenticator registration changes
Employee loses access to an old device
Recovery information becomes outdated
```

Periodic reconfirmation helps ensure that recovery information remains valid.

For this lab I configured:

```text
Authentication information reconfirmation = 90 days
```

---

# Task 6 — Configure Password Reset Notifications

I then opened:

```text
Password reset
        ↓
Notifications
```

I enabled:

```text
Notify users on password resets = Yes
```

and:

```text
Notify all admins when other admins reset their password = Yes
```

![SSPR notification configuration](PR%20Notifications%20fixed.png)

---

# Why User Notifications Matter

Password-reset notifications provide users with a security signal when account-recovery activity occurs.

Conceptually:

```text
Password reset occurs
        ↓
User receives notification
        ↓
Was this expected?
       / \
     Yes  No
     ↓     ↓
   Normal Possible security incident
```

If a user receives an unexpected password-reset notification, it can indicate suspicious account activity that should be investigated.

---

# Why Administrator Notifications Matter

Administrator identities have elevated privileges and represent more sensitive accounts.

For that reason, I enabled:

```text
Notify all admins when another admin resets their password
```

Conceptually:

```text
Administrator password reset
        ↓
Notification generated
        ↓
Other administrators informed
        ↓
Unexpected activity can be investigated
```

This provides additional visibility around privileged-account recovery activity.

---

# Final SSPR Configuration

The completed configuration was:

| SSPR Control | Final Configuration |
|---|---|
| SSPR enabled | Yes |
| Scope | Selected |
| Pilot group | Project23 |
| Methods required to reset | 1 |
| Security Questions | Disabled |
| Registration required when signing in | Yes |
| Reconfirmation period | 90 days |
| Notify users after password reset | Yes |
| Notify admins when admins reset password | Yes |

The modern authentication environment also included:

| Authentication Method | Status |
|---|---|
| Microsoft Authenticator | Enabled |
| Software OATH | Enabled |
| Passkey (FIDO2) | Enabled |
| Temporary Access Pass | Enabled |
| Email OTP | Enabled |
| SMS | Disabled |
| Voice call | Disabled |

---

# Final Architecture

The completed lab can be represented as:

```text
                    Microsoft Entra ID
                           │
                           ▼
                Self-Service Password Reset
                           │
                           ▼
                    Selected users
                           │
                           ▼
                       Project23
                           │
                           ▼
              Authentication registration
                           │
                           ▼
              Authentication verification
                           │
                           ▼
                    Password reset
                           │
                           ▼
                 Security notifications
```

The authentication-method layer is managed centrally:

```text
Microsoft Entra ID
        ↓
Authentication Methods Policy
        ↓
Microsoft Authenticator
Passkeys
OATH
Temporary Access Pass
Other supported methods
```

---

# Before vs After

| Setting | Before | After |
|---|---|---|
| SSPR enabled | None | **Selected** |
| SSPR group | None | **Project23** |
| Methods required | 1 | **1** |
| Security Questions | Disabled | **Disabled** |
| Registration required | Yes | **Yes** |
| Reconfirmation | 180 days | **90 days** |
| User reset notification | Not configured for lab | **Enabled** |
| Admin reset notification | Not configured for lab | **Enabled** |

---

# Security Concepts Demonstrated

By completing this lab, I practiced:

- Microsoft Entra ID administration
- Self-Service Password Reset
- Identity recovery
- Authentication-method registration
- Authentication Methods policy
- Pilot security deployments
- Group-scoped configuration
- Microsoft Authenticator
- Software OATH
- Temporary Access Pass awareness
- Passkey awareness
- Password-reset notifications
- Privileged-account monitoring
- Legacy-to-modern Microsoft Entra configuration
- Identity lifecycle management

---

# Security and Operational Observations

## 1. SSPR Reduces Help-Desk Dependency

Traditional password recovery may require:

```text
User forgets password
        ↓
Help-desk ticket
        ↓
Administrator verifies identity
        ↓
Administrator resets password
```

With SSPR:

```text
User forgets password
        ↓
User verifies identity
        ↓
User resets password
```

This can reduce support workload and improve recovery speed.

---

## 2. SSPR Is a Security Control, Not Only a Convenience Feature

Password recovery is effectively another authentication process.

The system must answer:

> Is the person requesting the password reset really the legitimate account owner?

Therefore:

```text
Password Reset
      =
Identity Verification
      +
Credential Recovery
```

Weak recovery controls could undermine otherwise strong authentication.

---

## 3. Recovery Methods Must Be Protected

An organization can deploy strong passwords and MFA, but if the account-recovery process is weak, attackers may attempt to bypass normal authentication through recovery mechanisms.

Therefore, SSPR design is part of the broader identity-security architecture.

---

## 4. Pilot Deployment Reduces Operational Risk

I enabled SSPR only for:

```text
Project23
```

rather than immediately enabling it tenant-wide.

This allowed the configuration to be treated as a controlled pilot.

A production deployment could follow:

```text
Pilot
  ↓
Test
  ↓
Monitor
  ↓
Resolve problems
  ↓
Expand
```

---

## 5. Authentication Methods Are Becoming More Centralized

The training lab expected several options to appear directly inside the SSPR authentication-method page.

My current tenant instead directed me to:

```text
Authentication Methods Policy
```

This showed how Microsoft Entra administration continues to evolve.

Administrators therefore need to understand the security objective instead of relying purely on memorized portal navigation.

---

## 6. Avoid Reintroducing Retiring Controls

Security Questions were available on the legacy SSPR page, but the portal explicitly stated that they are retiring.

I therefore left them disabled.

This demonstrates an important operational principle:

> Training material should be interpreted in the context of the current supported platform.

---

## 7. Notifications Provide Detection Capability

SSPR is not only about prevention and recovery.

Notifications provide an additional detection layer:

```text
Password reset
        ↓
Notification
        ↓
User/Admin visibility
        ↓
Unexpected activity detected
        ↓
Investigation
```

This supports better identity monitoring.

---

# Production Improvements I Would Consider

For a real production deployment, I would consider:

- Maintaining SSPR as a pilot before tenant-wide rollout.
- Measuring authentication-method registration coverage.
- Reviewing SSPR audit logs.
- Monitoring failed password-reset attempts.
- Monitoring unusual recovery activity.
- Reviewing privileged-account reset events.
- Ensuring users have reliable recovery methods.
- Using stronger phishing-resistant authentication methods where appropriate.
- Expanding passwordless authentication.
- Using Temporary Access Pass for controlled onboarding.
- Reviewing Authentication Methods policies periodically.
- Removing obsolete authentication methods.
- Applying Conditional Access.
- Combining SSPR with MFA.
- Implementing stronger privileged identity controls.

---

# Connection to Previous Labs

This lab builds on the earlier identity-security exercises in my Microsoft Security lab environment.

## Lab 01 — Basic Group Management

```text
Users
Groups
Dynamic membership
External identities
Group ownership
Group-based licensing
```

## Lab 02 — Password Protection

```text
Smart Lockout
Custom banned passwords
Password protection
Authentication security
```

## Lab 03 — Self-Service Password Reset

```text
Password recovery
Authentication registration
Identity verification
Notifications
```

Together:

```text
Identity Administration
        +
Password Protection
        +
Secure Account Recovery
        =
Stronger Identity Lifecycle Security
```

---

# Key Lesson: Understanding the Objective Instead of Memorizing Buttons

One of the most valuable lessons from this exercise was that the Microsoft training instructions did not exactly match the current Entra interface.

Instead of treating this as a blocker, I identified the purpose of the original configuration and located the current equivalent controls.

The process became:

```text
Understand requirement
        ↓
Identify current Microsoft implementation
        ↓
Evaluate security implications
        ↓
Configure
        ↓
Save
        ↓
Verify
        ↓
Document
```

This is closer to real-world administration than simply reproducing screenshots from a tutorial.

---

# Final Result

## ✅ Lab Completed Successfully

I successfully:

- Reviewed the original Self-Service Password Reset configuration.
- Identified that SSPR was initially disabled for end users.
- Enabled SSPR for a selected pilot group.
- Selected the existing `Project23` group.
- Reviewed the password-reset authentication requirements.
- Kept one authentication method required for reset as specified by the lab.
- Reviewed Microsoft's modern Authentication Methods policy.
- Identified that the current interface differs from the older Microsoft training environment.
- Reviewed Microsoft Authenticator, Passkeys, OATH, Temporary Access Pass, Email OTP, SMS and other authentication methods.
- Intentionally left Security Questions disabled because the feature is being retired.
- Kept the existing stronger modern authentication configuration rather than enabling SMS only to reproduce an older lab.
- Required authentication-method registration during sign-in.
- Changed authentication-information reconfirmation from 180 days to 90 days.
- Enabled user password-reset notifications.
- Enabled administrator notifications for administrator password resets.
- Saved and verified the configuration.

The most valuable part of this lab was learning how to translate an **older Microsoft training exercise into the current Microsoft Entra architecture** while maintaining the intended identity-security objective.

---

## Skills Practiced

`Microsoft Entra ID` · `IAM` · `SSPR` · `Self-Service Password Reset` · `Authentication Methods` · `Microsoft Authenticator` · `Passkeys` · `OATH` · `Temporary Access Pass` · `Identity Recovery` · `Password Reset` · `Authentication Registration` · `Pilot Deployment` · `Privileged Identity Monitoring` · `Microsoft 365 Security` · `Identity Security`
