### 1. Standard Streams in Bash

Bash has **three standard streams**:

| Stream | File Descriptor | Purpose         |
| ------ | --------------- | --------------- |
| stdin  | 0               | Standard input  |
| stdout | 1               | Standard output |
| stderr | 2               | Standard error  |

* **stdin** → input to commands
* **stdout** → normal output
* **stderr** → error messages


### 2. Output Redirection

### **Redirect stdout to a file**

```bash
echo "Hello World" > output.txt
```

* `>` → overwrite file.

```bash
echo "Append this" >> output.txt
```

* `>>` → append to file.

### **Redirect stderr**

```bash
ls /notexist 2> error.log
```

* `2>` → redirect **error messages**.

```bash
ls /notexist 2>> error.log
```

* Append errors instead of overwriting.

### **Redirect both stdout and stderr**

```bash
command > all.log 2>&1
# or equivalently
command &> all.log
```

* Captures **everything** (output + errors) into one file.

### 3. Input Redirection

```bash
sort < input.txt
```

* `<` → takes input from a file instead of keyboard.

* Common in loops:

```bash
while read line; do
    echo "Line: $line"
done < file.txt
```

### 4. Pipes `|`

* **Pipe** sends the **stdout of one command** to the **stdin of another**.

```bash
ls -l | grep ".txt"
```

* Output of `ls -l` goes into `grep ".txt"`.

```bash
cat file.txt | sort | uniq
```

* Combine multiple commands.
* Avoid unnecessary `cat` when possible: `sort file.txt | uniq`.

### 5. Combining Pipes and Redirection

```bash
grep "error" logfile.txt | tee errors.txt
```

* `tee` writes output to **file and stdout simultaneously**.

```bash
grep "error" logfile.txt 2>&1 | tee errors.txt
```

* Redirects **errors too**.

### 6. Here Documents (`<<`)

* Useful for **multi-line input** to commands.

```bash
cat << EOF > file.txt
Line 1
Line 2
Line 3
EOF
```

* Everything between `<< EOF` and `EOF` is sent to `cat` → written to `file.txt`.

* Can also use variables:

```bash
name="Alice"
cat << EOF
Hello $name
Welcome!
EOF
```

### 7. Here Strings (`<<<`)

* Sends a **single string** as input to a command:

```bash
grep "hello" <<< "hello world"
```

* Equivalent to: `echo "hello world" | grep "hello"`

### 8. Advanced Redirection Tricks

* **Discard output:**

```bash
command > /dev/null
command &> /dev/null   # discard output + errors
```

* **Swap stdout and stderr:**

```bash
command 3>&2 2>&1 1>&3
```

* Rare, used in complex logging.

### Summary

1. **`>`** → stdout overwrite, **`>>`** → stdout append
2. **`2>`** → stderr, **`2>&1`** → redirect errors to stdout
3. **`|`** → pipe stdout to another command
4. **`<`** → input from file
5. **`<<`** → here document (multi-line input)
6. **`<<<`** → here string (single-line input)
7. **`tee`** → write to file **and stdout**


---

## let’s go step by step with **examples using actual file content**, so you can clearly see how **redirection and pipes** work in practice.

### 1. Sample File: `data.txt`

Let’s assume `data.txt` contains:

```
Alice,20
Bob,25
Charlie,30
David,25
Eve,20
```

### 2. Redirect stdout to a file

```bash
# Create a new file with sorted content
sort data.txt > sorted.txt
```

* `sorted.txt` will now contain:

```
Alice,20
Bob,25
Charlie,30
David,25
Eve,20
```

* If you use `>>` instead of `>`, it would **append** instead of overwrite.

### 3. Redirect stderr

```bash
# Try listing a non-existent file
ls missingfile.txt 2> error.log
```

* `error.log` will contain the error message like:

```
ls: cannot access 'missingfile.txt': No such file or directory
```

* Nothing appears on the terminal because stderr is redirected.

### 4. Redirect both stdout and stderr

```bash
ls data.txt missingfile.txt > output.log 2>&1
```

* `output.log` now contains:

```
data.txt
ls: cannot access 'missingfile.txt': No such file or directory
```

* Both normal output and errors go to the same file.

### 5. Pipe Example

```bash
# Find users with age 25
cat data.txt | grep ",25"
```

Output:

```
Bob,25
David,25
```

* Here, `cat` outputs the file, and `grep` filters lines containing `,25`.

### 6. Combine Pipe with Sort and Uniq

```bash
# Find unique ages
cut -d',' -f2 data.txt | sort | uniq
```

Output:

```
20
25
30
```

Explanation:

1. `cut -d',' -f2` → extract the second field (age).
2. `sort` → sort numerically or alphabetically.
3. `uniq` → remove duplicates.

### 7. Using `tee` to save and display output

```bash
grep ",25" data.txt | tee age25.txt
```

* Output on terminal:

```
Bob,25
David,25
```

* Also saved in `age25.txt`.

### 8. Here Document (`<<`)

```bash
cat << EOF > newfile.txt
Name,Age
Frank,40
Grace,35
EOF
```

* Creates `newfile.txt` with:

```
Name,Age
Frank,40
Grace,35
```

### 9. Here String (`<<<`)

```bash
grep "Alice" <<< "$(cat data.txt)"
```

* Equivalent to: `cat data.txt | grep "Alice"`
* Output:

```
Alice,20
```


### Summary

| Feature            | Example        | Result                       |                                |
| ------------------ | -------------- | ---------------------------- | ------------------------------ |
| stdout redirection | `> file.txt`   | overwrite file               |                                |
| append             | `>> file.txt`  | append to file               |                                |
| stderr redirection | `2> error.txt` | capture errors               |                                |
| both stdout+stderr | `> out 2>&1`   | capture everything           |                                |
| pipe               | \`             | \`                           | send output to another command |
| tee                | \`             | tee file.txt\`               | display and save               |
| here document      | `<< EOF`       | multi-line input to command  |                                |
| here string        | `<<<`          | single-line input to command |                                |
