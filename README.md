# APT28 (Fancy Bear) — Purple Team Emulation & Detection

Purple team exercise emulating APT28 credential-theft tradecraft, 
with end-to-end detection and forensic analysis.

## Overview
- **Threat actor emulated:** APT28 / Fancy Bear (Russian GRU)
- **Attack chain:** T1566.001 → T1204.002 → T1003.001 → T1041
- **Tools:** Sysmon, Wireshark, Volatility 3, winpmem
- **Environment:** Isolated Windows 11 VM

## Key Findings
- LOTL discovery via native binaries (net/ipconfig/tasklist)
- Encrypted SMTP exfiltration over port 587 (Gmail, IPv6)
- Detection engineering for each ATT&CK technique

## Contents
- `report.pdf` — Full analysis report
- `mindmap.png` — Report structure overview

## Disclaimer
Conducted in an isolated lab for educational purposes only.
