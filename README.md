# ⚙️ ADSetupKit

> **Automated Windows Server Provisioning & Active Directory Bootstrapping Module**

[![PowerShell Version](https://img.shields.io/badge/PowerShell-5.1%20%7C%207.x-5391FE?style=flat-square&logo=powershell&logoColor=white)](https://github.com/Karanth1992/ADSetupKit)
[![Platform](https://img.shields.io/badge/Platform-Windows%20Server%202016--2025-0078D4?style=flat-square&logo=windows&logoColor=white)](https://github.com/Karanth1992/ADSetupKit)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](LICENSE)

**ADSetupKit** is a companion PowerShell module engineered to automate the end-to-end lifecycle of standing up Windows Servers from bare-metal or fresh VM templates — from IP configuration and hostname assignment to role installation, Domain Controller promotion, domain join, and post-provisioning hardening.

---

## 🌟 Key Features

* 🧙‍♂️ **Interactive Setup Wizard (`Start-ADSKSetupWizard`):** Guided terminal interface for fast, error-free server deployment.
* 🌐 **Network & Identity Provisioning:** Automated static IP allocation, DNS configuration, and computer renaming with automatic reboots.
* 🏰 **Active Directory Promotion:** Unattended DC promotion (`Install-ADDSForest` / `Install-ADDSDomainController`) with automated FSMO, SYSVOL, and NTDS directory pathing.
* 📦 **Role & Feature Automation:** One-line installation of AD DS, DNS, DHCP, AD CS (Certificate Services), and IIS roles.
* 🛡️ **Post-Provisioning Hardening:** Disables legacy protocols (SMBv1, NTLMv1), configures enterprise NTP time hierarchy, and applies baseline security configurations.
* 📋 **Pre-Flight Diagnostics (`Test-ADSKProvisioningReadiness`):** Verifies disk space, network connectivity, administrator privileges, and OS version before executing provisioning tasks.

---

## 🏗️ Provisioning Workflow

```mermaid
graph TD
    A[Fresh Windows Server / VM] --> B[Pre-Flight Diagnostic Check]
    B --> C{Readiness Passed?}
    C -- No --> D[Report Errors & Stop]
    C -- Yes --> E[Network & Hostname Configuration]
    E --> F[Install Windows Roles & Features]
    F --> G{Deployment Type?}
    G -- New Forest --> H[Promote Root Domain Controller]
    G -- Additional DC --> I[Join Existing Domain & Promote DC]
    G -- Member Server --> J[Join Domain & Place in Target OU]
    H --> K[Post-Provisioning Security & NTP Hardening]
    I --> K
    J --> K
    K --> L[Provisioning Complete Report]
