# SSH Brute Force Attack — Detection & Log Analysis

**Date:** 2026-05-21  
**Attacker (Kali Linux):** `192.168.18.127`  
**Target (Ubuntu Server):** `192.168.18.32`  
**Tool used:** Nmap (reconnaissance) + Hydra (brute force)  
**Log source:** `/var/log/auth.log` on Ubuntu Server

---

## Objective

Simulate a real SSH brute force attack from Kali Linux against an Ubuntu Server VM, observe and analyze the generated logs, and understand what indicators of compromise (IoCs) are visible from the defender's perspective.

---

## Phase 1 — Reconnaissance (Nmap)

Before launching the attack, the target was scanned to confirm SSH is running.

### Mistake Made — Wrong IP Address

The first nmap scan was run against `182.168.18.32` (typo) instead of `192.168.18.32`:

```bash
nmap -sV -p 22 182.168.18.32
```

```
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
```

The result showed `filtered` — meaning no response was received. This was **not** the Ubuntu Server. The scan hit a completely different host on the internet that drops packets silently (firewall DROP behavior — no RST returned).

> **Lesson:** Always double-check the target IP before scanning. A `filtered` result doesn't mean the port is closed — it can mean wrong target, firewall, or network filtering. Compare with `closed` (RST received) vs `filtered` (no response).

### Correct Scan

```bash
nmap -sV -p 22 192.168.18.32
```

Expected result on the actual Ubuntu Server:
```
PORT   STATE  SERVICE VERSION
22/tcp open   ssh     OpenSSH ...
```

---

## Phase 2 — Brute Force Attack (Hydra)

### First Attempt — Wrong Username

The first Hydra run used an incorrect username. No successful authentication, but logs on the server still recorded the attempts.

