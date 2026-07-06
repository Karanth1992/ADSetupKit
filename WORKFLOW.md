# ADSetupKit — Workflow

> ADSetupKit provisions a new Windows Server and prepares it for its Active Directory role.
> Run it **before** [ADOpsKit](https://github.com/Karanth1992/ADOpsKit) — ADSetupKit stands the server up, ADOpsKit then monitors it.

## 1 · End-to-End Overview

```mermaid
flowchart LR
    subgraph INSTALL["📦 Install"]
        PSG["PowerShell Gallery\nInstall-Module ADSetupKit"]
        MOD["ADSetupKit Module\nv1.0.0 · 12 functions"]
        PSG --> MOD
    end

    subgraph PREP["⚙️ Prepare"]
        NET["Set-ADSKNetworkConfig\nStatic IP · DNS · Gateway"]
        RN["Rename-ADSKComputer\nHostname before join"]
        APP["Install-ADSKApplications\nNumbered install scripts"]
        ROLE["Install-ADSKRoles\nADDS · DNS · DHCP · IIS …"]
        MOD --> NET --> RN --> APP --> ROLE
    end

    subgraph PROMOTE["🏛️ Promote"]
        FOREST["New-ADSKForest"]
        REPLICA["Add-ADSKDomainController"]
        CHILD["New-ADSKChildDomain"]
        MEMBER["Add-ADSKDomainMember"]
        ROLE --> FOREST & REPLICA & CHILD & MEMBER
    end

    subgraph POSTBOOT["🔁 Post-Restart"]
        SITE["New-ADSKSite"]
        DHCP["New-ADSKDhcpScope"]
        FWD["Set-ADSKDnsForwarder"]
        FOREST & REPLICA & CHILD --> SITE & DHCP & FWD
    end

    subgraph HANDOFF["✅ Handoff"]
        READY(["Server ready\nfor ADOpsKit monitoring"])
        MEMBER --> READY
        SITE & DHCP & FWD --> READY
    end
```

---

## 2 · Wizard Flow (Start-ADSKSetupWizard)

```mermaid
flowchart TD
    START([Start-ADSKSetupWizard]) --> N

    N["1 · Network Config\nSet-ADSKNetworkConfig\nStatic IP · DNS · Gateway"] --> R

    R["2 · Rename Computer\nRename-ADSKComputer\nBefore domain join"] --> A

    A["3 · Install Applications\nInstall-ADSKApplications\nNumbered scripts in order\n01-app.ps1 · 02-agent.bat"] --> ROLE

    ROLE["4 · Install Roles\nInstall-ADSKRoles\nADDS · DNS · DHCP · IIS\nGPMC · RSAT · File · Print"] --> SCENE

    SCENE{5 · AD Scenario}
    SCENE -->|New forest| F["New-ADSKForest\nFirst DC in new domain"]
    SCENE -->|Replica DC| DC["Add-ADSKDomainController\nJoin existing domain as DC"]
    SCENE -->|Child domain| CD["New-ADSKChildDomain\nNew child under parent"]
    SCENE -->|Member server| MS["Add-ADSKDomainMember\nJoin domain — no DC role"]
    SCENE -->|Standalone| SA["No AD role\nServer only"]

    F & DC & CD --> REBOOT(["Mandatory reboot\nDC promotion requires restart"])
    REBOOT --> POST

    POST["6 · Post-Restart Tasks\nScheduled task auto-runs after reboot\n· New-ADSKSite\n· New-ADSKDhcpScope\n· Set-ADSKDnsForwarder"]

    MS & SA --> DONE([Done — ready for ADOpsKit])
    POST --> DONE
```

---

## 3 · Functions at a Glance

| Function | Stage | Purpose |
|----------|-------|---------|
| `Set-ADSKNetworkConfig` | Prepare | Static IP, DNS servers, default gateway |
| `Rename-ADSKComputer` | Prepare | Set hostname before domain join |
| `Install-ADSKApplications` | Prepare | Run numbered install scripts in order |
| `Install-ADSKRoles` | Prepare | Install Windows roles/features (ADDS, DNS, DHCP, IIS, GPMC, RSAT, File, Print) |
| `New-ADSKForest` | Promote | Create first DC in a new forest/domain |
| `Add-ADSKDomainController` | Promote | Join an existing domain as a replica DC |
| `New-ADSKChildDomain` | Promote | Create a new child domain under a parent |
| `Add-ADSKDomainMember` | Promote | Join domain as a member server (no DC role) |
| `New-ADSKSite` | Post-restart | Create AD site/subnet definitions |
| `New-ADSKDhcpScope` | Post-restart | Create and authorize a DHCP scope |
| `Set-ADSKDnsForwarder` | Post-restart | Configure DNS conditional/global forwarders |
| `Start-ADSKSetupWizard` | Orchestrator | Interactive wizard tying all stages together |

---

## 4 · Version History

| Version | Date | What changed |
|---------|------|-------------|
| **1.0.0** | 2026-06-28 | Initial PSGallery release — 12 provisioning functions and interactive setup wizard |
