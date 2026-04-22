I’ll solve it in a **clean, structured way**, focusing on reusable patterns instead of repeating similar code.

# Setup

```python id="base_df"
import numpy as np
import pandas as pd

data = {
    "OrderID": [101, 102, 103, 104, 105, 106, 107],
    "Customer": ["A", "B", "C", "A", "B", "C", "D"],
    "Product": ["Laptop", "Mobile", "Tablet", "Laptop", "Mobile", "Tablet", "Laptop"],
    "Price": [80000, 20000, 30000, 85000, 22000, 28000, 90000],
    "Quantity": [1, 2, 1, 1, 3, 2, 1]
}

df = pd.DataFrame(data)
```

# Section 1: Feature Engineering

## 1. Total order value

```python id="f1"
df["TotalValue"] = df["Price"] * df["Quantity"]
```

## 2. Discount column

```python id="f2"
df["Discount"] = np.where(df["TotalValue"] > 50000,
                          df["TotalValue"] * 0.10,
                          df["TotalValue"] * 0.05)
```

## 3. Final payable amount

```python id="f3"
df["FinalAmount"] = df["TotalValue"] - df["Discount"]
```

## 4. High-value order flag

```python id="f4"
mean_value = df["TotalValue"].mean()

df["HighValueFlag"] = df["TotalValue"] > mean_value
```

## 5. Normalization (standard scaling)

```python id="f5"
df["NormalizedValue"] = (
    df["TotalValue"] - df["TotalValue"].mean()
) / df["TotalValue"].std()
```

# Section 2: Filtering & Conditions

## 6. Quantity > 2

```python id="s1"
df[df["Quantity"] > 2]
```

## 7. Above average order value

```python id="s2"
df[df["TotalValue"] > df["TotalValue"].mean()]
```

## 8. Customer spends > X

```python id="s3"
X = 50000

df[df["Customer"] == "B"][df["TotalValue"] > X]
```

## 9. Products with quantity > average

```python id="s4"
df[df["Quantity"] > df["Quantity"].mean()]
```

## 10. Price above product average

```python id="s5"
df[df["Price"] > df.groupby("Product")["Price"].transform("mean")]
```

# Section 3: GroupBy Analysis

## 11. Revenue per customer

```python id="g1"
df.groupby("Customer")["TotalValue"].sum()
```

## 12. Average order value per product

```python id="g2"
df.groupby("Product")["TotalValue"].mean()
```

## 13. Quantity per product

```python id="g3"
df.groupby("Product")["Quantity"].sum()
```

## 14. Top 3 customers

```python id="g4"
df.groupby("Customer")["TotalValue"].sum().sort_values(ascending=False).head(3)
```

## 15. Best product by sales

```python id="g5"
df.groupby("Product")["TotalValue"].sum().sort_values(ascending=False).head(1)
```

## 16. Most orders

```python id="g6"
df.groupby("Customer")["OrderID"].count().sort_values(ascending=False).head(1)
```

## 17. Revenue contribution %

```python id="g7"
total = df["TotalValue"].sum()

(df.groupby("Customer")["TotalValue"].sum() / total) * 100
```

# Section 4: Advanced Aggregation

## 18. Min, max, avg per product

```python id="a1"
df.groupby("Product")["TotalValue"].agg(["min", "max", "mean"])
```

## 19. Variance per customer

```python id="a2"
df.groupby("Customer")["TotalValue"].var()
```

## 20. Most consistent customer

```python id="a3"
df.groupby("Customer")["TotalValue"].std().sort_values()
```

## 21. Customer ranking

```python id="a4"
df["CustomerRank"] = df.groupby("Customer")["TotalValue"].transform("sum").rank(ascending=False)
```

## 22. Declining performance (concept)

If time existed:

```python id="a5"
df.sort_values("Date").groupby("Product")["TotalValue"].apply(list)
```

# Section 5: Business Logic

## 23. Top 20% customers

```python id="b1"
threshold = df.groupby("Customer")["TotalValue"].sum().quantile(0.8)

top_customers = df.groupby("Customer")["TotalValue"].sum()
top_customers[top_customers >= threshold]
```

## 24. One-time buyers

```python id="b2"
df.groupby("Customer")["OrderID"].count()[lambda x: x == 1]
```

## 25. Unusually high orders

```python id="b3"
df[df["TotalValue"] > df["TotalValue"].mean() + 2 * df["TotalValue"].std()]
```

## 26. High revenue, low quantity products

```python id="b4"
df.groupby("Product").agg({
    "TotalValue": "sum",
    "Quantity": "sum"
}).sort_values("TotalValue", ascending=False)
```

## 27. High buyers, low spending

```python id="b5"
df.groupby("Customer").agg({
    "Quantity": "sum",
    "TotalValue": "sum"
})
```

# Section 6: Multi-step Logic

## 28. Best customer-product combo

```python id="c1"
df.groupby(["Customer", "Product"])["TotalValue"].sum().sort_values(ascending=False).head(1)
```

## 29. Best product per customer

```python id="c2"
df.loc[df.groupby("Customer")["TotalValue"].idxmax()]
```

## 30. Order classification

```python id="c3"
df["Category"] = np.where(df["TotalValue"] > 80000, "High",
                  np.where(df["TotalValue"] > 40000, "Medium", "Low"))
```

## 31. Quantity vs revenue imbalance

```python id="c4"
df["Efficiency"] = df["TotalValue"] / df["Quantity"]
```

## 32. Top order contributor

```python id="c5"
df.loc[df["TotalValue"].idxmax()]
```

# Section 7: Analytical Thinking (answers conceptually)

## 33. Retention focus

Customers with:

- high total spend
- high frequency

```python id="d1"
df.groupby("Customer")["TotalValue"].sum().sort_values(ascending=False)
```

## 34. Best product to promote

Highest revenue but moderate quantity:

```python id="d2"
df.groupby("Product")["TotalValue"].sum().sort_values(ascending=False)
```

## 35. Quantity vs revenue

Check correlation:

```python id="d3"
df[["Quantity", "TotalValue"]].corr()
```

## 36. Discounts effect

Compare before/after:

```python id="d4"
df["TotalValue"].sum(), df["FinalAmount"].sum()
```

## 37. Most profitable segment

Customer grouping:

```python id="d5"
df.groupby("Customer")["FinalAmount"].sum()
```

# Section 8: Edge Cases

## 38. Missing values

```python id="e1"
df.fillna(df.mean(numeric_only=True))
```

## 39. Duplicates

```python id="e2"
df.drop_duplicates()
```

## 40. Invalid values

```python id="e3"
df[(df["Price"] <= 0) | (df["Quantity"] <= 0)]
```

## 41. Outliers

```python id="e4"
df[df["TotalValue"] > df["TotalValue"].mean() + 3 * df["TotalValue"].std()]
```

## 42. Business rule violation

```python id="e5"
df[(df["Quantity"] == 0) & (df["Price"] > 0)]
```

# What this exercise really teaches

If you understand this fully, you now know:

- Feature engineering
- Business-level filtering
- Group-based analytics
- Real-world decision logic
- Outlier detection
- Revenue analysis thinking

This is exactly what data analyst interviews test.
