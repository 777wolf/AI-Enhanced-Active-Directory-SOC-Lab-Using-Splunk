# Troubleshooting Guide

## 1. Logs Not Appearing in Last 15 Minutes

Cause:
Timezone mismatch.

Fix:
Synchronize DC and Splunk server timezone.

---

## 2. Forwarder Inactive

Cause:
Port 9997 not enabled.

Fix:
Enable receiving port on Splunk server.

---

## 3. System Error 1219

Cause:
Existing SMB session.

Fix:
net use * /delete /y

---

## 4. 4625 Not Generated

Cause:
Audit policy disabled.

Fix:
Enable via auditpol.

---

## 5. Only One Host Appearing

Cause:
Forwarder misconfiguration.

Fix:
Verify outputs.conf server IP.