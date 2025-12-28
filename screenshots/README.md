# Evidence (Screenshots) — Azure RBAC Baseline

These screenshots prove RBAC behavior at **Resource Group scope**: `rg-rbac-baseline`.

## Files included

| File | What it shows | Why it matters |
|---|---|---|
| `01-iam-role-assignments.png` | `rg-rbac-baseline` → **Access control (IAM)** → Role assignments grouped by Role (Owner / Reader / Virtual Machine Contributor) with groups: `RBAC-RG-Owners`, `RBAC-Readers`, `RBAC-VMOperators` | Confirms the baseline design: **group-based RBAC** applied at the **resource group** scope |
| `02-reader-denied-create.PNG` | Signed in as **lab-reader** attempting “Create a storage account” and receiving an error indicating `Microsoft.Storage` provider not registered + no permission to register | Strong proof of **least privilege**: reader cannot create resources and also lacks **subscription-level** rights like provider registration |
| `03-vmops-start-stop.PNG` | Signed in as **lab-vmops** on VM `vm-rbac-lab` showing VM management actions (Start/Restart/Stop) | Confirms `RBAC-VMOperators` has the intended **VM operations** capability (via Virtual Machine Contributor) |
| `04-owner-can-manage-iam.PNG` | `rg-rbac-baseline` → **Access control (IAM)** page showing IAM management options (View my access / Check access / Add role assignment) while signed in as Owner context | Confirms the Owner path can access IAM controls for governance at RG scope |

## Notes
- The `02-reader-denied-create.PNG` message includes **resource provider registration**. This is expected in locked-down labs: registering providers is a **subscription-level** action and typically requires elevated permissions.
- For even stronger evidence, optional additions:
  - `05-check-access-reader.png` (IAM → Check access → lab-reader)
  - `06-check-access-vmops.png` (IAM → Check access → lab-vmops)

