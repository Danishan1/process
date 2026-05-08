# SET — GATE DA QUICK REVISION

### syntax:

S = {1, 2, 3}

### FEATURES:

- unordered
- mutable (can add/remove)
- no duplicates
- no indexing

### EMPTY SET:

S = set() # NOT {}

### EMPTY {} -> dict

### DUPLICATES AUTO REMOVED:

{1,1,2,2} -> {1,2}

### ADD ELEMENT:

S.add(5)

### ADD MULTIPLE:

S.update([6,7])

### REMOVE:

S.remove(5) -> ERROR if not present
S.discard(5) -> no error

pop() -> removes random element

### OPERATIONS:

union (|) -> A | B
intersection (&) -> A & B
difference (-) -> A - B
symmetric diff (^) -> A ^ B

### EXAMPLE:

A = {1,2,3}
B = {3,4}

A | B -> {1,2,3,4}
A & B -> {3}
A - B -> {1,2}
A ^ B -> {1,2,4}

### LOOP:

for i in S:
print(i)

### GATE TRAPS:

1.  S = {}
    -> dict, NOT set

2.  S = set([1,1,2])
    -> {1,2}

3.  S.remove(10) -> ERROR

4.  S.discard(10) -> SAFE

5.  No indexing:
    S[0] -> ERROR

### MEMORY IDEA:

- implemented using hash table
- elements must be hashable (immutable)

### HASHABLE TYPES:

valid -> int, str, tuple
invalid -> list, set, dict

### QUICK SUMMARY:

set = unordered + unique + hash-based
fast membership test (O(1) avg)
use for removing duplicates + set operations
