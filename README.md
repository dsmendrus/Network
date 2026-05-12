#  Home Network Security Lab

A personal project documenting the setup, configuration, and security analysis of my home lab environment. The goal is to build a practical foundation in network administration, VPN management, and SOC-style traffic monitoring — skills directly applicable to junior roles in cybersecurity and network operations.

---

##  Repository Structure

```
Network/
├── network/        # Network topology, routing, and VM adapter configuration
├── tailscale/      # VPN setup, device connectivity, and SSH over Tailscale
└── SOC/            # Traffic analysis, attack simulation, and alerting (in progress)
```

---

##  Network Setup

**Folder:** [`network/`](./network/Readme.md)

Documentation of the core lab network, including:

- **Topology** — diagram and description of all devices and their roles
- **Routing** — inter-network routing configuration between lab segments
- **VM Adapters** — virtual machine network modes: Bridge (direct LAN access) vs NAT (isolated via host), with use cases for each

**Key technologies:** Oracle VirutalBox, static routing, Bridge & NAT networking

---

## Tailscale VPN

**Folder:** [`tailscale/`](./tailscale/tailscale.md)

Setup and configuration of a Tailscale mesh VPN across lab devices:

- Installing and registering Tailscale nodes
- Establishing secure connections between machines across different networks
- Configuring **SSH access over the Tailscale tunnel** — no exposed ports, no password auth

**Why Tailscale?** It removes the need to expose SSH or other services to the public internet — a practical example of zero-trust network access (ZTNA) principles.

---

##  SOC & Threat Simulation *(in progress)*

**Folder:** [`SOC/`](./SOC/Readme.md)

Planned work to simulate a basic Security Operations Center workflow within the lab:

- [ ] **Traffic analysis** — capturing and inspecting packets with Wireshark / tcpdump
- [ ] **Attack simulation** — using a Kali Linux VM as an adversary to generate realistic attack traffic (port scans, brute force attempts)
- [ ] **Firewall & filtering** — pfSense deployment for network segmentation and rule-based traffic control
- [ ] **Log collection & alerting** — aggregating logs and setting up basic alerts for suspicious activity

---

##  Goals & Motivation

This lab is my hands-on learning environment for topics covered in my **Bachelor's degree in Information Systems Security** and the **Google Cybersecurity Certificate** I'm currently completing. Rather than relying solely on theory, I use this setup to:

- Test concepts in a real (not simulated) environment
- Break things intentionally and learn how to fix them
- Document findings as I would in a professional setting

---

## Tools & Technologies

| Category          | Tools |
|-------------------|--------------------------------|
| Virtualization    | VirtualBox / VMware            |
| VPN               | Tailscale                      |
| Firewall          | pfSense *(planned)*            |
| Traffic Analysis  | Wireshark, tcpdump *(planned)* |
| Attack Simulation | Kali Linux                     |
| Remote Access     | SSH over Tailscale             |
| OS                | Linux (Ubuntu Serwer), Windows |

---

##  Status

This is an actively evolving project. New findings, configurations, and SOC exercises will be added regularly.

> *If you have suggestions or spot something worth improving, feel free to open an issue.*
