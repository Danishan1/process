### 1. Basic `if` Statements

```bash
x=10
if [ $x -gt 5 ]; then
  echo "x is greater than 5"
fi
```

* `[ ]` → test command (POSIX standard).
* `-gt`, `-lt`, `-eq` → numeric comparison.


### 2. `if-else`

```bash
if [ -f "file.txt" ]; then
  echo "file exists"
else
  echo "file missing"
fi
```

* `-f` → check if file exists and is a regular file.

### 3. `if-elif-else`

```bash
if [ $x -eq 0 ]; then
  echo "zero"
elif [ $x -lt 0 ]; then
  echo "negative"
else
  echo "positive"
fi
```

* Similar to `else if` in other languages.


### 4. File Test Conditions

Bash has many **file operators**:

```bash
[ -d mydir ]   # directory exists
[ -r file ]    # readable
[ -w file ]    # writable
[ -x file ]    # executable
[ file1 -nt file2 ]  # newer than
```

### 5. String Conditions

```bash
str="hello"
if [ -z "$str" ]; then
  echo "empty string"
fi

if [ "$str" = "hello" ]; then
  echo "match"
fi
```

* `-z` = empty string, `-n` = non-empty.
* `=` / `!=` → equality.

### 6. Compound Conditions

```bash
if [ $x -gt 0 ] && [ $x -lt 100 ]; then
  echo "x is between 1 and 99"
fi
```

* `&&` = AND, `||` = OR.
* `!` = NOT.


### 7. `[[ ]]` (Advanced Bash Test)

Bash’s extended test syntax:

```bash
if [[ $str == h* ]]; then
  echo "starts with h"
fi

if [[ $x -ge 10 && $x -le 20 ]]; then
  echo "in range"
fi
```

* Supports **regex**:

```bash
if [[ "abc123" =~ [0-9]+ ]]; then
  echo "string has numbers"
fi
```

### 8. `case` Statement

For multiple choices (cleaner than many `elif`):

```bash
fruit="apple"
case $fruit in
  apple)  echo "red fruit" ;;
  banana) echo "yellow fruit" ;;
  *)      echo "unknown" ;;
esac
```

### 9. Arithmetic Conditions (`(( ))`)

Used for **numeric checks**:

```bash
num=5
if (( num % 2 == 0 )); then
  echo "even"
else
  echo "odd"
fi
```

### 10. Exit Status Conditions

Every command returns an exit code (`0 = success`, nonzero = failure).

```bash
if grep "hello" file.txt > /dev/null; then
  echo "found"
else
  echo "not found"
fi
```


### 11. Combining with Loops

Conditions also drive loops:

```bash
while [ $x -lt 5 ]; do
  echo "x=$x"
  ((x++))
done
```


**Summary of Progression**

1. `if` basics → numeric, strings, files.
2. Logical operators (`&&`, `||`, `!`).
3. Extended `[[ ]]` and regex.
4. `case` for multi-branching.
5. Arithmetic `(( ))` and exit codes.


---

# The Four Main Condition Styles in Bash

### 1. `if [ ... ]`  (POSIX test command)

* Oldest, most portable (works in `/bin/sh`).
* Limited functionality.
* Syntax can be fragile (spaces matter).

Example:

```bash
if [ "$x" -gt 5 ]; then
  echo "greater than 5"
fi
```

Use this when:

* Writing **portable scripts** that must run on any Unix-like system.
* Simple checks (numbers, strings, files).

Pitfalls:

* Must quote variables to avoid errors:

  ```bash
  if [ "$str" = "hello" ]; then ...
  ```


### 2. `if [[ ... ]]`  (Bash/Ksh/Zsh extension)

* Safer and more powerful.
* Supports pattern matching (`== h*`), regex (`=~`), no need for quotes in most cases.
* Handles complex logical expressions cleanly.

Example:

```bash
if [[ $str == h* && $x -ge 10 ]]; then
  echo "starts with h and >=10"
fi
```

Use this when:

* You are **sure the script runs under Bash** (not plain sh).
* Need **regex, wildcards, or compound logic**.
* Want fewer bugs with unquoted variables.

Limitation:

* Not POSIX portable (won’t work in `/bin/sh`).


### 3. `if (( ... ))`  (Arithmetic evaluation)

* Special syntax for **numeric comparisons**.
* No need for `-gt`, `-lt` → uses C-style operators (`<`, `>`, `==`, `!=`).

Example:

```bash
if (( x > 5 && x < 20 )); then
  echo "x between 6 and 19"
fi
```

