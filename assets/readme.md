# 📁 assets/

> This folder contains all screenshots, diagrams, and visual documentation for Project Mursad.
> Images are organized by phase and step number, matching the deployment docs in `README.md`.

---

## 📂 Folder Structure

```
assets/
│
├── 📂 diagrams/                         # Network topology & architecture diagrams
│   └── mursad-topology.svg
│
├── 📂 phase-1/                          # Infrastructure & Perimeter Initialization
│   ├── 📂 01-project-architecture/      # [01] Project overview screenshots
│   ├── 📂 02-proxmox-deployment/        # [02] Proxmox install + bridge config
│   ├── 📂 03-pfsense-installation/      # [03] pfSense VM + OS install + interfaces
│   └── 📂 04-firewall-routing/          # [04] WebConfigurator wizard + firewall rules
│
├── 📂 phase-2/                          # Enterprise Identity & Network Segmentation
│   ├── 📂 05-domain-controller/         # [05] DC provisioning + network integration
│   ├── 📂 06-active-directory/          # [06] AD domain install + GPO config
│   ├── 📂 07-dmz-architecture/          # [07] DMZ server setup
│   └── 📂 08-traffic-isolation/         # [08] LAN/DMZ rules + DNS
│
├── 📂 phase-3/                          # SOC Telemetry & Traffic Analysis
│   ├── 📂 09-suricata-ids-ips/          # [09] Suricata install + rule tuning
│   └── 📂 10-wazuh-siem/               # [10] Wazuh server + agent rollout
│
└── 📂 phase-4/                          # Endpoint Defense, Hardening & Validation
    ├── 📂 11-antivirus-siem/            # [11] AV integration + SIEM testing
    ├── 📂 12-cis-benchmarks/            # [12] CIS audit results
    ├── 📂 13-kaspersky-edr/             # [13] Kaspersky Security Center setup
    └── 📂 14-final-review/              # [14] Final security review screenshots
```

---

## 📌 Naming Convention

When adding screenshots to a folder, name them sequentially by step number:

```
[phase]-[step]-[description]-[sequence].png

Examples:
  phase1-02-proxmox-boot-01.png
  phase1-02-proxmox-boot-02.png
  phase1-03-pfsense-interfaces-01.png
  phase1-04-wizard-wan-config-01.png
  phase2-05-dc-server-manager-01.png
```

---

## ✅ Status

| Phase | Folder | Screenshots |
|:-----:|--------|:-----------:|
| 1 | `phase-1/01-project-architecture/` | ⬜ Pending upload |
| 1 | `phase-1/02-proxmox-deployment/` | ✅ Complete |
| 1 | `phase-1/03-pfsense-installation/` | ✅ Complete |
| 1 | `phase-1/04-firewall-routing/` | ✅ Complete |
| 2 | `phase-2/05-domain-controller/` | 🔄 In progress |
| 2 | `phase-2/06-active-directory/` | ⬜ Pending |
| 2 | `phase-2/07-dmz-architecture/` | ⬜ Pending |
| 2 | `phase-2/08-traffic-isolation/` | ⬜ Pending |
| 3 | `phase-3/09-suricata-ids-ips/` | ⬜ Pending |
| 3 | `phase-3/10-wazuh-siem/` | ⬜ Pending |
| 4 | `phase-4/11-antivirus-siem/` | ⬜ Pending |
| 4 | `phase-4/12-cis-benchmarks/` | ⬜ Pending |
| 4 | `phase-4/13-kaspersky-edr/` | ⬜ Pending |
| 4 | `phase-4/14-final-review/` | ⬜ Pending |
