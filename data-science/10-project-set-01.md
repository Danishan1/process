We’ll create a **Mini Project Set-1 (Core Data Science Practice Set)** using:

- Python
- NumPy
- Pandas
- Matplotlib (and optionally Seaborn later)

Each project will force you to practice:
**data cleaning → EDA → feature engineering → visualization → insights**

```py
import numpy as np
import pandas as pd
from datetime import datetime, timedelta

np.random.seed(42)

# =========================
# 1. STUDENT PERFORMANCE
# =========================
n = 200

students = pd.DataFrame({
    "student_id": range(1, n+1),
    "name": [f"Student_{i}" for i in range(1, n+1)],
    "gender": np.random.choice(["Male", "Female"], n),
    "math_marks": np.random.randint(30, 100, n),
    "science_marks": np.random.randint(25, 100, n),
    "english_marks": np.random.randint(20, 100, n),
    "study_hours": np.round(np.random.uniform(1, 8, n), 1),
    "attendance": np.random.randint(50, 100, n)
})

students.to_csv("students.csv", index=False)


# =========================
# 2. SALES DATA
# =========================
n = 300

start_date = datetime(2023, 1, 1)

products = ["Laptop", "Phone", "Headphones", "Keyboard", "Mouse", "Monitor"]
categories = ["Electronics", "Electronics", "Accessories", "Accessories", "Accessories", "Electronics"]
cities = ["Delhi", "Mumbai", "Bangalore", "Chennai", "Kolkata"]

sales = pd.DataFrame({
    "order_id": range(1001, 1001+n),
    "product": np.random.choice(products, n),
    "category": np.random.choice(categories, n),
    "price": np.random.randint(300, 50000, n),
    "quantity": np.random.randint(1, 5, n),
    "date": [start_date + timedelta(days=int(x)) for x in np.random.randint(0, 365, n)],
    "customer_city": np.random.choice(cities, n)
})

sales.to_csv("sales.csv", index=False)


# =========================
# 3. WEATHER DATA
# =========================
n = 365

weather_dates = [start_date + timedelta(days=i) for i in range(n)]

weather = pd.DataFrame({
    "date": weather_dates,
    "temperature": np.random.normal(30, 8, n).round(1),
    "humidity": np.random.randint(30, 100, n),
    "wind_speed": np.random.uniform(0, 20, n).round(1),
    "rainfall": np.random.exponential(2, n).round(2)
})

weather.to_csv("weather.csv", index=False)


# =========================
# 4. EMPLOYEE DATA
# =========================
n = 150

departments = ["IT", "HR", "Finance", "Marketing", "Sales"]
cities_emp = ["Delhi", "Pune", "Hyderabad", "Bangalore"]

employees = pd.DataFrame({
    "employee_id": range(1, n+1),
    "department": np.random.choice(departments, n),
    "age": np.random.randint(22, 60, n),
    "experience_years": np.random.randint(0, 35, n),
    "salary": np.random.randint(20000, 200000, n),
    "city": np.random.choice(cities_emp, n)
})

employees.to_csv("employees.csv", index=False)


# =========================
# 5. CUSTOMER BEHAVIOR
# =========================
n = 250

customers = pd.DataFrame({
    "customer_id": range(1, n+1),
    "age": np.random.randint(18, 70, n),
    "gender": np.random.choice(["Male", "Female"], n),
    "purchase_amount": np.random.randint(100, 50000, n),
    "visits_per_month": np.random.randint(1, 30, n),
    "category_preference": np.random.choice(products, n)
})

customers.to_csv("customers.csv", index=False)

print("Datasets created successfully!")

```

# Mini Project Set-1 (Custom Data Science Practice Set)

We’ll use **simulated + real-world style datasets** so you can focus on concepts without waiting for external data.

# Project 1: Student Performance Analyzer

### Goal:

Analyze student marks and find patterns in performance.

### Dataset columns:

- student_id
- name
- gender
- math_marks
- science_marks
- english_marks
- study_hours
- attendance %

### Tasks:

#### 1. Data preprocessing

- Handle missing marks
- Fix data types
- Check duplicates

#### 2. Feature engineering

- total_marks
- average_marks
- performance_category (fail/pass/good/excellent)

#### 3. Analysis

- Which factor affects marks more? (study hours vs attendance)
- Gender-wise performance comparison

#### 4. Visualization

- Histogram of marks
- Bar chart of average by gender
- Scatter plot: study_hours vs marks

# Project 2: Sales Data Analyzer

### Goal:

Understand sales patterns of a small store.

### Dataset:

- order_id
- product
- category
- price
- quantity
- date
- customer_city

### Tasks:

- Handle missing prices or quantities
- Create revenue column (price × quantity)
- Extract month, weekday from date
- Top selling products
- Best months for sales

### Visualizations:

- Monthly sales trend line
- Top products bar chart
- Category revenue distribution

# Project 3: Weather Data Analysis

### Goal:

Analyze weather trends

### Dataset:

- date
- temperature
- humidity
- wind_speed
- rainfall

### Tasks:

- Clean missing weather values
- Create “feels_like_index”
- Identify extreme weather days

### Visuals:

- Temperature trend line
- Rainfall distribution
- Correlation heatmap

# Project 4: Employee Salary Analysis

### Goal:

Understand salary structure in a company

### Dataset:

- employee_id
- department
- age
- experience_years
- salary
- city

### Tasks:

- Clean salary outliers
- Encode department
- Create experience buckets

### Analysis:

- Salary vs experience
- Department-wise salary comparison

# Project 5: Customer Behavior Analysis (Mini ML Prep)

### Goal:

Understand customer purchase behavior

### Dataset:

- customer_id
- age
- gender
- purchase_amount
- visits_per_month
- category_preference

### Tasks:

- Feature scaling (purchase_amount, visits)
- Create “high_value_customer” label
- Segment customers

### Visuals:

- Age vs spending scatter
- Customer segmentation chart

# How we will practice (IMPORTANT)

Each project follows SAME workflow:

## Step 1: Load data

## Step 2: Data cleaning (Pandas)

## Step 3: EDA (NumPy + Pandas)

## Step 4: Feature engineering

## Step 5: Visualization (Matplotlib)

## Step 6: Insights

# Suggested Learning Strategy

Do in this order:

### Week 1:

- Project 1 (Student Performance)
- Project 2 (Sales)

### Week 2:

- Project 3 (Weather)
- Project 4 (Salary)

### Week 3:

- Project 5 (Customer behavior)
- Combine everything into one mini portfolio
