This is basically the core of real-world data analysis in pandas.

I’ll give you **clean, structured solutions** (no extra noise), and where useful I’ll show the most important patterns instead of repetitive code.

Assume:

```python id="p0df21"
import pandas as pd
import numpy as np

df = pd.read_csv("data.csv")
```

# 1. Basic DataFrame Understanding

### 1. Load CSV

```python id="l2a91d"
df = pd.read_csv("data.csv")
```

### 2. First 5 rows

```python id="m91d2a"
df.head()
```

### 3. Last 5 rows

```python id="k12p0s"
df.tail()
```

### 4. Shape

```python id="q8x2lp"
df.shape
```

### 5. Column names

```python id="c9w1qa"
df.columns
```

### 6. Data types

```python id="v0p2ks"
df.dtypes
```

### 7. Summary statistics

```python id="n1x9qk"
df.describe()
```

# 2. Data Selection & Filtering

### 1. Single column

```python id="s2k9qp"
df["column_name"]
```

### 2. Multiple columns

```python id="d9q1pw"
df[["col1", "col2"]]
```

### 3. Filter > X

```python id="f1k9as"
df[df["column"] > X]
```

### 4. Multiple conditions

```python id="g2p9xd"
df[(df["col1"] > 50) & (df["col2"] < 100)]
df[(df["col1"] > 50) | (df["col2"] < 100)]
```

### 5. Null values

```python id="z8k1pw"
df[df["column"].isnull()]
```

### 6. Not null

```python id="x1p9qa"
df[df["column"].notnull()]
```

### 7. String match

```python id="t9q2ms"
df[df["city"] == "Delhi"]
```

# 3. Data Cleaning

### 1. Missing values per column

```python id="c1p9xq"
df.isnull().sum()
```

### 2. Fill with mean

```python id="n8p1as"
df["col"].fillna(df["col"].mean(), inplace=True)
```

### 3. Fill with median

```python id="k9x2ps"
df["col"].fillna(df["col"].median(), inplace=True)
```

### 4. Drop missing rows

```python id="p2x9qa"
df.dropna(inplace=True)
```

### 5. Rename columns

```python id="v9p1kx"
df.rename(columns={"old":"new"}, inplace=True)
```

### 6. Change data type

```python id="q1k9pa"
df["col"] = df["col"].astype(int)
```

### 7. Remove duplicates

```python id="m8p2qa"
df.drop_duplicates(inplace=True)
```

# 4. GroupBy (VERY IMPORTANT)

### 1. Sum

```python id="g9p1qa"
df.groupby("col")["value"].sum()
```

### 2. Mean

```python id="h2p9qa"
df.groupby("col")["value"].mean()
```

### 3. Multiple columns

```python id="j9p1qa"
df.groupby(["col1", "col2"]).sum()
```

### 4. Count

```python id="k1p9qa"
df.groupby("col").size()
```

### 5. Max / Min

```python id="l9p2qa"
df.groupby("col")["value"].max()
df.groupby("col")["value"].min()
```

### 6. Top group

```python id="m1p9qa"
df.groupby("col")["value"].sum().sort_values(ascending=False).head(1)
```

# 5. Sorting & Ranking

### 1. Sort ascending

```python id="n9p1qa"
df.sort_values("col")
```

### 2. Multiple columns

```python id="p1q9ka"
df.sort_values(["col1", "col2"])
```

### 3. Descending

```python id="q9p1qa"
df.sort_values("col", ascending=False)
```

### 4. Top 5

```python id="r1p9qa"
df.sort_values("col", ascending=False).head(5)
```

### 5. Ranking

```python id="s9p1qa"
df["rank"] = df["col"].rank(ascending=False)
```

# 6. New Columns (Feature Engineering)

### 1. New column

```python id="t1p9qa"
df["new"] = df["col1"] + df["col2"]
```

### 2. Condition

```python id="u9p1qa"
df["status"] = np.where(df["marks"] > 50, "Pass", "Fail")
```

### 3. Categories

```python id="v1p9qa"
df["category"] = np.where(df["marks"] > 80, "High",
                   np.where(df["marks"] > 50, "Medium", "Low"))
```

### 4. Percentage

```python id="w9p1qa"
df["percent"] = (df["obtained"] / df["total"]) * 100
```

### 5. Lambda

```python id="x1p9qa"
df["new"] = df["col"].apply(lambda x: x * 2)
```

# 7. Aggregation

### 1. Basic stats

```python id="y9p1qa"
df["col"].sum()
df["col"].mean()
df["col"].max()
df["col"].min()
```

### 2. Multiple aggregations

```python id="z1p9qa"
df["col"].agg(["sum", "mean", "max", "min"])
```

### 3. Group aggregation

```python id="a9p1qa"
df.groupby("col")["value"].agg(["sum", "mean"])
```

### 4. Compare groups

```python id="b1p9qa"
df.groupby("group")["value"].mean().sort_values()
```

# 8. Real-World Questions

### Highest selling product

```python id="c9p1qa"
df.groupby("product")["sales"].sum().sort_values(ascending=False).head(1)
```

### Highest revenue city

```python id="d1p9qa"
df.groupby("city")["revenue"].sum().sort_values(ascending=False).head(1)
```

### Top customer

```python id="e9p1qa"
df.groupby("customer")["spend"].sum().sort_values(ascending=False).head(1)
```

### Average sales per category

```python id="f1p9qa"
df.groupby("category")["sales"].mean()
```

### Underperforming products

```python id="g9p1qa"
df.groupby("product")["sales"].sum().sort_values().head(5)
```

### Percentage contribution

```python id="h1p9qa"
total = df["sales"].sum()
df.groupby("category")["sales"].sum() / total * 100
```

# 9. Advanced Filtering

### 1. Top N per group

```python id="i9p1qa"
df.groupby("category").apply(lambda x: x.nlargest(3, "sales"))
```

### 2. Range filter

```python id="j1p9qa"
df[(df["col"] >= 10) & (df["col"] <= 50)]
```

### 3. Multi-column filter

```python id="k9p1qa"
df[(df["a"] > 10) & (df["b"] < 20)]
```

### 4. String match

```python id="l1p9qa"
df[df["name"].str.contains("del")]
```

### 5. Date filter

```python id="m9p1qa"
df[df["date"] > "2024-01-01"]
```

# 10. Mini Challenge (What you should try yourself)

Do this without looking:

1. Create a DataFrame manually
2. Add missing values
3. Clean it
4. Create new columns
5. Group and analyze
6. Rank results
7. Write final insights

# What this covers

If you master this, you can:

- Handle real datasets
- Do 80% of data analyst job tasks
- Move into SQL + ML easily
