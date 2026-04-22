I’ll give you **clean, standard solutions + reusable patterns** (because in real jobs, you don’t memorize answers—you reuse patterns).

Assume:

```python id="base01"
import pandas as pd
import numpy as np
```

# 1. Complex Filtering & Conditions

## 1. Above column mean

```python id="f1"
df[df["col"] > df["col"].mean()]
```

## 2. Multiple numeric conditions

```python id="f2"
df[(df["a"] > 50) & (df["b"] < 100)]
```

## 3. Top 10%

```python id="f3"
threshold = df["col"].quantile(0.9)
df[df["col"] >= threshold]
```

## 4. String patterns

```python id="f4"
df[df["city"].str.contains("delhi")]
df[df["name"].str.startswith("A")]
df[df["name"].str.endswith("n")]
```

## 5. Value appears more than N times

```python id="f5"
counts = df["col"].value_counts()
valid = counts[counts > N].index

df[df["col"].isin(valid)]
```

# 2. GroupBy (Advanced)

## 1. Top 2 per group

```python id="g1"
df.groupby("group").apply(lambda x: x.nlargest(2, "value"))
```

## 2. Multiple aggregations

```python id="g2"
df.groupby("group")["value"].agg(["sum", "mean", "max"])
```

## 3. % contribution

```python id="g3"
total = df["value"].sum()

(df.groupby("group")["value"].sum() / total) * 100
```

## 4. Trends (basic idea)

```python id="g4"
df.sort_values("time").groupby("group")["value"].apply(list)
```

(Then analyze increasing/decreasing manually or via diff)

## 5. Rank within group

```python id="g5"
df["rank"] = df.groupby("group")["value"].rank(ascending=False)
```

# 3. Window-like Operations

## 1. Cumulative sum

```python id="w1"
df["cumsum"] = df["value"].cumsum()
```

## 2. Rolling average

```python id="w2"
df["rolling_mean"] = df["value"].rolling(3).mean()
```

## 3. Difference between rows

```python id="w3"
df["diff"] = df["value"].diff()
```

## 4. Percentage change

```python id="w4"
df["pct_change"] = df["value"].pct_change()
```

## 5. Spike detection

```python id="w5"
df["rolling_std"] = df["value"].rolling(3).std()

df[df["value"] > df["value"].mean() + 2 * df["rolling_std"]]
```

# 4. Multi-Column Logic

## 1. Multiple conditions column

```python id="m1"
df["flag"] = np.where((df["a"] > 50) & (df["b"] < 20), 1, 0)
```

## 2. Compare columns

```python id="m2"
df["compare"] = np.where(df["a"] > df["b"], "A", "B")
```

## 3. Interaction feature

```python id="m3"
df["new"] = (df["a"] * df["b"]) / df["c"]
```

## 4. Normalize using another column

```python id="m4"
df["norm"] = df["a"] / df["b"]
```

## 5. Conditional replacement

```python id="m5"
df.loc[df["a"] < 0, "a"] = 0
```

# 5. Missing Data (Advanced)

## 1. Group-wise mean fill

```python id="m6"
df["col"] = df["col"].fillna(df.groupby("group")["col"].transform("mean"))
```

## 2. Forward / backward fill

```python id="m7"
df.fillna(method="ffill")
df.fillna(method="bfill")
```

## 3. Interpolation

```python id="m8"
df["col"] = df["col"].interpolate()
```

## 4. Missing pattern detection

```python id="m9"
df.isnull().mean()
```

## 5. Conditional fill

```python id="m10"
df["col"] = np.where(df["col"].isnull(), 0, df["col"])
```

# 6. Joins & Merging

## 1. Inner join

```python id="j1"
pd.merge(df1, df2, on="key", how="inner")
```

## 2. Left join

```python id="j2"
pd.merge(df1, df2, on="key", how="left")
```

## 3. Right join

```python id="j3"
pd.merge(df1, df2, on="key", how="right")
```

## 4. Outer join

```python id="j4"
pd.merge(df1, df2, on="key", how="outer")
```

## 5. Multiple keys

```python id="j5"
pd.merge(df1, df2, on=["k1", "k2"])
```

# 7. Reshaping Data

## 1. Pivot table

```python id="r1"
df.pivot_table(values="value", index="row", columns="col", aggfunc="sum")
```

## 2. Melt

```python id="r2"
pd.melt(df, id_vars=["id"], value_vars=["A", "B"])
```

## 3. Stack / Unstack

```python id="r3"
df.set_index(["A", "B"]).stack()
df.unstack()
```

## 4. Reverse aggregation

```python id="r4"
# depends on structure, usually melt + explode
```

## 5. Reporting format

```python id="r5"
df.pivot(index="date", columns="category", values="sales")
```

# 8. Real-World Business Logic

## 1. Category revenue ranking

```python id="b1"
df.groupby("category")["revenue"].sum().sort_values(ascending=False)
```

## 2. Churn-like logic

```python id="b2"
df.groupby("customer")["purchase_date"].max()
```

## 3. Consistent performer

```python id="b3"
df.groupby("employee")["score"].std().sort_values()
```

(low std = consistent)

## 4. Outliers

```python id="b4"
df[df["sales"] > df["sales"].mean() + 3*df["sales"].std()]
```

## 5. Best region per product

```python id="b5"
df.loc[df.groupby("product")["sales"].idxmax()]
```

# 9. Time-Based Analysis

## 1. Convert datetime

```python id="t1"
df["date"] = pd.to_datetime(df["date"])
```

## 2. Extract features

```python id="t2"
df["month"] = df["date"].dt.month
df["year"] = df["date"].dt.year
df["day"] = df["date"].dt.day
```

## 3. Monthly trend

```python id="t3"
df.groupby(df["date"].dt.month)["sales"].sum()
```

## 4. Growth rate

```python id="t4"
monthly = df.groupby(df["date"].dt.month)["sales"].sum()
monthly.pct_change()
```

## 5. Compare periods

```python id="t5"
df.groupby(df["date"].dt.year)["sales"].sum()
```

# 10. Interview-Level Problems

## 1. Second highest per group

```python id="i1"
df.groupby("group")["value"].apply(lambda x: x.nlargest(2).min())
```

## 2. Duplicate detection

```python id="i2"
df[df.duplicated(subset=["col1", "col2"])]
```

## 3. Business rule violations

```python id="i3"
df[df["sales"] < 0]
```

## 4. Scoring system

```python id="i4"
df["score"] = df["a"]*0.5 + df["b"]*0.3 + df["c"]*0.2
```

## 5. Summary report

```python id="i5"
df.groupby("category").agg({
    "sales": ["sum", "mean"],
    "profit": "mean"
})
```

# Final Reality Check

If you can do these without looking:

- You are ready for pandas interviews
- You can handle real datasets (Excel / SQL-like work)
- You are close to junior data analyst level
