<div align="center">

# 👁️ Project Mursad
**Enterprise Security Architecture & SOC Telemetry Lab**

<p align="center">
  <img src="https://img.shields.io/badge/Proxmox-E57000?style=for-the-badge&logo=proxmox&logoColor=white" alt="Proxmox" />
  <img src="https://img.shields.io/badge/pfSense-000000?style=for-the-badge&logo=pfsense&logoColor=white" alt="pfSense" />
  <img src="https://img.shields.io/badge/Active_Directory-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="Active Directory" />
  <img src="https://img.shields.io/badge/Wazuh-6B4CFF?style=for-the-badge&logo=wazuh&logoColor=white" alt="Wazuh" />
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Kali" />
</p>

> *A comprehensive virtualized SOC environment built on Proxmox. Features a segmented pfSense network, Active Directory deployment, and centralized threat monitoring using Wazuh SIEM.*

</div>

---

##  Project Roadmap & Deployment Phases

This project is executed in a structured, four-phase approach, moving from bare-metal virtualization to advanced threat detection.

<br>

###  Phase 1: Infrastructure & Perimeter Initialization
> **Objective:** *Establishing the hypervisor environment and securing the edge.*

* `[ 01 ]` **Project Architecture** & Introduction
* `[ 02 ]` **Proxmox Hypervisor** Deployment & Bridge VLAN Engineering
* `[ 03 ]` **pfSense Edge Firewall** Installation & Baseline Setup
* `[ 04 ]` **Advanced Firewall** Routing & Network Configuration

<br>

###  Phase 2: Enterprise Identity & Network Segmentation
> **Objective:** *Building the corporate network, managing identities, and isolating vulnerable services.*

* `[ 05 ]` **Domain Controller** Provisioning & Network Integration
* `[ 06 ]` **Active Directory** Domain Installation & Configuration
* `[ 07 ]` **DMZ Architecture** Setup (Demilitarized Zone)
* `[ 08 ]` **LAN/DMZ Traffic Isolation** & Secure Local DNS Mapping

<br>

###  Phase 3: SOC Telemetry & Traffic Analysis
> **Objective:** *Deploying the "eyes and ears" of the network to monitor all traffic and endpoint behavior.*

* `[ 09 ]` **IDS/IPS Configuration** (Intrusion Detection & Prevention)
* `[ 10 ]` **Wazuh SIEM Installation**, Syslog Ingestion & Agent Rollout

<br>

###  Phase 4: Endpoint Defense, Hardening & Validation
> **Objective:** *Locking down the endpoints and testing the SIEM's ability to catch malicious activity.*

* `[ 11 ]` **Antivirus Integration** & Initial SIEM Efficiency Testing
* `[ 12 ]` **Infrastructure Auditing** via CIS Benchmarks
* `[ 13 ]` **Kaspersky Security Center** (EDR/AV) Enterprise Setup
* `[ 14 ]` **Final Security Review** & Operations Wrap-Up

---

<div align="center">
  <code><b>[ ! ] DEPLOYMENT IN PROGRESS...</b></code>
</div>


# Phase 1 — `[02]` Proxmox Hypervisor Deployment & Bridge VLAN Configuration

> **Scope:** Nested installation of **Proxmox VE 9.1** inside **VMware Workstation Pro**, establishing the foundational hypervisor layer and internal network backbone for the Mursad SOC environment.

---

## Overview

```
[ VMware Workstation Pro 17 ]
        └── VM: "Project Mursad"  (50 GB · 4 GB RAM · 2 vCPUs)
                └── Proxmox VE 9.1
                        ├── vmbr0  →  NAT / Management  (192.168.140.129/24)
                        └── vmbr1  →  Internal SOC Bridge  (10.22.0.1/24)
```

| Step | Section | Description |
|------|---------|-------------|
| A | VM Preparation | Define the VMware virtual hardware container |
| B | Proxmox OS Install | Deploy the hypervisor operating system |
| C | Web GUI & Network | Engineer the internal SOC bridge network |

