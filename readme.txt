<div align="center">

# 🐧 Accessing Your Linux System

### A beginner-to-intermediate reference guide for accessing Linux systems —
### locally, remotely, and in the cloud via AWS EC2.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Stars](https://img.shields.io/github/stars/kelvintechnical/accessing-your-linux-system?style=social)](https://github.com/kelvintechnical/accessing-your-linux-system)

> Built for learners worldwide. Star ⭐ this repo if it helped you.

</div>

---

## 📚 Table of Contents

- [Overview](#-overview)
- [Method 1 — Console Access (Direct)](#-method-1--console-access-direct)
- [Method 2 — Windows → Linux via PuTTY](#-method-2--windows--linux-via-putty)
- [Method 3 — Linux/Mac → Linux via SSH](#-method-3--linuxmac--linux-via-ssh)
- [Method 4 — AWS EC2 (Cloud Linux)](#-method-4--aws-ec2-cloud-linux)
- [Quick Reference Commands](#-quick-reference-commands)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [Author](#-author)

---

## 🗺 Overview

There are **four primary ways** to access a Linux system. Choose the method that matches your environment:

| # | Method | Best For |
|---|---|---|
| 1 | **Console (Direct)** | Physical or virtual machine, in-person access |
| 2 | **PuTTY (Windows)** | Remote access from a Windows machine |
| 3 | **SSH (Linux/Mac)** | Remote access from any Unix-based machine |
| 4 | **AWS EC2** | Cloud-hosted Linux, accessible from anywhere |

---

## 🖥 Method 1 — Console Access (Direct)

> Access the machine **directly** — no network required.

### Physical Machine

```
1. Connect a monitor and keyboard to the server
2. Power on the machine
3. Login with your username and password at the prompt
```

### Virtual Machine (VirtualBox / VMware)

```
1. Open your hypervisor (e.g., Oracle VirtualBox)
2. Select your VM → click Start
3. Login at the console prompt inside the VM window
4. Open a terminal: Activities → Terminal
```

### Enterprise Hardware (Remote Console)

| Hardware | Remote Console Tool |
|---|---|
| Dell servers | **iDRAC** |
| HP servers | **iLO** |
| VMware vSphere | **vCenter Console** |

> These tools allow console access **over the network** without SSH — useful for headless servers.

---

## 🪟 Method 2 — Windows → Linux via PuTTY

> **PuTTY** is a free, open-source SSH client for Windows.

### Step 1 — Install PuTTY

Download from → [https://www.putty.org](https://www.putty.org)

### Step 2 — Find your Linux machine's IP address

Run one of the following on your Linux machine:

```bash
ifconfig
```

```bash
# On newer systems (RHEL 7+, Ubuntu 20+):
ip addr
```

Look for the IP under the adapter named `enp0s3` (or similar):

```
inet 10.253.1.17/24
```

### Step 3 — Connect via PuTTY

```
1. Open PuTTY
2. Enter the Linux IP in the "Host Name" field
3. Port: 22
4. Connection type: SSH
5. Click Open
6. Enter your username and password when prompted
```

> ⚠️ **Note:** PuTTY gives you **terminal access only** — no GUI is available over SSH by default.

---

## 🍎 Method 3 — Linux/Mac → Linux via SSH

> If your source machine runs Linux, macOS, or any Unix-based OS — SSH is built in.

### Step 1 — Find the target machine's IP

```bash
ip addr
# Look for: inet 10.253.1.159 under enp0s3
```

### Step 2 — Connect via SSH

```bash
# Full syntax:
ssh -l your_username 10.253.1.159

# Shorthand (more common):
ssh your_username@10.253.1.159
```

### Step 3 — First-time fingerprint confirmation

```
The authenticity of host '10.253.1.159' can't be established.
Are you sure you want to continue connecting (yes/no)? yes
```

Type `yes` → enter your password → you're in.

### Step 4 — Disconnect

```bash
exit
```

You'll return to your original (local) machine.

---

## ☁️ Method 4 — AWS EC2 (Cloud Linux)

> AWS EC2 lets you run a Linux server in the cloud — accessible from **anywhere in the world**.

### Step 1 — Launch an EC2 Instance

```
1. Go to https://console.aws.amazon.com/ec2
2. Click "Launch Instance"
3. Choose an AMI:
   - Amazon Linux 2 (recommended for beginners)
   - Ubuntu 22.04 LTS (popular for dev work)
4. Instance type: t2.micro (Free Tier eligible)
5. Create a Key Pair (.pem file) — download and store it securely
6. Security Group: allow SSH (port 22) from your IP
7. Click "Launch Instance"
```

### Step 2 — Get your Public IP

```
EC2 Console → Instances → Select your instance → Copy "Public IPv4 address"
```

### Step 3a — Connect from Linux / Mac

```bash
# Set correct permissions on your key file (required)
chmod 400 your-key.pem

# Connect:
ssh -i "your-key.pem" ec2-user@YOUR_PUBLIC_IP

# Ubuntu AMIs use a different default user:
ssh -i "your-key.pem" ubuntu@YOUR_PUBLIC_IP
```

### Step 3b — Connect from Windows via PuTTY

```
1. Open PuTTYgen → Load your .pem file → Save as .ppk
2. Open PuTTY → enter your EC2 Public IP
3. Go to: Connection → SSH → Auth → Browse to your .ppk file
4. Click Open
5. Login as: ec2-user (Amazon Linux) or ubuntu (Ubuntu)
```

---

### ⚠️ Important EC2 Notes

| Topic | What to Know |
|---|---|
| **Stopped vs Terminated** | Stopped = data preserved. Terminated = instance deleted. |
| **Public IP** | Changes on every stop/start unless you assign an **Elastic IP** |
| **Billing** | Always stop your instance when not in use |
| **Auto-Stop** | Use **AWS EventBridge Scheduler** to auto-stop at a set time (e.g., 6PM daily) |
| **Key file security** | Never commit your `.pem` file to GitHub — add it to `.gitignore` |

---

## ⚡ Quick Reference Commands

| Command | Purpose |
|---|---|
| `ifconfig` | Show network interfaces (older systems) |
| `ip addr` | Show IP address (RHEL 7+ / Ubuntu 20+) |
| `hostname -I` | Quick local IP lookup |
| `whoami` | Show current logged-in user |
| `ssh -l user IP` | Connect to remote Linux machine |
| `ssh user@IP` | Connect (shorthand) |
| `ssh -i key.pem user@IP` | Connect to EC2 with key pair |
| `chmod 400 key.pem` | Set correct permissions on EC2 key |
| `exit` | Disconnect from SSH session |
| `sudo systemctl status sshd` | Check if SSH service is running |

---

## 🔧 Troubleshooting

### `ifconfig` not found

```bash
# Use the modern replacement:
ip addr

# Or install net-tools:
sudo apt install net-tools    # Ubuntu/Debian
sudo yum install net-tools    # RHEL/CentOS
```

---

### SSH: Connection refused

```
Possible causes:
✗ Port 22 is not open in your firewall or EC2 Security Group
✗ Wrong IP address
✗ SSH service not running on the target machine
```

```bash
# Check if SSH is running on the target:
sudo systemctl status sshd

# Start it if needed:
sudo systemctl start sshd
```

---

### EC2: Permission denied (publickey)

```
Possible causes:
✗ Wrong .pem key file
✗ Wrong username (ec2-user vs ubuntu vs root)
✗ .pem file permissions not set correctly
```

```bash
# Fix permissions:
chmod 400 your-key.pem

# Verify correct username:
# Amazon Linux → ec2-user
# Ubuntu        → ubuntu
# RHEL          → ec2-user
# Debian        → admin
```

---

### PuTTY: Network error — Connection timed out

```
Possible causes:
✗ EC2 Security Group does not allow port 22 from your IP
✗ Instance is not in "Running" state
✗ Wrong Public IP (IP changes after stop/start)
```

---

## 🤝 Contributing

Pull requests are welcome! This is a **free, open learning resource** — help make it better for everyone.

```
1. Fork this repo
2. Create a branch: git checkout -b feature/your-topic
3. Make your changes
4. Submit a Pull Request with a clear description
```

### What we welcome

- ✅ New access methods (WSL2, Tailscale VPN, VNC, etc.)
- ✅ OS-specific guides (Fedora, Arch, Debian)
- ✅ Fixes to commands or outdated steps
- ✅ Translations to other languages

See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

---

## 👤 Author

**Kelvin R. Tobias**
Software Engineer | AI Engineering Student | Linux & Cloud Practitioner

- 🌐 [kelvinintech.com](https://kelvinintech.com)
- 💻 [github.com/kelvintechnical](https://github.com/kelvintechnical)
- 💼 [Tech Affiliates Community](https://www.linkedin.com/in/kelvintobias)

---

<div align="center">

**If this helped you, please ⭐ star the repo and share it with someone learning Linux.**

*Part of the Linux Ops learning series*

</div>