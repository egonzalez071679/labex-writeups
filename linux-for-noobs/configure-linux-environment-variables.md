# Configure Linux Environment Variables — LabEx (Linux for Noobs)

> **Course:** Linux for Noobs  
> **Type:** Challenge (applied)  
> **Difficulty:** Beginner  
> **Completed:** 2026-05-04  
> **Lab Link:** https://labex.io/labs/linux-configure-linux-environment-variables  
> **Companion Lab:** [Environment Variables in Linux](./environment-variables-in-linux.md)

---

## Summary

Applied challenge requiring creation of a persistent environment variable (`PROJECT_DIR=/home/labex/project`) that survives shell restart. Builds directly on the prior Environment Variables lab — this challenge tests independent application of `export` + `~/.zshrc` persistence + `source` reload, without step-by-step guidance.

---

## Key Concepts Reinforced

- **`export`** — promotes a shell variable to environment variable scope
- **Persistence via `~/.zshrc`** — appending `export` lines to the shell config makes the variable survive across sessions
- **`source`** — re-loads the config file in the current shell (no logout/login needed)
- **Verification with `echo`** — confirms the variable is accessible after persistence is set up
- **Path syntax matters** — `~/.zshrc` (with `/`) vs `~.zshrc` (without `/`) are completely different files

---

## Challenge Requirements

The task as given:

| Requirement | Met by |
|-------------|--------|
| Create a permanent environment variable `PROJECT_DIR` pointing to `/home/labex/project` | `export PROJECT_DIR=/home/labex/project` added to `~/.zshrc` |
| Use the `export` command | Command included in the `~/.zshrc` line |
| Add the variable to `~/.zshrc` | Appended via `echo` redirect, then verified with `nano` |
| Verify accessibility via `echo $PROJECT_DIR` | Output: `/home/labex/project` |
| Work within `/home/labex` directory | All commands run from there |

---

## Notes & Walkthrough

### First attempt — caught a path typo

The first append command had a subtle bug:

```bash
echo "export MY_VARIABLE='Permanent Environment Variable'" >> ~.zshrc
```

The destination is `~.zshrc` — note the **missing slash** between `~` and `.zshrc`. This redirects to a file literally named `~.zshrc` in the current working directory, NOT to `.zshrc` in the home directory.

The corrected command:

```bash
echo "export MY_VARIABLE='Permanent Environment Variable'" >> ~/.zshrc
```

The `/` after `~` is what makes the shell expand `~` to `$HOME`. Without it, `~` is treated as a literal character.

### Second attempt — verified the persistence pattern works

After fixing the path:

```bash
echo "export MY_VARIABLE='Permanent Environment Variable'" >> ~/.zshrc
source ~/.zshrc
echo $MY_VARIABLE
# Output: Permanent Environment Variable
```

This confirmed the persistence mechanism works end-to-end: append → reload → verify.

### Solving the actual challenge

The challenge required `PROJECT_DIR`, not `MY_VARIABLE`. Edited `~/.zshrc` directly with `nano` to add the correct line:

```bash
nano ~/.zshrc
# Added line: export PROJECT_DIR=/home/labex/project
```

Reload and verify:

```bash
source ~/.zshrc
echo $PROJECT_DIR
# Output: /home/labex/project
```

Challenge complete.

### Why use both `>>` and `nano` for editing config files

Two valid ways to modify `~/.zshrc`:

| Method | When to use |
|--------|-------------|
| `echo "..." >> ~/.zshrc` | Quick append for a single line, scripted/repeatable |
| `nano ~/.zshrc` | Editing existing lines, reviewing the full file, structured edits |

For real work, `nano` (or `vim`) is safer because you can see what's already there. For automation, `>>` redirect is faster.

---

## Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `export` | `export PROJECT_DIR=/home/labex/project` | Define an environment variable |
| `echo` | `echo "..." >> ~/.zshrc` | Append text to shell config |
| `nano` | `nano ~/.zshrc` | Edit shell config interactively |
| `source` | `source ~/.zshrc` | Reload config in current shell |
| `echo $VAR` | `echo $PROJECT_DIR` | Verify variable accessibility |

---

## Takeaways

- **The `~/` vs `~` distinction is small but consequential** — a missing `/` redirects output to a wrong file with a confusing name. In production scripts that automate config changes, this kind of typo can fail silently and lead to "why isn't my variable persisting?" debugging hours later. Worth lock-in muscle memory for the slash
- **`~/.zshrc` (and `~/.bashrc`) modification is a confirmed persistence technique** — every line in this challenge is functionally identical to what attackers do for shell-based persistence. T1546.004 (Unix Shell Configuration Modification) lives here. Knowing this from the inside (as someone who has *legitimately* modified these files) makes you faster at recognizing malicious modifications during DFIR work
- **`source` is forensically interesting on its own** — when investigators see `source` calls referencing files in unusual locations (`/tmp`, user-writable directories, anything outside `~/`), it's worth investigating. Attackers use `source` to execute payload-loaded scripts while making it look like routine shell activity
- **Challenges train independent application** — the lab taught the mechanism; this challenge proved I can apply it without guidance. Portfolio-relevant because hiring managers know that "completed a guided lab" is weaker evidence than "solved an applied challenge"
- **For Sec+ readiness**: configuration file persistence is testable in scripting/automation domains. The `export VAR=value` syntax + config file persistence is foundational

---

## References

- [LabEx — Configure Linux Environment Variables](https://labex.io/labs/linux-configure-linux-environment-variables)
- [Companion Lab — Environment Variables in Linux](./environment-variables-in-linux.md)
- [MITRE ATT&CK — Unix Shell Configuration Modification (T1546.004)](https://attack.mitre.org/techniques/T1546/004/)
- [Linux man page — bash(1)](https://man7.org/linux/man-pages/man1/bash.1.html) — Environment section
- [Linux man page — zsh(1)](https://man7.org/linux/man-pages/man1/zsh.1.html)
