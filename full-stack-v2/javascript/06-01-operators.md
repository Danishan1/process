Absolutely. Let's continue with **Part 4: Operators & Expressions** at the same SDE-2 depth.

# 4. Operators & Expressions

Operators are the mechanisms JavaScript uses to **perform operations on values**.

An expression is code that **evaluates to a value**.

This distinction is fundamental:

```js
10 + 20
```

is an expression because it evaluates to:

```text
30
```

Whereas:

```js
let x = 10;
```

is a declaration statement containing an initialization expression.

The important mental model for this section is:

```text
Expression
    ↓
Evaluate operands
    ↓
Apply operator semantics
    ↓
Possible coercion
    ↓
Produce a value
```

---

# 4.1 What Is an Operator?

An operator is syntax that performs an operation.

Examples:

```js
a + b
a === b
a && b
!a
typeof a
```

Operators can be classified by the number of operands they operate on.

### Unary

One operand:

```js
!value
typeof value
-delete obj.x
```

### Binary

Two operands:

```js
a + b
a === b
a * b
```

### Ternary

Three operands:

```js
condition ? value1 : value2
```

So:

```text
Unary   → 1 operand
Binary  → 2 operands
Ternary → 3 operands
```

---

# 4.2 What Is an Expression?

An expression is something that JavaScript can evaluate to produce a value.

Examples:

```js
10
"hello"
x
x + 10
x === 10
foo()
condition ? a : b
```

For example:

```js
const result = 10 + 20;
```

The expression:

```js
10 + 20
```

evaluates to:

```text
30
```

So:

```text
Expression → value
```

This becomes extremely important when understanding operators.

---

# 4.3 Arithmetic Operators

The basic arithmetic operators are:

```text
+    addition
-    subtraction
*    multiplication
/    division
%    remainder
**   exponentiation
```

Examples:

```js
10 + 5; // 15
10 - 5; // 5
10 * 5; // 50
10 / 5; // 2
10 % 3; // 1
2 ** 3; // 8
```

But JavaScript arithmetic is strongly connected to **type coercion**.

For example:

```js
"10" - 5
```

produces:

```text
5
```

while:

```js
"10" + 5
```

produces:

```text
"105"
```

The `+` operator is special because it can perform either numeric addition or string concatenation.

---

# 4.4 Addition vs String Concatenation

Consider:

```js
1 + 2
```

Result:

```text
3
```

But:

```js
"1" + 2
```

Result:

```text
"12"
```

And:

```js
1 + "2"
```

Result:

```text
"12"
```

JavaScript may convert operands to strings when `+` encounters a string-like operand after the relevant coercion steps.

This is why these produce surprising results:

```js
1 + 2 + "3"
```

Result:

```text
"33"
```

Evaluation proceeds left-to-right:

```text
1 + 2
 ↓
3

3 + "3"
 ↓
"33"
```

Compare:

```js
"1" + 2 + 3
```

First:

```text
"1" + 2
 ↓
"12"
```

Then:

```text
"12" + 3
 ↓
"123"
```

Therefore:

```js
1 + 2 + "3"   // "33"
"1" + 2 + 3   // "123"
```

This is an excellent example of **expression evaluation order + operator semantics + coercion**.

---

# 4.5 Remainder Operator `%`

JavaScript's `%` operator gives the remainder.

```js
10 % 3
```

→

```text
1
```

It is often called the "modulo operator," but technically its behavior is based on the **remainder** operation and therefore interacts with negative numbers differently from mathematical modulo in some contexts.

For example:

```js
-10 % 3
```

produces:

```text
-1
```

This distinction can matter in algorithms.

---

# 4.6 Exponentiation

JavaScript supports:

```js
2 ** 3
```

→

```text
8
```

It can also be used with assignment:

```js
x **= 2;
```

which is equivalent in effect to:

```js
x = x ** 2;
```

subject to normal evaluation semantics.

---

# 4.7 Assignment Operators

Basic assignment:

```js
x = 10;
```

Compound assignment operators include:

```text
+=
-=
*=
/=
%=
**=
<<=
>>=
>>>=
&=
|=
^=
&&=
||=
??=
```

Examples:

```js
let x = 10;

x += 5;
```

Now:

```text
x === 15
```

Conceptually:

```js
x = x + 5;
```

But don't treat every compound assignment as a simplistic textual substitution. The actual evaluation semantics matter, especially when the left-hand side has side effects.

