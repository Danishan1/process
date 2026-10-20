Absolutely. Let's rewrite all five using **array-based memoization**, keeping the recursive structure as close as possible to the original recursion.

 The key idea is:

```
Recursive state:
r(index, target)

Memo:
memo[index][target]
```

 For `377`, the state is only `target`, so we need only a 1D array.

 ## 1\. LeetCode 322 — Coin Change

 **Unbounded + minimum**

```
var coinChange = function (coins, amount) {
    const n = coins.length;

    const memo = Array.from(
        { length: n },
        () => Array(amount + 1).fill(undefined)
    );

    const r = (index, target) => {
        if (target === 0) return 0;
        if (index < 0) return Infinity;

        if (memo[index][target] !== undefined) {
            return memo[index][target];
        }

        const notPick = r(index - 1, target);

        let pick = Infinity;

        if (coins[index] <= target) {
            pick = 1 + r(
                index,
                target - coins[index]
            );
        }

        memo[index][target] =
            Math.min(pick, notPick);

        return memo[index][target];
    };

    const result = r(n - 1, amount);

    return result === Infinity ? -1 : result;
};
```

 Notice:

```
pick = r(index, target - coins[index]);
```

 Same `index` → **coin can be reused**.

---

 ## 2\. LeetCode 518 — Coin Change II

 **Unbounded + counting**

```
var change = function (amount, coins) {
    const n = coins.length;

    const memo = Array.from(
        { length: n },
        () => Array(amount + 1).fill(undefined)
    );

    const r = (index, target) => {
        if (target === 0) return 1;
        if (index < 0) return 0;

        if (memo[index][target] !== undefined) {
            return memo[index][target];
        }

        const notPick = r(index - 1, target);

        let pick = 0;

        if (coins[index] <= target) {
            pick = r(
                index,
                target - coins[index]
            );
        }

        memo[index][target] =
            pick + notPick;

        return memo[index][target];
    };

    return r(n - 1, amount);
};
```

 The recursion is almost identical to `322`.

 Only the **meaning of the answer** changes:

```
322 → minimum
518 → number of ways
```

---

 ## 3\. LeetCode 279 — Perfect Squares

 Here we first create our "coins":

```
1, 4, 9, 16, ...
```

 Then it's essentially the same recursion as `322`.

```
var numSquares = function (n) {
    const squares = [];

    for (let i = 1; i * i <= n; i++) {
        squares.push(i * i);
    }

    const size = squares.length;

    const memo = Array.from(
        { length: size },
        () => Array(n + 1).fill(undefined)
    );

    const r = (index, target) => {
        if (target === 0) return 0;
        if (index < 0) return Infinity;

        if (memo[index][target] !== undefined) {
            return memo[index][target];
        }

        const notPick =
            r(index - 1, target);

        let pick = Infinity;

        if (squares[index] <= target) {
            pick = 1 + r(
                index,
                target - squares[index]
            );
        }

        memo[index][target] =
            Math.min(pick, notPick);

        return memo[index][target];
    };

    return r(size - 1, n);
};
```

 Again:

```
r(index, target - squares[index])
```

 because a square can be used repeatedly.

---

 ## 4\. LeetCode 1449 — Form Largest Integer

 Here the DP state is still:

```
(index, target)
```

 but the answer is a **string** rather than a number.

```
var largestNumber = function (cost, target) {
    const n = cost.length;

    const memo = Array.from(
        { length: n },
        () => Array(target + 1).fill(undefined)
    );

    const r = (index, target) => {
        if (target === 0) return "";
        if (index < 0) return null;

        if (memo[index][target] !== undefined) {
            return memo[index][target];
        }

        const notPick =
            r(index - 1, target);

        let pick = null;

        if (cost[index] <= target) {
            const remaining = r(
                index,
                target - cost[index]
            );

            if (remaining !== null) {
                pick =
                    String(index + 1) +
                    remaining;
            }
        }

        let result;

        if (pick === null) {
            result = notPick;
        } else if (notPick === null) {
            result = pick;
        } else if (pick.length !== notPick.length) {
            result =
                pick.length > notPick.length
                    ? pick
                    : notPick;
        } else {
            result =
                pick > notPick
                    ? pick
                    : notPick;
        }

        memo[index][target] = result;

        return result;
    };

    const result = r(n - 1, target);

    return result === null ? "0" : result;
};
```

 Here:

```
memo[index][target]
```

 stores:

 > The largest number that can be constructed using digits `0...index` with exactly `target` cost.

---

 ## 5\. LeetCode 377 — Combination Sum IV

 This one is different.

 There is no `index` in our recursion:

```
r(target)
```

 Therefore we only need:

```
memo[target]
```

```
var combinationSum4 = function (nums, target) {
    const memo = Array(target + 1).fill(undefined);

    const r = (target) => {
        if (target === 0) return 1;
        if (target < 0) return 0;

        if (memo[target] !== undefined) {
            return memo[target];
        }

        let ways = 0;

        for (const num of nums) {
            ways += r(target - num);
        }

        memo[target] = ways;

        return ways;
    };

    return r(target);
};
```

---

 # The five together

 | LC | State | Memo | Pick | Result |
| --- | --- | --- | --- | --- |
| **322** | `(index, target)` | `memo[index][target]` | Same `index` | `MIN` |
| **518** | `(index, target)` | `memo[index][target]` | Same `index` | `SUM` |
| **279** | `(index, target)` | `memo[index][target]` | Same `index` | `MIN` |
| **1449** | `(index, target)` | `memo[index][target]` | Same `index` | `BEST` |
| **377** | `(target)` | `memo[target]` | `target - num` | `SUM` |

 And the central distinction from our **0/1 subset-sum** problems remains:

```
0/1 Knapsack

pick:
r(index - 1, target - item)
          ↑
       move back
```

 versus:

```
Unbounded Knapsack

pick:
r(index, target - item)
       ↑
   stay here
```

 That one line is what you should pay the most attention to when we eventually convert these five from **memoized recursion → bottom-up → final 1D optimization**.