# 02 - Environment Setup

## 1. Hardware Requirements

- Minimum 16GB RAM
- 8+ CPU threads recommended
- 200GB disk space

---

## 2. Virtual Machines

| VM | OS | RAM | Role |
|----|----|-----|------|
| DC01 | Windows Server 2019 | 4GB | Domain Controller |
| WIN10 | Windows 10 | 4GB | Client |
| SPLUNK-SERVER | Ubuntu | 4GB | SIEM |

---

## 3. Network Configuration

All VMs configured in:

VirtualBox → Adapter 1 → Internal Network  
Network Name: SOC-LAB

---

## 4. Static IP Configuration

| VM | IP Address |
|----|------------|
| DC01 | 192.168.50.10 |
| WIN10 | 192.168.50.20 |
| SPLUNK | 192.168.50.40 |

Subnet Mask: 255.255.255.0  
DNS (Domain Joined Machines): 192.168.50.10

---

## 5. Snapshot Strategy

Snapshots were taken after:

- OS installation
- AD installation
- Splunk installation
- Forwarder configuration
- Detection rule implementation