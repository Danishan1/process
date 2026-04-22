# Real-World Practice Set: E-commerce Dataset

Assume a dataset with columns:

- OrderID
- Customer
- Product
- Price
- Quantity
- Date

You may use NumPy and pandas as needed.

```py

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
df


```

## Section 1: Feature Engineering (NumPy + Pandas)

1. Create a new column for total order value (Price × Quantity)
2. Create a discount column:
   - 10% if total value > 50,000
   - 5% otherwise

3. Create final payable amount after discount
4. Create a flag column for high-value orders (based on mean order value)
5. Normalize order values using statistical scaling

## Section 2: Filtering & Conditions

6. Find all orders where quantity is greater than 2
7. Find orders where total value is above average order value
8. Filter orders from a specific customer who spent more than X
9. Find products sold in quantities greater than dataset average
10. Identify orders where price is above product average price

## Section 3: GroupBy Analysis (Core Interview Area)

11. Find total revenue per customer
12. Find average order value per product
13. Find total quantity sold per product
14. Identify top 3 customers by revenue
15. Find product with highest total sales
16. Find customer with most number of orders
17. Compute revenue contribution percentage per customer

## Section 4: Advanced Aggregations

18. Find max, min, and average order value per product
19. Find variance in spending per customer
20. Identify most consistent customer (least variation in spending)
21. Rank customers based on total spending
22. Identify products with declining performance (if time is considered)

## Section 5: Real Business Logic

23. Identify high-value customers (top 20% spenders)
24. Find customers who only made one purchase
25. Detect orders that are unusually high compared to average
26. Identify products that generate revenue but have low quantity
27. Find customers who buy multiple products but low total spending

## Section 6: Multi-Step Thinking Problems

28. Find customer-product combination generating highest revenue
29. Identify best performing product in each customer segment
30. Find revenue per order and classify into categories
31. Detect imbalance between quantity sold and revenue generated
32. Find top contributing order to total revenue

## Section 7: Analytical Thinking (Important)

33. Which customer should the company focus retention efforts on?
34. Which product should be promoted based on sales distribution?
35. Are high quantity orders always high revenue orders?
36. Do discounts improve or reduce revenue?
37. Which segment of customers is most profitable?

## Section 8: Edge Case Thinking

38. Handle missing values in price or quantity before analysis
39. Identify duplicate orders and remove them logically
40. Find inconsistent data entries (negative or zero values)
41. Detect outliers in order values
42. Find rows that violate business rules (e.g., quantity = 0 but price > 0)

## Section 9: Optional Advanced (If You Want Challenge)

43. Simulate monthly sales grouping and analyze trends
44. Create rolling average of order values
45. Compare performance of customers over time
46. Detect sudden spikes in product sales
47. Rank products dynamically based on time window

# How to Practice

- Pick 5–7 questions per day
- Try solving without looking at any reference
- Revisit unsolved ones after 1–2 days
- Focus on logic, not syntax memorization
