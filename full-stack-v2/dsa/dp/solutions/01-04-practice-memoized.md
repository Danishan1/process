Absolutely. Let's take the **recursive versions we just established** and add memoization to each one.

 The key principle stays the same:

 > **Don't change the recursive logic. Just cache the result of each state.**

 I'll keep the memoization style explicit so you can see exactly what is being cached.

---

 # 1\. 416 — Partition Equal Subset Sum

 Our recursive state was:

```
(index, target)
```

 So our cache is:

```
memo[index][target]
```

 ### Memoized recursion

```
var canPartition = function (nums) {
    const total = nums.reduce((sum, num) => sum + num, 0);

    if (total % 2 !== 0) return false;

    const target = total / 2;

    const memo = Array.from(
        { length: nums.length },
        () => Array(target + 1)
    );

    const r = (index, target) => {
        if (target === 0) return true;
        if (index < 0 || target < 0) return false;

        if (memo[index][target] !== undefined) {
            return memo[index][target];
        }

        const pick =
            r(index - 1, target - nums[index]);

        const notPick =
            r(index - 1, target);

        memo[index][target] = pick || notPick;

        return memo[index][target];
    };

    return r(nums.length - 1, target);
};
```

 The important transformation is simply:

```
r(index, target)
        ↓
memo[index][target]
```

 Complexity:

```
Time:  O(n × target)
Space: O(n × target)
```

---

 # 2\. 494 — Target Sum

 Our recursive state was:

```
(index, sum)
```

 And our function returns:

```
number of ways
```

 So:

```
memo[index][sum]
```

 But `sum` can be negative.

 For example:

```
nums = [1, 2]

0
├── +1
│   ├── +2 → 3
│   └── -2 → -1
└── -1
    ├── +2 → 1
    └── -2 → -3
```

 So a normal array indexed by `sum` isn't convenient.

 We can use a `Map`.

 ### Memoized recursion

```
var findTargetSumWays = function (nums, target) {
    const memo = new Map();

    const r = (index, sum) => {
        if (index < 0) {
            return sum === target ? 1 : 0;
        }

        const key = `${index},${sum}`;

        if (memo.has(key)) {
            return memo.get(key);
        }

        const plus =
            r(index - 1, sum + nums[index]);

        const minus =
            r(index - 1, sum - nums[index]);

        const result = plus + minus;

        memo.set(key, result);

        return result;
    };

    return r(nums.length - 1, 0);
};
```

 Notice the difference from 416.

 416:

```
return pick || notPick;
```

 494:

```
return plus + minus;
```

 Because we're counting.

---

 # 3\. 1049 — Last Stone Weight II

 Our recursive state:

```
(index, target)
```

 Return value:

```
maximum achievable subset sum
```

 Therefore:

```
memo[index][target]
```

 ### Memoized recursion

```
var lastStoneWeightII = function (stones) {
    const total = stones.reduce(
        (sum, stone) => sum + stone,
        0
    );

    const target = Math.floor(total / 2);

    const memo = Array.from(
        { length: stones.length },
        () => Array(target + 1)
    );

    const r = (index, target) => {
        if (index < 0) return 0;

        if (memo[index][target] !== undefined) {
            return memo[index][target];
        }

        const notPick =
            r(index - 1, target);

        let pick = 0;

        if (stones[index] <= target) {
            pick =
                stones[index] +
                r(
                    index - 1,
                    target - stones[index]
                );
        }

        memo[index][target] =
            Math.max(pick, notPick);

        return memo[index][target];
    };

    const best =
        r(stones.length - 1, target);

    return total - 2 * best;
};
```

 Again:

```
Recursive:

r(index, target)

        ↓

Memoized:

memo[index][target]
```

 Exactly the same state.

---

 # 4\. 474 — Ones and Zeroes

 Now our state has **three dimensions**:

```
(index, zerosLeft, onesLeft)
```

 Therefore our memo has three dimensions:

```
memo[index][zerosLeft][onesLeft]
```

 ### Memoized recursion

