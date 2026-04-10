# LabEx — Cybersecurity Labs

> Documenting my hands-on learning through LabEx as part of my career transition from **Project Management** to **IR/DFIR Analyst**.

![Platform](https://img.shields.io/badge/Platform-LabEx-4A90D9?style=flat-square)
![Focus](https://img.shields.io/badge/Focus-Linux%20%7C%20Security%2B%20Prep-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat-square)

---

## What is LabEx?

LabEx is an AI-powered, hands-on learning platform that runs entirely in the browser — no VPN, no local setup required. It uses a strict "learn by doing" approach with interactive labs, automated step-by-step checks, and an AI assistant (Labby) for when you get stuck.

It covers:

- **Linux fundamentals** — terminal operations, file system, package management, SSH
- **Network security** — traffic analysis with tcpdump and Wireshark, Snort IDS, packet inspection
- **Cybersecurity basics** — ethical hacking intro, encryption, password security, web vulnerabilities
- **Security+ prep** — threat analysis, risk management, cryptography, identity management, incident response

LabEx is used as a **supplement** to TryHackMe and HTB Academy — not a replacement. It shines for Linux command practice and Security+ lab reinforcement.

---

## My Background & Goal

| | |
|---|---|
| **Coming from** | Project Management |
| **Target role** | IR/DFIR Analyst |
| **Why LabEx** | Linux practice + Security+ hands-on lab reinforcement |
| **Next step** | TryHackMe SOC Level 1 → Security+ → HTB CDSA → INE eCIR |

---

## Lab Progress

```
Linux Fundamentals:    ███████░░░░░░░░░░░░░░░░░  3 / 10 labs completed  ← currently active
Security+ Prep:        ░░░░░░░░░░░░░░░░░░░░░░░░  0 labs completed
Network Security:      ░░░░░░░░░░░░░░░░░░░░░░░░  0 labs completed
Cybersecurity Basics:  ░░░░░░░░░░░░░░░░░░░░░░░░  0 labs completed
```

### Currently Working On 🔥

**Quick Start with Linux** — Linux Skill Tree  
A beginner's guide for Linux aimed at those new to the system and looking to begin promptly. By completing ten labs, you will grasp the basics of Linux, enabling you to perform fundamental tasks with ease.

`███████░░░░░░░░░░░░░░░░░` 3 / 10 labs completed

### Completed ✅

| # | Lab | Course | Category | Key Skills | Notes |
|---|-----|--------|----------|------------|-------|
| 1 | Your First Linux Lab | Quick Start with Linux | 🐧 Linux | Linux environment setup, terminal basics, navigating the shell | [Notes](labs/linux/your-first-linux-lab.md) |
| 2 | Display User and Group Information | Quick Start with Linux | 🐧 Linux | `whoami`, `id`, `groups`, user/group management concepts | [Notes](labs/linux/display-user-and-group-information.md) |
| 3 | Basic File Operations | Quick Start with Linux | 🐧 Linux | `ls`, `cp`, `mv`, `rm`, `touch`, file and directory management | [Notes](labs/linux/basic-file-operations.md) |

### Up Next 🔜

#### Linux Fundamentals — Quick Start with Linux (in progress)
| Lab | Category |
|-----|----------|
| Environment Variables and Path | 🐧 Linux |
| File Permissions and Ownership | 🐧 Linux |
| User and Group Management | 🐧 Linux |
| Process Management Basics | 🐧 Linux |
| Text Processing with grep and sed | 🐧 Linux |
| Shell Scripting Basics | 🐧 Linux |
| Package Management with APT | 🐧 Linux |

#### Linux Fundamentals — Next Courses
| Lab | Category |
|-----|----------|
| Linux Terminal Operations | 🐧 Linux |
| Remote Access & Secure File Transfer (SSH/SCP) | 🐧 Linux |

#### Security+ Prep
| Lab | Category |
|-----|----------|
| Threat Analysis & Risk Management | 🔐 Security+ |
| Network Security Fundamentals | 🔐 Security+ |
| Identity & Access Management | 🔐 Security+ |
| Cryptography Basics | 🔐 Security+ |
| Incident Response Fundamentals | 🔐 Security+ |

#### Network Security
| Lab | Category |
|-----|----------|
| Network Traffic Analysis with tcpdump | 🌐 Networking |
| Protocol Analysis with tshark | 🌐 Networking |
| Intrusion Detection with Snort | 🌐 Networking |
| Packet Capture with Wireshark | 🌐 Networking |

#### Cybersecurity Basics
| Lab | Category |
|-----|----------|
| Password Cracking Basics with Hydra | ⚔️ Ethical Hacking |
| Network Scanning with Nmap | ⚔️ Ethical Hacking |
| Basic Encryption with OpenSSL | 🔒 Cryptography |
| Web Vulnerabilities Intro | 🌐 Web Security |

---

## Skills Being Built

### 🐧 Linux
- Terminal navigation and file system operations
- User management and permissions
- Package installation and system updates
- SSH, SCP, and secure remote access

### 🔐 Security+ Prep
- Threat analysis and risk management frameworks
- Network security principles and controls
- Identity and access management (IAM)
- Cryptography — symmetric, asymmetric, hashing
- Incident response fundamentals

### 🌐 Network Security
- Live packet capture and traffic analysis with `tcpdump`
- Protocol-level inspection with `tshark`
- Writing and deploying Snort IDS rules
- PCAP file analysis for forensic investigations

### ⚔️ Ethical Hacking Basics
- Password cracking techniques with `Hydra`
- Network scanning and service discovery with `Nmap`
- Web vulnerability introduction — SQLi, XSS, command injection

---

## Tools Introduced

| Tool | Command | Purpose |
|------|---------|---------|
| `tcpdump` | `tcpdump -i eth0 -w capture.pcap` | Live packet capture for traffic analysis |
| `tshark` | `tshark -r capture.pcap` | Command-line protocol analysis |
| `nmap` | `nmap -sV <target>` | Network scanning and service detection |
| `hydra` | `hydra -l user -P wordlist <target>` | Password cracking and brute-force testing |
| `openssl` | `openssl enc -aes-256-cbc -in file` | Encryption and certificate operations |
| `snort` | `snort -A console -c snort.conf` | Intrusion detection and rule-based alerting |

```

Each lab writeup includes:
- Summary of what the lab covers
- Key concepts learned with explanations
- Step-by-step notes and commands used
- Tools table with actual syntax
- Takeaways connecting the lab to IR/DFIR work

---

## Connect

- 🔗 LinkedIn: [linkedin.com/in/esteban-gonzalez-3400ab233](https://www.linkedin.com/in/esteban-gonzalez-3400ab233)
- 💀 TryHackMe: [tryhackme.com/p/egonzalez.1679](https://tryhackme.com/p/egonzalez.1679)