---

# 4.8 Assignment Is an Expression

This is an important JavaScript property.

Consider:

```js
let x;

x = 10;
```

The assignment itself evaluates to the assigned value:

```js
x = 10
```

evaluates to:

```text
10
```

Therefore:

```js
let a;
let b;

b = a = 10;
```

works as:

```text
a = 10
   ↓
10
   ↓
b = 10
```

So both become `10`.

This is possible because assignment is an expression.

---

# 4.9 Comparison Operators

Comparison operators include:

```text
>
<
>=
<=
```

Examples:

```js
10 > 5   // true
10 < 5   // false
10 >= 10 // true
10 <= 9  // false
```

These produce Boolean values:

```text
true
false
```

But comparisons can also involve coercion.

For example:

```js
"10" > 5
```

performs numeric conversion in the comparison process and results in:

```text
true
```

So comparison operators aren't independent from the type system we just studied.

---

# 4.10 Equality Operators

JavaScript provides:

```text
==
!=
===
!==
```

There are two fundamentally different equality models:

```text
Loose equality
    ↓
==

Strict equality
    ↓
===
```

---

# 4.11 Strict Equality `===`

Strict equality does not perform the coercive conversion associated with `==`.

Examples:

```js
5 === 5
// true

5 === "5"
// false

true === 1
// false
```

For objects:

```js
{} === {}
```

is:

```text
false
```

because these are two distinct object values.

But:

```js
const a = {};
const b = a;

a === b
```

is:

```text
true
```

because both bindings refer to the same object.

---

# 4.12 Strict Equality and `NaN`

Remember:

```js
NaN === NaN
```

is:

```text
false
```

If you need to determine whether a value is NaN:

```js
Number.isNaN(value)
```

is the preferred precise check.

---

# 4.13 Strict Equality and `-0`

Another edge case:

```js
-0 === 0
```

returns:

```text
true
```

But:

```js
Object.is(-0, 0)
```

returns:

```text
false
```

So `===` and `Object.is()` represent slightly different notions of sameness.

---

# 4.14 Loose Equality `==`

Loose equality allows JavaScript's equality algorithm to perform coercion.

For example:

```js
5 == "5"
```

returns:

```text
true
```

because the values can be coerced into compatible forms for comparison.

Other famous examples:

```js
0 == false
```

→ `true`

```js
"" == false
```

→ `true`

```js
null == undefined
```

→ `true`

But:

```js
null == 0
```

→ `false`

This is why `==` is often difficult to reason about if you haven't studied its actual algorithm.

---

# 4.15 Why `==` Shouldn't Be Memorized as "Converts Everything"

This statement is too simplistic:

> "`==` converts both values to the same type."

The equality algorithm has **special cases**.

For example:

```js
null == undefined
```

is true.

But:

```js
null == 0
```

is false.

And:

```js
undefined == 0
```

is also false.

Therefore, the better mental model is:

```text
==
 ↓
Apply JavaScript's Abstract Equality Comparison algorithm
 ↓
Depending on operand types
 ↓
Possible coercion / special-case handling
 ↓
Boolean result
```

---

# 4.16 Logical Operators

JavaScript has three major logical operators:

```text
&&
||
!
```

They are often described as Boolean operators, but there is a very important JavaScript-specific detail:

> **`&&` and `||` do not necessarily return a Boolean.**

---

# 4.17 Logical AND `&&`

Consider:

```js
true && true
```

→

```text
true
```

But:

```js
true && "hello"
```

→

```text
"hello"
```

And:

```js
false && "hello"
```

→

```text
false
```

So `&&` returns one of its operands.

The general idea:

```text
A && B
```

means:

```text
Evaluate A
    ↓
If A is falsy
    ↓
Return A

Otherwise
    ↓
Evaluate B
    ↓
Return B
```

This is **short-circuit evaluation**.

---

# 4.18 Logical OR `||`

Similarly:

```js
true || "hello"
```

returns:

```text
true
```

while:

```js
false || "hello"
```

returns:

```text
"hello"
```

Conceptually:

```text
A || B

Evaluate A
   ↓
If A is truthy
   ↓
Return A

Otherwise
   ↓
Evaluate B
   ↓
Return B
```

Again:

> `||` returns operands, not necessarily Boolean values.

---

# 4.19 Short-Circuit Evaluation

