**AI-Enhanced Active Directory SOC Lab Using Splunk**
📌 Project Overview
This project demonstrates the design and implementation of a Security Operations Center (SOC) lab to monitor Active Directory authentication events using Splunk Enterprise.

The lab simulates real-world attack scenarios and implements:
Brute force detection
Password spray detection
Lateral movement detection
Privilege escalation monitoring
AI-based login anomaly detection
Risk-based scoring model

The objective is to replicate enterprise SOC monitoring in an isolated virtual lab environment.

**Lab Architecture**
Network Design (Internal Network Mode)
WIN10 (192.168.50.20)
        ↓
DC01 (192.168.50.10)
        ↓
Splunk Server (192.168.50.40)

Isolated internal network

No internet exposure

Controlled attack simulation

**Environment Setup**
Component	Role	IP
DC01	Domain Controller	192.168.50.10
WIN10	Client / Attacker	192.168.50.20
SPLUNK-SERVER	SIEM	192.168.50.40

**Resources:**
16 GB RAM total
4 GB allocated per VM
Internal Network mode in VirtualBox

**Active Directory Configuration**

Installed AD DS role
Created domain: adai.local
Created test users:
rohit
helpdesk
admin2
john

**Enabled advanced audit policies:**

Logon
Account Logon
Security Group Management
User Account Management

**Splunk Deployment**
Splunk Enterprise (Ubuntu)
Installed Splunk Enterprise
Enabled receiving port 9997
Web interface: http://192.168.50.40:8000

**Universal Forwarder (DC01)**

Configured:
**outputs.conf**
[tcpout]
defaultGroup = default-autolb-group
[tcpout:default-autolb-group]
server = 192.168.50.40:9997

**inputs.conf**

[WinEventLog://Security]
disabled = 0
index = main

[WinEventLog://System]
disabled = 0
index = main

[WinEventLog://Application]
disabled = 0
index = main

**Event IDs Monitored**
Event ID	Description
4624	Successful logon
4625	Failed logon
4672	Special privileges assigned
4728	User added to global group
4732	User added to local group
4756	User added to universal group
4738	Account modified

**Detection Engineering**

1️⃣ **Brute Force Detection**
index=main host=DC01 (EventCode=4625 OR EventCode=4624)
| bin _time span=5m
| stats count(eval(EventCode=4625)) as failed,
        count(eval(EventCode=4624)) as success
        by _time Source_Network_Address Account_Name
| where failed >= 3 AND success >= 1

MITRE:
T1110 – Brute Force

2️⃣ **Password Spray Detection**
index=main host=DC01 EventCode=4625
| stats dc(Account_Name) as unique_users count by Source_Network_Address
| where unique_users >= 3

MITRE:
T1110.003 – Password Spraying

3️⃣ **Lateral Movement Detection**
index=main host=DC01 EventCode=4624 Logon_Type IN (3,10)
| table _time Account_Name Source_Network_Address Logon_Type

MITRE:
T1021 – Remote Services

4️⃣ **Privilege Escalation Detection**
index=main host=DC01 (EventCode=4672 OR EventCode=4728 OR EventCode=4732 OR EventCode=4756 OR EventCode=4738)
| table _time EventCode Account_Name Target_Account_Name Group_Name

MITRE:
T1068 – Privilege Escalation
T1098 – Account Manipulation

**AI-Based Anomaly Detection**
Behavioral Baseline Model

Uses statistical anomaly detection:

Threshold = Average + (2 × Standard Deviation)

**SPL:**

index=main host=DC01 EventCode=4624 earliest=-24h
| bin _time span=1h
| stats count as login_count by _time Account_Name
| eventstats avg(login_count) as avg_login stdev(login_count) as stdev_login by Account_Name
| eval threshold = avg_login + (2*stdev_login)
| where login_count > threshold

**Technique:**
Unsupervised anomaly detection using 2-sigma rule.

**Risk Scoring Model**
index=main host=DC01 (EventCode=4625 OR EventCode=4624)
| stats count(eval(EventCode=4625)) as failed,
        count(eval(EventCode=4624)) as success
        by Source_Network_Address
| eval risk_score = (failed*2) + success
| sort - risk_score

**Logic:**

Failed login weighted ×2
Successful login weighted ×1
Used to prioritize suspicious IP addresses.

**SOC Dashboard Panel**s

Authentication Failures Timeline
Successful Logins Timeline
Top Source IPs
Brute Force Correlation Table
Administrator Logons
Privilege Escalation Events
AI Login Anomaly Detection
Risk Score Table

**Troubleshooting Highlights**

**Logs Not Appearing in Last 15 Minutes**
Cause:
Timezone mismatch.
Fix:
Synchronize DC and Splunk server time.

**Forwarder Inactive**
Cause:
Port 9997 not enabled.
Fix:
Enable receiving port on Splunk server.

**System Error 1219**
Cause:
Existing SMB session.
Fix:
net use * /delete /y

**Key Skills Demonstrated**

Active Directory configuration
Log ingestion pipeline design
Splunk query development (SPL)
Detection engineering
Windows event log analysis
AI-based anomaly modeling
Risk scoring methodology
SOC dashboard creation

**MITRE ATT&CK Mapping**
Detection	Technique
Brute Force	T1110
Password Spray	T1110.003
Lateral Movement	T1021
Privilege Escalation	T1068
Account Manipulation	T1098

**Project Outcome**

This lab replicates enterprise SOC authentication monitoring using Splunk and Active Directory.
It integrates rule-based detection with statistical anomaly modeling to enhance threat visibility.

**Author**

Name: [Tushar Kumar Swami]
Role: Cybersecurity Student / SOC Enthusiast
Focus: Detection Engineering & Threat Monitoring
