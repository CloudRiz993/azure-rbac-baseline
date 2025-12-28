# Troubleshooting Runbook — Azure RBAC Baseline (Portal Only)

## Issue 1: User cannot see `rg-rbac-baseline`
**Likely causes**
- User not added to the correct Entra group
- Role assignment created at the wrong scope (subscription instead of RG)
- Token/RBAC propagation delay

**Checks**
1. Microsoft Entra ID → Groups → open the group → confirm the user is a member
2. Azure Portal → Resource groups → `rg-rbac-baseline` → Access control (IAM) → Role assignments → confirm the group + role exist
3. Have the user sign out/in (or use Incognito/InPrivate) and wait 5–10 minutes

**Fix**
- If user is not in the group: add user to the group → sign out/in → retest
- If assignment exists but scope is wrong: remove it and recreate at **Resource Group: `rg-rbac-baseline`**
- Use `rg-rbac-baseline` → IAM → **Check access** to confirm effective permissions for the user

---

## Issue 2: Cannot add role assignment / role assignment fails
**Likely cause**
- Your account lacks permissions (needs Owner or User Access Administrator at scope)

**Fix**
- Confirm your role (at the intended scope):
  - Resource groups → `rg-rbac-baseline` → IAM → **View my access**
- If checking subscription level:
  - Subscriptions → <your subscription> → IAM → Role assignments (should show Owner/UAA)

**Notes**
- Contributor cannot assign roles by default
- Common error indicator:
  - `Microsoft.Authorization/roleAssignments/write` denied

---

## Issue 3: Reader can create resources (unexpected)
**Likely causes**
- Reader user also has broader permissions at subscription scope (directly or via another group)
- Incorrect group used in role assignment
- Role assigned at subscription scope accidentally
- Multiple assignments combine permissions (direct + group)

**Checks**
1. Check the user’s effective access at the RG:
   - Resource groups → `rg-rbac-baseline` → IAM → **Check access**
   - Search the user and review:
     - Direct assignments
     - Group assignments
     - Inherited assignments

2. Check the user’s effective access at the subscription:
   - Subscriptions → <your subscription> → IAM → **Check access**
   - Search the same user and verify whether they have Contributor/Owner/UAA (directly or through a group)

3. Confirm the intended Reader assignment was created at the correct scope:
   - Resource groups → `rg-rbac-baseline` → IAM → Role assignments
   - Verify:
     - Role = Reader
     - Member = correct user/group
     - Scope = Resource group (`rg-rbac-baseline`)

**Fix**
- If the user has broader permissions at subscription scope:
  - Remove the broader assignment OR remove the user from the higher-privilege group (whichever is appropriate for the lab)
- If the wrong group/user was assigned at RG:
  - Remove the incorrect assignment and recreate with the correct identity
- If Reader (or any role) was assigned at subscription scope accidentally:
  - Remove the subscription-scope assignment and recreate at **Resource Group: `rg-rbac-baseline`**
- Retest after:
  - User sign out/in (or Incognito)
  - 5–10 minutes propagation

**Expected outcome**
- Reader can **view** resources in `rg-rbac-baseline`
- Reader cannot **create/update/delete** resources
- Reader cannot **change IAM** (no role assignment changes)

---

## Issue 4: User still has access after role removal
**Likely causes**
- RBAC propagation delay
- Cached sign-in token

**Checks**
1. `rg-rbac-baseline` → IAM → Role assignments → confirm the assignment is actually removed
2. Ask user to sign out/in and close/reopen browser (or use Incognito)

**Fix**
- Wait a few minutes and retry
- If access persists, re-check for another assignment (group or subscription-level) using **Check access**

---

## Issue 5: Assignment exists but user still gets “Access denied”
**Likely causes**
- Scope mismatch (assigned on different scope than where action is performed)
- User is using a different account/tenant than expected
- The role does not include the required action

**Checks**
1. Verify scope shows **Resource group (rg-rbac-baseline)**
2. Verify identity (correct UPN, correct tenant)
3. Copy the exact error text and identify the provider/action if shown

**Fix**
- Recreate assignment at the correct scope (RG)
- If the action requires a different built-in role, assign the **narrowest** role that grants it (still at RG scope)

---

## Evidence to capture (for GitHub)
Store under `assets/screenshots/`:
- `rg-rbac-baseline` → IAM → Role assignments (show scope + role + member)
- IAM → Check access for User1/User2
- “Access denied” screenshot when validating least-privilege (optional but useful)

---

## Escalation checklist (if you need to involve another team)
Provide:
- Subscription name/ID (masked if needed)
- Resource Group: `rg-rbac-baseline`
- User/Group name (UPN / display name)
- Role(s) and scope(s)
- Exact error text + timestamp
- Screenshots: Role assignments + Check access
