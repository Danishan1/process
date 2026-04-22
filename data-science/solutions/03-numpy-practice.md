I’ll give you **clean, practical NumPy solutions** so you can directly practice and rewrite.

I’ll assume:

```python
import numpy as np
```

# ✅ Phase 1: Basics

### 1. Create 1D and 2D arrays

```python
a = np.array([1, 2, 3, 4])  # 1D

b = np.array([[1, 2, 3],
              [4, 5, 6]])   # 2D
```

### 2. Shape, size, dimensions

```python
print(a.shape)
print(a.size)
print(a.ndim)
```

### 3. Reshape array

```python
arr = np.array([1,2,3,4,5,6])

reshaped = arr.reshape(2, 3)
print(reshaped)
```

### 4. Zeros, ones, identity

```python
np.zeros((3, 3))
np.ones((2, 4))
np.eye(3)
```

### 5. Random numbers

```python
np.random.rand(3, 3)      # 0 to 1
np.random.randint(1, 10, (3, 3))
```

# ✅ Phase 2: Indexing & Slicing

### 1. Access elements

```python
arr = np.array([[1,2,3],
                [4,5,6]])

print(arr[0, 1])   # row 0 col 1
```

### 2. Slice rows & columns

```python
print(arr[:, 1])   # all rows, column 1
print(arr[0, :])    # row 0
```

### 3. Reverse array

```python
arr = np.array([1,2,3,4])

print(arr[::-1])
```

### 4. Sub-matrix

```python
print(arr[0:2, 1:3])
```

### 5. Boolean indexing

```python
arr = np.array([10, 20, 30, 40])

print(arr[arr > 20])
```

# ✅ Phase 3: Operations

### 1. Element-wise operations

```python
a = np.array([1,2,3])
b = np.array([4,5,6])

print(a + b)
print(a * b)
print(a - b)
```

### 2. Matrix multiplication

```python
a = np.array([[1,2],
              [3,4]])

b = np.array([[5,6],
              [7,8]])

print(np.dot(a, b))
```

### 3. Mean, median, std

```python
arr = np.array([1,2,3,4,5])

print(np.mean(arr))
print(np.median(arr))
print(np.std(arr))
```

### 4. Row/column sum

```python
arr = np.array([[1,2,3],
                [4,5,6]])

print(np.sum(arr, axis=0))  # columns
print(np.sum(arr, axis=1))  # rows
```

### 5. Normalize array

```python
arr = np.array([10, 20, 30])

norm = (arr - np.mean(arr)) / np.std(arr)
print(norm)
```

# ✅ Phase 4: Real Data Problems

### 1. Marks analysis

```python
marks = np.array([80, 90, 70, 60])

print(np.mean(marks))
print(np.max(marks))
print(np.min(marks))
```

### 2. Replace negatives with 0

```python
arr = np.array([10, -5, 20, -2])

arr[arr < 0] = 0
print(arr)
```

### 3. Indices where values > threshold

```python
arr = np.array([10, 20, 30, 40])

print(np.where(arr > 25))
```

### 4. Standardize data

```python
arr = np.array([10, 20, 30, 40])

standard = (arr - np.mean(arr)) / np.std(arr)
print(standard)
```

### 5. Combine datasets

```python
a = np.array([1,2,3])
b = np.array([4,5,6])

print(np.concatenate((a, b)))
```

# ✅ Phase 5: Interview-Level Problems

### 1. Find duplicates

```python
arr = np.array([1,2,2,3,3,3,4])

unique, counts = np.unique(arr, return_counts=True)

print(unique[counts > 1])
```

### 2. Sort array (logic way)

```python
arr = np.array([5,2,9,1])

# simple selection sort logic
for i in range(len(arr)):
    for j in range(i+1, len(arr)):
        if arr[i] > arr[j]:
            arr[i], arr[j] = arr[j], arr[i]

print(arr)
```

### 3. Top N values

```python
arr = np.array([10, 50, 20, 40, 30])

N = 3
print(np.sort(arr)[-N:])
```

### 4. Frequency count

```python
arr = np.array([1,2,2,3,3,3])

values, counts = np.unique(arr, return_counts=True)

print(dict(zip(values, counts)))
```

### 5. Rotate array

```python
arr = np.array([1,2,3,4,5])

k = 2
rotated = np.concatenate((arr[-k:], arr[:-k]))

print(rotated)
```
