# 03 - Active Directory Setup

## 1. Install AD DS Role

Server Manager → Add Roles and Features  
Select: Active Directory Domain Services

---

## 2. Promote to Domain Controller

- Add new forest
- Domain name: adai.local
- Set DSRM password
- Reboot

---

## 3. Create Organizational Units

Created:
- SOC_Users
- SOC_Admins

---

## 4. Create Test Users

Users created:

- rohit
- helpdesk
- admin2
- john

Password never expires enabled (lab environment only).

---

## 5. Enable Audit Policies

Commands used:
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
auditpol /set /subcategory:"Account Logon" /success:enable /failure:enable
auditpol /set /subcategory:"Security Group Management" /success:enable /failure:enable
auditpol /set /subcategory:"User Account Management" /success:enable /failure:enable

---

## 6. Domain Join (WIN10)

- Set DNS to 192.168.50.10
- Join domain adai.local
- Login using domain credentials