<div align="center">

<br>

<h1>👁️ PROJECT MURSAD</h1>
<h3>Enterprise Security Architecture & SOC Telemetry Lab</h3>

<br>

<p align="center">
  <img src="https://img.shields.io/badge/Proxmox_VE-E57000?style=for-the-badge&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/pfSense_CE-212121?style=for-the-badge&logo=pfsense&logoColor=white"/>
  <img src="https://img.shields.io/badge/Active_Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white"/>
  <img src="https://img.shields.io/badge/Wazuh_SIEM-6B4CFF?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/Suricata_IDS%2FIPS-EF6C00?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white"/>
</p>

<br>

<p align="center">
  <img src="https://img.shields.io/badge/Phase_1-COMPLETE-2ecc71?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Phase_2-IN_PROGRESS-e94560?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Phase_3-PENDING-555555?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Phase_4-PENDING-555555?style=flat-square&labelColor=1a1a2e"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Progress-Phase%202%20In%20Progress%20%7C%2050%25-e94560?style=flat-square&labelColor=1a1a2e"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Proxmox%20VE%209.1-E57000?style=flat-square&logo=proxmox&logoColor=white&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/AD%20Domain-mursad.local-0078D4?style=flat-square&logo=microsoft&logoColor=white&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Host%20FQDN-mursad.me-6B4CFF?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/License-MIT-2ecc71?style=flat-square&labelColor=1a1a2e"/>
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
- [Repository Structure](#-repository-structure)
- [Disclaimer](#-disclaimer)

---

## 🔍 Overview

Project Mursad is a fully virtualized, enterprise-grade Security Operations Center lab built for hands-on Blue Team and Red Team practice. Running entirely on a single Proxmox host nested inside VMware Workstation Pro, it simulates a realistic corporate environment with proper zone segmentation, Active Directory identity management, inline intrusion detection via Suricata, and centralized SIEM telemetry through Wazuh.

**Core objectives:**

| Objective | Description |
|-----------|-------------|
| 🔒 Network Segmentation | Multi-zone pfSense firewall — Workstation, Servers, DMZ isolation |
| 🎯 Adversary Simulation | Red Team capability via isolated Kali Linux on the WAN |
| 📊 SOC Telemetry | Wazuh SIEM collecting logs from all endpoints and network infrastructure |
| 🚨 Intrusion Detection | Suricata IDS/IPS inline on pfSense for live traffic inspection |
| 🛡️ Endpoint Hardening | CIS benchmarks, EDR via Kaspersky Security Center, AV integration |
| 🔁 Incident Response | Realistic alert triage, correlation, and response workflows |

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
║  │            10.22.0/24 10.22.1/24 10.22.2/24 192.168.50/24 10.22.7/24      │   ║
║  │               HR         IT        OPs        DMZ       SERVERS            │   ║
║  │               │          │          │           │           │               │   ║
║  │         ┌─────┴───┐  ┌───┴──┐  ┌───┴──┐  ┌────┴────┐  ┌───┴─────┐         │   ║
║  │         │DC .0.2  │  │IT WS │  │Ops WS│  │DMZ Srvr │  │  Wazuh  │         │   ║
║  │         │mursad   │  │.1.x  │  │.2.x  │  │.50.10   │  │  SIEM   │         │   ║
║  │         │.local   │  └──────┘  └──────┘  └─────────┘  │ .7.2    │         │   ║
║  │         └─────────┘                                    └─────────┘         │   ║
║  │                                                                             │   ║
║  │   [ Kali Linux ]  ──  WAN/DHCP  ──  External Red Team Attacker             │   ║
║  └───────────────────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════════════════╝
```

---

## 🌐 Network Zones

| Zone | Bridge | Subnet | Gateway | Purpose |
|------|:------:|--------|:-------:|---------|
| 🌍 WAN / Management | `vmbr0` | `192.168.140.0/24` | `192.168.140.2` | NAT uplink · Proxmox host access |
| 💻 HR Workstation | `vmbr1` | `10.22.0.0/24` | `10.22.0.1` | HR domain-joined endpoints · DC |
| 💻 IT Workstation | `vmbr2` | `10.22.1.0/24` | `10.22.1.1` | IT department endpoints |
| 💻 OPs Workstation | `vmbr3` | `10.22.2.0/24` | `10.22.2.1` | Operations department endpoints |
| 🔶 DMZ | `vmbr4` | `192.168.50.0/24` | `192.168.50.1` | Isolated public-facing services |
| 🖥️ Servers | `vmbr5` | `10.22.7.0/24` | `10.22.7.1` | Internal services · Wazuh SIEM |

---

## 📡 IP Address Table

| Host | IP Address | Zone | Role |
|------|:----------:|:----:|------|
| Proxmox Node | `192.168.140.129` | Management | Hypervisor — Web UI `:8006` |
| pfSense — WAN | `192.168.140.x` (DHCP) | WAN | Internet uplink |
| pfSense — HR | `10.22.0.1` | HR Workstation | HR segment gateway |
| pfSense — IT | `10.22.1.1` | IT Workstation | IT segment gateway |
| pfSense — OPs | `10.22.2.1` | OPs Workstation | Operations segment gateway |
| pfSense — DMZ | `192.168.50.1` | DMZ | DMZ gateway |
| pfSense — SERVERS | `10.22.7.1` | Servers | Servers gateway |
| Windows Server DC | `10.22.0.2` | HR Workstation | AD · DNS · `mursad.local` |
| IT Workstation | `10.22.1.x` | IT Workstation | Domain-joined · Wazuh agent |
| OPs Workstation | `10.22.2.x` | OPs Workstation | Domain-joined · Wazuh agent |
| Wazuh SIEM | `10.22.7.2` | Servers | Log aggregation · alerting |
| DMZ Server | `192.168.50.10` | DMZ | Public-facing services |
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
| <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kali-linux&logoColor=white"/> | Red Team · penetration testing | Latest |

---

## 🗓️ Roadmap

<br>

<div align="center">

```
  PHASE 1          PHASE 2          PHASE 3          PHASE 4
Infrastructure  ──►  Identity    ──►  Telemetry   ──►  Hardening
  & Perimeter     & Segmentation  & Detection      & Validation
  [ COMPLETE ]     [ ACTIVE ]       [ PENDING ]      [ PENDING ]
```

</div>

<br>

### ◆ Phase 1 — Infrastructure & Perimeter Initialization

> *Establishing the hypervisor environment and securing the network edge.*

| # | Task | Status |
|:---:|------|:------:|
| `[01]` | Project Architecture & Introduction | `✅` |
| `[02]` | Proxmox Hypervisor Deployment & Bridge VLAN Engineering | `✅` |
| `[03]` | pfSense Edge Firewall Installation & Baseline Setup | `✅` |
| `[04]` | Advanced Firewall Routing & Network Configuration | `✅` |

<br>

### ◆ Phase 2 — Enterprise Identity & Network Segmentation

> *Building the corporate network, managing identities, and isolating services.*

| # | Task | Status |
|:---:|------|:------:|
| `[05]` | Domain Controller Provisioning & Network Integration | `✅` |
| `[06]` | Active Directory Domain Installation & Configuration | `🔄` |
| `[07]` | DMZ Architecture Setup | `⬜` |
| `[08]` | LAN/DMZ Traffic Isolation & Secure Local DNS Mapping | `⬜` |

<br>

### ◆ Phase 3 — SOC Telemetry & Traffic Analysis

> *Deploying the eyes and ears of the network.*

| # | Task | Status |
|:---:|------|:------:|
| `[09]` | Suricata IDS/IPS Configuration | `⬜` |
| `[10]` | Wazuh SIEM Installation · Syslog Ingestion · Agent Rollout | `⬜` |

<br>

### ◆ Phase 4 — Endpoint Defense, Hardening & Validation

> *Locking down endpoints and validating detection capability.*

| # | Task | Status |
|:---:|------|:------:|
| `[11]` | Antivirus Integration & SIEM Efficiency Testing | `⬜` |
| `[12]` | Infrastructure Auditing via CIS Benchmarks | `⬜` |
| `[13]` | Kaspersky Security Center (EDR/AV) Enterprise Setup | `⬜` |
| `[14]` | Final Security Review & Operations Wrap-Up | `⬜` |

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
                        ├── vmbr2  →  IT Workstation          10.22.1.1/24
                        ├── vmbr3  →  OPs Workstation         10.22.2.1/24
                        ├── vmbr4  →  DMZ Zone                192.168.50.1/24
                        └── vmbr5  →  Servers Segment         10.22.7.1/24
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Preparation | Define the VMware virtual hardware container |
| **B** | Proxmox OS Install | Deploy the hypervisor operating system |
| **C** | Web GUI & Network | Engineer the internal SOC bridge network |

---

### Part A — Virtual Machine Preparation

#### Step 1 — Download Proxmox VE

<img width="1394" height="1263" alt="proxmox-download" src="https://github.com/user-attachments/assets/6136a1bf-d31d-43a1-a7ba-00084520a044" />

Navigate to [proxmox.com/en/downloads](https://www.proxmox.com/en/downloads) and download **Proxmox VE 9.1-1 ISO Installer**.

---

#### Step 2 — Create a New Virtual Machine

<img width="842" height="590" alt="vmware-new-vm" src="https://github.com/user-attachments/assets/27bafdc6-26e3-4ed8-b2a8-06c4ab5a46f6" />

Launch **VMware Workstation Pro 17** and create a new VM via:
- **"Create a New Virtual Machine"** on the dashboard, or
- **File → New Virtual Machine**

---

#### Step 3 — Mount the Proxmox ISO

<img width="840" height="592" alt="vmware-iso" src="https://github.com/user-attachments/assets/006037a3-4952-49e9-99f6-8befcd6191c3" />

Select **"Installer disc image file (iso)"** → Browse → mount:

```
proxmox-ve_9.1-1.iso
```

---

#### Step 4 — Select Guest OS

<img width="842" height="587" alt="vmware-guestos" src="https://github.com/user-attachments/assets/ad4f4547-5645-4c28-b6c9-cfa2ff6dd13b" />

| Setting | Value |
|---------|-------|
| Guest OS | Linux |
| Version | Ubuntu *(Proxmox is Debian-based — Ubuntu profile ensures nested compatibility)* |

---

#### Step 5 — Name & Storage Path

<img width="841" height="589" alt="vmware-name" src="https://github.com/user-attachments/assets/5fcf6d79-262c-4e30-a6f7-5ef0b69fc8f9" />

| Setting | Value |
|---------|-------|
| VM Name | `Project Mursad` |
| Location | *(path with sufficient free space)* |

---

#### Step 6 — Disk Allocation

<img width="842" height="589" alt="vmware-disk" src="https://github.com/user-attachments/assets/ea02e56c-d17f-46c8-a2db-5968fdd4be4d" />

| Setting | Value |
|---------|-------|
| Maximum Disk Size | `50.0 GB` |
| Storage Mode | Split virtual disk into multiple files |

---

#### Step 7 — Hardware Summary & Power On

<img width="835" height="588" alt="vmware-summary" src="https://github.com/user-attachments/assets/f173782e-2f3f-49ce-b098-e9a12173d976" />
<img width="836" height="581" alt="vmware-poweron" src="https://github.com/user-attachments/assets/060645ad-cf64-4aa7-abb4-c02bb7527544" />

Review the summary and click **Finish**:

| Resource | Minimum |
|----------|:-------:|
| RAM | 4 GB (4096 MB) |
| CPU Cores | 2 |
| Disk | 50 GB |

Click **"Power on this virtual machine"** to begin OS installation.

---

### Part B — Proxmox OS Installation

#### Step 8 — Boot Menu

<img width="1227" height="928" alt="proxmox-boot1" src="https://github.com/user-attachments/assets/d1fa235c-1821-46c9-8ddc-4aab6477d786" />
<img width="1473" height="960" alt="proxmox-boot2" src="https://github.com/user-attachments/assets/19e1346d-be49-4302-b5e2-7fd7f0e8d6b8" />

Select with arrow keys:

```
Install Proxmox VE (Graphical)
```

> ⚠️ **Nested Virtualization Warning**
>
> <img width="1478" height="931" alt="kvm-warning" src="https://github.com/user-attachments/assets/3c189837-43ed-440d-aa27-60ad447042ad" />
>
> If you receive a *"No support for hardware-accelerated KVM"* error, verify **"Virtualize AMD-V/RVI"** is enabled under VMware Processor settings. If the issue persists on Windows 11, Credential Guard / VBS may be blocking AMD-V passthrough — run Microsoft's `DG_Readiness_Tool_v3.6.ps1 -Disable` and reboot.

---

#### Step 9 — EULA

<img width="1483" height="905" alt="proxmox-eula" src="https://github.com/user-attachments/assets/7b11544f-f915-46fd-9884-8d790fba64f0" />

Click **"I agree"** to proceed.

---

#### Step 10 — Target Disk

<img width="1269" height="791" alt="proxmox-disk" src="https://github.com/user-attachments/assets/6716b425-5a3f-4d8c-8670-eb209eae3c5f" />

Select:

```
/dev/sda  —  50 GiB VMware Virtual Disk
```

---

#### Step 11 — Localization

<img width="1268" height="797" alt="proxmox-locale" src="https://github.com/user-attachments/assets/89429566-d436-47dd-9503-eb06446dd6b2" />

| Setting | Value |
|---------|-------|
| Country | Bahrain |
| Time Zone | Asia/Bahrain |
| Keyboard Layout | U.S. English |

---

#### Step 12 — Root Password & Email

<img width="1274" height="799" alt="proxmox-password" src="https://github.com/user-attachments/assets/f138431e-6bd9-4fae-be67-b7737c5d0c6c" />

Set a strong password for the `root` account and provide an email for system alerts.

---

#### Step 13 — Management Network

<img width="1268" height="793" alt="proxmox-network" src="https://github.com/user-attachments/assets/b508270e-e6a3-483e-8ed4-cf06687c5c72" />

| Parameter | Value |
|-----------|-------|
| Hostname (FQDN) | `mursad.me` |
| IP Address / CIDR | `192.168.140.129/24` |
| Gateway | `192.168.140.2` |
| DNS Server | `192.168.140.2` |

---

#### Step 14 — Install & Reboot

<img width="1271" height="793" alt="proxmox-install" src="https://github.com/user-attachments/assets/a50801af-ad88-49b7-9679-a902339a9122" />
<img width="1274" height="826" alt="proxmox-reboot" src="https://github.com/user-attachments/assets/23d36733-fd11-49ae-9e55-3d6b4ec3fd6f" />

Check **"Automatically reboot after successful installation"** → click **Install**.

After reboot, the CLI console displays:

```
https://192.168.140.129:8006/
```

> 📋 Save this URL — primary access point for all Proxmox management.

---

### Part C — Web GUI & Network Engineering

#### Step 15 — Login to Web UI

<img width="1910" height="937" alt="proxmox-login1" src="https://github.com/user-attachments/assets/048302eb-2f9e-43e3-b356-c8a231eb1d55" />
<img width="1911" height="938" alt="proxmox-login2" src="https://github.com/user-attachments/assets/bd1ffa4c-1ca1-4c3e-8386-7b16338a63e6" />

Navigate to `https://192.168.140.129:8006/`

| Field | Value |
|-------|-------|
| User name | `root` |
| Password | *(configured root password)* |
| Realm | `Linux PAM standard authentication` |

---

#### Step 16 — Create All SOC Bridges

<img width="1917" height="942" alt="bridges-create" src="https://github.com/user-attachments/assets/82639e21-95ed-4d53-9090-90aa7e5be0bb" />
<img width="634" height="281" alt="bridges-dialog" src="https://github.com/user-attachments/assets/6648ee3c-91c5-4450-bf1b-a11dcd7e55bb" />

Navigate to **mursad → System → Network → Create → Linux Bridge**.

Repeat for each bridge below:

| Bridge | CIDR | Autostart | Comment | Role |
|:------:|------|:---------:|---------|------|
| `vmbr0` | `192.168.140.129/24` | ✅ | — | Management / NAT uplink via `nic0` |
| `vmbr1` | `10.22.0.1/24` | ✅ | `HR` | HR Workstation segment |
| `vmbr2` | `10.22.1.1/24` | ✅ | `IT` | IT Workstation segment |
| `vmbr3` | `10.22.2.1/24` | ✅ | `OPs` | OPs Workstation segment |
| `vmbr4` | `192.168.50.1/24` | ✅ | `DMZ` | Demilitarized Zone |
| `vmbr5` | `10.22.7.1/24` | ✅ | `Servers` | Internal servers segment |

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
+    address 10.22.1.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr3
+iface vmbr3 inet static
+    address 10.22.2.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr4
+iface vmbr4 inet static
+    address 192.168.50.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
+
+auto vmbr5
+iface vmbr5 inet static
+    address 10.22.7.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
```

---

#### Step 17 — Verify Final Network State

<img width="1601" height="687" alt="bridges-verify1" src="https://github.com/user-attachments/assets/3fdd4993-4f7a-41d5-8672-14a15c35f35d" />
<img width="1610" height="698" alt="bridges-verify2" src="https://github.com/user-attachments/assets/2b9c6b02-c07e-48db-a001-3a26f7a0ccf3" />

| Bridge | Type | Active | Autostart | CIDR | Comment |
|:------:|------|:------:|:---------:|------|---------|
| `nic0` | Network Device | ✅ | — | — | Physical uplink `enp2s1` |
| `vmbr0` | Linux Bridge | ✅ | ✅ | `192.168.140.129/24` | Management / NAT |
| `vmbr1` | Linux Bridge | ✅ | ✅ | `10.22.0.1/24` | HR Workstation |
| `vmbr2` | Linux Bridge | — | ✅ | `10.22.1.1/24` | IT Workstation |
| `vmbr3` | Linux Bridge | — | ✅ | `10.22.2.1/24` | OPs Workstation |
| `vmbr4` | Linux Bridge | — | ✅ | `192.168.50.1/24` | DMZ |
| `vmbr5` | Linux Bridge | — | ✅ | `10.22.7.1/24` | Servers |

```
Proxmox Node: mursad
│
├── vmbr0  ──►  Management / NAT     192.168.140.129/24   (nic0 uplink)
├── vmbr1  ──►  HR Workstation       10.22.0.1/24
├── vmbr2  ──►  IT Workstation       10.22.1.1/24
├── vmbr3  ──►  OPs Workstation      10.22.2.1/24
├── vmbr4  ──►  DMZ Zone             192.168.50.1/24
└── vmbr5  ──►  Servers Segment      10.22.7.1/24
```

---

### ✅ Phase Checklist

- [ ] Proxmox VE 9.1 installed and booting correctly
- [ ] Web GUI accessible at `https://192.168.140.129:8006/`
- [ ] `vmbr0` active — IP `192.168.140.129/24`, gateway `192.168.140.2`
- [ ] `vmbr1` created — `10.22.0.1/24`, comment `HR`, Autostart ✅
- [ ] `vmbr2` created — `10.22.1.1/24`, comment `IT`, Autostart ✅
- [ ] `vmbr3` created — `10.22.2.1/24`, comment `OPs`, Autostart ✅
- [ ] `vmbr4` created — `192.168.50.1/24`, comment `DMZ`, Autostart ✅
- [ ] `vmbr5` created — `10.22.7.1/24`, comment `Servers`, Autostart ✅
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
        ├── vtnet0  →  vmbr0  →  WAN    192.168.140.x/24  (DHCP from host)
        ├── vtnet1  →  vmbr1  →  LAN    10.22.0.1/24      (HR Workstation)
        ├── vtnet2  →  vmbr2  →  OPT1   10.22.1.1/24      (IT Workstation)
        ├── vtnet3  →  vmbr3  →  OPT2   10.22.2.1/24      (OPs Workstation)
        ├── vtnet4  →  vmbr4  →  OPT3   192.168.50.1/24   (DMZ)
        └── vtnet5  →  vmbr5  →  OPT4   10.22.7.1/24      (Servers)
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Provisioning | Create the VM, upload ISO, attach all NICs before first boot |
| **B** | OS Installation | Install pfSense CE onto the virtual disk |
| **C** | Interface & Routing | Map bridges, assign IPs, enable DHCP |

---

### Part A — Virtual Machine Provisioning

#### Step 1 — Download pfSense CE ISO

<img width="1222" height="887" alt="pfsense-download1" src="https://github.com/user-attachments/assets/3fb40940-64ec-4a72-b7c2-b4bdcbb50f0c" />
<img width="1111" height="776" alt="pfsense-download2" src="https://github.com/user-attachments/assets/a34523a6-f5f4-4924-8f3b-de108e33b15c" />

Navigate to [pfsense.org/download](https://www.pfsense.org/download/) and select:

| Setting | Value |
|---------|-------|
| Architecture | AMD64 (64-bit) |
| Image Type | **AMD64 ISO IPMI/Virtual Machines** |

> ⚠️ Download arrives as `.gz`. Extract to obtain the `.iso` before uploading to Proxmox.

---

#### Step 2 — Upload ISO to Proxmox

<img width="1051" height="738" alt="pfsense-upload1" src="https://github.com/user-attachments/assets/08d05f59-4370-45b7-b6f7-11b673e2335e" />
<img width="887" height="605" alt="pfsense-upload2" src="https://github.com/user-attachments/assets/76858e07-0a3a-4700-a28b-8951af9330c1" />
<img width="406" height="312" alt="pfsense-upload3" src="https://github.com/user-attachments/assets/ace26d91-a790-4c7b-8bbb-9d179aa0a95a" />
<img width="1911" height="537" alt="pfsense-upload4" src="https://github.com/user-attachments/assets/2ad9c755-c04d-4643-af6b-2a995e7d2235" />

1. Navigate to **mursad → local (mursad) → ISO Images**
2. Click **Upload → Select File** → browse to extracted `.iso`
3. Wait for task log:

```
TASK OK
```

---

#### Step 3 — Create the VM

<img width="460" height="472" alt="pfsense-createvm" src="https://github.com/user-attachments/assets/d874199a-96e4-45e3-8bdd-e974653b0c3d" />

Click **"Create VM"** in the top-right corner.

---

#### Step 4 — General Tab

<img width="721" height="534" alt="pfsense-general" src="https://github.com/user-attachments/assets/2d20dc43-a1bc-43ab-9b7e-fb4c624bc64a" />

| Setting | Value |
|---------|-------|
| VM ID | `100` |
| Name | `Firewall` |
| Node | `mursad` |

---

#### Step 5 — OS Tab

<img width="788" height="539" alt="pfsense-os" src="https://github.com/user-attachments/assets/eca6d83d-692b-4b37-b0bf-d6f696bd12d5" />

| Setting | Value |
|---------|-------|
| Storage | `local (mursad)` |
| ISO Image | pfSense CE ISO |
| Guest OS Type | Other |

---

#### Step 6 — System Tab

<img width="716" height="537" alt="pfsense-system" src="https://github.com/user-attachments/assets/4dd7f1b1-b566-4a4e-ba26-63516c470873" />

| Setting | Value |
|---------|-------|
| BIOS | SeaBIOS |
| Machine | i440fx |
| SCSI Controller | VirtIO SCSI |

> Leave defaults — pfSense CE does not require OVMF/UEFI.

---

#### Step 7 — Disks Tab

<img width="719" height="539" alt="pfsense-disks" src="https://github.com/user-attachments/assets/da1a221c-1442-4245-af10-4d7f1bef1445" />

| Setting | Value |
|---------|-------|
| Storage | `local-lvm` |
| Disk Size | `32 GiB` |
| Format | raw |

---

#### Step 8 — CPU Tab

<img width="722" height="534" alt="pfsense-cpu" src="https://github.com/user-attachments/assets/6efdcca8-1312-4ba1-9b8a-2321629bd46b" />

| Setting | Value |
|---------|-------|
| Cores | `2` |
| Type | `host` |

> ⚠️ **Critical:** CPU type `host` passes AMD Ryzen flags directly into the VM. Without this, pfSense/FreeBSD will fail to boot stably under nested virtualization.

---

#### Step 9 — Memory Tab

<img width="722" height="539" alt="pfsense-memory" src="https://github.com/user-attachments/assets/62db1e06-74a7-4d0c-9995-95a1bcdb7812" />

| Setting | Value |
|---------|-------|
| Memory | `4096 MiB (4 GB)` |

> 4 GB provides headroom for pfSense plus Suricata IDS/IPS when installed.

---

#### Step 10 — Network Tab

<img width="721" height="540" alt="pfsense-nic" src="https://github.com/user-attachments/assets/00513ee8-acbf-4af8-8fe6-4d58835ed1a2" />

| Setting | Value |
|---------|-------|
| Bridge | `vmbr0` |
| Model | `VirtIO (paravirtualized)` |
| Firewall | Unchecked |

> WAN interface only. Remaining bridges are added in Step 12 — **before first boot**.

---

#### Step 11 — Confirm & Finish

<img width="718" height="538" alt="pfsense-finish" src="https://github.com/user-attachments/assets/a5e3caca-e066-4773-96b7-c52a9ec0930c" />

Review and click **Finish**. Do **not** check "Start after created".

---

#### Step 12 — Add Remaining Network Interfaces

> ⚠️ **Complete this before first boot.** All six NICs must exist when pfSense first boots so FreeBSD can detect and enumerate them during setup.

Navigate to **VM 100 → Hardware → Add → Network Device**:

| NIC | Bridge | Model | Zone |
|:---:|:------:|:-----:|------|
| net1 | `vmbr1` | VirtIO | HR Workstation |
| net2 | `vmbr2` | VirtIO | IT Workstation |
| net3 | `vmbr3` | VirtIO | OPs Workstation |
| net4 | `vmbr4` | VirtIO | DMZ |
| net5 | `vmbr5` | VirtIO | Servers |

---

### Part B — pfSense OS Installation

#### Step 13 — Boot & Launch Console

<img width="1914" height="743" alt="pfsense-boot1" src="https://github.com/user-attachments/assets/209f7861-d692-491a-bf8c-27fc7571d6f1" />
<img width="1826" height="1021" alt="pfsense-boot2" src="https://github.com/user-attachments/assets/f51641f6-ebbd-4457-b4b4-12f920a0d8e3" />

Select **VM 100 → Start → Console**. Allow bootloader timer to expire or press **Enter**.

---

#### Step 14 — Accept License

<img width="1832" height="1008" alt="pfsense-license" src="https://github.com/user-attachments/assets/aa7419e4-6074-4384-86de-c4acf363d09a" />

Accept the Netgate Copyright and Trademark notice.

---

#### Step 15 — Select Install

<img width="1445" height="767" alt="pfsense-install" src="https://github.com/user-attachments/assets/d4a80dcc-f2a1-4b68-9ad6-c1cfb2795730" />

Select **Install pfSense** from the welcome menu.

---

#### Step 16 — Partition Scheme

<img width="1812" height="1009" alt="pfsense-zfs" src="https://github.com/user-attachments/assets/a9cf0420-c040-4159-8c74-597dd4eaaf26" />

Select **Auto (ZFS)** — provides data integrity, snapshot support, and is ideal for firewall appliances. Accept defaults and select the virtual disk when prompted.

---

### Part C — Interface Assignment & Routing

#### Step 17 — VLAN Setup

<img width="1830" height="1012" alt="pfsense-vlan" src="https://github.com/user-attachments/assets/62497691-b1de-4e89-a0cc-27a1de8848f3" />

```
Should VLANs be set up now? [y/n]  →  n
```

Segmentation is handled by separate Proxmox bridges — VLANs are not required.

---

#### Step 18 — Assign Interfaces

<img width="1823" height="976" alt="pfsense-assign1" src="https://github.com/user-attachments/assets/71ffea5d-b1d2-4d8a-abbc-9df6d232f0c8" />
<img width="1818" height="1002" alt="pfsense-assign2" src="https://github.com/user-attachments/assets/bfe16e14-bfe2-437a-a141-281c20c39efc" />
<img width="1827" height="1006" alt="pfsense-assign3" src="https://github.com/user-attachments/assets/557a26d7-1b4c-4c35-8deb-995254bddfcf" />
<img width="1832" height="1005" alt="pfsense-assign4" src="https://github.com/user-attachments/assets/c4499423-f730-413b-853a-7d0d643d47d1" />

| Prompt | Input | Maps To |
|--------|:-----:|---------|
| WAN interface | `vtnet0` | `vmbr0` — Internet |
| LAN interface | `vtnet1` | `vmbr1` — HR Workstation |
| OPT1 interface | `vtnet2` | `vmbr2` — IT Workstation |
| OPT2 interface | `vtnet3` | `vmbr3` — OPs Workstation |
| OPT3 interface | `vtnet4` | `vmbr4` — DMZ |
| OPT4 interface | `vtnet5` | `vmbr5` — Servers |

Type `y` → Enter to confirm.

---

#### Step 19 — Verify Boot & WAN DHCP

<img width="1820" height="1006" alt="pfsense-wan1" src="https://github.com/user-attachments/assets/dc6b95ec-adc2-476d-9330-236e0ec4e277" />
<img width="1820" height="1003" alt="pfsense-wan2" src="https://github.com/user-attachments/assets/439cfa86-0660-44a4-b7bc-c3d07eeecbcb" />

```
WAN (vtnet0) →  192.168.140.xxx/24   ✅  DHCP from Proxmox host
LAN (vtnet1) →  no IP yet            ←   configure next
```

---

#### Step 20 — Set LAN IP Address

<img width="1829" height="1012" alt="pfsense-lan1" src="https://github.com/user-attachments/assets/be136960-8327-4237-8582-bc752d7a646a" />
<img width="1914" height="989" alt="pfsense-lan2" src="https://github.com/user-attachments/assets/359a4de9-de78-40d7-acd3-016d239679d1" />

Console menu → type `2` → Enter → select `2` for **LAN**.

---

#### Step 21 — Configure LAN

<img width="1203" height="660" alt="pfsense-lan3" src="https://github.com/user-attachments/assets/4835f9b7-2b7a-4cee-bc91-233205219248" />
<img width="1137" height="654" alt="pfsense-lan4" src="https://github.com/user-attachments/assets/837976e1-3106-4774-ba8d-91957acb0fb1" />
<img width="1177" height="404" alt="pfsense-lan5" src="https://github.com/user-attachments/assets/e2307588-7ea4-4810-a4fc-6d59cd16dd8a" />

| Prompt | Value |
|--------|-------|
| IPv4 Address | `10.22.0.1` |
| Subnet bit count | `24` |
| Upstream gateway | *(Enter — none)* |
| IPv6 address | *(Enter — skip)* |

---

#### Step 22 — Enable DHCP on LAN

<img width="1163" height="556" alt="pfsense-dhcp1" src="https://github.com/user-attachments/assets/d0705f08-fa84-4cf5-811b-f373c31dec22" />
<img width="1151" height="186" alt="pfsense-dhcp2" src="https://github.com/user-attachments/assets/7eea0b2c-b401-4fdf-9801-00769d530a62" />
<img width="1175" height="388" alt="pfsense-dhcp3" src="https://github.com/user-attachments/assets/a47bfb2f-c184-40a0-a1e6-80a075cbddef" />
<img width="1176" height="267" alt="pfsense-dhcp4" src="https://github.com/user-attachments/assets/19c5ba61-d30e-42b4-827a-10ca312bc302" />

Enable DHCP server: `y`

| Prompt | Value |
|--------|-------|
| DHCP range start | `10.22.0.100` |
| DHCP range end | `10.22.0.200` |

---

#### Step 23 — Confirm & Access WebConfigurator

<img width="1169" height="716" alt="pfsense-confirm1" src="https://github.com/user-attachments/assets/3f21233a-6fc6-4268-a484-15ed81cceeda" />
<img width="1173" height="879" alt="pfsense-confirm2" src="https://github.com/user-attachments/assets/3acda81b-24a7-4aed-98ef-54e76eb81e0d" />

Revert to HTTP: `n` — keep HTTPS.

```
LAN (vtnet1) →  10.22.0.1/24  ✅
```

Access the WebConfigurator from any HR-segment machine:

```
https://10.22.0.1
```

| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `pfsense` |

> 🔐 **Change the default password immediately upon first login.**

---

### Interface Summary

| pfSense | vtnet | Bridge | IP | Zone |
|:-------:|:-----:|:------:|:--:|:----:|
| WAN | vtnet0 | vmbr0 | DHCP `192.168.140.x` | Internet |
| LAN | vtnet1 | vmbr1 | `10.22.0.1/24` | HR Workstation |
| OPT1 | vtnet2 | vmbr2 | `10.22.1.1/24` | IT Workstation *(configured in `[04]`)* |
| OPT2 | vtnet3 | vmbr3 | `10.22.2.1/24` | OPs Workstation *(configured in `[04]`)* |
| OPT3 | vtnet4 | vmbr4 | `192.168.50.1/24` | DMZ *(configured in `[04]`)* |
| OPT4 | vtnet5 | vmbr5 | `10.22.7.1/24` | Servers *(configured in `[04]`)* |

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

> **Scope:** Attaching all five VLAN bridges to the pfSense VM, provisioning and naming every interface (HR · IT · OPs · DMZ · SERVERS), configuring Hybrid Outbound NAT, and establishing the baseline WAN firewall rule that brings the full Mursad SOC network online.

---

### Overview

```
Proxmox Node: mursad
└── VM 100  —  Firewall  (pfSense CE)
        ├── vtnet0  →  vmbr0  →  WAN      192.168.140.x/24  (DHCP)
        ├── vtnet1  →  vmbr1  →  HR       10.22.0.1/24
        ├── vtnet2  →  vmbr2  →  IT       10.22.1.1/24
        ├── vtnet3  →  vmbr3  →  OPs      10.22.2.1/24
        ├── vtnet4  →  vmbr4  →  DMZ      192.168.50.1/24
        └── vtnet5  →  vmbr5  →  SERVERS  10.22.7.1/24
```

| Part | Section | Description |
|:----:|---------|-------------|
| **A** | VM Network Provisioning | Attach all VLAN bridges to the Firewall VM in Proxmox |
| **B** | Firewall Rules | WAN baseline rule for management access |
| **C** | Outbound NAT | Switch to Hybrid mode, add manual mappings per zone |
| **D** | Interface Assignment | Enable, name, and address all OPT interfaces in pfSense |

---

### Part A — Virtual Machine Network Provisioning

> *Before touching pfSense, all bridges must exist as virtual NICs on the Firewall VM so FreeBSD can enumerate them on boot.*

#### Step 1 — Review Proxmox Network Bridges

<img width="1448" height="406" alt="1" src="https://github.com/user-attachments/assets/79a53dca-d936-4bef-8485-b94ddabf875c" />

Confirm all five internal bridges are present on the **mursad** node before proceeding:

| Bridge | Zone | Subnet |
|:------:|------|--------|
| `vmbr1` | HR Workstation | `10.22.0.0/24` |
| `vmbr2` | IT Workstation | `10.22.1.0/24` |
| `vmbr3` | OPs Workstation | `10.22.2.0/24` |
| `vmbr4` | DMZ | `192.168.50.0/24` |
| `vmbr5` | Servers | `10.22.7.0/24` |

---

#### Step 2 — Add Network Devices to Firewall VM

<img width="1237" height="631" alt="2" src="https://github.com/user-attachments/assets/935bbc5e-6e3f-41d6-ac35-6cd053913779" />

Navigate to **VM 100 (Firewall) → Hardware → Add → Network Device**.

---

#### Step 3 — Assign All Bridges to Firewall

<img width="647" height="287" alt="3" src="https://github.com/user-attachments/assets/848bfd3b-462b-4510-8453-9c54bcb0e616" />

Add one **VirtIO** Network Device per bridge:

| NIC | Bridge | Model |
|:---:|:------:|:-----:|
| net1 | `vmbr1` | VirtIO |
| net2 | `vmbr2` | VirtIO |
| net3 | `vmbr3` | VirtIO |
| net4 | `vmbr4` | VirtIO |
| net5 | `vmbr5` | VirtIO |

> ⚠️ All NICs must be added **before** booting the VM. If pfSense is already running, shut it down first, add all devices, then start it again.

---

#### Step 4 — Verify Firewall Hardware

<img width="1008" height="438" alt="4" src="https://github.com/user-attachments/assets/231b245c-5a4d-422c-9c0f-50aa714802ac" />

Open **VM 100 → Hardware** and confirm the full NIC list:

```
net0  →  vmbr0   WAN
net1  →  vmbr1   HR
net2  →  vmbr2   IT
net3  →  vmbr3   OPs
net4  →  vmbr4   DMZ
net5  →  vmbr5   SERVERS
```

---

### Part B — pfSense Firewall Rules

> *pfSense blocks all traffic by default on new interfaces. The WAN rule below enables management access to the firewall itself from the upstream network.*

#### Step 5 — Navigate to Firewall Rules

<img width="1912" height="936" alt="5" src="https://github.com/user-attachments/assets/4b8208c4-9cfd-4aad-ac4f-044105a94d40" />

Log into the pfSense WebConfigurator at `https://10.22.0.1` and navigate to **Firewall → Rules**.

---

#### Step 6 — Add WAN Firewall Rule

<img width="1171" height="570" alt="6" src="https://github.com/user-attachments/assets/5c5f0b73-8a0d-4628-9f88-aff4b2693c6b" />

Select the **WAN** tab. Click **Add ↑** to insert a new rule at the top of the list.

---

#### Step 7 — Configure WAN Rule Parameters

<img width="1167" height="917" alt="7" src="https://github.com/user-attachments/assets/85dd7330-3c39-41ad-920f-47c341e9414b" />

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

<img width="1209" height="595" alt="8" src="https://github.com/user-attachments/assets/65da654e-8ade-4cb0-ab67-e55986c8495c" />

The new rule appears in the WAN tab. Click **Apply Changes** to activate it.

---

### Part C — Outbound NAT Configuration

> *Automatic NAT works for basic setups but gives no per-zone control. Switching to **Hybrid** mode keeps the automatic rules intact while letting us add manual mappings for each subnet.*

#### Step 9 — Navigate to Outbound NAT

<img width="1203" height="580" alt="9" src="https://github.com/user-attachments/assets/e91faaf1-f89a-4cfb-bb04-7c962e351528" />

Navigate to **Firewall → NAT → Outbound**.

---

#### Step 10 — Enable Hybrid Outbound NAT

<img width="1216" height="665" alt="10" src="https://github.com/user-attachments/assets/c87dd688-e179-4008-9a99-eeda1dfc95ab" />

Under **Outbound NAT Mode**, select **Hybrid Outbound NAT rule generation**.

> ℹ️ Hybrid keeps automatic rules for internet access while allowing custom mappings per zone. This is the correct mode for a segmented lab — pure Manual mode would break internet access until every subnet is manually mapped.

Click **Save**.

---

#### Step 11 — Add HR Workstation NAT Mapping

<img width="1184" height="652" alt="11" src="https://github.com/user-attachments/assets/d7a44a09-c4b0-4665-9a80-e5d23eb127b9" />

Click **Add ↑** to create the first manual mapping.

| Field | Value |
|-------|-------|
| Interface | WAN |
| Address Family | IPv4 |
| Protocol | Any |
| Source | Network or Alias |
| Source Network | `10.22.0.0` / `24` |
| Destination | Any |
| Description | `Manual NAT — HR Workstation` |

Click **Save**.

---

#### Step 12 — Add SERVERS NAT Mapping

<img width="1175" height="685" alt="12" src="https://github.com/user-attachments/assets/0c56f1ee-9ae1-471b-872d-edae3b01707d" />

| Field | Value |
|-------|-------|
| Interface | WAN |
| Address Family | IPv4 |
| Protocol | Any |
| Source | Network or Alias |
| Source Network | `10.22.7.0` / `24` |
| Destination | Any |
| Description | `Manual NAT — SERVERS` |

Click **Save**.

---

#### Step 13 — Add DMZ NAT Mapping

<img width="1178" height="805" alt="13" src="https://github.com/user-attachments/assets/706da33c-17d1-482a-bd93-bd1ca0df8079" />

DMZ is restricted to TCP/UDP only — not raw IP or ICMP:

| Field | Value |
|-------|-------|
| Interface | WAN |
| Address Family | IPv4 |
| Protocol | **TCP/UDP** |
| Source | Network or Alias |
| Source Network | `192.168.50.0` / `24` |
| Destination | Any |
| Description | `Manual NAT — DMZ` |

Click **Save**.

---

#### Step 14 — Review & Apply NAT Mappings

<img width="1196" height="386" alt="14" src="https://github.com/user-attachments/assets/9f783a64-5251-45ce-a3c6-ad695d83c788" />

Confirm three manual mappings appear at the top of the list:

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

> *The OPT interfaces exist as raw detected ports — each must be enabled, named, and given a static IP before pfSense routes any traffic through them.*

#### Step 15 — Navigate to Interface Assignments

<img width="1155" height="243" alt="15" src="https://github.com/user-attachments/assets/bea7d50d-9ad1-4f11-a5ea-1addcca3548f" />

Navigate to **Interfaces → Assignments**.

---

#### Step 16 — Configure HR Interface (OPT1)

<img width="1164" height="289" alt="16" src="https://github.com/user-attachments/assets/9503ea2e-0529-47af-915d-3e13c26062dc" />

Click **OPT1** and configure:

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `HR` |
| IPv4 Configuration Type | Static IPv4 |
| IPv4 Address | `10.22.0.1` / `24` |

Click **Save** → **Apply Changes**.

---

#### Step 17 — Configure IT Interface (OPT2)

<img width="1183" height="937" alt="17" src="https://github.com/user-attachments/assets/5550c802-da20-419e-b2cd-60342c7a985e" />

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `IT` |
| IPv4 Configuration Type | Static IPv4 |
| IPv4 Address | `10.22.1.1` / `24` |

Click **Save** → **Apply Changes**.

---

#### Step 18 — Configure OPs Interface (OPT3)

<img width="1174" height="544" alt="18" src="https://github.com/user-attachments/assets/17493441-dd4b-403b-8698-e936ad8bbd39" />

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `OPs` |
| IPv4 Configuration Type | Static IPv4 |
| IPv4 Address | `10.22.2.1` / `24` |

Click **Save** → **Apply Changes**.

---

#### Step 19 — Configure DMZ Interface (OPT4)

<img width="1166" height="676" alt="19" src="https://github.com/user-attachments/assets/c75ef365-6623-4919-98d6-4b96fa14326c" />

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `DMZ` |
| IPv4 Configuration Type | Static IPv4 |
| IPv4 Address | `192.168.50.1` / `24` |

Click **Save** → **Apply Changes**.

---

#### Step 20 — Configure SERVERS Interface (OPT5)

<img width="1197" height="756" alt="20" src="https://github.com/user-attachments/assets/07a17413-37b3-4b86-a4bf-57e5a8d0a87d" />

| Field | Value |
|-------|-------|
| Enable | ✅ Checked |
| Description | `SERVERS` |
| IPv4 Configuration Type | Static IPv4 |
| IPv4 Address | `10.22.7.1` / `24` |

Click **Save** → **Apply Changes**.

---

#### Step 21 — Verify Complete Interface Table

<img width="1158" height="786" alt="21" src="https://github.com/user-attachments/assets/548fc934-c9b2-4907-874e-ada079721918" />

Navigate to **Interfaces → Assignments** and confirm:

| Interface | vtnet | Bridge | Description | IPv4 Address |
|:---------:|:-----:|:------:|-------------|:------------:|
| WAN | vtnet0 | vmbr0 | INTERNET | DHCP |
| LAN | vtnet1 | vmbr1 | HR | `10.22.0.1/24` |
| OPT1 | vtnet2 | vmbr2 | IT | `10.22.1.1/24` |
| OPT2 | vtnet3 | vmbr3 | OPs | `10.22.2.1/24` |
| OPT3 | vtnet4 | vmbr4 | DMZ | `192.168.50.1/24` |
| OPT4 | vtnet5 | vmbr5 | SERVERS | `10.22.7.1/24` |

---

#### Step 22 — Review pfSense Dashboard

<img width="1908" height="931" alt="22" src="https://github.com/user-attachments/assets/cef2f0bf-4ec0-4076-a112-4c267b57eadb" />

Click the pfSense logo to return to the dashboard. The **Interfaces** widget confirms all six zones active:

```
WAN      →  192.168.140.x/24   ✅
HR       →  10.22.0.1/24       ✅
IT       →  10.22.1.1/24       ✅
OPs      →  10.22.2.1/24       ✅
DMZ      →  192.168.50.1/24    ✅
SERVERS  →  10.22.7.1/24       ✅
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
- [ ] OPT1 enabled as `HR` — `10.22.0.1/24`
- [ ] OPT2 enabled as `IT` — `10.22.1.1/24`
- [ ] OPT3 enabled as `OPs` — `10.22.2.1/24`
- [ ] OPT4 enabled as `DMZ` — `192.168.50.1/24`
- [ ] OPT5 enabled as `SERVERS` — `10.22.7.1/24`
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
        ├── Network: vmbr5 (SERVERS)
        ├── Compute: 2 vCores · 8 GB RAM
        └── Storage: 60 GB Disk · VirtIO Drivers

pfSense WebConfigurator
└── DHCP Server
        ├── Interface: SERVERS
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

Before booting our Domain Controller, the SERVERS network needs a DHCP pool so the VM can acquire an IP address dynamically during installation. Navigate to **Services → DHCP Server** and select the **SERVERS** tab.

| Field | Value |
|-------|-------|
| Enable | ✅ Enable DHCP server on SERVERS interface |
| Range From | `10.22.7.3` |
| Range To | `10.22.7.50` |

> **Note:** The range starts at `.3` to reserve `.1` for the gateway and `.2` for the future Wazuh SIEM server.

Click **Save**.

<img width="1169" height="936" alt="1" src="https://github.com/user-attachments/assets/e1c72fb1-6cc6-46be-9f05-679a8db4b7b1" />

---

#### Step 2 — Switch to Kea DHCP Backend

Newer versions of pfSense display a deprecation warning for the legacy ISC DHCP server. To future-proof our SOC lab, we switch to the modern **Kea DHCP** engine. Navigate to **System → Advanced → Networking**, scroll to the **DHCP Backend** section, select **Kea DHCP**, and click **Save**.

<img width="1188" height="830" alt="2" src="https://github.com/user-attachments/assets/430e3f8c-1544-4d96-a049-0eaa81fdde6e" />

---

#### Step 3 — Configure DHCP DNS Servers

Navigate back to **Services → DHCP Server → SERVERS** and scroll down to the **Servers** section. Add the firewall's own IP (`10.22.7.1`) as the primary DNS server so machines in this zone can resolve internet hostnames before the Active Directory DNS role is installed.

Click **Save**.

<img width="1172" height="333" alt="3" src="https://github.com/user-attachments/assets/d66ea4da-1b6c-42b6-82fb-96f496f3a194" />

---

### Part B — Domain Controller VM Provisioning

> ⚠️ **Prerequisite:** Ensure you have downloaded the **Windows Server ISO** and the **VirtIO Windows Drivers ISO** and uploaded both to your Proxmox local storage before proceeding.

#### Step 4 — Create the DC Virtual Machine

In the Proxmox web interface, click **Create VM** in the top-right corner. Set the **VM ID** to `101` and name the machine `DC`. Click **Next**.

<img width="812" height="590" alt="4" src="https://github.com/user-attachments/assets/6933494a-cb94-4c1a-adcc-d2d2a43d5013" />

---

#### Step 5 — Select Guest OS

Under the **OS** tab, select your Proxmox local storage and choose the **Windows Server ISO**. Set the **Guest OS Type** to `Microsoft Windows` and manually select the version matching your ISO (e.g., `2019` or `2022`).

<img width="748" height="551" alt="5" src="https://github.com/user-attachments/assets/dfb357ad-56f0-4244-abd9-62e140d5261d" />

---

#### Step 6 — Disk Allocation

Under the **Disks** tab, set the disk size to **60 GB** — sufficient for the Windows OS, future Active Directory databases (NTDS), and system logs. Set the bus/device to **VirtIO Block** for optimal I/O performance.

<img width="750" height="549" alt="6" src="https://github.com/user-attachments/assets/d83792c9-56df-4e74-924f-b7a4761030ff" />

---

#### Step 7 — CPU Configuration

Under the **CPU** tab, set **Cores** to `2`. Change the **CPU Type** to `host` to pass the hypervisor's processor flags directly into the VM, ensuring maximum compatibility with nested virtualization.

> ⚠️ **Critical:** CPU type `host` passes the AMD Ryzen flags directly into the VM. Without this, Windows Server may fail to boot stably under nested virtualization.

<img width="749" height="549" alt="7" src="https://github.com/user-attachments/assets/f40f0339-1000-4b1b-bfdb-142c01cc9133" />

---

#### Step 8 — Memory (RAM) Allocation

Under the **Memory** tab, assign **8192 MiB (8 GB)** of RAM. This ensures the Windows GUI and Active Directory services run smoothly without bottlenecking.

<img width="744" height="550" alt="8" src="https://github.com/user-attachments/assets/08fcf3a0-b27c-4ce8-9bd6-faf3b7b16e05" />

---

#### Step 9 — Network Configuration

Under the **Network** tab, attach the VM to our segmented server network:

| Field | Value |
|-------|-------|
| Bridge | `vmbr5` (SERVERS) |
| Model | `VirtIO (paravirtualized)` |
| Firewall | Unchecked |

Review the summary and complete the VM creation. **Do not power it on yet.**

<img width="733" height="545" alt="9" src="https://github.com/user-attachments/assets/3f745984-85d4-4bea-a9c8-1e95c39da3bd" />

---

#### Step 10 — Attach VirtIO Drivers CD/DVD

Because we selected VirtIO for both the storage and network adapters, the Windows installer will not natively recognize either device. We must mount the drivers ISO before first boot.

Navigate to **VM 101 (DC) → Hardware → Add → CD/DVD Drive**, choose your local storage, and select the `virtio-win` ISO.

> ⚠️ **Note:** Make sure the VirtIO ISO is uploaded to Proxmox storage before this step.

<img width="1913" height="716" alt="10" src="https://github.com/user-attachments/assets/4f77de02-a2e4-4374-85c7-0ba72e7b339e" />

---

### Part C — Windows Server Installation

#### Step 11 — Power On and Start Setup

With both ISOs mounted, start the VM and open the **Console**. Press any key when prompted to boot from the CD/DVD and wait for the Windows Setup screen to load.

<img width="1088" height="810" alt="11" src="https://github.com/user-attachments/assets/a21a357c-0b44-40cc-818d-a838ed3c75c3" />

---

#### Step 12 — Install Now

Confirm your language, time, and keyboard preferences, then click **Install now**.

<img width="1090" height="805" alt="12" src="https://github.com/user-attachments/assets/36aac1cb-a7ef-4f04-a034-f24d739472b9" />

---

#### Step 13 — Select Desktop Experience

When prompted to select the OS version, choose **Windows Server (Desktop Experience)**.

> **Note:** Skipping the "Desktop Experience" option installs **Server Core** — a CLI-only environment with no GUI. Always select Desktop Experience for this lab.

<img width="1090" height="810" alt="13" src="https://github.com/user-attachments/assets/b0f110a8-ee0b-454d-a5b7-2a6e15443505" />

---

#### Step 14 — Custom Installation

Accept the license terms. When asked for the installation type, select **Custom: Install Windows only (advanced)**. We are performing a clean installation, not an upgrade.

<img width="1089" height="814" alt="14" src="https://github.com/user-attachments/assets/aac7bd1c-d9d8-4bff-beb8-45e80574f6ff" />

---

#### Step 15 — Select the VirtIO Storage Drive

You should see **Drive 0** with 60 GB of unallocated space. Select it and click **Next** to begin writing the OS to disk.

> **Note:** If the drive does not appear, click **Load driver** and browse to the `viostor` folder on the mounted VirtIO CD.

<img width="1082" height="808" alt="15" src="https://github.com/user-attachments/assets/d5711040-6d6c-4a7c-8a60-c95f62994466" />

---

#### Step 16 — Set Local Administrator Password

After several automated reboots, Windows will prompt you to set a password. Enter a strong password for the built-in **Local Administrator** account and click **Finish**.

<img width="1090" height="813" alt="16" src="https://github.com/user-attachments/assets/32c96d8b-25b6-4d98-91cd-1d1748bc5c89" />

---

#### Step 17 — Welcome to Windows Server

Press **Ctrl+Alt+Delete** (via the Proxmox console flyout) to log in. You will land on the Windows Server desktop with **Server Manager** launching automatically.

<img width="1081" height="813" alt="17" src="https://github.com/user-attachments/assets/bc4810d4-3fc0-4a84-a967-35e133cefeb0" />

---

### Part D — Drivers & Hostname Configuration

#### Step 18 — Run the VirtIO Installer

Because we used the VirtIO network adapter, the VM currently has no network connectivity. Open **File Explorer**, navigate to the `virtio-win` CD Drive (typically `E:`), and double-click **`virtio-win-gt-x64`** to launch the driver installer.

<img width="1080" height="815" alt="18" src="https://github.com/user-attachments/assets/8ac08a49-c106-4cb4-aab6-f0c0adcc1131" />

---

#### Step 19 — Complete VirtIO Setup

Click **Next** through the standard setup prompts to install all paravirtualized drivers. Once complete, click **Finish**. The network adapter will initialize immediately in the background.

<img width="1090" height="816" alt="19" src="https://github.com/user-attachments/assets/4161063e-5034-4a45-b9d2-a6e0c43bc747" />

---

#### Step 20 — Verify DHCP Network Assignment

Open **PowerShell** from the Start menu and run:

```powershell
ipconfig
```

The Ethernet adapter should now show a lease from our pfSense Kea DHCP server — the first address in our pool: `10.22.7.3`.

<img width="1091" height="812" alt="20" src="https://github.com/user-attachments/assets/aee18131-69b3-4c13-8816-435004950e84" />

---

#### Step 21 — Access System Properties

Before promoting this server to a Domain Controller, we need to replace its auto-generated hostname. Search for **Control Panel**, navigate to **System and Security → System**, and click **Change settings** under the computer name section.

<img width="1088" height="812" alt="21" src="https://github.com/user-attachments/assets/11557706-1735-41f1-a6c0-9d3a32601f9f" />

---

#### Step 22 — Update Computer Description

In the **System Properties** window, set the Computer description to `DC`. Then click the **Change...** button to modify the actual NetBIOS computer name.

<img width="1083" height="814" alt="22" src="https://github.com/user-attachments/assets/55c7ec7c-55a3-4340-84c4-bcbdc3103b6a" />

---

#### Step 23 — Rename Computer to DC

Type `DC` into the **Computer name** field and click **OK → Apply**. Windows will prompt for a restart to apply the changes — click **Restart Now**.

<img width="1083" height="814" alt="23" src="https://github.com/user-attachments/assets/0e82c160-05ef-439b-a36d-e6864cad71ab" />

---

### Part E — Routing & Internet Verification

#### Step 24 — Identify Connectivity Issue

After the reboot, the network icon in the taskbar may show a yellow warning triangle (**No Internet Access**). DHCP is functioning correctly, but outbound traffic has no defined route to the WAN interface (`192.168.140.130`).

<img width="1089" height="807" alt="24" src="https://github.com/user-attachments/assets/34d6a443-a8d7-40a6-a0bc-8da79e903e1a" />

---

#### Step 25 — Add WAN Gateway in pfSense

Log back into the **pfSense WebConfigurator**. Navigate to **System → Routing → Gateways** and click the green **Add** button.

<img width="1235" height="670" alt="25" src="https://github.com/user-attachments/assets/518bca0a-3bc4-4cba-9b99-54792fd31ff3" />

---

#### Step 26 — Configure External Gateway

Set the **Default gateway IPv4** to point to your WAN interface. This tells pfSense where to forward all traffic that doesn't belong to the local `10.22.x.x` subnets.

<img width="1177" height="524" alt="26" src="https://github.com/user-attachments/assets/cd92867b-a884-480a-b145-40ea411aae0a" />

---

#### Step 27 — Update SERVERS Firewall Rules

Navigate to **Firewall → Rules → SERVERS** and create the following two rules:

| Rule | Action | Protocol | Source | Destination | Port | Purpose |
|------|--------|----------|--------|-------------|:----:|---------|
| 1 — DNS | Pass | IPv4 UDP | SERVERS net | This Firewall (self) | `53` | Allow DNS queries to pfSense |
| 2 — Outbound | Pass | IPv4 Any | SERVERS net | Any | Any | Allow outbound internet traffic |

Click **Apply Changes**.

<img width="1165" height="470" alt="27" src="https://github.com/user-attachments/assets/a5319eca-2b23-4b78-9b8d-459a8c6428bc" />

---

#### Step 28 — Restart Routing Services

If the connection is still not resolving after applying the rules, navigate to **Status → Services** in pfSense and restart both the `dpinger` (Gateway Monitoring) and `kea-dhcp4` services to force the new routing configuration to take effect immediately.

<img width="1183" height="477" alt="28" src="https://github.com/user-attachments/assets/e7e06cdb-b62a-4226-8182-df53679b8614" />

---

#### Step 29 — Ping Verification

Back on the Domain Controller, open **PowerShell** and run:

```powershell
ping 8.8.8.8
```

Successful ICMP replies confirm that traffic is routing correctly: `DC → SERVERS VLAN → pfSense → WAN → Internet`.

<img width="1084" height="816" alt="29" src="https://github.com/user-attachments/assets/154d5b80-fddd-4428-a271-1490a2952cc1" />

---

#### Step 30 — Browser Verification

Open **Microsoft Edge** on the Domain Controller and navigate to `google.com`. The page should load without issue, confirming full end-to-end connectivity.

The underlying infrastructure is now complete. The server is fully prepped to be promoted to an **Active Directory Domain Controller** in the next phase.

<img width="1074" height="814" alt="30" src="https://github.com/user-attachments/assets/35ec7491-c54a-4939-bb81-244641e46958" />

---

### VM Summary

| Component | vtnet | Bridge | IP | Zone |
|:---------:|:-----:|:------:|:--:|:----:|
| Windows Server DC | — | vmbr5 | `10.22.7.3` (DHCP) | SERVERS |
| pfSense — SERVERS | vtnet5 | vmbr5 | `10.22.7.1/24` | SERVERS gateway |

---

### ✅ Phase Checklist

- [ ] Windows Server ISO and VirtIO ISO uploaded to Proxmox local storage
- [ ] SERVERS DHCP pool enabled — range `10.22.7.3` to `10.22.7.50`
- [ ] DHCP backend switched from ISC to **Kea DHCP**
- [ ] DNS server `10.22.7.1` configured on SERVERS DHCP scope
- [ ] VM 101 created — `2 cores · 8 GB RAM · 60 GB VirtIO Block disk`
- [ ] Network bridge set to `vmbr5` (SERVERS), model VirtIO, firewall unchecked
- [ ] VirtIO drivers ISO attached as second CD/DVD drive before first boot
- [ ] Windows Server 2019 **Desktop Experience** installed via Custom install
- [ ] Local Administrator password set
- [ ] VirtIO drivers installed — `virtio-win-gt-x64`
- [ ] DHCP lease confirmed — `10.22.7.3` via `ipconfig`
- [ ] Computer renamed to `DC` and restarted
- [ ] WAN gateway added in pfSense — **System → Routing → Gateways**
- [ ] SERVERS firewall rules created — DNS (`UDP/53`) + outbound (`Any`)
- [ ] Routing services restarted if required — `dpinger` and `kea-dhcp4`
- [ ] Ping to `8.8.8.8` successful from DC
- [ ] Browser connectivity confirmed — `google.com` loads in Edge

<div align="center"><br>

**🟢 Phase Complete**

`[04] Advanced Firewall Routing` ◄── **`[05] Domain Controller Provisioning`** ──► `[06] Active Directory Domain Installation`

<br></div>

</details>

---

## 📁 Repository Structure

```
Project-Mursad/
│
├── 📂 assets/                        # Screenshots, diagrams, topology SVG
│   ├── diagrams/
│   │   └── mursad-topology.svg
│   ├── phase-1/
│   │   ├── 02-proxmox-deployment/
│   │   ├── 03-pfsense-installation/
│   │   └── 04-firewall-routing/
│   └── phase-2/  phase-3/  phase-4/  ...
│
├── 📂 configs/                       # Exported configs and rule sets
│   ├── pfsense/
│   ├── suricata/
│   ├── wazuh/
│   ├── active-directory/
│   └── windows/
│
├── 📂 docs/                          # Phase-by-phase deployment guides
│   ├── phase-1/
│   │   ├── 02-proxmox-deployment.md
│   │   ├── 03-pfsense-installation.md
│   │   └── 04-firewall-routing.md
│   └── phase-2/  phase-3/  phase-4/  ...
│
├── 📂 scripts/                       # Automation scripts
│   ├── ad-provisioning.ps1
│   ├── wazuh-agent-deploy.sh
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

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f3460,50:16213e,100:1a1a2e&height=120&section=footer&text=Built%20to%20understand%20the%20attack.%20Designed%20to%20defend%20against%20it.&fontSize=14&fontColor=a8b2d8&fontAlignY=65&animation=fadeIn" width="100%"/>

</div>
