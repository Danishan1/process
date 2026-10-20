# 4.54 — Expressions & Operators

## Expressions

### 1. What is an expression?

An **expression is any valid piece of JavaScript code that evaluates to a value**.

Examples:

```js
10;
```

```js
a + b;
```

```js
user.name;
```

```js
a = 10;
```

```js
condition ? "yes" : "no";
```

Even a function call is an expression:

```js
getUser();
```

The important definition:

> **Expression → something that produces a value.**

---

### 2. What is the difference between an expression and a statement?

A **statement performs an action**, while an **expression produces a value**.

Example:

```js
const x = 10;
```

This is a declaration statement containing an expression (`10`).

Another example:

```js
if (x > 5) {
  console.log(x);
}
```

`if` is a statement.

But:

```js
x > 5;
```

is an expression because it evaluates to `true` or `false`.

Think:

```text
Expression
    ↓
produces a value

Statement
    ↓
performs an action / controls execution
```

Some constructs can blur the distinction because expressions can appear inside statements.

---

### 3. Is assignment an expression in JavaScript?

**Yes.**

```js
const result = (a = 10);

console.log(result);
// 10
```

The assignment itself evaluates to the assigned value.

```js
a = 10;
```

produces:

```text
10
```

This is why you can write:

```js
let a;

a = b = 10;
```

---

### 4. Why does `a = b = 10` work?

Because the **assignment operator is right-associative**.

JavaScript evaluates it conceptually as:

```js
a = b = 10;
```

First:

```js
b = 10;
```

produces `10`.

Then:

```js
a = 10;
```

So:

```js
console.log(a); // 10
console.log(b); // 10
```

Mental model:

```text
a = b = 10
    ↓
a = (b = 10)
       ↓
      10
    ↓
a = 10
```

---

### 5. What happens during expression evaluation?

JavaScript evaluates an expression according to the rules of its operators and operands.

For example:

```js
const result = 10 + "20";
```

Conceptually:

```text
1. Evaluate 10
2. Evaluate "20"
3. Apply +
4. + sees string involvement
5. Convert/concatenate according to + rules
6. Produce "1020"
7. Assign result
```

For a more complicated expression:

```js
a + b * c;
```

JavaScript first determines the **operator structure using precedence/associativity**, then evaluates the required operands and operations.

Important distinction:

> **Precedence determines how an expression is grouped; it does not mean JavaScript evaluates everything in that textual order.**

---

# Arithmetic & Coercion

### 6. Why does `"10" - 2` produce `8`?

Because `-` is a numeric operator.

JavaScript converts `"10"` to a number:

```text
"10" - 2
 ↓
10 - 2
 ↓
8
```

```js
Number("10"); // 10
```

Therefore:

```js
"10" - 2;
// 8
```

---

### 7. Why does `"10" + 2` produce `"102"`?

Because `+` has **two roles**:

```text
numeric addition
string concatenation
```

When string concatenation applies:

```text
"10" + 2
 ↓
"10" + "2"
 ↓
"102"
```

So:

```js
"10" + 2;
// "102"
```

This is one of the most important JavaScript coercion rules.

---

### 8. Explain `1 + 2 + "3"`

Result:

```js
"33";
```

The expression is evaluated left-to-right because `+` is left-associative:

```text
1 + 2 + "3"
 ↓
(1 + 2) + "3"
 ↓
3 + "3"
 ↓
"33"
```

The first operation is numeric addition.

The second operation becomes string concatenation.

---

### 9. Explain `"1" + 2 + 3`

Result:

```js
"123";
```

Again, evaluation is left-to-right:

```text
"1" + 2 + 3
 ↓
("1" + 2) + 3
 ↓
"12" + 3
 ↓
"123"
```

Once the first `+` produces a string, the next `+` continues concatenation.

Compare:

```js
1 + 2 + "3"; // "33"

"1" + 2 + 3; // "123"
```

This is a classic interview question.

---

### 10. What is the difference between `%` and mathematical modulo?

JavaScript's `%` is technically the **remainder operator**, not a mathematical modulo operation.

For positive numbers they often look identical:

```js
10 % 3;
// 1
```

