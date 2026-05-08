# BITWISE OPERATIONS — GATE DA QUICK REVISION

### WHAT ARE BITWISE OPS:
- operations on binary bits (0,1)

### OPERATORS:

&   -> AND
|   -> OR
^   -> XOR
~   -> NOT
<<  -> left shift
>>  -> right shift

### BINARY IDEA:

5 = 0101
3 = 0011

### BITWISE AND (&):

1 if both bits are 1

5 & 3 ->
0101
0011
----
0001 -> 1

### BITWISE OR (|):

1 if any bit is 1

5 | 3 ->
0101
0011
----
0111 -> 7

### BITWISE XOR (^):

1 if bits are different

5 ^ 3 ->
0101
0011
----
0110 -> 6

### BITWISE NOT (~):

~x = -(x + 1)

~5 -> -6
~0 -> -1

### LEFT SHIFT (<<):

x << n = x * (2^n)

5 << 1 -> 10
5 << 2 -> 20

### RIGHT SHIFT (>>):

x >> n = x // (2^n)

5 >> 1 -> 2
5 >> 2 -> 1

### QUICK SHORTCUTS:

x << 1 -> x * 2
x >> 1 -> x // 2

### GATE TRAPS:

1. 5 & 3 ≠ logical AND (&& is not Python)

2. ~x = -(x+1) always

3. bitwise works on binary form, not decimal logic

4. shifts depend on integer division

5. negative numbers use 2’s complement internally

### PRECEDENCE (IMPORTANT):

~  highest
<< >>
&
^
|

### QUICK SUMMARY:

&  -> common bits
|  -> any bit
^  -> different bits
~  -> -(x+1)
<< -> multiply by 2^n
>> -> divide by 2^n