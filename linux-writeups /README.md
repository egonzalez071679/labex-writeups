# LabEx — Quick Start with Linux

> **Platform:** LabEx  
> **Skill Tree:** Linux  
> **Course:** Quick Start with Linux  
> **Difficulty:** Beginner  
> **Progress:** 3 / 10 labs completed  
> **Course Link:** https://labex.io/learn/cybersecurity-engineer

---

## About This Course

A beginner's guide for Linux aimed at those new to the system and looking to begin promptly. By completing ten labs, you will grasp the basics of Linux, enabling you to perform fundamental tasks with ease.

This course is used alongside the TryHackMe Pre-Security path to build Linux terminal muscle memory — a non-negotiable skill for IR/DFIR work where most investigation tools, SIEM agents, and forensic utilities run on Linux.

---

## Lab 1 — Your First Linux Lab

### Summary

Introduction to the LabEx Linux environment. Covers how to access the browser-based terminal, orient yourself in an unfamiliar Linux system, and run your first basic commands from a cold start.

---

### Key Concepts Learned

- **Linux terminal** — A text-based interface for interacting directly with the operating system through commands
- **Shell** — The program that interprets and executes commands; `bash` is the default on most Linux systems
- **Command structure** — `command [options] [arguments]` is the universal pattern for Linux commands
- **Working directory** — Your current location in the filesystem; always know where you are before running commands
- **`pwd`** — Prints the full path of your current working directory
- **`ls`** — Lists the contents of a directory

---

### Notes & Walkthrough

#### Task 1 — Accessing the Lab Environment

LabEx runs a full Linux environment in the browser. No VPN or local install needed — the terminal is live and interactive from the start.

```bash
# First commands to run in any new Linux environment
pwd           # Where am I?
whoami        # Who am I logged in as?
ls            # What's in this directory?
ls -la        # Full listing including hidden files and permissions
```

#### Task 2 — Navigating the Filesystem

```bash
cd /          # Go to root of the filesystem
cd ~          # Go to home directory
cd ..         # Go up one directory level
cd /etc       # Navigate to a specific path

# Key directories to know
/home         # User home directories
/etc          # System configuration files
/var          # Variable data (logs live here: /var/log)
/tmp          # Temporary files — cleared on reboot
/bin          # Essential command binaries
```

---

### Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `pwd` | `pwd` | Print current working directory |
| `ls` | `ls -la` | List directory contents with details and hidden files |
| `cd` | `cd <path>` | Change directory |
| `whoami` | `whoami` | Display current logged-in user |
| `clear` | `clear` | Clear the terminal screen |

---

### Takeaways

- Every IR/DFIR investigation on a Linux system starts by knowing where you are (`pwd`) and what's around you (`ls`)
- `/var/log` is one of the most important directories for defenders — system and application logs live there
- Getting comfortable in an unfamiliar Linux environment quickly is a core field skill for incident responders
- The browser-based lab removes all setup friction — no excuse not to practice daily

---

### References

