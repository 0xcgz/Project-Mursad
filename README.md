<div align="center">

<br>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=28&duration=3000&pause=1000&color=E94560&center=true&vCenter=true&width=600&lines=%F0%9F%91%81%EF%B8%8F+PROJECT+MURSAD;SOC+Telemetry+Lab;Enterprise+Security+Architecture">
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=28&duration=3000&pause=1000&color=E94560&center=true&vCenter=true&width=600&lines=%F0%9F%91%81%EF%B8%8F+PROJECT+MURSAD;SOC+Telemetry+Lab;Enterprise+Security+Architecture" alt="Project Mursad"/>
</picture>

<br><br>

<p align="center">
  <img src="https://img.shields.io/badge/Proxmox_VE-E57000?style=for-the-badge&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/pfSense_CE-212121?style=for-the-badge&logo=pfsense&logoColor=white"/>
  <img src="https://img.shields.io/badge/Active_Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white"/>
  <img src="https://img.shields.io/badge/Wazuh_SIEM-6B4CFF?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/Suricata_IDS%2FIPS-EF6C00?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Phase_1-COMPLETE-2ecc71?style=flat-square&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/Phase_2-COMPLETE-2ecc71?style=flat-square&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/Phase_3-COMPLETE-2ecc71?style=flat-square&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/Phase_4-IN_PROGRESS-e94560?style=flat-square&labelColor=0d1117"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Progress-90%25-e94560?style=flat-square&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/Platform-Proxmox_VE_9.1-E57000?style=flat-square&logo=proxmox&logoColor=white&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/Domain-mursad.local-0078D4?style=flat-square&logo=microsoft&logoColor=white&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/FQDN-mursad.me-6B4CFF?style=flat-square&labelColor=0d1117"/>
  <img src="https://img.shields.io/badge/License-MIT-2ecc71?style=flat-square&labelColor=0d1117"/>
</p>

<br>

> *A fully virtualized, enterprise-grade SOC environment built on Proxmox.*
> *Segmented firewall zones · Active Directory · Wazuh SIEM · Suricata IDS/IPS · Red Team capability.*

<br>

