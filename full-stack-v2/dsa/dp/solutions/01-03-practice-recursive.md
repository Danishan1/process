### Take / Not Take — Subset DP

This is the **most important pattern to master first**.

| LeetCode # | Title                            | Small Summary                                               |
| ---------: | -------------------------------- | ----------------------------------------------------------- |
|    **416** | Partition Equal Subset Sum       | Can the array be divided into two subsets with equal sum?   |
|    **494** | Target Sum                       | Assign `+` or `-` to every number to reach a target.        |
|   **1049** | Last Stone Weight II             | Partition stones to minimize the remaining difference.      |
|    **474** | Ones and Zeroes                  | Select maximum strings within limited `0` and `1` capacity. |
|    **698** | Partition to K Equal Sum Subsets | Divide array into `k` subsets having equal sum.             |
|   **2597** | The Number of Beautiful Subsets  | Choose subsets while respecting a difference constraint.    |
|    **879** | Profitable Schemes               | Select jobs under member/profit constraints.                |
|    **956** | Tallest Billboard                | Partition rods into two groups with equal height.           |

There is also some overlap in your two tables (`474`, `416`, `1049`). I'll cover each **unique problem once**, and for each one we'll write the **recursive/top-down formulation first**—no bottom-up yet.

Our base pattern is:

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

The question is: **what changes when the problem changes?**

---

# 1\. LeetCode 416 — Partition Equal Subset Sum

This is our original problem.

The total must be even, and we need to find a subset with:

```
target = total / 2
```

### Recursive solution

```
var canPartition = function (nums) {
    const total = nums.reduce((sum, num) => sum + num, 0);

    if (total % 2 !== 0) return false;

    const target = total / 2;

    const r = (index, target) => {
        if (target === 0) return true;
        if (index < 0) return false;

        return (
            r(index - 1, target - nums[index]) ||
            r(index - 1, target)
        );
    };

    return r(nums.length - 1, target);
};
```

### Pattern

```
State:
(index, target)

Choices:
1. Pick nums[index]
2. Don't pick nums[index]

Goal:
target === 0
```

This is the **pure subset-sum template**.

---

# 2\. LeetCode 494 — Target Sum

Now the question is twisted.

Instead of:

> Pick or don't pick?

we must use **every number**, but each number gets either:

```
+ nums[index]
```

or:

```
- nums[index]
```

So our recursion changes from:

```
pick / don't pick
```

to:

```
plus / minus
```

### Recursive solution

```
var findTargetSumWays = function (nums, target) {

    const r = (index, sum) => {
        if (index < 0) {
            return sum === target ? 1 : 0;
        }

        const plus =
            r(index - 1, sum + nums[index]);

        const minus =
            r(index - 1, sum - nums[index]);

        return plus + minus;
    };

    return r(nums.length - 1, 0);
};
```

Notice something beautiful:

Our return value is no longer:

```
true / false
```

It's:

```
number of ways
```

Therefore:

```
return plus + minus;
```

instead of:

```
return pick || notPick;
```

### Compare

**416:**

```
return pick || notPick;
```

means:

> Does at least one possibility work?

**494:**

```
return plus + minus;
```

means:

> How many possibilities work?

That's a huge DP pattern.

---

# 3\. LeetCode 1049 — Last Stone Weight II

This one looks completely different initially.

But it's secretly:

> Divide stones into two groups whose sums are as close as possible.

Suppose:

```
Group A = a
Group B = b

total = a + b
```

Remaining stone:

```
a - b
```

We want to minimize:

```
|a - b|
```

Since:

```
b = total - a
```

we get:

```
|a - (total - a)|
```

So we want a subset sum `a` as close as possible to:

```
total / 2
```

This means the recursive state can be:

```
(index, sum)
```

but now we're not asking a boolean question.

We're asking:

> What's the **maximum achievable subset sum**?

### Recursive solution

```
var lastStoneWeightII = function (stones) {
    const total = stones.reduce((sum, stone) => sum + stone, 0);

    const r = (index, target) => {
        if (index < 0) return 0;

        const notPick =
            r(index - 1, target);

        let pick = 0;

        if (stones[index] <= target) {
            pick = stones[index] +
                r(index - 1, target - stones[index]);
        }

        return Math.max(pick, notPick);
    };

    const half = Math.floor(total / 2);

    const best = r(stones.length - 1, half);

    return total - 2 * best;
};
```

Notice how our basic recursion changed:

### 416

```
pick || notPick
```

### 1049

```
Math.max(pick, notPick)
```

Because we're solving an **optimization** problem.

---

# 4\. LeetCode 474 — Ones and Zeroes

Now things get more interesting.

Our original problem had one resource:

```
target
```

474 has **two resources**:

```
zeros
ones
```

Each string consumes some amount of each.

So our state becomes:

```
(index, zerosLeft, onesLeft)
```

And the question becomes:

> What's the maximum number of strings I can select?

### Recursive solution

