# ADSetupKit

[![PSScriptAnalyzer](https://github.com/Karanth1992/ADSetupKit/actions/workflows/ci.yml/badge.svg)](https://github.com/Karanth1992/ADSetupKit/actions/workflows/ci.yml)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?logo=powershell)
![Platform](https://img.shields.io/badge/platform-Windows%20Server-lightgrey)

Interactive Windows Server setup toolkit — network configuration, role installation, DC promotion, domain join, application deployment, and post-provisioning tasks.

Detailed write-ups are published at **[karanth.ovh](https://karanth.ovh)**.

---

## Installation

```powershell
# Clone and import locally
git clone https://github.com/Karanth1992/ADSetupKit.git
Import-Module .\ADSetupKit\ADSetupKit.psd1

# Launch the interactive wizard
Start-ADSKSetupWizard
```

---

## Functions

| Function | Purpose |
|----------|---------|
| `Start-ADSKSetupWizard` | Interactive 7-step wizard — network, rename, apps, roles, AD scenario, post-restart tasks |
| `Set-ADSKNetworkConfig` | Set static IP, subnet, gateway, and DNS on a network adapter |
| `Rename-ADSKComputer` | Rename the local server with optional immediate restart |
| `Install-ADSKApplications` | Run numbered `.ps1`/`.bat` installer scripts in sequence from a folder |
| `Install-ADSKRoles` | Multi-select menu to install Windows Server roles and features |
| `New-ADSKForest` | Promote server as first DC in a new Active Directory forest |
| `Add-ADSKDomainController` | Add a replica DC to an existing domain |
| `New-ADSKChildDomain` | Create a new child domain under a parent domain |
| `Add-ADSKDomainMember` | Join server to an existing domain as a member server |
| `New-ADSKSite` | Create an AD site, subnet, and site link |
| `New-ADSKDhcpScope` | Create and activate a DHCP scope with gateway and DNS options |
| `Set-ADSKDnsForwarder` | Set DNS forwarders on the local DNS server |

---

## Wizard Flow

```
Step 1  Network Config      → Static IP, gateway, DNS
Step 2  Computer Rename     → Rename before domain operations
Step 3  App Installation    → Run numbered installer scripts from a folder
Step 4  Role Selection      → Multi-select: ADDS, DNS, DHCP, IIS, File, Print, RSAT
Step 5  AD Scenario         → New forest / Replica DC / Child domain / Member server / Standalone
Step 6  Post-Restart Tasks  → AD sites, DHCP scope, DNS forwarders (auto-scheduled startup task)
Step 7  Review & Execute
```

### Application Installation

Drop numbered scripts into a folder — ADSetupKit runs them in order:

```
C:\Installers\
├── 01-dotnet.ps1
├── 02-chrome.bat
└── 03-agent.ps1
```

```powershell
Install-ADSKApplications -ScriptFolder 'C:\Installers'
```

### Role Selection

Multi-select by entering comma-separated numbers:

```
What roles do you want to install?
  [0]  Recommended for DC (ADDS + DNS + GPMC)
  [1]  AD Domain Services (ADDS)
  [2]  DNS Server
  [3]  DHCP Server
  [4]  File Server
  [5]  Print Server
  [6]  Web Server (IIS)
  [7]  Group Policy Management (GPMC)
  [8]  RSAT - AD Tools
  [9]  RSAT - DNS Tools
  [10] RSAT - DHCP Tools

Enter numbers separated by commas: 1,2,3
```

---

## Post-Restart Tasks

When DC promotion is selected, the wizard asks if you want to configure:
- AD replication sites and subnets
- DHCP scopes
- DNS forwarders

These are written to `C:\ADSKSetup\PostRestart.ps1` and registered as a startup scheduled task. After the mandatory DC promotion reboot the script runs automatically, then deletes itself.

---

## Repository Structure

```
ADSetupKit/
├── Public/                 ← 12 exported functions
├── Private/
│   └── Helpers.ps1         ← Shared helpers (menus, banners, admin check)
├── en-US/
│   └── about_ADSetupKit.help.txt
├── ADSetupKit.psd1
└── ADSetupKit.psm1
```

---

## Requirements

- Windows PowerShell 5.1 or later
- Windows Server 2016 or later (recommended)
- Run as Administrator
- AD DS role available for DC promotion functions
- ActiveDirectory module (RSAT) for site/DHCP/DNS post-config functions

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| **1.0.0** | 2026-06-28 | Initial release — 12 functions, interactive setup wizard |

Full changelog: [CHANGELOG.md](CHANGELOG.md)

---

## Author

**K Shankar R Karanth** — Active Directory & Hybrid Identity Engineer
[karanth.ovh](https://karanth.ovh) · [LinkedIn](https://www.linkedin.com/in/karanth-shankar/)

---

## License

MIT — see [LICENSE](LICENSE)
