# Microsoft Entra ID Lab — Perform Basic Password Protection Tasks

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft Lab:** [Perform basic Password Protection tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/03-perform-basic-password-protection.html)

---

## Overview

In this lab, I configured Microsoft Entra Password Protection to strengthen authentication security in my Microsoft 365 lab tenant.

The exercise focused on two important controls:

- **Smart Lockout**
- **Custom banned passwords**

Instead of copying Microsoft's sample Contoso configuration exactly, I adapted the exercise to my fictional company, **ExposedCybersecurity**, and created a custom password-protection policy based on names, locations, project terms, administrative terminology, and common weak-password patterns relevant to the environment.

The final configuration included:

- Smart Lockout threshold reduced from `10` to `5`
- Lockout duration reduced from `60` to `30 seconds`
- Custom banned-password protection enabled
- Organization-specific password terms added
- Password protection for Windows Server Active Directory enabled
- Protection mode changed from `Audit` to `Enforced`

---

# Objectives

The objectives of this lab were to:

- Review the existing Microsoft Entra Password Protection configuration.
- Understand how Smart Lockout protects against repeated password-guessing attempts.
- Configure a custom account lockout threshold.
- Configure the lockout duration.
- Enable a custom banned-password list.
- Adapt Microsoft's sample password list to my own fictional company.
- Understand the purpose of Microsoft's global banned-password protection.
- Configure password protection in **Enforced** mode.
- Verify that the final settings were saved successfully.

---

# Task 1 — Review the Existing Password Protection Configuration

I first opened:

```text
Microsoft Entra admin center
        ↓
Entra ID
        ↓
Authentication methods
        ↓
Password protection
```

Before making changes, the tenant was configured with the following values:

| Setting | Initial value |
|---|---:|
| Lockout threshold | 10 |
| Lockout duration | 60 seconds |
| Enforce custom banned-password list | No |
| Custom banned-password list | Empty |
| Windows Server Active Directory password protection | Yes |
| Mode | Audit |

![Initial Password Protection configuration](Password%20protection.png)

This gave me a clear baseline before modifying the security controls.

---

# Task 2 — Configure Smart Lockout

I changed the Smart Lockout settings to:

| Setting | New value |
|---|---:|
| Lockout threshold | **5** |
| Lockout duration | **30 seconds** |

### Lockout Threshold

The lockout threshold determines how many failed authentication attempts can occur before Microsoft Entra temporarily locks the account from further password authentication attempts.

I configured:

```text
Lockout threshold = 5
```

This makes the environment more restrictive than the original threshold of 10.

Conceptually:

```text
Wrong password
      ↓
Attempt 1
      ↓
Attempt 2
      ↓
Attempt 3
      ↓
Attempt 4
      ↓
Attempt 5
      ↓
Smart Lockout
```

The objective is to make automated password guessing and brute-force activity more difficult while still allowing legitimate users to recover from occasional typing mistakes.

---

## Lockout Duration

I configured:

```text
Lockout duration = 30 seconds
```

This means that after the Smart Lockout threshold is reached, password authentication is temporarily restricted according to the policy.

Smart Lockout is designed to provide protection against repeated authentication attempts without relying solely on traditional permanent account-lockout behavior.

---

# Task 3 — Enable Custom Banned Passwords

I enabled:

```text
Enforce custom list = Yes
```

This allowed me to extend Microsoft Entra's password protection with terminology specific to my fictional organization.

![Password Protection configuration being edited](Password%20protection%20edited.png)

---

# Adapting the Microsoft Lab to ExposedCybersecurity

The original Microsoft training lab uses fictional **Contoso** terminology.

Rather than copying company names that had no relationship to my environment, I adapted the exercise to my own fictional organization:

```text
ExposedCybersecurity
```

This made the exercise more representative of how password protection would actually be configured in a real company.

A real organization should consider banning passwords based on terms such as:

- Company name
- Company abbreviations
- Product names
- Project names
- Office locations
- Department names
- Internal systems
- Administrative terminology
- Common organizational phrases

These words are attractive to attackers because employees often use familiar company-related terms when creating passwords.

---

# Custom Banned-Password List

For the lab I configured the following list:

```text
Exposed
Cybersecurity
ExposedCyber
ExposedSec
Exposed2026
Exposed2027
Cyber2026
Cyber2027
Security2026
Security2027
Stockholm
Sweden
Bromma
Admin
Administrator
Helpdesk
Welcome
Company
Project23
Marketing
Finance
Password
Passw0rd
Qwerty
Letmein
Welcome1
Admin123
ChangeMe
Summer
Winter
Spring
Autumn
September
January
```

The list contains several different categories of terms.

---

## 1. Company-Specific Terms

```text
Exposed
Cybersecurity
ExposedCyber
ExposedSec
```

