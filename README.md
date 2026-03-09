<div align="center">

```
███╗   ███╗██╗   ██╗██████╗ ███████╗ █████╗ ██████╗
████╗ ████║██║   ██║██╔══██╗██╔════╝██╔══██╗██╔══██╗
██╔████╔██║██║   ██║██████╔╝███████╗███████║██║  ██║
██║╚██╔╝██║██║   ██║██╔══██╗╚════██║██╔══██║██║  ██║
██║ ╚═╝ ██║╚██████╔╝██║  ██║███████║██║  ██║██████╔╝
╚═╝     ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═════╝
```

**👁️ Enterprise Security Architecture & SOC Telemetry Lab**

<p align="center">
  <img src="https://img.shields.io/badge/Proxmox-E57000?style=for-the-badge&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/pfSense-000000?style=for-the-badge&logo=pfsense&logoColor=white"/>
  <img src="https://img.shields.io/badge/Active_Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white"/>
  <img src="https://img.shields.io/badge/Wazuh-6B4CFF?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/Suricata-EF6C00?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-DEPLOYMENT%20IN%20PROGRESS-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/Phase-1%20of%204-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Platform-Proxmox%20VE%209.1-E57000?style=flat-square"/>
  <img src="https://img.shields.io/badge/Domain-mursad.local-grey?style=flat-square"/>
</p>

