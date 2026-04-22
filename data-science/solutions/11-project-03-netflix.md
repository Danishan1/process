# Project 3: Netflix Movies & TV Shows — Full Solutions

```python id="netflix_base"
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("netflix_titles.csv")
df.head()
```

# DATA CLEANING

## 1. Handle missing values (director, country)

### Step 1: check missing values

```python id="n1"
df.isnull().sum()
```

### Step 2: fill missing values

```python id="n2"
df["director"].fillna("Unknown", inplace=True)
df["country"].fillna("Unknown", inplace=True)
```

### Insight:

We do NOT drop because:

- Missing = meaningful category ("Unknown")

## 2. Convert date_added to datetime

```python id="n3"
df["date_added"] = pd.to_datetime(df["date_added"])
```

## 3. Extract year and month

```python id="n4"
df["year_added"] = df["date_added"].dt.year
df["month_added"] = df["date_added"].dt.month
```

# FEATURE ENGINEERING

## 4. Content age

```python id="n5"
df["content_age"] = 2026 - df["release_year"]
```

## 5. Categorize duration

### Step 1: clean duration column

```python id="n6"
df["duration"] = df["duration"].str.replace(" min", "")
df["duration"] = df["duration"].str.replace(" Season", "")
df["duration"] = df["duration"].str.replace("s", "")
```

### Step 2: convert numeric part

```python id="n7"
df["duration_num"] = df["duration"].str.extract("(\d+)").astype(float)
```

### Step 3: categorize

```python id="n8"
df["duration_category"] = pd.cut(
    df["duration_num"],
    bins=[0, 60, 120, 300],
    labels=["Short", "Medium", "Long"]
)
```

# ANALYSIS

## 6. Top countries producing content

```python id="n9"
df["country"].value_counts().head(10)
```

### Insight:

- USA dominates
- India, UK also strong contributors

## 7. Most common genres

```python id="n10"
df["listed_in"].value_counts().head(10)
```

## 8. Movies vs TV Shows ratio

```python id="n11"
df["type"].value_counts()
```

### Visualization:

```python id="n12"
df["type"].value_counts().plot(kind="pie", autopct="%1.1f%%")
plt.title("Movies vs TV Shows")
plt.ylabel("")
plt.show()
```

## 9. Trend of content over years

```python id="n13"
df.groupby("release_year")["show_id"].count().plot()
plt.title("Content Released Over Years")
plt.show()
```

### Insight:

- Rapid growth after 2015
- Streaming boom phase

# VISUALIZATION

## 10. Year-wise content growth

```python id="n14"
df.groupby("year_added")["show_id"].count().plot(kind="bar")
plt.title("Content Added per Year")
plt.show()
```

## 11. Top genres bar chart

```python id="n15"
df["listed_in"].value_counts().head(10).plot(kind="bar")
plt.title("Top Genres")
plt.xticks(rotation=90)
plt.show()
```

## 12. Pie chart: Movies vs TV shows

```python id="n16"
df["type"].value_counts().plot(
    kind="pie",
    autopct="%1.1f%%",
    figsize=(6,6)
)
plt.title("Movies vs TV Shows")
plt.ylabel("")
plt.show()
```

# FINAL INSIGHTS (INTERVIEW-READY)

## 1. Content distribution

- Movies dominate compared to TV shows
- Platform is more movie-heavy

## 2. Country contribution

- USA leads content production
- India and UK are significant contributors

## 3. Content growth trend

- Sharp increase after 2015
- Peak expansion period between 2018–2021

## 4. Genre trends

- International Movies, Drama, Comedy are most common
- Platform focuses heavily on entertainment content

## 5. Content age insight

- Majority of content is recent (less than 10 years old)
- Netflix prioritizes modern content

# WHAT THIS PROJECT TESTS

If you understand this well, you can handle:

- Time series analysis (content trends)
- Text-based categorical parsing (genres, countries)
- Real-world messy column cleaning (duration parsing)
- Business insights from entertainment data

# WHAT YOU LEARNED HERE (IMPORTANT)

You practiced:

- datetime feature engineering
- string parsing + transformation
- categorical aggregation
- trend analysis
- visualization storytelling
