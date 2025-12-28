# Deployment (Azure Portal Only) — Azure RBAC Baseline

## Prerequisites
- Azure subscription (trial) with **Owner** access (confirmed)
- Access to Microsoft Entra ID to create groups and users (or have admin rights)

## Resources Created
- Resource Group: `rg-rbac-baseline`
- Entra ID Security Groups:
  - `RBAC-Readers`
  - `RBAC-VMOperators`
  - `RBAC-RG-Owners`
- Role assignments at **Resource Group** scope:
  - Reader → RBAC-Readers
  - Virtual Machine Contributor → RBAC-VMOperators
  - Owner → RBAC-RG-Owners

## Step 1 — Create Resource Group
1. Azure Portal → **Resource groups** → **Create**
2. Subscription: select trial subscription
3. Resource group name: `rg-rbac-baseline`
4. Region: select a region close to you
5. Click **Review + create** → **Create**

## Step 2 — Create Entra ID Security Groups
1. Azure Portal → search **Microsoft Entra ID** → open
2. Left menu → **Groups** → **New group**
3. Group type: **Security**
4. Membership type: **Assigned**
5. Create these groups:
   - `RBAC-Readers`
   - `RBAC-VMOperators`
   - `RBAC-RG-Owners`

## Step 3 — (Recommended) Create Test Users for Validation
1. Entra ID → **Users** → **New user** → **Create new user**
2. Create:
   - `lab-reader`
   - `lab-vmops`
3. Add members to groups:
   - Add `lab-reader` → `RBAC-Readers`
   - Add `lab-vmops` → `RBAC-VMOperators`
   - Add your main account → `RBAC-RG-Owners` (optional)

> Note: RBAC and group membership can take a few minutes to propagate. Use an Incognito window for sign-in tests.

## Step 4 — Assign Roles at Resource Group Scope (Critical)
1. Azure Portal → **Resource groups** → open `rg-rbac-baseline`
2. Left menu → **Access control (IAM)**
3. Click **Add** → **Add role assignment**
4. Add these assignments:
   - Role: **Reader** → Member: `RBAC-Readers`
   - Role: **Virtual Machine Contributor** → Member: `RBAC-VMOperators`
   - Role: **Owner** → Member: `RBAC-RG-Owners`
5. Confirm the scope is **Resource group: rg-rbac-baseline** by checking:
   - `rg-rbac-baseline` → IAM → **Role assignments** tab (scope should show RG)

## Optional — Create a Small VM (Only if used for VM Operator Proof)
If you created a VM to validate start/stop actions:
1. Azure Portal → **Virtual machines** → **Create**
2. Resource group: `rg-rbac-baseline`
3. VM name: `vm-rbac-lab`
4. Inbound ports: **None** (no login required for this lab)
5. Create, validate, then delete (see teardown).
