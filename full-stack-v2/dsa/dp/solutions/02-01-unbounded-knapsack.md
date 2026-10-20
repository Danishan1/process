Yes. These five are a great **Unbounded Knapsack family** because the major change from our previous subset-sum problems is:

 > **An item/coin can be used repeatedly.**

 Let's write **only the recursive solutions first**, keeping the style consistent with our `416` recursion.

---

 # 1\. LC 322 — Coin Change

 ### State

```
r(index, amount)
```

 Meaning:

 > Minimum number of coins needed to make `amount` using coins `0...index`.

 Because coins can be reused, when we **pick** a coin, we do **not** move to `index - 1`.

```
Pick     → r(index, amount - coin)
Don't pick → r(index - 1, amount)
```

 ### Recursive solution

```
var coinChange = function (coins, amount) {

    const r = (index, amount) => {
        if (amount === 0) return 0;
        if (index < 0) return Infinity;

        // Don't pick this coin
        const notPick =
            r(index - 1, amount);

        // Pick this coin again
        let pick = Infinity;

        if (coins[index] <= amount) {
            pick =
                1 +
                r(
                    index,
                    amount - coins[index]
                );
        }

        return Math.min(pick, notPick);
    };

    const result =
        r(coins.length - 1, amount);

    return result === Infinity ? -1 : result;
};
```

 The **critical line** is:

```
r(index, amount - coins[index])
```

 not:

```
r(index - 1, amount - coins[index])
```

 That's what makes it **unbounded**.

---

 # 2\. LC 518 — Coin Change II

 Now the question changes:

 > How many combinations can create the amount?

 So our state remains:

```
r(index, amount)
```

 but the return value changes.

 Instead of:

```
minimum
```

 we return:

```
number of combinations
```

 ### Recursive solution

```
var change = function (amount, coins) {

    const r = (index, amount) => {
        if (amount === 0) return 1;
        if (index < 0) return 0;

        // Don't pick this coin
        const notPick =
            r(index - 1, amount);

        // Pick this coin again
        let pick = 0;

        if (coins[index] <= amount) {
            pick =
                r(
                    index,
                    amount - coins[index]
                );
        }

        return pick + notPick;
    };

    return r(
        coins.length - 1,
        amount
    );
};
```

 Compare this with 322:

```
322:
return Math.min(pick, notPick)

518:
return pick + notPick
```

 Same recursion structure.

 Different question.

---

 # 3\. LC 279 — Perfect Squares

 This is basically **Coin Change with the coins generated dynamically**.

 Our "coins" are:

```
1, 4, 9, 16, 25, ...
```

 up to `n`.

 For example, if:

```
n = 12
```

 our available choices are:

```
1, 4, 9
```

 We want the **minimum number of squares**.

 ### Recursive solution

```
var numSquares = function (n) {

    const squares = [];

    for (let i = 1; i * i <= n; i++) {
        squares.push(i * i);
    }

    const r = (index, target) => {
        if (target === 0) return 0;
        if (index < 0) return Infinity;

        // Don't pick
        const notPick =
            r(index - 1, target);

        // Pick and reuse
        let pick = Infinity;

        if (squares[index] <= target) {
            pick =
                1 +
                r(
                    index,
                    target - squares[index]
                );
        }

        return Math.min(
            pick,
            notPick
        );
    };

    return r(
        squares.length - 1,
        n
    );
};
```

 Look at how close this is to `322`.

 The only real difference is where the "coins" come from.

```
322:
coins = input array

279:
coins = 1², 2², 3², ...
```

---

 # 4\. LC 1449 — Form Largest Integer With Digits That Add up to Target

 This one is particularly interesting.

 We have digits:

```
1 → 9
```

 Each digit has a cost.

 We can use each digit **repeatedly**.

 We want:

 > The largest possible integer whose total cost equals `target`.

 So again:

```
unbounded
```

 because a digit can be used multiple times.

 Our state:

```
r(index, target)
```

 But now the return isn't simply a number.

 We want the **best string**.

 ### Recursive solution

