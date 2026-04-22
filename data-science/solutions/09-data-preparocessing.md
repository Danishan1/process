This is a solid conceptual map of preprocessing and feature engineering. I’ll now convert it into a **practical, code-oriented cheat sheet** so you can actually implement each idea in Python (mainly pandas + sklearn).

# 1. Data Preprocessing (Practical Implementation)

Assume:

```python id="base"
import pandas as pd
import numpy as np
```

## 1.1 Handling Missing Values

### Check missing values

```python id="m1"
df.isnull().sum()
```

### Drop missing values

```python id="m2"
df.dropna()                 # rows
df.dropna(axis=1)          # columns
```

### Fill missing values

#### Numeric → mean/median

```python id="m3"
df["salary"].fillna(df["salary"].mean(), inplace=True)
df["salary"].fillna(df["salary"].median(), inplace=True)
```

#### Categorical → mode

```python id="m4"
df["city"].fillna(df["city"].mode()[0], inplace=True)
```

#### Time series → forward/backward fill

```python id="m5"
df.fillna(method="ffill")
df.fillna(method="bfill")
```

## 1.2 Handling Duplicates

```python id="d1"
df.duplicated().sum()
df.drop_duplicates(inplace=True)
```

## 1.3 Fixing Data Types

```python id="t1"
df["date"] = pd.to_datetime(df["date"])
df["price"] = df["price"].astype(float)
df["category"] = df["category"].astype("category")
```

## 1.4 Handling Outliers

### Z-score method

```python id="o1"
from scipy import stats

df = df[(np.abs(stats.zscore(df["value"])) < 3)]
```

### IQR method

```python id="o2"
Q1 = df["value"].quantile(0.25)
Q3 = df["value"].quantile(0.75)

IQR = Q3 - Q1

df = df[(df["value"] >= Q1 - 1.5 * IQR) & (df["value"] <= Q3 + 1.5 * IQR)]
```

## 1.5 Encoding Categorical Variables

### One-hot encoding

```python id="e1"
pd.get_dummies(df["city"])
```

### Label encoding

```python id="e2"
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
df["city_encoded"] = le.fit_transform(df["city"])
```

## 1.6 Feature Scaling

### Standardization (Z-score)

```python id="s1"
df["scaled"] = (df["value"] - df["value"].mean()) / df["value"].std()
```

### Min-Max scaling

```python id="s2"
df["scaled"] = (df["value"] - df["value"].min()) / (df["value"].max() - df["value"].min())
```

### Robust scaling (outlier-safe)

```python id="s3"
df["scaled"] = (df["value"] - df["value"].median()) / (df["value"].quantile(0.75) - df["value"].quantile(0.25))
```

# 2. Feature Engineering (Practical)

## 2.1 Date Features

```python id="f1"
df["date"] = pd.to_datetime(df["date"])

df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day"] = df["date"].dt.day
df["weekday"] = df["date"].dt.weekday
df["is_weekend"] = df["weekday"] >= 5
```

## 2.2 Mathematical Transformations

### Log transform

```python id="f2"
df["log_value"] = np.log1p(df["value"])
```

### Square root

```python id="f3"
df["sqrt_value"] = np.sqrt(df["value"])
```

### Polynomial features

```python id="f4"
df["value_squared"] = df["value"] ** 2
```

## 2.3 Binning (Discretization)

```python id="f5"
df["age_group"] = pd.cut(df["age"],
                         bins=[0, 18, 40, 60, 100],
                         labels=["child", "adult", "middle", "senior"])
```

## 2.4 Interaction Features

```python id="f6"
df["price_quantity"] = df["price"] * df["quantity"]
df["income_age"] = df["income"] * df["age"]
```

## 2.5 Aggregated Features

```python id="f7"
df["avg_spend_per_customer"] = df.groupby("customer")["sales"].transform("mean")
df["total_sales_per_customer"] = df.groupby("customer")["sales"].transform("sum")
```

## 2.6 Lag Features (Time Series)

```python id="f8"
df = df.sort_values("date")
df["lag_1"] = df["sales"].shift(1)
df["lag_7"] = df["sales"].shift(7)
```

## 2.7 Rolling Features

```python id="f9"
df["rolling_mean_3"] = df["sales"].rolling(3).mean()
df["rolling_std_3"] = df["sales"].rolling(3).std()
```

## 2.8 Interaction + Ratio Features

```python id="f10"
df["efficiency"] = df["sales"] / df["quantity"]
```

# 3. Feature Engineering vs Preprocessing (Practical View)

### Preprocessing examples:

- fill missing values
- remove outliers
- encode categories
- scale data

### Feature engineering examples:

- create revenue = price × quantity
- extract month from date
- rolling average sales
- customer lifetime value

# 4. Full Real Pipeline Example

```python id="pipeline"
# Step 1: clean
df = df.drop_duplicates()
df["price"].fillna(df["price"].median(), inplace=True)

# Step 2: feature creation
df["sales"] = df["price"] * df["quantity"]
df["month"] = pd.to_datetime(df["date"]).dt.month

# Step 3: aggregation
customer_sales = df.groupby("customer")["sales"].transform("sum")
df["customer_lifetime_value"] = customer_sales

# Step 4: scaling
df["scaled_sales"] = (df["sales"] - df["sales"].mean()) / df["sales"].std()
```

# 5. Key Interview Insight

If you remember only one thing:

Most machine learning improvement does not come from models.

It comes from:

- better features
- cleaner data
- correct transformations
