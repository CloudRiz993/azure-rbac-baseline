# Validation (Proof of Access) — Azure RBAC Baseline (Portal Only)

## Goal
Prove least-privilege access is enforced at **Resource Group** scope (`rg-rbac-baseline`).

## Evidence Files (Screenshots)
Place screenshots in `/screenshots` using these names:
- `01-iam-role-assignments.png`
- `02-reader-denied-create.png`
- `03-vmops-validation.png`
- `04-owner-can-manage-iam.png`

## A) Validate Role Assignments Exist (Admin View)
1. Azure Portal → Resource groups → `rg-rbac-baseline`
2. Access control (IAM) → **Role assignments**
3. Confirm:
   - `RBAC-Readers` has **Reader**
   - `RBAC-VMOperators` has **Virtual Machine Contributor**
   - `RBAC-RG-Owners` has **Owner**
4. Capture screenshot:
   - `/screenshots/01-iam-role-assignments.png`

## B) Validate Reader Permissions (lab-reader)
1. Open an Incognito/Private window → sign in as `lab-reader`
2. Azure Portal → Resource groups → `rg-rbac-baseline`
3. Attempt to create a resource (e.g., Storage account) inside the RG
4. Expected result: **Access denied / insufficient privileges**
5. Capture screenshot:
   - `/screenshots/02-reader-denied-create.png`

## C) Validate VM Operator Permissions (lab-vmops)
Two acceptable validation approaches:

### Option 1 — Validate IAM Restrictions (No VM Required)
1. Incognito → sign in as `lab-vmops`
2. Open `rg-rbac-baseline` → Access control (IAM)
3. Expected: cannot add role assignments / cannot manage access
4. Capture screenshot (use as proof):
   - `/screenshots/03-vmops-validation.png`

### Option 2 — Validate VM Start/Stop (Best Proof)
(Only if a VM exists in `rg-rbac-baseline`)
1. Incognito → sign in as `lab-vmops`
2. Open VM `vm-rbac-lab`
3. Attempt **Stop** then **Start**
4. Expected: actions allowed
5. Capture screenshot:
   - `/screenshots/03-vmops-validation.png`

## D) Validate Owner Permissions
1. Sign in as a member of `RBAC-RG-Owners` (or main Owner account)
2. Open `rg-rbac-baseline` → Access control (IAM)
3. Confirm ability to open **Add role assignment**
4. Capture screenshot:
   - `/screenshots/04-owner-can-manage-iam.png`

## Notes
- RBAC propagation can take several minutes. If permissions don’t reflect immediately:
  - sign out/in, or use Incognito
  - wait 5–10 minutes and retry

