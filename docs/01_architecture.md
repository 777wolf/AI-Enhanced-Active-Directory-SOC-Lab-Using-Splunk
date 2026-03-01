# 01 - Architecture Overview

## 1. Objective

This project implements a Security Operations Center (SOC) lab to monitor Active Directory authentication activity using Splunk Enterprise.

The system detects:

- Brute force attacks
- Password spraying
- Lateral movement
- Privilege escalation
- Behavioral login anomalies (AI-based statistical detection)

---

## 2. Lab Architecture

### Network Mode

VirtualBox Internal Network Mode was used to isolate all VMs.

Advantages:
- No internet exposure
- Safe attack simulation
- Controlled traffic environment

---

## 3. Network Topology

WIN10 (192.168.50.20)  
↓  
DC01 (192.168.50.10)  
↓  
Splunk Server (192.168.50.40)

---

## 4. Data Flow

1. Authentication occurs on WIN10.
2. DC01 processes authentication.
3. Windows Security Event Logs are generated.
4. Splunk Universal Forwarder collects logs.
5. Logs forwarded via TCP port 9997.
6. Splunk Enterprise indexes logs in index=main.
7. Detection rules and AI models analyze data.
8. Dashboard displays SOC monitoring panels.

---

## 5. Components Used

| Component | Role |
|------------|--------|
| DC01 | Domain Controller |
| WIN10 | Client / Attacker |
| SPLUNK-SERVER | SIEM |
| Splunk Universal Forwarder | Log Forwarder |

---

## 6. Security Monitoring Scope

Monitored Event IDs:

- 4624 – Successful Logon
- 4625 – Failed Logon
- 4672 – Special Privileges Assigned
- 4728 – User Added to Global Group
- 4732 – User Added to Local Group
- 4756 – User Added to Universal Group
- 4738 – Account Modified