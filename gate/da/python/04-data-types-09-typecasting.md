### TYPECASTING — GATE DA QUICK REVISION

### 1. WHAT IS TYPECASTING?

### converting one data type to another

### 2. TYPES

### (A) IMPLICIT (AUTO)

x = 5 + 2.5 ### int → float automatically

### result: 7.5

### RULE:

### smaller → larger type (safe)

int → float → complex

### (B) EXPLICIT (MANUAL)

int()
float()
str()
list()
tuple()
set()

### 3. INT CONVERSION

int(3.7) -> 3
int("10") -> 10
int(True) -> 1
int(False) -> 0

### ERROR:

int("10.5") -> ValueError

### 4. FLOAT CONVERSION

float(5) -> 5.0
float("3.5") -> 3.5
float(True) -> 1.0

### 5. STR CONVERSION

str(10) -> "10"
str([1,2]) -> "[1, 2]"

### 6. LIST CONVERSION

list("abc") -> ['a','b','c']
list((1,2)) -> [1,2]
list({1,2}) -> unordered list

### 7. TUPLE CONVERSION

tuple([1,2]) -> (1,2)
tuple("abc") -> ('a','b','c')

### 8. SET CONVERSION

set([1,1,2]) -> {1,2}
set("aba") -> {'a','b'}

### removes duplicates

### 9. BOOLEAN CONVERSION

bool(0) -> False
bool("") -> False
bool([]) -> False
bool({}) -> False
bool(None) -> False

### others -> True

### 10. COMPLEX TYPE

complex(2) -> 2+0j
complex(2,3) -> 2+3j
complex("2+3j")-> 2+3j

### 11. IMPORTANT TRAPS (GATE)

1.  int("3.5") -> ERROR

2.  list(10) -> ERROR (non-iterable)

3.  set([1,1,1]) -> {1}

4.  bool("0") -> True (non-empty string)

5.  float("10") -> 10.0

### 12. QUICK MEMORY MAP

IMPLICIT:
int → float → complex

EXPLICIT:
int(), float(), str(),
list(), tuple(), set(), bool()

### 13. CORE IDEA

### typecasting = converting data types safely or manually

### may lose data (float → int truncation)
