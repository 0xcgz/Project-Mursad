<div align="center">

# ⚙️ configs/

**Exported configurations, rule sets, and hardening baselines for Project Mursad**

<p align="center">
  <img src="https://img.shields.io/badge/pfSense-config.xml-212121?style=flat-square&logo=pfsense&logoColor=white&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Suricata-rules-EF6C00?style=flat-square&logoColor=white&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Wazuh-ossec.conf-6B4CFF?style=flat-square&logoColor=white&labelColor=1a1a2e"/>
  <img src="https://img.shields.io/badge/Active_Directory-GPO_Export-0078D4?style=flat-square&logo=microsoft&logoColor=white&labelColor=1a1a2e"/>
</p>

> All configuration files in this folder are version-controlled snapshots of live systems.
> Screenshots live in [`../assets/`](../assets/). Deployment guides live in [`../docs/`](../docs/).

</div>

---

## 📂 Structure

```
configs/
│
├── pfsense/                          # Firewall & routing configuration
│   ├── config.xml                    # Full pfSense backup (all settings)
│   ├── firewall-rules.txt            # Exported rule summary per interface
│   └── dhcp-leases.txt               # Static DHCP mappings
│
├── suricata/                         # IDS/IPS engine configuration
│   ├── suricata.yaml                 # Main Suricata configuration file
│   └── custom-rules/
│       ├── local.rules               # General custom detection rules
│       └── mursad-alerts.rules       # Lab-specific signatures
│
├── wazuh/                            # SIEM manager & agent configuration
│   ├── ossec.conf                    # Wazuh manager main config
│   ├── agent/
│   │   └── agent.conf                # Shared agent configuration
│   └── rules/
│       ├── local_rules.xml           # Custom Wazuh detection rules
│       └── mursad_rules.xml          # Lab-specific alert rules
│
├── active-directory/                 # Identity & policy configuration
│   ├── gpo-export/                   # GPO backups from Group Policy Mgmt
│   ├── ou-structure.txt              # OU hierarchy documentation
│   └── users-template.csv            # Bulk user creation template
│
└── windows/                          # Endpoint hardening baselines
    ├── cis-audit-results.html        # CIS benchmark scan output
    └── hardening-baseline.inf        # Security Template export
```

---

## 🗂️ Index

### 🔥 pfSense

| File | Description | How to Export |
|------|-------------|---------------|
| `config.xml` | Complete firewall backup — interfaces, rules, DHCP, DNS, VPN | **Diagnostics → Backup & Restore → Download configuration as XML** |
| `firewall-rules.txt` | Human-readable rule summary per interface zone | **Firewall → Rules** → screenshot or manual export |
| `dhcp-leases.txt` | Static DHCP mappings for all lab hosts | **Services → DHCP Server → Static Mappings** |

---

### 🛡️ Suricata

| File | Description | Location on Host |
|------|-------------|-----------------|
| `suricata.yaml` | Main engine config — interfaces, logging, rule paths | `/etc/suricata/suricata.yaml` |
| `local.rules` | Custom detection signatures | `/etc/suricata/rules/local.rules` |
| `mursad-alerts.rules` | Lab-specific rules targeting simulated attacks | `/etc/suricata/rules/mursad-alerts.rules` |

---

### 📊 Wazuh

| File | Description | Location on Host |
|------|-------------|-----------------|
| `ossec.conf` | Manager config — log sources, decoders, active response | `/var/ossec/etc/ossec.conf` |
| `agent.conf` | Shared config pushed to all enrolled agents | `/var/ossec/etc/shared/agent.conf` |
| `local_rules.xml` | Custom detection rules added on top of default ruleset | `/var/ossec/etc/rules/local_rules.xml` |
| `mursad_rules.xml` | Lab-specific alert rules for red team simulations | `/var/ossec/etc/rules/mursad_rules.xml` |

---

### 🏢 Active Directory

| File | Description | How to Export |
|------|-------------|---------------|
| `gpo-export/` | Full GPO backups for all policies | **Group Policy Management → right-click GPO → Back Up** |
| `ou-structure.txt` | Documented OU hierarchy for `mursad.local` | Manual — mirror from AD Users & Computers |
| `users-template.csv` | CSV template used for bulk user creation via PowerShell | Manual — `New-ADUser` import format |

---

### 🖥️ Windows Hardening

| File | Description | How to Export |
|------|-------------|---------------|
| `cis-audit-results.html` | CIS benchmark scan report for domain endpoints | Run CIS-CAT or equivalent scanner → export HTML |
| `hardening-baseline.inf` | Security Template applied via Group Policy | **Security Configuration and Analysis → Export Template** |

---

## ⚠️ Security Notice

> Before committing any config file, verify it contains **no secrets**, passwords, API keys, or private keys.

| ✅ Safe to commit | ❌ Never commit |
|-------------------|----------------|
| pfSense `config.xml` with passwords removed | pfSense backup with plaintext `<password>` tags |
| Suricata rule files | Private TLS certificates or keys |
| Wazuh rule XML files | `ossec.conf` with plaintext agent auth keys |
| GPO export folders | Any `.pfx`, `.pem`, `.key` files |
| CIS audit HTML reports | Windows SAM database exports |

To strip passwords from pfSense backups before committing:
```bash
# Remove password fields before committing config.xml
sed -i 's|<password>.*</password>|<password>REDACTED</password>|g' config.xml
sed -i 's|<bcrypt-hash>.*</bcrypt-hash>|<bcrypt-hash>REDACTED</bcrypt-hash>|g' config.xml
```

---

## 🔄 Update Cadence

Re-export and commit configs when you:

- Complete a new phase step
- Add or modify firewall rules
- Write new Suricata or Wazuh detection rules
- Apply new GPOs or hardening changes
- Run a CIS benchmark audit

---

## 🔗 Navigation

| ← Docs | Home | Assets |
|:------:|:----:|:------:|
| [`../docs/`](../docs/) | [`Project Mursad`](https://github.com/0xcgz/Project-Mursad) | [`../assets/`](../assets/) |
