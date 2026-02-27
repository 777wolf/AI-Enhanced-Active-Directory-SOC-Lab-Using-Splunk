# 06 - AI-Based Anomaly Detection

## 1. Model Overview

Unsupervised anomaly detection using:

Threshold = Average + (2 × Standard Deviation)

---

## 2. SPL Query

index=main host=DC01 EventCode=4624 earliest=-24h
| bin _time span=1h
| stats count as login_count by _time Account_Name
| eventstats avg(login_count) as avg_login stdev(login_count) as stdev_login by Account_Name
| eval threshold = avg_login + (2*stdev_login)
| where login_count > threshold

---

## 3. Risk Scoring Model

index=main host=DC01 (EventCode=4625 OR EventCode=4624)
| stats count(eval(EventCode=4625)) as failed,
        count(eval(EventCode=4624)) as success
        by Source_Network_Address
| eval risk_score = (failed*2) + success

---

## 4. Limitations

- Requires sufficient historical data
- Static threshold
- Not adaptive ML