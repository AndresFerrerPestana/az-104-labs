# Technical Decisions — Lab 03

## Tooling choices

- **Portal (Task 1–2):** good for learning and visual validation; generates/export ARM templates.
- **PowerShell (Task 3):** common in admin environments and automation with Az modules.
- **Azure CLI (Task 4):** cross-platform and frequently used in pipelines.
- **Bicep (Task 5):** improves readability and maintainability over raw ARM JSON.

## Scope choices

- Resource scope: **Resource Group** (`az104-rg3`)
- Reason: isolate resources for easy cleanup and cost tracking.

## Region choice

- Default: **East US** (align with the official lab steps)
- Can be changed if required by subscription limitations.
