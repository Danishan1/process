## Python Functions Mapped to Data Types — GATE Quick Table

| Data Type                 | Common Functions / Methods                                                                                                                                      | Key Notes / Trap Points                        |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **int / float / complex** | `int()`, `float()`, `complex()`, `abs()`, `round()`, `pow()`, `min()`, `max()`, `sum()`, `type()`, `id()`                                                       | Basic numeric operations; immutable types      |
| **string (str)**          | `len()`, `ord()`, `chr()`, `upper()`, `lower()`, `strip()`, `replace()`, `split()`, `'sep'.join()`, `isalpha()`, `isdigit()`, `isalnum()`                       | Immutable, supports indexing and slicing       |
| **list**                  | `append()`, `extend()`, `insert()`, `remove()`, `pop()`, `clear()`, `index()`, `count()`, `sort()`, `reverse()`, `sorted()`, `len()`, `min()`, `max()`, `sum()` | Mutable, ordered, allows duplicates            |
| **tuple**                 | `tuple()`, `count()`, `index()`, `len()`, `min()`, `max()`, `sum()`                                                                                             | Immutable, no add/remove methods               |
| **set**                   | `set()`, `add()`, `update()`, `remove()`, `discard()`, `pop()`, `clear()`, `union()`, `intersection()`, `difference()`, `symmetric_difference()`                | Mutable, unordered, no duplicates, no indexing |
| **frozenset**             | `frozenset()`, `union()`, `intersection()`, `difference()`, `symmetric_difference()`                                                                            | Immutable version of set, hashable             |
| **dictionary (dict)**     | `dict()`, `keys()`, `values()`, `items()`, `get()`, `pop()`, `popitem()`, `clear()`, `update()`                                                                 | Key-value mapping, keys must be immutable      |
| **boolean (bool)**        | `bool()`, `all()`, `any()`                                                                                                                                      | False values: `0`, `""`, `[]`, `{}`, `None`    |
| **type conversion**       | `int()`, `float()`, `str()`, `list()`, `tuple()`, `set()`                                                                                                       | Used for type casting between structures       |

### GATE Memory Rule (Quick Recall)

| Type      | Category                          |
| --------- | --------------------------------- |
| Immutable | int, float, str, tuple, frozenset |
| Mutable   | list, set, dict                   |

## LIST METHODS — QUICK REVISION SHEET

### 1. Adding Elements

```
append(x)   -> add at end
extend(it)  -> add multiple elements
insert(i,x) -> add at index
```

### 2. Removing Elements

```
remove(x) -> removes first occurrence (value-based)
pop()     -> removes last element
pop(i)    -> removes index i
clear()   -> empties list
```

### 3. Searching

```
index(x) -> first index of x (error if not found)
count(x) -> number of occurrences
```

### 4. Sorting

```
sort()        -> ascending (in-place)
sort(reverse=True)

sorted(L)     -> returns NEW sorted list
```

### 5. Reversing

```
reverse()     -> in-place reverse
L[::-1]       -> new reversed list
```

### 6. Copying

```
copy()        -> shallow copy
L[:]          -> shallow copy
```

### 7. Basic Info

```
len(L)        -> size
min(L)        -> minimum
max(L)        -> maximum
sum(L)        -> sum of elements
```

### 8. Common Utility Patterns

```
L * n         -> repetition
x in L        -> membership test
```

### 9. Important GATE TRAPS

#### append vs extend

```
L = [1,2]
L.append([3,4])  -> [1,2,[3,4]]
L.extend([3,4])  -> [1,2,3,4]
```

#### remove vs pop

```
remove(x) -> value-based
pop(i)    -> index-based
```

#### copy trap

```
a = [1,2]
b = a        # same reference
b = a.copy() # new list
```

#### sort vs sorted

```
sort()    -> modifies original
sorted()  -> returns new list
```

### 10. One-Line Memory Map

```
ADD:    append / extend / insert
DELETE: remove / pop / clear
FIND:   index / count
ORDER:  sort / reverse
COPY:   copy / [:]
INFO:   len / min / max / sum
```
