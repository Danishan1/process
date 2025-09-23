### 1. Basic Function Syntax

```bash
my_function() {
    echo "Hello from my_function"
}

# Call the function
my_function
```

* `function` keyword is optional:

```bash
function my_function {
    echo "Hello"
}
```


### 2. Functions with Parameters

```bash
greet() {
    echo "Hello, $1!"
}

greet "Alice"
```

* `$1, $2, ...` → positional parameters passed to the function.
* `$@` → all parameters.
* `$#` → number of parameters.

Example:

```bash
sum() {
    echo "Sum: $(($1 + $2))"
}

sum 5 7   # Output: Sum: 12
```

### 3. Return Value from Function

```bash
square() {
    local num=$1
    echo $((num * num))
}

result=$(square 4)
echo "Square is $result"
```

* Functions in Bash **cannot return strings directly** using `return`.
* `return` only returns **numeric exit code (0–255)**:

```bash
is_even() {
    local n=$1
    if (( n % 2 == 0 )); then
        return 0   # true
    else
        return 1   # false
    fi
}

is_even 6
echo $?  # 0 (true)
```


### 4. Local vs Global Variables

```bash
global_var="I am global"

my_func() {
    local local_var="I am local"
    echo "Inside: $local_var, $global_var"
}

my_func
echo "Outside: $local_var"  # Empty, local_var is not visible
```

* Use `local` keyword to **limit variable scope** to the function.


### 5. Functions with Default Values

```bash
greet() {
    local name=${1:-Guest}
    echo "Hello, $name"
}

greet          # Hello, Guest
greet Alice    # Hello, Alice
```

* `${1:-default}` → use default if argument is missing.


### 6. Functions Calling Other Functions

```bash
say_hello() {
    echo "Hello"
}

say_world() {
    say_hello
    echo "World"
}

say_world
```

* Functions can **call each other**, enabling modular scripts.


### 7. Tips for Using Functions in Bash

1. **Keep functions small and modular** – one task per function.
2. **Use `local` variables** inside functions to avoid accidental global changes.
3. **Return values using `echo`**, capture with `$(...)`.
4. **Use parameters** instead of hard-coding values.
5. Functions can be **defined anywhere** in the script, but must be defined before calling if you’re running in POSIX `sh`.


---

Now we move to **advanced Bash function concepts**. These are useful for **writing professional, reusable, and maintainable scripts**.

### 1. Functions with Arrays

```bash
sum_array() {
    local arr=("$@")  # Capture all arguments as array
    local sum=0
    for num in "${arr[@]}"; do
        ((sum += num))
    done
    echo "$sum"
}

numbers=(1 2 3 4 5)
result=$(sum_array "${numbers[@]}")
echo "Sum = $result"
```

* `"$@"` → all arguments passed to the function.
* Arrays are passed as **space-separated values**, so quoting is important.


### 2. Recursive Functions

```bash
factorial() {
    local n=$1
    if (( n <= 1 )); then
        echo 1
    else
        local prev=$(factorial $((n-1)))
        echo $((n * prev))
    fi
}

result=$(factorial 5)
echo "Factorial = $result"
```

* Functions can **call themselves**.
* Always include a **base case** to avoid infinite recursion.


# 3. Function Returning Exit Codes

```bash
is_even() {
    (( $1 % 2 == 0 ))
}

is_even 10
if (( $? == 0 )); then
    echo "Even"
else
    echo "Odd"
fi
```

* `return` or exit code is limited to **0–255**.
* Commonly used for **status checks**.


### 4. Functions with Named Parameters (Workaround)

Bash doesn’t support named parameters directly, but you can emulate:

```bash
greet() {
    local name=""
    local greeting="Hello"

    while [[ $# -gt 0 ]]; do
        case $1 in
            --name) name="$2"; shift 2 ;;
            --greeting) greeting="$2"; shift 2 ;;
            *) shift ;;
        esac
    done

    echo "$greeting, $name"
}

greet --name Alice --greeting Hi
```

* Allows flexible arguments like **command-line tools**.


### 5. Function Scope and `local`

```bash
global_var=10

my_func() {
    local local_var=5
    global_var=20
    echo "Inside: local_var=$local_var, global_var=$global_var"
}

my_func
echo "Outside: global_var=$global_var, local_var=${local_var:-undefined}"
```

* `local_var` is **not visible outside**.
* `global_var` **changes are global**.


### 6. Functions in Scripts / Sourcing

* Functions can be **defined in a separate file** and sourced:

```bash
# utils.sh
greet() { echo "Hello $1"; }

# main.sh
source ./utils.sh
greet Alice
```

* Allows **reusable libraries of functions** across scripts.


### 7. Anonymous Functions (Using Subshell / eval)

Bash does not support real anonymous functions like Python, but you can do tricks:

```bash
tmp_func=$(cat <<'EOF'
() { echo "I am temporary"; }
EOF
)

eval "myfunc$tmp_func"
myfunc
```

* Rarely used; mainly **advanced dynamic scripting**.


### 8. Combining Functions with Loops, Conditions, and Arrays

```bash
process_files() {
    local files=("$@")
    for f in "${files[@]}"; do
        if [[ -f "$f" ]]; then
            echo "Processing $f"
        else
            echo "$f not found"
        fi
    done
}

files=("file1.txt" "file2.txt")
process_files "${files[@]}"
```

* Functions become **powerful building blocks** when combined with other Bash constructs.


### 9. Tips for Advanced Functions

1. Use `local` for all internal variables.
2. Return **status codes** (`0 = success, non-zero = failure`) for **logical checks**.
3. For values, use `echo` and capture with `$(...)`.
4. Use arrays and loops inside functions for **bulk operations**.
5. Keep functions **small and modular**.
6. Source reusable functions for **large projects**.