```
var findMaxForm = function (strs, m, n) {

    const counts = strs.map(str => {
        let zeros = 0;

        for (const ch of str) {
            if (ch === "0") {
                zeros++;
            }
        }

        return [
            zeros,
            str.length - zeros
        ];
    });

    const memo = Array.from(
        { length: strs.length },
        () =>
            Array.from(
                { length: m + 1 },
                () => Array(n + 1)
            )
    );

    const r = (index, zerosLeft, onesLeft) => {
        if (index < 0) return 0;

        if (
            memo[index][zerosLeft][onesLeft] !== undefined
        ) {
            return memo[index][zerosLeft][onesLeft];
        }

        const notPick =
            r(index - 1, zerosLeft, onesLeft);

        const [zeros, ones] = counts[index];

        let pick = 0;

        if (
            zeros <= zerosLeft &&
            ones <= onesLeft
        ) {
            pick =
                1 +
                r(
                    index - 1,
                    zerosLeft - zeros,
                    onesLeft - ones
                );
        }

        memo[index][zerosLeft][onesLeft] =
            Math.max(pick, notPick);

        return memo[index][zerosLeft][onesLeft];
    };

    return r(
        strs.length - 1,
        m,
        n
    );
};
```

 This is a really good example of how the number of dimensions of your memo follows directly from your recursive state.

```
Recursive state
        ↓
(index, zerosLeft, onesLeft)

Memo
        ↓
memo[index][zerosLeft][onesLeft]
```

---

 # 5\. 879 — Profitable Schemes

 State:

```
(index, membersLeft, profitNeeded)
```

 Return:

```
number of valid schemes
```

 So the memo is:

```
memo[index][membersLeft][profitNeeded]
```

 ### Memoized recursion

```
var profitableSchemes = function (
    n,
    minProfit,
    group,
    profit
) {
    const MOD = 1_000_000_007;

    const memo = Array.from(
        { length: group.length },
        () =>
            Array.from(
                { length: n + 1 },
                () => Array(minProfit + 1)
            )
    );

    const r = (
        index,
        membersLeft,
        profitNeeded
    ) => {
        if (index < 0) {
            return profitNeeded === 0 ? 1 : 0;
        }

        if (
            memo[index][membersLeft][profitNeeded]
            !== undefined
        ) {
            return memo[index][membersLeft][profitNeeded];
        }

        // Don't pick
        const notPick =
            r(
                index - 1,
                membersLeft,
                profitNeeded
            );

        // Pick
        let pick = 0;

        if (group[index] <= membersLeft) {
            pick =
                r(
                    index - 1,
                    membersLeft - group[index],
                    Math.max(
                        0,
                        profitNeeded - profit[index]
                    )
                );
        }

        memo[index][membersLeft][profitNeeded] =
            (pick + notPick) % MOD;

        return memo[index][membersLeft][profitNeeded];
    };

    return r(
        group.length - 1,
        n,
        minProfit
    );
};
```

 Again, notice:

```
pick + notPick
```

 because this is a **counting** problem.

---

 # 6\. 698 — Partition to K Equal Sum Subsets

 This one is different.

 Our recursive state was conceptually:

```
(index, buckets)
```

 But `buckets` is an array.

 We cannot simply do:

```
memo[index][buckets]
```

 because arrays aren't useful as normal state keys.

 We need to encode the state.

 A straightforward approach is:

```
const key = `${index}|${buckets.join(",")}`;
```

 ### Memoized recursion

```
var canPartitionKSubsets = function (nums, k) {
    const total = nums.reduce(
        (sum, num) => sum + num,
        0
    );

    if (total % k !== 0) return false;

    const target = total / k;

    nums.sort((a, b) => b - a);

    const buckets = Array(k).fill(0);

    const memo = new Map();

    const r = (index) => {
        if (index === nums.length) {
            return true;
        }

        const key =
            `${index}|${buckets.join(",")}`;

        if (memo.has(key)) {
            return memo.get(key);
        }

        const num = nums[index];

        for (let i = 0; i < k; i++) {

            if (buckets[i] + num > target) {
                continue;
            }

            buckets[i] += num;

            if (r(index + 1)) {
                memo.set(key, true);
                buckets[i] -= num;
                return true;
            }

            buckets[i] -= num;

            if (buckets[i] === 0) {
                break;
            }
        }

        memo.set(key, false);

        return false;
    };

    return r(0);
};
```

 This one is worth remembering as a different category:

```
Simple subset problems:
(index, target)

K-subset:
(index, state of all buckets)
```

 So memoization becomes more complicated because the state itself is more complicated.

---

 # 7\. 2597 — The Number of Beautiful Subsets

 Our recursive solution used:

```
(index, selected)
```

 The issue is that `selected` is a `Set`.

 Again, we need to represent the state in a memoizable way.

 A simple approach is to use a `Set` and convert it to a sorted representation.

 However, there's an even cleaner state for this problem if we process the numbers in a suitable order.

 For now, staying close to our original recursion:

 ### Memoized recursion

