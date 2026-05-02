# Environment Variables in Linux — LabEx (Linux for Noobs)

> **Course:** Linux for Noobs  
> **Type:** Lab (guided)  
> **Difficulty:** Beginner  
> **Completed:** 2026-05-02  
> **Lab Link:** https://labex.io/labs/linux-environment-variables-in-linux-385274

---

## Summary

Hands-on lab covering Linux shell variables, environment variables, the PATH variable, and how to make environment changes permanent. These concepts are foundational for understanding how user environments work — and how attackers manipulate them for persistence and privilege escalation.

---

## Key Concepts Learned

- **Shell variables** — exist only in the current shell session; not inherited by child processes
- **Environment variables** — exported variables that propagate to child processes (every program a shell launches inherits them)
- **`export` command** — the bridge between shell variables and environment variables
- **PATH** — colon-separated list of directories the shell searches for executable commands
- **Persistence via shell config files** — `.bashrc` (Bash) and `.zshrc` (Zsh) load on every new shell session
- **`source` command** — re-loads a config file in the current shell without restarting
- **`unset`** — removes a variable from the current session (does not edit the config file)
- **Important built-in variables** — `$HOME`, `$USER`, `$SHELL`, `$PWD`, `$TERM`

---

## Notes & Walkthrough

### Step 1 — Shell variables vs environment variables

Shell variables are local to the current shell session:

```bash
my_var="Hello, Linux"
echo $my_var
echo "The value of my_var is: $my_var"
```

The variable exists in this shell only. A child process (like a script) won't see it. To make it visible to children, **export** it:

```bash
export MY_ENV_VAR="This is an environment variable"
```

### Step 2 — Demonstrating the difference

Create a small test script that tries to read both variables:

```bash
cat << 'EOF' > test_vars.sh
#!/bin/bash
echo "Shell variable: $my_var"
echo "Environment variable: $MY_ENV_VAR"
EOF

chmod +x test_vars.sh
./test_vars.sh
```

The output will show that `$my_var` is empty (shell variable, not exported) but `$MY_ENV_VAR` shows the value (environment variable, exported).

### Step 3 — Inspecting environment variables

```bash
# Show all environment variables, filtered to your variable
env | grep MY_ENV_VAR

# Or just print one
echo $MY_ENV_VAR
```

The `env` command lists every environment variable in the current session — useful for incident response when investigating what context a process is running in.

### Step 4 — Modifying the PATH

PATH controls which directories the shell searches for commands. Adding a custom scripts directory:

```bash
mkdir ~/my_scripts
export PATH="$PATH:$HOME/my_scripts"
```

The `$PATH:` part appends to the existing PATH using `:` as the separator — losing this would replace PATH entirely and break the shell.

Now create a script in that directory:

```bash
cat << 'EOF' > ~/my_scripts/hello.sh
#!/bin/zsh
echo "Hello from my custom script!"
EOF

chmod +x ~/my_scripts/hello.sh
hello.sh
```

The script can now be run from anywhere — try `cd /tmp && hello.sh`. The shell finds it because `~/my_scripts` is in PATH.

### Step 5 — Making changes permanent

Without persistence, all of the above disappears when you close the terminal. To survive:

```bash
nano ~/.zshrc
```

Add at the bottom of the file:

```bash
export MY_ENV_VAR="This is an environment variable"
export PATH="$PATH:$HOME/my_scripts"
```

Apply changes without restarting the shell:

```bash
source ~/.zshrc
```

| Shell | Config file |
|-------|-------------|
| Bash | `~/.bashrc` (or `~/.bash_profile` on login shells) |
| Zsh | `~/.zshrc` |

### Step 6 — Important built-in environment variables

| Variable | What it shows |
|----------|---------------|
| `$HOME` | User's home directory path |
| `$USER` | Current username |
| `$SHELL` | Path to the user's default shell |
| `$PWD` | Current working directory |
| `$TERM` | Terminal type (xterm, xterm-256color, etc.) |

These are set automatically by the system at login. They appear in nearly every shell script and are reference points for many investigation tools.

### Step 7 — Removing environment variables

```bash
echo $MY_ENV_VAR        # confirm it exists
unset MY_ENV_VAR        # remove it
unset -v MY_ENV_VAR     # explicit "unset variable" flag (not function)
```

Important caveat: `unset` only affects the current session. If the variable is defined in `~/.zshrc`, it will reappear in the next shell session unless removed from the config file itself.

---

## Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `echo` | `echo $VAR` | Print a variable's value |
| `export` | `export VAR=value` | Promote a shell variable to environment variable |
| `env` | `env` | List all environment variables |
| `unset` | `unset VAR` | Remove a variable from current session |
| `source` | `source ~/.zshrc` | Reload shell config in current session |
| `nano` | `nano ~/.zshrc` | Edit shell config file |
| `chmod` | `chmod +x file.sh` | Make a script executable |

---

## Takeaways

- **`.bashrc` / `.zshrc` are persistence goldmines for attackers** — modifying these files to add malicious aliases, PATH entries, or commands that run on every shell start is a classic persistence technique. T1546.004 (Unix Shell Configuration Modification) lives here. Any DFIR investigation involving a compromised Linux user account checks these files
- **PATH manipulation is a hijacking vector** — if an attacker prepends a malicious directory to PATH, their fake `ls`, `ps`, or `sudo` runs instead of the real ones. T1574.007 (Path Interception). Reviewing `echo $PATH` on a suspect host is a Day-1 IR check
- **`env` output is forensically valuable** — process trees in EDR captures often include the full environment a process inherited. Looking for unusual `LD_PRELOAD`, modified PATH entries, or attacker-set debugging variables is part of process triage
- **`LD_PRELOAD` deserves special mention** (not in the lab, but adjacent) — this environment variable forces the dynamic linker to load a malicious shared library before any other; it's a common Linux rootkit technique. Knowing about environment variables in general makes spotting this easier
- **Shell vs environment variable distinction matters in scripting** — when reading attacker scripts during incident analysis, knowing which variables are exported tells you what context they expect their tools to run in
- **`source` is benign as a command but worth flagging in audit logs** — sourcing arbitrary files (especially from `/tmp` or user-writable locations) is a common technique for executing attacker code while making it look like routine shell activity
- **For Sec+ readiness**: shell environments come up in scripting and command-line questions; understanding PATH and environment variables is foundational for those domains

---

## References

- [LabEx — Environment Variables in Linux](https://labex.io/labs/linux-environment-variables-in-linux-385274)
- [Linux man page — bash(1)](https://man7.org/linux/man-pages/man1/bash.1.html) — Environment section
- [Linux man page — env(1)](https://man7.org/linux/man-pages/man1/env.1.html)
- [MITRE ATT&CK — Unix Shell Configuration Modification (T1546.004)](https://attack.mitre.org/techniques/T1546/004/)
- [MITRE ATT&CK — Path Interception by PATH Environment Variable (T1574.007)](https://attack.mitre.org/techniques/T1574/007/)
- [MITRE ATT&CK — Hijack Execution Flow: Dynamic Linker Hijacking (T1574.006)](https://attack.mitre.org/techniques/T1574/006/) — the LD_PRELOAD technique
