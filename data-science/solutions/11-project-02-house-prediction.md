# Project 2: House Price Prediction — Full Solutions

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("train.csv")
```

# DATA CLEANING

## 1. Columns with >50% missing values

```python
missing_ratio = df.isnull().mean() * 100
missing_ratio[missing_ratio > 50].sort_values(ascending=False)
```

### Typical output (real dataset insight):

- PoolQC
- MiscFeature
- Alley
- Fence

### Interpretation:

These columns are mostly unusable → drop them.

```python
df.drop(columns=missing_ratio[missing_ratio > 50].index, inplace=True)
```

## 2. Handling categorical missing values

### Strategy:

- Fill with `"None"` if missing means "not available"
- Fill with mode if missing is accidental

```python
cat_cols = df.select_dtypes(include="object").columns

df[cat_cols] = df[cat_cols].fillna("None")
```

### Why:

In housing data, missing often means "does not exist" (e.g., no basement finish type)

## 3. Handling outliers in SalePrice

### Step 1: visualize

```python
sns.boxplot(df["SalePrice"])
plt.show()
```

### Step 2: IQR method

```python
Q1 = df["SalePrice"].quantile(0.25)
Q3 = df["SalePrice"].quantile(0.75)

IQR = Q3 - Q1

df = df[(df["SalePrice"] >= Q1 - 1.5 * IQR) &
        (df["SalePrice"] <= Q3 + 1.5 * IQR)]
```

# FEATURE ENGINEERING

## 4. Total Area

```python
df["TotalArea"] = (
    df["TotalBsmtSF"] +
    df["1stFlrSF"] +
    df["2ndFlrSF"]
)
```

## 5. House Age

```python
df["HouseAge"] = df["YrSold"] - df["YearBuilt"]
```

### Optional feature (renovation-aware):

```python
df["RemodelAge"] = df["YrSold"] - df["YearRemodAdd"]
```

## 6. Encoding categorical variables

### Option 1: One-hot encoding (best for linear models)

```python
df_encoded = pd.get_dummies(df, drop_first=True)
```

### Option 2: Label encoding (tree models)

```python
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()

for col in df.select_dtypes(include="object").columns:
    df[col] = le.fit_transform(df[col])
```

# ANALYSIS

## 7. Top 10 features correlated with price

```python
corr = df.corr(numeric_only=True)

top_features = corr["SalePrice"].sort_values(ascending=False).head(10)
top_features
```

### Insight:

Usually strongest:

- OverallQual
- TotalArea
- GrLivArea
- GarageCars

## 8. Does house age affect price?

```python
df.groupby("HouseAge")["SalePrice"].mean().plot()
plt.title("House Age vs Price")
plt.show()
```

### Insight:

- Older houses → generally lower price
- But renovated homes can break pattern

## 9. Does neighborhood impact price?

```python
df.groupby("Neighborhood")["SalePrice"].mean().sort_values().plot(kind="bar")
plt.xticks(rotation=90)
plt.show()
```

### Insight:

- Location is one of strongest predictors
- Some neighborhoods dominate pricing

# VISUALIZATION

## 10. Price distribution (skewness check)

```python
sns.histplot(df["SalePrice"], kde=True)
plt.title("SalePrice Distribution")
plt.show()
```

### Insight:

- Highly right-skewed (common in real estate)
- Needs log transformation for ML

```python
df["SalePrice_log"] = np.log1p(df["SalePrice"])
```

## 11. Area vs Price (relationship)

```python
plt.scatter(df["TotalArea"], df["SalePrice"])
plt.xlabel("Total Area")
plt.ylabel("Sale Price")
plt.show()
```

### Insight:

- Strong positive correlation
- But with outliers (luxury homes)

## 12. Neighborhood vs Price (boxplot)

```python
plt.figure(figsize=(12,6))

sns.boxplot(x="Neighborhood", y="SalePrice", data=df)

plt.xticks(rotation=90)
plt.show()
```

### Insight:

- Shows price distribution per location
- Helps identify premium vs low-value areas

# FINAL INSIGHTS (INTERVIEW ANSWER STYLE)

## 1. Key drivers of house price:

- Overall quality of house
- Living area size
- Location (neighborhood)
- Garage capacity
- Basement area

## 2. House age impact:

- Newer houses → higher price
- Older houses → lower price unless renovated

## 3. Price distribution:

- Highly skewed → not normal
- Needs log transformation for ML models

## 4. Location effect:

- Strongest categorical predictor
- Some neighborhoods dominate pricing significantly

# WHAT THIS PROJECT TEACHES

If you can solve this properly, you now understand:

- Real-world missing data handling
- Feature engineering for regression
- Correlation-based feature selection
- Business interpretation of numeric data
- Visualization for regression problems
