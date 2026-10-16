# Lockdown Lab: Incident Response & Digital Forensics Investigation

Welcome to my repository detailing the investigation of a multi-stage intrusion for the "Lockdown Lab" challenge. In this project, I reconstructed an attacker's complete kill chain against a public-facing IIS server at **TechNova Systems** using network packet captures (PCAP), memory dumps, and recovered malware artifacts.

This write-up and portfolio entry are mapped to the **MITRE ATT&CK** framework to demonstrate practical Digital Forensics and Incident Response (DFIR) methodologies.

---

## 🧭 Scenario Overview
TechNova Systems’ Security Operations Center (SOC) detected anomalous outbound traffic originating from a public-facing, cloud-hosted IIS server. The observed behavior pointed to a potential web-shell deployment followed by covert C2 (Command and Control) communications to an unknown external host.

As the lead forensic examiner, my objective was to analyze three core artifacts—a network PCAP, a full memory dump, and a malware sample recovered from disk—to determine the root cause, reconstruct the attacker’s timeline, and provide actionable containment recommendations.

---

## 🛠️ Tooling & Stack
* **Wireshark:** Network forensics, PCAP analysis, and HTTP payload extraction.
* **Volatility 3 & MemProcFS:** Memory forensics to identify malicious processes, injected memory regions, and active network sockets.
* **FLOSS & Strings:** Static malware analysis to extract obfuscated strings, hardcoded C2 endpoints, and execution routines.
* **Threat Intel Tools:** VirusTotal and IOC lookup engines for artifact attribution and risk assessment.

---

## 🎯 Investigation & Analysis Workflow

### 1. Network Forensics (Wireshark)
* Isolated the initial exploit vector against the IIS server.
* Identified suspicious HTTP POST traffic consistent with web-shell interaction and file drop operations.
* Analyzed encrypted/covert beaconing traffic back to the C2 infrastructure.

### 2. Memory Forensics (Volatility 3 / MemProcFS)
* Dumped and inspected the system's process tree for parent-child anomalies (e.g., `w3wp.exe` spawning command shells).
* Located memory-resident payloads and mapped suspicious network sockets back to specific PIDs.

### 3. Malware & Artifact Analysis (FLOSS / Threat Intel)
* Extracted the dropped executable from the disk image.
* Leveraged FLOSS to deobfuscate runtime strings, uncovering hidden domain names, IP addresses, and command-line execution parameters.

---

## 🗺️ MITRE ATT&CK Mapping

* **Execution:** Arbitrary command execution via web-shell interface.
* **Persistence:** File placement within web root directories to maintain access.
* **Privilege Escalation:** Local system enumeration attempts for elevated access.
* **Stealth (Defense Evasion):** Obfuscated malware strings and traffic encryption.
* **Discovery:** Internal network scanning and system configuration discovery.
* **Lateral Movement:** Internal pivoting attempts from the compromised host.
* **Command and Control (C2):** Outbound HTTP/HTTPS beaconing to an attacker-controlled external node.

---

## 📂 Repository Layout
* `/network_forensics/` — PCAP filters, extracted stream data, and Wireshark notes.
* `/memory_forensics/` — Volatility plugin outputs (`pslist`, `netscan`, `malfind`).
* `/malware_analysis/` — Deobfuscated strings and IOC lists.
* `Incident_Response_Report.md` — The complete forensic investigation report.

---
*Created as part of a hands-on Cyber Defense / DFIR portfolio project.*
