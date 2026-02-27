# 05 - Detection Engineering

## 1. Brute Force Detection

index=main host=DC01 (EventCode=4625 OR EventCode=4624)
| bin _time span=5m
| stats count(eval(EventCode=4625)) as failed,
        count(eval(EventCode=4624)) as success
        by _time Source_Network_Address Account_Name
| where failed >= 3 AND success >= 1

MITRE: T1110

---

## 2. Password Spray Detection

index=main host=DC01 EventCode=4625
| stats dc(Account_Name) as unique_users count by Source_Network_Address
| where unique_users >= 3

MITRE: T1110.003

---

## 3. Lateral Movement

index=main host=DC01 EventCode=4624 Logon_Type IN (3,10)

MITRE: T1021

---

## 4. Privilege Escalation

index=main host=DC01 (EventCode=4672 OR EventCode=4728 OR EventCode=4732 OR EventCode=4756 OR EventCode=4738)

MITRE: T1068, T1098