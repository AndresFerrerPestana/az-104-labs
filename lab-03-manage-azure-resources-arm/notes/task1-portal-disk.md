# Task 1 — Create Managed Disk and Export ARM Template (Azure Portal)

This task documents the creation of an Azure Managed Disk using the Azure Portal and the export of the corresponding ARM template.

---

## Step 1 — Open the Resource Group

1. Open the **Azure Portal**
2. Navigate to **Resource groups**
3. Open the Resource Group **`az104-rg3`**

---

## Step 2 — Create a Managed Disk (Disk 1)

1. Inside the Resource Group **`az104-rg3`**, click **Create**
2. Search for **Disk**
3. Select **Disk (Managed disk)** and click **Create**

---

## Step 3 — Configure disk settings

### Basics tab

- **Subscription:** Azure-Labs-PayG
- **Resource group:** az104-rg3
- **Disk name:** az104-disk1
- **Region:** East US
- **Source type:** None (Empty disk)
- **Size:** 32 GiB
- **SKU:** Standard HDD

Leave all other settings as default.

---

## Step 4 — Review and create

1. Click **Review + create**
2. Wait for validation to succeed
3. Click **Create**

---

## Step 5 — Export the ARM template

1. Open the newly created disk **`az104-disk1`**
2. In the left menu, select **Export template**
3. Click **Download**

The downloaded package contains:

- `template.json`
- `parameters.json`

These files are used in subsequent tasks.

---

## Result

- Disk **`az104-disk1`** created successfully
- ARM template exported and saved to the repository
