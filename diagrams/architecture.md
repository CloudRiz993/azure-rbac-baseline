# Architecture Diagram (Mermaid)

```mermaid
flowchart LR
  subgraph EntraID[Microsoft Entra ID]
    G1[RBAC-Readers]
    G2[RBAC-VMOperators]
    G3[RBAC-RG-Owners]
  end

  subgraph Azure[Azure Subscription]
    RG[Resource Group: rg-rbac-baseline]
  end

  G1 -->|Reader| RG
  G2 -->|Virtual Machine Contributor| RG
  G3 -->|Owner| RG