Consider:

```js
const user = null;

user && user.name;
```

Because:

```text
user
 ↓
null
 ↓
falsy
```

JavaScript doesn't need to evaluate:

```js
user.name
```

The result is:

```text
null
```

This is why the pattern:

```js
condition && doSomething();
```

works.

If `condition` is falsy:

```text
doSomething()
```

isn't evaluated.

This is both a logical operation and a **control-flow mechanism**.

---

# 4.20 Short-Circuiting Can Prevent Side Effects

Consider:

```js
isAuthenticated && logAccess();
```

If:

```js
isAuthenticated === false
```

then:

```js
logAccess()
```

is never called.

This matters because short-circuiting doesn't merely change a returned value—it can prevent the evaluation of expressions with:

* function calls
* property access
* mutations
* expensive calculations
* other side effects

---

# 4.21 Nullish Coalescing `??`

The nullish coalescing operator provides a different fallback mechanism:

```js
value ?? fallback
```

It uses the fallback only when `value` is:

```text
null
undefined
```

Example:

```js
const name = null;

name ?? "Guest";
```

→

```text
"Guest"
```

But:

```js
const count = 0;

count ?? 10;
```

→

```text
0
```

This is a major difference from `||`.

---

# 4.22 `||` vs `??`

Consider:

```js
const count = 0;

count || 10;
```

Result:

```text
10
```

because `0` is falsy.

But:

```js
count ?? 10;
```

Result:

```text
0
```

because `0` is not nullish.

So:

```text
|| 
→ fallback for any falsy value

??
→ fallback only for null or undefined
```

This distinction is extremely important in real applications.

For example, if:

```js
const pageSize = userProvidedPageSize;
```

and `0` is meaningful, using:

```js
pageSize || 10
```

may incorrectly replace `0`.

Using:

```js
pageSize ?? 10
```

preserves it.

---

# 4.23 Optional Chaining `?.`

Optional chaining allows safe access through potentially nullish values.

Instead of:

```js
if (user && user.profile && user.profile.address) {
    // ...
}
```

you can write:

```js
user?.profile?.address
```

If an intermediate value is:

```text
null
```

or:

```text
undefined
```

the chain short-circuits to:

```text
undefined
```

Example:

```js
const user = null;

user?.name;
```

returns:

```text
undefined
```

instead of throwing:

```text
TypeError
```

---

# 4.24 Optional Chaining Is More Than Property Access

It can also be used with function calls:

```js
callback?.();
```

This means:

> Call `callback` if it is not nullish.

And with indexed access:

```js
user?.["name"];
```

So the syntax supports several forms of conditional access/calling.

---

# 4.25 Combining `?.` and `??`

This is an extremely common production pattern:

```js
const city = user?.profile?.address?.city ?? "Unknown";
```

The evaluation is conceptually:

```text
user
 ↓
optional access
 ↓
profile
 ↓
optional access
 ↓
address
 ↓
optional access
 ↓
city
 ↓
if null/undefined
 ↓
"Unknown"
```

This combines two separate concepts:

```text
?. → safely traverse nullish values
?? → provide nullish fallback
```

---

# 4.26 Unary Operators

Unary operators operate on one operand.

Important ones include:

```text
!
+
-
~
typeof
void
delete
++ 
--
```

Examples:

```js
!true
// false

+"10"
// 10

-"10"
// -10

~10
// bitwise operation

typeof value
// type string
```

Some of these deserve special attention.

---

# 4.27 Unary `+`

Unary plus converts its operand to a number.

```js
+"10"
```

→

```text
10
```

Similarly:

```js
+true
```

→

```text
1
```

and:

```js
+false
```

→

```text
0
```

and:

```js
+"hello"
```

→

```text
NaN
```

It is essentially a concise numeric-conversion operator.

---

# 4.28 Unary `-`

Unary minus performs numeric conversion and negation.

```js
-"10"
```

→

```text
-10
```

And:

```js
-true
```

→

```text
-1
```

---

# 4.29 Logical NOT `!`

`!` converts its operand according to Boolean semantics and then negates the result.

```js
!true
```

→

```text
false
```

```js
!0
```

→

```text
true
```

```js
!"hello"
```

→

```text
false
```

A common trick for Boolean conversion is:

```js
!!value
```

For example:

```js
!!"hello"
```

→

```text
true
```

The first `!` converts to Boolean and negates.

