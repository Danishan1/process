# LIST — GATE DA QUICK REVISION

### syntax:

L = [1, 2, 3]

### FEATURES:

- ordered
- mutable
- allows duplicates
- indexed
- heterogeneous (mixed types allowed)

### INDEXING:

L[0] -> first element
L[-1] -> last element

### SLICING:

L[1:3] -> sublist (new list)

### ADD ELEMENTS:

append(x) -> add at end
insert(i,x) -> add at index
extend([..])-> add multiple

### REMOVE:

remove(x) -> by value (first match)
pop() -> last element
pop(i) -> index based
del L[i] -> delete

### COMMON OPS:

len(L) -> size
min(L), max(L)
sum(L)

### LOOP:

for i in L:
print(i)

### IMPORTANT PROPERTIES:

- mutable (can change elements)
- dynamic size
- stores references

### COPYING TRAP:

a = [1,2]
b = a
b[0]=100

### a also changes

REAL COPY:
b = a.copy()
b = a[:]

### LIST COMPREHENSION:

[x*x for x in range(5)] -> [0,1,4,9,16]

### GATE TRAPS:

1.  L = [1,2,3]
    L.append([4,5])
    -> [1,2,3,[4,5]]

2.  L.extend([4,5])
    -> [1,2,3,4,5]

3.  L = [1,2,3]
    L\*2 -> [1,2,3,1,2,3]

4.  L.remove(10) -> ERROR if not found

5.  L.pop() on empty list -> ERROR

### MEMORY IDEA:

- list stores references to objects
- mutable → changes reflect in all references

### QUICK SUMMARY:

list = ordered + mutable + indexed
append = single item
extend = multiple items
copy carefully (reference trap)
