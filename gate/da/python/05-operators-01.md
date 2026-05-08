### OPERATORS — GATE DA QUICK REVISION

- Python uses unicode to represent each character instead of ASCII

### 1. ARITHMETIC

- addition

* subtraction

- multiplication
  / true division (float)
  / / floor division
  % modulus
  \*\* exponent

### TRAPS:

5/2 -> 2.5
5//2 -> 2
-5//2 -> -3 (floor)
5%2 -> 1

### 2. COMPARISON

== equal value
!= not equal

> greater
> < less
> = >=
> <= <=

### returns bool

### 3. LOGICAL

and -> both True
or -> any True
not -> reverse

### TRUTH TABLE IDEA:

0, "", [], None -> False
others -> True

### SHORT-CIRCUIT:

A and B -> if A False, B not checked
A or B -> if A True, B not checked

### 4. ASSIGNMENT

=
+= -= \*= /= //= %= \*\*=

### example:

x = 5
x += 2 -> 7

### 5. BITWISE

& AND
| OR
^ XOR
~ NOT
<< left shift

> > right shift

### TRAPS:

5 & 3 -> bitwise AND
~5 -> -(5+1) = -6

### 6. IDENTITY

is
is not

### checks memory address (id)

a = [1,2]
b = [1,2]
a == b -> True
a is b -> False

### 7. MEMBERSHIP

in
not in

### example:

'a' in "cat" -> True
2 in [1,2,3] -> True

### 8. PRECEDENCE (IMPORTANT)

highest → lowest

()
**
+x, -x, ~x
*, /, //, %
+, -
<<, >>
&
^
|
==, !=, >, <
not
and
or

### 9. IMPORTANT GATE TRAPS

1.  True + True -> 2
    False + True -> 1

2.  0 and 5 -> 0
    1 or 5 -> 1

3.  5 < 3 < 2 -> False (chained comparison)

4.  not 5 -> False (5 is True)

5.  5 == 5.0 -> True (type ignored)

### 10. QUICK SUMMARY

ARITHMETIC -> math ops
LOGICAL -> boolean ops
BITWISE -> binary ops
ASSIGNMENT -> update value
IDENTITY -> memory check (is)
MEMBERSHIP -> in/not in