The second `!` negates again.

---

# 4.30 Increment and Decrement

JavaScript supports:

```text
++
--
```

Both prefix and postfix forms exist.

```js
++x
x++
```

and:

```js
--x
x--
```

The important difference is the **value of the expression**.

---

# 4.31 Prefix Increment

```js
let x = 10;

const result = ++x;
```

First:

```text
x → 11
```

Then:

```text
result → 11
```

So:

```js
x === 11
result === 11
```

---

# 4.32 Postfix Increment

```js
let x = 10;

const result = x++;
```

The current value is used first:

```text
result → 10
```

Then:

```text
x → 11
```

So:

```js
x === 11
result === 10
```

The same distinction applies to decrement.

---

# 4.33 Why `i++` Is More Than "Add One"

The statement:

```js
i++;
```

is simple.

But:

```js
const x = i++;
```

involves two conceptual operations:

```text
use current value
      ↓
produce expression result
      ↓
increment binding
```

Whereas:

```js
const x = ++i;
```

does:

```text
increment binding
      ↓
produce new value
```

This distinction frequently appears in interview questions.

---

# 4.34 Bitwise Operators

JavaScript also provides bitwise operators:

```text
&
|
^
~
<<
>>
>>>
```

These operate on the bit-level representation of numeric values, with JavaScript's specific conversion semantics.

For example:

```js
5 & 1
```

can be reasoned about using binary:

```text
5 → 101
1 → 001

101
001
---
001
```

Result:

```text
1
```

---

# 4.35 Important Bitwise Detail

JavaScript `number` values are IEEE-754 floating-point values, but bitwise operators operate using **32-bit integer representations** of the operands.

This means:

```js
5.5 | 0
```

does not preserve the floating-point value `5.5`.

The operands undergo the relevant integer conversion before the bitwise operation.

Bitwise operators therefore have very different semantics from ordinary arithmetic operators.

---

# 4.36 Conditional / Ternary Operator

Syntax:

```js
condition ? expressionIfTrue : expressionIfFalse
```

Example:

```js
const age = 20;

const status = age >= 18
    ? "adult"
    : "minor";
```

Result:

```text
"adult"
```

The ternary operator is an **expression**, meaning it produces a value.

That's why it can be used inside assignments:

```js
const result = condition ? a : b;
```

---

# 4.37 Ternary Evaluation Is Conditional

Only the selected branch is evaluated.

Conceptually:

```text
condition
   ↓
 true ──→ evaluate expressionIfTrue
   │
 false ─→ evaluate expressionIfFalse
```

This matters when the branches contain function calls or side effects.

For example:

```js
const result = condition
    ? expensiveOperation()
    : fallbackOperation();
```

Only one function call occurs.

---

# 4.38 `delete`

`delete` is a unary operator primarily used to remove an object's property.

Example:

```js
const user = {
    name: "Danishan",
    age: 25
};

delete user.age;
```

Now:

```js
user.age
```

produces:

```text
undefined
```

and the property no longer exists.

You can verify existence separately using mechanisms such as:

```js
"age" in user
```

which would be:

```text
false
```

---

# 4.39 `delete` Does Not Delete Variables

This distinction is important.

```js
let x = 10;

delete x;
```

does not simply mean:

> "Remove x from memory."

`delete` operates on object properties, and lexical bindings such as `let`/`const` are not deleted this way.

So don't think of:

```js
delete
```

as a general memory-management operation.

It is fundamentally a **property deletion operator**.

---

# 4.40 `void`

The `void` operator evaluates an expression and returns:

```text
undefined
```

Example:

```js
void 123
```

returns:

```text
undefined
```

But:

```js
void console.log("hello")
```

still evaluates the `console.log()` expression.

So:

```text
evaluate expression
      ↓
discard resulting value
      ↓
return undefined
```

One historical use is:

```js
javascript:void(0)
```

in old browser patterns, though this is generally not something you should use in modern application code.

---

# 4.41 `typeof` Is Also an Operator

We encountered `typeof` in the type-system section.

It is a unary operator:

```js
typeof value
```

Examples:

```js
typeof "hello"
// "string"

typeof 123
// "number"

typeof undefined
// "undefined"

typeof null
// "object"
```

It also has special behavior for undeclared identifiers:

```js
typeof unknownVariable
```

returns:

```text
"undefined"
```

while accessing a lexical binding in its TDZ can still throw:

