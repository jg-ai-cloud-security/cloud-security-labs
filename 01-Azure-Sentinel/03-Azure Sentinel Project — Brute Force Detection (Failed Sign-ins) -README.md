# cloud-security-labs

Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation




# Azure Sentinel Project — Brute Force Detection (Failed Sign-ins)

## 🎯 Goal
Detect potential **brute-force login attempts** by identifying repeated failed sign-ins from the same IP address and automatically creating incidents in **Microsoft Sentinel**.

This project shows my ability to:
- Build detections using **KQL**
- Create **Analytics Rules**
- Investigate suspicious sign-in behaviour
- Tune alerts to reduce false positives

---

## 🧩 Tools Used
- Microsoft Sentinel (SIEM/SOAR)
- Log Analytics Workspace
- Azure AD / Entra ID Sign-in Logs (`SigninLogs`)
- KQL (Kusto Query Language)
- Analytics Rules + Incidents

---

## ✅ Lab Setup / Prerequisites
Before running the detection:
1. Created a **Log Analytics Workspace**
2. Enabled **Microsoft Sentinel** on the workspace
3. Connected **Azure AD / Entra ID** data connector (SigninLogs)

✅ Output confirms log ingestion:
- `SigninLogs` table available in Log Analytics

---

## 🛠️ Detection Logic (How it Works)
This detection looks for:
- Multiple sign-in failures (`ResultType != 0`)
- From the same **IPAddress**
- Within a short window (15 minutes)
- Over the last 24 hours
- Threshold triggers at `>= 10` failed attempts

✅ This is a common real-world SOC detection for:
- Password spraying
- Brute-force attempts
- Automated authentication attacks

---

## 🔎 KQL Query (Brute Force Detection)
File location:
📁 `01-Azure-Sentinel/kql/brute_force_detection.kql`

```kql
// Brute Force Detection (Starter)
// Finds high failed sign-ins from the same IP

SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0
| summarize FailedAttempts=count(), Users=make_set(UserPrincipalName, 10)
    by IPAddress, bin(TimeGenerated, 15m)
| where FailedAttempts >= 10
| order by FailedAttempts desc


🧠 How I Tuned the Detection (Reducing Noise)

To reduce false positives, I can tune using:

Higher threshold (>= 15 or >= 20)

Exclude trusted IP ranges

Filter by specific risky apps

Add geolocation checks

Detect multiple users from one IP (spraying)

Example tuning idea:

Trigger only if 3+ users are targeted from the same IP in 15 minutes.

🚨 Create Analytics Rule (Step-by-Step)
1) Create the rule

Microsoft Sentinel → Analytics → Create → Scheduled query rule

2) Rule details

Name: Brute Force Login Attempts (Failed Sign-ins)

Severity: Medium (or High if strict threshold)

Tactics: Credential Access

3) Query

Paste the KQL query above

4) Query Scheduling

Run query every: 5 minutes

Lookup data from the last: 1 hour
(Optional: 15–30 mins if you want faster reaction)

5) Alert Threshold

Trigger when results are > 0

6) Entity Mapping

Map entities for better investigation:

IP Address → IPAddress

Account → Users (if possible)

7) Incident settings

✅ Turn ON: Create incidents from alerts

8) Automated response (optional)

Add playbook later to notify Teams/email or block IP (with approval)

🔍 Investigation Steps (SOC Workflow)

When the incident triggers, I validate:

✅ Step 1: Review the incident timeline

See how many failures and from what IP

✅ Step 2: Identify targeted users

Is it 1 account? or multiple users? (password spray)

✅ Step 3: Validate if the IP is suspicious

Is it known corporate IP?

Is it unusual country/location?

✅ Step 4: Check success attempts after failures
If suspicious, check if the attacker eventually succeeded.

Sample query to check if the same IP had success later:

SigninLogs
| where TimeGenerated > ago(24h)
| where IPAddress == "<SUSPICIOUS_IP>"
| summarize Attempts=count(), Success=countif(ResultType == 0), Failures=countif(ResultType != 0)
    by UserPrincipalName
| order by Failures desc


✅ Step 5: Decide response

Reset password / force MFA

Block sign-ins or add conditional access policy

Mark IP as suspicious

📸 Evidence / Results (Screenshots)

All evidence stored in:
📁 01-Azure-Sentinel/screenshots/

Examples to capture:

Analytics Rule created

Incident generated

Query results showing failures

Incident investigation view

🧠 What I Learned

How to detect brute-force patterns using KQL

How to build an Analytics Rule and incident workflow

How to tune detection thresholds to reduce noise

How to investigate failed sign-ins like a real SOC analyst

🚀 Next Improvements

Planned upgrades to make this detection stronger:

Add geolocation anomaly detection (impossible travel concept)

Detect password spraying using multi-user logic

Integrate TI enrichment (IP reputation)

Add SOAR playbook:

Auto alert in Teams

Create ticket

Block IP / disable account (with approval)

