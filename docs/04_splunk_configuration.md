# 04 - Splunk Configuration

## 1. Install Splunk Enterprise (Ubuntu)

Install .deb package and start Splunk:

/opt/splunk/bin/splunk start --accept-license

Web Access:
http://192.168.50.40:8000

---

## 2. Enable Receiving Port

Settings → Forwarding and Receiving  
Add port: 9997

---

## 3. Install Universal Forwarder (DC01)

Configure outputs.conf:

[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = 192.168.50.40:9997

---

Configure inputs.conf:

[WinEventLog://Security]
disabled = 0
index = main

---

## 4. Validate Log Ingestion

Search:
index=main

Verify host visibility:
| stats count by host