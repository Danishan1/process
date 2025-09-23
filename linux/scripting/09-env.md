# 1. What Are Environment Variables?

* **Variables that are available system-wide** to the shell and any child process.
* Used to **store configuration, paths, and runtime information**.
* Examples: `PATH`, `HOME`, `USER`, `SHELL`.

```bash
echo $HOME    # Prints home directory
echo $USER    # Prints logged-in username
```

---

# 2. Listing Environment Variables

```bash
printenv
# or
env
```

* Shows all environment variables currently set in your session.

---

# 3. Setting Environment Variables

### Temporary (current shell session)

```bash
MY_VAR="Hello"
echo $MY_VAR
```

* **Visible only in current shell session**.
* Child processes **inherit** this variable if exported.

### Export to make available to child processes

```bash
export MY_VAR="Hello"
bash -c 'echo $MY_VAR'   # Child shell sees MY_VAR
```

* `export` makes the variable **part of the environment**.

---

# 4. Persistent Environment Variables

* Add them to shell configuration files:

| Shell | Config File                      |
| ----- | -------------------------------- |
| Bash  | `~/.bashrc` or `~/.bash_profile` |
| Zsh   | `~/.zshrc`                       |
| All   | `/etc/environment` (system-wide) |

Example:

```bash
echo 'export MY_VAR="Hello"' >> ~/.bashrc
source ~/.bashrc
```

---

# 5. Using Environment Variables in Scripts

```bash
#!/bin/bash
echo "User: $USER"
echo "Home: $HOME"

MY_VAR="HelloWorld"
export MY_VAR

bash -c 'echo $MY_VAR'   # Child shell can access MY_VAR
```

---

# 6. Unsetting Environment Variables

```bash
unset MY_VAR
echo $MY_VAR   # Empty
```

---

# 7. Special Environment Variables

| Variable | Meaning                                      |
| -------- | -------------------------------------------- |
| `PATH`   | Directories to search for commands           |
| `PWD`    | Current working directory                    |
| `OLDPWD` | Previous working directory                   |
| `HOME`   | User home directory                          |
| `SHELL`  | Current shell                                |
| `USER`   | Current logged-in user                       |
| `UID`    | User ID                                      |
| `IFS`    | Internal Field Separator (used in splitting) |

---

# 8. Using Environment Variables in Scripts

```bash
#!/bin/bash
echo "Your shell: $SHELL"
echo "Your path: $PATH"

# Custom variable
export PROJECT_DIR="/home/$USER/project"
echo "Project dir is $PROJECT_DIR"
```

* `$VARIABLE` → access variable
* `export VARIABLE` → make it available to child processes

---

# 9. Environment Variables vs Shell Variables

| Shell Variable         | Environment Variable                   |
| ---------------------- | -------------------------------------- |
| Local to current shell | Available to shell and child processes |
| `MY_VAR="value"`       | `export MY_VAR="value"`                |

---

# 10. Advanced Tip: Using Environment Variables in Scripts

* Use variables for **configurable scripts**:

```bash
#!/bin/bash
echo "Backup directory: ${BACKUP_DIR:-/tmp/backup}"
```

* `${VAR:-default}` → use default if variable is not set.
