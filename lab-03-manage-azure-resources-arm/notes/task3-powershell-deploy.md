# Task 3 — Deploy ARM Template using Azure PowerShell (Cloud Shell)

This task documents how to deploy an ARM template using **Azure PowerShell** in **Azure Cloud Shell**.  
The goal is to reuse the exported ARM template from Task 1 and deploy a third managed disk (`az104-disk3`).

---

## Prerequisites

- Resource Group: **`az104-rg3`**
- ARM template files (from Task 1):
  - `templates/task1-exported/template.json`
  - `templates/task1-exported/parameters.json`
- Azure Portal access and permission to deploy resources
- Cloud Shell available in the Azure Portal

---

## Step 1 — Open Azure Cloud Shell (PowerShell)

1. In the Azure Portal, open **Cloud Shell**
2. Select **PowerShell** (not Bash)
3. When prompted, select **No storage account required**

![Cloud Shell PowerShell](../screenshots/21-task3-cloudshell-powershell.png)

> Note: Azure Cloud Shell can optionally use a storage account and file share to persist files between sessions.
>
> For this lab, **No storage account required** was selected because:
>
> - the environment is short-lived
> - templates are versioned in GitHub
> - persistence between sessions is not required

After selecting PowerShell and applying the configuration, Azure Cloud Shell initializes
a PowerShell session authenticated against the selected subscription.

This environment is now ready to execute Azure PowerShell cmdlets for ARM deployments.

---

## Step 2 — Upload the ARM template files

To reuse the ARM template exported in **Task 1**, the template files must be made
available inside **Azure Cloud Shell**.

This step ensures that the same template can be deployed using Azure PowerShell,
without copying or modifying its contents.

![Cloud Shell session ready](../screenshots/21-task3-cloudshell-powershell-ready.png)

### Upload the template files

1. In Cloud Shell, click **Upload/Download files** → **Upload**
2. Upload the following files from your repository:

   - `template.json`
   - `parameters.json`

   Recommended source folder:

   - `templates/task1-exported/`

![Upload ARM templates to Cloud Shell](../screenshots/22-task3-cloudshell-upload-templates.png)

### Verify file presence in Cloud Shell

After the upload completes, confirm that the files are available in the current
Cloud Shell session:

```powershell
ls
```

![ls Cloud Shell](../screenshots/23-task3-cloudshell-ls-templates.png)

This validation step ensures that the deployment will reuse the original ARM
template without modification, aligning with idempotent deployment principles.

## Step 3 — Review and adjust parameters (Cloud Shell / PowerShell)

Before deploying the ARM template using Azure PowerShell, review and adjust the **parameters** that control the deployment.

The ARM template used in this task is **the same template exported in Task 1**.

- ✅ The `template.json` file is **not modified**
- ✅ Only a **parameter value** is updated in `parameters.json`

---

### Goal of this step

Deploy a **new managed disk** using the same ARM template by changing only the disk name:

- **Previous deployment (Task 1):** `az104-disk1`
- **This deployment (Task 3):** `az104-disk3`

> ⚠️ The parameter name (for example, `disks_az104_disk1_name`) is generated automatically by the Azure Portal export.  
> What matters is the **parameter value**, which defines the resource name.

---

## Step-by-step procedure

### Step 3.1 — Open Azure Cloud Shell (PowerShell)

1. In the Azure Portal, open **Cloud Shell**
2. Select **PowerShell**

![Cloud Shell PowerShell session ready](../screenshots/21-task3-step3-cloudshell-powershell-ready.png)

_Azure Cloud Shell initialized in PowerShell and ready to manage ARM templates._

---

### Step 3.2 — Create the folder structure in Cloud Shell

Cloud Shell does not automatically contain local project files.  
Create the same folder structure used in the repository:

```powershell
mkdir templates
mkdir templates/task1-exported
```

Confirm the folder was created:

```powershell
ls templates
```

### Step 3.3 — Upload the exported ARM files to Cloud Shell

Now upload the exported files from your local machine into Cloud Shell.

1. In Cloud Shell, click **Manage files**
2. Select **Upload**
3. Upload the following files:
   - `template.json`
   - `parameters.json`
4. Ensure they are uploaded into:
   - `/home/azureuser/templates/task1-exported`

### Step 3.4 — Organize the uploaded files

After the upload, confirm the files are in the home directory:

```powershell
ls
```

Move the files to the correct project folder:

```powershell
mv template.json templates/task1-exported/
mv parameters.json templates/task1-exported/
```

Verify that the files are now correctly placed:

```powershell
ls templates/task1-exported
```

Expected output:

```powershell
template.json
parameters.json
```

