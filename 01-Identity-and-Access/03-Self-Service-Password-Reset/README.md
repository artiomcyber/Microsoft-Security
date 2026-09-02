# Microsoft Entra ID Lab — Configure Self-Service Password Reset (SSPR)

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft Lab:** [Perform basic Self-Service Password Reset (SSPR) tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/04-perform-basic-sspr-tasks.html)

---

## Overview

In this lab, I configured **Self-Service Password Reset (SSPR)** in Microsoft Entra ID.

SSPR allows users to reset their own passwords after successfully verifying their identity, reducing dependence on the IT help desk while maintaining authentication controls.

The Microsoft training exercise was based on an older Entra interface. My tenant uses the newer authentication-method management model, so I adapted the exercise to the current Microsoft Entra configuration while preserving the intended learning objectives.

The final implementation included:

- Enabling SSPR for a controlled pilot group
- Using the existing `Project23` group
- Reviewing modern authentication methods
- Avoiding deprecated Security Questions
- Requiring users to register authentication information
- Setting authentication-information reconfirmation to 90 days
- Enabling password-reset notifications
- Enabling administrator-reset notifications
- Reviewing the difference between legacy and modern authentication-method configuration

---

# Objectives

The objectives of this lab were to:

- Enable Self-Service Password Reset.
- Scope SSPR to a selected group.
- Review authentication methods available for password reset.
- Configure user registration requirements.
- Configure authentication-information reconfirmation.
- Configure password-reset notifications.
- Understand the modern Microsoft Entra Authentication Methods policy.
- Avoid outdated authentication methods where more modern alternatives are available.
- Verify the completed SSPR configuration.

---

# Task 1 — Review the Existing SSPR Configuration

I first opened:

```text
Microsoft Entra admin center
        ↓
Entra ID
        ↓
Password reset
        ↓
Properties
```

The original tenant configuration showed:

```text
Self service password reset enabled = None
```

![Initial SSPR configuration](Password%20reset%281%29.png)

This meant that normal users were not yet enabled for Self-Service Password Reset.

---

# Task 2 — Enable SSPR for a Pilot Group

Instead of immediately enabling SSPR for the entire tenant, I selected:

```text
Self service password reset enabled = Selected
```

and chose:

```text
Project23
```

as the target group.

![SSPR enabled for Project23](Password%20reset%20assigned%20group%281%29.png)

This created a controlled pilot deployment.

The `Project23` group had already been created during my previous Microsoft Entra group-management lab, allowing me to reuse an existing group rather than create another temporary group.

---

## Why Use a Pilot Group?

Enabling a new identity-security feature for a limited group first is safer than immediately applying it to every user.

The deployment model becomes:

```text
New security capability
        ↓
Pilot group
        ↓
Testing
        ↓
User feedback
        ↓
Troubleshooting
        ↓
Broader deployment
```

This approach reduces operational risk and provides an opportunity to identify configuration problems before organization-wide rollout.

---

# Task 3 — Review SSPR Authentication Methods

The Microsoft training instructions referenced authentication methods such as:

- Email
- Mobile phone
- Mobile app code

However, the current Microsoft Entra portal no longer manages these methods in the same way from the legacy SSPR page.

On the SSPR authentication-method page, the portal displayed:

```text
Use the auth methods policy to manage other authentication methods.
```

It also displayed a notice that:

```text
Security Questions are retiring in March 2027.
```

![Legacy SSPR authentication methods page](PR%20Authentication%20methods%281%29.png)

Because the Microsoft lab was based on an older interface, I did not enable Security Questions simply to reproduce an outdated configuration.

Instead, I followed the current Microsoft Entra authentication-method model.

---

# Task 4 — Review the Modern Authentication Methods Policy

I opened the centralized:

```text
Authentication Methods Policy
```

The tenant showed the following authentication methods:

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

![Modern Authentication Methods policy](Auth%20Methods.png)

This was an important difference between the current environment and the older Microsoft lab.

---

# Modernizing the Microsoft Training Exercise

Rather than blindly enabling legacy options, I adapted the exercise to the current Microsoft Entra architecture.

The older training model was conceptually:

```text
Password Reset
      ↓
Authentication Methods
      ↓
Email
Mobile phone
Mobile app code
```

The modern model is:

```text
Microsoft Entra
      ↓
Authentication Methods Policy
      ↓
Central management of authentication methods
      ↓
MFA + SSPR + other authentication scenarios
```

This centralized approach makes authentication-method administration more consistent across Microsoft Entra.

---

# Authentication Methods Available in My Tenant

The existing environment already included several stronger authentication capabilities.

### Microsoft Authenticator

```text
Status = Enabled
Target = All users
```

Microsoft Authenticator can provide stronger authentication than traditional SMS-based verification.

---

### Software OATH Tokens

```text
Status = Enabled
Target = All users
```

Software OATH supports time-based one-time passwords generated by compatible authenticator applications.

