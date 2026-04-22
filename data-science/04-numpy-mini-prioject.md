# Mini Project: Student Performance Analysis (Using NumPy Only)

## Problem Statement

You are given marks of students in 3 subjects:

- Math
- Science
- English

You need to analyze performance and extract insights using NumPy.

## Step 1: Create Dataset

```python
import numpy as np

# Rows = students, Columns = subjects
marks = np.array([
    [85, 78, 92],
    [70, 88, 75],
    [90, 92, 89],
    [60, 65, 70],
    [95, 98, 94]
])
```

## Step 2: Basic Exploration

Practice:

- Shape of dataset
- Number of students
- Number of subjects

```python
print("Shape:", marks.shape)
```

## Step 3: Student-wise Analysis

### Tasks:

1. Total marks per student
2. Average marks per student

```python
total = np.sum(marks, axis=1)
average = np.mean(marks, axis=1)

print("Total:", total)
print("Average:", average)
```

## Step 4: Subject-wise Analysis

### Tasks:

1. Average marks per subject
2. Highest score in each subject
3. Lowest score in each subject

```python
subject_avg = np.mean(marks, axis=0)
subject_max = np.max(marks, axis=0)
subject_min = np.min(marks, axis=0)
```

## Step 5: Performance Classification

### Task:

Classify students:

- Average ≥ 85 → "Excellent"
- 70–84 → "Good"
- < 70 → "Needs Improvement"

```python
performance = np.where(average >= 85, "Excellent",
              np.where(average >= 70, "Good", "Needs Improvement"))

print(performance)
```

## Step 6: Advanced Filtering

### Tasks:

1. Find students scoring above 90 in Math
2. Find students who failed (< 70 in any subject)

```python
high_math = marks[:, 0] > 90
failed = np.any(marks < 70, axis=1)
```

## Step 7: Normalize Data

(Standardization: mean = 0, std = 1)

```python
normalized = (marks - np.mean(marks)) / np.std(marks)
```

## Step 8: Add Bonus Marks

Add 5 marks to all students using vectorization:

```python
updated_marks = marks + 5
```

# What You Learn From This

- Array operations
- Axis-based calculations
- Filtering and conditions
- Vectorization (no loops)

This is exactly how raw data is processed before moving to pandas.

# Make It Stronger (Important)

Don’t stop at copying.

Extend the project:

- Add more students
- Add more subjects
- Rank students based on total marks
- Find top 3 performers

# Expected Outcome

After this, you should be able to:

- Think in terms of arrays
- Avoid loops
- Perform analysis using NumPy
