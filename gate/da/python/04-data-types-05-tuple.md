# TUPLE — GATE DA QUICK REVISION

### syntax:

T = (1, 2, 3)

### FEATURES:

- ordered
- immutable
- allows duplicates
- indexed
- faster than list

### SINGLE ELEMENT TUPLE:

T = (5,) # comma mandatory
T = (5) # int, NOT tuple

### INDEXING:

T[0] -> first element
T[-1] -> last element

### SLICING:

T[1:3] -> new tuple

### IMMUTABILITY:

T[0] = 10 -> ERROR

### OPERATIONS:

len(T)
min(T), max(T), sum(T)

### LOOP:

for i in T:
print(i)

### PACKING & UNPACKING:

a = 1,2,3 # packing
x,y,z = a # unpacking

### SWAP USING TUPLE:

a,b = b,a

### CONCATENATION:

(1,2) + (3,4) -> (1,2,3,4)

### REPETITION:

(1,2)\*2 -> (1,2,1,2)

### GATE TRAPS:

1.  (5) -> int, not tuple
    (5,) -> tuple

2.  T[0] = 10 -> ERROR (immutable)

3.  T += (4,) -> creates NEW tuple (not modification)

4.  Tuple with mutable object:
    (1, [2,3]) -> list inside can change

### MEMORY IDEA:

- immutable sequence
- fixed structure
- faster than list (less overhead)

### QUICK SUMMARY:

tuple = ordered + immutable + indexed
use for fixed data + fast access
