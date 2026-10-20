# 1. `+` + coercion

### Example 1

```js
console.log(1 + "2" + 3);
```

**Answer:**

```js
"123"
```

Step by step:

```text
1 + "2"
→ "12"

"12" + 3
→ "123"
```

---

### Example 2

```js
console.log(1 + 2 + "3" + 4 + 5);
```

**Answer:**

```js
"3345"
```

Because:

```text
1 + 2
→ 3

3 + "3"
→ "33"

"33" + 4
→ "334"

"334" + 5
→ "3345"
```

### Key lesson

Once `+` enters string concatenation, subsequent `+` operations continue concatenating.

---

# 2. `-` forces numbers

### Example 3

```js
console.log("10" - "5" + "2");
```

**Answer:**

```js
"52"
```

First:

```text
"10" - "5"
→ 10 - 5
→ 5
```

Then:

```text
5 + "2"
→ "52"
```

---

### Example 4

```js
console.log("10" + "5" - 2);
```

**Answer:**

```js
103
```

First:

```text
"10" + "5"
→ "105"
```

Then:

```text
"105" - 2
→ 105 - 2
→ 103
```

Notice how the operator determines what happens.

---

# 3. Boolean coercion

### Example 5

```js
console.log(true + true);
```

**Answer:**

```js
2
```

Because:

```text
true → 1
true → 1

1 + 1 = 2
```

---

### Example 6

```js
console.log(true + false + "1");
```

**Answer:**

```js
"11"
```

```text
true + false
→ 1 + 0
→ 1

1 + "1"
→ "11"
```

---

### Example 7

```js
console.log("5" - true);
```

**Answer:**

```js
4
```

```text
"5" → 5
true → 1

5 - 1
→ 4
```

---

# 4. `null` and `undefined`

### Example 8

```js
console.log(null + null);
```

**Answer:**

```js
0
```

In numeric arithmetic:

```text
null → 0
null → 0

0 + 0 = 0
```

---

### Example 9

```js
console.log(null + undefined);
```

**Answer:**

```js
NaN
```

Because:

```text
null → 0
undefined → NaN

0 + NaN
→ NaN
```

---

### Example 10

```js
console.log(null == 0);
console.log(null === 0);
```

**Answer:**

```js
false
false
```

Don't confuse:

```js
Number(null) // 0
```

with:

```js
null == 0     // false
```

The conversion rules depend on the operation.

---

# 5. Arrays are objects — and coercion gets weird

### Example 11

```js
console.log([] + []);
```

**Answer:**

```js
""
```

Arrays convert to strings.

```text
[] → ""

"" + ""
→ ""
```

---

### Example 12

```js
console.log([1] + [2]);
```

**Answer:**

```js
"12"
```

Because:

```text
[1] → "1"
[2] → "2"

"1" + "2"
→ "12"
```

---

### Example 13

```js
console.log([1, 2] + [3, 4]);
```

**Answer:**

```js
"1,23,4"
```

Because:

```text
[1,2] → "1,2"
[3,4] → "3,4"

"1,2" + "3,4"
→ "1,23,4"
```

---

# 6. The famous `[] ==` traps

### Example 14

```js
console.log([] == false);
```

**Answer:**

```js
true
```

Conceptually:

```text
[] → ""
"" → 0

false → 0

0 == 0
→ true
```

---

### Example 15

```js
console.log([] == 0);
```

**Answer:**

```js
true
```

```text
[] → ""
"" → 0

0 == 0
→ true
```

---

### Example 16

```js
console.log([] === 0);
```

**Answer:**

```js
false
```

No coercive equality conversion:

```text
[] → object
0  → number
```

Different types.

---

### Example 17

```js
console.log([1] == 1);
```

**Answer:**

```js
true
```

```text
[1]
 ↓
"1"
 ↓
1

1 == 1
→ true
```

---

### Example 18

```js
console.log([1, 2] == 12);
```

**Answer:**

```js
false
```

Because:

```text
[1,2]
 ↓
"1,2"
 ↓
Number("1,2")
 ↓
NaN
```

So:

```text
NaN == 12
→ false
```

---

# 7. Object equality

### Example 19

```js
const a = {};
const b = {};

console.log(a == b);
console.log(a === b);
```

**Answer:**

```js
false
false
```

Both objects are different references.

---

### Example 20

```js
const a = {};
const b = a;

console.log(a == b);
console.log(a === b);
```

**Answer:**

```js
true
true
```

Because both point to the same object.

---

### Example 21

```js
console.log([] == []);
```

**Answer:**

```js
false
```

Even though both are empty arrays.

```text
Array A → []
Array B → []
```

Different objects.

---

