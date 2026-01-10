# Architecture Notes — Lab 03

## What is being deployed?

This lab deploys **Azure Managed Disks** (Disk1 → Disk5) inside the same Resource Group (`az104-rg3`).

The focus is not the disk itself, but the **deployment mechanisms** used to create it.

## Deployment methods covered

- Azure Portal (manual deployment)
- ARM template export (JSON)
- Portal custom template deployment
- PowerShell deployment (Cloud Shell)
- Azure CLI deployment (Cloud Shell)
- Bicep deployment

## Deployment flow (based on the lab diagram)

- **Task 1:** Create Disk 1 in the Portal → Export ARM template
- **Task 2:** Edit the template → Deploy Disk 2 via Portal
- **Task 3:** Deploy Disk 3 via PowerShell
- **Task 4:** Deploy Disk 4 via Azure CLI
- **Task 5:** Deploy Disk 5 via Bicep

## Why a single Resource Group?

Using a single Resource Group simplifies:

- governance and auditing (deployments + activity log)
- cost tracking
- cleanup at the end of the lab
