# cloud-security-labs

Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation



# Microsoft Defender XDR — Incident Response & Advanced Hunting

## 🎯 Goal
Demonstrate real-world **incident triage and investigation** using Microsoft Defender XDR, including alert validation, endpoint evidence review, and hunting queries to identify malicious behaviour.

This project shows I can work like a SOC analyst:
✅ Triage → Investigate → Hunt → Contain → Report

---

## 🧩 Tools Used
- Microsoft Defender XDR (Incidents & Alerts)
- Microsoft Defender for Endpoint (EDR)
- Advanced Hunting (KQL)
- Microsoft security portal

---

## ✅ What I Built
- A structured incident response workflow for XDR alerts
- Hunting queries for suspicious process and persistence indicators
- Evidence screenshots and investigation notes
- A repeatable method to reduce false positives

---

## 🔥 Incident Response Workflow (SOC Style)

### 1) Alert Triage
- Validate alert severity and category
- Check affected devices and user accounts
- Identify alert source (Endpoint / Identity / Email)

### 2) Initial Investigation
- Review incident timeline + evidence
- Look for suspicious processes, file hashes, URLs, IPs
- Confirm whether it’s true positive or false positive

### 3) Advanced Hunting (KQL)
- Search across device activity
- Identify lateral movement or persistence indicators
- Pivot from 1 endpoint to the entire environment

### 4) Containment & Remediation (Safe Actions)
- Isolate device (if required)
- Block file hash / IOC
- Reset account credentials (if identity suspected)
- Document findings + close incident with notes

---

## 🔎 Advanced Hunting Queries (Starter Set)
Saved in: `/hunting-queries/`

### A) Suspicious PowerShell Activity
```kql
DeviceProcessEvents
| where ProcessCommandLine has "powershell"
| where ProcessCommandLine has_any ("-enc", "IEX", "DownloadString", "Invoke-WebRequest")
| project TimeGenerated, DeviceName, AccountName, ProcessCommandLine
| order by TimeGenerated desc
B) Rare Child Processes (Potential Malware)
DeviceProcessEvents
| summarize count() by InitiatingProcessFileName, FileName
| order by count_ asc
C) New or Unusual Autoruns
DeviceRegistryEvents
| where RegistryKey has_any ("Run", "RunOnce")
| project TimeGenerated, DeviceName, RegistryKey, RegistryValueName, RegistryValueData
| order by TimeGenerated desc
📸 Evidence / Results
Screenshots stored in: /screenshots/
Include:

Incident summary view

Timeline evidence

Advanced hunting query results

Device isolation/remediation action logs (if lab allows)

🧠 What I Learned
How to validate XDR alerts fast (triage)

How to investigate endpoints and understand attack chains

How to hunt suspicious behaviour with KQL

How to document incidents clearly for reporting

🚀 Next Improvements
Add phishing-related hunting queries

Add automated response via Sentinel + Logic Apps

Add MITRE ATT&CK mapping for each incident type