# 8. Truthiness vs equality

This distinction is extremely important.

### Example 22

```js
console.log(Boolean([]));
console.log([] == false);
```

**Answer:**

```js
true
true
```

This looks contradictory, but it isn't.

### `Boolean([])`

Checks whether the object itself is truthy.

All objects are truthy:

```js
Boolean([]) // true
```

### `[] == false`

Uses the `==` coercion algorithm:

```text
[] → "" → 0
false → 0

0 == 0
→ true
```

So:

> **Truthiness and loose equality are completely different mechanisms.**

---

# 9. `||` returns values

### Example 23

```js
console.log("hello" || "world");
```

Result:

```js
"hello"
```

Because `"hello"` is truthy.

---

### Example 24

```js
console.log("" || "world");
```

Result:

```js
"world"
```

---

### Example 25

```js
console.log(0 || "hello");
```

Result:

```js
"hello"
```

---

### Example 26

```js
console.log(false || 0 || null || "hello");
```

Result:

```js
"hello"
```

JavaScript keeps searching until it finds a truthy value.

---

# 10. `&&` returns values

### Example 27

```js
console.log("hello" && "world");
```

Result:

```js
"world"
```

Because `"hello"` is truthy, so `&&` evaluates and returns the second operand.

---

### Example 28

```js
console.log(0 && "hello");
```

Result:

```js
0
```

Because `0` is falsy, so evaluation stops.

---

### Example 29

```js
console.log("A" && "B" && "C");
```

Result:

```js
"C"
```

All previous operands are truthy, so the final operand is returned.

---

### Example 30

```js
console.log("A" && 0 && "C");
```

Result:

```js
0
```

Evaluation:

```text
"A" → truthy
 ↓
0 → falsy
 ↓
STOP
```

`"C"` is never evaluated.

---

# 11. `||` vs `??`

### Example 31

```js
console.log(0 || 100);
console.log(0 ?? 100);
```

Result:

```js
100
0
```

Because:

```text
|| → checks truthiness

?? → checks null/undefined
```

---

### Example 32

```js
console.log(false || "default");
console.log(false ?? "default");
```

Result:

```js
"default"
false
```

---

### Example 33

```js
console.log("" || "default");
console.log("" ?? "default");
```

Result:

```js
"default"
""
```

---

### Example 34

```js
console.log(null || "default");
console.log(null ?? "default");
```

Both:

```js
"default"
```

---

# 12. Precedence + logical operators

### Example 35

```js
console.log(false || true && "Hello");
```

Answer:

```js
"Hello"
```

Because:

```text
&& has higher precedence
```

Therefore:

```js
false || (true && "Hello")
```

Then:

```text
true && "Hello"
→ "Hello"

false || "Hello"
→ "Hello"
```

---

### Example 36

```js
console.log((false || true) && "Hello");
```

Result:

```js
"Hello"
```

But notice the grouping is now explicitly controlled by parentheses.

---

### Example 37

```js
console.log(true || false && false);
```

Result:

```js
true
```

Grouping:

```js
true || (false && false)
```

`true || ...` short-circuits, so the right side isn't needed.

---

# 13. Optional chaining traps

### Example 38

```js
const user = null;

console.log(user?.profile?.name);
```

Result:

```js
undefined
```

No error.

---

### Example 39

```js
const user = {
    profile: null
};

console.log(user?.profile?.name);
```

Result:

```js
undefined
```

Again, safe.

---

### Example 40

```js
const user = {
    profile: null
};

console.log(user?.profile.name);
```

Result:

```text
TypeError
```

Why?

Because only this is optional:

```js
user?.profile
```

After that, JavaScript effectively tries:

```js
null.name
```

which throws.

---

# 14. Optional chaining + `??`

### Example 41

```js
const user = {};

console.log(user?.profile?.name ?? "Guest");
```

Result:

```js
"Guest"
```

Pipeline:

```text
user
 ↓
profile doesn't exist
 ↓
undefined
 ↓
?? "Guest"
 ↓
"Guest"
```

---

### Example 42

```js
const user = {
    profile: {
        name: ""
    }
};

console.log(user?.profile?.name ?? "Guest");
```

Result:

```js
""
```

This is where `??` is useful.

The empty string is preserved.

---

### Compare:

```js
user?.profile?.name || "Guest"
```

would produce:

```js
"Guest"
```

because `""` is falsy.

---

# 15. Prefix/postfix increment

### Example 43

```js
let x = 5;

console.log(x++);
console.log(x);
```

Result:

```text
5
6
```

Postfix:

```text
return old value
then increment
```

---

### Example 44

```js
let x = 5;

console.log(++x);
console.log(x);
```