But negative numbers expose the difference:

```js
-10 % 3;
// -1
```

Mathematical modulo is often expected to produce a non-negative result for a positive modulus:

```text
-10 mod 3 = 2
```

So don't automatically call `%` "modulo" in a precise language discussion.

If you need mathematical modulo:

```js
((n % m) + m) % m;
```

For example:

```js
((-10 % 3) + 3) % 3;
// 2
```

---

### 11. What happens when arithmetic involves `NaN` or `Infinity`?

`NaN` generally **propagates through arithmetic**:

```js
NaN + 10;
// NaN

NaN * 10;
// NaN
```

`Infinity` follows numeric arithmetic rules:

```js
Infinity + 10;
// Infinity

Infinity * 2;
// Infinity

Infinity - Infinity;
// NaN
```

Some important cases:

```js
1 / 0 -
  // Infinity

  1 / 0;
// -Infinity

0 / 0;
// NaN
```

Remember:

```js
typeof NaN;
// "number"

typeof Infinity;
// "number"
```

---

# Equality

### 12. Difference between `==` and `===`?

### `==` — loose equality

Allows type coercion according to JavaScript's equality algorithm.

```js
5 == "5";
// true
```

### `===` — strict equality

Does not perform the same coercive conversion between different types.

```js
5 === "5";
// false
```

For production code, `===` is generally preferred because it avoids surprising coercion.

Mental model:

```text
==
↓
"Can these values be considered equal after the equality algorithm?"

===
↓
"Are these values strictly equal?"
```

---

### 13. Why is `null == undefined` true?

Because loose equality has a **specific rule** that considers:

```js
null;
```

and:

```js
undefined;
```

equal to each other.

Therefore:

```js
null == undefined;
// true
```

But:

```js
null === undefined;
// false
```

Don't explain this as "`null` converts to `undefined`." That's incorrect.

It's a **special rule in the `==` algorithm**.

---

### 14. Why is `null == 0` false?

This is another important `==` rule.

You might incorrectly think:

```text
null → 0
```

because:

```js
Number(null);
// 0
```

But loose equality doesn't simply perform `Number()` conversion in every situation.

For:

```js
null == 0;
```

the special `null`/`undefined` equality rule doesn't match `0`, so the result is:

```js
false;
```

Therefore:

```js
null == undefined; // true
null == 0; // false
```

---

### 15. Why is `NaN === NaN` false?

Because `NaN` has special numeric semantics.

```js
NaN === NaN;
// false
```

It also means:

```js
NaN == NaN;
// false
```

To check for NaN:

```js
Number.isNaN(value);
```

Example:

```js
Number.isNaN(NaN);
// true
```

---

### 16. Difference between `===` and `Object.is()`?

They are almost identical for most ordinary values, but differ in two important cases.

### `NaN`

```js
NaN === NaN;
// false

Object.is(NaN, NaN);
// true
```

### `-0`

```js
-0 === 0;
// true

Object.is(-0, 0);
// false
```

Table:

| Comparison     |   `===` | `Object.is()` |
| -------------- | ------: | ------------: |
| `NaN` vs `NaN` | `false` |        `true` |
| `0` vs `-0`    |  `true` |       `false` |
| `1` vs `1`     |  `true` |        `true` |
| `"a"` vs `"a"` |  `true` |        `true` |

---

### 17. How does object equality work?

Objects are compared by **identity/reference**, not by their contents.

```js
{} === {}
// false
```

Because these are two different objects.

```js
const a = {};
const b = a;

a === b;
// true
```

Both variables refer to the same object.

Think:

```text
a ─────┐
       ↓
     Object
       ↑
b ─────┘
```

versus:

```text
a → Object A

b → Object B
```

Even if:

```js
{
  name: "John";
}
```

and:

```js
{
  name: "John";
}
```

have identical properties, they are different object identities.

---

# Logical Operators

### 18. Do `&&` and `||` return Booleans?

**No.**

This is extremely important.

They return one of their **operands**.

```js
const result = "hello" || "world";

console.log(result);
// "hello"
```

And:

```js
const result = "" || "world";

console.log(result);
// "world"
```

