# Changelog

All notable changes to ADSetupKit are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

### [1.0.0] – 2026-06-28

#### Added
- `Start-ADSKSetupWizard` — 7-step interactive wizard: network config, computer rename, application installation, role selection, AD scenario (new forest / replica DC / child domain / member server / standalone), post-restart task scheduling, and a review/confirm step before execution.
- `Set-ADSKNetworkConfig` — Configure static IP, subnet prefix, default gateway, and DNS servers on any network adapter. Cleans existing DHCP-assigned addresses before applying.
- `Rename-ADSKComputer` — Rename the local server. Optional `-Restart` switch for immediate reboot.
- `Install-ADSKApplications` — Sequentially execute numbered `.ps1` and `.bat` installer scripts from a folder. Per-script timeout enforced. Results exported to CSV and log file.
- `Install-ADSKRoles` — Multi-select interactive menu for installing Windows Server roles (ADDS, DNS, DHCP, File, Print, IIS, GPMC, RSAT). Also callable non-interactively via `-Roles`.
- `New-ADSKForest` — Promote server as first DC in a new Active Directory forest using `Install-ADDSForest`.
- `Add-ADSKDomainController` — Add server as a replica DC to an existing domain using `Install-ADDSDomainController`.
- `New-ADSKChildDomain` — Create a new child domain under a parent domain using `Install-ADDSDomain`.
- `Add-ADSKDomainMember` — Join the local server to an existing domain (member server). Optional OU placement and restart.
- `New-ADSKSite` — Create an AD replication site, associate a subnet, and optionally create a site link to an existing site.
- `New-ADSKDhcpScope` — Create and activate a DHCP scope with gateway (option 3) and DNS (option 6). Authorises the DHCP server in AD.
- `Set-ADSKDnsForwarder` — Replace DNS forwarder list on the local DNS server. Optional root hints toggle.
- `Private\Helpers.ps1` — Shared banner/status output helpers, admin check, single and multi-select menu readers, and `Register-ADSKStartupTask` for scheduling post-restart scripts.
- `en-US\about_ADSetupKit.help.txt` — Module help file covering all functions, wizard flow, and requirements.
