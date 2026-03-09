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

# Phase 1 — `[03]` pfSense Edge Firewall Installation & Baseline Setup

> **Scope:** Deployment of the **Netgate pfSense Plus** virtual appliance within Proxmox, mapping all network interfaces to the Linux Bridges engineered in `[02]`, and establishing baseline routing for the Mursad SOC environment.

---

## Overview

```
Proxmox Node: mursad
└── VM 100 — Firewall (pfSense Plus)
        ├── vtnet0  →  vmbr0  →  WAN   (192.168.140.x — DHCP from host)
        ├── vtnet1  →  vmbr1  →  LAN   (10.22.0.1/24   — Workstation)
        ├── vtnet2  →  vmbr2  →  OPT1  (10.22.7.1/24   — Servers)
        └── vtnet3  →  vmbr3  →  OPT2  (192.168.50.1/24 — DMZ)
```

| Step | Section | Description |
|------|---------|-------------|
| A | VM Provisioning | Create the VM container and attach network interfaces |
| B | OS Installation | Install pfSense onto the virtual disk |
| C | Interface & Routing | Map bridges, assign IPs, and enable DHCP |

---

## Part A — Virtual Machine Provisioning

> *Create the virtual hardware container, upload the pfSense ISO, and configure the VM before first boot.*

### Step 1 — Download pfSense ISO

<img width="1222" height="887" alt="1" src="https://github.com/user-attachments/assets/3fb40940-64ec-4a72-b7c2-b4bdcbb50f0c" />
<img width="1111" height="776" alt="2" src="https://github.com/user-attachments/assets/a34523a6-f5f4-4924-8f3b-de108e33b15c" />


