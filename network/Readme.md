# 🖧 Network Architecture

This section documents the structure and configuration of the lab network — physical layout, virtual machine setup, and how traffic flows between segments.

For VPN connectivity, see [`tailscale/`](../tailscale/tailscale.md).
For security monitoring and attack simulation, see [`SOC/`](../SOC/Readme.md).

---

## Overview

The lab consists of three distinct network layers:

| Layer | Description |
|---|---|
| **ISP / Home network** | Physical devices, internet gateway |
| **Lab network** | Virtual machines for learning and testing |
| **VPN overlay** | Tailscale mesh — documented separately |

Detailed breakdown in [`network.md`](./network.md).

---

## Virtual Machines

| VM | OS | Role |
|---|---|---|
| `ubuntu-desktop` | Ubuntu | General-purpose workstation |
| `ubuntu-server` | Ubuntu Server | Server administration practice |
| `kali` | Kali Linux | Attack simulation *(SOC exercises)* |
| `pfsense` | pfSense | Firewall & network segmentation *(planned)* |

---

## VM Network Adapters

Each VM can be assigned different adapter modes depending on the use case:

**Bridge Adapter**
- VM gets its own IP on the physical LAN
- Directly reachable from other devices on the home network
- Used when the VM needs to act as a real network participant

**NAT**
- VM shares the host's IP; outbound internet access only
- Isolated from the LAN — other devices cannot reach it directly
- Used for isolated testing or when external exposure isn't needed

**Host-only / Internal Network** *(planned with pfSense)*
- Traffic stays within the hypervisor — no external access
- Useful for isolated attack/defense scenarios between VMs

---

## Current Status

- [x] VMs can communicate with each other over the local network
- [x] Routing between lab segments working
- [x] SSH connectivity confirmed between machines
- [ ] pfSense deployment — planned
- [ ] Network diagram — in progress
