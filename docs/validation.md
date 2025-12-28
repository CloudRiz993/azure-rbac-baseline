# Validation (Proof of Access) — Azure RBAC Baseline (Portal Only)

## Goal
Prove least-privilege access is enforced at **Resource Group** scope (`rg-rbac-baseline`).

## Evidence Files (Screenshots)
Stored in `/screenshots` (use these exact filenames):
- `01-iam-role-assignments.png`
- `02-reader-denied-create.PNG`
- `03-vmops-start-stop.PNG`
- `04-owner-can-manage-iam.PNG`

---

## A) Validate Role Assignments Exist (Admin View)
1. Azure Portal → Resource groups → `rg-rbac-baseline`
2. Access control (IAM) → **Role assignments**
3. Confirm group-based RBAC:
   - `RBAC-Readers` has **Reader**
   - `RBAC-VMOperators` has **Virtual Machine Contributor**
   - `RBAC-RG-Owners` has **Owner**
4. Capture screenshot:
   - `/screenshots/01-iam-role-assignments.png`

---

## B) Validate Reader Permissions (lab-reader)
1. Open an Incognito/Private window → sign in as `lab-reader`
2. Azure Portal → Resource groups → `rg-rbac-baseline`
3. Attempt to create a resource (example: Storage account) inside the RG
4. Expected result:
   - Reader cannot create resources, and cannot perform subscription-level actions such as provider registration
   - You may see an error indicating `Microsoft.Storage` provider is not registered and you don’t have permissions to register it
5. Capture screenshot:
   - `/screenshots/02-reader-denied-create.PNG`

---

## C) Validate VM Operator Permissions (lab-vmops) — Best Proof
(Using an existing VM inside `rg-rbac-baseline`)

1. Incognito/Private → sign in as `lab-vmops`
2. Open VM: `vm-rbac-lab`
3. Perform VM operations:
   - **Stop** (or Restart) then **Start**
4. Expected result: actions allowed (VM operator can manage VM lifecycle)
5. Capture screenshot:
   - `/screenshots/03-vmops-start-stop.PNG`

---

## D) Validate Owner Permissions
1. Sign in as a member of `RBAC-RG-Owners` (or your main Owner account)
2. Open `rg-rbac-baseline` → Access control (IAM)
3. Confirm ability to manage access (examples):
   - **View my access**
   - **Check access**
   - **Add role assignment**
4. Capture screenshot:
   - `/screenshots/04-owner-can-manage-iam.PNG`

---

## Notes
- RBAC propagation can take several minutes. If permissions don’t reflect immediately:
  - sign out/in, or use Incognito
  - wait 5–10 minutes and retry
