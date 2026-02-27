# IP Addressing Scheme

## 1. Subnet Design

Network Range:
192.168.50.0/24

Subnet Mask:
255.255.255.0

Gateway:
Not required (isolated lab)

---

## 2. Static IP Assignments

| System | IP Address |
|--------|------------|
| DC01 | 192.168.50.10 |
| WIN10 | 192.168.50.20 |
| SPLUNK-SERVER | 192.168.50.40 |
| KALI (optional) | 192.168.50.30 |

---

## 3. DNS Configuration

For domain-joined machines:

Preferred DNS:
192.168.50.10 (Domain Controller)

Without correct DNS:
- Domain join will fail
- Authentication resolution will fail

---

## 4. Verification Commands

On Windows:

ipconfig

On Ubuntu:

ip a