Similarly:

```js
"hello" && 123;
// 123
```

```js
0 && 123;
// 0
```

So:

```text
&& / ||
   ↓
evaluate truthiness
   ↓
return an operand
```

Not necessarily `true` or `false`.

---

### 19. Explain short-circuit evaluation.

Short-circuiting means JavaScript **stops evaluating as soon as the result is already determined**.

For `&&`:

```js
false && doSomething();
```

JavaScript knows:

```text
false AND anything = false
```

so it doesn't call `doSomething()`.

For `||`:

```js
true || doSomething();
```

JavaScript knows:

```text
true OR anything = true
```

so it doesn't call `doSomething()`.

This can prevent unnecessary work and side effects.

---

### 20. Why is this useful in production code?

A common pattern is:

```js
condition && doSomething();
```

Meaning:

> Execute `doSomething()` only if `condition` is truthy.

Example:

```js
isAuthenticated && loadDashboard();
```

If:

```js
isAuthenticated === false;
```

then:

```js
loadDashboard();
```

is never executed.

Other common uses include:

```js
const name = user && user.name;
```

and fallback values:

```js
const port = config.port || 3000;
```

Although for defaults where `0`, `false`, or `""` are valid values, `??` is often more appropriate.

---

### 21. Difference between `||` and `??`?

This is **very important in modern JavaScript**.

`||` checks **truthiness**.

`??` checks specifically for:

```text
null
undefined
```

Example:

```js
0 || 10;
// 10
```

because `0` is falsy.

But:

```js
0 ?? 10;
// 0
```

because `0` is neither `null` nor `undefined`.

Comparison:

| Value       |    `value |           | 10` | `value ?? 10` |
| ----------- | --------: | --------: | --- | ------------- |
| `0`         |      `10` |       `0` |
| `""`        |      `10` |      `""` |
| `false`     |      `10` |   `false` |
| `null`      |      `10` |      `10` |
| `undefined` |      `10` |      `10` |
| `"hello"`   | `"hello"` | `"hello"` |

Use:

```js
||
```

when you want a **falsy fallback**.

Use:

```js
??
```

when you want a fallback only for **missing values**.

---

### 22. Why does `0 || 10` return `10`, while `0 ?? 10` returns `0`?

Because:

```js
Boolean(0);
// false
```

Therefore:

```js
0 || 10;
```

means approximately:

```text
0 is falsy
↓
return 10
```

But:

```js
0 ?? 10;
```

asks:

```text
Is 0 null or undefined?
↓
No
↓
return 0
```

This distinction is particularly important for configuration:

```js
const timeout = config.timeout ?? 5000;
```

If the user intentionally configured:

```js
timeout: 0;
```

you preserve it.

With:

```js
config.timeout || 5000;
```

you would incorrectly replace `0`.

---

# Optional Chaining

### 23. What does `?.` do?

Optional chaining safely accesses a property or calls a method when the preceding value may be `null` or `undefined`.

Instead of:

```js
user && user.profile && user.profile.name;
```

you can write:

```js
user?.profile?.name;
```

If `user` is `null` or `undefined`, the result is:

```js
undefined;
```

rather than throwing an error.

---

### 24. How does optional chaining short-circuit?

Consider:

```js
user?.profile?.name;
```

If:

```js
user === null;
```

JavaScript stops the chain and returns:

```js
undefined;
```

It doesn't attempt:

```js
user.profile;
```

because that would throw.

Conceptually:

```text
user?
 ↓
null/undefined?
 ↓
YES
 ↓
stop
 ↓
undefined
```

---

### 25. Difference between `user?.profile.name` and `user?.profile?.name`?

This is a **very common interview trap**.

### First:

```js
user?.profile.name;
```

Only `user` is protected.

If:

```js
user === null;
```

the result is:

```js
undefined;
```

But if:

```js
user = {
  profile: null,
};
```

then:

```js
user?.profile.name;
```

throws because `.name` is still accessed on `null`.

### Second:

```js
user?.profile?.name;
```

Both accesses are protected.

If:

```js
user = {
  profile: null,
};
```

result:

```js
undefined;
```

So:

```text
user?.profile.name
     ↑
only user protected

user?.profile?.name
     ↑       ↑
  user     profile protected
```

---

### 26. How do `?.` and `??` commonly work together?

This is an extremely useful production pattern:

```js
const name = user?.profile?.name ?? "Guest";
```

Two separate operations happen:

```text
user?.profile?.name
        ↓
safely access nested property
        ↓
undefined if unavailable
        ↓
?? "Guest"
        ↓
fallback only if null/undefined
```

Example:

```js
const name = user?.profile?.name ?? "Guest";
```

If:

```js
user = null;
```

then:

```js
name === "Guest";
```

If:

```js
user.profile.name = "";
```

then:

```js
name === "";
```

That's different from:

```js
user?.profile?.name || "Guest";
```

which would replace the empty string too.

---

# Unary Operators

### 27. What does unary `+` do?

Unary `+` attempts to convert its operand into a **Number**.

```js
+"10" +
  // 10

  true +
  // 1

  false +
  // 0

  null +
  // 0

  "hello";
// NaN
```

So:

```js
const value = +"42";
```

is equivalent in intent to:

```js
const value = Number("42");
```

For production code, `Number()` is often clearer.

---

### 28. Difference between `++x` and `x++`?

Both increment `x`, but they differ in **the value produced by the expression**.

### Prefix

```js
let x = 5;

const y = ++x;
```

First increment:

```text
x → 6
```

Then return:

```text
6
```

So:

```js
x; // 6
y; // 6
```

### Postfix

```js
let x = 5;

const y = x++;
```

First return the old value:

```text
y → 5
```

Then increment:

```text
x → 6
```

So:

```js
x; // 6
y; // 5
```

Mental model:

```text
++x → increment → return new value

x++ → return old value → increment
```

---

### 29. What does `delete` actually delete?

`delete` removes a **property from an object**, assuming the property is configurable.

```js
const user = {
  name: "John",
  age: 30,
};

delete user.age;

console.log(user);
// { name: "John" }
```

It does **not** mean:

> "Destroy this JavaScript variable."

For example:

```js
let x = 10;

delete x;
```

doesn't remove the lexical binding.

Also:

```js
delete user.age;
```

returns a Boolean indicating whether the deletion succeeded according to the relevant semantics.

---

### 30. What does `void` return?

`void` evaluates its operand and **always returns `undefined`**.

```js
void 10;
// undefined

void console.log("hello");
// prints hello
// returns undefined
```

So:

```js
const result = void someFunction();
```

means:

```text
execute someFunction()
ignore its result
result = undefined
```

Historically, you'll sometimes see:

```js
javascript: void 0;
```

in old browser code.

---

### 31. Why is `typeof` considered a unary operator?

Because it operates on **one operand**.

```js
typeof value;
```

Compare:

```text
+a       → unary
!a       → unary
typeof a → unary

a + b    → binary
a > b    → binary
```

`typeof` takes one operand and produces a string describing its runtime type category.

---

# Precedence & Associativity

### 32. What is operator precedence?

**Precedence determines how operators are grouped when multiple operators appear in an expression.**

Example:

```js
2 + 3 * 4;
```

Multiplication has higher precedence than addition.

So it's interpreted as:

```js
2 + 3 * 4;
```

Result:

```js
14;
```

Without precedence, you might incorrectly calculate:

```text
(2 + 3) * 4 = 20
```

---

### 33. What is associativity?

Associativity determines how operators with the **same precedence** are grouped.

For example, subtraction is left-associative:

```js
10 - 5 - 2;
```

means:

```js
10 - 5 - 2;
```

not:

```js
10 - (5 - 2);
```

Assignment is right-associative:

```js
a = b = 10;
```

means:

```js
a = b = 10;
```

So:

```text
Precedence
→ which operator binds first?

Associativity
→ when precedence is equal, which direction?
```

---

### 34. Difference between precedence and evaluation order?

This distinction is **very important**.

**Precedence determines the structure/grouping of an expression.**

**Evaluation order determines when operands/subexpressions are actually evaluated.**

Example:

```js
a() + b() * c();
```