_ARM template and parameters files organized in the correct Cloud Shell directory._

### Step 3.5 — Review and update the parameters file

Review the current contents of the parameters file:

```powershell
Get-Content templates/task1-exported/parameters.json
```

![Parameters file before updating the disk name](../screenshots/21-task3-step3-parameters-before-edit.png)

_Viewing the contents of `parameters.json` before updating the disk name parameter (value is `null`)._

At this stage, the disk name parameter has no value assigned (`null`), so it must be updated before deployment.

Locate the disk name parameter value and confirm it is set to:

```powershell
"value": "az104-disk3"
```

If necessary, edit the file using:

```powershell
code templates/task1-exported/parameters.json
```

```powershell
nano templates/task1-exported/parameters.json
```

![Editing parameters.json using nano in Azure Cloud Shell](../screenshots/21-task3-step3-edit-parameters-nano.png)

_Editing the `parameters.json` file in Azure Cloud Shell using `nano` to update the disk name parameter._

**Save and exit the editor**

After updating the parameter value, save the file and exit the `nano` editor:

1. Press **Ctrl + O** to write the changes to the file
2. Press **Enter** to confirm the file name
3. Press **Ctrl + X** to exit the editor

You are then returned to the Cloud Shell prompt.

### Step 3.6 — Confirm updated parameter value

![Parameters file after updating the disk name](../screenshots/21-task3-step3-parameters-after-edit.png)

_Confirming that the disk name parameter was successfully updated to `az104-disk3` in `parameters.json`._

## Step 4 — Deploy ARM template using Azure PowerShell

In this step, the ARM template is deployed using **Azure PowerShell** in **Azure Cloud Shell**.

The deployment uses:

- The **same ARM template** exported in Task 1
- The **updated parameters file** from Step 3
- A **new managed disk** (`az104-disk3`) is created

---

### Prerequisites

- Resource Group: `az104-rg3`
- ARM template files available in Cloud Shell:
  - `templates/task1-exported/template.json`
  - `templates/task1-exported/parameters.json`
- Azure Cloud Shell opened in **PowerShell**

---

### Step 4.1 — Verify the template and parameters files

Before deploying, confirm that the files exist in the expected location:

```powershell
ls templates/task1-exported
```

![ARM template files ready for deployment](../screenshots/21-task3-step4-verify-template-files.png)

_Verifying that `template.json` and `parameters.json` are present in the expected directory before deployment._

### Step 4.2 — Deploy the ARM template

Run the following command to deploy the ARM template to the resource group:

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName az104-rg3 `
  -TemplateFile templates/task1-exported/template.json `
  -TemplateParameterFile templates/task1-exported/parameters.json

```

This command:

- Uses **Azure PowerShell**

- Deploys resources to the specified **Resource Group**

- Applies the updated parameter values

- Creates a new managed disk `(az104-disk3)`

![ARM template deployment succeeded](../screenshots/21-task3-step4-deployment-succeeded.png)

_Successful deployment of the ARM template using Azure PowerShell. The provisioning state is `Succeeded` and the managed disk `az104-disk3` was created._

The ARM template was deployed using Azure PowerShell with an incremental deployment mode.  
The updated parameters file was applied, resulting in the creation of a new managed disk named `az104-disk3`.

The deployment completed successfully with the provisioning state set to `Succeeded`.

### Step 4.3 — Review the deployment result

If the deployment is successful, the output will show:

- `ProvisioningState : Succeeded`

No existing resources are modified.

### Step 4.4 — Verify the resource in Azure

Verify that the new managed disk was created:

- In the Azure Portal, navigate to:

  - Resource Groups → az104-rg3 → Disks

- Confirm that az104-disk3 exists

![Managed disks created in az104-rg3](../screenshots/21-task3-step4-portal-verify-managed-disks.png)

_Verification in the Azure Portal showing that the managed disk `az104-disk3` was successfully created in the resource group `az104-rg3`, alongside existing disks._

The Azure Portal confirms that the deployment was successful.  
The new managed disk `az104-disk3` is visible in the resource group `az104-rg3`, together with previously created disks.

Alternatively, verify using PowerShell:

```powershell
Get-AzDisk -ResourceGroupName az104-rg3
```

![Verifying managed disk using Azure PowerShell](../screenshots/21-task3-step4-verify-disk-powershell.png)

_Verification of the managed disk using Azure PowerShell. The output confirms that the disk exists in resource group `az104-rg3` with provisioning state `Succeeded`._

As an additional verification step, Azure PowerShell was used to list managed disks in the resource group.  
The output confirms that the managed disk exists and was successfully provisioned.

## Step 5 - Validate the deployment