```
var findMaxForm = function (strs, m, n) {

    const count = (str) => {
        let zeros = 0;

        for (const ch of str) {
            if (ch === "0") {
                zeros++;
            }
        }

        return [zeros, str.length - zeros];
    };

    const r = (index, zerosLeft, onesLeft) => {
        if (index < 0) return 0;

        const [zeros, ones] = count(strs[index]);

        const notPick =
            r(index - 1, zerosLeft, onesLeft);

        let pick = 0;

        if (zeros <= zerosLeft && ones <= onesLeft) {
            pick =
                1 +
                r(
                    index - 1,
                    zerosLeft - zeros,
                    onesLeft - ones
                );
        }

        return Math.max(pick, notPick);
    };

    return r(strs.length - 1, m, n);
};
```

Now compare with 416:

```
416:

(index, target)

474:

(index, zerosLeft, onesLeft)
```

The recursion is still:

```
PICK
DON'T PICK
```

But the **state has gained another dimension**.

---

# 5\. LeetCode 879 — Profitable Schemes

This is very similar to 474.

In 474 we have:

```
zeros capacity
ones capacity
```

In 879 we have:

```
members capacity
profit requirement
```

For each job:

```
group[i] = members required
profit[i] = profit generated
```

We want to count schemes.

Therefore:

```
State:

(index, membersLeft, profitNeeded)
```

And our answer is the **number of ways**, not max value.

### Recursive solution

```
var profitableSchemes = function (
    n,
    minProfit,
    group,
    profit
) {
    const MOD = 1_000_000_007;

    const r = (index, membersLeft, profitNeeded) => {
        if (index < 0) {
            return profitNeeded === 0 ? 1 : 0;
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

        return (pick + notPick) % MOD;
    };

    return r(
        group.length - 1,
        n,
        minProfit
    );
};
```

The interesting part is:

```
Math.max(0, profitNeeded - profit[index])
```

We don't actually care if we exceed the required profit.

For example:

```
Need 10
Current job gives 15

remaining need = 0
```

not:

```
-5
```

So we clamp it.

---

# 6\. LeetCode 698 — Partition to K Equal Sum Subsets

This one is related to 416, but there's an important difference.

416 asks:

> Can I find **one** subset with target `total / 2`?

698 asks:

> Can I create **K different subsets**, each having target `total / k`?

We can't simply use:

```
(index, target)
```

because we need to keep track of **multiple buckets**.

A clean recursive formulation is:

```
(index, buckets)
```

where `buckets[i]` represents the current sum of subset `i`.

### Recursive solution

```
var canPartitionKSubsets = function (nums, k) {
    const total = nums.reduce((sum, num) => sum + num, 0);

    if (total % k !== 0) return false;

    const target = total / k;

    nums.sort((a, b) => b - a);

    const buckets = Array(k).fill(0);

    const r = (index) => {
        if (index === nums.length) {
            return true;
        }

        const num = nums[index];

        for (let i = 0; i < k; i++) {
            if (buckets[i] + num > target) {
                continue;
            }

            // Pick this bucket
            buckets[i] += num;

            if (r(index + 1)) {
                return true;
            }

            // Undo
            buckets[i] -= num;

            // Avoid equivalent empty buckets
            if (buckets[i] === 0) {
                break;
            }
        }

        return false;
    };

    return r(0);
};
```

This is still recursion, but the structure is now:

```
                    number
                 /    |    \
             bucket1 bucket2 bucket3
```

rather than:

```
              number
              /    \
            pick   skip
```

So this is an important distinction:

> **Not every subset problem is best represented by simple pick/not-pick recursion.**

---

# 7\. LeetCode 2597 — The Number of Beautiful Subsets

Here we're selecting a subset, but there's a constraint:

For any two selected numbers:

```
|a - b| !== k
```

A natural recursive state is:

```
(index, selected)
```

where `selected` keeps track of what we've already chosen.

For a straightforward recursive solution, a `Set` works.

### Recursive solution

```
var beautifulSubsets = function (nums, k) {
    const selected = new Set();

    const r = (index) => {
        if (index === nums.length) {
            return 1;
        }

        // Don't pick
        let notPick = r(index + 1);

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

        return pick + notPick;
    };

    // Subtract empty subset
    return r(0) - 1;
};
```

Again:

```
return pick + notPick;
```

because we're counting.

The key difference from 416 is that the legality of `pick` depends on **what we've already selected**.

---

# 8\. LeetCode 956 — Tallest Billboard

This is perhaps the most interesting one in your list.

We have rods and want two groups with equal height.

Instead of tracking:

```
sum of group A
sum of group B
```

we can track their **difference**.

Define:

```
difference = heightA - heightB
```

Then:

```
difference === 0
```

means the two billboards are equal.

Our state can be:

```
(index, difference)
```

For each rod, we have **three choices**:

```
1. Don't use it
2. Put it on left
3. Put it on right
```

### Recursive solution

