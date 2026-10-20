
### 1. 1D / Fibonacci-style DP

| LeetCode # | Title                    | Small Summary                                            |
| ---------: | ------------------------ | -------------------------------------------------------- |
|     **70** | Climbing Stairs          | At each step choose 1 or 2 steps.                        |
|    **746** | Min Cost Climbing Stairs | Reach the top with minimum accumulated cost.             |
|    **198** | House Robber             | Take/skip each house; cannot take adjacent houses.       |
|    **213** | House Robber II          | Same as House Robber, but houses form a circle.          |
|     **91** | Decode Ways              | At each position, decode 1 digit or 2 digits when valid. |
|    **509** | Fibonacci Number         | Basic recurrence and memoization foundation.             |

### 2. Take / Not Take — Subset DP

This is the **most important pattern to master first**.

| LeetCode # | Title                            | Small Summary                                               |
| ---------: | -------------------------------- | ----------------------------------------------------------- |
|    **416** | Partition Equal Subset Sum       | Can the array be divided into two subsets with equal sum?   |
|    **494** | Target Sum                       | Assign `+` or `-` to every number to reach a target.        |
|   **1049** | Last Stone Weight II             | Partition stones to minimize the remaining difference.      |
|    **474** | Ones and Zeroes                  | Select maximum strings within limited `0` and `1` capacity. |
|    **698** | Partition to K Equal Sum Subsets | Divide array into `k` subsets having equal sum.             |
|   **2597** | The Number of Beautiful Subsets  | Choose subsets while respecting a difference constraint.    |

**Core recursion:**

```text
solve(index, target)

       /       \
    TAKE      NOT TAKE
```

### 3. 0/1 Knapsack

The item can be used **at most once**.

| LeetCode # | Title                      | Small Summary                                       |
| ---------: | -------------------------- | --------------------------------------------------- |
|    **474** | Ones and Zeroes            | Each string consumes two resources: zeros and ones. |
|    **879** | Profitable Schemes         | Select jobs under member/profit constraints.        |
|    **416** | Partition Equal Subset Sum | Treat numbers as items with a target capacity.      |
|   **1049** | Last Stone Weight II       | Convert stone partitioning into a 0/1 knapsack.     |
|    **956** | Tallest Billboard          | Partition rods into two groups with equal height.   |

The important recursive transition:

```js
take =
    value[index] +
    solve(index + 1, capacity - weight[index]);

notTake =
    solve(index + 1, capacity);
```

### 4. Unbounded Knapsack

Here an item can be selected **multiple times**.

| LeetCode # | Title                                                  | Small Summary                                      |
| ---------: | ------------------------------------------------------ | -------------------------------------------------- |
|    **322** | Coin Change                                            | Minimum number of coins needed for an amount.      |
|    **518** | Coin Change II                                         | Number of combinations to create an amount.        |
|    **279** | Perfect Squares                                        | Minimum number of square numbers whose sum is `n`. |
|   **1449** | Form Largest Integer With Digits That Add up to Target | Repeatedly use digits to maximize a number.        |
|    **377** | Combination Sum IV                                     | Count ordered combinations that produce target.    |

The critical difference:

```js
// 0/1
solve(index + 1, remaining);

// Unbounded
solve(index, remaining);
```

That single difference is **very important for interviews**.

### 5. Grid DP

| LeetCode # | Title                          | Small Summary                                 |
| ---------: | ------------------------------ | --------------------------------------------- |
|     **62** | Unique Paths                   | Count paths from top-left to bottom-right.    |
|     **63** | Unique Paths II                | Same, but obstacles exist.                    |
|     **64** | Minimum Path Sum               | Find minimum-cost path through the grid.      |
|    **120** | Triangle                       | Find minimum path sum through a triangle.     |
|    **931** | Minimum Falling Path Sum       | Move downward with three possible directions. |
|    **221** | Maximal Square                 | Find the largest square consisting of `1`s.   |
|   **1301** | Number of Paths with Max Score | Maximum score + number of ways to achieve it. |

Core state:

```text
solve(row, col)
```

Typical choices:

