# LabEx — Quick Start with Linux

> **Platform:** LabEx  
> **Skill Tree:** Linux  
> **Course:** Quick Start with Linux  
> **Difficulty:** Beginner  
> **Progress:** 6 / 10 labs completed  
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

---

## Lab 4 — Files and Directories

### Summary

Deeper dive into Linux file and directory management. Covers listing directory trees, understanding file types, working with hidden files, and using wildcards to operate on multiple files at once — building the navigation confidence needed to move quickly through an unfamiliar system during an investigation.

---

### Key Concepts Learned

- **`tree`** — Displays a directory structure visually as a branching tree
- **`file`** — Identifies the type of a file regardless of its extension
- **Hidden files** — Files starting with `.` are hidden from standard `ls` output; use `ls -a` to reveal them
- **Wildcards** — `*` matches any characters; `?` matches a single character; used to operate on multiple files at once
- **`find`** — Searches for files and directories by name, type, size, or modification time
- **Absolute vs relative paths** — Absolute paths start from `/`; relative paths start from your current directory

---

### Notes & Walkthrough

#### Task 1 — Visualizing Directory Structure

```bash
tree                        # Show full directory tree from current location
tree -L 2                   # Limit depth to 2 levels
tree /var/log               # Show tree of a specific path
tree -a                     # Include hidden files in the tree
```

#### Task 2 — Identifying File Types

```bash
file notes.txt              # Identify file type
file /bin/bash              # Shows: ELF 64-bit LSB executable
file suspicious_binary      # Useful for identifying disguised malware files
```

**Key insight:** Attackers often rename executables with innocent-looking extensions (e.g. `document.pdf`). `file` reads the magic bytes in the file header — not the extension — revealing the true type.

#### Task 3 — Working with Hidden Files

```bash
ls -a                       # Show all files including hidden ones
ls -la                      # Show hidden files with full details
ls -la /home/esteban/       # Check for hidden files in a user's home directory
```

#### Task 4 — Using Wildcards

```bash
ls *.log                    # List all .log files
cp *.txt /tmp/backup/       # Copy all .txt files to backup
rm temp_*                   # Delete all files starting with temp_
ls file?.txt                # Match file1.txt, file2.txt but not file10.txt
```

#### Task 5 — Finding Files

```bash
find / -name "passwd"                    # Find a file by name from root
find /var/log -name "*.log"              # Find all .log files in /var/log
find /home -user esteban                 # Find all files owned by a user
find /tmp -mtime -1                      # Files modified in the last 24 hours
find / -perm -4000 2>/dev/null           # Find SUID binaries — privilege escalation check
```

---

### Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `tree` | `tree -L 2` | Visualize directory structure to a set depth |
| `file` | `file <filename>` | Identify true file type from magic bytes |
| `ls` | `ls -la` | List all files including hidden with full details |
| `find` | `find <path> -name <pattern>` | Search for files by name, type, or attributes |

---

### Takeaways

- `file` is essential in malware triage — never trust a file extension; always verify the true type
- `find / -mtime -1` quickly surfaces recently modified files during an active incident investigation
- Hidden files (`.bash_history`, `.ssh/`, `.bashrc`) are prime artifacts in Linux forensics — always run `ls -la`
- `find / -perm -4000` finds SUID binaries — a common privilege escalation vector attackers exploit

---

### References