- [LabEx Linux Skill Tree](https://labex.io/learn/cybersecurity-engineer)
- [Linux Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)

---

## Lab 2 — Display User and Group Information

### Summary

Covers how Linux manages identity through users and groups. Introduces the commands used to query who you are, what groups you belong to, and how that determines what you can access — foundational knowledge for both system administration and privilege escalation detection.

---

### Key Concepts Learned

- **User** — A named identity on the system with a unique User ID (UID); `root` has UID 0
- **Group** — A collection of users that share permissions; every user has a primary group and can belong to multiple supplementary groups
- **UID / GID** — Numeric identifiers the kernel uses internally; names are just human-readable labels
- **`whoami`** — Returns the username of the current user
- **`id`** — Shows UID, GID, and all group memberships for a user
- **`groups`** — Lists all groups the current (or specified) user belongs to

---

### Notes & Walkthrough

#### Task 1 — Checking Current User Identity

```bash
whoami                  # Print current username
id                      # Show UID, GID and all groups
id <username>           # Show identity info for a specific user

# Example output of id:
# uid=1000(esteban) gid=1000(esteban) groups=1000(esteban),4(adm),27(sudo)
```

**Key insight:** If `id` shows you are in the `sudo` group, you have administrative privileges. During incident investigations, checking group memberships of suspicious accounts reveals their access level.

#### Task 2 — Listing Group Memberships

```bash
groups                  # List groups of current user
groups <username>       # List groups of a specific user
cat /etc/group          # View all groups defined on the system
cat /etc/passwd         # View all user accounts on the system
```

#### Task 3 — Understanding /etc/passwd and /etc/group

```bash
# /etc/passwd format:
# username:x:UID:GID:comment:home_dir:shell
cat /etc/passwd | grep <username>

# /etc/group format:
# groupname:x:GID:members
cat /etc/group | grep sudo      # Who has sudo access?
```

---

### Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `whoami` | `whoami` | Display current logged-in username |
| `id` | `id` / `id <user>` | Show UID, GID and all group memberships |
| `groups` | `groups <user>` | List group memberships for a user |
| `cat` | `cat /etc/passwd` | Read user account definitions |
| `cat` | `cat /etc/group` | Read group definitions |

---

### Takeaways

- During incident response, checking `id` and `groups` on a suspicious account immediately reveals its privilege level
- If an attacker account is in the `sudo` or `adm` group, the severity of the incident escalates significantly
- `/etc/passwd` and `/etc/group` are key artifacts in Linux forensics — changes to these files can indicate persistence techniques
- UID 0 = root regardless of username — attackers sometimes rename accounts to hide elevated privileges

---

### References

- [LabEx Linux Skill Tree](https://labex.io/learn/cybersecurity-engineer)
- [Linux User Management — man pages](https://man7.org/linux/man-pages/man5/passwd.5.html)
- [MITRE ATT&CK — Account Discovery (T1087)](https://attack.mitre.org/techniques/T1087/)

---

## Lab 3 — Basic File Operations

### Summary

Covers the essential commands for working with files and directories in Linux. Creating, copying, moving, renaming, and deleting — the everyday operations that underpin everything from system administration to forensic evidence handling.

---

### Key Concepts Learned

- **`touch`** — Creates an empty file or updates the timestamp of an existing one
- **`mkdir`** — Creates a new directory; `-p` creates nested directories in one command
- **`cp`** — Copies files or directories; `-r` flag required for directories
- **`mv`** — Moves or renames files and directories
- **`rm`** — Deletes files; `-r` for directories, `-f` to force without prompting
- **File paths** — Absolute paths start from `/`; relative paths start from your current location

---

### Notes & Walkthrough

#### Task 1 — Creating Files and Directories

```bash
touch notes.txt                  # Create an empty file
touch file1.txt file2.txt        # Create multiple files at once
mkdir logs                       # Create a directory
mkdir -p investigations/case-01  # Create nested directories in one step
```

#### Task 2 — Copying Files

```bash
cp notes.txt backup.txt          # Copy a file
cp -r logs/ logs-backup/         # Copy a directory and its contents
cp notes.txt /tmp/               # Copy to a different location
```

#### Task 3 — Moving and Renaming

```bash
mv notes.txt report.txt          # Rename a file
mv report.txt /home/esteban/     # Move a file to another directory
mv logs/ /var/log/investigation/ # Move a directory
```

#### Task 4 — Deleting Files and Directories

```bash
rm notes.txt                     # Delete a file
rm -r logs/                      # Delete a directory and its contents
rm -rf /tmp/old-evidence/        # Force delete without prompting — use carefully
```

**Warning:** `rm -rf` is irreversible. There is no recycle bin in Linux. In forensic work, always work on copies — never the original evidence.

---

### Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `touch` | `touch <filename>` | Create an empty file or update timestamps |
| `mkdir` | `mkdir -p <path>` | Create directories including nested paths |
| `cp` | `cp -r <src> <dest>` | Copy files or directories |
| `mv` | `mv <src> <dest>` | Move or rename files and directories |
| `rm` | `rm -rf <target>` | Delete files or directories permanently |

---

### Takeaways

- File timestamps (`touch` and `ls -la`) are critical forensic artifacts — modified, accessed, and created times (`mtime`, `atime`, `ctime`) tell the story of what happened and when
- `mv` is how attackers rename malicious files to look legitimate — knowing this helps spot renamed binaries in investigations
- Always work on a copy of evidence (`cp -r`) before analyzing — never modify original artifacts
- `mkdir -p` is essential for quickly building organized case folder structures during an incident

---

### References

- [LabEx Linux Skill Tree](https://labex.io/learn/cybersecurity-engineer)
- [Linux File Timestamps Explained](https://man7.org/linux/man-pages/man1/stat.1.html)
- [MITRE ATT&CK — Masquerading (T1036)](https://attack.mitre.org/techniques/T1036/)
