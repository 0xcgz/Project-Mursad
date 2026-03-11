<div align="center">

# 📚 docs/

**Step-by-step deployment documentation for Project Mursad**

<p align="center">
  <img src="https://img.shields.io/badge/Phase_1-COMPLETE-2ecc71?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Phase_2-IN_PROGRESS-e94560?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Phase_3-PENDING-555555?style=flat-square&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Phase_4-PENDING-555555?style=flat-square&labelColor=1a1a2e"/>
</p>

> Each file in this folder is a full technical guide for one deployment step.
> Screenshots live in [`../assets/`](../assets/). The project overview lives in [`../README.md`](../README.md).

</div>

---

## 📂 Structure

```
docs/
│
├── phase-1/                          ✅ Complete
│   ├── 01-project-architecture.md
│   ├── 02-proxmox-deployment.md
│   ├── 03-pfsense-installation.md
│   └── 04-firewall-routing.md
│
├── phase-2/                          🔄 In Progress
│   ├── 05-domain-controller.md
│   ├── 06-active-directory.md
│   ├── 07-dmz-architecture.md
│   └── 08-traffic-isolation.md
│
├── phase-3/                          ⬜ Pending
│   ├── 09-suricata-ids-ips.md
│   └── 10-wazuh-siem.md
│
└── phase-4/                          ⬜ Pending
    ├── 11-antivirus-siem.md
    ├── 12-cis-benchmarks.md
    ├── 13-kaspersky-edr.md
    └── 14-final-review.md
```

---

## 🗂️ Index

### ◆ Phase 1 — Infrastructure & Perimeter Initialization
> Establishing the hypervisor environment and securing the network edge.

| # | Document | Description | Status |
|:---:|----------|-------------|:------:|
| `[01]` | [01-project-architecture.md](./phase-1/01-project-architecture.md) | Project scope, goals, hardware specs, and lab design rationale | `✅` |
| `[02]` | [02-proxmox-deployment.md](./phase-1/02-proxmox-deployment.md) | Proxmox VE 9.1 install inside VMware + all 4 bridge VLAN configuration | `✅` |
| `[03]` | [03-pfsense-installation.md](./phase-1/03-pfsense-installation.md) | pfSense CE VM creation, OS install, interface assignment, DHCP setup | `✅` |
| `[04]` | [04-firewall-routing.md](./phase-1/04-firewall-routing.md) | WebConfigurator wizard, SERVERS/DMZ provisioning, baseline firewall rules | `✅` |

---

### ◆ Phase 2 — Enterprise Identity & Network Segmentation
> Building the corporate network, managing identities, and isolating services.

| # | Document | Description | Status |
|:---:|----------|-------------|:------:|
| `[05]` | [05-domain-controller.md](./phase-2/05-domain-controller.md) | Windows Server 2022 VM deployment + network integration | `🔄` |
| `[06]` | [06-active-directory.md](./phase-2/06-active-directory.md) | AD DS install, `mursad.local` domain, users, OUs, GPOs | `⬜` |
| `[07]` | [07-dmz-architecture.md](./phase-2/07-dmz-architecture.md) | DMZ server provisioning + service isolation | `⬜` |
| `[08]` | [08-traffic-isolation.md](./phase-2/08-traffic-isolation.md) | Inter-zone firewall rules + local DNS mapping | `⬜` |

---

### ◆ Phase 3 — SOC Telemetry & Traffic Analysis
> Deploying the eyes and ears of the network.

| # | Document | Description | Status |
|:---:|----------|-------------|:------:|
| `[09]` | [09-suricata-ids-ips.md](./phase-3/09-suricata-ids-ips.md) | Suricata install on pfSense + rule sets + inline IPS mode | `⬜` |
| `[10]` | [10-wazuh-siem.md](./phase-3/10-wazuh-siem.md) | Wazuh server install, syslog ingestion, agent rollout to all endpoints | `⬜` |

---

### ◆ Phase 4 — Endpoint Defense, Hardening & Validation
> Locking down endpoints and validating detection capability.

| # | Document | Description | Status |
|:---:|----------|-------------|:------:|
| `[11]` | [11-antivirus-siem.md](./phase-4/11-antivirus-siem.md) | AV integration + initial SIEM alert testing | `⬜` |
| `[12]` | [12-cis-benchmarks.md](./phase-4/12-cis-benchmarks.md) | CIS Level 1/2 benchmark auditing on all hosts | `⬜` |
| `[13]` | [13-kaspersky-edr.md](./phase-4/13-kaspersky-edr.md) | Kaspersky Security Center enterprise EDR deployment | `⬜` |
| `[14]` | [14-final-review.md](./phase-4/14-final-review.md) | Full security review, attack simulations, detection validation | `⬜` |

---

## 📝 Document Template

Every guide in this folder follows the same structure:

```markdown
# Phase X · [XX] — Title

> **Scope:** What this step covers.

## Overview
[ ASCII diagram or summary table ]

## Part A — ...
### Step 1 — ...
### Step 2 — ...

## Part B — ...
## Part C — ...

## ✅ Checklist
- [ ] Item
```

---

## 🔗 Navigation

| ← Back | Home | Assets |
|:------:|:----:|:------:|
| [`../README.md`](../README.md) | [`Project Mursad`](https://github.com/0xcgz/Project-Mursad) | [`../assets/`](../assets/) |
