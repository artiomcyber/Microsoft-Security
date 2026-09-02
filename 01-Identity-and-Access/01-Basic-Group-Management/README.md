# Microsoft Entra ID Lab — Perform Basic Group Management Tasks

**Status:** Completed ✅  
**Environment:** Microsoft Entra ID + Microsoft 365 Business Premium  
**Tenant:** ExposedCybersecurity  
**Date completed:** 2 September 2026  
**Microsoft lab:** [Exercise — Perform basic Group Management tasks](https://microsoftlearning.github.io/Get-started-Microsoft-Entra-Management-Tasks/Instructions/Labs/02-perform-basic-group-management.html)

## Overview

In this lab I practiced the basic group-management tasks that an IAM or Microsoft 365 administrator performs in Microsoft Entra ID. The exercise covered Microsoft 365 groups, security groups, assigned and dynamic membership, external guest identities, group ownership, and group-based license assignment.

I used my own Microsoft 365 Business Premium lab tenant instead of following the sample tenant exactly. This made the exercise more realistic because I had to adapt the Microsoft instructions to the users, licenses, and interface available in my environment.

## Objectives

The goals of the lab were to:

- Create a Microsoft 365 group with assigned membership.
- Create a Security group with dynamic user membership.
- Build a dynamic rule for guest accounts.
- Add an external guest identity to a Microsoft 365 group.
- Understand the difference between manual and automatic group membership.
- Review group ownership.
- Assign a Microsoft 365 license through group membership.
- Verify the result from both the group and user side.

---

## Task 1 — Create the `Project23` Microsoft 365 group

I created a new Microsoft 365 group named **Project23**.

Configuration:

| Setting | Value |
|---|---|
| Group type | Microsoft 365 |
| Group name | Project23 |
| Membership type | Assigned |
| Initial member | Sophie Williams |

The important point here is that **Assigned** membership is controlled manually. An administrator or group owner decides which users are members.

A Microsoft 365 group can also be used as the identity layer behind collaboration services such as a shared mailbox, calendar, SharePoint resources, and Microsoft Teams.

### Result

`Project23` was successfully created and appeared in the Entra ID group inventory as a Microsoft 365 group with Assigned membership.

![Groups overview](images/01-groups-overview.png)

---

## Task 2 — Create a dynamic Security group for guest users

Next, I created a Security group named **Guest Users**.

Configuration:

| Setting | Value |
|---|---|
| Group type | Security |
| Group name | Guest Users |
| Membership type | Dynamic User |
| Dynamic property | userType |
| Operator | Equals |
| Value | Guest |

The rule is conceptually:

```text
userType = Guest
```

This means I do not manually maintain the membership of this group. Microsoft Entra evaluates user attributes and automatically places any identity with a `Guest` user type into the group.

### Why this matters

This is one of the fundamental differences between **assigned** and **dynamic** groups:

- **Assigned group:** membership is explicitly managed by an administrator or owner.
- **Dynamic group:** membership is calculated automatically from user or device attributes.

Dynamic groups can reduce administrative work and make access management more consistent when they are based on reliable identity attributes.

### Result

After an external guest account was created, Entra processed the rule successfully and automatically added one guest to the `Guest Users` group.

The portal showed:

- Membership type: **Dynamic**
- Group type: **Security**
- Direct users: **1**
- Dynamic rules processing status: **Succeeded**

![Dynamic guest users group](images/02-dynamic-guest-users.png)

---

## Task 3 — Create and add an external guest user

The Microsoft exercise expected an existing external user to already be available. My tenant did not have one, so I created a real guest identity using an external Gmail account.

The external identity was created as:

**Display name:** `Artiom Pankrashkin2`  
**User type:** `Guest`

Microsoft Entra represents a B2B guest inside the tenant with an external-style user principal name containing `#EXT#`. The guest keeps their external identity but is represented as an object inside my tenant so that access can be assigned and controlled.

### Dynamic membership verification

Because the new account had `User type = Guest`, it was automatically included in the `Guest Users` dynamic Security group without any manual membership action.

This verified that the dynamic membership rule was working as intended.

### Assigned membership in `Project23`

I then manually added the same guest account to `Project23`.

The final membership of `Project23` included:

- Sophie Williams — internal tenant member
- Artiom Pankrashkin2 — external guest

![Project23 members](images/03-project23-members.png)

This gave me a useful comparison inside one lab:

```text
Guest Users
└── Guest added automatically by dynamic rule

Project23
└── Guest added manually by assigned membership
```

---

## Task 4 — Group ownership

The lab also covers group ownership. Group owners can manage parts of the group's lifecycle and membership without requiring broad tenant-wide administrative permissions.

Microsoft also notes that when no explicit owner is selected during group creation, the tenant administrator creating the group can become the owner automatically.

From an IAM perspective, ownership matters because groups should have clear accountability. In a production environment I would normally ensure that important groups have appropriate business or technical owners and avoid leaving groups orphaned.

---

## Task 5 — Assign a license to a group

The original Microsoft lab instructed me to assign **Microsoft Power Automate Free** to `Project23`.

My tenant did not contain that product as a separate assignable subscription. The only available organization license shown in the Microsoft 365 admin center was **Microsoft 365 Business Premium**.

Rather than stop the lab, I adapted the exercise and used Microsoft 365 Business Premium to practice the same IAM concept: **group-based licensing**.

### Before the change

The tenant showed:

- Microsoft 365 Business Premium licenses purchased: **25**
- Assigned: **6**
- Available: **19**

![License overview before assignment](images/04-license-overview-before.png)

### License assignment

I assigned **Microsoft 365 Business Premium** to the `Project23` group from the Microsoft 365 admin center.

The portal confirmed:

> Your licenses are being assigned

After processing, the tenant showed **7 of 25 licenses assigned**.

![Group-based license assignment](images/05-group-license-assignment.png)

### Verification from the guest account

The guest account showed membership in both:

- `Guest Users`
- `Project23`

![Guest group memberships](images/06-guest-memberships.png)

The guest account also showed **Microsoft 365 Business Premium** under **Licenses and apps**, confirming that the group-based assignment had taken effect in my lab environment.

![Guest license](images/07-guest-license.png)

### What I learned from this

Group-based licensing allows an administrator to connect licensing to group membership instead of manually licensing every user one at a time.

Conceptually:

```text
User
  ↓
Member of licensed group
  ↓
Group has Microsoft 365 license assigned
  ↓
License is inherited by eligible group members
```

This can make onboarding and license administration much easier at scale.

In a real production environment I would not automatically assign a full Business Premium license to every guest simply because they are external. Licensing should be based on actual business and service requirements. For this lab, the assignment was intentional so I could test and verify the group-based licensing process.

---

## Troubleshooting and adaptations

This lab was useful because the current tenant did not match the Microsoft instructions exactly.

### 1. External user was not available

The lab expected an `External User` to already exist. My tenant only contained internal users.

**Solution:** I invited a real external Gmail identity as a Microsoft Entra B2B guest and then added it to `Project23`.

### 2. Power Automate Free license was not available

The Microsoft instructions referenced a `Microsoft Power Automate Free` license, but my Microsoft 365 admin center only displayed Microsoft 365 Business Premium.

**Solution:** I used Microsoft 365 Business Premium to complete the same group-based licensing exercise.

### 3. Microsoft admin interfaces differ from the lab screenshots

The current Microsoft 365 and Entra admin center interfaces have changed compared with some of the training instructions.

**Solution:** I located the equivalent current controls and completed the intended administrative tasks rather than relying only on the exact position of buttons shown in the training material.

This was a useful reminder that real administration requires understanding the **goal of a configuration**, not just memorizing where a button is located.

---

## IAM concepts demonstrated

By completing the lab I practiced the following identity and access management concepts:

- Microsoft Entra ID user and group administration
- Microsoft 365 groups
- Security groups
- Assigned group membership
- Dynamic user membership
- Attribute-based membership rules
- B2B / external guest identities
- Guest versus Member user types
- Group ownership
- Group-based licensing
- License verification
- Basic identity lifecycle administration
- Administrative troubleshooting

---

## Security and operational observations

A few practical lessons stood out during the exercise:

1. **Dynamic membership reduces manual administration.** If identity attributes are maintained correctly, access and group membership can follow those attributes automatically.
2. **Guest identities still need governance.** External accounts should have a business reason, a defined owner, appropriate access, and eventually an expiration or review process.
3. **Licensing should follow business need.** A technically possible license assignment is not automatically a good production design.
4. **Groups are an important control point.** Membership can affect collaboration, application access, licensing, and later Conditional Access or other security policies.
5. **Verification matters.** I checked the result from both the group view and the individual user view rather than assuming the configuration had succeeded.

---

## Production improvements I would make

If I were implementing this for a real organization rather than a training tenant, I would improve the design by:

- Using a documented naming convention for groups.
- Separating collaboration groups from dedicated licensing groups where appropriate.
- Assigning at least two responsible owners to important groups.
- Periodically reviewing guest accounts and guest group memberships.
- Removing licenses from guests unless the services genuinely require them.
- Using access reviews or lifecycle processes for long-lived external identities.
- Monitoring audit logs for membership, ownership, and licensing changes.

---

## Final result

**Lab completed successfully.**

I created and managed both Microsoft 365 and Security groups, configured dynamic guest membership, created and managed a B2B guest identity, added the guest to an assigned Microsoft 365 group, reviewed group ownership, and successfully tested group-based Microsoft 365 Business Premium licensing.

The most valuable part of the exercise was seeing the difference between **manual identity administration** and **policy-driven identity administration** in a real Microsoft 365 tenant.

---

## Skills practiced

`Microsoft Entra ID` · `Microsoft 365` · `IAM` · `Group Management` · `Dynamic Groups` · `B2B Guest Access` · `Group-Based Licensing` · `Identity Lifecycle` · `Microsoft 365 Administration`
