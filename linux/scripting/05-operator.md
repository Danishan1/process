
### 1. Arithmetic Operators

Used for **numbers**. Works in `(( ))` or `$(( ))`.

| Operator | Meaning            | Example              |
| -------- | ------------------ | -------------------- |
| `+`      | Addition           | `(( sum = a + b ))`  |
| `-`      | Subtraction        | `(( diff = a - b ))` |
| `*`      | Multiplication     | `(( prod = a * b ))` |
| `/`      | Division           | `(( div = a / b ))`  |
| `%`      | Modulo / remainder | `(( rem = a % b ))`  |
| `**`     | Exponentiation     | `(( pow = a ** b ))` |
| `++`     | Increment          | `(( a++ ))`          |
| `--`     | Decrement          | `(( a-- ))`          |

Example:

```bash
x=5
y=3
echo $(( x + y ))   # 8
```

### 2. Relational / Comparison Operators

### Numeric (`(( ))` or `[ ]` / `[[ ]]`)

| Operator     | Meaning               | Example         |
| ------------ | --------------------- | --------------- |
| `-eq` / `==` | equal                 | `[ $x -eq 5 ]`  |
| `-ne` / `!=` | not equal             | `[ $x -ne 3 ]`  |
| `-lt` / `<`  | less than             | `[ $x -lt 10 ]` |
| `-le` / `<=` | less than or equal    | `[ $x -le 10 ]` |
| `-gt` / `>`  | greater than          | `[ $x -gt 2 ]`  |
| `-ge` / `>=` | greater than or equal | `[ $x -ge 3 ]`  |

* `[ ]` → use `-eq, -ne, -lt`
* `(( ))` → use `==, !=, <, >`

### String comparisons

| Operator | Meaning                | Example             |
| -------- | ---------------------- | ------------------- |
| `=`      | equal                  | `[ "$s" = "hi" ]`   |
| `!=`     | not equal              | `[ "$s" != "bye" ]` |
| `<`      | lexically less than    | `[[ $a < $b ]]`     |
| `>`      | lexically greater than | `[[ $a > $b ]]`     |
| `-z`     | zero length (empty)    | `[ -z "$s" ]`       |
| `-n`     | non-empty              | `[ -n "$s" ]`       |


### 3. Logical Operators

Used to **combine conditions**.

| Operator | Meaning     | Example                       |
| -------- | ----------- | ----------------------------- |
| `&&`     | AND         | `[[ $x -gt 0 && $x -lt 10 ]]` |
| `\|\|`   | OR          | `[[ $x -gt 0 \|\| $x -lt 10 ]]` |
| `!`      | NOT         | `[[ ! -f file.txt ]]`         |
| `-a`     | AND (POSIX) | `[ $x -gt 0 -a $x -lt 10 ]`   |
| `-o`     | OR (POSIX)  | `[ $x -lt 0 -o $x -gt 100 ]`  |

### 4. File Test Operators

| Operator          | Meaning                    | Example               |
| ----------------- | -------------------------- | --------------------- |
| `-f`              | regular file exists        | `[ -f file.txt ]`     |
| `-d`              | directory exists           | `[ -d /tmp ]`         |
| `-e`              | exists (file or directory) | `[ -e /path ]`        |
| `-r`              | readable                   | `[ -r file.txt ]`     |
| `-w`              | writable                   | `[ -w file.txt ]`     |
| `-x`              | executable                 | `[ -x script.sh ]`    |
| `-s`              | non-empty file             | `[ -s file.txt ]`     |
| `file1 -nt file2` | file1 is newer than file2  | `[ file1 -nt file2 ]` |

### 5. String / Pattern Operators (with `[[ ]]`)

| Operator | Meaning                      | Example                  |
| -------- | ---------------------------- | ------------------------ |
| `==`     | pattern matching (wildcards) | `[[ $file == *.txt ]]`   |
| `!=`     | not match                    | `[[ $file != *.bak ]]`   |
| `=~`     | regex match                  | `[[ $str =~ ^[0-9]+$ ]]` |


### 6. Assignment Operators (Arithmetic & Strings)