```
var tallestBillboard = function (rods) {

    const r = (index, diff) => {
        if (index < 0) {
            return diff === 0 ? 0 : -Infinity;
        }

        const rod = rods[index];

        // Don't use rod
        const skip =
            r(index - 1, diff);

        // Put rod on left
        const left =
            r(index - 1, diff + rod);

        // Put rod on right
        const right =
            r(index - 1, diff - rod);

        return Math.max(
            skip,
            left,
            right
        );
    };

    return r(rods.length - 1, 0);
};
```

But there's a subtle issue here:

The return value needs to represent the **height of the equal portion**, not simply whether the difference becomes zero.

So a more useful state is:

```
(index, diff)
→ maximum height of the shorter/equal contribution
```

A common recursive formulation is:

```
var tallestBillboard = function (rods) {

    const r = (index, diff) => {
        if (index < 0) {
            return diff === 0 ? 0 : -Infinity;
        }

        const rod = rods[index];

        // Skip
        const skip =
            r(index - 1, diff);

        // Add to taller side
        const addToTaller =
            r(index - 1, Math.abs(diff - rod));

        // Add to shorter side
        const addToShorter =
            r(index - 1, diff + rod);

        return Math.max(
            skip,
            addToTaller,
            addToShorter
        );
    };

    return r(rods.length - 1, 0);
};
```

However, this formulation still doesn't correctly accumulate the final billboard height by itself. The clean recursive formulation uses the **difference as state and returns the maximum height added to the shorter side**:

```
var tallestBillboard = function (rods) {

    const r = (index, diff) => {
        if (index < 0) {
            return diff === 0 ? 0 : -Infinity;
        }

        const rod = rods[index];

        // Skip
        const skip = r(index - 1, diff);

        // Put rod on the taller side.
        const taller =
            r(index - 1, Math.abs(diff - rod));

        // Put rod on the shorter side.
        const shorter =
            r(index - 1, diff + rod) +
            Math.min(diff, rod);

        return Math.max(skip, taller, shorter);
    };

    return r(rods.length - 1, 0);
};
```

This one is substantially more subtle than 416, so I would **not** use it as your first example of translating subset-sum recursion.

---

# The important pattern across all of them

Now look at what happened.

## 416 — Boolean subset sum

```
State:
(index, target)

Choices:
pick / don't pick

Return:
true / false

Combine:
OR
```

```
return pick || notPick;
```

---

## 494 — Count ways

```
State:
(index, sum)

Choices:
+ / -

Return:
number of ways

Combine:
+
```

```
return plus + minus;
```

---

## 1049 — Optimization

```
State:
(index, target)

Choices:
pick / don't pick

Return:
best achievable sum

Combine:
MAX
```

```
return Math.max(pick, notPick);
```

---

## 474 — Two-dimensional capacity

```
State:
(index, zeros, ones)

Choices:
pick / don't pick

Return:
maximum number selected

Combine:
MAX
```

```
return Math.max(pick, notPick);
```

---

## 879 — Count with two constraints

```
State:
(index, members, profit)

Choices:
pick / don't pick

Return:
number of schemes

Combine:
+
```

---

## 698 — Multiple subsets

```
State:
(index, buckets)

Choices:
put current number in bucket 0...k-1

Return:
true / false
```

This is **not** the same simple two-branch recursion.

---

## 2597 — Count valid subsets

```
State:
(index, selected)

Choices:
pick / don't pick

Return:
number of valid subsets

Combine:
+
```

---

## 956 — Three choices + difference state

```
State:
(index, difference)

Choices:
skip / left / right

Return:
maximum equal height

Combine:
MAX
```

---

# The bigger framework

This is what I'd want you to start recognizing instead of memorizing LeetCode numbers:

```
                    SUBSET / CHOICE PROBLEM
                             │
                             ↓
                     What is the STATE?
                             │
             ┌───────────────┼────────────────┐
             ↓               ↓                ↓
         index +          index +          index +
         target           capacity         difference
             │               │                │
             ↓               ↓                ↓
         What are the choices?
             │
             ↓
      ┌──────┼─────────┐
      ↓      ↓         ↓
    pick   skip      +/- / buckets
      │      │
      └──────┼─────────┘
             ↓
       What do we return?
             │
       ┌─────┼────────────┐
       ↓     ↓            ↓
    boolean count       best
       │     │            │
       ↓     ↓            ↓
      OR     +           MAX
```

That's the real connection between these problems.

And this is why I like your approach of starting from the recursive solution. **The recursive function forces you to answer three questions:**

1. **What is my state?**
2. **What choices do I have?**
3. **What does my function return?**

Once those three are correct, the memoized and bottom-up versions become much more mechanical.

The next useful exercise would be to take these **one at a time** and do exactly what we did with `416`: **recursive → memoized recursive → 2D bottom-up → 2-row → 1D optimized**, while explicitly deriving every initialization from the recursive base cases rather than memorizing templates.
