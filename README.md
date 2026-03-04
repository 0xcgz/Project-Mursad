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
