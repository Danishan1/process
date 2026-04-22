# 1. Data Preprocessing (Cleaning + Preparing Data)

This is about making raw data usable for machine learning.

## 1.1 Handling missing values

Real-world data is messy.

### Common strategies:

- **Remove rows/columns**
  - When missing data is small

- **Imputation (fill values)**
  - Mean / median (numeric)
  - Mode (categorical)
  - Forward/backward fill (time series)
  - Model-based imputation (advanced)

👉 Example:

- Salary missing → fill with median salary

## 1.2 Handling duplicates

- Duplicate rows can bias models
- Common in scraped or merged datasets

## 1.3 Fixing data types

- Convert strings → datetime
- Numbers stored as text → numeric
- Categorical variables → category type

## 1.4 Handling outliers

Outliers can distort models like regression.

### Techniques:

- Z-score filtering
- IQR method
- Winsorization (capping values)
- Domain-based rules

## 1.5 Encoding categorical variables

Machine learning models need numbers.

### Methods:

- **One-Hot Encoding**
  - Good for nominal categories

- **Label Encoding**
  - Good for ordinal data

- **Target Encoding**
  - Advanced, risk of leakage

## 1.6 Feature scaling

Important for distance-based models.

### Methods:

- Standardization (Z-score scaling)
- Min-Max scaling
- Robust scaling (handles outliers)

# 2. Feature Engineering (Creating Better Inputs)

This is the “art + science” part of data science.

## 2.1 Why feature engineering matters

Better features → simpler models perform better.

## 2.2 Feature creation

### From raw data:

- Date → extract:
  - day, month, year
  - weekday/weekend
  - hour (for time series)

- Text →:
  - word count
  - TF-IDF features
  - sentiment score

- Location →:
  - distance from city center
  - region grouping

## 2.3 Mathematical transformations

To make data more model-friendly:

- Log transform (for skewed data)
- Square root transform
- Polynomial features (x², x³)
- Box-Cox transformation

## 2.4 Binning / Discretization

Convert continuous → categorical:

- Age → child / adult / senior
- Income ranges

Useful for:

- Tree models
- Interpretability

## 2.5 Interaction features

Combine features:

- price × quantity
- age × income
- humidity × temperature

These often capture hidden relationships.

## 2.6 Aggregated features

Especially in grouped data:

- average spending per user
- total purchases per month
- click rate per session

## 2.7 Encoding time-based patterns

For time series:

- lag features (previous values)
- rolling mean
- seasonal indicators

## 2.8 Dimensionality reduction (advanced feature engineering)

When too many features exist:

- PCA (Principal Component Analysis)
- t-SNE (visualization)
- UMAP (modern alternative)

# 3. Preprocessing vs Feature Engineering (Difference)

| Preprocessing      | Feature Engineering |
| ------------------ | ------------------- |
| Clean data         | Create new signals  |
| Fix missing values | Add new columns     |
| Encode categories  | Transform meaning   |
| Scale features     | Combine features    |

# 4. Typical Workflow in Data Science

A real pipeline usually looks like:

1. Data collection
2. Cleaning (preprocessing)
3. EDA (understanding data)
4. Feature engineering
5. Train/test split
6. Model training
7. Evaluation
8. Iteration (back to features)

# 5. Key Insight (very important)

> Most ML performance gains come from **feature engineering**, not model complexity.

A simple model + great features often beats a complex model + bad features.

# 6. Tools you’ll commonly use

- **Pandas** → preprocessing & feature creation
- **NumPy** → math operations
- **scikit-learn** → preprocessing pipelines & encoders
