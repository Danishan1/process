Exactly. Now let's take those **array-based memoized recursive solutions** and derive the **final space-optimized bottom-up solutions**.

 The big transition is:

```
memo[index][target]
        ↓
dp[index][target]
        ↓
dp[target]
```

 For the unbounded problems, you'll notice something important:

 > **The loop goes FORWARD**, because we are allowed to reuse the same item.

---

 # 1\. LC 322 — Coin Change

 ### Final optimized

```
var coinChange = function (coins, amount) {
    const dp = Array(amount + 1).fill(Infinity);

    dp[0] = 0;

    for (const coin of coins) {
        for (let target = coin; target <= amount; target++) {
            dp[target] = Math.min(
                dp[target],
                1 + dp[target - coin]
            );
        }
    }

    return dp[amount] === Infinity
        ? -1
        : dp[amount];
};
```

 ### Why forward?

```
for (let target = coin; target <= amount; target++)
```

 Because:

```
dp[target - coin]
```

 may have been updated **by the same coin**.

 That's exactly what we want.

 Example:

```
coin = 2

dp[2]  → use 2 once
dp[4]  → use 2 again
dp[6]  → use 2 again
```

 So:

```
FORWARD = reuse allowed
```

---

 # 2\. LC 518 — Coin Change II

 Here we want **combinations**, not the minimum.

```
var change = function (amount, coins) {
    const dp = Array(amount + 1).fill(0);

    dp[0] = 1;

    for (const coin of coins) {
        for (let target = coin; target <= amount; target++) {
            dp[target] += dp[target - coin];
        }
    }

    return dp[amount];
};
```

 The key difference from `322`:

```
322:
dp[target] = min(...)

518:
dp[target] += ...
```

---

 ## Why does the coin loop come first?

 This is important for **combinations**.

```
for (const coin of coins) {
    for (let target = coin; target <= amount; target++) {
        ...
    }
}
```

 This ensures:

```
1 + 2
```

 and:

```
2 + 1
```

 are considered the **same combination**.

---

 # 3\. LC 279 — Perfect Squares

 This is essentially `322` where the coins are:

```
1², 2², 3², ...
```

 ### Final optimized

```
var numSquares = function (n) {
    const dp = Array(n + 1).fill(Infinity);

    dp[0] = 0;

    for (let square = 1; square * square <= n; square++) {
        const value = square * square;

        for (let target = value; target <= n; target++) {
            dp[target] = Math.min(
                dp[target],
                1 + dp[target - value]
            );
        }
    }

    return dp[n];
};
```

 Again:

```
Unbounded
    ↓
forward loop
```

 because:

```
4 can be used multiple times
9 can be used multiple times
16 can be used multiple times
...
```

---

 # 4\. LC 1449 — Form Largest Integer

 This is the more interesting one because our DP value is a **string**.

```
var largestNumber = function (cost, target) {
    const dp = Array(target + 1).fill(null);

    dp[0] = "";

    for (let digit = 1; digit <= 9; digit++) {
        const price = cost[digit - 1];

        for (let t = price; t <= target; t++) {
            if (dp[t - price] === null) {
                continue;
            }

            const candidate =
                String(digit) + dp[t - price];

            if (
                dp[t] === null ||
                candidate.length > dp[t].length ||
                (
                    candidate.length === dp[t].length &&
                    candidate > dp[t]
                )
            ) {
                dp[t] = candidate;
            }
        }
    }

    return dp[target] === null
        ? "0"
        : dp[target];
};
```

 Again:

```
for (let t = price; t <= target; t++)
```

 is forward because the same digit can be used repeatedly.

---

 # 5\. LC 377 — Combination Sum IV

 This one is special because **order matters**.

 Therefore we reverse the loop nesting.

```
var combinationSum4 = function (nums, target) {
    const dp = Array(target + 1).fill(0);

    dp[0] = 1;

    for (let t = 1; t <= target; t++) {
        for (const num of nums) {
            if (num <= t) {
                dp[t] += dp[t - num];
            }
        }
    }

    return dp[target];
};
```

 Compare this with `518`.

 ### 518 — combinations

```
for (const coin of coins) {
    for (let target = coin; target <= amount; target++) {
        dp[target] += dp[target - coin];
    }
}
```

 ### 377 — permutations

```
for (let target = 1; target <= amount; target++) {
    for (const num of nums) {
        dp[target] += dp[target - num];
    }
}
```

 That tiny difference is **extremely important**.

---

 # Final comparison

 | LC | Problem | DP Meaning | Loop Direction | Loop Order |
| --- | --- | --- | --- | --- |
| **322** | Coin Change | Minimum coins | Forward | Coin → Target |
| **518** | Coin Change II | Number of combinations | Forward | Coin → Target |
| **279** | Perfect Squares | Minimum squares | Forward | Square → Target |
| **1449** | Largest Integer | Best number | Forward | Digit → Target |
| **377** | Combination Sum IV | Number of ordered combinations | Forward | Target → Number |

---

 # The pattern you should remember

 For **0/1 knapsack**:

```
for (const item of items) {
    for (let target = capacity; target >= item; target--) {
        ...
    }
}
```

 ### Backward

```
item can be used once
```

 For **unbounded knapsack**:

```
for (const item of items) {
    for (let target = item; target <= capacity; target++) {
        ...
    }
}
```

 ### Forward

```
item can be reused
```

 And for **ordered combinations** like LC 377:

```
for (let target = 1; target <= capacity; target++) {
    for (const item of items) {
        ...
    }
}
```

 So now you have three major patterns:

```
┌─────────────────────────────┐
│       0/1 KNAPSACK          │
│                             │
│ item → target               │
│ target BACKWARD             │
│                             │
│ item used ONCE              │
└─────────────────────────────┘

┌─────────────────────────────┐
│     UNBOUNDED KNAPSACK      │
│                             │
│ item → target               │
│ target FORWARD              │
│                             │
│ item can REPEAT             │
└─────────────────────────────┘

┌─────────────────────────────┐
│    ORDERED COMBINATIONS     │
│                             │
│ target → item               │
│ target FORWARD              │
│                             │
│ ORDER MATTERS               │
└─────────────────────────────┘
```

 This is the real payoff of solving these five from **recursive → memoized → bottom-up → optimized**. You're no longer memorizing five solutions; you're recognizing the **DP family and the reason behind the loops**.