# cloud-security-labs

Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation



# Microsoft Sentinel — Threat Hunting & Brute Force Detection (KQL + Analytics Rule)

## 🎯 Goal
Detect **brute-force** and **password spraying** attempts by identifying repeated failed sign-ins, then automatically generate incidents in **Microsoft Sentinel**.

This project demonstrates my ability to:
✅ Build KQL detections  
✅ Create Analytics Rules (SIEM detections)  
✅ Investigate incidents like a SOC analyst  
✅ Tune alerts to reduce false positives  
✅ Document results professionally for real-world use  

---

## 🧩 Tools Used
- Microsoft Sentinel (SIEM/SOAR)
- Log Analytics Workspace
- Entra ID (Azure AD) Sign-in Logs
- KQL (Kusto Query Language)
- Sentinel Analytics Rules + Incidents

---

## ✅ Lab Setup / Prerequisites
Before deploying the detection:
1. Created a **Log Analytics Workspace**
2. Enabled **Microsoft Sentinel**
3. Connected the **Entra ID Sign-in Logs** data connector
4. Confirmed data ingestion into the **SigninLogs** table

✅ Validation Query (check logs exist):
```kql
SigninLogs
| take 10


🔥 Detection Use Case (SOC Real World)

This rule helps detect:

Brute-force login attempts against a single user

Password spraying (many users targeted from one IP)

Automated login attacks from unknown IP addresses

🛠️ Detection Logic (How It Works)

The detection searches for:

Failed sign-ins (ResultType != 0)

Same IP Address making repeated attempts

Within a short time window (15 minutes)

Over the last 24 hours

If the failures are above a threshold (default = 10)

🔎 KQL Query — Brute Force Detection

📁 Saved in: /kql/brute_force_detection.kql

// Brute Force Detection (Starter)
// Finds high failed sign-ins from the same IP

SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0
| summarize FailedAttempts=count(), Users=make_set(UserPrincipalName, 10)
    by IPAddress, bin(TimeGenerated, 15m)
| where FailedAttempts >= 10
| order by FailedAttempts desc

✅ Create Analytics Rule (Step-by-Step)
Step 1 — Create a new rule

Microsoft Sentinel → Analytics → Create → Scheduled query rule

Step 2 — Rule details

Name: Brute Force Login Attempts (Failed Sign-ins)

Description: Detect repeated failed sign-ins from a single IP address

Severity: Medium (or High if required)

Tactics: Credential Access

Techniques (optional): Brute Force

Step 3 — Set rule query

Paste the KQL query above

Step 4 — Query scheduling

Recommended settings:

Run query every: 5 minutes

Lookup data from the last: 1 hour

Step 5 — Alert logic

Create alert when results are greater than 0

Step 6 — Entity Mapping (for investigation)

Map these entities for better SOC visibility:

IP → IPAddress

Account → UserPrincipalName (if mapped separately)

Step 7 — Incident settings

✅ Enable: Create incidents from alerts

Step 8 — Save

Save the Analytics Rule and test it using activity or simulated failed logins.

🔍 Investigation Process (SOC Workflow)
1) Validate the Alert

I verify:

How many failures occurred

Time pattern (spike vs continuous)

Which accounts were targeted

If the IP address is internal or external

2) Check if the attacker got success later (Critical)

This query checks whether the same IP eventually succeeded:

SigninLogs
| where TimeGenerated > ago(24h)
| where IPAddress == "<SUSPICIOUS_IP>"
| summarize Attempts=count(),
          Success=countif(ResultType == 0),
          Failures=countif(ResultType != 0)
          by UserPrincipalName
| order by Failures desc


✅ If Success > 0 after many failures, it becomes HIGH priority.

3) Identify Password Spraying (multi-user attack)

This version detects multiple users targeted from one IP:

SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0
| summarize FailedAttempts=count(), TargetedUsers=dcount(UserPrincipalName)
    by IPAddress, bin(TimeGenerated, 15m)
| where FailedAttempts >= 10 and TargetedUsers >= 3
| order by FailedAttempts desc

🎯 Tuning Improvements (Reduce False Positives)

I improve detection quality by:

Increasing threshold from >=10 to >=15 or >=20

Filtering trusted company IPs

Excluding expected service accounts

Adding geo checks for suspicious countries

Looking for repeated attempts across multiple applications

📊 Workbook Ideas (SOC Visibility)

Recommended visuals to add later:
✅ Failed Sign-ins by IP
✅ Failed Sign-ins over time
✅ Top targeted users
✅ Incidents created per day/week

(Workbooks can be stored in /workbooks/ with screenshots in /screenshots/)

📸 Evidence / Results

📁 Screenshots saved in: /screenshots/

Recommended proof screenshots:
✅ Analytics Rule created
✅ Query results showing brute-force behaviour
✅ Incident created in Sentinel
✅ Incident investigation view

🧠 What I Learned

How to create real-world Sentinel detections using KQL

How to build Analytics Rules for incident generation

How to investigate suspicious sign-in behaviour

How to tune rules to reduce SOC noise and false positives

🚀 Next Improvements

Planned upgrades:

Add anomaly-based sign-in detection

Add impossible travel / unusual location sign-ins

Add automation playbook to notify Teams/email

Add Threat Intelligence enrichment (IP reputation)

✅ Status: Portfolio-ready Sentinel detection project


---

# ✅ After pasting it (IMPORTANT)
Go back to **GitHub Desktop** and do:

✅ Commit message: `Add Sentinel brute force detection README + KQL queries`  
✅ **Commit to main**  
✅ **Push origin**

---

If you want, next I can also write for you:

✅ `cloud-security-labs/README.md` (main homepage)  
so the whole repo looks clean like a professional portfolio.

Just say: **yes build main repo README** ✅
::contentReference[oaicite:0]{index=0}

