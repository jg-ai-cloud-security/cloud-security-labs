# cloud-security-labs

Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation



# Microsoft Defender XDR — Incident Response & Threat Hunting (SOC Workflow)

## 🎯 Goal
Demonstrate real-world **SOC investigation skills** using Microsoft Defender XDR:
✅ Alert triage  
✅ Incident investigation  
✅ Advanced hunting (KQL)  
✅ Evidence collection and response actions  

This project shows I can investigate security incidents end-to-end in a Microsoft security environment.

---

## 🧩 Tools Used
- Microsoft Defender XDR
- Microsoft Defender for Endpoint (EDR)
- Incidents & Alerts
- Advanced Hunting (KQL)
- Microsoft Security Portal

---

## ✅ What This Project Covers
- How I triage and validate alerts
- How I investigate endpoints and suspicious behaviour
- How I use hunting queries to confirm threat activity
- How I document evidence for reporting and learning

---

## 🛠️ SOC Incident Response Workflow (Step-by-Step)

### 1) Alert Triage (First 3 minutes)
I check:
- Severity and alert category
- What devices and accounts are affected
- Is the alert a real threat or expected activity?
- Does it match suspicious behaviour?

✅ Outcome: True Positive / False Positive decision

---

### 2) Incident Investigation
I review:
- Incident timeline (sequence of events)
- Evidence (processes, files, network connections)
- User actions (sign-ins and account behaviour)
- Any indicators of compromise (IOCs)

✅ Outcome: Confirm what happened and how it started

---

### 3) Advanced Hunting (KQL)
I use hunting queries to:
- Identify suspicious PowerShell activity
- Detect malicious persistence techniques
- Check if the behaviour exists across other devices

✅ Outcome: Scope the incident (1 device or many)

---

### 4) Response Actions (Safe Containment)
Depending on findings:
- Isolate device (if required)
- Stop suspicious process
- Block file hash / IOC (if applicable)
- Reset compromised user account
- Record actions in incident notes

✅ Outcome: Contain the threat and prevent spread

---

### 5) Post-Incident Documentation
I document:
- Root cause
- Timeline summary
- Evidence and screenshots
- Lessons learned
- Recommended improvements

✅ Outcome: Better detection and future prevention

---

## 🔎 Advanced Hunting Queries (Starter Set)

📁 Stored in: `/hunting-queries/`

### A) Suspicious PowerShell Commands
```kql
DeviceProcessEvents
| where TimeGenerated > ago(7d)
| where ProcessCommandLine has "powershell"
| where ProcessCommandLine has_any ("-enc", "IEX", "DownloadString", "Invoke-WebRequest")
| project TimeGenerated, DeviceName, AccountName, ProcessCommandLine
| order by TimeGenerated desc



B) Suspicious Command-Line Tools (LOLbins)

kql

DeviceProcessEvents
| where TimeGenerated > ago(7d)
| where FileName has_any ("cmd.exe","powershell.exe","wscript.exe","cscript.exe","rundll32.exe","regsvr32.exe")
| project TimeGenerated, DeviceName, AccountName, FileName, ProcessCommandLine
| order by TimeGenerated desc



C) Registry Persistence Checks (Run / RunOnce)

kqL

DeviceRegistryEvents
| where TimeGenerated > ago(7d)
| where RegistryKey has_any ("Run", "RunOnce")
| project TimeGenerated, DeviceName, RegistryKey, RegistryValueName, RegistryValueData
| order by TimeGenerated desc

📸 Evidence / Results (Screenshots)

📁 Save all proof here: /screenshots/

Recommended screenshots:
✅ Incident summary page
✅ Timeline evidence view
✅ Hunting query output
✅ Device response actions (if tested in lab)

🧠 What I Learned

How to triage Defender XDR incidents quickly

How to pivot from alert → endpoint → evidence

How to use hunting queries to confirm suspicious activity

How to document investigations like a professional SOC engineer

🚀 Next Improvements

Add email/phishing investigation workflow

Add Defender XDR to Sentinel integration use-case

Add MITRE ATT&CK mapping for key alerts

Build automated response playbooks via Sentinel SOAR

✅ Status: Portfolio-ready (SOC workflow + hunting queries)

