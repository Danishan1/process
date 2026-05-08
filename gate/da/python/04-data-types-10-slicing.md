
### SLICING (GATE DA QUICK REVISION)

### syntax:
s[start : stop : step]

### RULES:
- start included
- stop excluded
- step default = 1
- negative index allowed

### STRING EXAMPLE:
s = "abcdef"

s[1:4]     -> "bcd"
s[:3]      -> "abc"
s[2:]      -> "cdef"
s[:]       -> full string

### STEP EXAMPLE:
s[0:6:2]   -> "ace"
s[::2]     -> "ace"
s[::-1]    -> reverse -> "fedcba"

### NEGATIVE INDEX:
 a b c d e f
 0 1 2 3 4 5
-6-5-4-3-2-1

s[-3:]     -> "def"
s[:-2]     -> "abcd"

### REVERSE TRICK:
s[::-1]    -> reverse string/list

### LIST SLICING SAME RULE:
L = [1,2,3,4,5]

L[1:4]     -> [2,3,4]
L[::-1]    -> reverse list

### IMPORTANT GATE TRAPS:

s[::]      -> full copy
s[1:100]   -> no error, stops at end
s[5:1]     -> empty (unless negative step)
s[::0]     -> ERROR
s[::-2]    -> reverse with step 2

- `s[::]` → returns a full copy of the sequence when no start, stop, or step is given.
- `s[1:100]` → returns elements from index 1 to end when stop exceeds length (no error).
- `s[5:1]` → returns empty sequence when slicing forward but start > stop.
- `s[::0]` → raises `ValueError` because step cannot be zero.
- `s[::-1]` → returns the entire sequence in reverse order when step is negative.
- `s[::-2]` → returns reversed sequence while skipping every second element.
- `s[3:3]` → returns empty sequence when start equals stop.
- `s[-100:100]` → safely adjusts out-of-range indices and returns valid slice.
- `s[-1]`, `s[-n]` → accesses elements from the end using negative indexing.
- `s[2:8:-1]` → returns empty sequence due to mismatch between positive range and negative step.


### MEMORY IDEA:
- slicing creates new object (new string/list)

### QUICK SUMMARY:
start included, stop excluded, step optional
[::-1] = reverse
