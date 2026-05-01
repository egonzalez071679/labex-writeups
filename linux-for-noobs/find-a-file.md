# Find a File — LabEx (Linux for Noobs)

> **Course:** Linux for Noobs  
> **Type:** Challenge (applied)  
> **Difficulty:** Beginner  
> **Completed:** 2026  
> **Lab Link:** https://labex.io/courses/linux-for-noobs

---

## Summary

Challenge applying Linux file search techniques — `find`, `locate`, `which`, and `whereis`. Beyond surface utility, these are the most-used commands in Linux DFIR work for locating evidence: recently modified files, suspicious binaries, files matching specific signatures.

---

## Key Concepts Learned

- **`find`** — real-time recursive filesystem search; slow but accurate
- **`locate`** — fast cached search using a pre-built database (`updatedb`); fast but can be stale
- **`which`** — locates the executable of a command in `$PATH`; useful for "which version of `python` am I actually running?"
- **`whereis`** — locates binary, source, and man page for a command
- **Search criteria** — by name, type, size, modification time, ownership, permissions
- **Action operators** — `-print` (default), `-exec`, `-delete` for acting on results

---

## Notes & Walkthrough

### `find` — the workhorse

Basic syntax:

```bash
find <where> <criteria> <action>
```

Common patterns:

```bash
# Find by name (case-sensitive)
find /home -name "*.log"

# Find by name (case-insensitive)
find /var -iname "auth*"

# Find by file type
find /tmp -type f                # files only
find / -type d -name "secret"    # directories named secret

# Find by modification time
find /var/log -mtime -1          # modified in last 24 hours
find /home -mmin -60             # modified in last 60 minutes

# Find by size
find / -size +100M                # larger than 100MB
find /tmp -size 0                 # empty files

# Find by ownership
find / -user root
find / -nouser                    # files with no valid owner (red flag)

# Find by permissions
find / -perm 4000                 # SUID files (privilege escalation hunting)
find / -perm -o+w                 # world-writable files
```

### Combining `find` with actions

```bash
# Print results (default)
find /etc -name "*.conf" -print

# Execute a command per result
find /tmp -name "*.tmp" -exec rm {} \;

# Pipe to other tools
find /var/log -name "*.log" -mtime -1 | xargs grep "Failed password"
```

### `locate` — fast lookups

```bash
# Find any file matching a pattern (uses pre-built database)
locate passwd

# Update the database (typically run daily by cron)
sudo updatedb
```

`locate` is much faster than `find` for known filenames, but its results are only as fresh as the last `updatedb` run. For investigation work, `find` is more reliable.

### `which` and `whereis`

```bash
# Which python is in my PATH?
which python3
# /usr/bin/python3

# All locations for a command
whereis python3
# python3: /usr/bin/python3 /usr/lib/python3 /usr/share/man/man1/python3.1.gz
```

In incident response, `which` is critical for verifying you're running the legitimate binary, not an attacker's substitute earlier in `$PATH`.

---

## Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `find` | `find / -name "*.log" -mtime -1` | Real-time recursive search, supports complex criteria |
| `locate` | `locate filename` | Fast cached search using `updatedb` database |
| `which` | `which command` | Locate the active binary for a command |
| `whereis` | `whereis command` | Locate binary, source, and man page |
| `updatedb` | `sudo updatedb` | Refresh `locate`'s database |

---

## Takeaways

- **`find` is the single most-used command in Linux DFIR** — every investigation eventually asks "what files changed in the last X hours" or "which files have SUID set?"
- **SUID hunt (`find / -perm -4000`)** is a standard step in compromise assessments — attackers set SUID on binaries for privilege escalation
- **Time-based searches (`-mtime`, `-mmin`, `-newer`)** are how analysts narrow down the timeline of a breach to a specific window
- **`-nouser` and `-nogroup` flags** find files orphaned from valid accounts — often left behind after attacker cleanup
- **`which` matters in compromise scenarios** — if `$PATH` has been manipulated, `which` reveals which binary actually runs when you type a command
- **Combining `find -exec` with `grep`** lets you search across thousands of files with specific criteria — the Linux equivalent of an enterprise SIEM query
- **For Sec+ / SOC L1 readiness:** memorize `find -name`, `find -mtime`, `find -perm`, `find -user` — these four patterns cover 80% of investigation use cases

---

## References

- [LabEx — Linux for Noobs](https://labex.io/courses/linux-for-noobs)
- [Linux man page — find(1)](https://man7.org/linux/man-pages/man1/find.1.html)
- [Linux man page — locate(1)](https://man7.org/linux/man-pages/man1/locate.1.html)
- [MITRE ATT&CK — File and Directory Discovery (T1083)](https://attack.mitre.org/techniques/T1083/)
- [SANS — Linux DFIR Cheat Sheet](https://www.sans.org/posters/linux-shell-survival-guide/)
