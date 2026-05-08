## DATA TYPES — GATE DA QUICK REVISION

## 1. BASIC TYPES

int -> 10, -5
float -> 3.14, 2.0
complex -> 2+3j
bool -> True, False
str -> "AI"

type(x) -> returns data type

## 2. NONE TYPE

None -> absence of value

x = None

## Terms

- Mutable: Objects whose values can be changed after creation.
- Immutable: Objects whose values cannot be changed after creation.
- Ordered: Data types that preserve the insertion order of elements.
- Unordered: Data types that do not guarantee any specific order of elements.
- Indexed: Data types whose elements can be accessed using position (index).
- Iterable: Objects that can be traversed one element at a time using loops.
- Homogeneous: Data structures that store elements of the same data type.
- Heterogeneous: Data structures that can store elements of different data types.
- Slicing: The process of accessing a portion (subset) of a sequence using a range of indices.
- Hashable: Objects that have a fixed hash value and can be used as dictionary keys or set elements.
- Data Structure Type: it refers to the category of a data type based on how it stores, organizes, and allows access to data.

## 3. COLLECTION TYPES

list -> [1,2,3] ## mutable
tuple -> (1,2,3) ## immutable
set -> {1,2,3} ## unordered, unique
dict -> {"a":1,"b":2} ## key-value, mutable

## 4. MUTABILITY

immutable:
int, float, bool, str, tuple, complex

mutable:
list, set, dict

## 5. STRING

s = "hello"

> immutable, indexed, ordered, iterable, homogeneous (only characters), supports slicing → s[1:4], stores Unicode characters

## 6. LIST

L = [1,2,3]

> mutable, ordered, indexed, allows duplicates, heterogeneous, supports slicing → L[0:2], dynamic size, iterable

## 7. TUPLE

T = (1,2,3)

> immutable, ordered, indexed, allows duplicates, heterogeneous, supports slicing → T[1:], faster than list, hashable if all elements are immutable

- Tuple is immutable ≠ its contents are immutable

## 8. SET

S = {1,2,3}

> mutable, unordered, no indexing, no slicing, no duplicates, heterogeneous, iterable, elements must be hashable, uses hashing for fast lookup

- Set is muable but always contains immutable objects or it is mixed types allowed, but only unchangeable (hashable) items allowed
- A set is a heterogeneous collection that stores only immutable (hashable) objects and does not allow duplicates.
- What CAN be inside a set: Numbers, Strings, Tuples (only if elements inside are immutable),

## FROZENSET

FS = frozenset([1, "a", (2, 3)])

> immutable, unordered, no indexing, no slicing, no duplicates, heterogeneous, iterable, elements must be hashable, uses hashing for fast lookup

- Set is muable but always contains immutable objects or it is mixed types allowed, but only unchangeable (hashable) items allowed
- A set is a heterogeneous collection that stores only immutable (hashable) objects and does not allow duplicates.
- What CAN be inside a set: Numbers, Strings, Tuples (only if elements inside are immutable),

## 9. DICTIONARY

D = {"a":1,"b":2}

> mutable, key-value pairs, ordered (Python 3.7+), keys unique, values can repeat, keys must be immutable (hashable), no indexing (key-based access), iterable (over keys), heterogeneous

## range()

range(start, stop, step)
range(1,10,2) -> 1 3 5 7 9

> it is an immutable, memory-efficient sequence that generates values lazily and supports indexing and slicing but is not a list.

## 10. TYPE CONVERSION

int("10") -> 10
float("3.5")-> 3.5
str(10) -> "10"

list("abc") -> ['a','b','c']

## 11. IMPORTANT FUNCTIONS

len() -> size
type() -> type
id() -> memory address

## 12. GATE TRAPS

x=10
y=10
x is y -> may be True (small integers cached)

a=[1,2]
b=[1,2]
a==b -> True
a is b -> False

Python caches small integers: Typically integers from -5 to 256 are cached (Just of ducmentation and exam purpose)
10 is 10 :True
-5 is -5 :True
1000 is 1000 :Usually False (not guaranteed)

## 13. BOOLEAN RULES

bool(0) -> False
bool("") -> False
bool([]) -> False
bool(None) -> False
everything else -> True

## 14. MEMORY IDEA

variable -> reference to object (not object itself)

### Simple Summary Table

| Data Structure Type | Meaning                     | Examples            |
| ------------------- | --------------------------- | ------------------- |
| Sequence            | Linear ordered collection   | list, tuple, string |
| Set                 | Unordered unique collection | set                 |
| Mapping             | Key-value storage           | dictionary          |

### Python Data Types Comparison Table

| Data Type | Mutable?  | Ordered?              | Allows Duplicates? | Indexed?  | Syntax Example | Common Use          |
| --------- | --------- | --------------------- | ------------------ | --------- | -------------- | ------------------- |
| **int**   | Immutable | —                     | —                  | No        | `x = 10`       | Whole numbers       |
| **float** | Immutable | —                     | —                  | No        | `x = 10.5`     | Decimal numbers     |
| **str**   | Immutable | Yes                   | Yes                | Yes       | `"hello"`      | Text data           |
| **list**  | Mutable   | Yes                   | Yes                | Yes       | `[1, 2, 3]`    | Collection of items |
| **tuple** | Immutable | Yes                   | Yes                | Yes       | `(1, 2, 3)`    | Fixed data          |
| **set**   | Mutable   | No                    | No                 | No        | `{1, 2, 3}`    | Unique items        |
| **dict**  | Mutable   | Ordered (Python 3.7+) | Keys , Values      | Key-based | `{"a": 1}`     | Key-value mapping   |
| **bool**  | Immutable | —                     | —                  | No        | `True/False`   | Conditions          |

### Python Data Types Comparison Table

| Feature                    | String           | List     | Tuple                         | Set      | Dictionary            |
| -------------------------- | ---------------- | -------- | ----------------------------- | -------- | --------------------- |
| Mutable                    | No               | Yes      | No                            | Yes      | Yes                   |
| Ordered                    | Yes              | Yes      | Yes                           | No       | Yes (Python 3.7+)     |
| Indexed                    | Yes              | Yes      | Yes                           | No       | No (key-based access) |
| Allows Duplicates          | Yes              | Yes      | Yes                           | No       | Keys: No, Values: Yes |
| Heterogeneous              | No (homogeneous) | Yes      | Yes                           | Yes      | Yes                   |
| Slicing Support            | Yes              | Yes      | Yes                           | No       | No                    |
| Iterable                   | Yes              | Yes      | Yes                           | Yes      | Yes                   |
| Data Structure Type        | Sequence         | Sequence | Sequence                      | Set      | Mapping               |
| Access Method              | Index            | Index    | Index                         | No index | Key-based             |
| Hashable Elements Required | No               | No       | Yes (if used in set/dict key) | Yes      | Keys must be hashable |

### Quick Revision Insight

### Quick Insights

- Sequence types → String, List, Tuple (support indexing + slicing)
- Set → Unordered unique collection
- Dictionary → Key-value mapping (key-based access only)

* **Mutable types** (list, dict, set) → can change after creation
* **Immutable types** (int, float, str, tuple) → cannot be changed
* **Ordered types** → keep insertion order (like list, tuple, str, dict)
* **Unordered types** → set

### Example difference (list vs tuple)

```python
list_example = [1, 2, 3]
list_example[0] = 100   ## works

tuple_example = (1, 2, 3)
tuple_example[0] = 100  ## error
```
