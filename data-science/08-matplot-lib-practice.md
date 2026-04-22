# Matplotlib Practice Questions (Real-World Focus)

Assume you already have a dataset with columns like:

- Date
- Product
- Sales
- Quantity
- Customer
- Profit

```py

import pandas as pd
import numpy as np

np.random.seed(42)

# Dates (30 days)
dates = pd.date_range(start="2024-01-01", periods=30)

# Products
products = ["Laptop", "Mobile", "Tablet", "Headphones", "Smartwatch"]

# Customers
customers = ["A", "B", "C", "D", "E", "F"]

data = {
    "Date": np.random.choice(dates, 100),
    "Product": np.random.choice(products, 100),
    "Customer": np.random.choice(customers, 100),
    "Price": np.random.randint(5000, 90000, 100),
    "Quantity": np.random.randint(1, 5, 100),
    "Discount": np.random.randint(0, 30, 100)  # percentage
}

df = pd.DataFrame(data)

# Derived columns for analysis
df["Sales"] = df["Price"] * df["Quantity"]
df["Profit"] = df["Sales"] * (1 - df["Discount"] / 100)

df.head()


```

## Section 1: Basic Plots

1. Plot a line chart for Sales over time
2. Plot a bar chart of total Sales per Product
3. Plot a histogram of Sales distribution
4. Plot a scatter plot between Price and Quantity
5. Plot a pie chart of revenue contribution by Product

## Section 2: Customization (Very Important)

6. Add title, x-label, and y-label to a line chart
7. Change line color, style, and markers in a plot
8. Rotate x-axis labels for better readability
9. Add grid lines to a chart
10. Change figure size for better visualization

## Section 3: Multiple Plots

11. Create a subplot with Sales and Profit side by side
12. Compare Sales of two products in one chart
13. Plot multiple lines on the same graph (multi-series line chart)
14. Create subplots for different products’ sales trends
15. Compare monthly performance using multiple charts

## Section 4: Real Business Questions

16. Plot top 5 products by sales
17. Visualize customer-wise total spending
18. Show profit trend over time
19. Identify peak sales day using visualization
20. Compare quantity vs sales for insights

## Section 5: Advanced Visualization Thinking

21. Highlight highest sales point in a line chart
22. Annotate important data points in a graph
23. Create stacked bar chart for product category comparison
24. Plot cumulative sales over time
25. Visualize moving average trend of sales

## Section 6: Data Storytelling (Very Important)

26. Create a chart that shows which product is dominating revenue
27. Show relationship between discount and sales visually
28. Visualize customer segmentation based on spending
29. Show profit contribution vs sales contribution comparison
30. Build a dashboard-style multi-plot view

## Section 7: Interview-Level Practice

31. Explain why you chose a specific chart for a dataset
32. Identify misleading visualizations in a dataset
33. Convert raw data into meaningful visual insight
34. Choose best chart type for categorical vs numerical data
35. Combine two datasets and visualize comparison

## Section 8: Challenge Tasks

36. Recreate a real stock price chart using line plot
37. Visualize sales breakdown per region (multi-layer chart)
38. Create time-based trend analysis with annotations
39. Detect and visualize outliers in sales data
40. Build a complete report with multiple charts

# How to Practice

- Solve 3–5 questions daily
- Always think: “What story does this chart tell?”
- Avoid just plotting—focus on interpretation
- Combine with pandas for data preparation
