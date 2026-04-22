# HOW TO PRACTICE (IMPORTANT)

For each dataset:

## Step 1

Download from Kaggle

## Step 2

Load using:

```python
import pandas as pd
df = pd.read_csv("file.csv")
```

## Step 3

Solve in order:

- Cleaning
- EDA
- Feature engineering
- Visualization
- Insights

# HOW THIS SET IS DIFFERENT

This is not tutorial learning.

This is:

- real data
- incomplete data
- messy features
- real-world thinking

# PROJECT SET-2: Kaggle Real Dataset Practice

You will pick **1 dataset per project from Kaggle** and solve structured questions.

# Project 1: Titanic Survival Analysis

Dataset: Kaggle Titanic Dataset

### Goal:

Understand survival patterns.

## Practice Questions

### Data Preprocessing

1. How many missing values are in each column?
2. Which columns should be dropped and why?
3. How will you handle missing Age values?
4. How will you handle Embarked column missing values?

### Feature Engineering

5. Create a feature “FamilySize” = SibSp + Parch + 1
6. Create a feature “IsAlone”
7. Extract titles from Name (Mr, Mrs, etc.)

### Analysis

8. What is survival rate by gender?
9. Does passenger class affect survival?
10. Does age group affect survival?

### Visualization

11. Survival distribution (bar chart)
12. Age distribution of survivors vs non-survivors
13. Correlation heatmap

# Project 2: House Price Prediction Dataset

Dataset: Kaggle House Prices Dataset

## Goal:

Understand what affects house prices.

## Practice Questions

### Data Cleaning

1. Which columns have >50% missing values?
2. How will you handle categorical missing values?
3. How will you treat outliers in SalePrice?

### Feature Engineering

4. Create TotalArea = TotalBsmtSF + 1stFlrSF + 2ndFlrSF
5. Convert year-based features into house age
6. Encode categorical variables properly

### Analysis

7. Top 10 features correlated with price
8. Does house age affect price?
9. Does neighborhood impact price?

### Visualization

10. Price distribution (skewed or normal?)
11. Scatter plot: area vs price
12. Boxplot: neighborhood vs price

# Project 3: Netflix Movies & TV Shows

Dataset: Kaggle Netflix Dataset

## Goal:

Analyze content trends.

## Practice Questions

### Data Cleaning

1. Handle missing values in director and country
2. Convert date_added into datetime
3. Extract year and month from date_added

### Feature Engineering

4. Create content age (current year - release year)
5. Categorize duration into short/medium/long

### Analysis

6. Top countries producing content
7. Most common genres
8. Movies vs TV shows ratio
9. Trend of content over years

### Visualization

10. Year-wise content growth
11. Top genres bar chart
12. Pie chart: Movies vs TV shows

# Project 4: Credit Card Customer Segmentation

Dataset: Kaggle Credit Card Dataset

## Goal:

Understand customer behavior patterns.

## Practice Questions

### Data Preprocessing

1. Check missing values
2. Remove highly correlated features

### Feature Engineering

3. Create average purchase per transaction
4. Create utilization ratio
5. Segment customers into low/medium/high spenders

### Analysis

6. Who are high-value customers?
7. What is spending pattern vs income?
8. Which features define loyal customers?

### Visualization

9. Clustering scatter plot (optional)
10. Income vs spending
11. Feature correlation heatmap

# Project 5: Airbnb Listings Dataset

Dataset: Kaggle Airbnb Dataset

## Goal:

Analyze rental pricing trends.

## Practice Questions

### Data Cleaning

1. Handle missing reviews and prices
2. Remove extreme outliers in price

### Feature Engineering

3. Price per person (price / accommodates)
4. Availability score
5. Review density = number_of_reviews / years_active

### Analysis

6. Most expensive neighborhoods
7. Price distribution by room type
8. Host activity vs price

### Visualization

9. Map-style scatter (if possible)
10. Price distribution histogram
11. Neighborhood comparison boxplot