Use this when:

* Working with **numbers only**.
* Cleaner syntax than `[ ]` or `[[ ]]` for arithmetic.

Limitation:

* Only for integers (not strings, not files).

### 4. if `condition` (Command-Based Conditions)


* Bash allows **command directly in `if`**:

```bash
if command; then
    echo "success"
fi
```

* Equivalent to:

```bash
command
if [ $? -eq 0 ]; then
    echo "success"
fi
```

> `$?` holds the exit code of the last command.


### **The Principle**

* In Bash, **every command returns an exit status**:

  * `0` → success / true
  * Non-zero → failure / false
* `if` checks this **exit code**, not the output.

Example:

```bash
if grep "hello" file.txt > /dev/null; then
  echo "found"
else
  echo "not found"
fi
```

**Explanation:**

* `grep "hello" file.txt` searches for "hello".
* `> /dev/null` discards the output because we only care if it exists.
* If grep finds a match → exit code `0` → `then` block executes.
* If grep finds nothing → exit code `1` → `else` block executes.


### **Other Examples**

```bash
# Check if directory exists
if mkdir /tmp/mydir 2>/dev/null; then
  echo "Directory created"
else
  echo "Directory already exists"
fi

# Ping host
if ping -c 1 google.com &>/dev/null; then
  echo "Host reachable"
else
  echo "Host unreachable"
fi
```

## Rule of Thumb (When to Use What)

* **`[ ]`** → When you need **maximum portability**. (Old-school POSIX scripts).
* **`[[ ]]`** → Default choice in **modern Bash scripts**. Safer, more powerful.
* **`(( ))`** → When comparing **numbers** (cleaner than `-gt/-lt`).
* **`if command`** → command’s exit status → very powerful for real-world scripts

 In practice:

* If you’re writing **just for Linux with Bash** → prefer `[[ ]]` and `(( ))`.
* If you’re writing scripts that might run in **/bin/sh on older systems** → stick to `[ ]`.

---

**Complete list of condition operators and expressions** you can use inside each of the three types: `[ ]`, `[[ ]]`, and `(( ))`. I’ll categorize them clearly for numeric, string, file, and logical operations.

# 1. `[ ]` (POSIX / portable)

### **Numeric comparisons**

| Operator | Meaning               | Example         |
| -------- | --------------------- | --------------- |
| `-eq`    | equal                 | `[ $x -eq 5 ]`  |
| `-ne`    | not equal             | `[ $x -ne 0 ]`  |
| `-lt`    | less than             | `[ $x -lt 10 ]` |
| `-le`    | less than or equal    | `[ $x -le 5 ]`  |
| `-gt`    | greater than          | `[ $x -gt 2 ]`  |
| `-ge`    | greater than or equal | `[ $x -ge 3 ]`  |

### **String comparisons**

| Operator | Meaning                    | Example                |
| -------- | -------------------------- | ---------------------- |
| `=`      | equal                      | `[ "$str" = "hello" ]` |
| `!=`     | not equal                  | `[ "$str" != "hi" ]`   |
| `-z`     | zero length (empty string) | `[ -z "$str" ]`        |
| `-n`     | non-zero length            | `[ -n "$str" ]`        |

### **File tests**

| Operator          | Meaning                | Example               |
| ----------------- | ---------------------- | --------------------- |
| `-f`              | file exists (regular)  | `[ -f file.txt ]`     |
| `-d`              | directory exists       | `[ -d /tmp ]`         |
| `-e`              | exists (file/dir)      | `[ -e /path ]`        |
| `-r`              | readable               | `[ -r file.txt ]`     |
| `-w`              | writable               | `[ -w file.txt ]`     |
| `-x`              | executable             | `[ -x script.sh ]`    |
| `-s`              | non-empty              | `[ -s file.txt ]`     |
| `file1 -nt file2` | file1 newer than file2 | `[ file1 -nt file2 ]` |

### **Logical operators**

* Combine tests with `-a` (AND), `-o` (OR), `!` (NOT)

```bash
[ $x -gt 5 -a $x -lt 10 ]
[ ! -f file.txt ]
```


## 2. `[[ ]]` (Bash/Ksh/Zsh, modern)

### **Numeric comparisons**

* Can use `[ ]` style: `-eq`, `-ne`, etc.
* Or prefer `(( ))` style inside for arithmetic.

### **String comparisons**

