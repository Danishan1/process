This is where you move from “coding plots” to “data storytelling”. I’ll give you **clean Matplotlib solutions + patterns**, not repetitive explanations.

Assume:

```python id="base_plot"
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

Your dataset is already:

```python id="df_setup"
np.random.seed(42)

dates = pd.date_range(start="2024-01-01", periods=30)
products = ["Laptop", "Mobile", "Tablet", "Headphones", "Smartwatch"]
customers = ["A", "B", "C", "D", "E", "F"]

data = {
    "Date": np.random.choice(dates, 100),
    "Product": np.random.choice(products, 100),
    "Customer": np.random.choice(customers, 100),
    "Price": np.random.randint(5000, 90000, 100),
    "Quantity": np.random.randint(1, 5, 100),
    "Discount": np.random.randint(0, 30, 100)
}

df = pd.DataFrame(data)

df["Sales"] = df["Price"] * df["Quantity"]
df["Profit"] = df["Sales"] * (1 - df["Discount"] / 100)
```

# Section 1: Basic Plots

## 1. Sales over time (line chart)

```python id="p1"
daily_sales = df.groupby("Date")["Sales"].sum()

plt.plot(daily_sales.index, daily_sales.values)
plt.show()
```

## 2. Sales per product (bar)

```python id="p2"
product_sales = df.groupby("Product")["Sales"].sum()

plt.bar(product_sales.index, product_sales.values)
plt.show()
```

## 3. Histogram

```python id="p3"
plt.hist(df["Sales"], bins=10)
plt.show()
```

## 4. Scatter (Price vs Quantity)

```python id="p4"
plt.scatter(df["Price"], df["Quantity"])
plt.show()
```

## 5. Pie chart

```python id="p5"
product_sales = df.groupby("Product")["Sales"].sum()

plt.pie(product_sales.values, labels=product_sales.index, autopct="%1.1f%%")
plt.show()
```

# Section 2: Customization

## 6. Titles and labels

```python id="c1"
plt.plot(daily_sales.index, daily_sales.values)
plt.title("Daily Sales Trend")
plt.xlabel("Date")
plt.ylabel("Sales")
plt.show()
```

## 7. Line styling

```python id="c2"
plt.plot(daily_sales, color="red", linestyle="--", marker="o")
plt.show()
```

## 8. Rotate x labels

```python id="c3"
plt.xticks(rotation=45)
plt.plot(daily_sales)
plt.show()
```

## 9. Grid

```python id="c4"
plt.plot(daily_sales)
plt.grid(True)
plt.show()
```

## 10. Figure size

```python id="c5"
plt.figure(figsize=(10, 5))
plt.plot(daily_sales)
plt.show()
```

# Section 3: Multiple Plots

## 11. Subplots (Sales & Profit)

```python id="m1"
plt.figure(figsize=(10,4))

plt.subplot(1,2,1)
plt.plot(df.groupby("Date")["Sales"].sum())
plt.title("Sales")

plt.subplot(1,2,2)
plt.plot(df.groupby("Date")["Profit"].sum())
plt.title("Profit")

plt.show()
```

## 12. Compare products

```python id="m2"
lp = df[df["Product"] == "Laptop"].groupby("Date")["Sales"].sum()
mb = df[df["Product"] == "Mobile"].groupby("Date")["Sales"].sum()

plt.plot(lp, label="Laptop")
plt.plot(mb, label="Mobile")
plt.legend()
plt.show()
```

## 13. Multi-line chart

```python id="m3"
for p in df["Product"].unique():
    temp = df[df["Product"] == p].groupby("Date")["Sales"].sum()
    plt.plot(temp, label=p)

plt.legend()
plt.show()
```

## 14. Subplots per product

```python id="m4"
fig, ax = plt.subplots(3, 2, figsize=(10,8))

products_list = df["Product"].unique()

for i, p in enumerate(products_list):
    r, c = divmod(i, 2)
    temp = df[df["Product"] == p].groupby("Date")["Sales"].sum()
    ax[r, c].plot(temp)
    ax[r, c].set_title(p)

plt.tight_layout()
plt.show()
```

## 15. Monthly comparison

```python id="m5"
df["Month"] = df["Date"].dt.month
monthly = df.groupby(["Month", "Product"])["Sales"].sum().unstack()

monthly.plot(kind="bar", figsize=(10,5))
plt.show()
```

# Section 4: Business Questions

## 16. Top 5 products

```python id="b1"
df.groupby("Product")["Sales"].sum().nlargest(5).plot(kind="bar")
plt.show()
```

## 17. Customer spending

```python id="b2"
df.groupby("Customer")["Sales"].sum().plot(kind="bar")
plt.show()
```

## 18. Profit trend

```python id="b3"
df.groupby("Date")["Profit"].sum().plot()
plt.show()
```

## 19. Peak sales day

```python id="b4"
df.groupby("Date")["Sales"].sum().plot()
plt.show()
```

(Highest point visually = peak)

## 20. Quantity vs Sales

```python id="b5"
plt.scatter(df["Quantity"], df["Sales"])
plt.show()
```

# Section 5: Advanced Visualization

## 21. Highlight max point

```python id="a1"
s = df.groupby("Date")["Sales"].sum()

plt.plot(s)

max_day = s.idxmax()
plt.scatter(max_day, s.max(), color="red")

plt.show()
```

## 22. Annotation

```python id="a2"
plt.plot(s)

plt.annotate("Peak", (max_day, s.max()))
plt.show()
```

## 23. Stacked bar

```python id="a3"
df.groupby(["Product", "Customer"])["Sales"].sum().unstack().plot(kind="bar", stacked=True)
plt.show()
```

## 24. Cumulative sales

```python id="a4"
df.groupby("Date")["Sales"].sum().cumsum().plot()
plt.show()
```

## 25. Moving average

```python id="a5"
daily = df.groupby("Date")["Sales"].sum()

daily.rolling(3).mean().plot()
plt.plot(daily)
plt.show()
```

# Section 6: Storytelling

## 26. Dominant product

```python id="s1"
df.groupby("Product")["Sales"].sum().plot(kind="pie", autopct="%1.1f%%")
plt.show()
```

## 27. Discount vs Sales

```python id="s2"
plt.scatter(df["Discount"], df["Sales"])
plt.show()
```

## 28. Customer segmentation

```python id="s3"
df.groupby("Customer")["Sales"].sum().plot(kind="bar")
plt.show()
```

## 29. Profit vs Sales

```python id="s4"
plt.scatter(df["Sales"], df["Profit"])
plt.show()
```

## 30. Dashboard style

```python id="s5"
plt.figure(figsize=(12,8))

plt.subplot(2,2,1)
df.groupby("Product")["Sales"].sum().plot(kind="bar")

plt.subplot(2,2,2)
df.groupby("Customer")["Sales"].sum().plot(kind="bar")

plt.subplot(2,2,3)
df.groupby("Date")["Sales"].sum().plot()

plt.subplot(2,2,4)
plt.scatter(df["Discount"], df["Sales"])

plt.tight_layout()
plt.show()
```

# Key Reality (Important)

If you can do this set, you are no longer at beginner level. You are at:

- Data analyst visualization level
- Interview-ready for plotting questions
- Ready for dashboards (Power BI / Tableau / Plotly)
