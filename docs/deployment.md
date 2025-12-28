# Deployment (Azure Portal Only) — Azure RBAC Baseline

## Objective
Deploy a group-based RBAC baseline at **Resource Group scope** (`rg-rbac-baseline`) and prepare identities/resources for validation.

## Prerequisites
- Azure subscription (trial) with **Owner** access (confirmed)
- Access to Microsoft Entra ID to create groups and users (or appropriate admin rights)

## Resources Created
- Resource Group: `rg-rbac-baseline`
- Entra ID Security Groups:
  - `RBAC-Readers`
  - `RBAC-VMOperators`
  - `RBAC-RG-Owners`
- Role assignments at **Resource Group** scope:
  - Reader → `RBAC-Readers`
  - Virtual Machine Contributor → `RBAC-VMOperators`
  - Owner → `RBAC-RG-Owners`
- (Optional) VM for validation:
  - `vm-rbac-lab` (used only to validate VM operator start/stop actions)

## Evidence (Screenshots)
- Role assignments at RG scope:
  - `/screenshots/01-iam-role-assignments.png`

---

## Step 1 — Create Resource Group
1. Azure Portal → **Resource groups** → **Create**
2. Subscription: select trial subscription
3. Resource group name: `rg-rbac-baseline`
4. Region: select a region close to you
5. Click **Review + create** → **Create**

---

## Step 2 — Create Entra ID Security Groups
1. Azure Portal → search **Microsoft Entra ID** → open
2. Left menu → **Groups** → **New group**
3. Group type: **Security**
4. Membership type: **Assigned**
5. Create these groups:
   - `RBAC-Readers`
   - `RBAC-VMOperators`
   - `RBAC-RG-Owners`

---

## Step 3 — Create Test Users for Validation (Recommended)
1. Entra ID → **Users** → **New user** → **Create new user**
2. Create:
   - `lab-reader`
   - `lab-vmops`
3. Add members to groups:
   - Add `lab-reader` → `RBAC-Readers`
   - Add `lab-vmops` → `RBAC-VMOperators`
   - Add your main account → `RBAC-RG-Owners` (optional if you are already Owner)

> Note: RBAC and group membership can take a few minutes to propagate. Use an Incognito/InPrivate window for sign-in tests.

---

## Step 4 — Assign Roles at Resource Group Scope (Critical)
1. Azure Portal → **Resource groups** → open `rg-rbac-baseline`
2. Left menu → **Access control (IAM)**
3. Click **Add** → **Add role assignment**
4. Add these assignments:
   - Role: **Reader** → Member: `RBAC-Readers`
   - Role: **Virtual Machine Contributor** → Member: `RBAC-VMOperators`
   - Role: **Owner** → Member: `RBAC-RG-Owners`
5. Confirm scope is correct:
   - `rg-rbac-baseline` → IAM → **Role assignments**
   - Scope should show **This resource** / Resource group (`rg-rbac-baseline`)

---

## Step 5 — Provider Registration Note (Why Reader “Create Storage” Fails)
During validation, `lab-reader` may see a message indicating the `Microsoft.Storage` resource provider is not registered and they do not have permissions to register it.
- This is expected because provider registration is a **subscription-level** action.
- Reader at RG scope should not be able to register providers or create resources.

(See validation evidence: `/screenshots/02-reader-denied-create.PNG`)

---

## Optional — Create a Small VM (Used for VM Operator Proof)
Only do this if you want to validate `RBAC-VMOperators` can manage VM lifecycle actions.

1. Azure Portal → **Virtual machines** → **Create**
2. Resource group: `rg-rbac-baseline`
3. VM name: `vm-rbac-lab`
4. Keep defaults minimal (smallest practical size).
5. Security posture for this lab:
   - Do **not** open inbound management ports (no RDP/SSH rules needed for start/stop proof).
6. Create the VM.
7. Validate start/stop as `lab-vmops` (see `docs/validation.md`).
8. Delete the VM during teardown (see `teardown/teardown.md`).

Evidence reference:
- `/screenshots/03-vmops-start-stop.PNG`
