# Network Overview

## 1. ISP / Home Network

The base layer provided by the internet service provider.

- Acts as the internet gateway for all physical devices
- Limited configuration options (managed by ISP)
- All physical machines connect through this network

---

## 2. Lab Network (Virtual Machines)

An isolated environment running on a local hypervisor, used for learning and experimentation.

**Machines:**

| VM | OS |
|---|---|
| Ubuntu Desktop | Ubuntu |
| Ubuntu Server | Ubuntu Server |
| Kali Linux | Kali |
| pfSense | pfSense *(planned)* |

**Adapter modes in use:**
- **Bridge** — VM is visible on the home LAN, gets its own IP
- **NAT** — VM has outbound internet access only, not reachable from LAN

**Communication:**
- VMs can see and reach each other 
- SSH works correctly between machines 
- Internet access confirmed on relevant VMs 

---

## 3. VPN Layer (Tailscale)

A Tailscale mesh network runs on top of the physical and virtual infrastructure, enabling secure remote access. Documented in detail in [`tailscale/`](../tailscale/tailscale.md).

---

## Planned Additions

- [ ] Deploy pfSense as a virtual firewall between lab segments
- [ ] Proper network segmentation (separate VLANs for attack/defense VMs)
- [ ] Network traffic flow diagram
- [ ] Document IP address scheme
