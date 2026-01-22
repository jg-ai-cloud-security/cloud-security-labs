# cloud-security-labs

Hands-on Microsoft security labs: Sentinel, Defender XDR, Entra ID, Automation



# Project: Sentinel SOAR Automation (Logic Apps)

## 🎯 Goal
Automate incident response actions in Microsoft Sentinel using Logic Apps (SOAR) to reduce manual SOC work and speed up response.

## 🧩 Tools Used

- Microsoft Sentinel
- Logic Apps (Playbooks)
- Automation Rules
- Email / Teams Notifications (optional)

## ✅ Steps (Build Guide)

1. Create a Sentinel Playbook in Logic Apps
2. Configure the trigger (incident created or alert triggered)
3. Add actions (send email / Teams message / add comment to incident)
4. Attach playbook to an Automation Rule in Sentinel
5. Test the workflow using a sample incident

## 📸 Evidence / Results

Screenshots saved in: `/screenshots/`
- Playbook designer view
- Automation rule linked to the playbook
- Proof of notification/message output

## 🧠 What I Learned

- How Sentinel automation improves SOC response time
- How to build safe playbooks without causing disruption
- How to document automation workflows clearly

## 🚀 Next Improvements

- Add IP reputation enrichment
- Add auto ticket creation (Service Desk)
- Add approval step before containment actions