---

### Email OTP

```text
Status = Enabled
Target = All users
```

Email OTP was also enabled in the centralized Authentication Methods policy.

---

### SMS

```text
Status = Disabled
```

The old training exercise referenced mobile-phone authentication.

However, I did not enable SMS solely to reproduce the outdated lab.

For this lab, I preferred to retain the stronger authentication methods already configured in the tenant rather than introduce a weaker method simply because it appeared in older training documentation.

---

# Why I Did Not Enable Security Questions

The SSPR portal itself displayed a Microsoft notice explaining that **Security Questions are being retired in March 2027**.

For that reason, I deliberately left:

```text
Security Questions = Disabled
```

This was a conscious security and lifecycle decision.

Security questions can depend on information that may be:

- Guessable
- Publicly available
- Discoverable through social media
- Reused across services
- Forgotten by legitimate users

Instead, my environment already supported more modern authentication methods.

---

# Task 5 — Configure Number of Methods Required

On the SSPR Authentication Methods page, I kept:

```text
Number of methods required to reset = 1
```

This matched the Microsoft lab requirement.

Conceptually:

```text
User requests password reset
        ↓
Identity verification required
        ↓
One approved authentication method
        ↓
Successful verification
        ↓
Password reset allowed
```

In a higher-risk production environment, requiring multiple verification methods could be considered depending on the organization's security requirements.

---

# Task 6 — Configure User Registration

I then opened:

```text
Password reset
        ↓
Registration
```

The original registration configuration showed:

```text
Require users to register when signing in = Yes
Reconfirm authentication information = 180 days
```

![Original SSPR registration settings](PR%20Registration.png)

The Microsoft lab required users to reconfirm their authentication information every:

```text
90 days
```

I therefore changed the value from:

```text
180 days
```

to:

```text
90 days
```

and saved the configuration.

![Updated SSPR registration settings](PR%20Registration%20fixed.png)

The portal confirmed:

```text
Password reset policy saved
```

---

# Why Authentication Registration Matters

SSPR only works effectively when users have valid authentication information registered.

Without registered authentication methods:

```text
Forgot password
      ↓
No identity verification method
      ↓
Cannot complete SSPR
```

With proper registration:

```text
User registers authentication method
        ↓
Authentication information stored
        ↓
Password forgotten
        ↓
User verifies identity
        ↓
Password reset
```

Requiring registration during sign-in helps ensure that users prepare their recovery methods before they actually need them.

---

# Why Reconfirmation Matters

Authentication information can become outdated.

For example:

```text
Employee changes phone
Employee replaces device
Old authenticator removed
Email address changes
```

Periodically asking users to reconfirm their authentication information helps reduce the risk of relying on stale recovery data.

For this lab I configured:

```text
Reconfirmation interval = 90 days
```

---

# Task 7 — Configure SSPR Notifications

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

# Why Password Reset Notifications Matter

User notifications provide an additional security signal.

For example:

```text
Password reset occurs
       ↓
User receives notification
       ↓
Was this expected?
       ↓
Yes → no action required
No  → possible account compromise
```

If a user receives a password-reset notification they did not initiate, it can alert them to potentially suspicious activity.

---

# Why Administrator Notifications Matter

Administrator identities have elevated privileges and represent higher-value targets.

I therefore enabled:

```text
Notify all admins when another admin resets their password
```

This provides additional visibility into privileged-account password changes.

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

This contributes to better monitoring of privileged identities.

---

# Final SSPR Architecture

My completed lab configuration can be represented as:

```text
                    Microsoft Entra ID
                           │
                           ▼
                Self-Service Password Reset
                           │
                    Selected Users
                           │
                           ▼
                       Project23
                           │
                           ▼
              Authentication Registration
                           │
                           ▼
               Modern Authentication Methods
                           │
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
 Microsoft Authenticator  OATH         Email OTP
                           │
                           ▼
                 Identity Verification
                           │
                           ▼
                    Password Reset
                           │
                           ▼
                      Notification
```

---

# Final Configuration

| SSPR Control | Final Configuration |
|---|---|
| SSPR scope | Selected users |
| Pilot group | Project23 |
| Methods required to reset | 1 |
| Security Questions | Disabled |
| Microsoft Authenticator | Enabled |
| Software OATH | Enabled |
| Email OTP | Enabled |
| SMS | Disabled |
| Registration required | Yes |
| Reconfirmation period | 90 days |
| Notify users after reset | Yes |
| Notify admins of admin resets | Yes |

---

# Legacy Lab vs Modern Implementation

One of the most valuable parts of this exercise was identifying that the Microsoft training material was based on an older configuration experience.

| Training Lab | My Current Implementation |
|---|---|
| Authentication methods managed under SSPR | Authentication Methods policy |
| Email | Email OTP policy |
| Mobile app code | Authenticator / OATH |
| Mobile phone | SMS available but intentionally not enabled |
| Security Questions available | Not enabled; retiring |
| Direct legacy method configuration | Centralized authentication-method management |

