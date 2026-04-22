# Project 4: Credit Card Customer Segmentation — Full Solutions

```python id="cc_base"
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("credit_card.csv")
df.head()
```

# DATA PREPROCESSING

## 1. Check missing values

```python id="cc1"
df.isnull().sum()
```

### Insight:

- Most credit card datasets are clean
- If missing exists → usually impute or drop carefully

## 2. Remove highly correlated features

### Step 1: correlation matrix

```python id="cc2"
corr = df.corr(numeric_only=True)
```

### Step 2: visualize

```python id="cc3"
plt.figure(figsize=(10,6))
sns.heatmap(corr, annot=False, cmap="coolwarm")
plt.show()
```

### Step 3: remove highly correlated columns (>0.9)

```python id="cc4"
upper = corr.where(np.triu(np.ones(corr.shape), k=1).astype(bool))

to_drop = [col for col in upper.columns if any(upper[col] > 0.9)]

df.drop(columns=to_drop, inplace=True)
```

# FEATURE ENGINEERING

## 3. Average purchase per transaction

```python id="cc5"
df["avg_purchase"] = df["PURCHASES"] / (df["PURCHASES_TRX"] + 1)
```

## 4. Utilization ratio

### Key formula:

```text
balance / credit_limit
```

```python id="cc6"
df["utilization_ratio"] = df["BALANCE"] / df["CREDIT_LIMIT"]
```

## 5. Customer segmentation (low / medium / high spenders)

### Based on total purchases:

```python id="cc7"
df["spending_segment"] = pd.cut(
    df["PURCHASES"],
    bins=[0, 1000, 5000, df["PURCHASES"].max()],
    labels=["Low", "Medium", "High"]
)
```

# ANALYSIS

## 6. High-value customers

### Definition: high spending + low default risk + high utilization

```python id="cc8"
high_value = df[
    (df["PURCHASES"] > df["PURCHASES"].quantile(0.75)) &
    (df["CREDIT_LIMIT"] > df["CREDIT_LIMIT"].median())
]

high_value
```

## 7. Spending pattern vs income

```python id="cc9"
plt.scatter(df["CUST_ID"], df["PURCHASES"])
plt.title("Spending Pattern (raw view)")
plt.show()
```

### Better insight:

```python id="cc10"
df[["LIMIT_BAL", "PURCHASES"]].corr()
```

### Insight:

- Higher income → higher spending (expected but not always linear)

## 8. Features defining loyal customers

### Common proxy features:

```text
- high PURCHASES
- frequent transactions
- low payment delays
```

```python id="cc11"
df.groupby("spending_segment")[["PURCHASES", "PAYMENTS"]].mean()
```

# VISUALIZATION

## 9. Clustering scatter plot (optional preview)

```python id="cc12"
plt.scatter(df["PURCHASES"], df["BALANCE"])
plt.xlabel("Purchases")
plt.ylabel("Balance")
plt.show()
```

### Insight:

- Natural clusters often appear (good for KMeans)

## 10. Income vs spending

```python id="cc13"
plt.scatter(df["LIMIT_BAL"], df["PURCHASES"])
plt.xlabel("Credit Limit")
plt.ylabel("Purchases")
plt.show()
```

## 11. Correlation heatmap

```python id="cc14"
plt.figure(figsize=(12,8))
sns.heatmap(df.corr(numeric_only=True), cmap="coolwarm")
plt.title("Feature Correlation")
plt.show()
```

# FINAL INSIGHTS (INTERVIEW-READY)

## 1. Customer behavior is not linear

- High credit limit ≠ always high spending

## 2. Utilization ratio is important

- High utilization → active users
- Low utilization → inactive / cautious users

## 3. Spending segments exist naturally

- Low, medium, high spenders form clusters
- Good base for KMeans clustering

## 4. Loyalty indicators

Strong loyal customers usually have:

- high transaction frequency
- stable spending
- consistent repayments (if available)

# WHAT THIS PROJECT TEACHES

This project is **very important for interviews** because it introduces:

- customer segmentation thinking
- behavioral analytics
- correlation reduction
- clustering preparation mindset
- financial dataset interpretation

# REAL-WORLD CONNECTION

This is exactly how banks:

- target credit card offers
- detect premium customers
- identify risky users
- build reward programs
