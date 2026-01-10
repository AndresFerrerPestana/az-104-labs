# Governance & Costs — Lab 03

## Governance approach

- Use a dedicated Resource Group (`az104-rg3`) for isolation and cleanup.
- Prefer consistent naming (az104-\*).
- Track deployments via Resource Group > Deployments.

## Resource Group creation (lab setup)

The Resource Group `az104-rg3` was created specifically for this lab.

- Subscription: Azure-Labs-PayG
- Region: East US
- Purpose: isolate lab resources and enable full cleanup

Using a dedicated Resource Group aligns with governance best practices by:

- reducing blast radius
- improving cost visibility
- simplifying lifecycle management

## Cost awareness

Managed disks have costs depending on SKU and size.
This lab uses small disks for learning purposes.

## Cleanup plan

At the end of the lab, delete the Resource Group (`az104-rg3`) to avoid ongoing charges.
