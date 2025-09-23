### 1. `for` Loop (Iterating over a list)

**Basic syntax**

```bash
for item in a b c; do
  echo "Item: $item"
done
```

* Iterates over a **list of words**.
* Can also iterate over **files**:

```bash
for file in /tmp/*.txt; do
  echo "Processing $file"
done
```

* Or over **command output**:

```bash
for user in $(cut -d: -f1 /etc/passwd); do
  echo "User: $user"
done
```

> Use arrays or `while read` for safer handling of spaces/newlines.

### 2. C-style `for` Loop (Numeric)

```bash
for ((i=1; i<=5; i++)); do
  echo "Number $i"
done
```

* Like `for (i=1; i<=5; i++)` in C/Java.
* Supports **arithmetic expressions** directly.

### 3. `while` Loop (Condition-Based)

```bash
x=1
while [ $x -le 5 ]; do
  echo "x=$x"
  ((x++))
done
```

* Continues **while condition is true**.
* Good for **reading input or streaming data**:

```bash
while read line; do
  echo "Line: $line"
done < file.txt
```


### 4. `until` Loop (Opposite of `while`)

```bash
x=1
until [ $x -gt 5 ]; do
  echo "x=$x"
  ((x++))
done
```

* Runs **until condition becomes true** (inverse of while).


### 5. Loop Control Statements

| Statement  | Purpose                                       |
| ---------- | --------------------------------------------- |
| `break`    | Exit loop immediately                         |
| `continue` | Skip rest of current iteration, continue next |
| `exit`     | Exit script completely                        |

Example:

```bash
for i in {1..10}; do
  if (( i % 2 == 0 )); then
    continue    # skip even numbers
  fi
  if (( i > 7 )); then
    break       # stop loop
  fi
  echo $i
done
```

### 6. Nested Loops

```bash
for i in 1 2; do
  for j in a b; do
    echo "$i$j"
  done
done
```

* Useful for **matrix-like operations** or **multi-level processing**.


### 7. Infinite Loops

```bash
while true; do
  echo "Press [CTRL+C] to stop"
  sleep 1
done
```

* Common for **daemons, monitors, or watchers**.

### 8. Loops with Arrays

```bash
ARR=("apple" "banana" "cherry")
for fruit in "${ARR[@]}"; do
  echo "Fruit: $fruit"
done
```

* `[@]` → all elements
* `"${ARR[@]}"` preserves spaces in elements.

### 9. Loops with Command Output

```bash
for user in $(cut -d: -f1 /etc/passwd); do
  echo "User: $user"
done
```

* Beware: breaks on spaces/newlines in output.
* Safer alternative:

```bash
cut -d: -f1 /etc/passwd | while read user; do
  echo "User: $user"
done
```

### Summary

* **`for`** → iterate over lists, arrays, or command output
* **C-style `for`** → numeric iterations
* **`while` / `until`** → condition-based loops
* **Control statements** → `break`, `continue`, `exit`
* **Nested loops** → multi-level processing