---

## Part A — Virtual Machine Preparation

> *Define the virtual hardware container that will host the Proxmox environment inside VMware.*

### Step 1 — Download Proxmox VE

<img width="1394" height="1263" alt="0" src="https://github.com/user-attachments/assets/6136a1bf-d31d-43a1-a7ba-00084520a044" />


Navigate to [proxmox.com/en/downloads](https://www.proxmox.com/en/downloads) and download the **Proxmox VE 9.1-1 ISO Installer**.

---

### Step 2 — Create a New Virtual Machine

<img width="842" height="590" alt="2" src="https://github.com/user-attachments/assets/27bafdc6-26e3-4ed8-b2a8-06c4ab5a46f6" />

Launch **VMware Workstation Pro 17** and create a new VM via either:
- The **"Create a New Virtual Machine"** button on the main dashboard, or
- **File → New Virtual Machine** from the top menu bar

---

### Step 3 — Mount the Proxmox ISO

<img width="840" height="592" alt="3" src="https://github.com/user-attachments/assets/006037a3-4952-49e9-99f6-8befcd6191c3" />


Select **"Installer disc image file (iso)"**, click **Browse**, and mount the downloaded ISO:

```
proxmox-ve_9.1-1.iso
```

---

### Step 4 — Select Guest OS

<img width="842" height="587" alt="4" src="https://github.com/user-attachments/assets/ad4f4547-5645-4c28-b6c9-cfa2ff6dd13b" />

| Setting | Value |
|---------|-------|
| Guest OS | Linux |
| Version | Ubuntu *(Proxmox is Debian-based — Ubuntu profile ensures nested compatibility)* |

---

### Step 5 — Name & Storage Path

<img width="841" height="589" alt="5" src="https://github.com/user-attachments/assets/5fcf6d79-262c-4e30-a6f7-5ef0b69fc8f9" />

| Setting | Value |
|---------|-------|
| VM Name | `Project Mursad` |
| Location | *(choose a path with sufficient free space on your host drive)* |

---

### Step 6 — Disk Allocation

<img width="842" height="589" alt="6" src="https://github.com/user-attachments/assets/ea02e56c-d17f-46c8-a2db-5968fdd4be4d" />

| Setting | Value |
|---------|-------|
| Maximum Disk Size | `50.0 GB` |
| Storage Mode | Split virtual disk into multiple files |

---

### Step 7 — Hardware Summary & Power On

<img width="835" height="588" alt="7" src="https://github.com/user-attachments/assets/f173782e-2f3f-49ce-b098-e9a12173d976" />
<img width="836" height="581" alt="8" src="https://github.com/user-attachments/assets/060645ad-cf64-4aa7-abb4-c02bb7527544" />


Review the virtual hardware summary before clicking **Finish**:

| Resource | Minimum |
|----------|---------|
| RAM | 4 GB (4096 MB) |
| CPU Cores | 2 |
| Disk | 50 GB |

Once created, click **"Power on this virtual machine"** to begin the OS installation.

---

## Part B — Proxmox OS Installation

> *Deploy the bare-metal hypervisor operating system onto the virtual disk.*

### Step 8 — Boot Menu

<img width="1227" height="928" alt="9" src="https://github.com/user-attachments/assets/d1fa235c-1821-46c9-8ddc-4aab6477d786" />
<img width="1473" height="960" alt="10" src="https://github.com/user-attachments/assets/19e1346d-be49-4302-b5e2-7fd7f0e8d6b8" />


When the VM boots, use the arrow keys to select:

```
Install Proxmox VE (Graphical)
```

Allow the installer to load the Linux kernel and initialize setup components.

> ⚠️ **Nested Virtualization Warning** <img width="1478" height="931" alt="11" src="https://github.com/user-attachments/assets/3c189837-43ed-440d-aa27-60ad447042ad" />

> If you receive a *"No support for hardware-accelerated KVM"* error, verify that **"Virtualize Intel VT-x/EPT"** is enabled under VMware Processor settings before continuing.

---

### Step 9 — EULA

<img width="1483" height="905" alt="12" src="https://github.com/user-attachments/assets/7b11544f-f915-46fd-9884-8d790fba64f0" />


Review the End User License Agreement and click **"I agree"** to proceed.

---

### Step 10 — Target Disk

<img width="1269" height="791" alt="13" src="https://github.com/user-attachments/assets/6716b425-5a3f-4d8c-8670-eb209eae3c5f" />


Select the virtual disk as the installation target:

```
/dev/sda  —  50 GiB VMware Virtual Disk
```

---

### Step 11 — Localization

<img width="1268" height="797" alt="14" src="https://github.com/user-attachments/assets/89429566-d436-47dd-9503-eb06446dd6b2" />


| Setting | Value |
|---------|-------|
| Country | Bahrain |
| Time Zone | Asia/Bahrain |
| Keyboard Layout | U.S. English |

---

### Step 12 — Root Password & Email

<img width="1274" height="799" alt="15" src="https://github.com/user-attachments/assets/f138431e-6bd9-4fae-be67-b7737c5d0c6c" />


Set a strong password for the `root` account and provide a valid email address for system alerts.

---

### Step 13 — Management Network

<img width="1268" height="793" alt="16" src="https://github.com/user-attachments/assets/b508270e-e6a3-483e-8ed4-cf06687c5c72" />


Configure the static management network interface:

| Parameter | Value |
|-----------|-------|
| Hostname (FQDN) | `mursad.me` |
| IP Address / CIDR | `192.168.140.129/24` |
| Gateway | `192.168.140.2` |
| DNS Server | `192.168.140.2` |

---

### Step 14 — Install & Reboot

<img width="1271" height="793" alt="17" src="https://github.com/user-attachments/assets/a50801af-ad88-49b7-9679-a902339a9122" />
<img width="1274" height="826" alt="18" src="https://github.com/user-attachments/assets/23d36733-fd11-49ae-9e55-3d6b4ec3fd6f" />


Review the installation summary, check **"Automatically reboot after successful installation"**, and click **Install**.

After the reboot, the CLI console will display the web management URL:

```
https://192.168.140.129:8006/
```

> 📋 Save this URL — it is the primary access point for all Proxmox management going forward.

---

## Part C — Web GUI & Network Engineering

> *Access the management interface and engineer the internal SOC network backbone using Linux bridges.*

### Step 15 — Login to Web UI

<img width="1910" height="937" alt="19" src="https://github.com/user-attachments/assets/048302eb-2f9e-43e3-b356-c8a231eb1d55" />
<img width="1911" height="938" alt="20" src="https://github.com/user-attachments/assets/bd1ffa4c-1ca1-4c3e-8386-7b16338a63e6" />


Open a browser and navigate to `https://192.168.140.129:8006/`.

| Field | Value |
|-------|-------|
| User name | `root` |
| Password | *(your configured root password)* |
| Realm | `Linux PAM standard authentication` |

The dashboard will load displaying the **mursad** datacenter node and default storage volumes.

---

### Step 16 — Create Internal SOC Bridge (`vmbr1`)

<img width="1917" height="942" alt="21" src="https://github.com/user-attachments/assets/82639e21-95ed-4d53-9090-90aa7e5be0bb" />
<img width="634" height="281" alt="22" src="https://github.com/user-attachments/assets/6648ee3c-91c5-4450-bf1b-a11dcd7e55bb" />
<img width="1610" height="698" alt="23" src="https://github.com/user-attachments/assets/3fdd4993-4f7a-41d5-8672-14a15c35f35d" />


Navigate to **mursad → System → Network**, click **Create**, and select **Linux Bridge**.

Configure the new bridge with the following parameters:

| Parameter | Value |
|-----------|-------|
| Name | `vmbr1` |
| IPv4 / CIDR | `10.22.0.1/24` |
| Autostart | ✅ Enabled |
| Comment | `Internal SOC Bridge` |

Click **OK**, then click **"Apply Configuration"** at the top of the network pane to activate the bridge.

---

### Step 17 — Create Additional SOC Bridges

With the lab design finalised, three additional Linux Bridges are required beyond the default `vmbr0`. Repeat the **Create → Linux Bridge** process from Step 16 for each entry below:

| Bridge | CIDR | Autostart | Comment | Role |
|--------|------|-----------|---------|------|
| `vmbr0` | `192.168.140.129/24` | ✅ Yes | — | Management / NAT uplink via `nic0` |
| `vmbr1` | `10.22.0.1/24` | ✅ Yes | `Workstation` | Workstation VLAN segment |
| `vmbr2` | `10.22.7.1/24` | ✅ Yes | `Servers` | Internal servers segment |
| `vmbr3` | `192.168.50.1/24` | ✅ Yes | `DMZ` | Demilitarized Zone |

> ⚠️ **Note:** `vmbr2` and `vmbr3` will show **Active: No** until a VM is attached and the bridge comes up. This is expected.

Once all bridges are created, click **"Apply Configuration"** to push the changes to `/etc/network/interfaces`. The pending diff shown at the bottom of the Network pane confirms what will be written:

```diff
+auto vmbr2
+iface vmbr2 inet static
+    address 10.22.7.1/24
+    bridge-ports none
+    bridge-stp off
+    bridge-fd 0
```

---

## Final Network State

The screenshot below confirms the completed bridge configuration for the **mursad** node:

<img width="1601" height="687" alt="24" src="https://github.com/user-attachments/assets/2b9c6b02-c07e-48db-a001-3a26f7a0ccf3" />

![Proxmox Network Bridges](./assets/phase1-02-network-bridges.png)

| Bridge | Type | Active | Autostart | CIDR | Gateway | Comment |
|--------|------|--------|-----------|------|---------|---------|
| `nic0` | Network Device | Yes | No | — | — | Physical uplink (`enp2s1`) |
| `vmbr0` | Linux Bridge | ✅ Yes | ✅ Yes | `192.168.140.129/24` | `192.168.140.2` | Management / NAT |
| `vmbr1` | Linux Bridge | ✅ Yes | ✅ Yes | `10.22.0.1/24` | — | Workstation |
| `vmbr2` | Linux Bridge | No | ✅ Yes | `10.22.7.1/24` | — | Servers |
| `vmbr3` | Linux Bridge | No | ✅ Yes | `192.168.50.1/24` | — | DMZ |

---

## Network Summary

All four bridges are now engineered and ready to receive VMs. Each bridge maps directly to a network zone in the Mursad SOC architecture:

```
Proxmox Node: mursad
│
├── vmbr0  ──►  Management / NAT     192.168.140.129/24   (nic0 uplink)
├── vmbr1  ──►  Workstation Segment  10.22.0.1/24
├── vmbr2  ──►  Servers Segment      10.22.7.1/24
└── vmbr3  ──►  DMZ Zone             192.168.50.1/24
```

> These bridges will be assigned to pfSense interfaces in the next phase, giving the firewall full visibility and control over inter-zone traffic.

---

## Checklist

Before proceeding to the next phase, verify all of the following:

- [ ] Proxmox VE 9.1 installed and booting correctly
- [ ] Web GUI accessible at `https://192.168.140.129:8006/`
- [ ] `vmbr0` active — management IP `192.168.140.129/24`, gateway `192.168.140.2`
- [ ] `vmbr1` created — `10.22.0.1/24`, comment `Workstation`, Autostart enabled
- [ ] `vmbr2` created — `10.22.7.1/24`, comment `Servers`, Autostart enabled
- [ ] `vmbr3` created — `192.168.50.1/24`, comment `DMZ`, Autostart enabled
- [ ] **"Apply Configuration"** clicked — no pending changes remaining

---

<div align="center">

**🟢 Phase Complete**

`[01] Project Architecture` ← **You are here:** `[02] Proxmox Deployment` → `[03] pfSense Edge Firewall`

</div>
