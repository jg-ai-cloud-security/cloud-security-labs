# cloud-security-labs

Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation



---

# ✅ 03-Entra-ID/README.md (FULL)

```md
# Entra ID Security Baseline — Conditional Access, MFA & Identity Protection (SMB Ready)

## 🎯 Goal
Build a strong, realistic **identity security baseline** for Azure/Office 365 environments using Microsoft Entra ID.

This project is designed as an **SMB-ready security baseline**:
✅ MFA enforcement  
✅ Conditional Access policies  
✅ Admin protection  
✅ Least privilege and safe configuration  

---

## 🧩 Tools Used
- Microsoft Entra ID (Azure AD)
- Conditional Access
- Multi-Factor Authentication (MFA)
- Identity Protection (if enabled)
- RBAC / Least Privilege principles

---

## ✅ What I Implemented

### 1) Strong MFA Setup
- MFA enabled for all user accounts
- Stronger controls for admin accounts
- Option to use Authenticator app

---

### 2) Conditional Access Policies (Core Set)
Example policies I build:

✅ **Policy 1: Require MFA for All Users**
- All users must complete MFA when accessing cloud apps

✅ **Policy 2: Block Legacy Authentication**
- Blocks older protocols that bypass MFA (high risk)

✅ **Policy 3: Admin Protection**
- Admin role accounts require MFA every time
- Higher risk controls for privileged roles

✅ **Policy 4: Risk-Based Sign-in Controls (Optional)**
- Trigger MFA / block if sign-in risk is detected  
*(depending on tenant features)*

---

### 3) Least Privilege Design
- Avoid using Global Admin daily
- Limit privileged accounts
- Secure access based on roles and tasks

---

### 4) Break-Glass Account (Safety Best Practice)
To avoid lockout:
- Create 1 emergency admin account
- Exclude it from Conditional Access
- Store credentials securely
- Monitor sign-ins closely

✅ This keeps business operational during policy changes.

---

## 🛠️ Step-by-Step Build Guide

### Step 1 — Create Security Groups
Create groups:
- `SG-AllUsers`
- `SG-Admins`
- `SG-BreakGlass-Excluded`

---

### Step 2 — Enable MFA
Enforce MFA for:
- All users (baseline)
- Admin group (strong requirement)

---

### Step 3 — Create Conditional Access Policies
Create the core policies:

1) **Require MFA for All Users**  
2) **Block Legacy Authentication**  
3) **Admins: Require MFA Every Sign-in**  
4) **(Optional) Risk-based access controls**  

---

### Step 4 — Test Policies Safely
Validate with test accounts:
✅ Normal user login (MFA should prompt)  
✅ Admin login (stronger access required)  
✅ Confirm the break-glass account is excluded  

---

### Step 5 — Review Sign-in Logs
Verify the policies apply:
- Identify policy applied
- Confirm MFA succeeded
- Check sign-in risk / failures

---

## 📸 Evidence / Results
📁 Screenshots stored in: `/screenshots/`

Recommended evidence screenshots:
✅ Conditional Access policy list  
✅ MFA prompt during sign-in  
✅ Sign-in logs showing policy applied  
✅ Admin account protected configuration  

---

## 🧠 What I Learned
- How to implement Conditional Access safely without locking users out
- How MFA and policy enforcement reduces account compromise
- How to protect admins using least privilege principles
- How to validate configurations using sign-in logs

---

## 🚀 Next Improvements
- Add Intune compliance integration (require compliant devices)
- Add location-based policy controls
- Add guest user restrictions for external access
- Add Defender for Cloud Apps integration (CASB)

✅ Status: SMB-ready identity security baseline