| Operator | Meaning                         | Example              |
| -------- | ------------------------------- | -------------------- |
| `=`      | assign value                    | `x=5`                |
| `+=`     | add to numeric or append string | `x+=3`, `str+="abc"` |
| `-=`     | subtract                        | `x-=2`               |
| `*=`     | multiply                        | `x*=4`               |
| `/=`     | divide                          | `x/=2`               |
| `%=`     | modulo                          | `x%=3`               |


### 7. Misc / Special Operators

| Operator | Meaning                     | Example             |
| -------- | --------------------------- | ------------------- |
| `:`      | no-op / placeholder         | `: ${VAR:=default}` |
| `$?`     | last command exit status    | `if [ $? -eq 0 ]`   |
| `$(( ))` | arithmetic evaluation       | `echo $(( x+5 ))`   |
| `$@`     | all positional parameters   | `for arg in "$@"`   |
| `$#`     | number of positional params | `echo $#`           |


### Summary / Rule of Thumb 

1. **Arithmetic** → `+ - * / % ** ++ --` (use `(( ))`)
2. **Relational / Comparison** → `==, !=, <, >, -eq, -lt` (depends on context)
3. **Logical** → `&&, ||, !` (or `-a, -o` for POSIX)
4. **File checks** → `-f, -d, -r, -w, -x, -s, -nt`
5. **String / Pattern** → `=, !=, ==, !=, =~` (with `[[ ]]`)
6. **Assignment** → `=, +=, -=, *=, /=, %=`



### Bash Conditional Types: `(( ))` vs `[ ]` vs `[[ ]]`

| Feature / Use Case               | `[ ... ]` (POSIX test)                 | `[[ ... ]]` (Bash modern)                            | `(( ... ))` (Arithmetic)         |
| -------------------------------- | -------------------------------------- | ---------------------------------------------------- | -------------------------------- |
| **Purpose**                      | Value/file/string comparison, portable | Extended comparisons, safer, regex, pattern matching | Numeric / arithmetic evaluation  |
| **Portability**                  | High — works in `/bin/sh`              | Bash / Ksh / Zsh only                                | Bash / Ksh only                  |
| **Numeric comparison**           | `-eq, -ne, -lt, -le, -gt, -ge`         | Same, or use `(( ))` style                           | `==, !=, <, <=, >, >=` (C-style) |
| **String comparison**            | `=, !=, -z, -n`                        | `==, !=, <, >, =~`                                   | Not supported                    |
| **Pattern / Regex matching**     | No                                     | `==, !=` with `* ? []` or `=~`                       | No                               |
| **File tests**                   | `-f, -d, -e, -r, -w, -x, -s, -nt`      | Same as `[ ]`                                        | No                               |
| **Logical operators**            | `-a, -o, !`                            | `&&`, `\|\|` , `!`                                      | `&&`, `\|\|` , `!`            |
| **Arithmetic operators**         | `-eq` etc. only                        | Can use `(( ))` inside                               | `+,-,*,/,%,**,++,--`             |
| **Quoting variables**            | Required to avoid errors               | Usually not required                                 | Not required                     |
| **Supports exit-status command** | No                                     | No                                                   | No                               |
| **Examples**                     | `[ $x -eq 5 ]`                         | `[[ $x > 5 && $str == h* ]]`                         | `(( x > 5 && x < 10 ))`          |


### Rule of Thumb

1. **Use `[ ]`**

   * If **portable scripts** or very simple checks (numbers, strings, files).

2. **Use `[[ ]]`**

   * Default choice in **modern Bash scripts**.
   * Safer for unquoted strings.
   * Allows **pattern matching & regex**.
   * Handles complex logical expressions.

3. **Use `(( ))`**

   * For **arithmetic / numeric conditions**.
   * Clean, C-style syntax (`x < 5 && x > 0`).
   * No need for `-eq` / `-lt`.



### Examples Side-by-Side

```bash
x=10
str="hello"

# [ ] (portable)
if [ $x -gt 5 -a "$str" = "hello" ]; then
  echo "Match [ ]"
fi

# [[ ]] (modern Bash)
if [[ $x -gt 5 && $str == h* ]]; then
  echo "Match [[ ]]"
fi

# (( )) (arithmetic)
if (( x > 5 && x < 20 )); then
  echo "Match (( ))"
fi
```


This comparison makes it clear **which one to use depending on context**:

* `[ ]` → simple & portable
* `[[ ]]` → modern, strings, regex, patterns
* `(( ))` → numbers / arithmetic

