# Variables in Bash Scripting

### 1. **What is a variable?**

* A variable is a placeholder to store data (text, numbers, commands, etc.).
* In Bash, variables are **untyped** (everything is stored as a string unless you explicitly treat it as a number).


### 2. **Defining Variables**

```bash
NAME="Danishan"   # no spaces around =
AGE=25
```

Wrong:

```bash
NAME = "Danishan"  # spaces cause error
```


### 3. **Using Variables**

```bash
echo "My name is $NAME and I am $AGE years old"
```

* `$VAR` → expands the variable.
* Always quote variables `"${VAR}"` to prevent unexpected splitting.

### 4. **Environment vs Local Variables**

* **Local variable**: Only inside script/session

  ```bash
  VAR="hello"
  ```
* **Environment variable**: Exported so child processes can use it

  ```bash
  export VAR="hello"
  ```
* Example:

  ```bash
  ./script.sh
  echo $VAR  # Won’t work unless exported
  ```


### 5. **Command Substitution**

* Capture the output of a command into a variable:

  ```bash
  TODAY=$(date)
  FILES=$(ls)
  echo "Today is $TODAY"
  echo "Files: $FILES"
  ```

### 6. **Default Values**

* Syntax: `${VAR:-default}`

  ```bash
  echo "User is ${USER:-unknown}"
  echo "Editor is ${EDITOR:-nano}"
  ```


### 7. **Arithmetic with Variables**

* Use `(( ))` or `let` for integer math:

  ```bash
  X=5
  Y=3
  SUM=$((X + Y))
  echo "Sum = $SUM"
  ```

* Increment/Decrement:

  ```bash
  ((X++))
  ((Y--))
  ```


### 8. **Special Variables**

* `$0` → script name
* `$1, $2, ...` → positional arguments
* `$@` → all arguments
* `$#` → number of arguments
* `$$` → process ID of script
* `$?` → exit status of last command

### 9. **Arrays**

```bash
NAMES=("Alice" "Bob" "Charlie")
echo "First: ${NAMES[0]}"
echo "All: ${NAMES[@]}"
echo "Count: ${#NAMES[@]}"
```


### 10. **Associative Arrays (like dictionaries)**

```bash
declare -A COLORS
COLORS[apple]="red"
COLORS[banana]="yellow"
echo "Apple is ${COLORS[apple]}"
```


### 11. **Readonly & Unset**

```bash
readonly PI=3.14
unset VAR_NAME   # delete variable
```

---


# Bash Variables vs Complex Data Structures

Bash is **not a full programming language**; it’s a **scripting language** optimized for automation and command execution. Its variable system is simple, so the “complex data structures” are limited. Here’s what exists:


### 1. **Scalars (Basic Variables)**

* Strings or numbers (everything is essentially a string)

```bash
NAME="Danishan"
AGE=25
```


### 2. **Arrays**

* Indexed arrays (like lists)

```bash
FRUITS=("apple" "banana" "cherry")
echo ${FRUITS[0]}      # apple
echo ${FRUITS[@]}      # all elements
```

* Slicing and iteration are possible, but operations like sorting, filtering, or nested arrays are limited.


### 3. **Associative Arrays (like dictionaries)**

* Key-value mapping (bash ≥ 4)

```bash
declare -A COLORS
COLORS[apple]="red"
COLORS[banana]="yellow"

echo ${COLORS[apple]}   # red
```

* Limited: keys are strings, values are strings. No nested associative arrays naturally (workarounds exist using naming conventions).


### 4. **No Native Complex Structures**

* Bash **does not have objects, structs, tuples, or sets** like Python/Java.
* Nested structures require **workarounds**:

  * **Nested associative arrays**: emulate using delimiters in keys:

    ```bash
    declare -A USERS
    USERS["alice:age"]=25
    USERS["alice:city"]="NY"
    echo ${USERS["alice:city"]}
    ```
  * **Arrays of arrays**: tricky, often handled by combining indices or using strings with separators:

    ```bash
    ARR1=("a,b" "c,d")
    IFS=',' read -ra FIRST <<< "${ARR1[0]}"
    echo ${FIRST[0]}   # a
    ```


### 5. **External Workarounds**

For truly complex data, Bash scripts often **delegate to other tools**:

| Task                    | Tool/Method                                  |
| ----------------------- | -------------------------------------------- |
| Complex JSON objects    | `jq`                                         |
| Tabular data / matrices | `awk`, `csvkit`                              |
| Lists / dictionaries    | External programs or bash associative arrays |
| Multi-level data        | Use YAML/JSON + `yq` or `jq`                 |


### 6. **Comparison with Other Languages**

| Feature           | Bash            | Python / Java / C |
| ----------------- | --------------- | ----------------- |
| Scalar variables  | Yes             | Yes               |
| Arrays            | Indexed only    | Yes               |
| Dictionaries      | Associative     | Yes               |
| Objects / Structs | No              | Yes               |
| Sets / Tuples     | No              | Yes               |
| Nested structures | Only via tricks | Yes               |


### Summary

* **Bash variables are simple**: scalars, indexed arrays, associative arrays.
* **No native objects, nested structures, or complex data types**.
* For anything complex, Bash relies on **strings, delimiters, external tools**, or **switching to another language** (Python, Perl) from the script.