- [LabEx Linux Skill Tree](https://labex.io/learn/cybersecurity-engineer)
- [Linux `find` command — man page](https://man7.org/linux/man-pages/man1/find.1.html)
- [MITRE ATT&CK — Masquerading (T1036)](https://attack.mitre.org/techniques/T1036/)

---

## Lab 5 — File Contents and Comparing

### Summary

Covers reading, searching, and comparing file contents from the command line. Essential skills for log analysis and forensic investigation — the ability to quickly extract meaningful information from large text files without opening a GUI editor.

---

### Key Concepts Learned

- **`cat`** — Prints the full contents of a file to the terminal
- **`less`** — Opens a file for paginated reading; navigate with arrow keys, search with `/`
- **`head` / `tail`** — Show the first or last N lines of a file; `tail -f` follows live output
- **`grep`** — Searches file contents for a pattern; one of the most important tools in log analysis
- **`diff`** — Compares two files line by line and shows what changed between them
- **`wc`** — Counts lines, words, and characters in a file

---

### Notes & Walkthrough

#### Task 1 — Reading File Contents

```bash
cat /etc/passwd                     # Print full file contents
cat -n /var/log/syslog              # Print with line numbers
less /var/log/auth.log              # Open large file for paginated reading
                                    # Press / to search, q to quit
head -20 /var/log/syslog            # Show first 20 lines
tail -20 /var/log/syslog            # Show last 20 lines
tail -f /var/log/auth.log           # Follow live log output in real time
```

#### Task 2 — Searching File Contents with grep

```bash
grep "Failed password" /var/log/auth.log        # Find failed login attempts
grep -i "error" /var/log/syslog                 # Case-insensitive search
grep -r "password" /etc/                        # Recursive search through a directory
grep -n "sudo" /var/log/auth.log                # Show line numbers with matches
grep -c "FAILED" /var/log/auth.log              # Count matching lines
grep -v "INFO" /var/log/syslog                  # Show lines that do NOT match
grep "Failed" /var/log/auth.log | grep "root"   # Chain greps to narrow results
```

**Key insight:** `grep "Failed password" /var/log/auth.log` is one of the first commands run during a brute-force investigation — it instantly surfaces all failed SSH login attempts.

#### Task 3 — Comparing Files with diff

```bash
diff file1.txt file2.txt            # Show differences between two files
diff -u file1.txt file2.txt         # Unified format — easier to read
diff /etc/passwd /etc/passwd.bak    # Compare current config to a backup
```

**Key insight:** During incident response, `diff` is used to compare current system files against known-good baselines to detect unauthorized changes.

#### Task 4 — Counting with wc

```bash
wc -l /var/log/auth.log             # Count total lines in a log file
grep "Failed" /var/log/auth.log | wc -l   # Count failed login attempts
```

---

### Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `cat` | `cat -n <file>` | Print file contents with line numbers |
| `less` | `less <file>` | Paginated file reading with search |
| `head` | `head -N <file>` | Show first N lines of a file |
| `tail` | `tail -f <file>` | Show last lines; `-f` follows live output |
| `grep` | `grep -in <pattern> <file>` | Search file contents for a pattern |
| `diff` | `diff -u <file1> <file2>` | Compare two files and show differences |
| `wc` | `wc -l <file>` | Count lines in a file |

---

### Takeaways

- `tail -f` is how SOC analysts monitor logs in real time during an active incident — it streams new entries as they appear
- `grep "Failed password" /var/log/auth.log` is a foundational IR command — surfaces brute-force attempts instantly
- `diff` against a known-good baseline is a core file integrity checking technique used in DFIR investigations
- Chaining `grep | wc -l` quickly quantifies the scale of an event — how many failed logins, how many error entries

---

### References

- [LabEx Linux Skill Tree](https://labex.io/learn/cybersecurity-engineer)
- [Linux `grep` command — man page](https://man7.org/linux/man-pages/man1/grep.1.html)
- [MITRE ATT&CK — Log Enumeration (T1654)](https://attack.mitre.org/techniques/T1654/)

---

## Lab 6 — The Manuscript Mystery

### Summary

First hands-on challenge lab in the Quick Start with Linux course. Applies all skills from the previous five labs in a scenario-based investigation — navigating an unfamiliar filesystem, identifying files, reading and searching their contents, and piecing together clues to solve a mystery. Simulates the kind of exploratory, evidence-gathering work done during a real incident investigation.

---

### Key Concepts Learned

- **Scenario-based investigation** — Applying navigation, file identification, and content searching skills together in a realistic workflow
- **Command chaining** — Combining multiple commands with `|` to filter and extract information efficiently
- **Methodical filesystem exploration** — Starting broad (`ls`, `tree`) then narrowing down (`find`, `grep`) to locate relevant files
- **Reading unknown files** — Using `file` to identify type, then `cat` / `less` / `grep` to extract meaningful content

---

### Notes & Walkthrough

#### Investigation Approach

```bash
# Step 1 — Orient yourself
pwd && whoami && ls -la

# Step 2 — Map the environment
tree -L 3                           # Get a full picture of available directories

# Step 3 — Find relevant files
find . -name "*.txt" 2>/dev/null    # Search for text files
find . -mtime -7                    # Recently modified files

# Step 4 — Identify unknown files
file *                              # Check true type of all files in directory

# Step 5 — Read and search contents
cat clue.txt                        # Read a specific file
grep -r "manuscript" .              # Search all files for a keyword
grep -i "author" *.txt              # Search for author references

# Step 6 — Compare and correlate
diff document_v1.txt document_v2.txt   # Find what changed between versions
```

---

### Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `tree` | `tree -L 3` | Map the full directory structure |
| `find` | `find . -name "*.txt"` | Locate files matching a pattern |
| `file` | `file *` | Identify true type of all files |
| `cat` | `cat <file>` | Read file contents |
| `grep` | `grep -r <pattern> .` | Search recursively for a keyword |
| `diff` | `diff <file1> <file2>` | Compare two versions of a file |

---

### Takeaways

- Scenario-based labs are the best preparation for real IR work — following clues through a filesystem mirrors what analysts actually do during investigations
- A methodical approach (orient → map → find → identify → read → correlate) prevents missing evidence that a rushed search would skip
- Command chaining (`find | grep`, `cat | grep | wc -l`) dramatically speeds up evidence gathering in large filesystems
- The skills from Labs 1–5 — navigation, user info, file operations, directory exploration, content reading — all came together here as a unified investigative workflow

---

### References

- [LabEx Linux Skill Tree](https://labex.io/learn/cybersecurity-engineer)
- [Linux Command Line — Full Reference](https://man7.org/linux/man-pages/)
