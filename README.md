# Azure RBAC Baseline (Least Privilege) — Azure Portal Only (AZ-104)

## Goal
Implement a practical, group-based RBAC model using Microsoft Entra ID (Azure AD) and scoped role assignments to enforce least privilege.

## Scenario
A project team needs controlled access to resources in one Resource Group:
- Readers: view resources only
- VM Operators: manage VMs (start/stop/restart) but cannot change access control
- RG Owners: full control within the Resource Group

## Architecture
- Entra ID security groups: RBAC-Readers, RBAC-VMOperators, RBAC-RG-Owners
- Role assignments at **Resource Group** scope: `rg-rbac-baseline`
- Built-in roles: Reader, Virtual Machine Contributor, Owner

## What I implemented (Portal)
- Created `rg-rbac-baseline` as the RBAC scope boundary
- Created Entra security groups and added test members
- Assigned roles at Resource Group scope (no subscription-wide permissions)
- Validated access using real sign-in tests and portal permission outcomes

## Evidence
Screenshots in `/screenshots` show:
- IAM role assignments at RG scope
- Reader denied resource creation
- VM Operator permissions validation
- Owner IAM access

## Documentation
- Deployment: `docs/deployment.md`
- Validation: `docs/validation.md`
- Troubleshooting: `runbooks/troubleshooting.md`
- Teardown: `teardown/teardown.md`

## Outcome
A repeatable RBAC baseline demonstrating AZ-104 skills: identity, RBAC scope, least privilege, and access validation.
