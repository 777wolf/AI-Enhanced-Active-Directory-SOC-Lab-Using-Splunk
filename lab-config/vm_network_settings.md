# VM Network Configuration

## 1. Network Mode Selection

All virtual machines were configured using:

VirtualBox → Adapter 1 → Internal Network

Network Name:
SOC-LAB

This ensures:
- Full isolation from external network
- Safe attack simulation
- Controlled authentication traffic
- No exposure to production systems

---

## 2. Why Internal Network Was Chosen

| Mode | Reason Not Used |
|------|------------------|
| NAT | Does not allow easy inter-VM authentication testing |
| Bridged | Exposes lab to real LAN |
| Host-only | Limited isolation |
| Internal Network | Fully isolated, ideal for SOC lab |

---

## 3. VM Network Mapping

| VM | Interface | Mode | Network Name |
|----|-----------|------|--------------|
| DC01 | Adapter 1 | Internal Network | SOC-LAB |
| WIN10 | Adapter 1 | Internal Network | SOC-LAB |
| SPLUNK-SERVER | Adapter 1 | Internal Network | SOC-LAB |

All VMs must use the exact same Internal Network name.

---

## 4. Connectivity Validation

After configuration:

On each VM:

ping 192.168.50.10
ping 192.168.50.20
ping 192.168.50.40

If ping fails:

- Check firewall
- Verify same internal network name
- Verify static IP configuration

---

## 5. Firewall Adjustment (DC01)

To allow ICMP (Ping):

netsh advfirewall firewall add rule name="Allow ICMPv4" protocol=icmpv4 dir=in action=allow