```
var beautifulSubsets = function (nums, k) {
    nums.sort((a, b) => a - b);

    const selected = new Set();
    const memo = new Map();

    const r = (index) => {
        if (index === nums.length) {
            return 1;
        }

        const state = [...selected]
            .sort((a, b) => a - b)
            .join(",");

        const key = `${index}|${state}`;

        if (memo.has(key)) {
            return memo.get(key);
        }

        // Don't pick
        const notPick = r(index + 1);

        // Pick
        let pick = 0;

        if (
            !selected.has(nums[index] - k) &&
            !selected.has(nums[index] + k)
        ) {
            selected.add(nums[index]);

            pick = r(index + 1);

            selected.delete(nums[index]);
        }

        const result = pick + notPick;

        memo.set(key, result);

        return result;
    };

    return r(0) - 1;
};
```

 This works conceptually, but **I wouldn't recommend this implementation** as the final solution because converting the `Set` into a sorted string at every state is expensive.

 This is a good example where:

 > A recursive solution can be easy to write, while finding the right memoization state requires more thought.

---

 # 8\. 956 — Tallest Billboard

 This is another interesting one because our state is:

```
(index, difference)
```

 So we can memoize:

```
memo[index][difference]
```

 But `difference` can be negative.

 Again, a `Map` is convenient.

 Using the standard difference-based recursive formulation:

```
var tallestBillboard = function (rods) {
    const memo = new Map();

    const r = (index, diff) => {
        if (index < 0) {
            return diff === 0 ? 0 : -Infinity;
        }

        const key = `${index},${diff}`;

        if (memo.has(key)) {
            return memo.get(key);
        }

        const rod = rods[index];

        // Skip
        const skip =
            r(index - 1, diff);

        // Add rod to the taller side
        const taller =
            r(
                index - 1,
                Math.abs(diff - rod)
            );

        // Add rod to the shorter side
        const shorter =
            r(
                index - 1,
                diff + rod
            ) +
            Math.min(diff, rod);

        const result = Math.max(
            skip,
            taller,
            shorter
        );

        memo.set(key, result);

        return result;
    };

    return r(rods.length - 1, 0);
};
```

 Again, `Map` is useful because the state can contain values that aren't convenient array indexes.

---

 # The important part: look at the transformation

 Now put all of these next to each other.

 | Problem | Recursive state | Memo |
| --- | --- | --- |
| **416** | `(index, target)` | `memo[index][target]` |
| **494** | `(index, sum)` | `Map(index, sum)` |
| **1049** | `(index, target)` | `memo[index][target]` |
| **474** | `(index, zeros, ones)` | `memo[index][zeros][ones]` |
| **879** | `(index, members, profit)` | `memo[index][members][profit]` |
| **698** | `(index, buckets)` | `Map(encoded state)` |
| **2597** | `(index, selected)` | `Map(encoded state)` |
| **956** | `(index, difference)` | `Map(index, difference)` |

 This is the pattern I really want you to notice.

 ## Memoization isn't a new algorithm

 Take your 416 recursion:

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

 Memoization essentially says:

```
const r = (index, target) => {
    if (target === 0) return true;
    if (index < 0) return false;

    if (memo[index][target] !== undefined) {
        return memo[index][target];
    }

    const pick =
        r(index - 1, target - nums[index]);

    const notPick =
        r(index - 1, target);

    memo[index][target] = pick || notPick;

    return memo[index][target];
};
```

 **That's it.**

 The recursive decision tree hasn't changed.

 We've simply said:

 > "Before solving this state, check whether I've solved this state before."

---

 # And this gives us a very useful rule

 When you're given a recursive DP problem:

 ### Step 1 — Write the recursive state

```
What variables completely describe my problem?
```

 ### Step 2 — Write the recursion

```
What choices can I make?
```

 ### Step 3 — Write base cases

```
When do I know the answer immediately?
```

 ### Step 4 — Memoize the state

```
memo[state] = answer
```

 And **the dimensions of your memo come directly from the dimensions of your recursive state.**

 For example:

```
(index, target)
        ↓
2D memo

(index, zeros, ones)
        ↓
3D memo

(index, members, profit)
        ↓
3D memo

(index, difference)
        ↓
2D memo / Map
```

 That connection is much more important than memorizing individual LeetCode solutions.

 One note: **698, 2597, and 956 have more nuanced state representations than 416**, so their memoized versions are useful for learning, but I would treat them as a second layer after you're completely comfortable with the `416 → memo → bottom-up` transformation.