These terms represent the company name and possible abbreviations.

Passwords based directly on an organization's name can be easier for attackers to guess because the organization itself is public information.

Examples of passwords I wanted to discourage include:

```text
Exposed123!
Cybersecurity2026
ExposedSec1!
```

---

## 2. Company Name + Year Patterns

```text
Exposed2026
Exposed2027
Cyber2026
Cyber2027
Security2026
Security2027
```

Users often create predictable passwords by combining familiar words with the current year.

For example:

```text
Company2026!
Security2026!
Exposed2026!
```

These passwords may technically satisfy traditional complexity requirements while still being predictable.

---

## 3. Location-Based Terms

```text
Stockholm
Sweden
Bromma
```

Locations associated with a company or its employees can also become predictable password components.

This is especially relevant when office locations or headquarters information is publicly available.

---

## 4. Administrative and IT Terms

```text
Admin
Administrator
Helpdesk
```

Administrative terminology is particularly important because privileged and IT-related accounts are high-value targets.

Attackers commonly test predictable combinations based on account function.

---

## 5. Internal Business Terms

```text
Company
Project23
Marketing
Finance
```

`Project23` was intentionally included because it was created during my previous Microsoft Entra group-management lab.

This connects the labs together and demonstrates how internal project names can become password risks if employees use familiar business terminology when creating credentials.

---

## 6. Common Weak Password Patterns

I also included examples such as:

```text
Password
Passw0rd
Qwerty
Letmein
Welcome
Welcome1
Admin123
ChangeMe
```

These are representative of predictable password patterns that users frequently choose.

However, an important lesson from this lab is that I **would not try to manually reproduce every known weak or compromised password in the custom list**.

Microsoft Entra already maintains its own global banned-password protection designed to detect commonly used and weak password patterns.

The custom list is therefore most valuable for adding **organization-specific context** that Microsoft could not automatically know about my company.

---

# Global vs Custom Banned Password Protection

The password-protection model can be understood as two complementary layers.

```text
                   Password attempt
                         ↓
              Microsoft Entra evaluates
                         ↓
        ┌────────────────┴────────────────┐
        ↓                                 ↓
Global banned-password list      Custom banned-password list
        ↓                                 ↓
Known/common weak terms          Company-specific terms
        └────────────────┬────────────────┘
                         ↓
                 Password evaluation
```

### Global protection

Microsoft provides protection against broadly known weak and predictable passwords.

### Custom protection

The organization adds terminology specific to its own environment.

Examples:

```text
Company name
Project names
Locations
Internal brands
Products
Departments
Abbreviations
```

The two mechanisms complement each other.

---

# Task 4 — Configure Enforced Mode

The original configuration showed:

```text
Mode = Audit
```

I changed this to:

```text
Mode = Enforced
```

The difference is important.

### Audit Mode

```text
Password evaluated
       ↓
Policy violation detected
       ↓
Event recorded
       ↓
Password is not actively blocked by this mode
```

Audit mode is useful when an organization wants to understand the impact of a policy before enforcing it.

### Enforced Mode

```text
Password evaluated
       ↓
Policy violation detected
       ↓
Password rejected
```

Enforced mode actively applies the password-protection policy.

For this lab I selected:

**Enforced**

because the objective was to implement active password protection.

---

# Task 5 — Windows Server Active Directory Password Protection

The configuration also showed:

```text
Enable password protection on Windows Server Active Directory = Yes
```

This setting relates to extending Microsoft Entra Password Protection capabilities to supported on-premises Active Directory environments.

My current lab is primarily cloud-based, so I did not deploy the required on-premises components as part of this exercise.

However, reviewing this setting helped me understand that Microsoft Entra Password Protection can also participate in hybrid identity security architectures.

Conceptually:

```text
Microsoft Entra Password Protection
              ↓
      Password policy data
              ↓
   On-premises AD components
              ↓
Windows Server Active Directory
```

This becomes important in organizations using hybrid identity.

---

# Final Configuration

After completing the changes, my final configuration was:

| Security Control | Configuration |
|---|---|
| Smart Lockout threshold | **5** |
| Lockout duration | **30 seconds** |
| Custom banned passwords | **Enabled** |
| Organization-specific list | **Configured** |
| Windows Server AD password protection | **Enabled** |
| Protection mode | **Enforced** |

![Final Password Protection configuration](Password%20protection%20configured.png)

The disabled **Save** button in the final view confirmed that the configuration had already been saved and there were no unsaved changes remaining.

---

# Before vs After

| Setting | Before | After |
|---|---:|---:|
| Lockout threshold | 10 | **5** |
| Lockout duration | 60 sec | **30 sec** |
| Custom banned-password list | Disabled | **Enabled** |
| Custom banned terms | None | **Configured** |
| Windows Server AD protection | Enabled | **Enabled** |
| Mode | Audit | **Enforced** |

