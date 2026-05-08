### DICTIONARY — GATE DA QUICK REVISION

### syntax:

D = {"a":1, "b":2}

### FEATURES:

- key:value pair
- keys must be unique
- values can repeat
- mutable
- insertion ordered (Python 3.7+)

### KEYS RULES:

- immutable only (int, str, tuple, bool)
- list/set NOT allowed as key

### VALUES:

- any type allowed

### ACCESS:

D["a"] -> 1

### GET METHOD:

D.get("a") -> 1
D.get("x") -> None (no error)

### ADD / UPDATE:

D["c"] = 3

### DELETE:

del D["a"]
D.pop("b")

### COMMON METHODS:

D.keys() -> all keys
D.values() -> all values
D.items() -> (key,value) pairs

### LOOP:

for k in D:
print(k, D[k])

### or:

for k,v in D.items():
print(k,v)

### IMPORTANT PROPERTIES:

- unordered (conceptually), but insertion ordered in modern Python
- fast lookup (O(1) average)

### GATE TRAPS:

1.  {"a":1, "a":2}
    -> {'a':2} (last value wins)

2.  D["x"] -> ERROR if key not present

3.  D.get("x") -> safe (returns None)

4.  mutable values allowed:
    {"a":[1,2]}

5.  key uniqueness only, not value uniqueness

### MEMORY IDEA:

- hash table based structure
- key → hash → index → value

### QUICK SUMMARY:

dict = key-value store
keys unique + immutable
values any type
fast lookup