### Second Attempt — Correct Username (`admin`)

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.18.32 -t 4 -V
```

**Flags explained:**
- `-l admin` — single username to target
- `-P rockyou.txt` — password wordlist
- `-t 4` — 4 parallel threads
- `-V` — verbose output (show each attempt)

---

## Phase 3 — Log Analysis (`/var/log/auth.log`)

### What Was Observed

Logs were monitored in real time using:
```bash
sudo tail -f /var/log/auth.log
```

### Stage 1 — Attack Begins (Image 1)

```
2026-05-21T13:34:04 sshd[20777]: PAM 3 more authentication failures; rhost=192.168.18.127
2026-05-21T13:34:04 sshd[20777]: PAM service(sshd) ignoring max retries; 4 > 3
2026-05-21T13:34:35 sshd[20787]: pam_unix(sshd:auth): authentication failure; rhost=192.168.18.127 user=admin
2026-05-21T13:34:37 sshd[20785]: Failed password for admin from 192.168.18.127 port 40736 ssh2
2026-05-21T13:34:37 sshd[20786]: Failed password for admin from 192.168.18.127 port 40752 ssh2
2026-05-21T13:34:37 sshd[20787]: Failed password for admin from 192.168.18.127 port 40758 ssh2
2026-05-21T13:34:37 sshd[20788]: Failed password for admin from 192.168.18.127 port 40774 ssh2
```

**Key observations:**

| Indicator | Meaning |
|---|---|
| 4 different PIDs at the same time (20785–20788) | Hydra opened 4 parallel connections (`-t 4`) |
| Rapidly changing source ports (40736, 40752, 40758, 40774) | Each new connection uses a different ephemeral port — classic brute force signature |
| `PAM ignoring max retries; 4 > 3` | PAM tried to enforce a 3-attempt limit, but Hydra bypassed it by opening new sessions |
| `pam_unix: authentication failure` | Low-level PAM module logging each failed credential check |
| `Failed password for admin` repeated every ~2 seconds | Automated tool — no human types this fast |

### Stage 2 — Server Starts Recognizing the Pattern (Image 2)

```
2026-05-21T13:35:43 sshd[20813]: error: maximum authentication attempts exceeded for admin from 192.168.18.127 port 37748 ssh2 [preauth]
2026-05-21T13:35:43 sshd[20813]: Disconnecting authenticating user admin 192.168.18.127 port 37748: Too many authentication failures [preauth]
2026-05-21T13:35:43 sshd[20813]: PAM 5 more authentication failures; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.18.127 user=admin
2026-05-21T13:35:43 sshd[20813]: PAM service(sshd) ignoring max retries; 6 > 3
```

**Key observations:**

| Indicator | Meaning |
|---|---|
| `maximum authentication attempts exceeded` | SSH server hit its `MaxAuthTries` limit per connection |
| `Disconnecting ... Too many authentication failures [preauth]` | Server forcibly closed the connection before authentication completed |
| `PAM 5 more authentication failures` | Failure count escalating — PAM tracking cumulative failures |
| `ignoring max retries; 6 > 3` | Hydra continued opening new connections, bypassing per-connection limits |
| New PIDs spawning (20822, 20824, 20826, 20828) | Hydra immediately opened fresh connections after being disconnected |

### Timeline Summary

| Time | Event |
|---|---|
| 13:34:04 | Attack begins — first PAM failures recorded |
| 13:34:35 | `pam_unix` auth failures — PAM tracking individual attempts |
| 13:34:37 | Burst of `Failed password` entries — 4 threads running simultaneously |
| 13:35:39 | `maximum authentication attempts exceeded` — server starts disconnecting |
| 13:35:43 | `Too many authentication failures [preauth]` — server recognizing repeated abuse |
| 13:35:45+ | Hydra spawns new connections, attack continues with new PIDs |

**Total attack window visible in logs: ~1.5 minutes**

---

## Indicators of Compromise (IoCs)

If you were a SOC analyst reviewing these logs, the following patterns would trigger an alert:

1. **Hundreds of `Failed password` entries from a single IP** in a short timeframe
2. **Multiple simultaneous SSH sessions from the same source IP** (multiple PIDs)
3. **Rapidly changing source ports** — automated tooling signature
4. **`maximum authentication attempts exceeded`** — server-side rate limiting triggered
5. **`PAM service ignoring max retries`** — attacker bypassing per-connection limits with new connections
6. **All attempts targeting the same username** (`admin`) — credential stuffing / dictionary attack pattern

---

## How to Defend Against This Attack

### ✅ fail2ban — Automatic IP Banning

Install and configure fail2ban to automatically ban IPs that exceed a threshold:

```bash
sudo apt install fail2ban
```

Default behavior: ban an IP after 5 failed SSH attempts within 10 minutes.

After enabling fail2ban, the attack above would have been stopped after the first few attempts — the attacker's IP (`192.168.18.127`) would be blocked at the firewall level.

### ✅ Disable Password Authentication — Use SSH Keys Only

```bash
# On Ubuntu Server — edit /etc/ssh/sshd_config
PasswordAuthentication no
PubkeyAuthentication yes
```

Even if the attacker has the correct password, they cannot log in without the private key.

### ✅ Change Default SSH Port

```bash
# /etc/ssh/sshd_config
Port 2222
```

Reduces automated scanning noise (most bots scan port 22 only).

### ✅ Restrict SSH Access by IP (firewall rule)

```bash
sudo ufw allow from 192.168.18.0/24 to any port 22
sudo ufw deny 22
```

Only allow SSH from trusted network segments.

---

## Tools Used

| Tool | Purpose |
|---|---|
| Nmap | Port discovery and service version detection |
| Hydra | Automated SSH brute force |
| `tail -f /var/log/auth.log` | Real-time log monitoring on target |

---

## Conclusion

This exercise demonstrated how a brute force attack looks from both sides. The `auth.log` provides a clear and detailed record of every failed attempt, the attacker's IP, source ports, and the username being targeted. Even without dedicated SIEM tooling, a SOC analyst reviewing these logs manually would immediately recognize the attack pattern.

The most impactful defense in this scenario would be **fail2ban** combined with **key-based authentication only** — together they make SSH brute force attacks practically useless.