```js
console.log(typeof x);
let x = 10;
```

→ `ReferenceError`.

---

# 4.42 Operator Precedence

Consider:

```js
2 + 3 * 4
```

Does JavaScript evaluate:

```text
(2 + 3) * 4
```

or:

```text
2 + (3 * 4)
```

The result is:

```text
14
```

because multiplication has higher precedence than addition.

Conceptually:

```text
2 + (3 * 4)
```

→

```text
14
```

Precedence determines which operators bind more tightly.

---

# 4.43 Parentheses Override Precedence

If we write:

```js
(2 + 3) * 4
```

we force addition to happen first:

```text
5 * 4
 ↓
20
```

Therefore:

```js
2 + 3 * 4
// 14

(2 + 3) * 4
// 20
```

A good engineering practice is:

> **Use parentheses when they make the intended evaluation order clearer, even if you know the precedence rules.**

---

# 4.44 Precedence Is Not Evaluation Order

This distinction is extremely important.

**Precedence** answers:

> Which operator groups with which operands?

It does not universally answer:

> Which operand gets evaluated first?

For example:

```js
a() + b() * c()
```

Precedence determines the structural grouping:

```text
a() + (b() * c())
```

But JavaScript also has rules governing evaluation order.

You should therefore distinguish:

```text
Precedence
→ how operators are grouped

Evaluation order
→ when subexpressions are evaluated
```

---

# 4.45 Associativity

Associativity determines how operators of the same precedence are grouped.

For example:

```js
10 - 5 - 2
```

is grouped as:

```text
(10 - 5) - 2
```

because subtraction is left-associative.

Result:

```text
3
```

Not:

```text
10 - (5 - 2)
```

which would be:

```text
7
```

---

# 4.46 Assignment Is Right-Associative

Consider:

```js
a = b = 10;
```

Assignment is right-associative.

So conceptually:

```text
a = (b = 10)
```

First:

```text
b = 10
```

produces `10`.

Then:

```text
a = 10
```

Therefore both become `10`.

---

# 4.47 Expression Evaluation

Now let's combine everything.

Consider:

```js
const result = 10 + "5" * 2;
```

We need to reason rather than memorize.

First, precedence:

```text
10 + ("5" * 2)
```

Multiplication happens first.

`"5"` is converted to a number:

```text
5 * 2
 ↓
10
```

Then:

```text
10 + 10
```

→

```text
20
```

So:

```js
result === 20
```

---

# 4.48 Another Example

Consider:

```js
const result = "10" + 5 * 2;
```

First:

```text
5 * 2
 ↓
10
```

Then:

```text
"10" + 10
```

Since `+` is now operating with a string:

```text
"1010"
```

So:

```js
result === "1010"
```

Same operators, different operand arrangement, different result.

---

# 4.49 A More Difficult Example

Consider:

```js
const result = 1 + 2 * 3 === 7;
```

First precedence:

```text
1 + (2 * 3)
```

Then:

```text
1 + 6
 ↓
7
```

Then comparison:

```text
7 === 7
```

Result:

```text
true
```

So:

```js
result === true
```

The evaluation pipeline is:

```text
2 * 3
 ↓
6
 ↓
1 + 6
 ↓
7
 ↓
7 === 7
 ↓
true
```

---

# 4.50 Combining `&&` and `||`

Consider:

```js
const result = false || true && "hello";
```

`&&` has higher precedence than `||`.

So:

```text
false || (true && "hello")
```

Then:

```text
true && "hello"
 ↓
"hello"
```

Then:

```text
false || "hello"
 ↓
"hello"
```

Therefore:

```js
result === "hello"
```

Not:

```text
true
```

and not:

```text
false
```

This demonstrates why understanding that logical operators return operands is essential.

---

# 4.51 `??` and `||` Cannot Be Mixed Arbitrarily

There is an important syntax rule:

```js
a ?? b || c
```

without explicit grouping is not allowed.

JavaScript requires parentheses to make the intended grouping explicit.

For example:

```js
(a ?? b) || c
```

or:

```js
a ?? (b || c)
```

This is intentional because unrestricted mixing could make the fallback semantics difficult to reason about.

---

# 4.52 Optional Chaining Short-Circuits

Consider:

```js
user?.profile.name
```

The optional chain only protects the access immediately associated with the chain.

For deeply nested access, the safer form is:

