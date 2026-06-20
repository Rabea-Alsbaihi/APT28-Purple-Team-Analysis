# 🐻 APT28 (Fancy Bear) — Purple Team Emulation & Detection

![Status](https://img.shields.io/badge/Status-Completed-success)
![Type](https://img.shields.io/badge/Type-Purple%20Team-blueviolet)
![Framework](https://img.shields.io/badge/MITRE-ATT%26CK-red)
![Tools](https://img.shields.io/badge/Tools-Sysmon%20%7C%20Wireshark%20%7C%20Volatility3-informational)
![Environment](https://img.shields.io/badge/Lab-Isolated%20Windows%2011%20VM-orange)

> A controlled purple team exercise emulating the credential-theft tradecraft of **APT28 (Fancy Bear / BlueDelta)** — executed in an isolated lab, then detected and investigated end-to-end using industry-standard blue team tooling.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Attack Chain](#-attack-chain)
- [Repository Structure](#-repository-structure)
- [Tools & Telemetry](#-tools--telemetry)
- [Key Findings](#-key-findings)
- [Detection Highlights](#-detection-highlights)
- [How to Use This Repository](#-how-to-use-this-repository)
- [Disclaimer](#-disclaimer)
- [Contact & Connect](#-contact--connect)

---

## 🎯 Overview

| Field | Detail |
| --- | --- |
| **Threat Actor Emulated** | APT28 / Fancy Bear / Sednit (Russian GRU, Unit 26165) |
| **Exercise Type** | Purple Team — Attack Emulation + Blue Team Detection |
| **Environment** | Windows 11 Pro — Isolated VM (VMware Workstation Pro) |
| **Tactics** | Credential Harvesting + Exfiltration |
| **Evidence Sources** | Email artifacts, Sysmon logs, memory image, packet capture |

The exercise simulates how APT28 moves from an innocent-looking phishing email to stolen credentials, then walks through detecting every stage with host, network, and memory forensics.

---

## ⛓️ Attack Chain

```
T1566.001  →  T1204.002  →  T1087/T1016/T1057/T1033  →  T1003.001  →  T1041
Spearphish    Execution        Discovery (LOTL)          Cred Access   Exfiltration
```

| Stage | Technique | MITRE ID | Evidence |
| --- | --- | --- | --- |
| Initial Access | Spearphishing Attachment | `T1566.001` | Email analysis |
| Execution | User Execution: Malicious File | `T1204.002` | Sysmon EID 1 |
| Discovery (LOTL) | Account / Network / Process / Owner | `T1087/16/57/33` | Process tree |
| Credential Access | OS Credential Dumping: LSASS | `T1003.001` | Sysmon EID 10 (expected) |
| Exfiltration | Exfiltration Over C2 Channel | `T1041` | Packet capture (587) |

---

## 📂 Repository Structure

| Folder / File | Description |
| --- | --- |
| `Report/` | Full purple team incident analysis report (PDF). |
| `Diagrams/` | Attack/defense mind map and process-tree visuals. |
| `IOCs/` | Indicators of Compromise — hashes, network, host. |
| `Detection-Rules/` | Sysmon, Suricata, and behavioral detection content. |
| `Evidence/` | Sanitized excerpts of Sysmon / Wireshark / Volatility output. |

---

## 🛠️ Tools & Telemetry

| Tool | Purpose |
| --- | --- |
| **Sysmon v15.20** | Host process / network / file telemetry |
| **Wireshark** | Network packet capture & analysis |
| **winpmem** | Live memory acquisition |
| **Volatility 3** | Memory forensic analysis |
| **MITRE ATT&CK** | Technique mapping & detection framework |

---

## 🔑 Key Findings

- **Living-off-the-Land (LOTL) discovery:** reconnaissance performed entirely via native Windows binaries (`net`, `ipconfig`, `tasklist`) to blend with legitimate activity.
- **Interpreter spawning shells:** `python.exe` running at **High Integrity** spawned multiple `cmd.exe` children — an abnormal parent/child pattern.
- **Encrypted exfiltration:** outbound **TLS 1.3** session to Gmail SMTP over **port 587** (SNI `smtp.gmail.com`), observed over IPv6.
- **Baseline vs. threat:** distinguished benign LSASS access (Defender / svchost, `0x101000`) from the malicious credential-dumping signature (`0x1010`).

---

## 🛡️ Detection Highlights

| Technique | Detection Approach |
| --- | --- |
| `T1003.001` | Sysmon EID 10 — alert on `GrantedAccess 0x1010` from a non-system process against `lsass.exe`. |
| `T1204.002` | Sysmon EID 1 — interpreter (`python.exe`) spawning `cmd.exe` recon shells. |
| `T1041` | SNI inspection + process-to-port correlation for SMTP egress outside the corporate relay (IPv4 **and** IPv6). |

---

## 🚀 How to Use This Repository

| Action | Description |
| --- | --- |
| Read the **Report** | Full step-by-step methodology and analysis. |
| Review **Detection-Rules** | Reusable Sysmon / Suricata content for SOC use. |
| Study the **Diagrams** | Visual overview of the attack and detection coverage. |
| Reference **IOCs** | Indicators for threat hunting and detection tuning. |

---

## ⚠️ Disclaimer

This project was conducted **strictly within an isolated lab environment for educational and authorized security-training purposes**. All simulated attack techniques must only be executed in controlled environments. Unauthorized use against live systems is illegal and unethical.

---

## 📬 Contact & Connect

- **Name:** Rabea Alsbaihi
- **Role:** SOC / Incident Response Specialist
- **LinkedIn:** [linkedin.com/in/rabea-alsbaihi](https://www.linkedin.com/in/rabea-alsbaihi/)
- **Email:** rabea.alsbaihi@gmail.com

---

<p align="center"><i>Educational purple team exercise — APT28 emulation & detection.</i></p>
