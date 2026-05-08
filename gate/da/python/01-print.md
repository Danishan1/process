# Summary

```txt
print(*obj, sep=' ', end='\n', file=sys.stdout, flush=False)

sep -> between objects
end -> after output
default sep=' '
default end='\n'

print(1,2,3)          -> 1 2 3
print(1,2,sep='-')    -> 1-2
print("A",end='')     -> no newline

Escapes:
\n newline
\t tab
\\ backslash
\" double quote
\' single quote

print() returns None

print(print("Hi"))
# Hi
# None

print() -> blank line

"A"*3 -> AAA

Formatting:
f"{x}"          # most important
"{}".format(x)
"%d"%x

print vs return:
print -> display
return -> send value back

print("Hi", file=f)   # write to file
print("Hi", flush=True)

Important patterns:
print("A","B",sep='')      -> AB
print("A",end='');print("B")-> AB
print(1,2,3,end='');print(4)-> 1 2 34

Complexity: O(size of output)
```



# 1. Basic Syntax of `print()`

```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

### Parameters

| Parameter  | Meaning                   |
| ---------- | ------------------------- |
| `*objects` | Values to display         |
| `sep`      | Separator between objects |
| `end`      | What to print at the end  |
| `file`     | Output destination        |
| `flush`    | Force immediate output    |

# 2. Default Behavior

```python
print("Hello")
```

Output:

```python
Hello
```

Equivalent to:

```python
print("Hello", end="\n")
```

# 3. Multiple Arguments

```python
print(10, 20, 30)
```

Output:

```python
10 20 30
```

Default separator is space.

# 4. `sep` Parameter (Very Important for MCQs)

```python
print(1, 2, 3, sep='-')
```

Output:

```python
1-2-3
```

Another example:

```python
print("2026", "05", "08", sep="/")
```

Output:

```python
2026/05/08
```

### GATE-style Question

```python
print("A", "B", "C", sep="***")
```

Output?

```python
A***B***C
```

# 5. `end` Parameter

Controls what happens after printing.

```python
print("Hello", end=" ")
print("World")
```

Output:

```python
Hello World
```

Without `end=" "`:

```python
Hello
World
```

# 6. Combining `sep` and `end`

```python
print(1, 2, 3, sep=':', end=' DONE')
```

Output:

```python
1:2:3 DONE
```

# 7. Escape Sequences (Frequently Asked)

| Escape | Meaning      |
| ------ | ------------ |
| `\n`   | New line     |
| `\t`   | Tab          |
| `\\`   | Backslash    |
| `\'`   | Single quote |
| `\"`   | Double quote |

Example:

```python
print("A\nB")
```

Output:

```python
A
B
```

# 8. Printing Quotes

```python
print("He said \"Hello\"")
```

Output:

```python
He said "Hello"
```

# 9. String Formatting Approaches

## (A) Old `%` Formatting

```python
x = 10
print("Value = %d" % x)
```

## (B) `format()` Method

```python
x = 10
print("Value = {}".format(x))
```

## (C) f-Strings (Most Important Modern Approach)

```python
x = 10
print(f"Value = {x}")
```

Output:

```python
Value = 10
```

# 10. Printing Variables and Expressions

```python
a = 5
b = 2

print(a + b)
```

Output:

```python
7
```

# 11. Type Conversion in `print()`

`print()` automatically converts objects to strings internally.

```python
print(10)
print(3.14)
print(True)
```

# 12. Printing Data Structures

```python
print([1,2,3])
print((1,2))
print({"a":1})
```

Output:

```python
[1, 2, 3]
(1, 2)
{'a': 1}
```

# 13. Important Concept: `print()` Returns `None`

Very common conceptual question.

```python
x = print("Hello")
print(x)
```

Output:

```python
Hello
None
```

Because `print()` performs output but returns nothing.

# 14. Difference Between `print()` and `return`

| `print()`                 | `return`             |
| ------------------------- | -------------------- |
| Displays output           | Sends value back     |
| Used for debugging/output | Used in functions    |
| Returns `None`            | Returns actual value |

Example:

```python
def f():
    print(5)

x = f()
print(x)
```

Output:

```python
5
None
```

# 15. Output Prediction Questions (Very Important for GATE)

## Example 1

```python
print("A", end="")
print("B")
```

Output:

```python
AB
```

## Example 2

```python
print("X", "Y", sep="", end="Z")
```

Output:

```python
XYZ
```

## Example 3

```python
print(print("Hi"))
```

Execution:

- inner `print("Hi")` prints `Hi`
- inner returns `None`
- outer prints `None`

Output:

```python
Hi
None
```

# 16. File Output

```python
f = open("a.txt", "w")
print("Hello", file=f)
f.close()
```

Writes into file instead of terminal.

# 17. `flush=True`

Useful in real-time systems.

```python
print("Loading...", flush=True)
```

Forces immediate display.

# 18. Time Complexity Perspective

For GATE DA:

- `print()` complexity depends on output size.
- Printing very large data structures is expensive.
- In competitive coding, excessive printing can cause TLE.

Approximate:

```text
O(length of output)
```

# 19. Tricky GATE-Level MCQs

## Q1

```python
print("A", "B", sep=None)
```

Output:

```python
A B
```

Because `None` means default separator.

## Q2

```python
print()
```

Output:

(blank line)

## Q3

```python
print("A"*3)
```

Output:

```python
AAA
```

## Q4

```python
print(1, 2, 3, end='')
print(4, 5)
```

Output:

```python
1 2 34 5
```

# 20. Most Important GATE Takeaways

You should remember:

### Core Concepts

- `sep`
- `end`
- escape sequences
- `print()` returns `None`
- output prediction
- formatting methods
- string repetition

### Frequently Asked Patterns

- nested `print()`
- `end=''`
- `sep=''`
- newline behavior
- mixing data types

# 21. Mini Practice Set

Predict outputs:

## (1)

```python
print("GATE", "DA", sep="-", end="!")
```

## (2)

```python
x = print("AI")
print(type(x))
```

## (3)

```python
print("A\nB\tC")
```

Try these yourself first.

# 22. Expected Answers

## (1)

```python
GATE-DA!
```

## (2)

```python
AI
<class 'NoneType'>
```

## (3)

```python
A
B    C
```
