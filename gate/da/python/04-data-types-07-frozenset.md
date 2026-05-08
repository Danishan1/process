# FROZENSET — GATE DA QUICK REVISION

### syntax:

F = frozenset([1,2,3])

### FEATURES:

- immutable version of set
- unordered
- no duplicates
- hashable (unlike set)

### CREATION:

frozenset(iterable)

F = frozenset([1,1,2]) -> {1,2}

### IMMUTABILITY:

F.add(5) -> ERROR
F.remove(1) -> ERROR

### OPERATIONS (ALLOWED):

union -> F | G
intersection -> F & G
difference -> F - G
symmetric ^ -> F ^ G

### EXAMPLE:

A = frozenset([1,2,3])
B = frozenset([3,4])

A | B -> frozenset({1,2,3,4})

### INDEXING:

NOT allowed

### MEMBERSHIP:

1 in F -> True/False

### IMPORTANT PROPERTY:

- can be used as dictionary key
- can be used inside set

### EXAMPLE:

d = {frozenset([1,2]): "value"}

### GATE TRAPS:

1.  frozenset([1,1,2]) -> duplicates removed

2.  F.add(3) -> ERROR (immutable)

3.  {} vs frozenset:
    {} -> dict
    frozenset() -> immutable set

4.  set cannot contain set
    frozenset CAN contain frozenset

### MEMORY IDEA:

- immutable hash-based set
- safe for keys in dict

### QUICK SUMMARY:

frozenset = immutable set
supports set operations
hashable → can be dict key
no modification allowed