```js
user?.profile?.name
```

This distinction matters.

If `user` exists but:

```js
user.profile === null
```

then:

```js
user?.profile.name
```

can still fail because `.name` isn't itself optional.

Whereas:

```js
user?.profile?.name
```

safely handles both nullish points.

---

# 4.53 The Big Picture

At this point, we can connect the entire section:

```text
                         EXPRESSION
                             │
                             ↓
                     Identify operators
                             │
                             ↓
                       Precedence
                             │
                             ↓
                       Associativity
                             │
                             ↓
                    Evaluate operands
                             │
                             ↓
                     Type conversion?
                       /           \
                     yes           no
                      │             │
                      ↓             ↓
                 Coercion       Existing types
                      │             │
                      └──────┬──────┘
                             ↓
                     Apply operator
                             │
                             ↓
                    Produce a value
```

And logical operators add:

```text
&& / ||
    ↓
Evaluate left operand
    ↓
Truthiness
    ↓
Short-circuit?
   /       \
 yes       no
  │         │
return     evaluate
operand    right
```

While:

```text
?? 
 ↓
null / undefined?
 ↓
fallback
```

and:

```text
?.
 ↓
null / undefined?
 ↓
short-circuit
```

---

# 4.54 SDE-2 High-Probability Questions

These are the questions I'd prioritize rather than trying to memorize every operator equally.

### Expressions

1. What is an expression?
2. What is the difference between an expression and a statement?
3. Is assignment an expression in JavaScript?
4. Why does `a = b = 10` work?
5. What happens during expression evaluation?

### Arithmetic & coercion

6. Why does `"10" - 2` produce `8`?
7. Why does `"10" + 2` produce `"102"`?
8. Explain:

```js
1 + 2 + "3"
```

9. Explain:

```js
"1" + 2 + 3
```

10. What is the difference between `%` and mathematical modulo?
11. What happens when arithmetic involves `NaN` or `Infinity`?

### Equality

12. Difference between `==` and `===`.
13. Why is `null == undefined` true?
14. Why is `null == 0` false?
15. Why is `NaN === NaN` false?
16. Difference between `===` and `Object.is()`.
17. How does object equality work?

### Logical operators

18. Do `&&` and `||` return Booleans?
19. Explain short-circuit evaluation.
20. Why is this useful in production code?

```js
condition && doSomething();
```

21. What is the difference between `||` and `??`?
22. Why does:

```js
0 || 10
```

return `10`, while:

```js
0 ?? 10
```

returns `0`?

### Optional chaining

23. What does `?.` do?
24. How does optional chaining short-circuit?
25. Difference between:

```js
user?.profile.name
```

and:

```js
user?.profile?.name
```

26. How do `?.` and `??` commonly work together?

### Unary operators

27. What does unary `+` do?
28. Difference between `++x` and `x++`.
29. What does `delete` actually delete?
30. What does `void` return?
31. Why is `typeof` considered a unary operator?

### Precedence & associativity

32. What is operator precedence?
33. What is associativity?
34. Difference between precedence and evaluation order?
35. Evaluate:

```js
2 + 3 * 4
```

36. Evaluate:

```js
10 - 5 - 2
```

37. Evaluate:

```js
a = b = 10
```

38. Why is:

```js
false || true && "hello"
```

equal to `"hello"`?

---

# 4.55 The Mental Model to Remember

For SDE-2 interviews, don't memorize an enormous precedence table.

Instead, understand this hierarchy:

```text
                EXPRESSION
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      Operators             Values
          │
   ┌──────┼─────────────┐
   ↓      ↓             ↓
Arithmetic Comparison   Logical
   │          │           │
   ↓          ↓           ↓
 coercion   equality   short-circuit
                          │
                   ┌──────┼──────┐
                   ↓      ↓      ↓
                  &&      ||     ??
                                   │
                                   ↓
                            nullish fallback
```

And always reason through expressions in this order:

```text
1. What are the operands?
2. What is the operator?
3. What is the precedence?
4. What is the associativity?
5. Are operands coerced?
6. Does the operator short-circuit?
7. What value does the operator actually return?
8. Is there a side effect?
```

That framework is much more powerful than memorizing individual "tricky JavaScript outputs."

**The next natural step after this is Functions & Scope**, where these expression rules meet function declarations, function expressions, parameters, return values, closures, lexical scope, `this`, and execution contexts.
