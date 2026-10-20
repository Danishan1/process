Yes. Now we take the bottom-up versions and push them to their **final practical space-optimized forms**.

 The rule is:

 > If `dp[i]` only depends on `dp[i - 1]`, remove the `i` dimension.

 For 0/1 subset problems, this usually gives a **1D DP array**.

---

 # 1\. 416 — Partition Equal Subset Sum

 From:

```
dp[i][target]
```

 to:

```
dp[target]
```

 We need to process targets **backwards**.

 Why?

 Because each number can be used only once.

```
var canPartition = function (nums) {
    const total = nums.reduce((sum, num) => sum + num, 0);

    if (total % 2 !== 0) return false;

    const target = total / 2;

    const dp = Array(target + 1).fill(false);

    dp[0] = true;

    for (const num of nums) {
        for (let t = target; t >= num; t--) {
            dp[t] = dp[t] || dp[t - num];
        }
    }

    return dp[target];
};
```

 ### Complexity

```
Time:  O(n × target)
Space: O(target)
```

 The important part:

```
for (let t = target; t >= num; t--)
```

 **Backward = 0/1 usage.**

---

 # 2\. 494 — Target Sum

 Here our state is:

```
(index, sum)
```

 and we need the number of ways.

 Because `sum` can be negative, we use an offset.

 We don't actually need the `index` dimension because every row only depends on the previous row.

```
var findTargetSumWays = function (nums, target) {
    const total = nums.reduce((sum, num) => sum + num, 0);

    if (Math.abs(target) > total) return 0;

    const offset = total;
    const size = 2 * total + 1;

    let dp = Array(size).fill(0);

    dp[offset] = 1;

    for (const num of nums) {
        const next = Array(size).fill(0);

        for (let sum = -total; sum <= total; sum++) {
            const ways = dp[sum + offset];

            if (ways === 0) continue;

            next[sum + num + offset] += ways;
            next[sum - num + offset] += ways;
        }

        dp = next;
    }

    return dp[target + offset];
};
```

 ### Complexity

```
Time:  O(n × total)
Space: O(total)
```

 Here we use a separate `next` array because the two transitions:

```
sum + num
sum - num
```

 must both come from the **previous item**.

---

 # 3\. 1049 — Last Stone Weight II

 This is basically the same 0/1 subset-sum structure as 416, except our DP stores the **best sum**, not a boolean.

```
var lastStoneWeightII = function (stones) {
    const total = stones.reduce(
        (sum, stone) => sum + stone,
        0
    );

    const target = Math.floor(total / 2);

    const dp = Array(target + 1).fill(0);

    for (const stone of stones) {
        for (let t = target; t >= stone; t--) {
            dp[t] = Math.max(
                dp[t],
                stone + dp[t - stone]
            );
        }
    }

    return total - 2 * dp[target];
};
```

 Look at the similarity:

 ### 416

```
dp[t] = dp[t] || dp[t - num];
```

 ### 1049

```
dp[t] = Math.max(
    dp[t],
    num + dp[t - num]
);
```

 Same DP structure.

 Different **meaning of the cell**.

---

 # 4\. 474 — Ones and Zeroes

 Now we have two capacities:

```
zeros
ones
```

 So after removing the `index` dimension, we get:

```
dp[zeros][ones]
```

 Because each string can be used once, **both dimensions must go backward**.

```
var findMaxForm = function (strs, m, n) {
    const dp = Array.from(
        { length: m + 1 },
        () => Array(n + 1).fill(0)
    );

    for (const str of strs) {
        let zeros = 0;

        for (const ch of str) {
            if (ch === "0") {
                zeros++;
            }
        }

        const ones = str.length - zeros;

        for (let z = m; z >= zeros; z--) {
            for (let o = n; o >= ones; o--) {
                dp[z][o] = Math.max(
                    dp[z][o],
                    1 + dp[z - zeros][o - ones]
                );
            }
        }
    }

    return dp[m][n];
};
```

 This is the same 0/1 knapsack principle, just with **two capacities**.

```
One capacity:

dp[target]

Two capacities:

dp[zeros][ones]
```

---

 # 5\. 879 — Profitable Schemes

 Again, after removing the job/index dimension:

```
dp[members][profit]
```

 The value is:

```
number of ways
```

 We process members backward because a job can only be selected once.

```
var profitableSchemes = function (
    n,
    minProfit,
    group,
    profit
) {
    const MOD = 1_000_000_007;

    const dp = Array.from(
        { length: n + 1 },
        () => Array(minProfit + 1).fill(0)
    );

    dp[0][0] = 1;

    for (let i = 0; i < group.length; i++) {
        const members = group[i];
        const earned = profit[i];

        for (let m = n; m >= members; m--) {
            for (let p = minProfit; p >= 0; p--) {

                const previousProfit =
                    Math.max(
                        0,
                        p - earned
                    );

                dp[m][p] = (
                    dp[m][p] +
                    dp[m - members][previousProfit]
                ) % MOD;
            }
        }
    }

    return dp[n][minProfit];
};
```

 The key is:

```
dp[m][p]
```

 means:

 > Number of ways to achieve at least `p` profit using at most `m` members.

 And:

```
Math.max(0, p - earned)
```

 handles the "minimum profit" nature of the problem.

---

 # 6\. 698 — Partition to K Equal Sum Subsets

 This one is **not a normal knapsack compression**.

 The clean optimized iterative solution uses a bitmask:

```
dp[mask]
```

 where `mask` tells us which numbers have already been used.

 The value stored is:

```
current sum of the bucket we're building
```

