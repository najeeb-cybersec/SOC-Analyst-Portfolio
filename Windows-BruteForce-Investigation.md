# 🛡️ SOC Incident Triage: Windows Host RDP Brute Force Attack

---

## 📋 1. Executive Summary
* **Investigator Profile:** Najeeb Ahmed (Aspiring SOC Analyst)
* **Severity Level:** `MEDIUM (Triage Required)`
* **Triage Status:** **True Positive** (Mitigated via Perimeter Firewall Rule)
* **Core Objective:** Investigated a high-volume authentication anomaly targeting the local corporate Active Directory server via Remote Desktop Protocol (RDP).

---

## 🔍 2. Artifacts & Indicators of Compromise (IoCs) Extracted

| Artifact Element | Extracted Value / Metadata Signature |
| :--- | :--- |
| **Attacking Source IP** | `185.220.101.4` (Tor Exit Node Segment) |
| **Target Destination Host** | `AD-DOMAIN-CONTROLLER` |
| **Target Destination IP** | `192.168.10.20` |
| **Target Port Hit** | Port 3389 (Remote Desktop Protocol) |
| **Target Usernames List** | `administrator`, `admin`, `root`, `guest` |

---

## 🧠 3. Deep Log Correlation & Verification (Splunk Analytics)

### A. Volumetric Authentication Audit (Windows Event ID 4625)
By running granular searches on the Windows Security event index, I tracked over **25,000 continuous authentication failure events (Event ID 4625)** generated within a tight 5-minute timeline window. The strict mathematical distribution pattern confirms automated dictionary-based password guessing.

### B. Logon Type Verification Matrix
Inside the security event logs telemetry, the metadata parameter was inspected:
- **Logon Type Flag:** `Logon Type 10` (Remote Interactive / Remote Desktop Protocol connection).
*Analysis:* The high frequency of failed logins under Logon Type 10 explicitly maps to a critical internet-exposed perimeter configuration loophole where Port 3389 was open to unverified foreign IP grids.

### C. Success vs Failure Assessment (Event ID 4624 Check)
I cross-referenced the timeline matrix to check if any successful authentication occurred after the brute force loops. Zero instances of **Event ID 4624** matching the attacker's source IP address were found, confirming the network perimeter compromise attempt was successfully defended.

---

## 🛠️ 4. Playbook Remediation Actions
1. **Network Layer Invalidation:** Issued an urgent request to the network boundary engineering desk to manually update external perimeter firewall policies, dropping all inbound connection requests originating from `185.220.101.4`.
2. **Account Hardening:** Verified enterprise local account lockout threshold behaviors are functioning correctly to prevent potential credential leaks.

---

## 📊 5. Reference Matrix Mapping
* **MITRE ATT&CK Tactic:** `TA0006 - Credential Access`
* **MITRE ATT&CK Technique ID:** `T1110.001` (Brute Force: Password Guessing)