> *A fully virtualized SOC environment on Proxmox featuring a segmented pfSense network,*
> *Active Directory, Wazuh SIEM, Suricata IDS/IPS, and isolated Red Team capability.*

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
  - [Phase 1 · 02 — Proxmox Deployment](#phase-1--02--proxmox-hypervisor-deployment--bridge-vlan-configuration)
  - [Phase 1 · 03 — pfSense Installation](#phase-1--03--pfsense-edge-firewall-installation--baseline-setup)
- [Repository Structure](#-repository-structure)
- [Disclaimer](#-disclaimer)

---

## 🔍 Overview

Project Mursad is a fully virtualized, enterprise-grade Security Operations Center lab designed for hands-on Blue Team and Red Team practice. Running entirely on a single Proxmox host nested inside VMware Workstation, it simulates a realistic corporate environment with proper zone segmentation, Active Directory identity management, inline intrusion detection, and centralized SIEM telemetry.

**Core objectives:**
- Deploy and harden a segmented enterprise network with pfSense
- Simulate adversary TTPs from an isolated Red Team machine
- Monitor endpoint and network behavior through Wazuh SIEM
- Validate detection with Suricata IDS/IPS signature testing
- Practice incident response in a realistic, controlled environment

---

## 🗺️ Architecture

```
╔══════════════════════════════════════════════════════════════════════╗
║              VMware Workstation Pro 17  ·  Windows 11 Host           ║
║  ┌─────────────────────────────────────────────────────────────────┐ ║
║  │                   Proxmox VE 9.1  —  mursad                     │ ║
║  │                                                                  │ ║
║  │   [ INTERNET / WAN ]  ◄──  vmbr0  ·  192.168.140.x/24          │ ║
║  │           │                                                      │ ║
║  │    ┌──────▼──────────────────────────────────────────────────┐  │ ║
║  │    │             VM 100  ·  pfSense CE  ·  Firewall           │  │ ║
║  │    │     vtnet0=WAN  vtnet1=LAN  vtnet2=OPT1  vtnet3=OPT2    │  │ ║
║  │    │              │               │              │            │  │ ║
║  │    │           [Suricata IDS/IPS inline]                      │  │ ║
║  │    └──────┬────────────────────────┬──────────────┬───────────┘  │ ║
║  │           │                        │              │              │ ║
║  │     vmbr1 · LAN              vmbr2 · OPT1   vmbr3 · OPT2        │ ║
║  │     10.22.0.0/24             10.22.7.0/24   192.168.50.0/24      │ ║
║  │     WORKSTATION              SERVERS        DMZ                  │ ║
║  │    ┌──────────────┐         ┌──────────┐   ┌──────────────┐     │ ║
║  │    │  DC · .0.2   │         │  Wazuh   │   │  DMZ Server  │     │ ║
║  │    │  mursad.local│         │  SIEM    │   │  .50.10      │     │ ║
║  │    │  IT WS · .0.3│         │  .7.2    │   └──────────────┘     │ ║
║  │    │  FIN WS · .0.4│        └──────────┘                        │ ║
║  │    └──────────────┘                                              │ ║
║  │                                                                  │ ║
║  │   [ Kali Linux ]  ◄──  WAN/DHCP  ·  External Red Team           │ ║
║  └─────────────────────────────────────────────────────────────────┘ ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 🌐 Network Zones

| Zone | Bridge | Subnet | Gateway | Purpose |
|------|--------|--------|---------|---------|
| WAN / Management | `vmbr0` | `192.168.140.0/24` | `192.168.140.2` | NAT uplink · Proxmox host access |
| Workstation | `vmbr1` | `10.22.0.0/24` | `10.22.0.1` | Domain-joined endpoints |
| Servers | `vmbr2` | `10.22.7.0/24` | `10.22.7.1` | Internal services · Wazuh SIEM |
| DMZ | `vmbr3` | `192.168.50.0/24` | `192.168.50.1` | Isolated public-facing services |

---

## 📡 IP Address Table

| Host | IP | Zone | Role |
|------|----|------|------|
| Proxmox Node | `192.168.140.129` | Management | Hypervisor — Web UI `:8006` |
| pfSense WAN | `192.168.140.x` (DHCP) | WAN | Internet uplink |
| pfSense LAN | `10.22.0.1` | Workstation | Default gateway |
| pfSense OPT1 | `10.22.7.1` | Servers | Servers gateway |
| pfSense OPT2 | `192.168.50.1` | DMZ | DMZ gateway |
| Windows Server DC | `10.22.0.2` | Workstation | AD · DNS · `mursad.local` |
| IT Workstation | `10.22.0.3` | Workstation | Domain-joined · Wazuh agent |
| Finance Workstation | `10.22.0.4` | Workstation | Domain-joined · Wazuh agent |
| Wazuh SIEM | `10.22.7.2` | Servers | Log aggregation · alerting |
| DMZ Server | `192.168.50.10` | DMZ | Public-facing services |
| Kali Linux | WAN DHCP | WAN | External Red Team attacker |

---

## 🧰 Tech Stack

| Component | Role | Version |
|-----------|------|---------|
| **Proxmox VE** | Type-1 hypervisor · bridge host | 9.1 |
| **pfSense CE** | Edge firewall · router · VPN | CE AMD64 |
| **Suricata** | Inline IDS/IPS via pfSense package | Latest |
| **Wazuh** | SIEM · XDR · log aggregation | 4.x |
| **Windows Server 2022** | Active Directory · DNS · AD CS | Eval |
| **Windows 10 Pro** | Domain endpoints — IT · Finance | Eval |
| **Kali Linux** | Red Team · penetration testing | Latest |

---

## 🗓️ Roadmap

### ▸ Phase 1 — Infrastructure & Perimeter Initialization
> Establishing the hypervisor environment and securing the network edge.

| # | Task | Status |
|---|------|:------:|
| `[01]` | Project Architecture & Introduction | ✅ |
| `[02]` | Proxmox Hypervisor Deployment & Bridge VLAN Engineering | ✅ |
| `[03]` | pfSense Edge Firewall Installation & Baseline Setup | ✅ |
| `[04]` | Advanced Firewall Routing & Network Configuration | 🔄 |

### ▸ Phase 2 — Enterprise Identity & Network Segmentation
> Building the corporate network, managing identities, isolating services.

| # | Task | Status |
|---|------|:------:|
| `[05]` | Domain Controller Provisioning & Network Integration | ⬜ |
| `[06]` | Active Directory Domain Installation & Configuration | ⬜ |
| `[07]` | DMZ Architecture Setup | ⬜ |
| `[08]` | LAN/DMZ Traffic Isolation & Secure Local DNS Mapping | ⬜ |

### ▸ Phase 3 — SOC Telemetry & Traffic Analysis
> Deploying the eyes and ears of the network.

| # | Task | Status |
|---|------|:------:|
| `[09]` | Suricata IDS/IPS Configuration | ⬜ |
| `[10]` | Wazuh SIEM Installation · Syslog Ingestion · Agent Rollout | ⬜ |

### ▸ Phase 4 — Endpoint Defense, Hardening & Validation
> Locking down endpoints and validating detection capability.

| # | Task | Status |
|---|------|:------:|
| `[11]` | Antivirus Integration & SIEM Efficiency Testing | ⬜ |
| `[12]` | Infrastructure Auditing via CIS Benchmarks | ⬜ |
| `[13]` | Kaspersky Security Center (EDR/AV) Enterprise Setup | ⬜ |
| `[14]` | Final Security Review & Operations Wrap-Up | ⬜ |

> ✅ Complete · 🔄 In Progress · ⬜ Pending

---

## 📂 Deployment Docs

---

<details>
<summary><b>📘 Phase 1 · [02] — Proxmox Hypervisor Deployment & Bridge VLAN Configuration</b></summary>

<br>

> **Scope:** Nested installation of **Proxmox VE 9.1** inside **VMware Workstation Pro**, establishing the foundational hypervisor layer and complete internal network backbone for the Mursad SOC environment.

---

### Overview

```
[ VMware Workstation Pro 17 ]
        └── VM: "Project Mursad"  (50 GB · 4 GB RAM · 2 vCPUs)
                └── Proxmox VE 9.1  —  node: mursad
                        ├── vmbr0  →  Management / NAT    192.168.140.129/24
                        ├── vmbr1  →  Workstation Segment  10.22.0.1/24
                        ├── vmbr2  →  Servers Segment      10.22.7.1/24
                        └── vmbr3  →  DMZ Zone             192.168.50.1/24
```

| Part | Section | Description |
|------|---------|-------------|
| A | VM Preparation | Define the VMware virtual hardware container |
| B | Proxmox OS Install | Deploy the hypervisor operating system |
| C | Web GUI & Network | Engineer the internal SOC bridge network |

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
|----------|---------|
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
> If you receive a *"No support for hardware-accelerated KVM"* error, verify **"Virtualize AMD-V/RVI"** is enabled under VMware Processor settings. If the issue persists, see the troubleshooting section for the VBS/Credential Guard fix.

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

Set a strong root password and provide an email for system alerts.

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

After reboot the console displays:

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
|--------|------|:---------:|---------|------|
| `vmbr0` | `192.168.140.129/24` | ✅ | — | Management / NAT uplink via `nic0` |
| `vmbr1` | `10.22.0.1/24` | ✅ | `Workstation` | Workstation VLAN segment |
| `vmbr2` | `10.22.7.1/24` | ✅ | `Servers` | Internal servers segment |
| `vmbr3` | `192.168.50.1/24` | ✅ | `DMZ` | Demilitarized Zone |

> ⚠️ `vmbr2` and `vmbr3` will show **Active: No** until a VM is attached — this is expected.

Click **"Apply Configuration"**. The pending diff confirms the changes written to `/etc/network/interfaces`:

```diff
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
```

---

#### Step 17 — Verify Final Network State

<img width="1601" height="687" alt="bridges-verify1" src="https://github.com/user-attachments/assets/3fdd4993-4f7a-41d5-8672-14a15c35f35d" />
<img width="1610" height="698" alt="bridges-verify2" src="https://github.com/user-attachments/assets/2b9c6b02-c07e-48db-a001-3a26f7a0ccf3" />

| Bridge | Type | Active | Autostart | CIDR | Comment |
|--------|------|:------:|:---------:|------|---------|
| `nic0` | Network Device | ✅ | — | — | Physical uplink `enp2s1` |
| `vmbr0` | Linux Bridge | ✅ | ✅ | `192.168.140.129/24` | Management / NAT |
| `vmbr1` | Linux Bridge | ✅ | ✅ | `10.22.0.1/24` | Workstation |
| `vmbr2` | Linux Bridge | — | ✅ | `10.22.7.1/24` | Servers |
| `vmbr3` | Linux Bridge | — | ✅ | `192.168.50.1/24` | DMZ |

```
Proxmox Node: mursad
│
├── vmbr0  ──►  Management / NAT     192.168.140.129/24   (nic0 uplink)
├── vmbr1  ──►  Workstation Segment  10.22.0.1/24
├── vmbr2  ──►  Servers Segment      10.22.7.1/24
└── vmbr3  ──►  DMZ Zone             192.168.50.1/24
```

---

### ✅ Phase Checklist

- [ ] Proxmox VE 9.1 installed and booting correctly
- [ ] Web GUI accessible at `https://192.168.140.129:8006/`
- [ ] `vmbr0` active — IP `192.168.140.129/24`, gateway `192.168.140.2`
- [ ] `vmbr1` created — `10.22.0.1/24`, comment `Workstation`, Autostart ✅
- [ ] `vmbr2` created — `10.22.7.1/24`, comment `Servers`, Autostart ✅
- [ ] `vmbr3` created — `192.168.50.1/24`, comment `DMZ`, Autostart ✅
- [ ] **"Apply Configuration"** clicked — no pending changes remaining

<div align="center"><br>

**🟢 Phase Complete**

`[01] Project Architecture` ◄ **You are here: `[02] Proxmox Deployment`** ► `[03] pfSense Edge Firewall`

<br></div>

</details>

---

<details>
<summary><b>📗 Phase 1 · [03] — pfSense Edge Firewall Installation & Baseline Setup</b></summary>

<br>

> **Scope:** Deployment of the **pfSense CE** virtual appliance within Proxmox, mapping all network interfaces to the Linux Bridges engineered in `[02]`, and establishing baseline routing and DHCP for the Mursad SOC environment.

---

### Overview

```
Proxmox Node: mursad
└── VM 100  —  Firewall  (pfSense CE)
        ├── vtnet0  →  vmbr0  →  WAN    192.168.140.x/24  (DHCP from host)
        ├── vtnet1  →  vmbr1  →  LAN    10.22.0.1/24      (Workstation)
        ├── vtnet2  →  vmbr2  →  OPT1   10.22.7.1/24      (Servers)
        └── vtnet3  →  vmbr3  →  OPT2   192.168.50.1/24   (DMZ)
```

| Part | Section | Description |
|------|---------|-------------|
| A | VM Provisioning | Create the VM, upload ISO, attach all NICs before first boot |
| B | OS Installation | Install pfSense CE onto the virtual disk |
| C | Interface & Routing | Map bridges, assign IPs, enable DHCP |

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

> ⚠️ Download arrives as `.gz`. Extract to get the `.iso` before uploading to Proxmox.

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

> WAN interface only. Remaining bridges added in Step 12 before first boot.

---

#### Step 11 — Confirm & Finish

<img width="718" height="538" alt="pfsense-finish" src="https://github.com/user-attachments/assets/a5e3caca-e066-4773-96b7-c52a9ec0930c" />

Review and click **Finish**. Do **not** check "Start after created".

---

#### Step 12 — Add Remaining Network Interfaces

> ⚠️ **Complete this before first boot.**
>
> Navigate to **VM 100 → Hardware → Add → Network Device**:

| NIC | Bridge | Model |
|-----|--------|-------|
| net1 | `vmbr1` | VirtIO |
| net2 | `vmbr2` | VirtIO |
| net3 | `vmbr3` | VirtIO |

All four interfaces must be present before pfSense boots so they are detected and assignable during setup.

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

Select **Auto (ZFS)**. Provides data integrity, snapshot support, and is ideal for firewall appliances. Accept defaults and select the virtual disk when prompted.

---

### Part C — Interface Assignment & Routing

#### Step 17 — VLAN Setup

<img width="1830" height="1012" alt="pfsense-vlan" src="https://github.com/user-attachments/assets/62497691-b1de-4e89-a0cc-27a1de8848f3" />

```
Should VLANs be set up now? [y/n]  →  n
```

Segmentation is handled by separate Proxmox bridges — VLANs not required.

---

#### Step 18 — Assign Interfaces

<img width="1823" height="976" alt="pfsense-assign1" src="https://github.com/user-attachments/assets/71ffea5d-b1d2-4d8a-abbc-9df6d232f0c8" />
<img width="1818" height="1002" alt="pfsense-assign2" src="https://github.com/user-attachments/assets/bfe16e14-bfe2-437a-a141-281c20c39efc" />
<img width="1827" height="1006" alt="pfsense-assign3" src="https://github.com/user-attachments/assets/557a26d7-1b4c-4c35-8deb-995254bddfcf" />
<img width="1832" height="1005" alt="pfsense-assign4" src="https://github.com/user-attachments/assets/c4499423-f730-413b-853a-7d0d643d47d1" />

| Prompt | Input | Maps To |
|--------|-------|---------|
| WAN interface | `vtnet0` | `vmbr0` — Internet |
| LAN interface | `vtnet1` | `vmbr1` — Workstation |
| OPT1 interface | `vtnet2` | `vmbr2` — Servers |
| OPT2 interface | `vtnet3` | `vmbr3` — DMZ |

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

Access the WebConfigurator from any Workstation-segment machine:

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
|---------|-------|--------|----|------|
| WAN | vtnet0 | vmbr0 | DHCP `192.168.140.x` | Internet |
| LAN | vtnet1 | vmbr1 | `10.22.0.1/24` | Workstation |
| OPT1 | vtnet2 | vmbr2 | `10.22.7.1/24` | Servers *(configured in `[04]`)* |
| OPT2 | vtnet3 | vmbr3 | `192.168.50.1/24` | DMZ *(configured in `[04]`)* |

---

### ✅ Phase Checklist

- [ ] pfSense CE ISO uploaded to Proxmox local storage
- [ ] VM 100 created — CPU type set to `host`, machine `i440fx`
- [ ] All 4 NICs attached (vmbr0–vmbr3) **before first boot**
- [ ] pfSense installed with Auto (ZFS) partitioning
- [ ] Interfaces assigned: vtnet0=WAN · vtnet1=LAN · vtnet2=OPT1 · vtnet3=OPT2
- [ ] WAN acquiring DHCP from host network
- [ ] LAN configured at `10.22.0.1/24` with DHCP pool `100–200`
- [ ] WebConfigurator accessible at `https://10.22.0.1`
- [ ] Default `admin` password changed

<div align="center"><br>

**🟢 Phase Complete**

`[02] Proxmox Deployment` ◄ **You are here: `[03] pfSense Installation`** ► `[04] Advanced Firewall Routing`

<br></div>

</details>

---

## 📁 Repository Structure

```
Project-Mursad/
│
├── 📂 assets/                  # Screenshots, diagrams, topology SVG
│   ├── mursad-topology.svg
│   └── phase1-*/
│
├── 📂 configs/                 # Exported configs and rule sets
│   ├── pfsense-config.xml
│   ├── suricata-rules/
│   └── wazuh-rules/
│
├── 📂 docs/                    # Phase-by-phase deployment guides
│   ├── phase1-02-proxmox.md
│   ├── phase1-03-pfsense.md
│   ├── phase1-04-routing.md
│   └── ...
│
├── 📂 scripts/                 # Automation scripts
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

<br>

*Built to understand the attack. Designed to defend against it.*

<br>

[![GitHub](https://img.shields.io/badge/github-0xcgz%2FProject--Mursad-181717?style=for-the-badge&logo=github)](https://github.com/0xcgz/Project-Mursad)

<br>

```
[ ! ] DEPLOYMENT IN PROGRESS...
```

</div>