```
var canPartitionKSubsets = function (nums, k) {
    const total = nums.reduce(
        (sum, num) => sum + num,
        0
    );

    if (total % k !== 0) return false;

    const target = total / k;
    const n = nums.length;

    const dp = Array(1 << n).fill(-1);

    dp[0] = 0;

    for (let mask = 0; mask < (1 << n); mask++) {
        if (dp[mask] === -1) continue;

        const currentSum = dp[mask];

        for (let i = 0; i < n; i++) {
            if (mask & (1 << i)) continue;

            const nextSum =
                currentSum + nums[i];

            if (nextSum > target) continue;

            const nextMask =
                mask | (1 << i);

            dp[nextMask] =
                nextSum % target;
        }
    }

    return dp[(1 << n) - 1] === 0;
};
```

 Complexity:

```
Time:  O(2^n × n)
Space: O(2^n)
```

 This is fundamentally different from the `416` style of compression.

---

 # 7\. 2597 — Beautiful Subsets

 For this one, I would **not use our earlier Set-based recursive state as the basis for a "final optimized" DP**.

 That recursive state:

```
(index, selected Set)
```

 doesn't collapse naturally into a useful small-dimensional array.

 The optimized solution exploits the fact that the forbidden difference is exactly `k`, grouping numbers by:

```
num % k
```

 and then doing a frequency-based DP.

 So this is an example where:

```
recursive solution
        ↓
memoized solution
```

 is useful for understanding the problem,

 but:

```
memoized solution
        ↓
mechanically compressed DP
```

 is **not necessarily the best final algorithm**.

 That's an important lesson in itself.

---

 # 8\. 956 — Tallest Billboard

 This one can be space optimized from:

```
dp[index][difference]
```

 to:

```
dp[difference]
```

 We use a `Map` because the difference range is more naturally represented that way.

```
var tallestBillboard = function (rods) {
    let dp = new Map();

    // difference = 0
    // equal height = 0
    dp.set(0, 0);

    for (const rod of rods) {
        const next = new Map(dp);

        for (const [diff, height] of dp) {

            // Put rod on the taller side
            const newDiff = diff + rod;

            next.set(
                newDiff,
                Math.max(
                    next.get(newDiff) ?? -Infinity,
                    height
                )
            );

            // Put rod on the shorter side
            const newDiff2 =
                Math.abs(diff - rod);

            const newHeight =
                height + Math.min(diff, rod);

            next.set(
                newDiff2,
                Math.max(
                    next.get(newDiff2) ?? -Infinity,
                    newHeight
                )
            );
        }

        dp = next;
    }

    return dp.get(0) ?? 0;
};
```

 The important thing here is that we still preserve the **previous iteration** while processing the current rod.

---

 # The final picture

 Now you can see the entire evolution.

 ## 416

```
Recursive

r(index, target)
        ↓
Memo

memo[index][target]
        ↓
Bottom-up

dp[index][target]
        ↓
Space optimized

dp[target]
```

 Final:

```
for (const num of nums) {
    for (let target = total; target >= num; target--) {
        dp[target] =
            dp[target] ||
            dp[target - num];
    }
}
```

---

 ## 1049

 Same exact structural progression:

```
r(index, target)
        ↓
memo[index][target]
        ↓
dp[index][target]
        ↓
dp[target]
```

 but the operation changes:

```
Math.max(...)
```

---

 ## 474

```
r(index, zeros, ones)
        ↓
memo[index][zeros][ones]
        ↓
dp[index][zeros][ones]
        ↓
dp[zeros][ones]
```

---

 ## 879

```
r(index, members, profit)
        ↓
memo[index][members][profit]
        ↓
dp[index][members][profit]
        ↓
dp[members][profit]
```

---

 ## 494

```
r(index, sum)
        ↓
memo[index][sum]
        ↓
dp[index][sum]
        ↓
dp[sum]
```

 with an offset because `sum` can be negative.

---

 # And here's the part I really want you to internalize

 There are **two separate optimizations** happening.

 ### Optimization 1 — Memoization

 You start with:

```
                   recursive tree
                /       |       \
              ...      ...      ...
```

 and notice:

```
same state
same state
same state
```

 So you cache it.

```
recursion
    ↓
memoization
```

---

 ### Optimization 2 — Space compression

 Then you inspect the DP dependency:

```
dp[i][target]
        ↓
depends only on
        ↓
dp[i - 1][...]
```

 Therefore `i` doesn't need to physically exist.

```
2D DP
 ↓
1D DP
```

 And that's why your original principle is actually **correct**, with one refinement:

 > **Bottom-up is derived from the recursive state and transitions, but the final optimized DP is derived from the dependency structure of the bottom-up table.**

 So the full mental pipeline is:

```
                RECURSION
                    │
                    ↓
            Identify STATE
                    │
                    ↓
            Identify BASE CASE
                    │
                    ↓
             Identify CHOICES
                    │
                    ↓
             MEMOIZATION
                    │
                    ↓
             BOTTOM-UP DP
                    │
                    ↓
        Analyze DEPENDENCIES
                    │
                    ↓
          SPACE OPTIMIZATION
                    │
                    ↓
              FINAL DP
```

 And for the classic 0/1 subset family, the beautiful recurring pattern is:

```
                PICK / DON'T PICK
                       │
                       ↓
                 2D DP
              dp[index][target]
                       │
                       ↓
                 1D DP
                  dp[target]
                       │
                       ↓
              iterate target
                  BACKWARD
```

 That **backward loop** is the crucial final optimization:

```
for (let target = capacity; target >= num; target--)
```

 because it prevents the current item from being used more than once.