This required understanding the **security objective** rather than simply reproducing each button shown in old training screenshots.

---

# Security Concepts Demonstrated

By completing this lab, I practiced:

- Microsoft Entra ID administration
- Self-Service Password Reset
- Authentication-method management
- Authentication registration
- Identity verification
- Password recovery
- Group-scoped security deployment
- Pilot deployments
- Microsoft Authenticator
- Software OATH
- Email OTP
- Security-question lifecycle awareness
- Password-reset notifications
- Privileged-account monitoring
- Modern authentication architecture
- Legacy-to-modern configuration migration

---

# Security and Operational Observations

## 1. SSPR reduces help-desk dependency

Without SSPR:

```text
User forgets password
        ↓
Help-desk ticket
        ↓
Administrator verifies user
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

This can reduce support workload while allowing users to recover access more quickly.

---

## 2. SSPR is an IAM control, not only a convenience feature

The reset process must verify that the person requesting the reset is actually the legitimate account owner.

Therefore:

```text
Password reset
     ≠
Simply choosing a new password
```

It is an identity-verification process.

---

## 3. Strong recovery methods matter

A password reset mechanism can become a security weakness if attackers can easily bypass normal authentication through account recovery.

For this reason, recovery authentication methods must be protected carefully.

---

## 4. Pilot deployment reduces operational risk

I enabled SSPR only for:

```text
Project23
```

rather than immediately enabling it for the entire tenant.

This allows:

- Testing
- Troubleshooting
- User feedback
- Policy validation

before wider deployment.

---

## 5. Avoid implementing obsolete controls just to match training

The Microsoft lab referenced an older authentication-method configuration.

Instead of deliberately reintroducing older methods, I used the current Entra architecture.

This reflects a practical administrative principle:

> Understand the purpose of the control and implement it using the current supported platform design.

---

## 6. Notifications improve visibility

Password-reset notifications can help users and administrators detect unexpected password-management activity.

They provide an additional monitoring layer around identity recovery.

---

# Production Improvements I Would Consider

For a real production deployment, I would consider:

- Expanding SSPR gradually after successful pilot testing.
- Requiring stronger authentication methods.
- Reviewing authentication-method registration coverage.
- Monitoring SSPR usage and failures.
- Monitoring password-reset audit logs.
- Reviewing privileged-account password resets.
- Requiring additional verification for higher-risk identities where appropriate.
- Using Conditional Access together with modern authentication.
- Expanding passwordless authentication.
- Using Temporary Access Pass for controlled onboarding scenarios.
- Reviewing authentication-method policies regularly.
- Removing weak or obsolete authentication methods.
- Implementing stronger privileged identity controls.

---

# Connection to Previous Labs

This lab continues the identity-security environment I have been building.

### Lab 01 — Group Management

```text
Users
Groups
Dynamic membership
External identities
Group ownership
Licensing
```

### Lab 02 — Password Protection

```text
Smart Lockout
Banned passwords
Authentication protection
```

### Lab 03 — Self-Service Password Reset

```text
Password recovery
Authentication registration
Recovery verification
Notifications
```

Together:

```text
Identity Administration
        +
Password Protection
        +
Secure Password Recovery
        =
Stronger Identity Lifecycle
```

---

# Final Result

## ✅ Lab Completed Successfully

I successfully:

- Reviewed the initial SSPR configuration.
- Enabled Self-Service Password Reset.
- Scoped SSPR to the `Project23` pilot group.
- Reviewed the legacy SSPR authentication-method interface.
- Identified that Microsoft has moved authentication methods to the centralized Authentication Methods policy.
- Reviewed the modern authentication methods enabled in my tenant.
- Kept Microsoft Authenticator enabled.
- Kept Software OATH enabled.
- Kept Email OTP enabled.
- Intentionally did not enable SMS simply to reproduce an outdated training exercise.
- Did not enable Security Questions because Microsoft is retiring the feature.
- Configured one authentication method as required for password reset.
- Required authentication-method registration during sign-in.
- Changed authentication-information reconfirmation from 180 to 90 days.
- Enabled user password-reset notifications.
- Enabled administrator notifications for administrator password resets.
- Saved and verified the final configuration.

The most valuable part of this lab was learning how to translate an **older Microsoft training exercise into the current Microsoft Entra architecture** while preserving the security objective.

Rather than following outdated screenshots mechanically, I used the modern authentication-method management model and documented why certain legacy options were intentionally not enabled.

---

## Skills Practiced

`Microsoft Entra ID` · `IAM` · `SSPR` · `Self-Service Password Reset` · `Authentication Methods` · `Microsoft Authenticator` · `OATH` · `Email OTP` · `Identity Recovery` · `Password Reset` · `Authentication Registration` · `Privileged Identity Monitoring` · `Microsoft 365 Security` · `Identity Security`
