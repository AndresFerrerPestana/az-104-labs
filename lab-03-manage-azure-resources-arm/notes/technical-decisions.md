# Technical Decisions — Lab 03

## Tooling choices

- **Portal (Task 1–2):** good for learning and visual validation; generates and exports ARM templates.
- **PowerShell (Task 3):** common in admin environments and automation with Az modules.
- **Azure CLI (Task 4):** cross-platform and frequently used in pipelines.
- **Bicep (Task 5):** improves readability and maintainability over raw ARM JSON.

## Scope choices

- Resource scope: **Resource Group** (`az104-rg3`)
- Reason: isolate resources for easy cleanup and cost tracking.

## Region choice

- Default: **East US** (align with the official lab steps)
- Can be changed if required by subscription limitations.

## Disk SKU selection

For Task 1, **Standard HDD (S4, 32 GiB)** was selected instead of Premium SSD.

Reasoning:

- aligns with the official lab guidance
- minimizes cost for a short-lived lab environment
- disk performance is not relevant for ARM template deployment learning

## Disk retirement notice (future consideration)

During disk creation, the Azure Portal displays the following notice:

> **“Standard HDD OS disks will be retired on September 8, 2028.”**

Although this lab uses a **data disk** and not an OS disk, this notice is still relevant from a governance and lifecycle perspective.

Key takeaways:

- Azure services and SKUs evolve over time and may be deprecated.
- Administrators must consider **service lifecycle** when designing long-term solutions.
- For production workloads, disk SKU selection should account for:
  - support timelines
  - migration paths
  - future compatibility

For this lab environment, **Standard HDD** remains an appropriate choice because:

- the environment is short-lived
- performance is not a requirement
- cost optimization is a priority

## AZ-104 relevance

This task reinforces:

- understanding of ARM as the Azure control plane
- resource lifecycle and SKU considerations
- cost-aware design decisions
- separation between lab and production environments

## Resource Group creation documentation scope

The creation of the Resource Group `az104-rg3` was intentionally not documented as a full step-by-step tutorial.

Rationale:

- Resource Group creation is a foundational operation, not the focus of this lab
- The primary learning objective is ARM-based deployment methods
- Recreating the Resource Group solely for documentation would introduce unnecessary churn

Instead, the Resource Group creation is documented through:

- architectural context
- governance rationale
- visual confirmation of the final state

This approach also aligns with the cleanup strategy defined for the lab.