Result:

```text
6
6
```

Prefix:

```text
increment
then return new value
```

---

### Example 45 — tricky

```js
let x = 5;

const result = x++ + ++x;

console.log(result);
console.log(x);
```

Let's carefully evaluate:

```text
x++ 
→ returns 5
→ x becomes 6

++x
→ x becomes 7
→ returns 7

5 + 7
→ 12
```

Therefore:

```text
result = 12
x = 7
```

This is technically valid JavaScript, but **avoid writing code like this in production** because it hurts readability.

---

# 16. `NaN` traps

### Example 46

```js
console.log(NaN == NaN);
console.log(NaN === NaN);
console.log(Object.is(NaN, NaN));
```

Result:

```text
false
false
true
```

---

### Example 47

```js
console.log(Number.isNaN("hello"));
console.log(Number.isNaN(NaN));
```

Result:

```text
false
true
```

Don't use:

```js
value === NaN
```

to check for NaN.

---

# 17. `-0` trap

### Example 48

```js
console.log(0 === -0);
console.log(Object.is(0, -0));
```

Result:

```text
true
false
```

This is one of the reasons `Object.is()` exists.

---

# 18. Precedence vs associativity

### Example 49

```js
console.log(10 - 5 - 2);
```

Result:

```js
3
```

Because:

```js
(10 - 5) - 2
```

Subtraction is left-associative.

---

### Example 50

```js
console.log(10 - (5 - 2));
```

Result:

```js
7
```

Parentheses change the grouping.

---

### Example 51

```js
let a, b;

a = b = 10;

console.log(a, b);
```

Result:

```text
10 10
```

Because assignment is right-associative:

```js
a = (b = 10);
```

---

# 19. A serious interview-level combination

Now let's combine everything.

### Example 52

```js
console.log([] || "hello");
```

What do you think?

Answer:

```js
[]
```

Why?

`[]` is an object.

Objects are truthy:

```js
Boolean([])
// true
```

Therefore `||` returns the first operand:

```js
[] || "hello"
// []
```

---

### Example 53

```js
console.log([] ?? "hello");
```

Result:

```js
[]
```

`??` only checks:

```text
null
undefined
```

`[]` is neither.

---

### Example 54

```js
console.log("" || [] || "hello");
```

Result:

```js
[]
```

Because:

```text
"" → falsy
[] → truthy
```

So `||` stops at `[]`.

---

# 20. The "evil" one

Try this mentally:

```js
console.log([] + {} + [1]);
```

Let's break it down.

First:

```js
[] + {}
```

Arrays and objects are converted to primitive values for `+`.

Conceptually:

```text
[] → ""
{} → "[object Object]"
```

So:

```text
"" + "[object Object]"
→ "[object Object]"
```

Then:

```text
"[object Object]" + [1]
```

`[1]` becomes `"1"`:

```text
"[object Object]" + "1"
```

Final:

```js
"[object Object]1"
```

---

# 🎯 Your Practice Round

Now **don't look for the answers yet**. Try these yourself.

### Level 1 — Warm-up

```js
// Q1
console.log("5" + 2 - 1);

// Q2
console.log(5 + "2" - 1);

// Q3
console.log(true + false + "3");

// Q4
console.log(null + 5);

// Q5
console.log(undefined + 5);
```

### Level 2 — Equality

```js
// Q6
console.log([] == 0);

// Q7
console.log([] === 0);

// Q8
console.log([1] == "1");

// Q9
console.log([1, 2] == "1,2");

// Q10
console.log({} == {});
```

### Level 3 — Logical operators

```js
// Q11
console.log(0 || "" || null || "Hello");

// Q12
console.log("Hello" && 0 && "World");

// Q13
console.log(false || true && 0 || "Done");

// Q14
console.log([] && "Hello");

// Q15
console.log(null ?? 10 || 20);
```

### Level 4 — SDE-2

```js
// Q16
let a = 1;

console.log(a++ + ++a);
```

```js
// Q17
console.log(1 + 2 + "3" + 4 - 1);
```

```js
// Q18
console.log([] + [] + [1]);
```

```js
// Q19
const user = {
    profile: null
};

console.log(user?.profile?.name ?? "Guest");
```

```js
// Q20
const user = {
    profile: null
};

console.log(user?.profile.name ?? "Guest");
```

### ⭐ And these two are particularly important:

```js
// Q21
console.log(false || true && false || "hello");
```

```js
// Q22
console.log(0 ?? 10 || 20);
```

For **Q1–Q22**, try to write the output **without executing them**. Then send me your answers. I'll check them one by one and, more importantly, identify **which JavaScript rule you're getting wrong** rather than just giving you the output.
