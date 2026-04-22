Great set of problems — this is basically a solid Python fundamentals track. I’ll give you **clean, beginner-friendly solutions** for each level. Try reading, understanding, then re-writing them without looking.

# ✅ Level 1: Basics (Loops, Conditions)

### 1. Print numbers from 1 to N

```python
n = int(input())
for i in range(1, n + 1):
    print(i)
```

### 2. Print even numbers between 1 and N

```python
n = int(input())
for i in range(1, n + 1):
    if i % 2 == 0:
        print(i)
```

### 3. Sum of first N natural numbers

```python
n = int(input())
total = 0

for i in range(1, n + 1):
    total += i

print(total)
```

### 4. Even or Odd

```python
n = int(input())

if n % 2 == 0:
    print("Even")
else:
    print("Odd")
```

### 5. Multiplication table

```python
n = int(input())

for i in range(1, 11):
    print(n * i)
```

### 6. Reverse a number

```python
n = int(input())
rev = 0

while n > 0:
    digit = n % 10
    rev = rev * 10 + digit
    n //= 10

print(rev)
```

### 7. Count digits

```python
n = int(input())
count = 0

if n == 0:
    count = 1
else:
    while n > 0:
        count += 1
        n //= 10

print(count)
```

# ✅ Level 2: Strings

### 1. Palindrome check

```python
s = input()

if s == s[::-1]:
    print("Palindrome")
else:
    print("Not Palindrome")
```

### 2. Count vowels and consonants

```python
s = input().lower()
vowels = "aeiou"

v = c = 0

for ch in s:
    if ch.isalpha():
        if ch in vowels:
            v += 1
        else:
            c += 1

print("Vowels:", v)
print("Consonants:", c)
```

### 3. Frequency of each character

```python
s = input()
freq = {}

for ch in s:
    freq[ch] = freq.get(ch, 0) + 1

print(freq)
```

### 4. Remove duplicates

```python
s = input()
result = ""

for ch in s:
    if ch not in result:
        result += ch

print(result)
```

### 5. Longest word in sentence

```python
sentence = input()
words = sentence.split()

longest = max(words, key=len)
print(longest)
```

### 6. Title case

```python
s = input()
print(s.title())
```

### 7. Count words

```python
s = input()
words = s.split()
print(len(words))
```

# ✅ Level 3: Lists

### 1. Largest and smallest

```python
lst = list(map(int, input().split()))

print(max(lst), min(lst))
```

### 2. Remove duplicates

```python
lst = list(map(int, input().split()))
result = []

for x in lst:
    if x not in result:
        result.append(x)

print(result)
```

### 3. Second largest

```python
lst = list(set(map(int, input().split())))
lst.sort()

print(lst[-2])
```

### 4. Reverse list

```python
lst = list(map(int, input().split()))
print(lst[::-1])
```

### 5. Merge lists

```python
a = [1, 2, 3]
b = [4, 5, 6]

print(a + b)
```

### 6. Common elements

```python
a = list(map(int, input().split()))
b = list(map(int, input().split()))

common = []

for x in a:
    if x in b:
        common.append(x)

print(common)
```

### 7. Frequency in list

```python
lst = list(map(int, input().split()))
freq = {}

for x in lst:
    freq[x] = freq.get(x, 0) + 1

print(freq)
```

# ✅ Level 4: Dictionaries

### 1. Frequency (same as above)

```python
lst = [1,2,2,3,3,3]
freq = {}

for x in lst:
    freq[x] = freq.get(x, 0) + 1

print(freq)
```

### 2. Merge dictionaries

```python
a = {"a": 1, "b": 2}
b = {"c": 3, "d": 4}

a.update(b)
print(a)
```

### 3. Max value key

```python
d = {"a": 10, "b": 30, "c": 20}

print(max(d, key=d.get))
```

### 4. Invert dictionary

```python
d = {"a": 1, "b": 2}
inv = {}

for k, v in d.items():
    inv[v] = k

print(inv)
```

### 5. Group by category

```python
students = [("A", "A"), ("B", "B"), ("C", "A")]

group = {}

for name, grade in students:
    group.setdefault(grade, []).append(name)

print(group)
```

# ✅ Level 5: Functions

### 1. Factorial

```python
def factorial(n):
    result = 1
    for i in range(1, n + 1):
        result *= i
    return result

print(factorial(5))
```

### 2. Fibonacci

```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        print(a, end=" ")
        a, b = b, a + b

fibonacci(10)
```

### 3. Prime check

```python
def is_prime(n):
    if n < 2:
        return False

    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True
```

### 4. Prime in range

```python
def primes_in_range(a, b):
    for i in range(a, b + 1):
        if is_prime(i):
            print(i)

primes_in_range(10, 50)
```

### 5. Flatten list

```python
def flatten(lst):
    result = []
    for item in lst:
        if isinstance(item, list):
            result.extend(flatten(item))
        else:
            result.append(item)
    return result

print(flatten([1, [2, [3, 4]], 5]))
```

# ✅ Level 6: Practical Problems

### 1. Total revenue

```python
sales = [100, 200, 300]
print(sum(sales))
```

### 2. Most frequent item

```python
lst = [1,2,2,3,3,3]
freq = {}

for x in lst:
    freq[x] = freq.get(x, 0) + 1

print(max(freq, key=freq.get))
```

### 3. Grades

```python
marks = [80, 60, 40]

for m in marks:
    avg = m
    if avg >= 75:
        print("A")
    elif avg >= 50:
        print("B")
    else:
        print("C")
```

### 4. Filter invalid values

```python
lst = [10, -5, 20, None, 30]

clean = []

for x in lst:
    if isinstance(x, int) and x >= 0:
        clean.append(x)

print(clean)
```

### 5. Word frequency

```python
text = "this is this python is easy"
words = text.split()

freq = {}

for w in words:
    freq[w] = freq.get(w, 0) + 1

print(freq)
```
