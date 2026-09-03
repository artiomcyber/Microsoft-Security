# Microsoft Entra — Identity & Access Management Labs

Hands-on Microsoft Entra ID and IAM labs completed in my **ExposedCybersecurity** Microsoft 365 Business Premium tenant.

This section documents practical work across user and group administration, password security, Self-Service Password Reset, multifactor authentication, and Conditional Access.

---

## Microsoft Applied Skills Credential ✅

### Get started with identities and access using Microsoft Entra

**Assessment result:** 100%  
**Passing score required:** 73%  
**Completed:** September 3, 2026

After completing the associated hands-on labs, I completed the official Microsoft Applied Skills assessment and achieved a **100% score**.

The assessment validated practical skills in:

- User management
- Group management
- Password protection
- Self-Service Password Reset (SSPR)
- Multifactor Authentication (MFA)
- Conditional Access

📄 **[View Microsoft Applied Skills Credential](./Microsoft%20Applied%20Skills%20Entra.pdf)**

---

## Hands-On Labs

| # | Lab | Key Skills | Status |
|---|---|---|---|
| 01 | [Basic Group Management](./01-Basic-Group-Management/) | Groups, dynamic membership, guest users, ownership, group licensing | ✅ Completed |
| 02 | [Basic Password Protection](./02-Basic-Password-Protection/) | Smart Lockout, banned passwords, password security | ✅ Completed |
| 03 | [Self-Service Password Reset](./03-Self-Service-Password-Reset/) | SSPR, authentication registration, recovery methods, notifications | ✅ Completed |
| 04 | [Multifactor Authentication](./04-Multifactor-Authentication/) | MFA, authentication methods, account lockout, suspicious activity reporting | ✅ Completed |
| 05 | [Conditional Access](./05-Conditional-Access/) | Conditional Access, report-only policies, What If analysis, application blocking | ✅ Completed |

---

## Lab Environment

```text
Microsoft 365 Business Premium
        ↓
Microsoft Entra ID P1
        ↓
ExposedCybersecurity Tenant
        ↓
Users & Groups
        ↓
Password Protection
        ↓
Self-Service Password Reset
        ↓
Multifactor Authentication
        ↓
Conditional Access
```

The environment was used as a practical Microsoft security lab rather than only following Microsoft Learn screenshots.

Where Microsoft training material referenced older interfaces or legacy controls, I reviewed the current Microsoft Entra implementation and adapted the configuration where appropriate.

---

## What I Configured

### Identity and Group Management

- Created Microsoft 365 and Security groups.
- Configured assigned and dynamic membership.
- Created a dynamic group for guest identities.
- Invited and managed an external B2B guest user.
- Configured group ownership.
- Assigned Microsoft 365 licensing through group membership.

### Password Protection

- Configured Smart Lockout.
- Changed the lockout threshold and duration.
- Enabled a custom banned-password list.
- Adapted Microsoft’s Contoso examples to the fictional **ExposedCybersecurity** company.
- Added organization-specific terms, project names, locations, and weak-password patterns.
- Configured password protection in Enforced mode.

### Self-Service Password Reset

- Enabled SSPR for the `Project23` pilot group.
- Reviewed the modern Authentication Methods policy.
- Avoided Security Questions because Microsoft is retiring the feature.
- Required authentication registration.
- Changed authentication-information reconfirmation from 180 to 90 days.
- Enabled password-reset notifications for users and administrators.

### Multifactor Authentication

- Tested legacy per-user MFA.
- Reviewed MFA Service Settings.
- Disabled app-password creation.
- Left Trusted IP MFA bypass disabled.
- Configured MFA account-lockout values.
- Enabled **Report suspicious activity**.
- Reviewed system-preferred authentication.
- Returned legacy per-user MFA to Disabled in preparation for Conditional Access enforcement.

### Conditional Access

- Created `CA-Block-Sway-ChrisGreen`.
- Targeted Chris Green.
- Excluded the administrator account.
- Targeted Microsoft Sway.
- Configured Block access.
- Used Report-only mode.
- Tested the policy using Conditional Access **What If**.
- Verified that the policy would apply and block access.

---

## Modernizing Older Microsoft Labs

Several of the Microsoft training exercises referenced older or legacy administration paths.

Instead of reproducing legacy configurations without review, I compared the lab instructions with the current Microsoft Entra implementation.

Examples included:

- Using the centralized **Authentication Methods policy** instead of older SSPR authentication checkboxes.
- Avoiding Security Questions because they are being retired.
- Disabling legacy app passwords.
- Using **Report suspicious activity** instead of older Fraud Alert controls.
- Returning per-user MFA to Disabled and preparing to enforce MFA using Conditional Access.
- Using Report-only Conditional Access policies before enforcement.

This allowed me to complete the learning objectives while also understanding the current Microsoft security architecture.

---

## Security Concepts Practiced

- Identity and Access Management
- User lifecycle administration
- Group-based access management
- Dynamic membership
- External identities and B2B collaboration
- Password protection
- Smart Lockout
- Self-Service Password Reset
- Multifactor Authentication
- Authentication Methods
- Conditional Access
- Zero Trust
- Least privilege
- MFA fatigue protection
- Account lockout and rate limiting
- Report-only policy deployment
- What If analysis
- Application access control
- Administrator lockout prevention

---

## Skills Demonstrated

`Microsoft Entra ID` · `Microsoft 365` · `IAM` · `Identity Administration` · `User Management` · `Group Management` · `Dynamic Groups` · `B2B Guest Access` · `Password Protection` · `Smart Lockout` · `SSPR` · `MFA` · `Authentication Methods` · `Conditional Access` · `What If Analysis` · `Zero Trust` · `Microsoft 365 Security`

---

## Module Progression

```text
Users & Groups
      ↓
Password Security
      ↓
Secure Account Recovery
      ↓
Multifactor Authentication
      ↓
Conditional Access
```

Each lab built on the previous one and gradually moved from basic identity administration toward policy-based identity security.

---

## Final Result

### ✅ Microsoft Applied Skills Module Completed

I completed all hands-on tasks associated with:

**Microsoft Applied Skills: Get started with identities and access using Microsoft Entra**

and passed the official assessment with:

# **100%**

The module provided practical experience across the core Microsoft Entra identity and access management workflow, from basic user and group administration through authentication security and Conditional Access.
