
 | Order | LeetCode # | Problem | Pattern | Main Concept |
| --- | --- | --- | --- | --- |
| 1 | **416** | Partition Equal Subset Sum | 0/1 Subset Sum | Pick/skip + Boolean DP |
| 2 | **1049** | Last Stone Weight II | 0/1 Knapsack | Pick/skip + Optimization |
| 3 | **494** | Target Sum | Subset / +/- | Counting DP |
| 4 | **474** | Ones and Zeroes | 0/1 Knapsack | 2D capacity |
| 5 | **879** | Profitable Schemes | 0/1 Knapsack | 2D capacity + Counting |
| 6 | **322** | Coin Change | Unbounded Knapsack | Minimum / Reuse items |
| 7 | **518** | Coin Change II | Unbounded Knapsack | Counting + Reuse items |
| 8 | **377** | Combination Sum IV | Unbounded DP | Counting + Order matters |
| 9 | **2787** | Ways to Express an Integer as Sum of Powers | 0/1 Subset Sum | Distinct items + Counting |
| 10 | **2035** | Partition Array Into Two Arrays to Minimize Sum Difference | Partition | Meet-in-the-middle |
| 11 | **698** | Partition to K Equal Sum Subsets | Partition | Backtracking + Bitmask DP |
| 12 | **956** | Tallest Billboard | Difference DP | Difference state + Optimization |
| 13 | **2597** | The Number of Beautiful Subsets | Constrained Subsets | State design |
| 14 | **1986** | Minimum Number of Work Sessions | Bitmask DP | Subset/state compression |
| 15 | **1723** | Find Minimum Time to Finish All Jobs | Partition | Bitmask + Optimization |

### Recommended progression

 | Stage | Problems | What to practice |
| --- | --- | --- |
| 🟢 **Foundation** | 416 → 1049 → 494 | Recursive → Memo → Bottom-up → 1D |
| 🟡 **Multi-dimensional** | 474 → 879 | Multiple state variables |
| 🟠 **Unbounded** | 322 → 518 → 377 | Forward vs backward loops |
| 🔵 **Advanced Subset** | 2787 → 2035 | Distinct selection / meet-in-the-middle |
| 🔴 **Advanced State** | 698 → 956 → 2597 | Bitmask / difference / custom state |

For **each problem**, use the same workflow:

```
Recursive
   ↓
Memoized Recursive
   ↓
2D/3D Bottom-Up
   ↓
Space Optimized
   ↓
Understand initialization
   ↓
Understand loop direction
```

 The **first five** are the most important for the framework you're currently learning.