| Operator | Meaning                      | Example                  |
| -------- | ---------------------------- | ------------------------ |
| `==`     | equal / pattern matching     | `[[ $str == hello ]]`    |
| `!=`     | not equal / pattern matching | `[[ $str != hi ]]`       |
| `<`      | lexically less than          | `[[ $str < z ]]`         |
| `>`      | lexically greater than       | `[[ $str > a ]]`         |
| `=~`     | regex match                  | `[[ $str =~ ^[0-9]+$ ]]` |

* Pattern matching with `*` / `?` / `[]`:

```bash
[[ $file == *.txt ]]
[[ $name == A* ]]
```

### **Logical operators**

* `&&` → AND, `||` → OR, `!` → NOT

```bash
[[ $x -gt 0 && $x -lt 10 ]]
[[ ! -f file.txt ]]
```

### **File tests**

* Same as `[ ]`, but no quoting issues:

```bash
[[ -f file.txt && -r file.txt ]]
```


## 3. `(( ))` (Arithmetic / integer evaluation)

* Only numeric comparisons; no strings, no files.
  | Operator          | Meaning                        | Example            |
  |-------------------|--------------------------------|--------------------|
  | `==`              | equal                          | `(( x == 5 ))`     |
  | `!=`              | not equal                      | `(( x != 0 ))`     |
  | `<`               | less than                      | `(( x < 10 ))`     |
  | `<=`              | less than or equal             | `(( x <= 10 ))`    |
  | `>`               | greater than                   | `(( x > 2 ))`      |
  | `>=`              | greater than or equal          | `(( x >= 3 ))`     |
  | `+ - * / % **`    | arithmetic operations          | `(( sum = x + y ))` |
  | `++` / `--`       | increment / decrement          | `(( x++ ))`        |
  | `&&` / `||`       | logical AND / OR               | `(( x>0 && y<5 ))` |
  | `!`               | logical NOT                    | `(( !x ))`         |

* Supports **C-style arithmetic expressions**.

---

## Bash Conditional Operators Cheat Sheet

| **Category**                   | **\[ ] (POSIX / portable)**                   | **\[\[ ]] (Bash modern)**                    | **(( )) (Arithmetic)**                           | **Notes / Examples**                             |
| ------------------------------ | --------------------------------------------- | -------------------------------------------- | ------------------------------------------------ | ------------------------------------------------ |
| **Numeric**                    | `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`      | Same as `[ ]`, or use `(( ))`                | `==`, `!=`, `<`, `<=`, `>`, `>=`, `+,-,*,/,%,**` | `[ $x -gt 5 ]`, `(( x>5 && x<10 ))`              |
| **String equality**            | `=` , `!=`                                    | `==`, `!=`                                   | N/A                                              | `[ "$s" = "hi" ]`, `[[ $s == h* ]]`              |
| **String tests**               | `-z` (empty), `-n` (non-empty)                | `-z`, `-n`                                   | N/A                                              | `[ -z "$str" ]`, `[[ -n $str ]]`                 |
| **File tests**                 | `-f, -d, -e, -r, -w, -x, -s, file1 -nt file2` | Same as `[ ]`                                | N/A                                              | `[ -f file.txt ]`, `[[ -x script.sh ]]`          |
| **Pattern match**              | N/A                                           | `==`, `!=`, `=~` (regex), wildcards `* ? []` | N/A                                              | `[[ $file == *.txt ]]`, `[[ $str =~ ^[0-9]+$ ]]` |
| **Logical AND / OR**           | `-a` (AND), `-o` (OR), `!` (NOT)              | `&&` (AND), \`                               |                                                  | `(OR),`!\` (NOT), `&&`, \`,  `, `!`, `[ $x -gt 0 -a $x -lt 10 ]` vs `[[ $x>0 && $x<10 ]]`|
| **Regex**                      | N/A                                           | `=~`                                         | N/A                                              | `[[ $str =~ ^[a-z]+$ ]]`                         | 
| **Increment / Arithmetic ops** | N/A                                           | N/A                                          | `++`, `--`, `+,-,*,/,%,**`                       | `(( x++ ))`, `(( sum = a + b ))`                 | 
| **Lexical comparison**         | N/A                                           | `<`, `>`                                     | N/A                                              | `[[ $a < $b ]]` → string comparison              |
---

### Usage Guidelines

1. **`[ ]`** → portable, simple scripts, numeric/string/file comparisons.
2. **`[[ ]]`** → modern Bash, supports regex, wildcards, compound logic; safer with unquoted vars.
3. **`(( ))`** → numeric-only arithmetic, C-style operators, clean syntax for numbers.