```text
        current
       /       \
    DOWN      RIGHT
```

### 6. String DP — `i, j`

These are extremely important for interviews.

| LeetCode # | Title                                    | Small Summary                                                               |
| ---------: | ---------------------------------------- | --------------------------------------------------------------------------- |
|   **1143** | Longest Common Subsequence               | Find longest common subsequence between two strings.                        |
|    **583** | Delete Operation for Two Strings         | Minimum deletions to make two strings equal.                                |
|    **712** | Minimum ASCII Delete Sum for Two Strings | Delete characters with minimum total ASCII cost.                            |
|     **72** | Edit Distance                            | Insert, delete, or replace characters to transform one string into another. |
|   **1092** | Shortest Common Supersequence            | Construct shortest string containing both strings.                          |
|    **115** | Distinct Subsequences                    | Count ways one string can form another as a subsequence.                    |

Core state:

```text
solve(i, j)
```

If characters match:

```js
solve(i + 1, j + 1);
```

Otherwise:

```text
skip i
skip j
```

or, depending on the problem:

```text
insert
delete
replace
```

### 7. Pick / Skip

This is closely related to Take/Not-Take but usually has a simple `index` state.

| LeetCode # | Title                    | Small Summary                                               |
| ---------: | ------------------------ | ----------------------------------------------------------- |
|    **198** | House Robber             | Rob or skip a house; adjacent houses cannot both be robbed. |
|    **213** | House Robber II          | Same problem with circular arrangement.                     |
|    **740** | Delete and Earn          | Taking one number prevents taking adjacent values.          |
|    **337** | House Robber III         | Rob tree nodes without robbing parent and child together.   |
|    **983** | Minimum Cost For Tickets | At each day, choose among different ticket durations.       |

The basic pattern:

```js
const skip = solve(index + 1);

const take =
    nums[index] + solve(index + 2);

return Math.max(take, skip);
```

### 8. Partition DP

This is a different family and worth learning separately.

| LeetCode # | Title                        | Small Summary                                                              |
| ---------: | ---------------------------- | -------------------------------------------------------------------------- |
|    **132** | Palindrome Partitioning II   | Minimum cuts required to partition a string into palindromes.              |
|    **312** | Burst Balloons               | Choose the last balloon to burst within an interval.                       |
|   **1547** | Minimum Cost to Cut a Stick  | Choose the order of cuts minimizing total cost.                            |
|   **1000** | Minimum Cost to Merge Stones | Merge groups of stones with minimum total cost.                            |
|    **664** | Strange Printer              | Divide string into intervals to minimize printing operations.              |
|     **87** | Scramble String              | Determine whether one string can be formed through recursive partitioning. |

Core structure:

```js
function solve(left, right) {

    let answer = Infinity;

    for (let k = left; k < right; k++) {

        const leftResult = solve(left, k);

        const rightResult = solve(k + 1, right);

        // combine results

        answer = Math.min(answer, result);
    }

    return answer;
}
```

# Recommended DP Revision Order

Don't solve them randomly. I'd do them in this order:

| Order | Pattern              | Questions to Focus On |
| ----: | -------------------- | --------------------- |
| **1** | 1D DP                | 70, 746, 198          |
| **2** | Take / Not Take      | 416, 494, 1049        |
| **3** | 0/1 Knapsack         | 474, 879              |
| **4** | Unbounded Knapsack   | 322, 518, 279         |
| **5** | Grid DP              | 62, 63, 64, 120       |
| **6** | String DP            | 1143, 72, 115         |
| **7** | Pick / Skip variants | 740, 337, 983         |
| **8** | Partition DP         | 132, 312, 1547        |

### ⭐ If your goal is interview revision

You don't need to solve every question above immediately.

I'd use this **20-question core set**:

**70 → 746 → 198 → 416 → 494 → 1049 → 474 → 322 → 518 → 279 → 62 → 63 → 64 → 120 → 1143 → 72 → 115 → 740 → 132 → 312**

If you can look at these 20 and correctly identify **state + choices + base case + recurrence**, you'll have a much clearer understanding of the major recursive DP patterns.
