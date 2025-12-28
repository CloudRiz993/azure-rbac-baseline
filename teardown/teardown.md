# Teardown (Azure Portal Only) — Azure RBAC Baseline

## Goal
Remove all lab access and resources to prevent lingering permissions and avoid unexpected cost.

## What this teardown covers
- Remove RBAC role assignments at **Resource Group** scope (`rg-rbac-baseline`)
- Delete lab resources (VM, VNet/NIC/Public IP, disks, etc.)
- Delete the Resource Group (recommended)
- Optional cleanup in Microsoft Entra ID (test users + security groups)

> Tip: If you delete the Resource Group, most Azure resources inside it will be removed automatically.  
> You should still remove RBAC assignments (or delete the RG) to ensure access is not retained.

---

## Step 1 — Remove RBAC role assignments (RG scope)

1. Azure Portal → **Resource groups** → open `rg-rbac-baseline`
2. Select **Access control (IAM)**
3. Go to **Role assignments**
4. Remove the lab assignments (as applicable):
   - **Reader** → `RBAC-Readers`
   - **Virtual Machine Contributor** → `RBAC-VMOperators`
   - **Owner** → `RBAC-RG-Owners`

**How to remove**
- Select the assignment → click **Remove** (or **Delete**) → confirm.

**Validation**
- Refresh the Role assignments tab and confirm the groups are no longer listed.

---

## Step 2 — Delete lab resources inside the RG (if any)

If you created a VM for validation (`vm-rbac-lab`), it likely created related resources:
- NIC
- Public IP (if used)
- OS disk (and possibly data disk)
- VNet/subnet
- NSG (if created)
- Diagnostics settings (optional)

### Option A (Recommended): Delete the Resource Group (fastest)
Skip to **Step 3** — deleting the RG will remove everything in one go.

### Option B: Delete resources individually (only if you want to keep the RG)
1. Azure Portal → **Resource groups** → `rg-rbac-baseline` → **Resources**
2. Delete in this practical order:
   1) Virtual machine: `vm-rbac-lab`
   2) Disks (OS/data) if not removed automatically
   3) Public IP address (if present)
   4) Network interface (NIC)
   5) Network security group (if present)
   6) Virtual network (if present)

**Validation**
- Confirm the Resources list is empty (or contains only what you intended to keep).

---

## Step 3 — Delete the Resource Group (recommended)

1. Azure Portal → **Resource groups**
2. Select `rg-rbac-baseline`
3. Click **Delete resource group**
4. Type the RG name to confirm: `rg-rbac-baseline`
5. Click **Delete**

**Validation**
- Wait for completion, then verify `rg-rbac-baseline` no longer appears in Resource groups.

---

## Step 4 — Optional: Remove test users (if created only for this lab)

If you created these users only for validation:
- `lab-reader`
- `lab-vmops`

Steps:
1. Azure Portal → **Microsoft Entra ID** → **Users**
2. Search each user (`lab-reader`, `lab-vmops`)
3. Delete the user(s)

---

## Step 5 — Optional: Remove Entra security groups (if created only for this lab)

If these groups were created only for this lab:
- `RBAC-Readers`
- `RBAC-VMOperators`
- `RBAC-RG-Owners`

Steps:
1. Azure Portal → **Microsoft Entra ID** → **Groups**
2. Search each group
3. Delete the group(s)

---

## Post-teardown checks (quick)

- `rg-rbac-baseline` is deleted (or empty, if retained)
- IAM role assignments for the lab groups are removed (if RG retained)
- No VM-related resources remain (VM/NIC/IP/Disks/VNet/NSG)
- Optional: lab users/groups removed from Entra ID

---

## Notes
- RBAC removals and deletions can take a few minutes to reflect.
- If you see unexpected residual resources, check:
  - Resource group → **Resources**
  - Subscriptions → **Cost Management** (to confirm no running resources)
