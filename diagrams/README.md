# Diagrams — Azure RBAC Baseline

This folder contains architecture diagrams that explain the RBAC design used in this lab.

## Diagram 1 — RBAC Model (Entra Groups → RG Scope)

**Purpose**
Show the relationship between:
- Microsoft Entra ID security groups
- Azure RBAC built-in roles
- The scope boundary: Resource Group `rg-rbac-baseline`

### Mermaid (rendered by GitHub)
You can keep the diagram as Mermaid so it stays editable and version-controlled.

```mermaid
flowchart LR
  subgraph EntraID[Microsoft Entra ID]
    R[RBAC-Readers]
    V[RBAC-VMOperators]
    O[RBAC-RG-Owners]
  end

  subgraph Azure[Azure Subscription]
    RG[Resource Group: rg-rbac-baseline]
  end

  R -->|Reader| RG
  V -->|Virtual Machine Contributor| RG
  O -->|Owner| RG