---

# Security Concepts Demonstrated

By completing this lab, I practiced and reviewed:

- Microsoft Entra Password Protection
- Authentication security
- Smart Lockout
- Account lockout thresholds
- Password-guessing protection
- Brute-force mitigation
- Password spraying concepts
- Custom banned passwords
- Global banned-password protection
- Organization-specific password policies
- Hybrid identity password protection
- Audit vs Enforced configuration
- Microsoft Entra administration
- Security-policy customization

---

# Security Observations

## 1. Password complexity does not automatically mean password strength

A password such as:

```text
ExposedCybersecurity2026!
```

may appear complex because it contains:

- Uppercase characters
- Lowercase characters
- Numbers
- Special characters

However, it is still highly predictable because it is based on:

```text
Company name + year
```

Password protection therefore needs to consider predictability, not just traditional complexity requirements.

---

## 2. Attackers know company information

An attacker can often discover information such as:

```text
Company name
Office locations
Departments
Product names
Employee names
Project terminology
```

through public websites, LinkedIn, job advertisements, social media, leaked data, or reconnaissance.

Organization-specific banned passwords help reduce the usefulness of this information during password guessing.

---

## 3. Custom lists should complement Microsoft protection

The goal is not to maintain a gigantic manual dictionary of every breached password.

That would be difficult to maintain and would duplicate Microsoft's global password-protection functionality.

Instead:

```text
Microsoft
    ↓
Protects against broadly weak passwords

Organization
    ↓
Adds organization-specific terminology
```

This creates a more maintainable approach.

---

## 4. Lockout policies require balance

A very high lockout threshold may allow too many password guesses.

A threshold that is too aggressive could negatively affect legitimate users.

The appropriate configuration therefore depends on:

- Threat model
- User population
- Authentication architecture
- MFA deployment
- Conditional Access
- Identity Protection
- Operational requirements

The `5 / 30-second` configuration used here followed the Microsoft training exercise and was suitable for demonstrating the control in my lab.

---

## 5. Password protection is only one security layer

Strong password policies alone are not sufficient.

A stronger identity architecture combines controls such as:

```text
Password Protection
        +
MFA
        +
Conditional Access
        +
Risk Detection
        +
Least Privilege
        +
Monitoring
        =
Stronger Identity Security
```

This lab therefore represents one layer of the broader Microsoft identity-security architecture I am building.

---

# Production Improvements I Would Consider

If I were implementing password protection for a real organization, I would:

- Identify actual organization-specific terminology.
- Include company and product abbreviations.
- Include important project names.
- Include office and geographic names where appropriate.
- Include internal platform names.
- Review the custom banned-password list periodically.
- Remove outdated terms where appropriate.
- Add new product or project names.
- Monitor authentication failures.
- Review Smart Lockout behavior.
- Combine password protection with MFA.
- Implement Conditional Access.
- Use passwordless authentication where appropriate.
- Monitor risky authentication activity.
- Test policy changes before broad production rollout.

---

# Connection to My Previous Lab

This exercise builds directly on my previous **Basic Group Management** lab.

In that lab I configured:

```text
Users
Groups
Dynamic membership
Guest access
Group ownership
Group-based licensing
```

This lab adds another identity-security layer:

```text
Identity Administration
          ↓
Password Protection
          ↓
Authentication Security
```

As I continue through the Microsoft labs, these individual configurations will gradually form a more complete Microsoft identity and security environment.

---

# Final Result

## ✅ Lab Completed Successfully

I successfully:

- Reviewed the original Microsoft Entra Password Protection configuration.
- Changed the Smart Lockout threshold from `10` to `5`.
- Changed the lockout duration from `60` to `30 seconds`.
- Enabled the custom banned-password list.
- Adapted Microsoft's Contoso-based exercise to my own fictional **ExposedCybersecurity** company.
- Created an organization-specific banned-password list.
- Included company, project, location, administrative, and weak-password terminology.
- Reviewed the relationship between Microsoft's global banned-password protection and organization-specific custom protection.
- Changed password protection from **Audit** to **Enforced** mode.
- Reviewed Windows Server Active Directory password-protection integration.
- Saved and verified the final configuration.

The most useful part of this lab was understanding that password security is not simply about requiring uppercase letters, numbers, and symbols.

Effective password protection also requires identifying **predictable passwords based on organizational context** and combining that protection with broader identity-security controls.

---

## Skills Practiced

`Microsoft Entra ID` · `Microsoft 365` · `IAM` · `Password Protection` · `Smart Lockout` · `Authentication Security` · `Custom Banned Passwords` · `Password Security` · `Brute-Force Protection` · `Password Spraying` · `Hybrid Identity` · `Microsoft Entra Administration` · `Identity Security`
