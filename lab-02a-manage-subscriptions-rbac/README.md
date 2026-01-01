# Lab 02a — Manage Subscriptions and RBAC ![](https://img.shields.io/badge/English-EN-green)

🇵🇹 [Versão em Português](README.pt.md)

![Module: Administer Governance & Compliance](https://img.shields.io/badge/Module-Administer%20Governance%20%26%20Compliance-0078D4?logo=microsoft&logoColor=white)

> 📘 **AZ-104 Hands-on Lab (English)**  
> Part of my practical preparation for the Microsoft Azure Administrator certification.

## Overview

This lab focuses on **Azure governance and access control**, specifically:

- Managing subscriptions using **Management Groups**
- Understanding **RBAC scope inheritance**
- Assigning **built-in Azure roles** to groups
- Creating **Custom RBAC roles** to refine permissions
- Applying the **principle of least privilege**

These tasks are core objectives of the **AZ-104: Microsoft Azure Administrator** exam.

---

## Prerequisites

- An active Azure subscription
- Global Administrator or Owner permissions
- Microsoft Entra ID tenant access

---

## Lab Objectives

By completing this lab, you will be able to:

- Validate subscription status and ownership
- Enable tenant-level access management
- Create and manage **Management Groups**
- Create a **Help Desk security group**
- Assign the **Virtual Machine Contributor** role at Management Group scope
- Create a **Custom RBAC Role** with specific permission exclusions
- Verify RBAC assignments and inheritance

---

## Evidence (Screenshots)

All screenshots are stored in the `./screenshots/` directory and validate the successful execution of each task.

---

## 1️⃣ Subscription Validation

### Subscription active and ownership confirmed

![Subscription active](screenshots/00-subscription-active.png)

This screenshot confirms that:

- The subscription is **Active**
- The logged-in user has the **Owner** role
- The subscription is associated with the correct tenant

---

## 2️⃣ Tenant Access Management Enabled

### Microsoft Entra ID — Access management for Azure resources

![Entra ID access management](screenshots/01-entra-id-properties-access-management.png)

This screenshot confirms that:

- **Access management for Azure resources** is enabled
- The user can manage access across **subscriptions and management groups**
- Tenant-level permissions are correctly configured

---

## 3️⃣ Management Groups

### Management Groups overview

![Management groups overview](screenshots/01-management-groups-overview.png)

This screenshot shows:

- The **Tenant Root Group**
- Existing subscriptions
- Centralized governance entry point

---

### Management Group creation

![Create management group](screenshots/02-management-group-create-form.png)

A management group was created with the following configuration:

- **Management Group ID:** `az104-mg1`
- **Display name:** `az104-mg1`
- Parent: **Tenant Root Group**

---

### Management Group hierarchy validation

![Management group hierarchy](screenshots/03-management-group-hierarchy.png)

This screenshot confirms that:

- `az104-mg1` exists
- It is correctly placed under the **Tenant Root Group**
- Subscriptions can inherit policies and RBAC from this scope

---

## 4️⃣ Help Desk Security Group

### Help Desk group created

![Help Desk group](screenshots/04-help-desk-group-created.png)

This screenshot confirms the creation of a security group with:

- **Name:** Help Desk
- **Group type:** Security
- **Membership type:** Assigned
- Intended use: Support team access delegation

---

## 5️⃣ RBAC Role Assignment

### Virtual Machine Contributor assigned at Management Group scope

![RBAC assignment](screenshots/05-rbac-vm-contributor-assigned.png)

This screenshot confirms that:

- The **Help Desk** group is assigned the **Virtual Machine Contributor** role
- Scope: **Management Group (`az104-mg1`)**
- Permissions are inherited by all subscriptions under the group

This configuration allows Help Desk users to:

- Create and manage virtual machines
- Perform VM-related operations
- Without granting full administrative access

---

## 6️⃣ Custom RBAC Role Creation

### Creating the "Custom Support Request" Role

![Custom Role Basics](screenshots/07-custom-role-basics-clone.png)

To strictly apply the **principle of least privilege**, a custom role was created by cloning a built-in role.

- **Role Name:** Custom Support Request
- **Baseline Permissions:** Cloned from **Support Request Contributor**
- **Description:** A custom contributor role for support requests.

---

### Excluding Sensitive Permissions

![Exclude Permissions](screenshots/08-custom-role-exclude-register-provider.png)

The standard "Support Request Contributor" role allows registering resource providers, which is an elevated privilege not required for daily Help Desk tasks.

- **Excluded Permission:** `Microsoft.Support/register/action`
- **Result:** This permission is added to `NotActions`, explicitly preventing the Help Desk from registering the Support Resource Provider.

---

### Defining Assignable Scopes

![Assignable Scopes](screenshots/09-custom-role-assignable-scopes.png)

The custom role is restricted to a specific scope to prevent usage elsewhere in the tenant.

- **Scope:** `az104-mg1` (Management Group)
- This ensures the role is only available for assignment within this specific management group hierarchy.

---

### JSON Review and Validation

![JSON Review](screenshots/10-custom-role-review-json.png)

The final JSON definition validates the customization:

```json
"permissions": [
    {
        "actions": [
            "Microsoft.Authorization/*/read",
            "Microsoft.Resources/subscriptions/resourceGroups/read",
            "Microsoft.Support/*"
        ],
        "notActions": [
            "Microsoft.Support/register/action"
        ],
        "assignableScopes": [
            "/providers/Microsoft.Management/managementGroups/az104-mg1"
        ]
    }
]
```

This custom role ensures that Help Desk users can manage support requests
without gaining elevated privileges that could impact tenant governance.

---

## 7️⃣ Monitor Role Assignments (Activity Log)

![Custom Role Basics](screenshots/11-activity-log-rbac-events.png)

### Creating the "Custom Support Request" Role

# Reviewing Role Assignment Events

To validate and audit the RBAC changes performed during this lab, the
**Azure Activity Log** was used. The Activity Log records
subscription-level and management group-level operations, including role
assignments and custom role creation.

---

## Steps taken

1.  Navigated to **Management Groups** → `az104-mg1`.
2.  Selected **Activity log** from the left menu.
3.  Filtered the log to isolate relevant events:
    - **Category:** `Administrative`
    - **Operation:** `Create role assignment` and
      `Create or update custom role`

---

## Validation

The screenshot confirms that:

- The **Virtual Machine Contributor** role assignment to the Help Desk
  group was logged.
- The creation of the **Custom Support Request** role was recorded.
- All administrative changes are traceable for governance and auditing
  purposes.

## ✅ Lab Validation Summary

The following requirements were successfully met:

- [x] Subscription validated and active
- [x] Tenant-level access management enabled
- [x] Management Group created and verified
- [x] Help Desk security group created
- [x] RBAC role assigned at correct scope
- [x] Custom RBAC role created (Cloned & Modified)
- [x] Role assignments audited via Activity Log

---

## 🔑 Key Takeaways

- **Management Groups** enable scalable governance across
  subscriptions.
- **RBAC inheritance** simplifies access control.
- **Custom Roles** are essential when built-in roles grant too many
  permissions (over-privileged).
- **Activity Log** is a critical governance tool that supports
  security audits by tracking _who changed what and when_.
- Always grant permissions at the **lowest required scope**.

---

## 📝 Exam Tips (AZ-104)

- Management Groups sit **above subscriptions**.
- RBAC assignments inherit **downwards**.
- Custom Roles require defining **Assignable Scopes** (can be MG,
  Subscription, or Resource Group).
- **NotActions** are subtractive: even if an Action grants `*` (all),
  a `NotAction` will block that specific operation.
- **Activity Log retention** is **90 days** by default.
- Activity Log records **control-plane** operations (Administrative),
  not data-plane.

---

## 📌 Lab Status

✅ **Completed**
