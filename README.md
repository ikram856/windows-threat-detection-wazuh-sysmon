# 🛡️ SOC Lab — Threat Detection & Investigation with Wazuh

A fully virtualized Security Operations Center (SOC) lab built to simulate, detect, and respond to real-world cyberattacks using **Wazuh** as the SIEM/XDR platform.

![Wazuh](https://img.shields.io/badge/Wazuh-4.9.2-blue?style=flat-square)
![Linux](https://img.shields.io/badge/Linux-Ubuntu_22.04-orange?style=flat-square&logo=ubuntu)
![Kali](https://img.shields.io/badge/Kali_Linux-Attacker-557C94?style=flat-square&logo=kalilinux)
![MITRE](https://img.shields.io/badge/MITRE_ATT%26CK-Mapped-red?style=flat-square)
![VirtualBox](https://img.shields.io/badge/VirtualBox-Virtualization-183A61?style=flat-square)

---

## 🧪 Lab Architecture

| Machine | Role | IP |
|---------|------|----|
| Ubuntu Server 22.04 | Wazuh Manager + Indexer + Dashboard | 192.168.56.10 |
| Ubuntu Desktop 22.04 | Agent (Linux target) | 192.168.56.11 |
| Kali Linux | Attacker (Hydra, Nmap, Netcat) | 192.168.56.12 |
| Windows 10 | Agent (Windows target) | 192.168.56.13 |

All machines communicate over an isolated **Host-only network** (192.168.56.0/24).

---

## 🔍 Detection Scenarios

### Scenario 1 — SSH Brute Force (T1110)
- Tool: **Hydra**
- Wazuh rules triggered: `5760`, `5503`
- 11 alerts generated in seconds
- MITRE: `T1110 Brute Force`, `T1078 Valid Accounts`

### Scenario 2 — Port Scanning / Nmap (T1046)
- Tool: **Nmap** (TCP SYN + UDP)
- Detection via **iptables logging** + custom rules
- Rules triggered: `100100` (TCP), `100101` (UDP) — Level 15
- MITRE: `T1046 Network Service Discovery`

### Scenario 3 — File Integrity Monitoring (T1565)
- Module: **Wazuh Syscheck** + **Auditd** (Whodata mode)
- Monitored directories: `/etc`, `/home`
- Detected: file creation (554), modification (550), deletion (553)
- MITRE: `T1565 Data Manipulation`, `T1070.004 File Deletion`

### Scenario 4 — Unauthorized User Creation (T1136)
- Command: `sudo adduser`
- Detected via FIM on `/etc/passwd`, `/etc/shadow`, `/etc/group`
- Rules triggered: `5902` (new user), `5402` (sudo), `5501/5502` (PAM session)
- MITRE: `T1136 Create Account`

### Scenario 5 — Active Response — Auto IP Block (T1110)
- Wazuh automatically blocks attacker IP via `firewall-drop` script
- Triggered by rule `5551` (multiple SSH failures)
- Block duration: **600 seconds**
- MITRE: `T1110 Brute Force`

---

## 🗂️ Project Structure

```
soc-wazuh-lab/
├── configs/
│   ├── ossec.conf          # Wazuh agent configuration (FIM + Active Response)
│   └── iptables-rules.sh   # iptables logging rules for port scan detection
├── rules/
│   └── local_rules.xml     # Custom Nmap detection rules
├── screenshots/            # Dashboard alerts & detection evidence
└── README.md
```

---

## ⚙️ Key Components

- **Wazuh Manager** — central SIEM/XDR engine (correlation, alerting)
- **Wazuh Indexer** — log storage and search
- **Wazuh Dashboard** — visualization and alert management
- **Syscheck** — File Integrity Monitoring (FIM)
- **Auditd** — kernel-level audit (Whodata enrichment)
- **Active Response** — automated firewall blocking

---

## 📊 Results Summary

| Scenario | Detection Time | Alerts Generated |
|----------|---------------|-----------------|
| SSH Brute Force | ~seconds | 11 |
| Port Scanning | ~seconds | 10 |
| File Integrity | ~seconds | 17 |
| User Creation | ~seconds | 5 |
| Active Response | ~seconds | 3 |

---

## 👩‍💻 Author

**Ikram Ezziouani** — Cybersecurity Student (SOC)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ikram-ezziouani-b7737b25b/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat-square&logo=github&logoColor=white)](https://github.com/ikram856)
