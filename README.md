# 🔐 Microsoft Entra ID Conditional Access Lab  
## Enforcing MFA with Privileged Identity Management & Sign-In Log Validation

<img width="1536" height="1024" alt="10548829-43f0-4ead-acd4-4cd822f87741" src="https://github.com/user-attachments/assets/dbfcb52f-708c-43bd-83ff-08b383397df1" />

**Author:** Felipe Restrepo  
**Category:** Identity & Access Management / Cloud Security  
**Technologies:** Microsoft Entra ID, Conditional Access, PIM, MFA, Sign-in Logs  

---

# 📘 Project Summary

This lab demonstrates how to design, deploy, enforce, and validate a Microsoft Entra Conditional Access policy that requires Multi-Factor Authentication (MFA).

The project follows a real enterprise security workflow:

- Privileged Identity Management (PIM) for just-in-time admin access  
- Conditional Access policy creation  
- Safe deployment using Report-only  
- Live enforcement  
- Real authentication testing  
- Sign-in log and Conditional Access validation  
- Audit evidence collection  

---

# 🎯 Security Objectives

- Enforce MFA using Conditional Access  
- Reduce attack surface by blocking weak authentication paths  
- Secure administrative actions using PIM  
- Validate controls using identity telemetry  
- Simulate SOC-style identity verification  

---

# 🏗 Environment

- Microsoft Entra ID tenant  
- Standard user account (Felipe)  
- Break-glass administrator account  
- Microsoft Authenticator  
- Azure Portal  

---

# 🧪 LAB EXECUTION — STEP BY STEP

---

# 🟦 PHASE 1 — Privileged Access Setup (PIM)

## Purpose
Ensure all sensitive configuration is performed using temporary privileged access.

---

### 1. Open Privileged Identity Management

Path:  
Microsoft Entra ID → Privileged Identity Management → My roles

<img width="110" height="116" alt="Screenshot 2026-01-13 094746" src="https://github.com/user-attachments/assets/c77dffd6-91cc-4eac-b4ae-c7ca765efa5e" />

---

### 2. Activate Global Administrator

- Locate **Global Administrator**
- Click **Activate**
- Set duration (example: 1 hour)
- Enter reason: `Conditional Access Project`
- Click **Activate**
- Approve MFA request

✅ Result: Temporary Global Administrator access enabled

📸 Evidence:
- PIM activation request  
- MFA approval  
- Role activation confirmation  

<img width="1359" height="603" alt="Screenshot 2026-01-13 095238" src="https://github.com/user-attachments/assets/ce154fff-4fcb-4bb2-97e3-9914fdc77b23" />

---

# 🟦 PHASE 2 — Conditional Access Policy Creation

## Purpose
Create a Conditional Access policy requiring MFA.

---

### 3. Open Conditional Access

Path:  
Microsoft Entra ID → Protection → Conditional Access → Policies

<img width="662" height="227" alt="Screenshot 2026-01-13 091756" src="https://github.com/user-attachments/assets/d0bbbfd8-a538-46c6-b9f3-0b063283719f" />

---

### 4. Create new policy

Click: **+ New policy**

Policy name:  
`CA-01 – Require MFA for all users`
Users:
- Include → All users  
- Exclude → Break-glass administrator  

Target resources:
- All cloud apps

<img width="373" height="465" alt="Screenshot 2026-01-13 092140" src="https://github.com/user-attachments/assets/e9e8f671-06c8-4b0d-a9ca-e2b746f40e7a" />

---

### 6. Configure Grant controls

Grant access → Require multi-factor authentication  
For multiple controls → Require one of the selected controls  

Click **Select**

<img width="319" height="546" alt="Screenshot 2026-01-13 092159" src="https://github.com/user-attachments/assets/dff310b8-d944-466f-8ab0-e60dd64b7be8" />

---

### 7. Deploy in Report-only

- Enable policy → Report-only  
- Click **Create**

<img width="1363" height="599" alt="Screenshot 2026-01-13 092311" src="https://github.com/user-attachments/assets/60004957-79b6-4296-b30e-a4254f063c5a" />

✅ Result: Policy created safely without enforcement

📸 Evidence:
- Policy creation  
- Grant configuration  
- Policy listed in Conditional Access  
- Policies list  
- Policy state = ON  

<img width="1357" height="601" alt="Screenshot 2026-01-13 092420" src="https://github.com/user-attachments/assets/b48f0018-97e8-4f89-8cd5-695a2e48906b" />

