
**Commit message:** `Add architecture diagram`

---

## Step 4 — Create the two “planned” repos so your profile doesn’t point to nowhere

Your profile README currently references both repos, but they do not exist yet. :contentReference[oaicite:9]{index=9}

### 4.1 Create `azure-vnet-nsg-lab`
1. GitHub → New repository
2. Name: `azure-vnet-nsg-lab`
3. Public → Add README → Create

Then add these folders/files (same pattern you used in RBAC):
- `docs/deployment.md`
- `docs/validation.md`
- `runbooks/troubleshooting.md`
- `teardown/teardown.md`
- `diagrams/architecture.md`
- `screenshots/README.md`

**Commit message (first scaffold):** `Initialize VNet/NSG lab documentation scaffold`

### 4.2 Create `azure-monitoring-alerts-kql`
Repeat the same steps.

**Commit message (first scaffold):** `Initialize Monitoring/KQL lab documentation scaffold`

After both repos exist, go back to your **profile README** table and turn repo names into clickable links (GitHub will auto-link if you type them as full repo links, or you can link in Markdown).

---

## Step 5 — Pin the right repos on your GitHub profile
1. GitHub profile → Customize profile → **Pinned repositories**
2. Pin:
- `azure-rbac-baseline`
- `azure-vnet-nsg-lab`
- `azure-monitoring-alerts-kql`

This aligns your profile top section with your portfolio story.

---

## What is “missing” right now (summary)
- Two repos referenced in your profile README do not exist yet. :contentReference[oaicite:10]{index=10}  
- `azure-rbac-baseline` lacks About metadata (description/topics). :contentReference[oaicite:11]{index=11}  
- The remaining docs should be standardized to match your README architecture (groups + roles at RG scope). :contentReference[oaicite:12]{index=12}  

---

If you want to move fastest: do **Step 3** (finish RBAC docs) first, then **Step 4** (create placeholders) and **Step 1** (fix profile table links). This yields an immediately credible profile with no dead ends.
::contentReference[oaicite:13]{index=13}
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

