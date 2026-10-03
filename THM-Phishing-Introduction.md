# 🛡️ SOC Baseline Simulation: Introduction to Phishing Log Analysis

---

## 📋 1. Executive Summary
* **Investigator Profile:** Najeeb Ahmed (Aspiring SOC Analyst)
* **Severity Level:** `LOW (Baseline Training)`
* **Triage Status:** **True Positive** (Simulation Triage Complete)
* **Core Objective:** Successfully simulated an introductory phishing detection playbook run. Analyzed basic email routing traces, source indicators validation loops, and verified domain spoofing identifiers parameters.

---

## 🔍 2. Investigative Workflow & Log Extraction Steps

### A. Domain Integrity Checks (SPF & DKIM Audit)
During the simulation analysis, I triaged mail headers parameters to verify compliance structures against organizational domain policies:
- Audited how cyber threat actors register look-alike domains to target corporate employees.
- Evaluated **Sender Policy Framework (SPF)** pass/fail variables to track source IP authorization metrics.
- Inspected **DKIM (DomainKeys Identified Mail)** signatures to ensure the message payload alignment was not tampered with during transition paths.

### B. Link & Attachment Safety Profiling
- Traced the methods used to identify **Credential Harvesting URLs** hidden beneath deceptive hyperlinks or images.
- Documented standard practices for safely collecting suspect document attachment file hashes (`MD5/SHA256`) without executing threats inside host platforms.
- Utilized OSINT reputation metrics platforms (**VirusTotal**) to cross-reference extracted indicators configurations.

---

## 📊 3. Reference Matrix Mapping
* **MITRE ATT&CK Tactic:** `TA0001 - Initial Access`
* **MITRE ATT&CK Technique ID:** `T1566` (Phishing Email Delivery)
