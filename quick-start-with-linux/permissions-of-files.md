# Permissions of Files — LabEx (Quick Start with Linux)

> **Course:** Quick Start with Linux  
> **Type:** Lab (guided)  
> **Difficulty:** Beginner  
> **Completed:** 2026  
> **Lab Link:** https://labex.io/courses/quick-start-with-linux

---

## Summary

Hands-on lab covering Linux file permissions — the foundational access control model that every Linux process, every log entry, and every privilege escalation attack ultimately ties back to. Read/Write/Execute permissions across Owner/Group/Other, expressed in both symbolic and octal notation.

---

## Key Concepts Learned

- **Three permission types** — read (`r`), write (`w`), execute (`x`)
- **Three permission classes** — owner (user), group, other (everyone else)
- **Symbolic notation** — `rwxr-xr--` reads left-to-right as owner / group / other
- **Octal notation** — each class summed: read=4, write=2, execute=1; common values like `755` (rwxr-xr-x) and `644` (rw-r--r--)
- **Special bits** — setuid, setgid, sticky bit (introduced briefly; deeper coverage in later labs)
- **Default permissions** — controlled by `umask`; new files typically `644`, new directories `755`

---

## Notes & Walkthrough

### Reading permissions with `ls -l`

```bash
ls -l file.txt
# -rw-r--r-- 1 user group 1234 Apr 18 10:30 file.txt
```

The leading 10 characters break down as:

| Position | Meaning |
|----------|---------|
| 1 | File type (`-` regular file, `d` directory, `l` symlink) |
| 2-4 | Owner permissions (rwx) |
| 5-7 | Group permissions (rwx) |
| 8-10 | Other permissions (rwx) |

### Symbolic notation with `chmod`

```bash
# Add execute for owner
chmod u+x script.sh

# Remove write from group and other
chmod go-w file.txt

# Set exact permissions for everyone
chmod a=r file.txt
```

| Symbol | Meaning |
|--------|---------|
| `u` | user (owner) |
| `g` | group |
| `o` | other |
| `a` | all |
| `+` | add permission |
| `-` | remove permission |
| `=` | set exactly |

### Octal notation with `chmod`

Each digit is the sum of permissions for that class:

| Value | Permissions |
|-------|-------------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 0 | --- |

```bash
# Owner full, group read/execute, other read
chmod 754 script.sh

# Standard executable script
chmod 755 script.sh

# Standard data file
chmod 644 data.txt

# Lock down to owner only
chmod 600 secret.txt
```

### Verifying changes

After every `chmod`, verify with `ls -l`. Permissions changes are silent — no error means it worked, but you should always confirm visually.

---

## Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `ls` | `ls -l` | Display file permissions in long format |
| `chmod` | `chmod 755 file` | Modify permissions (symbolic or octal) |
| `stat` | `stat file` | Detailed file metadata including permissions |
| `umask` | `umask` | View/set default permission mask for new files |

---

## Takeaways

- **Permissions are the first thing to check when investigating suspicious activity** — a script suddenly executable for "other" is a privilege escalation red flag
- **World-writable files (`o+w`, anything ending in `2`, `3`, `6`, `7`)** are common findings during compromise assessments — attackers create them to maintain persistence
- **644 and 755 are the safe defaults** — anything more permissive deserves a justification; SOC alerting often flags `chmod 777` events
- **Octal notation reads faster than symbolic once internalized** — `755` tells you owner-can-everything, group-and-other-can-read-and-execute in one glance, no parsing needed
- **For DFIR work**, file permissions tell you *who could have done this* — narrowing the suspect pool starts here
- **Permission changes leave audit trails** — on Linux, `chmod` events appear in audit logs; correlating them with login sessions is bread-and-butter incident reconstruction

---

## References

- [LabEx — Quick Start with Linux](https://labex.io/courses/quick-start-with-linux)
- [Linux man page — chmod(1)](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [MITRE ATT&CK — Linux and Mac File and Directory Permissions Modification (T1222.002)](https://attack.mitre.org/techniques/T1222/002/)
