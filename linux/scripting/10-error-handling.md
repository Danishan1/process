
# 1. What is an Exit Code?

* Every command in Bash returns an **exit status** (integer between 0–255).
* Convention:

  * `0` → success
  * Non-zero → failure (specific numbers may indicate specific errors)

```bash
ls /tmp
echo $?   # $? holds exit code of last command
```

---

# 2. Using Exit Codes in Conditions

```bash
if ls /notexist; then
    echo "Command succeeded"
else
    echo "Command failed"
fi
```

* Bash automatically checks the **exit code** of the command in `if`.

Equivalent with `$?`:

```bash
ls /notexist
if [ $? -eq 0 ]; then
    echo "Success"
else
    echo "Failure"
fi
```

---

# 3. Custom Exit Codes in Scripts

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "No arguments supplied"
    exit 1   # Custom exit code
fi

echo "Arguments provided"
exit 0
```

* `exit N` sets the **script’s exit code**.
* Useful for **automation and CI/CD pipelines**.

---

# 4. Error Handling with `set` Options

```bash
#!/bin/bash
set -e   # Exit immediately if any command fails
set -u   # Treat unset variables as errors
set -o pipefail  # Pipe fails if any command fails
```

* `set -e` → stops script on first error.
* `set -u` → avoids undefined variable mistakes.
* `set -o pipefail` → useful for pipes:

```bash
cat file.txt | grep "hello"
# If grep fails, script stops with pipefail
```

---

# 5. Trapping Errors

```bash
trap 'echo "Error occurred at line $LINENO"; exit 1' ERR
```

* `trap` catches **errors and signals**.
* `ERR` → triggered on any command failure.
* `$LINENO` → shows line number of the error.

---

# 6. Checking Command Success

* Logical AND (`&&`) → run next command only if previous succeeds:

```bash
mkdir /tmp/testdir && echo "Directory created"
```

* Logical OR (`||`) → run next command only if previous fails:

```bash
mkdir /tmp/testdir || echo "Directory already exists"
```

* Combine for fallback logic:

```bash
command1 && command2 || command3
```

---

# 7. Using Exit Codes in Functions

```bash
my_func() {
    ls /notexist
    return $?   # propagate last command exit code
}

my_func
if [ $? -ne 0 ]; then
    echo "Function failed"
fi
```

---

# 8. Logging Errors

```bash
#!/bin/bash
logfile="script.log"

echo "Starting script..." >> "$logfile"
cp file1.txt /tmp/ 2>> "$logfile"  # Append errors to log
```

* `2>>` → captures errors without stopping the script.
* Combine with `set -e` for stricter error handling.

---

# Summary / Best Practices

1. Always **check exit codes** for critical commands.
2. Use `set -euo pipefail` for **safer scripts**.
3. Use `trap` to **handle unexpected errors**.
4. Use **custom exit codes** for better automation.
5. Log errors using **stderr redirection** for debugging.
6. Combine `&&` and `||` for **concise error handling**.