Precedence tells us the structure is:

```js
a() + b() * c();
```

But JavaScript still evaluates the relevant operands according to its evaluation rules, rather than simply saying:

> "Multiplication happens before `a()`."

So don't confuse:

```text
operator precedence
```

with:

```text
execution/evaluation order
```

A good interview statement:

> **Precedence determines how an expression is parsed/grouped; it doesn't by itself specify the runtime order in which every subexpression is evaluated.**

---

### 35. Evaluate `2 + 3 * 4`

`*` has higher precedence than `+`.

Therefore:

```js
2 + 3 * 4;
```

Then:

```text
3 * 4 = 12
2 + 12 = 14
```

Answer:

```js
14;
```

---

### 36. Evaluate `10 - 5 - 2`

Both operators are subtraction and have the same precedence.

Subtraction is **left-associative**:

```js
10 - 5 - 2;
```

Therefore:

```text
10 - 5 = 5
5 - 2 = 3
```

Answer:

```js
3;
```

---

### 37. Evaluate `a = b = 10`

Assignment is **right-associative**:

```js
a = b = 10;
```

First:

```js
b = 10;
```

The assignment expression evaluates to:

```js
10;
```

Then:

```js
a = 10;
```

Final:

```js
a === 10;
b === 10;
```

---

### 38. Why is `false || true && "hello"` equal to `"hello"`?

This combines **precedence + short-circuiting + operand-returning behavior**.

First, `&&` has higher precedence than `||`.

Therefore:

```js
false || (true && "hello");
```

Evaluate `&&`:

```js
true && "hello";
```

Since the left operand is truthy, `&&` returns the right operand:

```js
"hello";
```

Now we have:

```js
false || "hello";
```

`false` is falsy, so `||` returns the right operand:

```js
"hello";
```

Therefore:

```js
false || (true && "hello");
// "hello"
```

### The full chain

```text
false || true && "hello"
        │
        │ && has higher precedence
        ▼
false || (true && "hello")
             │
             ▼
        "hello"
             │
             ▼
false || "hello"
        │
        ▼
    "hello"
```

---

# 🔥 SDE-2 Cheat Sheet

If you want to retain this entire section, I'd reduce it to these **core rules**:

| Concept          | Rule to remember                                   |
| ---------------- | -------------------------------------------------- |
| Expression       | Produces a value                                   |
| Statement        | Performs an action / controls execution            |
| Assignment       | Is an expression                                   |
| `=`              | Right-associative                                  |
| `+`              | Addition **or** string concatenation               |
| `-`, `*`, `/`    | Numeric operations → coercion may occur            |
| `==`             | Loose/coercive equality                            |
| `===`            | Strict equality                                    |
| `Object.is()`    | Differs from `===` for `NaN` and `-0`              |
| Objects          | Compared by identity                               |
| `&&`             | Returns an operand; stops on falsy                 |
| `\|\|`           | Returns an operand; stops on truthy                |
| `??`             | Fallback only for `null`/`undefined`               |
| `?.`             | Safely accesses/calls through nullish values       |
| `?.` + `??`      | Safe access + default                              |
| Unary `+`        | Converts to Number                                 |
| `++x`            | Increment, then return new value                   |
| `x++`            | Return old value, then increment                   |
| `delete`         | Removes an object property                         |
| `void`           | Evaluates expression, returns `undefined`          |
| `typeof`         | Unary runtime type operator                        |
| Precedence       | Determines grouping                                |
| Associativity    | Determines grouping direction for equal precedence |
| Evaluation order | Runtime order of evaluating subexpressions         |

### The 5 rules I'd be able to derive under interview pressure

```text
1. + is special
   → number + number = addition
   → string involved = concatenation

2. == can coerce
   → === doesn't do that coercive equality conversion

3. Objects compare by identity
   → {} !== {}

4. && / || return operands
   → they are not necessarily Boolean operators in their result

5. Precedence ≠ evaluation order
   → precedence tells you how the expression is grouped
```

If you can **derive the result from these rules**, rather than memorizing individual tricks, you'll be in a much stronger position for the SDE-2 JavaScript round.
