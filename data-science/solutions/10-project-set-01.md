Now I’ll help you in the most useful way: instead of just restating it, I’ll turn it into a **practical execution blueprint + coding workflow template** you can reuse for all 5 projects.

# MASTER TEMPLATE (Use for ALL Projects)

Every project should follow this structure:

```python id="template1"
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("file.csv")
```

# 1. DATA UNDERSTANDING (Always start here)

```python id="step1"
df.head()
df.info()
df.describe()
df.isnull().sum()
```

# 2. DATA CLEANING

## Missing values

```python id="step2"
df.fillna(df.mean(numeric_only=True), inplace=True)
df.fillna(df.mode().iloc[0], inplace=True)
```

## Duplicates

```python id="step3"
df.drop_duplicates(inplace=True)
```

# 3. FEATURE ENGINEERING (VERY IMPORTANT)

This is where projects become “data science”.

## Example patterns for ALL projects:

### A. Numerical features

```python id="feat1"
df["total"] = df["price"] * df["quantity"]
df["average"] = df[["a","b","c"]].mean(axis=1)
```

### B. Time features (Sales / Weather)

```python id="feat2"
df["date"] = pd.to_datetime(df["date"])

df["month"] = df["date"].dt.month
df["weekday"] = df["date"].dt.day_name()
```

### C. Categorical grouping

```python id="feat3"
df["category_encoded"] = df["category"].astype("category").cat.codes
```

### D. Business logic features

```python id="feat4"
df["high_value"] = np.where(df["total"] > df["total"].mean(), 1, 0)
```

# 4. EXPLORATORY DATA ANALYSIS (EDA)

## Core patterns you will reuse everywhere:

### Group analysis

```python id="eda1"
df.groupby("category")["total"].sum()
df.groupby("city")["total"].mean()
```

### Sorting insights

```python id="eda2"
df.groupby("product")["total"].sum().sort_values(ascending=False)
```

### Correlation (important for Student + Employee + Weather)

```python id="eda3"
df.corr(numeric_only=True)
```

# 5. VISUALIZATION PATTERNS (Matplotlib)

## 1. Trend line (Sales / Weather)

```python id="viz1"
plt.plot(df.groupby("date")["total"].sum())
plt.show()
```

## 2. Bar chart (category comparison)

```python id="viz2"
df.groupby("category")["total"].sum().plot(kind="bar")
plt.show()
```

## 3. Histogram (distribution)

```python id="viz3"
plt.hist(df["salary"])
plt.show()
```

## 4. Scatter (relationship check)

```python id="viz4"
plt.scatter(df["study_hours"], df["math_marks"])
plt.show()
```

# PROJECT-SPECIFIC EXECUTION GUIDES

# PROJECT 1: STUDENT ANALYSIS

## Must-do features

```python id="p1f"
students["total_marks"] = students[["math_marks","science_marks","english_marks"]].sum(axis=1)
students["avg_marks"] = students["total_marks"] / 3
```

## Performance classification

```python id="p1c"
students["grade"] = np.where(students["avg_marks"] > 85, "A",
                      np.where(students["avg_marks"] > 60, "B", "C"))
```

## Key insight questions

- Does study hours increase marks?

```python id="p1i"
students[["study_hours","avg_marks"]].corr()
```

# PROJECT 2: SALES ANALYSIS

## Revenue feature

```python id="p2f"
sales["revenue"] = sales["price"] * sales["quantity"]
```

## Monthly analysis

```python id="p2m"
sales["month"] = pd.to_datetime(sales["date"]).dt.month

sales.groupby("month")["revenue"].sum()
```

# PROJECT 3: WEATHER ANALYSIS

## Feels-like index

```python id="p3f"
weather["feels_like"] = weather["temperature"] - (weather["wind_speed"] * 0.5)
```

## Extreme detection

```python id="p3e"
weather[weather["temperature"] > weather["temperature"].mean() + 2*weather["temperature"].std()]
```

# PROJECT 4: EMPLOYEE ANALYSIS

## Experience grouping

```python id="p4f"
employees["exp_group"] = pd.cut(employees["experience_years"],
                               bins=[0,5,10,20,40],
                               labels=["Junior","Mid","Senior","Expert"])
```

## Salary vs experience

```python id="p4s"
employees.groupby("exp_group")["salary"].mean()
```

# PROJECT 5: CUSTOMER ANALYSIS

## High value customer

```python id="p5f"
customers["high_value"] = customers["purchase_amount"] > customers["purchase_amount"].mean()
```

## Segmentation

```python id="p5s"
customers.groupby("category_preference")["purchase_amount"].mean()
```

# FINAL REALITY CHECK (IMPORTANT)

If you complete all 5 projects properly, you will be able to:

- Clean real datasets confidently
- Do full EDA independently
- Build features like a data scientist
- Create meaningful visualizations
- Answer interview-level questions

# HOW TO LEVEL UP NEXT

After this set, your next logical steps are:

1. Combine all projects into a **portfolio notebook**
2. Convert one project into **dashboard (Plotly / Power BI style)**
3. Start **SQL for data analysis interviews**
4. Move to **Machine Learning basics (scikit-learn)**
