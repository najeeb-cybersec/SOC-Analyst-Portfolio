# 🛡️ SOC Incident Investigation: Follina RCE (CVE-2022-30190)

---

## 📋 1. Executive Summary
* **Investigator Profile:** Najeeb Ahmed (Aspiring SOC Analyst)
* **Incident Timeline:** Sunday, October 4, 2026
* **Severity Level:** `CRITICAL (P1 Threat Matrix Event)`
* **Triage Status:** **True Positive** (Remediated via Host Isolation)
* **Core Objective:** Investigated a highly deceptive macro-based Microsoft Word document weaponized with the Follina Remote Code Execution (RCE) vector designed to trigger arbitrary code sub-execution.

---

## 🔍 2. Artifacts & Indicators of Compromise (IoCs) Extracted
During the baseline forensic triage phase, the following digital signatures were isolated from the target workspace:

| Artifact Element | Extracted Value / Metadata Signature |
| :--- | :--- |
| **Malicious File Name** | `sample.doc` |
| **SHA1 Hash Fingerprint** | `06727ffda60359236a8029e0b3e8a0fd11c23313` |
| **Verified File Type** | Office Open XML Document (Zipped Archive) |
| **Contacted C2 Destination**| `https://xmlformats.com` |

---

## 🧠 3. Advanced Forensic Deep-Dive & Log Analysis

### A. Document Deconstruction (`XML` Relationships Audit)
By modifying the source asset extension from `.doc` to `.zip` and extraction, the inner structure configuration matrices were evaluated. Inside the configuration path `word/_rels/document.xml.rels`, a malicious external OLE relationship script link targeting a remote HTML file was uncovered.

### B. The 4096-Byte Engine Processing Boundary
Analysis of the attacker's server script highlighted explicit padding techniques. The native Microsoft HTML layout execution processor bypass checks only invoke parsing loops if the payload buffer size exceeds exactly **4096 bytes**. The threat actor bypassed this operational constraint by manually injecting dummy comment strings into the source file.

### C. Host Process Tree Deflection Tracking (Sysmon Event ID 1 / Windows 4688)
Granular process creation logs flagged a severe structural execution anomaly:
* **Parent Image:** `C:\Program Files\Microsoft Office\Office16\winword.exe`
* **Child Process Spawned:** `C:\Windows\System32\msdt.exe` (Microsoft Support Diagnostic Tool)
* **Captured Command Line Parameter:** `taskkill /f /im msdt.exe`
* *Analysis:* Under standard enterprise baselines, Microsoft Word has zero operational dependencies to spawn diagnostic utilities. The command line syntax immediately attempts a forced silent termination of `msdt.exe` tasks to completely suppress default OS crash warnings and evade analyst monitoring.

---

## 🛠️ 4. Tactical Incident Response Playbook Execution
Upon validation of the threat vectors, the following containment loop was systematically deployed:
1. **Network Containment:** Triggered instant interface isolation rules via the centralized EDR console dashboard for the infected machine node to halt lateral movement.
2. **Perimeter Hardening:** Injected blocking records for the remote command domain (`xmlformats.com`) across edge gateway routing tables.
3. **Session Revocation:** Invalidated active authentication tokens and forced high-complexity password modifications across core Active Directory sync points.

---

## 📊 5. Reference Matrix Mapping
* **MITRE ATT&CK Tactic:** `TA0002 - Execution`
* **MITRE ATT&CK Technique ID:** `T1059` (Command and Scripting Interpreter)
* **Associated CVE Mapping:** CVE-2022-30190
