# Lab 02b — Manage Governance via Azure Policy ![](https://img.shields.io/badge/English-EN-green)

🇵🇹 [Versão em Português](README.pt.md)

![Module: Administer Governance & Compliance](https://img.shields.io/badge/Module-Administer%20Governance%20%26%20Compliance-0078D4?logo=microsoft&logoColor=white)

> 📘 **AZ-104 Hands-on Lab**  
> Part of my practical preparation for the Microsoft Azure Administrator certification.

## Overview

In this lab, you learn how to implement your organization’s governance plans. You learn how Azure policies can ensure operational decisions are enforced across the organization. You learn how to use resource tagging to improve reporting.

This lab focuses on implementing **Azure governance controls** using **Azure Policy**, **resource tagging**, and **resource locks**.

You will learn how to:

- Apply **resource tags** for **governance** and **cost management**
- Enforce **tagging rules** using **Azure Policy**
- Automatically **remediate missing tags**
- Protect resources using **Azure resource locks**

These tasks are core objectives of the **AZ-104: Microsoft Azure Administrator** certification.

## Estimated Time

⏱️ 30 minutes

## Prerequisites

- An active Azure subscription
- Owner or Contributor permissions on the subscription
- Access to the Azure portal
- Subscription associated with a Management Group (recommended)

> ⚠️ Note: The lab steps reference **East US**, but you may use another region if needed.

## Lab Scenario

Your organization’s cloud footprint has grown significantly over the past year.
A recent audit revealed that many Azure resources do not have a defined:

- Owner
- Project
- Cost center

To improve governance and resource management, you decide to implement the following:

- Apply resource tags to attach metadata to resources
- Enforce mandatory tagging using Azure Policy
- Automatically update existing resources with missing tags
- Protect critical resources using resource locks

## Architecture Diagram

![Architecture Diagram](screenshots/az104-lab02b-architecture.png)

The following diagram represents the governance flow implemented in this lab:

**Explanation:**

- Tags are defined at the resource group level
- Azure Policy enforces tag compliance
- Non-compliant resources are blocked or remediated
- Resource locks protect configured resources

## Evidence (Screenshots)

All screenshots are stored in the `./screenshots/` directory and validate the successful execution of each task.

## Job Skills Practiced

- Create and assign resource tags
- Enforce governance rules using Azure Policy
- Apply automatic remediation for non-compliant resources
- Configure and test Azure resource locks

## Task 1️⃣ — Assign Tags via the Azure Portal

### Objective

In this task, you will create and assign a tag to an Azure resource group via the Azure portal. Tags are a critical component of a governance strategy as outlined by the Microsoft Well-Architected Framework and Cloud Adoption Framework. Tags can allow you to quickly identify resource owners, sunset dates, group contacts, and other name/value pairs that your organization deems important. For this task, you assign a tag identifying the resource Cost Center.

Create a resource group and assign a Cost Center tag using the Azure portal.
Tags are a critical governance mechanism, aligned with the:

- Microsoft Well-Architected Framework
- Cloud Adoption Framework
  They allow organizations to track ownership, cost, and responsibility.

### Steps

1. Sign in to the **Azure portal** [https://portal.azure.com](https://portal.azure.com)
2. Search for and select **Resource groups**
3. Select **+ Create**
4. Configure the resource group:

| Setting             | Value             |
| :------------------ | :---------------- |
| Subscription        | Your subscription |
| Resource group name | `az104-rg2`       |
| Location            | East US           |

> ℹ️ **Note** > Each lab uses a dedicated resource group to simplify management and cleanup.

5. Select **Next: Tags**
6. Add the following tag:

| Name        | Value |
| :---------- | :---- |
| Cost Center | 000   |

7. Select **Review** + **Create**, then **Create**

![Imagem dos Passos](screenshots/task1-resource-group-tags.png)

This screenshot confirms that:

🔹 **Resource Group**

- **Name:** az104-rg2 ✅
- **Subscription:** Azure-Labs-PayG ✅
- **Region:** East US ✅

### Validation

- The resource group az104-rg2 is created successfully
- The Cost Center: 000 tag is visible on the resource group

This tag will later be used by Azure Policy for enforcement and remediation.

### 🧠 How this connects to Task 2 (important)

This step is **correctly done** because:

- How this connects to **Task 2** will depend on this **Cost Center** existing
- The **Azure Policy** will:
  - Block **untagged** resources
  - Or inherit the tag from the RG (in Task 3).

If Task 1 was done poorly, **everything else would fail**.

## Task 2️⃣ — Enforce Tagging via Azure Policy

### Objective

Assign a built-in **Azure Policy** to enforce the presence of a mandatory
**Cost Center** tag on all resources within the resource group.

This ensures that governance rules are **automatically enforced**, preventing
the creation of non-compliant resources.

---

### Policy Overview

The built-in Azure Policy **Require a tag and its value on resources** is used to:

- Validate the presence of a specific tag
- Validate the tag value
- **Deny resource creation** if the requirement is not met

This policy enforces governance **at deployment time**, preventing configuration drift.

---

### Policy Behavior

- Azure Policy evaluates resources **before deployment**
- If a resource is created **without the required tag**, the deployment is denied
- Enforcement occurs automatically and consistently

---

### Policy Assignment

The policy was assigned with the following configuration:

| Setting            | Value                                              |
| ------------------ | -------------------------------------------------- |
| Policy definition  | Require a tag and its value on resources           |
| Scope              | Resource Group `az104-rg2`                         |
| Assignment name    | Require Cost Center tag and its value on resources |
| Policy enforcement | Enabled                                            |
| Tag name           | Cost Center                                        |
| Tag value          | 000                                                |

> ℹ️ The policy is scoped at the **resource group level** to ensure that all resources created within the group comply with tagging requirements.

---

### Implementation Steps

1. In the **Azure portal**, search for and select **Policy**.

2. In **Authoring**, select **Definitions** and search for the built-in policy:

   - **Require a tag and its value on resources**

3. Select the policy definition and choose **Assign**.

4. Set the **Scope** to the resource group:

   - Resource group: `az104-rg2`

5. In **Basics**, configure:

   - Assignment name: `Require Cost Center tag and its value on resources`
   - Description: `Require Cost Center tag and its value on all resources in the resource group`
   - Policy enforcement: **Enabled**

6. In **Parameters**, set:

   - Tag name: `Cost Center`
   - Tag value: `000`

7. In **Remediation**, leave the defaults and **do not** select **Create a Managed Identity** for this task.

8. Select **Review + create**, then select **Create** to assign the policy.

9. Wait a few minutes for the assignment to take effect (it can take **5–10 minutes**).

10. To test enforcement, attempt to create a **Storage Account** in `az104-rg2` **without** adding the required tag.

11. Confirm the deployment fails with **RequestDisallowedByPolicy**.

---

### Testing Policy Enforcement

To validate the policy, a **Storage Account** deployment was attempted
**without specifying the required tag**.

- Resource type: Storage Account
- Resource group: `az104-rg2`
- Tag provided: **None**

---

### Evidence — Policy Assignment

![Policy assignment scoped to az104-rg2](screenshots/task2-policy-assignment.png)

### Evidence — Policy Parameters

![Policy parameters – Cost Center tag](screenshots/task2-policy-parameters.png)

### Policy Definition (Built-in)

![Built-in policy definition – Require a tag and its value on resources](screenshots/task2-policy-definition.png)

The built-in Azure Policy **Require a tag and its value on resources** is implemented
as a parameterized **Deny** policy.

Key characteristics of this policy:

- **Policy type:** Built-in
- **Mode:** Indexed
- **Effect:** Deny
- **Category:** Tags
- **Applies to:** Resources (does not apply to resource groups)

The policy uses parameters to dynamically validate resource tags:

- **tagName** — the required tag name (e.g., `Cost Center`)
- **tagValue** — the required tag value (e.g., `000`)

At deployment time, Azure Policy evaluates the request using the following logic:

- If the resource **does not contain the specified tag**
- Or if the tag value **does not match the required value**
- The deployment is **denied**

This behavior ensures that governance rules are enforced consistently
and prevents non-compliant resources from being created.

### Evidence — Deployment Blocked by Policy

The deployment failed with the following error:

```text
Validation failed.
Resource was disallowed by policy.
(Code: RequestDisallowedByPolicy)
```

## Task 3️⃣ — Apply Tagging via an Azure Policy (Remediation)

### Objective

Apply tagging automatically by using an Azure Policy with a **Modify** effect.
Resources that are missing the required tag will **inherit the tag from the resource group**.

This approach ensures compliance **without blocking deployments** and enables
automatic remediation of non-compliant resources.

---

### Policy Overview

The built-in Azure Policy **Inherit a tag from the resource group if missing** is used to:

- Detect resources missing a specific tag
- Automatically apply the tag based on the resource group value
- Remediate existing and newly created resources

This policy uses the **Modify** effect and supports remediation.

---

### Policy Behavior

- Resources created **without the required tag** are still deployed
- Azure Policy automatically **adds the missing tag**
- The tag value is inherited from the resource group
- Deployments are **not blocked**

---

### Pre-requisite — Remove Previous Deny Policy (Task 2)

Before applying the remediation policy, the previous **Deny** policy assignment
must be removed.

The assignment to delete is:

- **Name:** `Require Cost Center tag and its value on resources`
- **Type:** Policy
- **Scope:** `Azure-Labs-PayG/az104-rg2`
- **Effect:** Deny

> ℹ️ **Why this is mandatory**  
> **Deny** policies take precedence over **Modify** policies.  
> If the Deny assignment is not removed, remediation will never execute
> and deployments will continue to fail.

#### Evidence — Removing Deny Policy Assignment

![Removing previous Deny policy assignment](screenshots/task3-remove-deny-policy.png)

---

### Implementation Steps

1. In the **Azure portal**, navigate to **Policy**.

2. Under **Authoring**, select **Assignments** and delete the previous assignment:

   - **Require a tag and its value on resources**

3. Select **Assign policy** and set the **Scope** to:

   - Resource group: `az104-rg2`

4. Select the built-in policy:

   - **Inherit a tag from the resource group if missing**

5. In **Basics**, configure:
   - Assignment name: `Inherit Cost Center tag from resource group if missing`
   - Description: `Automatically inherit the Cost Center tag from the resource group`
   - Policy enforcement: **Enabled**

#### Evidence — Policy Assignment (Modify)

![Policy assignment with Modify effect](screenshots/task3-policy-assignment-modify.png)

6. In **Parameters**, set:
   - Tag name: `Cost Center`

#### Evidence — Policy Parameters

![Policy parameters – Tag Name](screenshots/task3-policy-parameters-tag-name.png)

7. In **Remediation**, enable:

   - **Create a remediation task**

   > ℹ️ This policy uses the **Modify** effect, therefore a **managed identity is required**.

#### Evidence — Policy Remediation Enabled

![Policy remediation task enabled](screenshots/task3-policy-remediation.png)

8. Select **Review + create**, then **Create**.

---

### Evidence — Final Policy Assignment State

This confirms that only the **Modify** policy is active at the resource group scope.

![Final policy assignment state](screenshots/task3-policy-assignment-final.png)

---

### Testing Policy Remediation

1. Create a new **Storage Account** in the resource group `az104-rg2`
   **without specifying the Cost Center tag**.

2. Verify that the deployment **succeeds**.

3. Open the resource and navigate to **Tags**.

---

### Validation

- The resource was created successfully **without manual tagging**
- The **Cost Center: 000** tag was automatically added
- The tag value was inherited from the resource group

This confirms that Azure Policy remediation is working as expected.

---

### AZ-104 Relevance

- **Modify** policies can change resource configuration
- Remediation requires a **managed identity**
- Azure Policy can enforce compliance **without denying deployments**
- Tag inheritance is a core **governance pattern** tested in AZ-104

## Task 4️⃣ — Configure and Test Resource Locks

### Objective

Protect lab resources from accidental deletion by configuring **Azure Resource Locks**
at the **resource group** level.

Resource Locks are a **post-deployment governance control** that prevent destructive
operations, even when performed by users with elevated permissions.

In this task, a **Delete lock** is applied to a resource group and validated by
attempting a destructive operation.

---

### Resource Lock Overview

**Azure Resource Locks** protect critical resources against:

- Accidental deletions
- Unintentional changes

There are two lock types:

| Lock type    | Description                        |
| ------------ | ---------------------------------- |
| **Delete**   | Prevents deletion of the resource  |
| **ReadOnly** | Prevents modification and deletion |

This task uses a **Delete** lock.

Locks can be applied at the following scopes:

- Subscription
- Resource Group
- Individual Resource

> ℹ️ Resource Locks **do not replace RBAC** — they **override user permissions**.

---

### Governance Context

Azure governance controls operate at different stages:

- **Azure Policy** → governs configuration and compliance (pre-deployment)
- **RBAC** → controls who can perform actions
- **Resource Locks** → protect critical operations (post-deployment)

👉 Resource Locks are evaluated **after RBAC**, acting as a final safety barrier.

---

### Implementation Steps — Create a Delete Lock

1. In the **Azure portal**, navigate to **Resource groups**.
2. Select the resource group:
   - `az104-rg2`
3. From the left-hand menu, select **Locks**.
4. Select **+ Add** to create a new lock.
5. Configure the lock with the following values:

| Setting   | Value                                        |
| --------- | -------------------------------------------- |
| Lock name | `rg-delete-lock`                             |
| Lock type | **Delete**                                   |
| Notes     | Prevent accidental deletion of lab resources |

6. Select **OK** to apply the lock.

---

### Evidence — Resource Lock Creation

This screenshot shows the **creation of a Delete lock** at the resource group level,
including the lock name, type, and description.

![Create resource group delete lock](screenshots/task4-resource-group-delete-lock.png)

---

### Evidence — Resource Lock Active (Post-Creation)

The following screenshot confirms that the **Delete lock is active and enforced**
on the resource group.

- **Lock name:** `rg-delete-lock`
- **Lock type:** Delete
- **Scope:** `az104-rg2`

![Resource group delete lock active](screenshots/task4-resource-group-delete-lock-list.png)

---

### Testing the Delete Lock — Attempt Resource Group Deletion

To validate the lock behavior, a destructive operation was intentionally attempted.

Steps performed:

1. Navigate to the resource group `az104-rg2`.
2. Select **Delete resource group**.
3. Enter the resource group name to confirm deletion.
4. Select **Delete**.

---

### Evidence — Deletion Attempt (Confirmation Screen)

This screenshot shows the deletion confirmation screen where the user attempts
to delete the resource group.

![Delete resource group confirmation](screenshots/task4-delete-resource-group.png)

---

### Evidence — Deletion Blocked by Resource Lock

The deletion attempt failed as expected.

Azure returned an error indicating that the resource group is **locked** and
cannot be deleted.

![Deletion blocked by resource lock](screenshots/task4-delete-resource-group-blocked.png)

---

### Expected Result

- ❌ The resource group **cannot be deleted**
- ❌ The operation is blocked even with elevated permissions
- ✅ The **Delete lock** is active and enforced

---

### Validation

- The resource group `az104-rg2` is protected by a **Delete lock**
- Destructive operations are blocked
- Governance protection is successfully enforced

This confirms that the **Azure Resource Lock** is working as expected.

---

### AZ-104 Relevance

- Resource Locks protect critical resources from accidental deletion
- Locks override RBAC permissions
- Locks can be applied at **subscription**, **resource group**, or **resource** scope
- Locks are evaluated **after RBAC**
- Locks are a **post-deployment governance control**

---

### 🧠 Key Exam Insight

> **Azure Policy** defines _what can be deployed_  
> **RBAC** defines _who can perform actions_  
> **Resource Locks** define _what cannot be changed or deleted_

> **Important:**  
> Even users with **Owner** permissions **cannot delete** a locked resource.

This governance model is **frequently tested in the AZ-104 exam**.
