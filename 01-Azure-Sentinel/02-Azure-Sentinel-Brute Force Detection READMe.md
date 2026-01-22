# cloud-security-labs



Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation



\# Project: Azure Sentinel Threat Hunting (Brute Force Detection)



\## 🎯 Goal

Detect brute-force sign-in attempts and generate incidents automatically.



\## 🧩 Tools Used



\- Microsoft Sentinel

\- Log Analytics Workspace

\- KQL (Kusto Query Language)

\- Azure AD / Sign-in logs (or Windows Security Events)



\## 🏗️ Architecture / Setup

(Add 1 screenshot or diagram here)



\## ✅ Steps (Build Guide)

1\. Created Sentinel workspace

2\. Connected data sources (Azure AD / Security Events / Defender)

3\. Wrote KQL query to detect brute-force behaviour

4\. Created Analytics Rule + Incident creation

5\. (Optional) Added automated response with Logic Apps



\## 🔎 KQL Query

Stored in: `/kql/`



\## 📸 Results (Evidence)

Screenshots saved in: `/screenshots/`

\- Incident created

\- Workbook view

\- Alert timeline



\## 🧠 What I Learned

\- Example: How to tune thresholds to reduce false positives

\- Example: How to validate detection using sample sign-in activity

\- Example: How to pivot from alert → investigation



\## 🚀 Next Improvements

\- Add Geo-location anomaly detection

\- Add user/entity behaviour enrichment

\- Add automated containment response



