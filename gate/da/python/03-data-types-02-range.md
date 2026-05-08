### RANGE() — GATE DA QUICK REVISION

### syntax:

range(start, stop, step)

### RULES:

- start included
- stop excluded
- step default = 1

### CASES:

range(stop)
range(5) -> 0 1 2 3 4

range(start, stop)
range(2,6) -> 2 3 4 5

range(start, stop, step)
range(1,10,2) -> 1 3 5 7 9

### NEGATIVE STEP:

range(10,0,-2) -> 10 8 6 4 2

### IMPORTANT PROPERTIES:

- immutable sequence
- memory efficient (lazy evaluation)
- used in loops

### LOOP USE:

for i in range(5):
print(i)

### OUTPUT:

0 1 2 3 4

### COMMON TRAPS:

1.  range(5,1)
    -> empty (step missing, default +1 cannot go backwards)

2.  range(5,5)
    -> empty

3.  range(1,10,0)
    -> ERROR (step cannot be 0)

4.  range(-1,-5,-1)
    -> -1 -2 -3 -4

### INDEXING:

r = range(10)
r[0] -> 0
r[-1] -> 9

### SLICING WORKS:

list(range(10))[::2] -> [0,2,4,6,8]

### MEMORY IDEA:

range does NOT store full list
stores formula (start, stop, step)

### QUICK SUMMARY:

range = lazy sequence generator
start included, stop excluded
step controls direction
step ≠ 0
range() is an immutable, memory-efficient sequence that generates values lazily and supports indexing and slicing but is not a list.