```
var largestNumber = function (cost, target) {

    const digits = 9;

    const r = (index, target) => {

        if (target === 0) {
            return "";
        }

        if (index < 0) {
            return null;
        }

        // Don't pick this digit
        const notPick =
            r(index - 1, target);

        // Pick this digit again
        let pick = null;

        if (cost[index] <= target) {
            const remaining =
                r(
                    index,
                    target - cost[index]
                );

            if (remaining !== null) {
                pick =
                    String(index + 1) +
                    remaining;
            }
        }

        if (pick === null) return notPick;
        if (notPick === null) return pick;

        // Prefer more digits.
        if (pick.length !== notPick.length) {
            return pick.length > notPick.length
                ? pick
                : notPick;
        }

        // Same length:
        // lexicographically larger = larger number.
        return pick > notPick
            ? pick
            : notPick;
    };

    const result =
        r(digits - 1, target);

    return result === null ? "0" : result;
};
```

 There is an important conceptual difference here.

 For `322`:

```
return minimum number of coins
```

 For `1449`:

```
return best NUMBER STRING
```

 But the recursive structure is still:

```
                 current digit
                 /           \
              PICK          DON'T PICK
                |                |
          same index         index - 1
```

---

 # 5\. LC 377 — Combination Sum IV

 This one looks very similar to `518`, but there's a **critical difference**:

 > **Order matters.**

 For:

```
nums = [1, 2]
target = 3
```

 we count:

```
1 + 1 + 1
1 + 2
2 + 1
```

 So `1 + 2` and `2 + 1` are different.

 Because we're allowed to choose **any number at every step**, the recursive state can simply be:

```
r(target)
```

 We don't need an `index`.

 ### Recursive solution

```
var combinationSum4 = function (nums, target) {

    const r = (target) => {
        if (target === 0) return 1;
        if (target < 0) return 0;

        let ways = 0;

        for (const num of nums) {
            ways += r(target - num);
        }

        return ways;
    };

    return r(target);
};
```

 This is a beautiful example of how the **state can become smaller** when order matters.

---

 # Now compare all five

 | Problem | State | Pick transition | Don't Pick | Return |
| --- | --- | --- | --- | --- |
| **322** Coin Change | `(index, amount)` | `r(index, amount-coin)` | `r(index-1, amount)` | `MIN` |
| **518** Coin Change II | `(index, amount)` | `r(index, amount-coin)` | `r(index-1, amount)` | `SUM` |
| **279** Perfect Squares | `(index, target)` | `r(index, target-square)` | `r(index-1, target)` | `MIN` |
| **1449** Largest Integer | `(index, target)` | `r(index, target-cost)` | `r(index-1, target)` | `BEST` |
| **377** Combination Sum IV | `(target)` | `r(target-num)` | — | `SUM` |

---

 # The really important comparison with 416

 Remember our 416 recursion:

```
const r = (index, target) => {
    if (target === 0) return true;
    if (index < 0) return false;

    return (
        r(index - 1, target - nums[index]) ||
        r(index - 1, target)
    );
};
```

 The **0/1** part is:

```
r(index - 1, ...)
```

 When we pick the item, we move to the previous index.

---

 For unbounded knapsack:

```
r(index, target - item)
```

 We **stay at the same index** after picking.

 So this tiny difference:

```
0/1 Knapsack:

PICK
 ↓
index - 1

Unbounded Knapsack:

PICK
 ↓
same index
```

 is one of the most important things to recognize in DP.

---

 # And now the 5 problems form a very nice progression

```
416
Partition Equal Subset Sum
        │
        │ change:
        │ item can be reused
        ↓
322
Coin Change
        │
        │ change:
        │ ask for number of ways
        ↓
518
Coin Change II
        │
        │ change:
        │ choices themselves are generated
        ↓
279
Perfect Squares
        │
        │ change:
        │ return a complex "best" object
        ↓
1449
Largest Integer
        │
        │ change:
        │ order matters
        ↓
377
Combination Sum IV
```

 This is an excellent set to study **before** jumping into the bottom-up versions, because you'll get to see that the recursion doesn't fundamentally change. What changes is:

```
STATE
CHOICES
RETURN VALUE
COMBINATION OPERATION
```

 And especially:

```
0/1:
pick → index - 1

UNBOUNDED:
pick → same index
```

 That distinction will become **very important when we derive the final 1D bottom-up solutions**, because it directly explains why some loops go **backward** and others go **forward**.