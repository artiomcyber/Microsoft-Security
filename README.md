# Microsoft-Security

Hands-on Microsoft security portfolio covering **Identity & Access Management (IAM), Microsoft Entra ID, Conditional Access, MFA, Microsoft Defender, Purview, compliance, and cloud security**.

The repository documents practical configurations, testing, troubleshooting, security decisions, and results from my Microsoft security lab environment.

---

## 🏆 Microsoft Applied Skills

### Microsoft Applied Skills: Get started with identities and access using Microsoft Entra

**Status:** ✅ Passed  
**Assessment score:** **100%**  
**Required to pass:** 73%  
**Completed:** September 3, 2026

The assessment validated practical skills across:

- User management
- Group management
- Password Protection
- Self-Service Password Reset (SSPR)
- Multifactor Authentication (MFA)
- Conditional Access

📄 **[View Microsoft Applied Skills Credential](./01-Identity-and-Access/Microsoft%20Applied%20Skills%20Entra.pdf)**

---

# Portfolio

## 01 — Identity & Access Management ✅

**Microsoft Entra ID / Microsoft 365**

Hands-on IAM environment built in a Microsoft 365 Business Premium tenant using the fictional organization:

```text
ExposedCybersecurity
```

### Completed labs

| # | Lab | Main Skills |
|---|---|---|
| 01 | [Basic Group Management](./01-Identity-and-Access/01-Basic-Group-Management/) | Groups, dynamic membership, guest users, ownership, group licensing |
| 02 | [Basic Password Protection](./01-Identity-and-Access/02-Basic-Password-Protection/) | Smart Lockout, banned passwords, password security |
| 03 | [Self-Service Password Reset](./01-Identity-and-Access/03-Self-Service-Password-Reset/) | SSPR, registration, recovery methods, notifications |
| 04 | [Multifactor Authentication](./01-Identity-and-Access/04-Multifactor-Authentication/) | MFA, authentication methods, lockout, suspicious activity reporting |
| 05 | [Conditional Access](./01-Identity-and-Access/05-Conditional-Access/) | Conditional Access, Report-only, What If analysis, application access control |

➡️ **[View complete Identity & Access portfolio](./01-Identity-and-Access/)**

---

## Identity Security Architecture Practiced

```text
Users & Groups
      ↓
Password Protection
      ↓
Self-Service Password Reset
      ↓
Multifactor Authentication
      ↓
Conditional Access
      ↓
Policy-Based Identity Security
```

The labs progressed from basic identity administration toward modern policy-based access control.

---

## Key Technologies

`Microsoft Entra ID` · `Microsoft 365` · `IAM` · `Identity Administration` · `Dynamic Groups` · `B2B Guest Access` · `Smart Lockout` · `SSPR` · `MFA` · `Authentication Methods` · `Conditional Access` · `What If Analysis` · `Zero Trust` · `Microsoft 365 Security`

---

## Modern Security Approach

Some Microsoft training exercises referenced older or legacy configuration paths.

Where appropriate, I compared the original lab instructions with the current Microsoft Entra implementation and documented the modern approach.

Examples include:

- Centralized **Authentication Methods Policy**
- Avoiding retiring Security Questions
- Disabling legacy app passwords
- Using **Report suspicious activity**
- Moving away from legacy per-user MFA
- Preparing MFA enforcement through **Conditional Access**
- Testing Conditional Access policies in **Report-only**
- Using **What If** analysis before enforcement

The objective was not only to complete each exercise, but also to understand how the controls should be implemented in a current Microsoft environment.

---

# Security Portfolio Roadmap

This repository will continue to expand into:

### Identity & Access
✅ Microsoft Entra ID  
✅ MFA  
✅ SSPR  
✅ Conditional Access  
✅ Group and guest identity management  

### Microsoft Defender
🔄 Defender XDR  
🔄 Defender for Endpoint  
🔄 Defender for Cloud Apps  
🔄 Incident investigation and response  

### Microsoft Purview & Compliance
🔄 Data Loss Prevention  
🔄 Information Protection  
🔄 Sensitivity labels  
🔄 Retention and compliance controls  

### Cloud Security
🔄 Microsoft Defender for Cloud  
🔄 Azure security controls  
🔄 Identity and workload protection  

---

## Lab Philosophy

For each exercise I aim to document:

```text
Requirement
    ↓
Configuration
    ↓
Security reasoning
    ↓
Testing
    ↓
Troubleshooting
    ↓
Verification
    ↓
Evidence
```

This repository is intended to demonstrate practical Microsoft security and IAM skills rather than only certification study.

---

## Current Achievement

> **Microsoft Applied Skills — Get started with identities and access using Microsoft Entra**
>
> **100% assessment score — September 2026**

The next stages of this portfolio will expand from **identity security** into Microsoft Defender, cloud security, governance, and compliance.