---

# 🟦 PHASE 3 — Live Sign-In Test

## Purpose
Confirm MFA is enforced during real authentication.

---

### 9. Perform new sign-in

- Open a new incognito/private window  
- Go to: https://portal.azure.com  
- Sign in as Felipe  

Expected behavior:
- Password prompt  
- Microsoft Authenticator number match  
- Successful login  

📸 Evidence:
- Password screen  
- MFA approval request  
- Successful login  

<img width="607" height="431" alt="Screenshot 2026-01-13 093811" src="https://github.com/user-attachments/assets/53c19816-a218-4f23-bf0d-6ea23f3a81c5" />

<img width="521" height="556" alt="Screenshot 2026-01-13 093826" src="https://github.com/user-attachments/assets/7dd829e5-9185-45e7-9129-93db8a72151e" />

---

# 🟦 PHASE 4 — Sign-In Log Validation

## Purpose
Verify Conditional Access enforcement through logs.

---

### 10. Open sign-in logs

Path:  
Microsoft Entra ID → Monitoring → Sign-in logs

Open the most recent sign-in.

<img width="287" height="562" alt="Screenshot 2026-01-13 093946" src="https://github.com/user-attachments/assets/eac3fd0f-7ebb-4027-919c-011d990ff8d7" />

---

### 11. Review Overview tab

Confirm:
- Application
- Status = Success
- Authentication requirement

📸 Screenshot: Overview

<img width="1046" height="142" alt="Screenshot 2026-01-13 094019" src="https://github.com/user-attachments/assets/cc6ff62a-e2dc-4dcb-af6b-e95146acc4a2" />

---

### 12. Review Conditional Access tab

Confirm:
- Conditional Access policies applied  
- MFA under Grant controls  
- Result = Success  

📸 Screenshot: Conditional Access

<img width="837" height="511" alt="Screenshot 2026-01-13 094116" src="https://github.com/user-attachments/assets/e7a10560-8d05-4e10-927c-a19289d2b5e7" />

---

### 13. Review Authentication details tab

Confirm:
- First factor satisfied  
- MFA requirement satisfied  

📸 Screenshot: Authentication details

<img width="854" height="566" alt="Screenshot 2026-01-13 094352" src="https://github.com/user-attachments/assets/f81c5874-df0a-42af-bda8-fb81210b321e" />

## 🔴 Test A — Intentional failed password attempt

### 9A. Simulate failed authentication

- Open new incognito/private window  
- Go to: https://portal.azure.com  
- Enter Felipe’s email  
- Enter an **incorrect password on purpose**

Expected behavior:
- Password rejected  
- Sign-in fails  

📸 Evidence:
- Incorrect password screen  

<img width="526" height="428" alt="Screenshot 2026-01-13 101035" src="https://github.com/user-attachments/assets/37fa528d-e29b-47f4-9039-ca8182718ece" />

---

### 9B. Validate failed sign-in logs

Path:  
Microsoft Entra ID → Monitoring → Sign-in logs  

Open the most recent **failed** sign-in.

Review:

• Overview  
• Conditional Access  
• Authentication details  

Confirm:

- Status = Failure  
- First factor failed  
- Conditional Access evaluated  
- No MFA satisfied  

📸 Evidence:
- Failed sign-in overview  
- Conditional Access evaluation  
- Authentication details

<img width="1359" height="603" alt="Screenshot 2026-01-13 102100" src="https://github.com/user-attachments/assets/8cc17be8-2c3b-4af2-9065-bfe094e5ab65" />

<img width="851" height="309" alt="Screenshot 2026-01-13 102144" src="https://github.com/user-attachments/assets/c51630cb-a146-4b37-9c85-2a19312ef5e2" />

<img width="851" height="265" alt="Screenshot 2026-01-13 102153" src="https://github.com/user-attachments/assets/a954784c-ca30-46b3-9e17-35027f87786f" />

---

# 📊 Security Outcomes

- MFA successfully enforced  
- Conditional Access verified  
- PIM protecting admin access  
- Authentication flow validated  
- Audit trail documented  

---

# 🧠 Skills Demonstrated

- Microsoft Entra Conditional Access  
- Privileged Identity Management (PIM)  
- MFA enforcement  
- Zero Trust access controls  
- Identity telemetry analysis  
- Cloud audit logging  
- SOC-style security validation  