</div>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Network Zones](#-network-zones)
- [IP Address Table](#-ip-address-table)
- [Tech Stack](#-tech-stack)
- [Roadmap](#-roadmap)
- [Deployment Docs](#-deployment-docs)
  - [Phase 1 · 02 — Proxmox Deployment](#phase-1--02)
  - [Phase 1 · 03 — pfSense Installation](#phase-1--03)
  - [Phase 1 · 04 — Advanced Firewall Routing](#phase-1--04)
  - [Phase 2 · 05 — Domain Controller Provisioning](#phase-2--05)
  - [Phase 2 · 06 — Active Directory Installation](#phase-2--06)
  - [Phase 2 · 07 — DMZ Architecture Setup](#phase-2--07)
  - [Phase 2 · 08 — Suricata IDS/IPS Configuration](#phase-2--08)
  - [Phase 3 · 09 — LAN/DMZ Traffic Isolation & DNS Mapping](#phase-3--09)
  - [Phase 3 · 10 — Wazuh SIEM Installation](#phase-3--10)
  - [Phase 4 · 11 — Antivirus Integration & SIEM Testing](#phase-4--11)
  - [Phase 4 · 12 — Infrastructure Auditing via CIS Benchmarks](#phase-4--12)
  - [Phase 4 · 13 — Kaspersky Security Center EDR/AV Enterprise Setup](#phase-4--13)
- [Repository Structure](#-repository-structure)
- [Disclaimer](#-disclaimer)

---

## 🔍 Overview

Project Mursad is a fully virtualized, enterprise-grade Security Operations Center lab built for hands-on Blue Team and Red Team practice. Running entirely on a single Proxmox host nested inside VMware Workstation Pro, it simulates a realistic corporate environment with proper zone segmentation, Active Directory identity management, inline intrusion detection via Suricata, and centralized SIEM telemetry through Wazuh.

**Core objectives:**

| Objective | Description |
|-----------|-------------|
|  **Network Segmentation** | Multi-zone pfSense firewall — Workstation, Servers, DMZ isolation |
|  **Adversary Simulation** | Red Team capability via isolated Kali Linux on the WAN |
|  **SOC Telemetry** | Wazuh SIEM collecting logs from all endpoints and network infrastructure |
|  **Intrusion Detection** | Suricata IDS/IPS inline on pfSense for live traffic inspection |
|  **Endpoint Hardening** | CIS benchmarks, EDR via Kaspersky Security Center, AV integration |
|  **Incident Response** | Realistic alert triage, correlation, and response workflows |

---

## 🗺️ Architecture

```
╔══════════════════════════════════════════════════════════════════════════════════╗
║               VMware Workstation Pro 17  ·  Windows 11 Host                     ║
║  ┌───────────────────────────────────────────────────────────────────────────┐   ║
║  │                      Proxmox VE 9.1  —  node: mursad                      │   ║
║  │                                                                             │   ║
║  │          [ INTERNET ]                                                       │   ║
║  │               │                                                             │   ║
║  │          vmbr0 · 192.168.140.x/24  ◄── NAT via VMware                      │   ║
║  │               │                                                             │   ║
║  │  ┌────────────▼────────────────────────────────────────────────────────┐   │   ║
║  │  │                  VM 100  ·  pfSense CE  ·  Firewall                  │   │   ║
║  │  │           ┌──────────────────────────────────────┐                   │   │   ║
║  │  │           │         Suricata IDS/IPS (inline)     │                   │   │   ║
║  │  │           └──────────────────────────────────────┘                   │   │   ║
║  │  │  vtnet0   vtnet1    vtnet2    vtnet3    vtnet4    vtnet5              │   │   ║
║  │  │   WAN      LAN      OPT1      OPT2      OPT3      OPT4               │   │   ║
║  │  └────┼────────┼─────────┼─────────┼─────────┼─────────┼───────────────┘   │   ║
║  │       │        │         │         │         │         │                    │   ║
║  │  [ignored]  vmbr1      vmbr2     vmbr3     vmbr4     vmbr5                 │   ║
║  │            10.22.0/24 10.22.7/24 192.168.50/24 10.22.1/24 10.22.2/24      │   ║
║  │               HR       SERVERS      DMZ         IT        OPs              │   ║
║  │               │          │           │           │          │               │   ║
║  │         ┌─────┴───┐  ┌───┴──────┐  ┌┴────────┐  ┌──┴───┐  ┌──┴──┐         │   ║
║  │         │HR WS    │  │DC .7.3   │  │DMZ Srvr │  │IT WS │  │Ops  │         │   ║
║  │         │.0.x     │  │Wazuh .67 │  │.50.10   │  │.1.x  │  │.2.x │         │   ║
║  │         └─────────┘  └──────────┘  └─────────┘  └──────┘  └─────┘         │   ║
║  │                                                                             │   ║
║  │   [ Kali Linux ]  ──  WAN/DHCP  ──  External Red Team Attacker             │   ║
║  └───────────────────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════════════════╝
```

---

## 🌐 Network Zones

| Zone | Bridge | Subnet | Gateway | Purpose |
|------|:------:|--------|:-------:|---------|
|  WAN / Management | `vmbr0` | `192.168.140.0/24` | `192.168.140.2` | NAT uplink · Proxmox host access |
|  HR Workstation | `vmbr1` | `10.22.0.0/24` | `10.22.0.1` | HR domain-joined endpoints |
|  Servers | `vmbr2` | `10.22.7.0/24` | `10.22.7.1` | Internal services · DC · Wazuh SIEM |
|  DMZ | `vmbr3` | `192.168.50.0/24` | `192.168.50.1` | Isolated public-facing services |
|  IT Workstation | `vmbr4` | `10.22.1.0/24` | `10.22.1.1` | IT department endpoints |
|  OPs Workstation | `vmbr5` | `10.22.2.0/24` | `10.22.2.1` | Operations department endpoints |

---

## 📡 IP Address Table

| Host | IP Address | Zone | Role |
|------|:----------:|:----:|------|
| Proxmox Node | `192.168.140.129` | Management | Hypervisor — Web UI `:8006` |
| pfSense — WAN | `192.168.140.x` (DHCP) | WAN | Internet uplink |
| pfSense — HR | `10.22.0.1` | HR Workstation | HR segment gateway |
| pfSense — Servers | `10.22.7.1` | Servers | Servers segment gateway |
| pfSense — DMZ | `192.168.50.1` | DMZ | DMZ gateway |
| pfSense — IT | `10.22.1.1` | IT Workstation | IT segment gateway |
| pfSense — OPs | `10.22.2.1` | OPs Workstation | Operations segment gateway |
| Windows Server DC | `10.22.7.3` | Servers | AD · DNS · `mursad.local` |
| Wazuh SIEM | `10.22.7.67` | Servers | Log aggregation · alerting |
| EDR Server (KSC) | `10.22.7.5` | Servers | Kaspersky Security Center |
| IT Workstation | `10.22.1.x` | IT Workstation | Domain-joined · Wazuh agent |
| OPs Workstation | `10.22.2.x` | OPs Workstation | Domain-joined · Wazuh agent |
| DMZ Server | `192.168.50.10` | DMZ | Public-facing web server · XAMPP |
| Kali Linux | WAN DHCP | WAN | External Red Team attacker |

---

## 🧰 Tech Stack

| Component | Role | Version |
|-----------|------|:-------:|
| <img src="https://img.shields.io/badge/Proxmox_VE-E57000?style=flat-square&logo=proxmox&logoColor=white"/> | Type-1 hypervisor · bridge VLAN host | `9.1` |
| <img src="https://img.shields.io/badge/pfSense_CE-212121?style=flat-square&logo=pfsense&logoColor=white"/> | Edge firewall · router · VPN gateway | CE AMD64 |
| <img src="https://img.shields.io/badge/Suricata-EF6C00?style=flat-square&logoColor=white"/> | Inline IDS/IPS via pfSense package | Latest |
| <img src="https://img.shields.io/badge/Wazuh-6B4CFF?style=flat-square&logoColor=white"/> | SIEM · XDR · log aggregation · alerting | `4.x` |
| <img src="https://img.shields.io/badge/Windows_Server_2019-0078D4?style=flat-square&logo=microsoft&logoColor=white"/> | Active Directory · DNS · AD CS | Eval |
| <img src="https://img.shields.io/badge/Windows_10_Pro-0078D4?style=flat-square&logo=microsoft&logoColor=white"/> | Domain endpoints — IT · OPs | Eval |
| <img src="https://img.shields.io/badge/Kaspersky_KSC-006D5C?style=flat-square&logoColor=white"/> | Enterprise EDR · AV · centralized management | `12.x` |
| <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kali-linux&logoColor=white"/> | Red Team · penetration testing | Latest |

---

## 🗓️ Roadmap

<div align="center">

```
  PHASE 1          PHASE 2              PHASE 3          PHASE 4
Infrastructure  ──►  Identity       ──►  Telemetry   ──►  Hardening
  & Perimeter     & Segmentation       & Detection      & Validation
  [ COMPLETE ]     [ COMPLETE ]        [ COMPLETE ]       [ ACTIVE ]
```

</div>

<br>

### ◆ Phase 1 — Infrastructure & Perimeter Initialization

> *Establishing the hypervisor environment and securing the network edge.*

| # | Task | Status |
|:---:|------|:------:|
| `[01]` | Project Architecture & Introduction | ✅ |
| `[02]` | Proxmox Hypervisor Deployment & Bridge VLAN Engineering | ✅ |
| `[03]` | pfSense Edge Firewall Installation & Baseline Setup | ✅ |
| `[04]` | Advanced Firewall Routing & Network Configuration | ✅ |

<br>

### ◆ Phase 2 — Enterprise Identity & Network Segmentation

> *Building the corporate network, managing identities, and deploying IDS/IPS.*

| # | Task | Status |
|:---:|------|:------:|
| `[05]` | Domain Controller Provisioning & Network Integration | ✅ |
| `[06]` | Active Directory Domain Installation & Configuration | ✅ |
| `[07]` | DMZ Architecture Setup | ✅ |
| `[08]` | Suricata IDS/IPS Configuration | ✅ |

<br>

### ◆ Phase 3 — SOC Telemetry & Traffic Analysis

> *Deploying the eyes and ears of the network.*

| # | Task | Status |
|:---:|------|:------:|
| `[09]` | LAN/DMZ Traffic Isolation & Secure Local DNS Mapping | ✅ |
| `[10]` | Wazuh SIEM Installation · Syslog Ingestion · Agent Rollout | ✅ |

<br>

### ◆ Phase 4 — Endpoint Defense, Hardening & Validation

> *Locking down endpoints and validating detection capability.*

| # | Task | Status |
|:---:|------|:------:|
| `[11]` | Antivirus Integration & SIEM Efficiency Testing | ✅ |
| `[12]` | Infrastructure Auditing via CIS Benchmarks | ✅ |
| `[13]` | Kaspersky Security Center (EDR/AV) Enterprise Setup | ✅ |
| `[14]` | Final Security Review & Operations Wrap-Up | ⬜ |

<br>

<div align="center">

`✅` Complete &nbsp;·&nbsp; `🔄` In Progress &nbsp;·&nbsp; `⬜` Pending

</div>

---

## 📂 Deployment Docs

> Click any phase card below to expand the full step-by-step deployment guide.

---

<details>
<summary><b>📘 Phase 1 · [02] — Proxmox Hypervisor Deployment & Bridge VLAN Configuration</b></summary>
<a name="phase-1--02"></a>

<br>

> **Scope:** Nested installation of **Proxmox VE 9.1** inside **VMware Workstation Pro**, establishing the foundational hypervisor layer and complete internal network backbone for the Mursad SOC environment.

---

### Overview

```
[ VMware Workstation Pro 17 ]
        └── VM: "Project Mursad"  (50 GB · 4 GB RAM · 2 vCPUs)
                └── Proxmox VE 9.1  —  node: mursad
                        ├── vmbr0  →  Management / NAT       192.168.140.129/24
                        ├── vmbr1  →  HR Workstation          10.22.0.1/24
                        ├── vmbr2  →  Servers Segment         10.22.7.1/24
                        ├── vmbr3  →  DMZ Zone                192.168.50.1/24
                        ├── vmbr4  →  IT Workstation          10.22.1.1/24
                        └── vmbr5  →  OPs Workstation         10.22.2.1/24
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Preparation | Define the VMware virtual hardware container |
| **B** | Proxmox OS Install | Deploy the hypervisor operating system |
| **C** | Web GUI & Network | Engineer the internal SOC bridge network |

---

### Part A — Virtual Machine Preparation

#### Step 1 — Download Proxmox VE

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/1.png)

Navigate to [proxmox.com/en/downloads](https://www.proxmox.com/en/downloads) and download **Proxmox VE 9.1-1 ISO Installer**.

---

#### Step 2 — Create a New Virtual Machine

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/2.png)

Launch **VMware Workstation Pro 17** and create a new VM via:
- **"Create a New Virtual Machine"** on the dashboard, or
- **File → New Virtual Machine**

---

#### Step 3 — Mount the Proxmox ISO

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/3.png)

Select **"Installer disc image file (iso)"** → Browse → mount:

```
proxmox-ve_9.1-1.iso
```

---

#### Step 4 — Select Guest OS

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/4.png)

| Setting | Value |
|---------|-------|
| Guest OS | Linux |
| Version | Ubuntu *(Proxmox is Debian-based — Ubuntu profile ensures nested compatibility)* |

---

#### Step 5 — Name & Storage Path

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/5.png)

| Setting | Value |
|---------|-------|
| VM Name | `Project Mursad` |
| Location | *(path with sufficient free space)* |

---

#### Step 6 — Disk Allocation

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/6.png)

| Setting | Value |
|---------|-------|
| Maximum Disk Size | `50.0 GB` |
| Storage Mode | Split virtual disk into multiple files |

---

#### Step 7 — Hardware Summary & Power On

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/7.png)
![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/8.png)

| Resource | Minimum |
|----------|:-------:|
| RAM | 4 GB (4096 MB) |
| CPU Cores | 2 |
| Disk | 50 GB |

Click **"Power on this virtual machine"** to begin OS installation.

---

### Part B — Proxmox OS Installation

#### Step 8 — Boot Menu

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/9.png)
![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/10.png)

Select with arrow keys:

```
Install Proxmox VE (Graphical)
```

> ⚠️ **Nested Virtualization Warning**
>
> ![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/11.png)
>
> If you receive a *"No support for hardware-accelerated KVM"* error, verify **"Virtualize AMD-V/RVI"** is enabled under VMware Processor settings. If the issue persists on Windows 11, Credential Guard / VBS may be blocking AMD-V passthrough — run Microsoft's `DG_Readiness_Tool_v3.6.ps1 -Disable` and reboot.

---

#### Step 9 — EULA

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/12.png)

Click **"I agree"** to proceed.

---

#### Step 10 — Target Disk

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/13.png)

Select:

```
/dev/sda  —  50 GiB VMware Virtual Disk
```

---

#### Step 11 — Localization

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/14.png)

| Setting | Value |
|---------|-------|
| Country | Bahrain |
| Time Zone | Asia/Bahrain |
| Keyboard Layout | U.S. English |

---

#### Step 12 — Root Password & Email

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/15.png)

Set a strong password for the `root` account and provide an email for system alerts.

---

#### Step 13 — Management Network

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/16.png)

| Parameter | Value |
|-----------|-------|
| Hostname (FQDN) | `mursad.me` |
| IP Address / CIDR | `192.168.140.129/24` |
| Gateway | `192.168.140.2` |
| DNS Server | `192.168.140.2` |

---

#### Step 14 — Install & Reboot

![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/17.png)
![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/18.png)

Check **"Automatically reboot after successful installation"** → click **Install**.

After reboot, the CLI console displays:

```
https://192.168.140.129:8006/
```

> 📋 Save this URL — primary access point for all Proxmox management.

---

### Part C — Web GUI & Network Engineering

#### Step 15 — Login to Web UI

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/19.png)
![20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/20.png)

Navigate to `https://192.168.140.129:8006/`

| Field | Value |
|-------|-------|
| User name | `root` |
| Password | *(configured root password)* |
| Realm | `Linux PAM standard authentication` |

---

#### Step 16 — Create All SOC Bridges

![21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/21.png)
![22](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/22.png)

Navigate to **mursad → System → Network → Create → Linux Bridge**.

Repeat for each bridge below:

| Bridge | CIDR | Autostart | Comment | Role |
|:------:|------|:---------:|---------|------|
| `vmbr0` | `192.168.140.129/24` | ✅ | — | Management / NAT uplink via `nic0` |
| `vmbr1` | `10.22.0.1/24` | ✅ | `HR Workstation` | HR Workstation segment |
| `vmbr2` | `10.22.7.1/24` | ✅ | `Servers` | Internal servers segment |
| `vmbr3` | `192.168.50.1/24` | ✅ | `DMZ` | Demilitarized Zone |
| `vmbr4` | `10.22.1.1/24` | ✅ | `IT Workstation` | IT Workstation segment |
| `vmbr5` | `10.22.2.1/24` | ✅ | `Operations Workstation` | OPs Workstation segment |

> ⚠️ `vmbr2` through `vmbr5` will show **Active: No** until a VM is attached — this is expected.

Click **"Apply Configuration"**. The pending diff confirms changes written to `/etc/network/interfaces`:

```diff
+auto vmbr1
+iface vmbr1 inet static
+    address 10.22.0.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr2
+iface vmbr2 inet static
+    address 10.22.7.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr3
+iface vmbr3 inet static
+    address 192.168.50.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr4
+iface vmbr4 inet static
+    address 10.22.1.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr5
+iface vmbr5 inet static
+    address 10.22.2.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
```

---

#### Step 17 — Verify Final Network State

![23](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/23.png)
![24](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/02-proxmox-deployment/24.png)

| Bridge | Type | Active | Autostart | CIDR | Comment |
|:------:|------|:------:|:---------:|------|---------|
| `nic0` | Network Device | ✅ | — | — | Physical uplink `enp2s1` |
| `vmbr0` | Linux Bridge | ✅ | ✅ | `192.168.140.129/24` | Management / NAT |
| `vmbr1` | Linux Bridge | ✅ | ✅ | `10.22.0.1/24` | HR Workstation |
| `vmbr2` | Linux Bridge | ✅ | ✅ | `10.22.7.1/24` | Servers |
| `vmbr3` | Linux Bridge | — | ✅ | `192.168.50.1/24` | DMZ |
| `vmbr4` | Linux Bridge | — | ✅ | `10.22.1.1/24` | IT Workstation |
| `vmbr5` | Linux Bridge | — | ✅ | `10.22.2.1/24` | Operations Workstation |

```
Proxmox Node: mursad
│
├── vmbr0  ──►  Management / NAT     192.168.140.129/24   (nic0 uplink)
├── vmbr1  ──►  HR Workstation       10.22.0.1/24
├── vmbr2  ──►  Servers Segment      10.22.7.1/24
├── vmbr3  ──►  DMZ Zone             192.168.50.1/24
├── vmbr4  ──►  IT Workstation       10.22.1.1/24
└── vmbr5  ──►  OPs Workstation      10.22.2.1/24
```

---

### ✅ Phase Checklist

- [ ] Proxmox VE 9.1 installed and booting correctly
- [ ] Web GUI accessible at `https://192.168.140.129:8006/`
- [ ] `vmbr0` active — IP `192.168.140.129/24`, gateway `192.168.140.2`
- [ ] `vmbr1` created — `10.22.0.1/24`, comment `HR Workstation`, Autostart ✅
- [ ] `vmbr2` created — `10.22.7.1/24`, comment `Servers`, Autostart ✅
- [ ] `vmbr3` created — `192.168.50.1/24`, comment `DMZ`, Autostart ✅
- [ ] `vmbr4` created — `10.22.1.1/24`, comment `IT Workstation`, Autostart ✅
- [ ] `vmbr5` created — `10.22.2.1/24`, comment `Operations Workstation`, Autostart ✅
- [ ] **"Apply Configuration"** clicked — no pending changes remaining

<div align="center"><br>

**🟢 Phase Complete**

`[01] Project Architecture` ◄── **`[02] Proxmox Deployment`** ──► `[03] pfSense Edge Firewall`

<br></div>

</details>

---

<details>
<summary><b>📗 Phase 1 · [03] — pfSense Edge Firewall Installation & Baseline Setup</b></summary>
<a name="phase-1--03"></a>

<br>

> **Scope:** Deployment of the **pfSense CE** virtual appliance within Proxmox, mapping all network interfaces to the Linux Bridges engineered in `[02]`, and establishing baseline routing and DHCP for the Mursad SOC environment.

---

### Overview

```
Proxmox Node: mursad
└── VM 100  —  Firewall  (pfSense CE)
        ├── vtnet0  →  vmbr0  →  WAN      192.168.140.x/24  (DHCP from host)
        ├── vtnet1  →  vmbr1  →  LAN      10.22.0.1/24      (HR Workstation)
        ├── vtnet2  →  vmbr2  →  OPT1     10.22.7.1/24      (Servers)
        ├── vtnet3  →  vmbr3  →  OPT2     192.168.50.1/24   (DMZ)
        ├── vtnet4  →  vmbr4  →  OPT3     10.22.1.1/24      (IT Workstation)
        └── vtnet5  →  vmbr5  →  OPT4     10.22.2.1/24      (OPs Workstation)
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Provisioning | Create the VM, upload ISO, attach all NICs before first boot |
| **B** | OS Installation | Install pfSense CE onto the virtual disk |
| **C** | Interface & Routing | Map bridges, assign IPs, enable DHCP |

---

### Part A — Virtual Machine Provisioning

#### Step 1 — Download pfSense CE ISO

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/1.png)
![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/2.png)

Navigate to [pfsense.org/download](https://www.pfsense.org/download/) and select:

| Setting | Value |
|---------|-------|
| Architecture | AMD64 (64-bit) |
| Image Type | **AMD64 ISO IPMI/Virtual Machines** |

> ⚠️ Download arrives as `.gz`. Extract to obtain the `.iso` before uploading to Proxmox.

---

#### Step 2 — Upload ISO to Proxmox

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/3.png)
![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/4.png)
![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/5.png)
![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/6.png)

1. Navigate to **mursad → local (mursad) → ISO Images**
2. Click **Upload → Select File** → browse to extracted `.iso`
3. Wait for task log: `TASK OK`

---

#### Step 3 — Create the VM

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/7.png)

Click **"Create VM"** in the top-right corner.

---

#### Step 4 — General Tab

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/8.png)

| Setting | Value |
|---------|-------|
| VM ID | `100` |
| Name | `Firewall` |
| Node | `mursad` |

---

#### Step 5 — OS Tab

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/9.png)

| Setting | Value |
|---------|-------|
| Storage | `local (mursad)` |
| ISO Image | pfSense CE ISO |
| Guest OS Type | Other |

---

#### Step 6 — System Tab

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/10.png)

| Setting | Value |
|---------|-------|
| BIOS | SeaBIOS |
| Machine | i440fx |
| SCSI Controller | VirtIO SCSI |

> Leave defaults — pfSense CE does not require OVMF/UEFI.

---

#### Step 7 — Disks Tab

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/11.png)

| Setting | Value |
|---------|-------|
| Storage | `local-lvm` |
| Disk Size | `32 GiB` |
| Format | raw |

---

#### Step 8 — CPU Tab

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/12.png)

| Setting | Value |
|---------|-------|
| Cores | `2` |
| Type | `host` |

> ⚠️ **Critical:** CPU type `host` passes AMD Ryzen flags directly into the VM. Without this, pfSense/FreeBSD will fail to boot stably under nested virtualization.

---

#### Step 9 — Memory Tab

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/13.png)

| Setting | Value |
|---------|-------|
| Memory | `4096 MiB (4 GB)` |

> 4 GB provides headroom for pfSense plus Suricata IDS/IPS when installed.

---

#### Step 10 — Network Tab

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/14.png)

| Setting | Value |
|---------|-------|
| Bridge | `vmbr0` |
| Model | `VirtIO (paravirtualized)` |
| Firewall | Unchecked |

> WAN interface only. Remaining bridges are added in Step 12 — **before first boot**.

---

#### Step 11 — Confirm & Finish

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/15.png)

Review and click **Finish**. Do **not** check "Start after created".

---

#### Step 12 — Add Remaining Network Interfaces

> ⚠️ **Complete this before first boot.** All six NICs must exist when pfSense first boots so FreeBSD can detect and enumerate them during setup.

Navigate to **VM 100 → Hardware → Add → Network Device**:

| NIC | Bridge | Model | Zone |
|:---:|:------:|:-----:|------|
| net1 | `vmbr1` | VirtIO | HR Workstation |
| net2 | `vmbr2` | VirtIO | Servers |
| net3 | `vmbr3` | VirtIO | DMZ |
| net4 | `vmbr4` | VirtIO | IT Workstation |
| net5 | `vmbr5` | VirtIO | OPs Workstation |

---

### Part B — pfSense OS Installation

#### Step 13 — Boot & Launch Console

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/16.png)
![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/17.png)

Select **VM 100 → Start → Console**. Allow bootloader timer to expire or press **Enter**.

---

#### Step 14 — Accept License

![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/18.png)

Accept the Netgate Copyright and Trademark notice.

---

#### Step 15 — Select Install

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/19.png)

Select **Install pfSense** from the welcome menu.

---

#### Step 16 — Partition Scheme

![20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/20.png)

Select **Auto (ZFS)** — provides data integrity, snapshot support, and is ideal for firewall appliances.

---

### Part C — Interface Assignment & Routing

#### Step 17 — VLAN Setup

![21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/21.png)

```
Should VLANs be set up now? [y/n]  →  n
```

Segmentation is handled by separate Proxmox bridges — VLANs are not required.

---

#### Step 18 — Assign Interfaces

![22](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/22.png)
![23](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/23.png)
![24](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/24.png)
![25](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/25.png)

| Prompt | Input | Maps To |
|--------|:-----:|---------|
| WAN interface | `vtnet0` | `vmbr0` — Internet |
| LAN interface | `vtnet1` | `vmbr1` — HR Workstation |
| OPT1 interface | `vtnet2` | `vmbr2` — Servers |
| OPT2 interface | `vtnet3` | `vmbr3` — DMZ |
| OPT3 interface | `vtnet4` | `vmbr4` — IT Workstation |
| OPT4 interface | `vtnet5` | `vmbr5` — OPs Workstation |

Type `y` → Enter to confirm.

---

#### Step 19 — Verify Boot & WAN DHCP

![26](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/26.png)
![27](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/27.png)

```
WAN (vtnet0) →  192.168.140.xxx/24   ✅  DHCP from Proxmox host
LAN (vtnet1) →  no IP yet            ←   configure next
```

---

#### Step 20 — Set LAN IP Address

![28](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/28.png)
![29](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/29.png)

Console menu → type `2` → Enter → select `2` for **LAN**.

---

#### Step 21 — Configure LAN

![30](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/30.png)
![31](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/31.png)
![32](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/32.png)

| Prompt | Value |
|--------|-------|
| IPv4 Address | `10.22.0.1` |
| Subnet bit count | `24` |
| Upstream gateway | *(Enter — none)* |
| IPv6 address | *(Enter — skip)* |

---

#### Step 22 — Enable DHCP on LAN

![33](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/33.png)
![34](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/34.png)
![35](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/35.png)
![36](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/36.png)

Enable DHCP server: `y`

| Prompt | Value |
|--------|-------|
| DHCP range start | `10.22.0.100` |
| DHCP range end | `10.22.0.200` |

---

#### Step 23 — Confirm & Access WebConfigurator

![37](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/37.png)
![38](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/03-pfsense-installation/38.png)

Revert to HTTP: `n` — keep HTTPS.

```
LAN (vtnet1) →  10.22.0.1/24  ✅
```

Access the WebConfigurator: `https://10.22.0.1` · Username: `admin` · Password: `pfsense`

> 🔐 **Change the default password immediately upon first login.**

---

### Interface Summary

| pfSense | vtnet | Bridge | IP | Zone |
|:-------:|:-----:|:------:|:--:|:----:|
| WAN | vtnet0 | vmbr0 | DHCP `192.168.140.x` | Internet |
| LAN | vtnet1 | vmbr1 | `10.22.0.1/24` | HR Workstation |
| OPT1 | vtnet2 | vmbr2 | `10.22.7.1/24` | Servers *(configured in `[04]`)* |
| OPT2 | vtnet3 | vmbr3 | `192.168.50.1/24` | DMZ *(configured in `[04]`)* |
| OPT3 | vtnet4 | vmbr4 | `10.22.1.1/24` | IT Workstation *(configured in `[04]`)* |
| OPT4 | vtnet5 | vmbr5 | `10.22.2.1/24` | OPs Workstation *(configured in `[04]`)* |

---

### ✅ Phase Checklist

- [ ] pfSense CE ISO uploaded to Proxmox local storage
- [ ] VM 100 created — CPU type `host`, machine `i440fx`
- [ ] All 6 NICs attached (vmbr0–vmbr5) **before first boot**
- [ ] pfSense installed with Auto (ZFS) partitioning
- [ ] Interfaces assigned: vtnet0=WAN · vtnet1=LAN · vtnet2=OPT1 · vtnet3=OPT2 · vtnet4=OPT3 · vtnet5=OPT4
- [ ] WAN acquiring DHCP from host network
- [ ] LAN configured at `10.22.0.1/24` with DHCP pool `100–200`
- [ ] WebConfigurator accessible at `https://10.22.0.1`
- [ ] Default `admin` password changed

<div align="center"><br>

**🟢 Phase Complete**

`[02] Proxmox Deployment` ◄── **`[03] pfSense Installation`** ──► `[04] Advanced Firewall Routing`

<br></div>

</details>

---

<details>
<summary><b>📙 Phase 1 · [04] — Advanced Firewall Routing & Network Configuration</b></summary>
<a name="phase-1--04"></a>

<br>

> **Scope:** Attaching all five VLAN bridges to the pfSense VM, provisioning and naming every interface (HR · SERVERS · DMZ · IT · OPs), configuring Hybrid Outbound NAT, and establishing the baseline WAN firewall rule that brings the full Mursad SOC network online.

---

### Overview

```
Proxmox Node: mursad
└── VM 100  —  Firewall  (pfSense CE)
        ├── vtnet0  →  vmbr0  →  WAN      192.168.140.x/24  (DHCP)
        ├── vtnet1  →  vmbr1  →  HR       10.22.0.1/24
        ├── vtnet2  →  vmbr2  →  SERVERS  10.22.7.1/24
        ├── vtnet3  →  vmbr3  →  DMZ      192.168.50.1/24
        ├── vtnet4  →  vmbr4  →  IT       10.22.1.1/24
        └── vtnet5  →  vmbr5  →  OPs      10.22.2.1/24
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Network Provisioning | Attach all VLAN bridges to the Firewall VM in Proxmox |
| **B** | Firewall Rules | WAN baseline rule for management access |
| **C** | Outbound NAT | Switch to Hybrid mode, add manual mappings per zone |
| **D** | Interface Assignment | Enable, name, and address all OPT interfaces in pfSense |

---

### Part A — Virtual Machine Network Provisioning

#### Step 1 — Review Proxmox Network Bridges

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/1.png)

Confirm all five internal bridges are present on the **mursad** node before proceeding.

| Bridge | Zone | Subnet |
|:------:|------|--------|
| `vmbr1` | HR Workstation | `10.22.0.0/24` |
| `vmbr2` | Servers | `10.22.7.0/24` |
| `vmbr3` | DMZ | `192.168.50.0/24` |
| `vmbr4` | IT Workstation | `10.22.1.0/24` |
| `vmbr5` | OPs Workstation | `10.22.2.0/24` |

---

#### Step 2 — Add Network Devices to Firewall VM

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/2.png)

Navigate to **VM 100 (Firewall) → Hardware → Add → Network Device**.

---

#### Step 3 — Assign All Bridges to Firewall

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/3.png)

Add one **VirtIO** Network Device per bridge:

| NIC | Bridge | Model |
|:---:|:------:|:-----:|
| net1 | `vmbr1` | VirtIO |
| net2 | `vmbr2` | VirtIO |
| net3 | `vmbr3` | VirtIO |
| net4 | `vmbr4` | VirtIO |
| net5 | `vmbr5` | VirtIO |

> ⚠️ All NICs must be added **before** booting the VM.

---

#### Step 4 — Verify Firewall Hardware

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/4.png)

Open **VM 100 → Hardware** and confirm the full NIC list:

```
net0  →  vmbr0   WAN
net1  →  vmbr1   HR
net2  →  vmbr2   SERVERS
net3  →  vmbr3   DMZ
net4  →  vmbr4   IT
net5  →  vmbr5   OPs
```

---

### Part B — pfSense Firewall Rules

#### Step 5 — Navigate to Firewall Rules

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/5.png)

Log into the pfSense WebConfigurator at `https://10.22.0.1` and navigate to **Firewall → Rules**.

---

#### Step 6 — Add WAN Firewall Rule

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/6.png)

Select the **WAN** tab. Click **Add ↑** to insert a new rule at the top of the list.

---

#### Step 7 — Configure WAN Rule Parameters

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/7.png)

| Field | Value |
|-------|-------|
| Action | Pass |
| Interface | WAN |
| Address Family | IPv4 |
| Protocol | Any |
| Source | WAN subnets |
| Destination | This Firewall (self) |
| Description | `Allow Management and Services to Firewall` |

Click **Save**.

---

#### Step 8 — Apply WAN Rule

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/8.png)

Click **Apply Changes** to activate the rule.

---

### Part C — Outbound NAT Configuration

#### Step 9 — Navigate to Outbound NAT

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/9.png)

Navigate to **Firewall → NAT → Outbound**.

---

#### Step 10 — Enable Hybrid Outbound NAT

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/10.png)

Select **Hybrid Outbound NAT rule generation** and click **Save**.

> ℹ️ Hybrid keeps automatic rules for internet access while allowing custom mappings per zone.

---

#### Step 11 — Add HR Workstation NAT Mapping

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/11.png)

| Field | Value |
|-------|-------|
| Source Network | `10.22.0.0` / `24` |
| Description | `Manual NAT — HR Workstation` |

---

#### Step 12 — Add SERVERS NAT Mapping

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/12.png)

| Field | Value |
|-------|-------|
| Source Network | `10.22.7.0` / `24` |
| Description | `Manual NAT — SERVERS` |

---

#### Step 13 — Add DMZ NAT Mapping

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/13.png)

| Field | Value |
|-------|-------|
| Protocol | **TCP/UDP** |
| Source Network | `192.168.50.0` / `24` |
| Description | `Manual NAT — DMZ` |

---

#### Step 14 — Review & Apply NAT Mappings

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/14.png)

```
[MANUAL]  10.22.0.0/24    →  WAN  (Any)       Manual NAT — HR Workstation
[MANUAL]  10.22.7.0/24    →  WAN  (Any)       Manual NAT — SERVERS
[MANUAL]  192.168.50.0/24 →  WAN  (TCP/UDP)   Manual NAT — DMZ
──────────────────────────────────────────────────────────────────────
[AUTO]    ...automatic rules below...
```

Click **Apply Changes**.

---

### Part D — Interface Assignment

#### Step 15 — Navigate to Interface Assignments

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/15.png)

Navigate to **Interfaces → Assignments**.

---

#### Step 16 — Configure SERVERS Interface (OPT1)

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/16.png)

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `SERVERS` |
| IPv4 Address | `10.22.7.1` / `24` |

---

#### Step 17 — Configure DMZ Interface (OPT2)

![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/17.png)

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `DMZ` |
| IPv4 Address | `192.168.50.1` / `24` |

---

#### Step 18 — Configure IT Interface (OPT3)

![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/18.png)

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `IT` |
| IPv4 Address | `10.22.1.1` / `24` |

---

#### Step 19 — Configure OPs Interface (OPT4)

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/19.png)

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `OPs` |
| IPv4 Address | `10.22.2.1` / `24` |

---

#### Step 20 — Verify Complete Interface Table

![20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/20.png)

| Interface | vtnet | Bridge | Description | IPv4 Address |
|:---------:|:-----:|:------:|-------------|:------------:|
| WAN | vtnet0 | vmbr0 | INTERNET | DHCP |
| LAN | vtnet1 | vmbr1 | HR | `10.22.0.1/24` |
| OPT1 | vtnet2 | vmbr2 | SERVERS | `10.22.7.1/24` |
| OPT2 | vtnet3 | vmbr3 | DMZ | `192.168.50.1/24` |
| OPT3 | vtnet4 | vmbr4 | IT | `10.22.1.1/24` |
| OPT4 | vtnet5 | vmbr5 | OPs | `10.22.2.1/24` |

---

#### Step 21 — Review pfSense Dashboard

![21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-1/04-firewall-routing/21.png)

```
WAN      →  192.168.140.x/24   ✅
HR       →  10.22.0.1/24       ✅
SERVERS  →  10.22.7.1/24       ✅
DMZ      →  192.168.50.1/24    ✅
IT       →  10.22.1.1/24       ✅
OPs      →  10.22.2.1/24       ✅
```

---

### ✅ Phase Checklist

- [ ] All 5 Proxmox bridges verified before VM modification
- [ ] `net1` through `net5` added to Firewall VM (all VirtIO)
- [ ] WAN pass rule created — source: WAN subnets · destination: This Firewall
- [ ] Outbound NAT switched to **Hybrid** mode
- [ ] Manual NAT mapping created for HR `10.22.0.0/24` (Any)
- [ ] Manual NAT mapping created for SERVERS `10.22.7.0/24` (Any)
- [ ] Manual NAT mapping created for DMZ `192.168.50.0/24` (TCP/UDP)
- [ ] All NAT mappings applied
- [ ] OPT1 enabled as `SERVERS` — `10.22.7.1/24`
- [ ] OPT2 enabled as `DMZ` — `192.168.50.1/24`
- [ ] OPT3 enabled as `IT` — `10.22.1.1/24`
- [ ] OPT4 enabled as `OPs` — `10.22.2.1/24`
- [ ] Dashboard shows all 6 interfaces active with correct IPs

<div align="center"><br>

**🟢 Phase 1 Complete**

`[03] pfSense Installation` ◄── **`[04] Advanced Firewall Routing`** ──► `[05] Domain Controller Provisioning`

<br></div>

</details>

---

<details>
<summary><b>📘 Phase 2 · [05] — Domain Controller Provisioning & Network Integration</b></summary>
<a name="phase-2--05"></a>

<br>

> **Scope:** Establishing the DHCP and DNS routing for the SERVERS zone in pfSense, provisioning the Windows Server Domain Controller (DC) virtual machine in Proxmox, installing the OS and VirtIO drivers, configuring the host identity, and verifying outbound internet connectivity through the firewall.

---

### Overview

```text
Proxmox Node: mursad
└── VM 101  —  DC  (Windows Server 2019)
        ├── Network: vmbr2 (SERVERS)
        ├── Compute: 2 vCores · 8 GB RAM
        └── Storage: 60 GB Disk · VirtIO Drivers

pfSense WebConfigurator
└── DHCP Server
        ├── Interface: SERVERS (OPT1)
        ├── Backend: Kea DHCP
        └── Range: 10.22.7.3 – 10.22.7.50
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | pfSense DHCP Configuration | Establish DHCP and DNS resolution for the SERVERS zone |
| **B** | VM Hardware Provisioning | Create and allocate resources for the Domain Controller VM |
| **C** | Windows Server Installation | Deploy Windows Server 2019 Desktop Experience |
| **D** | Drivers & Hostname Configuration | Install VirtIO drivers and set the DC hostname |
| **E** | Routing & Internet Verification | Configure pfSense gateways and rules to allow outbound traffic |

---

### Part A — pfSense DHCP Configuration

#### Step 1 — Enable SERVERS DHCP Server

Navigate to **Services → DHCP Server → SERVERS** tab.

| Field | Value |
|-------|-------|
| Enable | ✅ Enable DHCP server on SERVERS interface |
| Range From | `10.22.7.3` |
| Range To | `10.22.7.50` |

> **Note:** The range starts at `.3` to reserve `.1` for the gateway and `.2` for future use.

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/1.png)

---

#### Step 2 — Switch to Kea DHCP Backend

Navigate to **System → Advanced → Networking → DHCP Backend**, select **Kea DHCP**, and click **Save**.

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/2.png)

---

#### Step 3 — Configure DHCP DNS Servers

Navigate back to **Services → DHCP Server → SERVERS → Servers** section. Add `10.22.7.1` as the primary DNS server.

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/3.png)

---

### Part B — Domain Controller VM Provisioning

> ⚠️ **Prerequisite:** Ensure the **Windows Server ISO** and **VirtIO Windows Drivers ISO** are uploaded to Proxmox local storage before proceeding.

#### Step 4 — Create the DC Virtual Machine

VM ID: `101` · Name: `DC`

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/4.png)

---

#### Step 5 — Select Guest OS

Guest OS Type: `Microsoft Windows` · ISO: Windows Server

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/5.png)

---

#### Step 6 — Disk Allocation

Bus/Device: **VirtIO Block** · Disk Size: **60 GB**

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/6.png)

---

#### Step 7 — CPU Configuration

Cores: `2` · Type: `host`

> ⚠️ **Critical:** CPU type `host` is required for nested virtualization stability.

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/7.png)

---

#### Step 8 — Memory (RAM) Allocation

Memory: **8192 MiB (8 GB)**

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/8.png)

---

#### Step 9 — Network Configuration

Bridge: `vmbr2` (SERVERS) · Model: `VirtIO` · Firewall: Unchecked

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/9.png)

---

#### Step 10 — Attach VirtIO Drivers CD/DVD

Navigate to **VM 101 → Hardware → Add → CD/DVD Drive** and select the `virtio-win` ISO.

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/10.png)

---

### Part C — Windows Server Installation

#### Step 11 — Power On and Start Setup

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/11.png)

---

#### Step 12 — Install Now

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/12.png)

---

#### Step 13 — Select Desktop Experience

Choose **Windows Server (Desktop Experience)** — Server Core has no GUI.

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/13.png)

---

#### Step 14 — Custom Installation

Select **Custom: Install Windows only (advanced)**.

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/14.png)

---

#### Step 15 — Select the VirtIO Storage Drive

Select **Drive 0** (60 GB). If it doesn't appear, load the `viostor` driver from the VirtIO CD.

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/15.png)

---

#### Step 16 — Set Local Administrator Password

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/16.png)

---

#### Step 17 — Welcome to Windows Server

![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/17.png)

---

### Part D — Drivers & Hostname Configuration

#### Step 18 — Run the VirtIO Installer

Navigate to the `virtio-win` CD Drive and run **`virtio-win-gt-x64`**.

![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/18.png)

---

#### Step 19 — Complete VirtIO Setup

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/19.png)

---

#### Step 20 — Verify DHCP Network Assignment

```powershell
ipconfig
```

Expected lease: `10.22.7.3`

![20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/20.png)

---

#### Step 21 — Access System Properties

**Control Panel → System and Security → System → Change settings**

![21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/21.png)

---

#### Step 22 — Update Computer Description

Set description to `DC`, then click **Change...** to modify the hostname.

![22](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/22.png)

---

#### Step 23 — Rename Computer to DC

![23](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/23.png)

---

### Part E — Routing & Internet Verification

#### Step 24 — Identify Connectivity Issue

Yellow warning triangle on the network icon — outbound route to WAN is not yet defined.

![24](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/24.png)

---

#### Step 25 — Add WAN Gateway in pfSense

**System → Routing → Gateways → Add**

![25](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/25.png)

---

#### Step 26 — Configure External Gateway

![26](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/26.png)

---

#### Step 27 — Update SERVERS Firewall Rules

**Firewall → Rules → SERVERS**

| Rule | Protocol | Source | Destination | Port | Purpose |
|------|----------|--------|-------------|:----:|---------|
| DNS | IPv4 UDP | SERVERS net | This Firewall | `53` | DNS queries to pfSense |
| Outbound | IPv4 Any | SERVERS net | Any | Any | Internet access |

![27](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/27.png)

---

#### Step 28 — Restart Routing Services

**Status → Services** — restart `dpinger` and `kea-dhcp4`.

![28](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/28.png)

---

#### Step 29 — Ping Verification

```powershell
ping 8.8.8.8
```

`DC → SERVERS VLAN → pfSense → WAN → Internet` ✅

![29](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/29.png)

---

#### Step 30 — Browser Verification

Navigate to `google.com` in Edge — full connectivity confirmed.

![30](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/05-domain-controller/30.png)

---

### VM Summary

| Component | Bridge | IP | Zone |
|:---------:|:------:|:--:|:----:|
| Windows Server DC | vmbr2 | `10.22.7.3` (DHCP) | SERVERS |
| pfSense — SERVERS | vmbr2 | `10.22.7.1/24` | SERVERS gateway |

---

### ✅ Phase Checklist

- [ ] Windows Server ISO and VirtIO ISO uploaded to Proxmox local storage
- [ ] SERVERS DHCP pool enabled — range `10.22.7.3` to `10.22.7.50`
- [ ] DHCP backend switched from ISC to **Kea DHCP**
- [ ] DNS server `10.22.7.1` configured on SERVERS DHCP scope
- [ ] VM 101 created — `2 cores · 8 GB RAM · 60 GB VirtIO Block disk`
- [ ] Network bridge set to `vmbr2` (SERVERS), model VirtIO, firewall unchecked
- [ ] VirtIO drivers ISO attached as second CD/DVD drive before first boot
- [ ] Windows Server 2019 **Desktop Experience** installed via Custom install
- [ ] Local Administrator password set
- [ ] VirtIO drivers installed — `virtio-win-gt-x64`
- [ ] DHCP lease confirmed — `10.22.7.3` via `ipconfig`
- [ ] Computer renamed to `DC` and restarted
- [ ] WAN gateway added in pfSense — **System → Routing → Gateways**
- [ ] SERVERS firewall rules created — DNS (`UDP/53`) + outbound (`Any`)
- [ ] Routing services restarted — `dpinger` and `kea-dhcp4`
- [ ] Ping to `8.8.8.8` successful from DC
- [ ] Browser connectivity confirmed — `google.com` loads in Edge

<div align="center"><br>

**🟢 Phase Complete**

`[04] Advanced Firewall Routing` ◄── **`[05] Domain Controller Provisioning`** ──► `[06] Active Directory Domain Installation`

<br></div>

</details>

---

<details>
<summary><b>📗 Phase 2 · [06] — Active Directory Domain Installation & Configuration</b></summary>
<a name="phase-2--06"></a>

<br>

> **Scope:** Installing the **Active Directory Domain Services** and **DNS Server** roles on the Domain Controller, promoting the server to the forest root of `mursad.local`, creating domain user accounts, provisioning a Windows 10 Pro workstation, and joining it to the new domain.

---

### Overview

```text
Proxmox Node: mursad
├── VM 101  —  DC  (Windows Server 2019)
│       ├── Roles: AD DS · DNS Server
│       ├── Forest Root: mursad.local
│       ├── DC IP: 10.22.7.3  (static, post-promotion)
│       └── Users: a.alaradi@mursad.local
│
└── VM 102  —  ITWS  (Windows 10 Pro)
        ├── Network: vmbr4 (IT Workstation)
        ├── DHCP: 10.22.1.x
        └── Domain-joined: mursad.local
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | Role Installation | Add AD DS and DNS Server roles via Server Manager |
| **B** | Domain Promotion | Promote the server to a new forest root Domain Controller |
| **C** | User Provisioning | Create domain user accounts in AD Users and Computers |
| **D** | Workstation Setup | Provision a Windows 10 Pro VM with VirtIO drivers |
| **E** | Network Preparation | Configure pfSense DHCP, NAT, and firewall rules for the IT zone |
| **F** | Domain Join | Configure DNS on the workstation and join `mursad.local` |

---

### Part A — Role Installation

#### Step 1 — Open Add Roles and Features

![Image 1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/1.png)

Open **Server Manager → Manage → Add Roles and Features**.

---

#### Step 2 — Before You Begin

![Image 2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/2.png)

Click **Next**.

---

#### Step 3 — Select Installation Type

![Image 3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/3.png)

Select **"Role-based or feature-based installation"** and click **Next**.

---

#### Step 4 — Select Destination Server

![Image 4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/4.png)

Verify the local server — hostname `DC`, IP `10.22.7.3`. Click **Next**.

---

#### Step 5 — Select Server Roles

![Image 5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/5.png)

Check both:
- **Active Directory Domain Services**
- **DNS Server**

Accept any prompts for required features. Click **Next**.

---

#### Step 6 — Select Features

![Image 6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/6.png)

Optionally add **Telnet Client** for network troubleshooting. Click **Next**.

---

#### Step 7 — Confirm Installation

![Image 7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/7.png)

Review selections and click **Install**.

---

#### Step 8 — Installation Complete

![Image 8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/8.png)

Role binaries installed. The server must now be **promoted** to become an actual Domain Controller.

---

### Part B — Domain Promotion

#### Step 9 — Promote the Server

![Image 9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/9.png)

In **Server Manager**, click the **yellow warning triangle** → **"Promote this server to a domain controller"**.

---

#### Step 10 — Add a New Forest

![Image 10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/10.png)

Select **"Add a new forest"**. Set Root domain name:

```
mursad.local
```

---

#### Step 11 — DSRM Password

![Image 11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/11.png)

Set a strong **Directory Services Restore Mode (DSRM)** password and store it securely. Click **Next**.

---

#### Step 12 — Verify NetBIOS Name

![Image 12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/12.png)

Confirm NetBIOS name auto-populated as `MURSAD`. Click **Next**.

---

#### Step 13 — AD DS Database Paths

![Image 13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/13.png)

Leave default paths for the AD DS database, log files, and SYSVOL. Click **Next**.

---

#### Step 14 — Prerequisites Check

![Image 14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/14.png)

Once the green checkmark appears, click **Install**.

---

#### Step 15 — Reboot to Apply DC Configuration

![Image 15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/15.png)

The system will sign out and **automatically restart** to finalize the Domain Controller configuration.

---

### Part C — User Provisioning

#### Step 16 — Provision the IT Workstation VM

![Image 16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/16.png)

While the DC reboots, create a new Windows 10 VM in Proxmox for the IT department.

---

#### Step 17 — Install Windows 10 Pro

![Image 17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/17.png)

> ⚠️ Always select **Windows 10 Pro** — the Home edition cannot join a domain.

---

#### Step 18 — Open Active Directory Users and Computers

![Image 18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/18.png)

**Server Manager → Tools → Active Directory Users and Computers**

---

#### Step 19 — Create a New User

![Image 19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/19.png)

Right-click the **Users** container → **New → User**.

---

#### Step 20 — Enter User Details

![Image 20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/20.png)

Set User logon name: `a.alaradi@mursad.local`. Click **Next**.

---

#### Step 21 — Set User Password

![Image 21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/21.png)

Set a password. **"Password never expires"** selected for lab use.

---

#### Step 22 — Confirm User Settings

![Image 22](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/22.png)

Verify all details and click **Finish**.

---

#### Step 23 — User Created Successfully

![Image 23](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/23.png)

The user `a.alaradi` is now visible in the AD Users and Computers directory.

---

### Part D — Workstation Driver Setup

#### Step 24 — Mount VirtIO Drivers on the Workstation

![Image 24](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/24.png)

Shut down the Windows 10 VM. In Proxmox Hardware settings, add the `virtio-win` ISO to CD/DVD.

---

#### Step 25 — Install VirtIO Drivers

![Image 25](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/25.png)

Run the VirtIO installer and click **Finish**. Restart the VM.

---

### Part E — Network Preparation

#### Step 26 — Enable DHCP for the IT Workstation Interface

![Image 26](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/26.png)

**pfSense → Services → DHCP Server → IT**

| Field | Value |
|-------|-------|
| Address Pool From | `10.22.1.2` |
| Address Pool To | `10.22.1.100` |
| DNS Server | `10.22.1.1` |

---

#### Step 27 — Add NAT Mapping for IT Subnet

![Image 27](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/27.png)

**Firewall → NAT → Outbound** — add manual mapping for `10.22.1.0/24`.

---

#### Step 28 — Add Firewall Rules for IT Workstation

![Image 28](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/28.png)

**Firewall → Rules → IT** — add pass rules for outbound traffic.

---

#### Step 29 — Verify Connectivity from Workstation

![Image 29](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/29.png)

```cmd
ping 10.22.7.3
ping 10.22.1.1
```

---

#### Step 30 — Configure DNS on the Workstation

![Image 30](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/30.png)

**Network → Ethernet → IPv4 Properties**

| Field | Value |
|-------|-------|
| Preferred DNS | `10.22.7.3` *(Domain Controller)* |
| Alternate DNS | `10.22.1.1` *(pfSense gateway)* |

---

### Part F — Domain Join

#### Step 31 — Initiate Domain Join

![Image 31](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/31.png)

**Settings → System → About → Advanced system settings → Computer Name → Change...**

Select **Domain** and enter `mursad.local`.

---

#### Step 32 — Authenticate with Domain Admin Credentials

![Image 32](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/32.png)

Enter Domain Admin credentials when prompted.

---

#### Step 33 — Domain Join Successful

![Image 33](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/06-active-directory/33.png)

```
Welcome to the mursad.local domain.
```

Restart the workstation to apply domain policies.

---

### VM Summary

| VM | OS | Bridge | IP | Role |
|:--:|:--:|:------:|:--:|------|
| VM 101 — DC | Windows Server 2019 | vmbr2 | `10.22.7.3` | AD DS · DNS · Forest Root |
| VM 102 — ITWS | Windows 10 Pro | vmbr4 | `10.22.1.x` (DHCP) | Domain-joined IT workstation |

---

### ✅ Phase Checklist

- [ ] AD DS and DNS Server roles installed via Server Manager
- [ ] Server promoted to Domain Controller — new forest `mursad.local`
- [ ] DSRM password set and stored securely
- [ ] NetBIOS name confirmed as `MURSAD`
- [ ] DC restarted and AD DS fully operational
- [ ] Domain user `a.alaradi@mursad.local` created in AD Users and Computers
- [ ] Windows 10 Pro VM provisioned in Proxmox on `vmbr4` (IT)
- [ ] VirtIO drivers installed on the workstation VM
- [ ] pfSense DHCP enabled on IT — pool `10.22.1.2`–`10.22.1.100`
- [ ] Manual NAT mapping added for `10.22.1.0/24`
- [ ] Firewall pass rules added for IT interface
- [ ] Workstation can ping DC (`10.22.7.3`) and gateway (`10.22.1.1`)
- [ ] Workstation DNS set to `10.22.7.3` (primary) · `10.22.1.1` (alternate)
- [ ] Workstation successfully joined to `mursad.local`
- [ ] Workstation restarted to apply domain policies

<div align="center"><br>

**🟢 Phase Complete**

`[05] Domain Controller Provisioning` ◄── **`[06] Active Directory Installation`** ──► `[07] DMZ Architecture Setup`

<br></div>

</details>

---

<details>
<summary><b>📙 Phase 2 · [07] — DMZ Architecture Setup</b></summary>
<a name="phase-2--07"></a>

<br>

> **Scope:** Organizing the Active Directory hierarchy with Organizational Units (OUs), provisioning the public-facing **DMZ web server** (`DMZ-SRV-01`), securing the pfSense management port, configuring **Destination NAT** (Port Forwarding) to expose web services through the WAN, and staging a vulnerable web application (XAMPP) for SOC telemetry and future attack simulation.

---

### Overview

```text
Proxmox Node: mursad
├── VM 101  —  DC  (Windows Server 2019)
│       └── Active Directory  ·  mursad.local
│           ├── OU: Admin-Department     ← Domain Admins
│           ├── OU: HR-Department
│           ├── OU: Operations-Department
│           └── OU: Accounting-Department
│
└── VM 103  —  DMZ-SRV-01  (Windows Server)
        ├── Network:  vmbr3  (DMZ)
        ├── IP:       192.168.50.10  (Static)
        └── Role:     Public Web Server  (Apache / XAMPP)

pfSense WebConfigurator
└── Edge Routing
        ├── WebGUI moved to TCP 4005   ← avoids conflict with HTTP forwarding
        └── Port Forwarding (WAN → DMZ)
                ├── TCP 80  →  192.168.50.10
                └── TCP 443 →  192.168.50.10
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | Active Directory Organization | Create Organizational Units and elevate domain user privileges |
| **B** | DMZ Server Provisioning | Create VM 103, verify DMZ networking, and stage web services |
| **C** | pfSense Port Forwarding | Secure the WebGUI port and expose the DMZ via Destination NAT |
| **D** | Web Application Staging | Initialize Apache and deploy the custom `index.html` payload |
| **E** | Domain Integration | Join `DMZ-SRV-01` to `mursad.local` and log in with a domain account |

---

### Part A — Active Directory Organization

#### Step 1 — Create Organizational Units

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/1.png)

Open **Server Manager → Tools → Active Directory Users and Computers**. Right-click the root domain `mursad.local`, select **New → Organizational Unit**.

---

#### Step 2 — Name the First OU

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/2.png)

Name the new OU (e.g., `Admin-Department`). Leave **"Protect container from accidental deletion"** checked.

---

#### Step 3 — Build the Full Department Hierarchy

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/3.png)

| OU Name | Purpose |
|---------|---------|
| `Admin-Department` | Tier 0 administrators · Domain Admin accounts |
| `HR-Department` | Human Resources endpoints and users |
| `Operations-Department` | Operations team endpoints and users |
| `Accounting-Department` | Finance and accounting endpoints and users |

---

#### Step 4 — Configure User Properties

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/4.png)

| Field | Value |
|-------|-------|
| Description | `Tier 0 Domain Administrator` |
| Office | `Security Operations Center (SOC)` |
| Email | `admin.ali@mursad.local` |

---

#### Step 5 — Assign Domain Admin Privileges

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/5.png)

**Member Of** tab → **Add** → type `domain admins` → **Check Names** → **OK** → **Apply**.

> ⚠️ In production, Domain Admin privileges should be granted sparingly and audited regularly.

---

### Part B — DMZ Server Provisioning

#### Step 6 — Create the DMZ Virtual Machine

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/6.png)

| Component | Value |
|-----------|-------|
| VM ID | `103` |
| Name | `DMZ-SRV-01` |
| Memory | `4096 MiB (4 GB)` |
| CPU Cores | `2` (Type: `host`) |
| Hard Disk | `60 GB` (VirtIO Block) |
| Network Bridge | `vmbr3` (DMZ) · Model: VirtIO |

---

#### Step 7 — Verify DMZ Network Connectivity

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/7.png)

```powershell
ipconfig
```

| Field | Expected Value |
|-------|---------------|
| IPv4 Address | `192.168.50.10` |
| Default Gateway | `192.168.50.1` |

---

#### Step 8 — Stage Web Services

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/8.png)

Download **XAMPP for Windows** to `DMZ-SRV-01`.

---

### Part C — pfSense Port Forwarding & WebGUI Security

> ⚠️ **Critical:** pfSense defaults to TCP **80/443** for its WebGUI. Move the port first before creating forwarding rules.

#### Step 9 — Secure the pfSense WebGUI Port

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/9.png)

**System → Advanced → Admin Access**

| Setting | Value |
|---------|-------|
| TCP Port | `4005` |
| Disable webConfigurator redirect rule | ✅ Checked |

---

#### Step 10 — Configure Destination NAT (Port Forwarding)

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/10.png)

**Firewall → NAT → Port Forward** — WAN interface:

| Rule | Protocol | WAN Port | Redirect Target | Redirect Port |
|------|----------|----------|-----------------|---------------|
| HTTP | TCP | `80` | `192.168.50.10` | `80` |
| HTTPS | TCP | `443` | `192.168.50.10` | `443` |

---

### Part D — Web Application Staging

#### Step 11 — Initialize Apache Server

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/11.png)

**XAMPP Control Panel → Start → Apache**

```
HTTP  →  Port 80   ✅
HTTPS →  Port 443  ✅
```

---

#### Step 12 — Deploy the Web Application

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/12.png)

Navigate to `C:\xampp\htdocs\` → clear defaults → create `index.html`.

---

#### Step 13 — Verify Local Web Access

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/13.png)

```
http://192.168.50.10/
```

---

### Part E — Domain Integration

#### Step 14 — Join the Active Directory Domain

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/14.png)

| Field | Value |
|-------|-------|
| Computer Name | `DMZ-SRV-01` |
| Member of | ◉ Domain: `mursad.local` |

---

#### Step 15 — Log In with Domain Credentials

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/07-dmz-architecture/15.png)

```
Username:  a.alaradi
Domain:    MURSAD
```

---

### VM Summary

| VM | OS | Bridge | IP | Role |
|:--:|:--:|:------:|:--:|------|
| VM 101 — DC | Windows Server 2019 | vmbr2 | `10.22.7.3` | AD DS · DNS · Forest Root |
| VM 103 — DMZ-SRV-01 | Windows Server | vmbr3 | `192.168.50.10` | Public-Facing DMZ Web Server |

---

### ✅ Phase Checklist

- [ ] Departmental OUs created — `Admin`, `HR`, `Operations`, `Accounting`
- [ ] Primary user account moved to `Admin-Department` OU
- [ ] User properties populated — Description, Office, Email
- [ ] User granted `Domain Admins` group membership
- [ ] VM 103 (`DMZ-SRV-01`) created — `4 GB RAM · 2 cores · 60 GB · vmbr3`
- [ ] DMZ IP verified — `192.168.50.10`, gateway `192.168.50.1`
- [ ] XAMPP installer staged on the DMZ server
- [ ] pfSense Admin Access port changed to `TCP 4005`
- [ ] pfSense WebGUI redirect rule disabled
- [ ] Port Forwarding rule — WAN `TCP 80` → `192.168.50.10:80`
- [ ] Port Forwarding rule — WAN `TCP 443` → `192.168.50.10:443`
- [ ] Apache started in XAMPP — status: green
- [ ] Custom `index.html` deployed to `C:\xampp\htdocs\`
- [ ] Web application verified at `http://192.168.50.10/`
- [ ] `DMZ-SRV-01` joined to `mursad.local` and logged in via domain account

<div align="center"><br>

**🟢 Phase Complete**

`[06] Active Directory Installation` ◄── **`[07] DMZ Architecture Setup`** ──► `[08] Suricata IDS/IPS Configuration`

<br></div>

</details>

---

<details>
<summary><b>📘 Phase 2 · [08] — Suricata IDS/IPS Configuration</b></summary>
<a name="phase-2--08"></a>

<br>

> **Scope:** Installing the Suricata Intrusion Detection and Prevention System (IDS/IPS) package on pfSense, configuring global rule providers (ETOpen, Feodo, ABUSE.ch), opening the DMZ firewall, launching Red Team attack simulations via Kali Linux, and validating the resulting Blue Team SOC telemetry.

---

### Overview

```text
pfSense WebConfigurator
└── Services: Suricata
        ├── Engine: Inline Mode (IDS/IPS)
        ├── Rule Providers:
        │    ├── ETOpen Emerging Threats
        │    ├── Feodo Tracker Botnet C2
        │    └── ABUSE.ch SSL Blacklist
        └── Management: Alerts, Blocks, Pass Lists, Suppress Lists

Attack Path Simulation
└── Kali Linux (WAN)  ──►  pfSense (Suricata)  ──►  DMZ-SRV-01 (Apache)
      │                       │
      ├── 1. Nmap Scan        ├── Logs: Web Application Attack
      └── 2. SQLMap Inject    └── Logs: SQL Injection Attempt
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | Package Installation | Download and install the Suricata package on pfSense |
| **B** | Global Settings & Rules | Enable threat intelligence feeds and update signatures |
| **C** | Management Interfaces | Overview of alerts, blocked hosts, pass lists, and suppression |
| **D** | Interface Configuration | Attach Suricata to the DMZ and assign rule categories |
| **E** | Target Preparation | Configure Windows Defender Firewall to allow inbound web traffic |
| **F** | Red Team Simulation | Launch Nmap and SQLMap attacks from Kali Linux |
| **G** | Blue Team Validation | Review and analyze the generated Suricata alerts |

---

### Part A — Package Installation

#### Step 1 — Introduction to IDS/IPS

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/1.png)

---

#### Step 2 — Install Suricata Package

Navigate to **System → Package Manager → Available Packages**. Search for `suricata` and click **Install**.

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/2.png)

---

#### Step 3 — Confirm Installation

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/3.png)

Wait for: `TASK OK`

> **Note:** Suricata is preferred over Snort for this lab due to multithreading capabilities and native integration with the emerging threats landscape.

---

### Part B — Global Settings & Rules Configuration

#### Step 4 — Configure Global Settings & Threat Feeds

**Services → Suricata → Global Settings** — enable:

- ✅ **Install ETOpen Emerging Threats rules**
- ✅ **Install Feodo Tracker Botnet C2 IP rules**
- ✅ **Install ABUSE.ch SSL Blacklist rules**

Set **Update Interval** to `1 DAY`. Click **Save**.

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/4.png)

---

#### Step 5 — Force Rule Updates

**Services → Suricata → Updates → Update** — force immediate download of all enabled rule sets.

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/5.png)

---

### Part C — Management Interfaces Overview

#### Step 6 — Alerts Interface

**Services → Suricata → Alerts** — primary dashboard for triggered rules, source/destination IPs, and payload details.

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/6.png)

---

#### Step 7 — Blocked Hosts Interface

**Services → Suricata → Blocked Hosts** — in IPS mode, malicious IPs are actively dropped and listed here.

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/7.png)

---

#### Step 8 — Files Interface

**Services → Suricata → Files** — file extraction and carving for malware analysis.

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/8.png)

---

#### Step 9 — Pass Lists (Whitelisting)

**Services → Suricata → Pass Lists** — whitelist trusted IPs to prevent false positives.

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/9.png)

---

#### Step 10 — Suppress Lists (Silencing Rules)

**Services → Suricata → Suppress Lists** — silence noisy rules without disabling them. Critical for reducing SOC alert fatigue.

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/10.png)

---

#### Step 11 — Logs View Interface

**Services → Suricata → Logs View** — raw Suricata engine logs from the pfSense web interface.

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/11.png)

---

#### Step 12 — Logs Management

**Services → Suricata → Logs Mgmt** — configure log rotation to prevent disk exhaustion.

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/12.png)

---

#### Step 13 — SID Management

**Services → Suricata → SID Mgmt** — automatically enable/disable/modify specific detection rules via configuration files.

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/13.png)

---

### Part D — Interface Configuration & Rule Categories

#### Step 14 — Configure DMZ Interface Settings

**Services → Suricata → Interfaces → Add**

| Field | Value |
|-------|-------|
| Interface | DMZ (`vtnet3`) |
| Enable | ✅ Enable Suricata inspection |

Running in **IDS mode** initially — alerts only, no blocking.

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/14.png)

---

#### Step 15 — IPS Mode Configuration (Optional)

To elevate to IPS (active blocking):

| Setting | Value |
|---------|-------|
| Block Offenders | ✅ Checked |
| IPS Mode | Inline Mode |
| Run Mode | Workers |

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/15.png)

---

#### Step 16 — Interface Status & Control

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/16.png)

Click the **Pencil** icon to continue configuring rules for the DMZ interface.

---

#### Step 17 — Select DMZ Rule Categories

**Categories** tab → Click **Select All** → **Save**.

> ⚠️ More rules = higher CPU load. For production, enable only categories relevant to running services.

![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/17.png)

---

#### Step 18 — View and Manage Individual Rules

**Rules** tab — select a category from the dropdown to view and manage individual SIDs.

![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/18.png)

---

#### Step 19 — Start Suricata on the DMZ

Return to the **Interfaces** tab. Click the **Play** button for the DMZ interface. The icon turns green — Suricata is now actively inspecting traffic.

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/19.png)

---

### Part E — Target Preparation

#### Step 20 — Create Inbound Firewall Rule

On **DMZ-SRV-01**, open **Windows Defender Firewall with Advanced Security → Inbound Rules → New Rule... → Port**.

![20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/20.png)

---

#### Step 21 — Specify Protocols and Ports

Select **TCP** · Specific local ports: `80, 443`.

![21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/21.png)

---

#### Step 22 — Rule Action

Select **Allow the connection**.

![22](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/22.png)

---

#### Step 23 — Apply to Profiles

Check all three: **Domain**, **Private**, **Public**.

![23](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/23.png)

---

#### Step 24 — Name the Rule

Name: `Allow Web Traffic (HTTP/HTTPS)` → **Finish**.

![24](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/24.png)

---

#### Step 25 — External Access Validation

Navigate to `http://192.168.140.130/` from the Host machine. The pfSense Destination NAT correctly forwards the request to the DMZ — the custom Mursad landing page loads.

![25](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/25.png)

---

### Part F — Red Team Adversary Simulation

#### Step 26 — Kali Linux Verification

Verify Kali Linux can access `http://192.168.140.130/`. Connectivity confirmed — proceed with attacks.

![26](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/26.png)

---

#### Step 27 — Execute SQL Injection Attack (SQLMap)

```bash
sqlmap -u 'http://192.168.140.130/index.html?id=' --batch --random-agent
```

![27](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/27.png)

---

#### Step 28 — Execute Network Reconnaissance (Nmap)

```bash
sudo nmap -sS -sV -sC -O -p- -T4 -Pn --reason 192.168.140.130
```

![28](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/28.png)

---

### Part G — Blue Team Telemetry Validation

#### Step 29 — Analyze Nmap Alerts

**Services → Suricata → Alerts** — multiple **Web Application Attack** alerts generated by the Nmap scan, identifying Kali Linux as the source.

![29](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/29.png)

---

#### Step 30 — Analyze SQL Injection Alerts

Numerous **SQL ATTEMPTS** logs visible, including `ET SCAN Sqlmap SQL Injection Scan` rule triggers. The IDS is successfully detecting live hostile traffic crossing the firewall boundary.

![30](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-2/08-suricata-ids-ips/30.png)

---

### VM Summary

| VM | Network Zone | IP Address | Role in Simulation |
|:--:|:------------:|:----------:|-------------------|
| pfSense | WAN / DMZ | `192.168.140.130` (WAN) | Inline Suricata IDS Engine |
| DMZ-SRV-01 | DMZ | `192.168.50.10` | Vulnerable Web Server Target |
| Kali Linux | WAN | DHCP | External Red Team Attacker |

---

### ✅ Phase Checklist

- [ ] Suricata package installed via pfSense Package Manager
- [ ] ETOpen, Feodo Tracker, and ABUSE.ch rules enabled
- [ ] Rule signatures successfully downloaded and updated
- [ ] Suricata mapped to the DMZ interface in IDS Mode
- [ ] "Select All" rule categories applied to the DMZ interface
- [ ] Suricata service actively running on the DMZ (Green Checkmark)
- [ ] Windows Defender Firewall rule created for TCP 80/443 on DMZ-SRV-01
- [ ] External web connectivity verified from Host and Kali Linux
- [ ] SQL Injection simulation executed via `sqlmap`
- [ ] Network reconnaissance simulation executed via `nmap`
- [ ] Web Application Attack alerts verified in Suricata logs
- [ ] SQL Injection Attempt alerts verified in Suricata logs

<div align="center"><br>

**🟢 Phase 2 Complete**

`[07] DMZ Architecture Setup` ◄── **`[08] Suricata IDS/IPS Configuration`** ──► `[09] LAN/DMZ Traffic Isolation`

<br></div>

</details>

---

<details>
<summary><b>📙 Phase 3 · [09] — LAN/DMZ Traffic Isolation & Secure Local DNS Mapping</b></summary>
<a name="phase-3--09"></a>

<br>

> **Scope:** Hardening the network perimeter by implementing strict inter-VLAN isolation rules, configuring rate-limiting to mitigate DoS attacks, and establishing secure local DNS resolution for the DMZ environment.

---

### Overview

```text
pfSense WebConfigurator
├── Traffic Shaper
│   └── Limiters: Rate Limiting & Anti-DDoS
├── Firewall Rules: IT
│   └── Block Rule: IT ──✗──► DMZ subnets
└── Firewall Rules: DMZ
    └── Block Rule: DMZ ──✗──► Internal Subnets (HR, SERVERS, IT, OPs)

Local DNS Resolution
└── Static Entry: alialaradi.bh ──► 192.168.140.130  (WAN VIP → DMZ)
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | DMZ Security Principles | Core architectural concepts governing perimeter hardening |
| **B** | Traffic Shaping | Configure pfSense limiters to mitigate resource exhaustion |
| **C** | Connectivity Baseline | Verify initial routing state before applying restrictive policies |
| **D** | Traffic Isolation | Implement strict deny rules between internal VLANs and the DMZ |
| **E** | DNS Mapping | Map a custom local domain for professional web access |

---

### Part A — DMZ Hardening & Security Best Practices

Before technical implementation, the following architectural principles govern the Mursad SOC perimeter:

#### Restrict DMZ-to-Internal Communication
Apply a **"Deny All"** policy for any traffic originating from the DMZ toward the internal network. Exceptions must be explicit — define exact protocols and ports (e.g., `TCP/3306` for MySQL) rather than broad `Any` rules.

#### Inbound Internet Traffic Control
Only specific services are exposed. Port `443` (HTTPS) is the primary inbound channel. Port `80` (HTTP) is kept open solely to redirect clients to `443`, ensuring all sessions remain encrypted.

#### Restrict Outbound Traffic (Egress Filtering)
Most DMZ assets have no reason to initiate outbound connections. Block all egress by default — allow only DNS resolution, NTP sync, and specific update repository endpoints.

#### IDS/IPS and Geo-IP Blocking
As a public-facing zone, the DMZ is a primary attack surface. Suricata must remain active to detect and drop malicious payloads inline. Geo-IP blocking restricts inbound traffic from high-risk regions while preserving access for authorized regions (e.g., Bahrain).

#### Least Privilege Service Execution
Never run web services (Apache, IIS) under `Administrator` or `root`. Isolate management ports (RDP, SSH, WebGUI) to a dedicated internal VLAN — never expose them to the WAN.

#### Rate Limiting & DDoS Mitigation
Prevent resource exhaustion via pfSense connection limits and Traffic Shaper Limiters. Tools like Fail2Ban or a WAF can automatically block IPs exceeding failed request thresholds.

#### SIEM Integration
Local logs are insufficient at scale. All perimeter telemetry must be shipped to Wazuh for centralized correlation, dashboard visibility, and coordinated attack detection.

---

### Part B — Traffic Shaping & DoS Mitigation

#### Step 1 — Configure Traffic Limiters

To protect the DMZ web server from connection exhaustion and basic DDoS attempts, we utilize the **pfSense Traffic Shaper** to cap bandwidth and connection state rates per source IP.

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/1.png)

Navigate to **Firewall → Traffic Shaper → Limiters** and define the rate-limit masks.

> This ensures no single abusive IP can overwhelm the Apache service by exhausting available connection slots.

---

### Part C — Connectivity Baseline

Before applying isolation rules, the current routing state must be verified to confirm what traffic is permitted and what will be blocked after lockdown.

#### Step 2 — Verify DMZ → Internal Connectivity

Initiate a ping from `DMZ-SRV-01` to the IT Workstation subnet.

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/2.png)

```powershell
# Executed from DMZ-SRV-01
ping 10.22.1.2
```

> ✅ **Result:** Successful replies confirm the DMZ can currently reach the internal IT subnet — this bidirectional access will be severed after isolation rules are applied.

---

#### Step 3 — Verify Internal → DMZ Connectivity

Perform the reverse test from the IT Workstation to the DMZ server.

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/3.png)

```powershell
# Executed from IT-Workstation
ping 192.168.50.10
```

> ✅ **Result:** Successful replies confirm full bidirectional routing is active. Baseline documented — proceed to lockdown.

---

### Part D — Traffic Isolation Implementation

#### Step 4 — Navigate to IT Firewall Rules

Navigate to **Firewall → Rules → IT** and click **Add ↑** to insert a new rule at the **top** of the list.

> ⚠️ **Rule order is critical.** pfSense evaluates rules top-to-bottom — the block rule must appear above any existing pass rules to take effect.

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/4.png)

---

#### Step 5 — Configure IT → DMZ Block Rule

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/5.png)

| Field | Value |
|-------|-------|
| Action | **Block** *(drops packets silently — no RST sent)* |
| Interface | `IT` |
| Address Family | IPv4 |
| Protocol | Any |
| Source | `IT` subnets |
| Destination | `DMZ` subnets |
| Description | `Block IT → DMZ — Enforce Zone Isolation` |

Click **Save** → **Apply Changes**.

---

#### Step 6 — Validate IT Isolation

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/6.png)

```powershell
# Executed from IT-Workstation
ping 192.168.50.10
```

> 🔴 **Result:** `Request timed out.` The firewall is silently dropping all traffic from the IT zone to the DMZ. Isolation confirmed.

---

#### Step 7 — Harden DMZ Outbound Rules

The most critical step in DMZ architecture is preventing a **compromised DMZ asset from pivoting into the internal network**. A breached web server must never be able to reach domain controllers, workstations, or the SIEM.

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/7.png)

Navigate to **Firewall → Rules → DMZ** and click **Add ↑** to create a block rule at the top:

| Field | Value |
|-------|-------|
| Action | **Block** |
| Interface | `DMZ` |
| Address Family | IPv4 |
| Protocol | Any |
| Source | `DMZ` subnets |
| Destination | Internal subnets *(HR · SERVERS · IT · OPs)* |
| Description | `Block DMZ → Internal — Prevent Lateral Movement` |

Click **Save** → **Apply Changes**.

> ⚠️ In a full production deployment, this rule is replicated for each internal VLAN to form an explicit deny matrix. No implicit trust should exist between the DMZ and any internal segment.

---

### Part E — Secure Local DNS Mapping

#### Step 8 — Static DNS Configuration

To provide a professional, enterprise-grade access point — and to make Red Team targeting more realistic — we map a custom domain to the WAN IP that forwards to the DMZ web server.

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/8.png)

On the local router (or pfSense **Services → DNS Resolver → Host Overrides**), add a static DNS entry:

| Field | Value |
|-------|-------|
| Host | `alialaradi` |
| Domain | `bh` |
| IP Address | `192.168.140.130` *(WAN VIP → NAT → DMZ)* |
| Description | `Project Mursad SOC Lab — Public DNS Mapping` |

Click **Save** → **Apply Changes**.

---

#### Step 9 — Final Web Validation

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/09-traffic-isolation/9.png)

Open a browser and navigate to the custom domain:

```
http://alialaradi.bh
```

> ✅ **Result:** The Project Mursad deployment page loads successfully via the FQDN — confirming the full chain: `DNS → WAN IP → pfSense NAT → DMZ web server`.

---

### Zone Isolation Summary

```
Before Lockdown:
  IT Workstation  ◄──────────►  DMZ-SRV-01       ✅ (open)
  DMZ-SRV-01      ◄──────────►  IT / HR / SERVERS  ✅ (open)

After Lockdown:
  IT Workstation  ─────✗──────►  DMZ-SRV-01       🔴 (blocked)
  DMZ-SRV-01      ─────✗──────►  IT / HR / SERVERS  🔴 (blocked)
  Internet        ────────────►  DMZ-SRV-01       ✅ (port 80/443 only)
```

---

### ✅ Phase Checklist

- [ ] DMZ architectural hardening principles reviewed and documented
- [ ] Traffic Limiters configured in pfSense — **Firewall → Traffic Shaper → Limiters**
- [ ] Baseline bidirectional connectivity verified: IT ↔ DMZ both responding
- [ ] Block rule created on `IT` — source: IT subnets · destination: DMZ subnets
- [ ] `ping 192.168.50.10` from IT Workstation returns **Request timed out** ✅
- [ ] Block rule created on `DMZ` — source: DMZ subnets · destination: all internal subnets
- [ ] Rules applied in correct top-down order on both interfaces
- [ ] Static DNS host override created — `alialaradi.bh` → `192.168.140.130`
- [ ] Web application verified via FQDN — `http://alialaradi.bh` loads correctly

<div align="center"><br>

**🟢 Phase 3 · [09] Complete**

`[08] Suricata IDS/IPS Configuration` ◄── **`[09] LAN/DMZ Traffic Isolation`** ──► `[10] Wazuh SIEM Installation`

<br></div>

</details>

---

<details>
<summary><b>📘 Phase 3 · [10] — Wazuh SIEM Installation · Syslog Ingestion · Agent Rollout</b></summary>
<a name="phase-3--10"></a>

<br>

> **Scope:** Provisioning the Wazuh SIEM server within the SERVERS zone, executing the All-in-One installation script, establishing a syslog pipeline from the pfSense firewall, and deploying the Wazuh endpoint agent to establish centralized SOC telemetry.

---

### Overview

```text
Proxmox Node: mursad
└── VM 104  —  Wazuh  (Ubuntu Server 22.04 LTS)
        ├── Network:  vmbr2 (SERVERS) · vmbr0 (WAN — temp, install only)
        ├── IP:       10.22.7.67  (Static — post-install)
        └── Role:     Wazuh Manager · Indexer · Dashboard

Telemetry Sources
├── pfSense Firewall  ──►  Syslog UDP/514      ──►  Wazuh Manager
└── Domain Controller ──►  Wazuh Agent TCP/1514 ──►  Wazuh Manager
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Provisioning & OS Setup | Deploy Ubuntu Server and configure network interfaces |
| **B** | Wazuh Installation | Execute the unattended All-in-One installation script |
| **C** | Initial Configuration | Set up groups, syslog listeners, and static IP assignment |
| **D** | pfSense Syslog Integration | Configure remote logging and verify ingestion pipeline |
| **E** | Endpoint Agent Rollout | Deploy and register the Wazuh agent on the Domain Controller |

---

### Part A — VM Provisioning & OS Setup

#### Step 1 — Create the Wazuh Virtual Machine

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/1.png)

Create **VM 104** in Proxmox. Wazuh requires substantial resources to run the Manager, Indexer, and Dashboard concurrently:

| Component | Value |
|-----------|-------|
| VM ID | `104` |
| Name | `WAZUH` |
| Memory | `8192 MiB (8 GB)` |
| CPU Cores | `2` (Type: `host`) |
| Hard Disk | `100 GB` (VirtIO SCSI) |
| Network 1 | `vmbr2` — SERVERS zone |
| Network 2 | `vmbr0` — WAN *(temporary, install only)* |

> ℹ️ The WAN interface (`vmbr0`) is attached temporarily to allow the install script to download packages from the internet. It can be removed after installation is complete.

---

#### Step 2 — Ubuntu Network Configuration

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/2.png)

Boot the Ubuntu Server ISO. On the **Network Configuration** screen, verify both interfaces have acquired a lease:

| Interface | Zone | Address |
|-----------|------|---------|
| `ens18` | SERVERS | `10.22.7.x` *(DHCP — temporary)* |
| `ens19` | WAN | `192.168.140.x` *(DHCP — internet access for install)* |

Click **Done**.

---

#### Step 3 — Profile Configuration

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/3.png)

| Field | Value |
|-------|-------|
| Your name | `wazuh` |
| Server name | `wazuh` |
| Username | `socadmin` |
| Password | *(set a strong password)* |

Click **Done**.

---

#### Step 4 — SSH Configuration

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/4.png)

Check **Install OpenSSH server** — this is required for remote management and running the installation scripts headless.

Leave all remaining options at defaults → click **Done** → allow installation to complete → **Reboot**.

---

### Part B — Wazuh All-in-One Installation

#### Step 5 — SSH Connection & System Verification

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/5.png)

From your host machine, SSH into the Wazuh server using its temporary WAN IP:

```bash
ssh socadmin@192.168.140.133
```

Verify successful login and confirm the system is ready to proceed.

---

#### Step 6 — Execute the Unattended Installation

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/6.png)

Elevate to root and download the official Wazuh installation script. Execute it with the `-a` flag for a full All-in-One deployment:

```bash
sudo su
curl -sO https://packages.wazuh.com/4.5/wazuh-install.sh
bash ./wazuh-install.sh -a
```

> 🔐 **Critical:** When the script completes, the generated `admin` password is printed to the terminal. **Save this immediately** — it cannot be recovered without resetting it manually.

---

#### Step 7 — Access the Wazuh Dashboard

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/7.png)

Open a browser and navigate to the Wazuh Web UI:

```
https://192.168.140.133/
```

Bypass the self-signed certificate warning and log in with the `admin` credentials generated in the previous step.

---

### Part C — Initial Configuration & Syslog Preparation

#### Step 8 — Create Agent Management Groups

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/8.png)

Navigate to **Management → Groups → Add new group** and create two logical containers:

| Group Name | Purpose |
|------------|---------|
| `Servers` | Domain Controllers, SIEM, infrastructure hosts |
| `Workstations` | IT, HR, and OPs domain-joined endpoints |

---

#### Step 9 — Configure Wazuh to Accept Syslog

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/9.png)

SSH back into the Wazuh server and open the main configuration file:

```bash
nano /var/ossec/etc/ossec.conf
```

Add the following `<remote>` block to instruct Wazuh to listen for incoming UDP syslog traffic from the pfSense firewall:

```xml
<remote>
  <connection>syslog</connection>
  <port>514</port>
  <protocol>udp</protocol>
  <allowed-ips>10.22.7.0/24</allowed-ips>
  <local_ip>10.22.7.67</local_ip>
</remote>
```

Save and exit: `Ctrl+O` → `Enter` → `Ctrl+X`

---

#### Step 10 — Assign Static IP via pfSense

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/10.png)

Navigate to **Services → DHCP Server → SERVERS → Static MAC Mappings → Add**:

| Field | Value |
|-------|-------|
| MAC Address | *(Wazuh VM MAC from Proxmox hardware tab)* |
| IP Address | `10.22.7.67` |
| Description | `Wazuh SIEM — Static Lease` |

Run `sudo reboot` on the Wazuh server to acquire the new static address. Validate with:

```bash
ip a
```

---

### Part D — pfSense Syslog Integration

#### Step 11 — Configure pfSense Remote Logging

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/11.png)

Navigate to **Status → System Logs → Settings → Remote Logging**:

| Setting | Value |
|---------|-------|
| Enable Remote Logging | ✅ Checked |
| Source Address | `SERVERS` |
| Remote log servers | `10.22.7.67:514` |
| Remote Syslog Contents | ✅ Everything |

Click **Save**.

---

#### Step 12 — Verify pfSense Logs in Wazuh

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/12.png)

Navigate to **Wazuh Dashboard → Modules → Security Events**. Verify that syslog messages are actively arriving from pfSense. An event such as `Syslogd restarted` associated with source `10.22.7.1` confirms the UDP/514 pipeline is fully operational.

---

#### Step 13 — Simulate & Detect SSH Brute Force

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/13.png)

To validate the SIEM's detection logic, simulate an SSH brute-force attack by attempting multiple failed logins to a monitored host. Wazuh immediately correlates the activity, triggering **Level 5** alerts (`sshd: authentication failed`) mapped to **MITRE ATT&CK** tactics:

```
Tactic:    Credential Access
Technique: T1110 — Brute Force
```

---

#### Step 14 — Review Security Events Stream

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/14.png)

Switch to the **Events** tab within Security Events. This interface provides granular, filterable logs of all triggered rules across the environment, giving the Blue Team full real-time visibility into the telemetry stream.

---

### Part E — Endpoint Agent Rollout

#### Step 15 — Generate Agent Deployment Command

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/15.png)

Navigate to **Wazuh → Agents → Deploy new agent** and configure:

| Field | Value |
|-------|-------|
| Operating System | Windows |
| Version | Windows 7+ |
| Wazuh server address | `10.22.7.67` |
| Group | `Servers` |

Copy the generated PowerShell deployment block.

---

#### Step 16 — Configure Wazuh Agent Listener (TCP 1514)

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/16.png)

Before deploying the Windows agent, ensure the Wazuh manager is listening for encrypted agent connections. SSH into the Wazuh server and append a second `<remote>` block to `/var/ossec/etc/ossec.conf`, directly below the syslog block:

```xml
<remote>
  <connection>secure</connection>
  <port>1514</port>
  <protocol>tcp</protocol>
  <queue_size>131072</queue_size>
</remote>
```

Restart the manager to apply:

```bash
sudo systemctl restart wazuh-manager
```

---

#### Step 17 — Execute Agent Deployment on DC

![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/17.png)

Log into the **Domain Controller** (VM 101). Open an **elevated Administrator PowerShell** session and paste the `Invoke-WebRequest` deployment command from Step 15 to silently download and install the MSI package.

---

#### Step 18 — Authenticate and Start the Agent

![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/18.png)

Navigate to the agent installation directory, authenticate the endpoint against the SIEM manager to obtain a valid registration key, then start the Wazuh service:

```powershell
cd "C:\Program Files (x86)\ossec-agent"
.\agent-auth.exe -m 10.22.7.67
net start wazuh
```

---

#### Step 19 — Verify Agent Registration

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-3/10-wazuh-siem/19.png)

Return to the Wazuh Dashboard and navigate to **Agents**. The DC endpoint should appear with status **Active** — confirming successful registration and full SIEM integration.

The SOC now has centralized visibility into the Active Directory environment.

---

### VM Summary

| VM | OS | Bridge | IP | Role |
|:--:|:--:|:------:|:--:|------|
| VM 104 — Wazuh | Ubuntu Server 22.04 | vmbr2 | `10.22.7.67` | Centralized SIEM · Manager · Indexer · Dashboard |
| VM 101 — DC | Windows Server 2019 | vmbr2 | `10.22.7.3` | Monitored Endpoint — Wazuh Agent |

---

### ✅ Phase Checklist

- [ ] Ubuntu Server VM provisioned — `2 cores · 8 GB RAM · 100 GB disk`
- [ ] Both NICs attached — `vmbr2` (SERVERS) + `vmbr0` (WAN temp)
- [ ] OpenSSH server installed during Ubuntu setup
- [ ] SSH login confirmed from host machine
- [ ] Wazuh All-in-One script executed successfully — `bash ./wazuh-install.sh -a`
- [ ] Admin dashboard credentials securely saved
- [ ] Wazuh Dashboard accessible at `https://192.168.140.133/`
- [ ] Agent groups created — `Servers` and `Workstations`
- [ ] `ossec.conf` updated with UDP syslog `<remote>` block on port `514`
- [ ] pfSense Static MAC Mapping applied — Wazuh anchored to `10.22.7.67`
- [ ] Wazuh server rebooted and static IP confirmed via `ip a`
- [ ] pfSense Remote Syslog configured to forward `Everything` to `10.22.7.67:514`
- [ ] Syslog ingestion verified via `Syslogd restarted` event in Security Events
- [ ] SSH brute-force simulation triggered and detected — Level 5 alerts visible
- [ ] `ossec.conf` updated with TCP secure `<remote>` block on port `1514`
- [ ] `wazuh-manager` restarted successfully
- [ ] Windows agent deployment command generated from Wazuh Dashboard
- [ ] Agent MSI silently installed on DC via elevated PowerShell
- [ ] Agent authenticated via `agent-auth.exe -m 10.22.7.67`
- [ ] Wazuh service started — `net start wazuh`
- [ ] DC endpoint showing as **Active** in Wazuh Agents dashboard

<div align="center"><br>

**🟢 Phase 3 Complete**

`[09] LAN/DMZ Traffic Isolation` ◄── **`[10] Wazuh SIEM Installation`** ──► `[11] Antivirus Integration`

<br></div>

</details>

---

<details>
<summary><b>📘 Phase 4 · [11] — Antivirus Integration & SIEM Efficiency Testing</b></summary>
<a name="phase-4--11"></a>

<br>

> **Scope:** Verifying Wazuh agent health on the Domain Controller, deploying **Kaspersky Small Office Security**, installing **Sysmon v15.15** for deep process telemetry, hardening the endpoint via the **Rasad** audit policy script and Group Policy, and executing **Atomic Red Team T1003.001** (LSASS credential dump) and a **Pass-the-Hash** attack from Kali Linux — validating the full defensive stack end-to-end.

---

### Overview

```text
Endpoint Protection Stack  (VM 101 — DC)
├── Wazuh Agent v4.5.4        — Active · ID: 001
├── Kaspersky KSOS             — File Server mode · 30-day trial
└── Sysmon v15.15              — SwiftOnSecurity config · schema 4.50

Telemetry Pipeline
├── Microsoft-Windows-PowerShell/Operational  ──►  ossec.conf  ──►  Wazuh
└── Microsoft-Windows-Sysmon/Operational      ──►  ossec.conf  ──►  Wazuh

Endpoint Hardening
├── Rasad script               — Bulk audit policy configuration
└── gpedit.msc                 — Audit Policy · User Rights · PowerShell logging

Red Team Simulations
├── Atomic Red Team T1003.001  — LSASS credential dump (Sysmon-enriched alerts)
└── crackmapexec PtH           — NTLM Pass-the-Hash toward DC (MITRE T1550.002)

Wazuh Detections
├── Rule 92032  — Suspicious Windows cmd shell execution (Level 3)
├── Rule 92052  — Windows cmd started by abnormal process (Level 4)
├── Rule 91815  — PowerShell executing process discovery (Level 4)
├── Rule 92027  — PowerShell process spawned PowerShell instance (Level 4)
├── Rule 92652  — Possible Pass-the-Hash · NTLM ANONYMOUS LOGON (Level 6)
├── Rule 60122  — Logon failure, unknown user or bad password (Level 5)
└── Rule 60137  — Windows User Logoff · session cleanup (Level 3)
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | Agent Verification | Confirm Wazuh agent registration and live connectivity on the DC |
| **B** | Antivirus Deployment | Install and configure Kaspersky Small Office Security |
| **C** | Sysmon Deployment | Install Sysmon v15.15 with SwiftOnSecurity config and pipe telemetry to Wazuh |
| **D** | Endpoint Hardening | Apply audit policies, user rights, and PowerShell logging via Rasad + GPO |
| **E** | Red Team — LSASS Dump | Execute Atomic Red Team T1003.001 credential dump simulation |
| **F** | Red Team — Pass-the-Hash | Execute CrackMapExec NTLM PtH attack from Kali Linux |
| **G** | SIEM Validation | Analyse all Wazuh detections, MITRE mappings, and forensic event fields |

---

### Part A — Wazuh Agent Verification

#### Step 1 — Open the Wazuh Agent Manager

<img width="974" height="519" alt="1" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/1.png" />

SSH into the Wazuh server and launch the agent management CLI:

```bash
sudo /var/ossec/bin/manage_agents
```

The **Wazuh v4.5.4 Agent Manager** presents four options. Select `l` to list all currently registered agents.

---

#### Step 2 — Confirm DC Agent Registration

<img width="974" height="519" alt="2" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/2.png" />

The agent list confirms the Domain Controller is fully registered:

| Field | Value |
|-------|-------|
| ID | `001` |
| Name | `DC` |
| IP | `any` *(dynamic — accepts from SERVERS subnet)* |

> ✅ The Wazuh agent on the DC is registered and actively reporting. Proceed to antivirus deployment.

---

#### Step 3 — Verify Agent Logs on the DC

<img width="1080" height="810" alt="3" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/3.png" />

On the **Domain Controller**, navigate to the agent installation directory:

```
C:\Program Files (x86)\ossec-agent\
```

Open `ossec.log` to inspect the agent runtime log. Key entries confirm healthy operation:

```
agent-auth: INFO: Valid key received
wazuh-agent: INFO: Connected to the server
wazuh-agent: INFO: Using AES as encryption method
rootcheck: INFO: Started (monitoring registry keys...)
```

> ℹ️ `ossec.log` is your first diagnostic stop if the agent goes silent — version mismatches between the agent and manager surface here immediately.

---

#### Step 4 — Inspect Agent Manager UI (win32ui)

<img width="1080" height="810" alt="4" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/4.png" />

Launch `win32ui.exe` from the same directory. The **Wazuh Agent Manager** GUI confirms:

| Field | Value |
|-------|-------|
| Agent | `DC (001)` |
| Status | **Running** |
| Manager IP | `10.22.7.67` |
| Authentication Key | *(auto-populated on registration)* |

> 📋 Use **Refresh** if the connection state appears stale. The auth key shown here was generated during `agent-auth.exe` execution in `[10]`.

---

### Part B — Antivirus Deployment (Kaspersky KSOS)

#### Step 5 — Download Kaspersky Small Office Security

<img width="1214" height="862" alt="5" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/5.png" />

Navigate to [kaspersky.com/small-to-medium-business-security/downloads](https://www.kaspersky.com/small-to-medium-business-security/downloads) and select **Kaspersky Small Office Security → Download**.

---

#### Step 6 — Launch the Installer

<img width="1080" height="810" alt="6" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/6.png" />

Launch the downloaded installer on the Domain Controller. When prompted to select the protection mode, choose **File Server** from the dropdown.

Click **Continue** to proceed through the installation wizard with default settings.

---

#### Step 7 — Remove Incompatible Software (Windows Defender)

<img width="1080" height="810" alt="7" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/7.png" />

Kaspersky detects **Windows Defender** as incompatible and flags it for automatic removal. Click **Delete** → system restarts to complete the transition.

> ⚠️ Running two real-time AV engines simultaneously causes performance degradation and signature conflicts. Kaspersky fully replaces the Defender stack.

---

#### Step 8 — Activate — Try for Free

<img width="1080" height="810" alt="8" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/8.png" />

After the restart, relaunch `startup.exe`. Click **Try for free** to activate the 30-day trial without a license key.

---

#### Step 9 — Skip Account Registration

<img width="1080" height="810" alt="9" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/9.png" />

Click **Skip** — account registration is not required for this isolated lab environment.

---

#### Step 10 — Kaspersky Dashboard — Protection Active

<img width="1080" height="810" alt="10" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/10.png" />

The Kaspersky **Small Office Security** dashboard confirms the DC is now protected in **File Server** mode. Run a database update to pull the latest threat signatures before proceeding.

---

### Part C — Sysmon Deployment & Telemetry Pipeline

#### Step 11 — Download Sysmon v15.15

<img width="1540" height="760" alt="15" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/15.png" />

Navigate to [learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) and download **Sysmon v15.15**.

---

#### Step 12 — Download the SwiftOnSecurity Configuration

<img width="1540" height="810" alt="16" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/16.png" />

Use the industry-standard **SwiftOnSecurity** config from [github.com/SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config).

---

#### Step 13 — Install Sysmon with the Config

<img width="1080" height="810" alt="17" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/17.png" />

```powershell
.\Sysmon64.exe -accepteula -i .\sysmonconfig-export.xml
```

---

#### Step 14 — Configure Wazuh Agent to Collect Sysmon and PowerShell Logs

<img width="1080" height="810" alt="18" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/18.png" />

Add the following two `<localfile>` blocks to `ossec.conf` on the DC:

```xml
<localfile>
  <location>Microsoft-Windows-PowerShell/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>

<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

---

### Part D — Endpoint Hardening (Rasad Script + Group Policy)

#### Step 15 — Run the Rasad Hardening Script

<img width="1080" height="810" alt="19" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/19.png" />

```powershell
.\Rasad.ps1
```

---

#### Step 16 — Open Group Policy Editor

<img width="1080" height="810" alt="20" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/20.png" />

Press **Win + R** → type `gpedit.msc` → click **OK**.

---

#### Step 17 — Review Audit Policy Configuration

<img width="1080" height="810" alt="21" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/21.png" />

| Audit Category | Setting |
|----------------|---------|
| Audit account logon events | Success, Failure |
| Audit account management | Success, Failure |
| Audit directory service access | Failure |
| Audit policy change | Success, Failure |
| Audit process tracking | Success, Failure |
| Audit system events | Success, Failure |

---

#### Step 18 — Review User Rights Assignment

<img width="1080" height="810" alt="22" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/22.png" />

Navigate to **User Rights Assignment**. Review and tighten assignments based on least privilege.

---

#### Step 19 — Enable PowerShell Logging via Group Policy

<img width="1080" height="810" alt="23" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/23.png" />

| Policy | State |
|--------|-------|
| Turn on PowerShell Script Block Logging | **Enabled** |
| Turn on Script Execution | **Enabled** |
| Turn on PowerShell Transcription | **Enabled** |

---

### Part E — Red Team Simulation: LSASS Credential Dump (T1003.001)

#### Step 20 — Inspect the Atomic Test Details

<img width="1080" height="810" alt="24" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/24.png" />

```powershell
Invoke-AtomicTest T1003.001 -ShowDetails
```

---

#### Step 21 — Execute the Atomic Test

<img width="1080" height="810" alt="25" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/25.png" />

```powershell
Invoke-AtomicTest T1003.001
```

| Sub-test | Method | Result |
|----------|--------|--------|
| T1003.001-1 | ProcDump | `Access is denied` — Kaspersky blocked |
| T1003.001-2 | comsvcs.dll | Exit `0` |
| T1003.001-3 | Dumpert | `Failed to get processhandle` — AV blocked |
| T1003.001-4 | NanoDump | Silently failed |

---

### Part F — Red Team Simulation: Pass-the-Hash Attack

#### Step 22 — Execute SMB Pass-the-Hash via CrackMapExec

<img width="1280" height="720" alt="11" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/11.png" />

```bash
crackmapexec smb 192.168.140.135 -u Administrator -H 43ba4b0afe48d65e5f47b0f7ac1e6f8e
```

```
SMB  192.168.140.135  445  DC  STATUS_LOGON_FAILURE
```

---

### Part G — SIEM Validation

#### Step 23 — Wazuh Security Events: Atomic Test Alerts

<img width="1540" height="680" alt="26" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/26.png" />

| Rule | Description | Level |
|------|-------------|:-----:|
| `92032` | Suspicious Windows cmd shell execution | 3 |
| `92052` | Windows command prompt started by abnormal process | 4 |
| `91815` | PowerShell executing process discovery | 4 |
| `92027` | PowerShell process spawned PowerShell instance | 4 |

---

#### Step 24 — Forensic Analysis: Rule 92032

<img width="1540" height="760" alt="27" src="https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/11-antivirus-siem/27.png" />

| Field | Value |
|-------|-------|
| `commandLine` | `procdump.exe -accepteula -mm lsass.exe C:\Windows\Temp\lsass_dump.dmp` |
| `parentImage` | `C:\Windows\System32\cmd.exe` |
| `integrityLevel` | `High` |
| `parentUser` | `MURSAD\Administrator` |

---

#### Step 25 — Pass-the-Hash SIEM Detections

**Rule 92652 — Pass-the-Hash Detection**

| Field | Value |
|-------|-------|
| Severity Level | `6` |
| MITRE Techniques | `T1550.002` · `T1078.002` |
| `authenticationPackageName` | `NTLM` |
| `targetUserName` | `ANONYMOUS LOGON` |
| `logonType` | `3` |
| Attacker IP | `192.168.140.131` |

---

### ✅ Phase Checklist

- [ ] Wazuh agent confirmed active — `ID: 001 · Name: DC`
- [ ] Kaspersky KSOS installed in **File Server** mode · trial activated
- [ ] Sysmon v15.15 installed with SwiftOnSecurity config
- [ ] `ossec.conf` updated with PowerShell and Sysmon log channels
- [ ] `Rasad.ps1` executed — all audit policies showing `[OK]`
- [ ] PowerShell Script Block Logging enabled via GPO
- [ ] `Invoke-AtomicTest T1003.001` executed — ProcDump blocked by Kaspersky
- [ ] 17 Wazuh alerts generated from the Atomic test window
- [ ] PtH attack executed from Kali — `STATUS_LOGON_FAILURE`
- [ ] Rule 92652 triggered — Level 6 · `T1550.002`
- [ ] Event ID 4624 forensics reviewed — `ANONYMOUS LOGON · NTLM V1`

<div align="center"><br>

**🟢 Phase 4 · [11] Complete**

`[10] Wazuh SIEM Installation` ◄── **`[11] Antivirus Integration & SIEM Testing`** ──► `[12] Infrastructure Auditing via CIS Benchmarks`

<br></div>

</details>

---

<details>
<summary><b>📙 Phase 4 · [12] — Infrastructure Auditing via CIS Benchmarks</b></summary>
<a name="phase-4--12"></a>

<br>

> **Objective:** *Assess the security posture of the Windows Server infrastructure against industry-standard CIS Controls benchmarks using CIS-CAT Lite, identify misconfigurations, and establish a baseline for hardening.*

---

### 🔍 Why CIS-CAT Lite?

**CIS-CAT Lite** (Configuration Assessment Tool) is a free, vendor-neutral auditing tool developed by the **Center for Internet Security (CIS)**. It evaluates a system's configuration against the **CIS Benchmarks** — globally recognized best practices for securing operating systems, applications, and network devices.

In the context of this SOC lab, CIS-CAT Lite serves as a **compliance baseline scanner** that gives us a measurable, scored snapshot of how hardened our Domain Controller is before any manual hardening takes place. Running this assessment allows us to:

- **Identify** out-of-the-box misconfigurations on the Windows Server that attackers could exploit.
- **Prioritize** remediation efforts using scored results with built-in remediation guidance.
- **Validate** future hardening changes by re-running assessments and comparing scores over time.
- **Map** findings to CIS Controls (IG1 Sub-Controls), which align with frameworks like NIST CSF, ISO 27001, and SOC 2.

> The **Lite** version is free and covers the most critical IG1 (Implementation Group 1) controls — the "essential cyber hygiene" baseline every organization should meet regardless of size.

---

### Step 1 — Tool Installation & File Structure

After downloading **CIS-CAT Lite Assessor v4.56.0**, the extracted folder contains all components needed to run both GUI and CLI-based assessments.

![CIS-CAT Lite Folder Structure](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/1.png)

---

### Step 2 — Launching the GUI & Selecting Scan Mode

Opening `Assessor-GUI` presents the CIS Configuration Assessment Tool welcome screen. Since we are auditing only the local system (the Domain Controller), the **Basic** scan mode is selected.

![CIS-CAT Lite Welcome Screen](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/2.png)

---

### Step 3 — Benchmark & Profile Selection

From the benchmark library, we select the **CIS Controls Assessment Module — Implementation Group 1 for Windows Server v1.0.0**.

![Benchmark Selection](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/3.png)

---

### Step 4 — Report Output & Assessment Options

![Assessment Options](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/4.png)

---

### Step 5 — Assessment Execution & Completion

CIS-CAT runs through the benchmark checklist automatically, exiting with **Exit Code: 0**.

![Assessment Complete](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/5.png)

---

### Step 6 — Report Overview

![Report Cover Page](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/6.png)

---

### Step 7 — Summary Results: Overall Score 74%

| Section | Pass | Fail | Score | Max | Percent |
|---|---|---|---|---|---|
| Automated Checks | 4 | 11 | 4.0 | 14.0 | **29%** |
| User Survey Questions | 28 | 1 | 28.0 | 29.0 | **97%** |
| **Total** | **32** | **12** | **32.0** | **43.0** | **74%** |

![Assessment Summary](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/7.png)

---

### Step 8 — Full Assessment Results Breakdown

Key failing automated checks include:

- `1.1` — **3.4 Deploy Automated OS Patch Management Tools** → ❌ Fail
- `1.2` — **4.2 Change Default Passwords** → ❌ Fail
- `1.4` — **8.2 Ensure Anti-Malware Software and Signatures are Updated** → ❌ Fail
- `1.5` — **8.4 Configure Anti-Malware Scanning of Removable Media** → ❌ Fail
- `1.6` — **8.5(a) Configure Devices to Not Auto-Run Content (AutoRun)** → ❌ Fail
- `1.7` — **8.5(b) Configure Devices to Not Auto-Run Content (AutoPlay)** → ❌ Fail
- `1.9` — **10.1 Ensure Regular Automated Backups** → ❌ Fail
- `1.10` — **10.2 Perform Complete System Backups** → ❌ Fail

![Assessment Results List](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/8.png)

---

### Step 9 — Failed Control Deep-Dive: Change Default Passwords (4.2)

![Change Default Passwords - Fail](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/9.png)

> **Remediation:** Navigate to `Computer Configuration > Windows Settings > Security Settings > Account Policies > Password Policy > Minimum password length` and set it to **14 or more characters**.

---

### Step 10 — Passed Control Deep-Dive: Activate Audit Logging (6.2)

![Activate Audit Logging - Pass](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/12-cis-benchmarks/10.png)

> Audit logging was already activated as part of the Wazuh SIEM integration in `[10]`. CIS-CAT confirms this is correctly configured.

---

### 📊 Baseline Summary

| Metric | Value |
|---|---|
| Tool | CIS-CAT Lite Assessor v4.56.0 |
| Benchmark | CIS Controls Assessment Module — IG1 for Windows Server v1.0.0 |
| Profile | Implementation Group 1 for Windows Server (Full) |
| Target | DC — `10.22.7.3` |
| Assessment Date | March 25, 2026 |
| **Pre-Hardening Score** | **74% (32/43)** |
| Automated Pass Rate | 29% (4/15) |
| Survey Pass Rate | 97% (28/29) |
| Critical Fails | Patch Management, Password Policy, Anti-Malware, AutoRun, Backups |

---

### ✅ Phase Checklist

- [ ] CIS-CAT Lite Assessor v4.56.0 extracted and verified
- [ ] `Assessor-GUI` launched — Basic scan mode selected
- [ ] Benchmark selected — CIS Controls Assessment Module IG1 for Windows Server v1.0.0
- [ ] Profile selected — Implementation Group 1 for Windows Server (Full)
- [ ] Assessment options confirmed — HTML output · reports path set
- [ ] Assessment run completed — Exit Code: 0
- [ ] HTML report opened and verified — target IP `10.22.7.3`
- [ ] Overall score recorded — **74% (32/43)**
- [ ] Automated pass rate noted — 29% (4/15) · Survey pass rate — 97% (28/29)
- [ ] All 11 failing automated controls reviewed
- [ ] Rule 1.2 (4.2 Default Passwords) remediation path documented
- [ ] Rule 1.3 (6.2 Audit Logging) confirmed as PASS — tied to `[10]` Wazuh work

<div align="center"><br>

**🟢 Phase 4 · [12] Complete**

`[11] Antivirus Integration & SIEM Testing` ◄── **`[12] Infrastructure Auditing via CIS Benchmarks`** ──► `[13] Kaspersky Security Center`

<br></div>

</details>

---

<details>
<summary><b>📗 Phase 4 · [13] — Kaspersky Security Center EDR/AV Enterprise Setup</b></summary>
<a name="phase-4--13"></a>

<br>

> **Scope:** Deploying **Kaspersky Security Center (KSC)** as a full enterprise EDR/AV management platform on a dedicated EDR server — including SQL Server 2022 backend, the KSC Web Console, agent package generation, and rolling out the Network Agent to the Domain Controller via the Run dialog.

---

### Overview

```text
Proxmox Node: mursad
└── EDR Server  (Windows Server — SERVERS zone)
        ├── SQL Server 2022       — KSC database backend
        ├── Kaspersky Security Center 12.12.0
        │       ├── Administration Server
        │       ├── Web Console   (127.0.0.1:8080)
        │       └── Network Agent package
        └── Managed Devices
                └── DC (10.22.7.3) ← Agent deployed via Run dialog
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | SQL Server Setup | Install SQL Server 2022 as the KSC database backend |
| **B** | KSC Installation | Install Kaspersky Security Center with Web Console |
| **C** | Initial Configuration | Quick Start Wizard, activation, and OS targeting |
| **D** | Web Console Access | Log in to the KSC Web Console and explore the dashboard |
| **E** | Agent Deployment | Generate a stand-alone agent package and push it to the DC |
| **F** | Verification | Confirm the DC appears as a managed device in KSC |

---

### Part A — SQL Server Installation

#### Step 1 — SQL Server Basic Setup

![1](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/1.png)

Download **SQL Server 2022**. Execute the application and click on **Basic** to begin the installation.

---

#### Step 2 — Completing SQL Setup

![2](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/2.png)

Accept all the default settings and terms. Once the installation completes successfully, click **Close**.

---

### Part B — Kaspersky Security Center Installation

#### Step 3 — Launching Kaspersky Installer

![3](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/3.png)

Download the **Kaspersky Security Center** for the EDR Server. Execute the application and click on **Install Kaspersky Security Center**.

---

#### Step 4 — Cluster Installation Type

![4](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/4.png)

Click **Next** through the default options and accept the license terms. On the **"Type of installation on cluster"** page, select **Locally**.

> ℹ️ Select the cluster option only if you have more than one node. For this single-node lab deployment, **Locally** is the correct choice.

---

#### Step 5 — Administrator Consoles

![5](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/5.png)

Choose the default option to **Install both administrator consoles** — this installs both the desktop MMC console and prepares the Web Console.

---

#### Step 6 — Network Size Configuration

![6](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/6.png)

Since the project involves fewer than 100 devices, select **Fewer than 100 networked devices**.

> ℹ️ This setting optimizes KSC's internal polling intervals and database indexing for a small-scale deployment.

---

#### Step 7 — Database Selection

![7](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/7.png)

Select **Microsoft SQL Server** as the database backend — corresponding to the SQL Server 2022 installation completed in Part A.

---

#### Step 8 — SQL Connection Settings

![8](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/8.png)

In the connection settings, click **Browse** to detect and select your SQL Server instance name, then click **Next**.

---

#### Step 9 — Authentication Mode

![9](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/9.png)

Keep the default **Microsoft Windows authentication mode** selected and click **Next**.

---

#### Step 10 — Web Console Configuration

![10](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/10.png)

Proceed through the previous default settings until you reach the **KSC Web Console** setup. Leave the default address (`127.0.0.1`) and port (`8080`) as-is, then click **Next**.

---

#### Step 11 — Finalizing Web Console Setup

![11](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/11.png)

Keep the default accounts and ensure the certificate is set to the EDR default before clicking **Next**. Leave trusted administration servers as default and click **Next**. On the **Identity and Access Management (IAM)** page, uncheck the IAM box unless you specifically want IAM configured in this environment.

---

### Part C — Initial KSC Configuration

#### Step 12 — Initial KSC Launch

![12](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/12.png)

Open the **KSC** desktop application. It will verify that your certificate is valid.

> ⚠️ **Note:** Ensure your EDR Server is connected through the Domain Controller before proceeding. Click **Yes** to accept the certificate and continue.

---

#### Step 13 — Quick Start Wizard

![13](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/13.png)

The **KSC Quick Start Wizard** will launch automatically. Work through the wizard as follows:

1. Click **Next**
2. Leave the proxy server settings **unchecked** (assuming no proxy) → click **Next**
3. Choose **Activate Application later** → click **Next**
4. Select the target OS environments: **Windows & Linux + Workstation** → click **Next**

---

### Part D — Web Console Access

#### Step 14 — Web Console Login

![14](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/14.png)

Once setup is complete, switch to the **KSC Web Console** and sign in using your EDR Server credentials.

---

#### Step 15 — Web Console Dashboard

![15](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/15.png)

You will now see the main dashboard of the **KSC Web Console** — providing a centralized view of the managed device estate, policy status, and security event summaries.

---

#### Step 16 — Managing Users and Groups

![16](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/16.png)

In the Web Console, navigate to **Users & Groups** where you can manage, add, or delete users and configure role-based access control for the KSC administration hierarchy.

---

#### Step 17 — Application Management

![17](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/17.png)

In the desktop KSC application, navigate to **Advanced → Application management**. Here you can configure policies to allow or deny specific applications from starting, manage application categories, and configure inventory controls across managed endpoints.

---

### Part E — Agent Deployment

#### Step 18 — Extracting the Agent Package

![18](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/18.png)

Navigate to **Remote installation → Installation packages**. Select **KSC for Windows 12.12.0** and click **Create stand-alone installation package**.

---

#### Step 19 — Stand-alone Package Wizard

![19](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/19.png)

In the creation wizard, choose the option to **move the device to the list of managed devices** and select the appropriate group. You can customize the target group based on your deployment strategy — for example, creating dedicated packages for the DMZ or specific workstation groups. Click **Next** to generate the package.

---

#### Step 20 — Installing Agent on DC

![20](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/20.png)

Copy the generated file path or command, switch to the **Domain Controller (DC)**, and paste it into the **Run** dialog (**Win + R**) to execute and install the EDR Network Agent directly onto the DC.

---

#### Step 21 — EDR Agent Installation

![21](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/21.png)

The installation process will begin on the Domain Controller. This typically takes a few minutes to complete.

> ⚠️ A **system restart** is required once the installation finishes to fully initialize the Kaspersky Network Agent service.

---

### Part F — Verification

#### Step 22 — Verifying Connection in KSC

![22](https://github.com/0xcgz/Project-Mursad/blob/main/assets/phase-4/13-kaspersky-edr/22.png)

Once the restart is complete, return to the **Kaspersky Security Center** console. Navigate to **Managed devices** — the Domain Controller should now appear in the list, confirming it has connected successfully and is under centralized KSC management.

---

### VM Summary

| VM | Role | Zone | Key Services |
|:--:|------|:----:|-------------|
| EDR Server | Kaspersky Security Center | SERVERS | SQL Server 2022 · KSC Admin · Web Console `:8080` |
| VM 101 — DC | Managed Endpoint | SERVERS | KSC Network Agent · `mursad.local` |

---

### ✅ Phase Checklist

- [ ] SQL Server 2022 downloaded and installed via **Basic** mode
- [ ] SQL Server installation completed successfully — Close clicked
- [ ] Kaspersky Security Center installer downloaded for the EDR Server
- [ ] KSC installed — cluster type set to **Locally**
- [ ] Both administrator consoles selected for installation
- [ ] Network size set to **Fewer than 100 networked devices**
- [ ] **Microsoft SQL Server** selected as the database backend
- [ ] SQL Server instance detected via **Browse** and confirmed
- [ ] **Windows authentication mode** kept as default
- [ ] KSC Web Console address left as `127.0.0.1:8080`
- [ ] Web Console certificate set to EDR default
- [ ] IAM unchecked (not required for this lab)
- [ ] Desktop KSC application opened — certificate accepted
- [ ] Quick Start Wizard completed — no proxy · activation deferred · Windows & Linux + Workstation selected
- [ ] KSC Web Console login successful using EDR Server credentials
- [ ] Main dashboard visible in Web Console
- [ ] Users & Groups section reviewed in Web Console
- [ ] Application Management explored in desktop KSC — **Advanced → Application management**
- [ ] Stand-alone installation package created from **KSC for Windows 12.12.0**
- [ ] Package targeted to correct managed device group
- [ ] Agent installation command copied and executed on DC via **Run** dialog
- [ ] Agent installation completed on DC — system restarted
- [ ] DC appears in **Managed devices** in KSC console ✅

<div align="center"><br>

**🟢 Phase 4 · [13] Complete**

`[12] Infrastructure Auditing via CIS Benchmarks` ◄── **`[13] Kaspersky Security Center EDR/AV Enterprise Setup`** ──► `[14] Final Security Review & Operations Wrap-Up`

<br></div>

</details>

---

## 📁 Repository Structure

```
Project-Mursad/
│
├── 📂 assets/
│   ├── phase-1/
│   │   ├── 02-proxmox-deployment/     # 24 screenshots
│   │   ├── 03-pfsense-installation/   # 38 screenshots
│   │   └── 04-firewall-routing/       # 21 screenshots
│   ├── phase-2/
│   │   ├── 05-dc-provisioning/        # 30 screenshots
│   │   ├── 06-active-directory/       # 33 screenshots
│   │   ├── 07-dmz-architecture/       # 15 screenshots
│   │   └── 08-suricata/               # 30 screenshots
│   ├── phase-3/
│   │   ├── 09-lan-dmz-isolation/      # 9 screenshots
│   │   └── 10-wazuh-siem/             # 19 screenshots
│   └── phase-4/
│       ├── 11-antivirus-siem/         # 27 screenshots
│       ├── 12-cis-benchmarks/         # 10 screenshots
│       └── 13-kaspersky-edr/          # 22 screenshots
│
├── 📂 configs/
│   ├── pfsense/
│   ├── suricata/
│   ├── wazuh/
│   ├── active-directory/
│   └── windows/
│
├── 📂 docs/
│   ├── phase-1/
│   │   ├── 02-proxmox-deployment.md
│   │   ├── 03-pfsense-installation.md
│   │   └── 04-firewall-routing.md
│   ├── phase-2/
│   │   ├── 05-dc-provisioning.md
│   │   ├── 06-active-directory.md
│   │   ├── 07-dmz-architecture.md
│   │   └── 08-suricata-ids-ips.md
│   ├── phase-3/
│   │   ├── 09-lan-dmz-isolation.md
│   │   └── 10-wazuh-siem.md
│   └── phase-4/
│       ├── 11-antivirus-siem.md
│       ├── 12-cis-benchmarks.md
│       └── 13-kaspersky-edr.md
│
├── 📂 scripts/
│   ├── ad-provisioning.ps1
│   ├── wazuh-agent-deploy.sh
│   ├── Rasad.ps1
│   └── cis-audit.sh
│
├── README.md
└── LICENSE
```

---

## ⚠️ Disclaimer

This project is built strictly for **educational purposes** and authorized security research in a fully isolated, privately-owned virtual environment. All attack simulations, penetration tests, and adversary emulations are conducted exclusively within this lab. Do not reproduce any offensive techniques outside of your own authorized and controlled environment. The author assumes no liability for misuse.

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:e94560,100:0d1117&height=120&section=footer&text=Built+to+understand+the+attack.+Designed+to+defend+against+it.&fontSize=13&fontColor=ffffff&fontAlignY=65&animation=fadeIn" width="100%"/>

</div>
