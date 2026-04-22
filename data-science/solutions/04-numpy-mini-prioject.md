# Mini Project: Student Performance Analysis (Completed + Enhanced)

We continue from your dataset:

```python
import numpy as np

marks = np.array([
    [85, 78, 92],
    [70, 88, 75],
    [90, 92, 89],
    [60, 65, 70],
    [95, 98, 94]
])
```

# Step 2: Basic Exploration

```python
print("Shape:", marks.shape)
print("Students:", marks.shape[0])
print("Subjects:", marks.shape[1])
```

# Step 3: Student-wise Analysis

```python
total = np.sum(marks, axis=1)
average = np.mean(marks, axis=1)

print("Total marks:", total)
print("Average marks:", average)
```

Optional student labels:

```python
students = np.arange(1, len(marks) + 1)
```

# Step 4: Subject-wise Analysis

```python
subject_avg = np.mean(marks, axis=0)
subject_max = np.max(marks, axis=0)
subject_min = np.min(marks, axis=0)

print("Subject Avg:", subject_avg)
print("Subject Max:", subject_max)
print("Subject Min:", subject_min)
```

# Step 5: Performance Classification

```python
performance = np.where(
    average >= 85, "Excellent",
    np.where(average >= 70, "Good", "Needs Improvement")
)

print(performance)
```

To display clearly per student:

```python
for i in range(len(students)):
    print("Student", students[i], ":", performance[i])
```

# Step 6: Advanced Filtering

## Students scoring > 90 in Math

```python
high_math_students = np.where(marks[:, 0] > 90)[0] + 1
print("Students > 90 in Math:", high_math_students)
```

## Students who failed in any subject

```python
failed_students = np.where(np.any(marks < 70, axis=1))[0] + 1
print("Failed students:", failed_students)
```

## Students who passed all subjects

```python
passed_all = np.where(np.all(marks >= 70, axis=1))[0] + 1
print("Passed all subjects:", passed_all)
```

# Step 7: Normalization (Standardization)

Correct approach is column-wise:

```python
normalized = (marks - np.mean(marks, axis=0)) / np.std(marks, axis=0)

print(normalized)
```

# Step 8: Add Bonus Marks

```python
updated_marks = marks + 5
print(updated_marks)
```

# Step 9: Ranking Students

```python
rank_order = np.argsort(total)[::-1]

print("Ranking (student index):", rank_order + 1)
```

Detailed ranking output:

```python
for rank, idx in enumerate(rank_order, start=1):
    print("Rank", rank, ": Student", idx + 1, ", Total =", total[idx])
```

# Step 10: Top 3 Performers

```python
top_3 = rank_order[:3]

for i in top_3:
    print("Student", i + 1, "Total:", total[i])
```

# Step 11: Subject Topper

```python
subject_toppers = np.argmax(marks, axis=0)

subjects = ["Math", "Science", "English"]

for i in range(len(subjects)):
    print(subjects[i], "Topper: Student", subject_toppers[i] + 1)
```

# Step 12: Overall Insights

## Best student

```python
best_student = np.argmax(total) + 1
print("Best Student:", best_student)
```

## Worst student

```python
worst_student = np.argmin(total) + 1
print("Worst Student:", worst_student)
```

# What You Should Learn From This

You practiced:

- Array aggregation using axis
- Filtering using boolean conditions
- Ranking using argsort
- Standardization (very important for ML)
- Real-world style analysis logic
