# Teardown — Azure RBAC Baseline

## Goal
Remove lab access and avoid lingering permissions/cost.

## Step 1 — Remove role assignments (RG scope)
1. Azure Portal → Resource groups → `rg-rbac-baseline`
2. Access control (IAM) → Role assignments
3. Remove assignments created for this lab:
   - Reader → `RBAC-Readers`
   - Virtual Machine Contributor → `RBAC-VMOperators`
   - Owner → `RBAC-RG-Owners`

## Step 2 — Delete lab resources (optional)
If you created test resources (VM/storage/etc.), delete them (or delete the RG in Step 3).

## Step 3 — Delete the Resource Group
1. Resource groups → `rg-rbac-baseline` → Delete resource group
2. Type `rg-rbac-baseline` to confirm → Delete

## Step 4 — (Optional) Remove Entra ID groups
If these groups were created only for the lab:
- Entra ID → Groups → delete:
  - RBAC-Readers
  - RBAC-VMOperators
  - RBAC-RG-Owners

## Post-teardown validation
- `rg-rbac-baseline` no longer exists
- IAM role assignments at that scope are gone

