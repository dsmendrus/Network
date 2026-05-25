# Tailscale – Notes

## What is Tailscale?

Tailscale is a VPN tool that lets you connect devices into one private network, even if they are located in different places.

---

## What do I use it for?

- access to machines outside my home
- no need to open ports on the router
- secure connection between devices
- access to my lab from anywhere

---

## How it works

- each device gets an IP address in the `100.x.x.x` range
- devices communicate directly (`peer-to-peer`)
- the connection is encrypted
- it works independently from my home network

---

## My setup

### Connected devices

- Laptop
- PC
- Virtual machines:
  - Ubuntu Server
  - Kali Linux

---

## What I already did

- Installed Tailscale on:
  - PC
  - laptop
  - Linux
- Logged all devices into one network
- Tested the connection (`ping` / `ssh`)
- Renamed my machines on the Tailscale admin page to make them easier to identify

---

## Problems / Errors

### 1. Devices sometimes did not connect on the first try

Sometimes Tailscale would not connect the first time, so I had to log in twice.

On the Tailscale admin page, the devices also showed as inactive even though I had already:

- installed Tailscale
- enabled it
- connected the device

#### Solution

```text
Turn it off and on again xd
```

---

### 2. SSH passwordless login did not work on Windows

Windows would not allow SSH login without using a password and kept throwing an error.

#### Solution

The SSH config path is:

```text
C:\ProgramData\ssh\sshd_config
```

not:

```text
C:\Users\User\.ssh\auth_keys
```

---

## Commands (to remember)

### Tailscale

```bash
tailscale up
tailscale status
tailscale ip
```

### CMD / PowerShell

```powershell
ssh-keygen

ren "$env:USERPROFILE\.ssh\config.txt" "config"

# ren = Rename-Item

scp $env:USERPROFILE\.ssh\id_ed25519.pub kali@kali.et:/tmp/

Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

### Linux / Bash

```bash
cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
```

---

# Connecting devices

At first, I was connecting through SSH using the static IP address provided by Tailscale.

Later, I found out that thanks to **MagicDNS** in Tailscale, I can connect using a hostname instead of an IP address.

Now I also created a file in:

```text
~/.ssh/config
```

Example:

```ssh
Host kali
    HostName kali.tail66fc55.ts.net
    User kali
```

Now I only need one command:

```bash
ssh kali
```

and I can log in immediately.

---

## SSH key authentication

To avoid entering passwords every time, I configured SSH keys.

### Step 1 — Generate SSH keys on Windows

```powershell
ssh-keygen
```

This creates:

- a private key
- a public key

---

### Step 2 — Send the public key to Linux

```powershell
scp $env:USERPROFILE\.ssh\id_ed25519.pub kali@kali.nt:/tmp/
```

---

### Step 3 — Add the key to `authorized_keys`

```bash
cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
```

---

### Result

Now I can log in using only:

```bash
ssh kali
```

without entering a password.

---

## Useful tip

Rename:

```text
config.txt
```

to:

```text
config
```

using:

```powershell
ren "$env:USERPROFILE\.ssh\config.txt" "config"
```