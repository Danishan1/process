# Titanic Survival Analysis — Full Solutions

```python
import pandas as pd
import numpy as np

df = pd.read_csv("titanic.csv")
```

# DATA PREPROCESSING

## 1. Missing values in each column

```python
df.isnull().sum()
```

### Expected insight:

- Age → missing (large)
- Cabin → many missing
- Embarked → few missing

## 2. Which columns to drop and why?

### Typical decision:

```text
Cabin → too many missing values (>70%), not reliable
Ticket → high cardinality, not useful directly
Name → used later for feature engineering (not dropped immediately)
PassengerId → only identifier
```

### Code:

```python
df.drop(columns=["Cabin", "Ticket", "PassengerId"], inplace=True)
```

## 3. Handling missing Age values

### Best practice: fill using median grouped by class or gender

```python
df["Age"] = df["Age"].fillna(df["Age"].median())
```

### Better approach (more realistic):

```python
df["Age"] = df["Age"].fillna(df.groupby("Pclass")["Age"].transform("median"))
```

## 4. Handling missing Embarked values

```python
df["Embarked"].fillna(df["Embarked"].mode()[0], inplace=True)
```

# FEATURE ENGINEERING

## 5. FamilySize

```python
df["FamilySize"] = df["SibSp"] + df["Parch"] + 1
```

## 6. IsAlone feature

```python
df["IsAlone"] = (df["FamilySize"] == 1).astype(int)
```

## 7. Extract titles from Name

```python
df["Title"] = df["Name"].str.extract(" ([A-Za-z]+)\.", expand=False)
```

### Group rare titles:

```python
df["Title"] = df["Title"].replace(
    ["Mlle", "Ms", "Mme"], "Miss"
)

df["Title"] = df["Title"].replace(
    ["Lady", "Countess", "Capt", "Col", "Don", "Dr", "Major", "Rev", "Sir", "Jonkheer"],
    "Rare"
)
```

# ANALYSIS

## 8. Survival rate by gender

```python
df.groupby("Sex")["Survived"].mean()
```

### Insight:

- Females → much higher survival rate
- Males → lower survival

## 9. Does passenger class affect survival?

```python
df.groupby("Pclass")["Survived"].mean()
```

### Insight:

- 1st class → highest survival
- 3rd class → lowest survival

## 10. Does age group affect survival?

### Step 1: create age groups

```python
df["AgeGroup"] = pd.cut(df["Age"],
                        bins=[0, 12, 18, 35, 60, 100],
                        labels=["Child", "Teen", "Adult", "MidAge", "Senior"])
```

### Step 2: analyze survival

```python
df.groupby("AgeGroup")["Survived"].mean()
```

# OPTIONAL VISUALIZATION (VERY IMPORTANT FOR INTERVIEWS)

## Survival by gender

```python
df.groupby("Sex")["Survived"].mean().plot(kind="bar")
```

## Survival by class

```python
df.groupby("Pclass")["Survived"].mean().plot(kind="bar")
```

## Age distribution of survivors vs non-survivors

```python
import matplotlib.pyplot as plt

df[df["Survived"] == 1]["Age"].hist(alpha=0.5, label="Survived")
df[df["Survived"] == 0]["Age"].hist(alpha=0.5, label="Not Survived")

plt.legend()
plt.show()
```

# FINAL INSIGHTS (WHAT YOU SHOULD SAY IN INTERVIEW)

## Key conclusions:

### 1. Gender mattered strongly

- Females had much higher survival probability

### 2. Class mattered significantly

- Higher class passengers were prioritized for survival

### 3. Age influenced survival

- Children had higher survival chances

### 4. Family size mattered

- Small families survived more than large ones

# WHAT THIS PROJECT TESTS

If you understand this properly, you are being tested on:

- Data cleaning logic
- Feature engineering thinking
- Group-based analysis
- Business interpretation of data