Navigate to [pfsense.org/download](https://www.pfsense.org/download/) and download with these options:

| Setting | Value |
|---------|-------|
| Architecture | AMD64 (64-bit) |
| Installer | DVD Image (ISO) |
| Console | VGA |

> ⚠️ The download arrives as a `.gz` compressed file. Extract it to obtain the `.iso` before uploading to Proxmox.

---

### Step 2 — Upload ISO to Proxmox

<img width="1051" height="738" alt="3" src="https://github.com/user-attachments/assets/08d05f59-4370-45b7-b6f7-11b673e2335e" />
<img width="887" height="605" alt="4" src="https://github.com/user-attachments/assets/76858e07-0a3a-4700-a28b-8951af9330c1" />
<img width="406" height="312" alt="5" src="https://github.com/user-attachments/assets/ace26d91-a790-4c7b-8bbb-9d179aa0a95a" />
<img width="1911" height="537" alt="6" src="https://github.com/user-attachments/assets/2ad9c755-c04d-4643-af6b-2a995e7d2235" />

1. In the Proxmox Web GUI, navigate to **mursad → local (mursad) → ISO Images**
2. Click **Upload → Select File**, browse to the extracted pfSense `.iso`
3. Click **Upload** and wait for the task log to show:

```
TASK OK
```

---

### Step 3 — Create the VM

<img width="460" height="472" alt="7" src="https://github.com/user-attachments/assets/d874199a-96e4-45e3-8bdd-e974653b0c3d" />

Click **"Create VM"** in the top-right corner of the Proxmox interface.

---

### Step 4 — General Tab

<img width="721" height="534" alt="8" src="https://github.com/user-attachments/assets/2d20dc43-a1bc-43ab-9b7e-fb4c624bc64a" />


| Setting | Value |
|---------|-------|
| VM ID | `100` |
| Name | `Firewall` |
| Node | `mursad` |

---

### Step 5 — OS Tab

<img width="788" height="539" alt="9" src="https://github.com/user-attachments/assets/eca6d83d-692b-4b37-b0bf-d6f696bd12d5" />


| Setting | Value |
|---------|-------|
| Storage | `local (mursad)` |
| ISO Image | pfSense ISO you uploaded |
| Guest OS Type | Other |

---

### Step 6 — System Tab


<img width="716" height="537" alt="10" src="https://github.com/user-attachments/assets/4dd7f1b1-b566-4a4e-ba26-63516c470873" />

| Setting | Value |
|---------|-------|
| BIOS | SeaBIOS |
| Machine | i440fx |
| SCSI Controller | VirtIO SCSI |

> Leave defaults — do not change to OVMF/UEFI as pfSense CE installer does not require it.

---

### Step 7 — Disks Tab

<img width="719" height="539" alt="11" src="https://github.com/user-attachments/assets/da1a221c-1442-4245-af10-4d7f1bef1445" />


| Setting | Value |
|---------|-------|
| Storage | `local-lvm` |
| Disk Size | `32 GiB` |
| Format | raw |
| Cache | No cache |

---

### Step 8 — CPU Tab

<img width="722" height="534" alt="12" src="https://github.com/user-attachments/assets/6efdcca8-1312-4ba1-9b8a-2321629bd46b" />

| Setting | Value |
|---------|-------|
| Cores | `2` |
| Type | `host` |

> Setting CPU type to `host` passes AMD Ryzen flags directly to the VM — critical for stable pfSense/FreeBSD operation.

---

### Step 9 — Memory Tab

<img width="722" height="539" alt="13" src="https://github.com/user-attachments/assets/62db1e06-74a7-4d0c-9995-95a1bcdb7812" />


| Setting | Value |
|---------|-------|
| Memory | `4096 MiB (4 GB)` |

> 4 GB provides headroom for pfSense plus future Suricata IDS/IPS package installation.

---

### Step 10 — Network Tab

<img width="721" height="540" alt="14" src="https://github.com/user-attachments/assets/00513ee8-acbf-4af8-8fe6-4d58835ed1a2" />


| Setting | Value |
|---------|-------|
| Bridge | `vmbr0` |
| Model | `VirtIO (paravirtualized)` |
| Firewall | Unchecked |

> This attaches the WAN interface only. The remaining 3 bridges will be added after OS installation.

---

### Step 11 — Confirm & Finish

<img width="718" height="538" alt="15" src="https://github.com/user-attachments/assets/a5e3caca-e066-4773-96b7-c52a9ec0930c" />


Review the hardware summary and click **Finish**. Do **not** check "Start after created" — additional NICs need to be added first.

---

## Part B — pfSense OS Installation

> *Boot the VM from the ISO and install the FreeBSD-based firewall OS.*

### Step 12 — Add Remaining Network Interfaces

Before first boot, attach the remaining three bridges.

Navigate to **VM 100 → Hardware → Add → Network Device** and add:

| NIC | Bridge | Model |
|-----|--------|-------|
| net1 | `vmbr1` | VirtIO |
| net2 | `vmbr2` | VirtIO |
| net3 | `vmbr3` | VirtIO |

---

### Step 13 — Boot & Launch Console

<img width="1914" height="743" alt="16" src="https://github.com/user-attachments/assets/209f7861-d692-491a-bf8c-27fc7571d6f1" />
<img width="1826" height="1021" alt="17" src="https://github.com/user-attachments/assets/f51641f6-ebbd-4457-b4b4-12f920a0d8e3" />


Select **VM 100 (Firewall)** from the sidebar, click **Start**, then open **Console**.

The pfSense bootloader will appear. Allow the timer to expire or press **Enter** to boot immediately.

---

### Step 14 — Accept License

<img width="1832" height="1008" alt="18" src="https://github.com/user-attachments/assets/aa7419e4-6074-4384-86de-c4acf363d09a" />


Accept the Netgate Copyright and Trademark notice to proceed.

---

### Step 15 — Select Install

<img width="1445" height="767" alt="19" src="https://github.com/user-attachments/assets/d4a80dcc-f2a1-4b68-9ad6-c1cfb2795730" />

Select **Install pfSense** from the welcome menu.

---

### Step 16 — Partition Scheme

<img width="1812" height="1009" alt="20" src="https://github.com/user-attachments/assets/a9cf0420-c040-4159-8c74-597dd4eaaf26" />


Select **Auto (ZFS)** for the partition setup.

| Option | Reason |
|--------|--------|
| Auto (ZFS) | Superior data integrity, snapshot support, ideal for firewall appliances |

Proceed with default ZFS options and select the virtual disk when prompted.

---

## Part C — Interface Assignment & Routing Configuration

> *Map the Proxmox bridges to pfSense interfaces and configure IP addressing for each zone.*

### Step 17 — VLAN Setup

<img width="1830" height="1012" alt="22" src="https://github.com/user-attachments/assets/62497691-b1de-4e89-a0cc-27a1de8848f3" />

When prompted:
```
Should VLANs be set up now? [y/n]
```
Type `n` and press **Enter**. VLANs are not required — our segmentation is handled by separate bridges.

---

### Step 18 — Assign Interfaces

<img width="1823" height="976" alt="23" src="https://github.com/user-attachments/assets/71ffea5d-b1d2-4d8a-abbc-9df6d232f0c8" />
<img width="1818" height="1002" alt="24" src="https://github.com/user-attachments/assets/bfe16e14-bfe2-437a-a141-281c20c39efc" />
<img width="1827" height="1006" alt="25" src="https://github.com/user-attachments/assets/557a26d7-1b4c-4c35-8deb-995254bddfcf" />
<img width="1832" height="1005" alt="26" src="https://github.com/user-attachments/assets/c4499423-f730-413b-853a-7d0d643d47d1" />


Assign each interface as follows when prompted:

| Prompt | Input | Maps To |
|--------|-------|---------|
| WAN interface | `vtnet0` | `vmbr0` — Host NAT / Internet |
| LAN interface | `vtnet1` | `vmbr1` — Workstation segment |
| OPT1 interface | `vtnet2` | `vmbr2` — Servers segment |
| OPT2 interface | `vtnet3` | `vmbr3` — DMZ |

When the assignment summary is displayed, type `y` and press **Enter** to confirm.

---

### Step 19 — Verify Boot & WAN DHCP

<img width="1820" height="1006" alt="27" src="https://github.com/user-attachments/assets/dc6b95ec-adc2-476d-9330-236e0ec4e277" />
<img width="1820" height="1003" alt="28" src="https://github.com/user-attachments/assets/439cfa86-0660-44a4-b7bc-c3d07eeecbcb" />


pfSense will initialize all interfaces and start routing services. Once the console menu appears, verify:

```
WAN (vtnet0) →  192.168.140.xxx/24   ← DHCP from Proxmox host ✅
LAN (vtnet1) →  no IP yet            ← we configure this next
```

---

### Step 20 — Set LAN IP Address

<img width="1829" height="1012" alt="29" src="https://github.com/user-attachments/assets/be136960-8327-4237-8582-bc752d7a646a" />
<img width="1914" height="989" alt="30" src="https://github.com/user-attachments/assets/359a4de9-de78-40d7-acd3-016d239679d1" />

From the pfSense console menu, type `2` → press **Enter** to enter **"Set interface(s) IP address"**.

Select option `2` for the **LAN** interface.

---

### Step 21 — Configure LAN

<img width="1203" height="660" alt="31" src="https://github.com/user-attachments/assets/4835f9b7-2b7a-4cee-bc91-233205219248" />
<img width="1137" height="654" alt="32" src="https://github.com/user-attachments/assets/837976e1-3106-4774-ba8d-91957acb0fb1" />
<img width="1177" height="404" alt="33" src="https://github.com/user-attachments/assets/e2307588-7ea4-4810-a4fc-6d59cd16dd8a" />


Enter the following when prompted:

| Prompt | Value |
|--------|-------|
| IPv4 Address | `10.22.0.1` |
| Subnet bit count | `24` |
| Upstream gateway | *(press Enter — none)* |
| IPv6 address | *(press Enter — skip)* |

---

### Step 22 — Enable DHCP on LAN

<img width="1151" height="186" alt="35" src="https://github.com/user-attachments/assets/7eea0b2c-b401-4fdf-9801-00769d530a62" />
<img width="1175" height="388" alt="36" src="https://github.com/user-attachments/assets/a47bfb2f-c184-40a0-a1e6-80a075cbddef" />
<img width="1176" height="267" alt="37" src="https://github.com/user-attachments/assets/19c5ba61-d30e-42b4-827a-10ca312bc302" />
<img width="1163" height="556" alt="34" src="https://github.com/user-attachments/assets/d0705f08-fa84-4cf5-811b-f373c31dec22" />


When asked to enable the DHCP server on LAN, type `y`.

| Prompt | Value |
|--------|-------|
| DHCP range start | `10.22.0.100` |
| DHCP range end | `10.22.0.200` |

---

### Step 23 — Confirm & Access WebConfigurator

<img width="1169" height="716" alt="38" src="https://github.com/user-attachments/assets/3f21233a-6fc6-4268-a484-15ed81cceeda" />
<img width="1173" height="879" alt="39" src="https://github.com/user-attachments/assets/3acda81b-24a7-4aed-98ef-54e76eb81e0d" />

When asked to revert webConfigurator to HTTP, type `n` (keep HTTPS).

The console will confirm the LAN IP is set:

```
LAN (vtnet1) →  10.22.0.1/24  ✅
```

The pfSense WebConfigurator is now accessible from any machine on the Workstation network at:

```
https://10.22.0.1
```

Default credentials:
| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `pfsense` |

> 🔐 **Change the default password immediately** upon first login.

---

## Interface Summary

| pfSense Interface | vtnet | Bridge | IP | Zone |
|-------------------|-------|--------|----|------|
| WAN | vtnet0 | vmbr0 | DHCP (`192.168.140.x`) | Internet uplink |
| LAN | vtnet1 | vmbr1 | `10.22.0.1/24` | Workstation |
| OPT1 | vtnet2 | vmbr2 | `10.22.7.1/24` | Servers *(configured in `[04]`)* |
| OPT2 | vtnet3 | vmbr3 | `192.168.50.1/24` | DMZ *(configured in `[04]`)* |

---

## Checklist

- [ ] pfSense ISO uploaded to Proxmox local storage
- [ ] VM 100 created with correct CPU type `host`
- [ ] All 4 NICs attached (vmbr0 → vmbr3)
- [ ] pfSense installed with Auto (ZFS) partitioning
- [ ] Interfaces assigned — vtnet0=WAN, vtnet1=LAN, vtnet2=OPT1, vtnet3=OPT2
- [ ] WAN pulling DHCP from host network
- [ ] LAN configured at `10.22.0.1/24` with DHCP pool `100–200`
- [ ] WebConfigurator accessible at `https://10.22.0.1`
- [ ] Default `admin` password changed

---

<div align="center">

**🟢 Phase Complete**

`[02] Proxmox Deployment` ← **You are here:** `[03] pfSense Installation` → `[04] Advanced Firewall Routing`

</div>
