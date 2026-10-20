# SDE-2 Questions

These are the ones I'd prioritize from this section.

### Types

1. What are JavaScript's primitive types?
2. What is the difference between primitive and object values?
3. Why is JavaScript dynamically typed?
4. Is a function a primitive or an object?
5. Why is `typeof null === "object"`?
6. What is a Symbol used for?
7. Why was BigInt introduced?
8. Why can't you directly mix `BigInt` and `Number`?

### `typeof` / `instanceof`

9. What does `typeof` actually tell you?
10. Why does `typeof []` return `"object"`?
11. Why does `typeof function(){}` return `"function"`?
12. How does `instanceof` work?
13. What's the difference between `typeof` and `instanceof`?
14. Why can `instanceof` fail across realms?
15. Why is `Array.isArray()` preferable for arrays?
16. What does `Object.prototype.toString.call()` provide?

### Truthiness

17. What are JavaScript's falsy values?
18. Is `[]` falsy?
19. Is `{}` falsy?
20. Why does `Boolean([])` return `true`?
21. What's the difference between truthiness and equality?

### Coercion

22. What is type coercion?
23. What's the difference between explicit and implicit coercion?
24. Explain:

```js
"5" + 2
```

25. Explain:

```js
"5" - 2
```

26. Explain:

```js
true + 1
```

27. Explain:

```js
null + 1
```

28. Explain:

```js
undefined + 1
```

### `null` / `undefined`

29. What's the difference between `null` and `undefined`?
30. Why is:

```js
null == undefined
```

true?

31. Why is:

```js
null === undefined
```

false?

### Numeric edge cases

32. What is `NaN`?
33. Why is:

```js
typeof NaN
```

`"number"`?

34. Why does:

```js
NaN === NaN
```

return `false`?

35. How do you correctly check for NaN?
36. What is `Infinity`?
37. What is `-0`?
38. Difference between:

```js
===
```

and:

```js
Object.is()
```

39. Why does:

```js
0.1 + 0.2
```

not exactly equal `0.3`?

40. What is IEEE-754 and how does it affect JavaScript numbers?

Absolutely. I’ll answer these at an **SDE-2 interview level**: not just the output, but the reasoning behind it. I’ll keep the structure aligned with your notes. 

# Answers

## Types

### 1. What are JavaScript's primitive types?

JavaScript has **7 primitive types**:

```text
string
number
bigint
boolean
undefined
symbol
null
```

Everything else is an **object value**.

```js
typeof "hello"     // "string"
typeof 10          // "number"
typeof 10n         // "bigint"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof Symbol()    // "symbol"
typeof null        // "object"  ← historical behavior
```

### 2. Difference between primitive and object values?

The key distinction is that primitives represent **individual immutable values**, while objects are **reference values** that can have properties and can be mutated. 

```js
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
```

But:

```js
const a = { count: 10 };
const b = a;

b.count = 20;

console.log(a.count); // 20
```

Because:

```text
Primitive:

a → 10
b → 10

Object:

a ──┐
    ├──→ { count: 20 }
b ──┘
```

This becomes extremely important with **function arguments, mutation, copying, React state, caching, and APIs**.

### 3. Why is JavaScript dynamically typed?

Because **variables/bindings aren't permanently associated with a type; the values they refer to have runtime types**. 

```js
let value = 10;

value = "hello";
value = true;
```

The same binding can refer to values of different types.

Compare with a statically typed language:

```cpp
int value = 10;
// value = "hello"; // compilation error
```

JavaScript determines the relevant type information at runtime.

### 4. Is a function a primitive or an object?

**Object.**

Functions are objects with special **callable behavior**.

```js
function hello() {}

typeof hello; // "function"
```

Notice the interesting part:

```js
typeof hello // "function"
```

even though functions are objects.

JavaScript gives functions this special `typeof` result. 

### 5. Why is `typeof null === "object"`?

Because of a **historical behavior in JavaScript**.

```js
typeof null === "object"; // true
```

But:

```text
null = primitive
```

It is **not actually an object**. 

So don't say in an interview:

> "`null` is an object."

Say:

> "`null` is a primitive, but `typeof null` returns `"object"` because of a historical JavaScript behavior."

### 6. What is a Symbol used for?

`Symbol` creates a **unique primitive identifier**.

```js
const id1 = Symbol("id");
const id2 = Symbol("id");

console.log(id1 === id2); // false
```

Even though both have the same description, they're different symbols.

A major use case is creating unique object property keys:

```js
const id = Symbol("id");

const user = {
  [id]: 123
};
```

Symbols are also used internally by JavaScript protocols, such as:

```js
Symbol.iterator
```

which is fundamental to `for...of` and iterables. 

### 7. Why was BigInt introduced?

JavaScript's normal `number` uses IEEE-754 double precision and cannot represent every integer beyond:

```js
Number.MAX_SAFE_INTEGER
// 9007199254740991
```

For example:

```js
9007199254740992 === 9007199254740993
// true
```

That's a precision problem.

BigInt allows integers beyond this safe range:

```js
const id = 9007199254740993n;
```

The `n` means it's a BigInt. 

### 8. Why can't you directly mix BigInt and Number?

Because they're **different numeric types with different semantics**.

```js
10n + 10
```

throws:

```text
TypeError
```

You need explicit conversion:

```js
10n + BigInt(10);
```

or:

```js
Number(10n) + 10;
```

But converting BigInt to Number can lose precision for sufficiently large values. 

# `typeof` / `instanceof`

### 9. What does `typeof` actually tell you?

`typeof` returns a **string representing a runtime type category**.

```js
typeof "hello"     // "string"
typeof 10          // "number"
typeof true        // "boolean"
typeof 10n         // "bigint"
typeof Symbol()    // "symbol"
typeof undefined   // "undefined"
```

For objects:

```js
typeof {}          // "object"
typeof []          // "object"
typeof null        // "object"
typeof function(){} // "function"
```

So `typeof` is useful for **basic runtime type identification**, but it isn't a complete type-checking mechanism. 

### 10. Why does `typeof []` return `"object"`?

Because arrays are objects.

```js
const arr = [];

typeof arr;
// "object"
```

`typeof` doesn't distinguish between:

```text
Array
Date
Map
Set
ordinary Object
```

They all generally produce:

```text
"object"
```

For arrays, use:

```js
Array.isArray(arr);
```

### 11. Why does `typeof function(){}` return `"function"`?

Functions are objects, but they have **callable behavior**.

JavaScript therefore gives callable function values a special `typeof` result:

```js
typeof function() {}
// "function"
```

So:

```text
Function
   ↓
Object
   +
Callable behavior
   ↓
typeof → "function"
```

This is a special case of `typeof`. 

### 12. How does `instanceof` work?

`instanceof` checks whether the prototype chain of the left-hand object contains the `prototype` property of the right-hand constructor/function, according to `instanceof` semantics. 

```js
function Animal() {}

const dog = new Animal();

dog instanceof Animal;
// true
```

Conceptually:

```text
dog
 ↓
Animal.prototype
 ↓
Object.prototype
 ↓
null
```

Since `Animal.prototype` is found in the chain:

```js
dog instanceof Animal // true
```

### 13. Difference between `typeof` and `instanceof`?

Think:

```text
typeof
   ↓
"What basic runtime category is this?"

instanceof
   ↓
"Does this object's prototype chain contain this constructor's prototype?"
```

Example:

```js
const arr = [];

typeof arr;
// "object"

arr instanceof Array;
// true
```

So `typeof` is about a broad runtime category, whereas `instanceof` is about a **prototype relationship**. 

### 14. Why can `instanceof` fail across realms?

Different JavaScript realms—such as different browser iframes—can have **different intrinsic constructors and prototypes**.

Conceptually:

```text
Main Window
Array
  ↓
Array.prototype

Iframe
Array
  ↓
Different Array.prototype
```

An array created inside the iframe may therefore not satisfy:

```js
iframeArray instanceof Array
```

using the main window's `Array`.

This is why `instanceof` isn't a universal type-checking mechanism. 

### 15. Why is `Array.isArray()` preferable for arrays?

Because it directly answers the question:

> "Is this value an Array?"

```js
Array.isArray([]);
// true

Array.isArray({});
// false
```

It also works correctly for arrays across realms, unlike relying on the current realm's `Array.prototype` relationship.

So:

```js
Array.isArray(value)
```

is the preferred array check. 

### 16. What does `Object.prototype.toString.call()` provide?

It provides a more specific **built-in object classification** than `typeof`.

```js
Object.prototype.toString.call([]);
// "[object Array]"

Object.prototype.toString.call(new Date());
// "[object Date]"

Object.prototype.toString.call(null);
// "[object Null]"
```

Compare:

```js
typeof [];
// "object"
```

versus:

```js
Object.prototype.toString.call([]);
// "[object Array]"
```

So the mental model is:

```text
typeof
→ basic runtime category

instanceof
→ prototype relationship

Object.prototype.toString.call()
→ built-in object classification
```



# Truthiness

### 17. What are JavaScript's falsy values?

The falsy values are:

```text
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is truthy. 

A useful interview memory:

> **false, zeroes, empty string, null, undefined, NaN**

### 18. Is `[]` falsy?

**No.**

```js
Boolean([]);
// true
```

An empty array is still an object, and objects are truthy.

```js
if ([]) {
  console.log("runs");
}
```

It runs.

### 19. Is `{}` falsy?

**No.**

```js
Boolean({});
// true
```

Objects are truthy, even empty objects.

### 20. Why does `Boolean([])` return `true`?

Because **truthiness is not based on whether a collection is empty**.

JavaScript doesn't say:

> "Empty array → false."

Instead, objects are truthy:

```js
Boolean([]); // true
Boolean({}); // true
```

This is different from languages that treat empty collections as false.

### 21. Difference between truthiness and equality?

They're completely different concepts.

**Truthiness** asks:

> What happens when this value is converted to Boolean?

```js
Boolean([]); // true
```

**Equality** asks:

> Are these two values considered equal?

```js
[] === true; // false
```

So:

```text
Boolean([]) → true

[] === true → false
```

Truthy does **not** mean:

```js
value === true
```

This distinction is a common interview trap. 

# Coercion

### 22. What is type coercion?

**Type coercion is the conversion of a value from one type to another according to JavaScript's conversion rules.**

It can happen explicitly:

```js
Number("42");
```

or implicitly:

```js
"42" - 1;
```

The second one causes JavaScript to convert `"42"` into a number because subtraction requires numeric operands. 

### 23. Explicit vs implicit coercion?

**Explicit:**

You tell JavaScript to convert:

```js
Number("42");
String(42);
Boolean(42);
```

**Implicit:**

JavaScript converts automatically because the operation/context requires it:

```js
"42" - 1;
// 41
```

A good production principle:

> Prefer explicit conversion when it makes your intent clearer, but understand implicit coercion because JavaScript uses it extensively. 

### 24. Explain `"5" + 2`

Result:

```js
"52"
```

Why?

`+` is special because it can mean either:

```text
numeric addition
```

or:

```text
string concatenation
```

Because one operand is a string, the operation proceeds with string concatenation:

```text
"5" + 2
 ↓
"5" + "2"
 ↓
"52"
```



### 25. Explain `"5" - 2`

Result:

```js
3
```

Unlike `+`, subtraction is a numeric operation.

So:

```text
"5" - 2
 ↓
Number("5") - 2
 ↓
5 - 2
 ↓
3
```



### 26. Explain `true + 1`

Result:

```js
2
```

For numeric addition, Boolean conversion to number occurs:

```text
true
 ↓
1

1 + 1
 ↓
2
```

Therefore:

```js
true + 1;
// 2
```

This follows JavaScript's numeric coercion rules.

### 27. Explain `null + 1`

Result:

```js
1
```

Because:

```js
Number(null);
// 0
```

Therefore:

```text
null + 1
 ↓
0 + 1
 ↓
1
```

The source notes specifically highlight this distinction from `undefined`. 

### 28. Explain `undefined + 1`

Result:

```js
NaN
```

Because:

```js
Number(undefined);
// NaN
```

Therefore:

```text
undefined + 1
 ↓
NaN + 1
 ↓
NaN
```

This is one of the important `null` vs `undefined` coercion differences. 

# `null` / `undefined`

### 29. Difference between `null` and `undefined`?

A useful mental model:

```text
undefined
→ value hasn't been provided / produced by default language semantics

null
→ intentional representation of absence
```

Examples:

```js
let x;
console.log(x);
// undefined
```

versus:

```js
let user = null;
```

which explicitly represents the absence of a user. 

But don't treat this as an absolute rule—JavaScript has historical inconsistencies.

### 30. Why is `null == undefined` true?

Because the **loose equality (`==`) algorithm has a specific rule treating `null` and `undefined` as equal to each other**.

```js
null == undefined;
// true
```

This doesn't mean they're the same type or same value.

It means the **loose equality algorithm considers this pair equal**.

### 31. Why is `null === undefined` false?

Because strict equality doesn't perform that loose-equality coercive equivalence.

They are different primitive values/types:

```js
typeof null       // "object" (historical typeof result)
typeof undefined  // "undefined"
```

Therefore:

```js
null === undefined;
// false
```

The key interview distinction:

```text
==   → coercive equality rules

===  → strict equality
```



# Numeric Edge Cases

### 32. What is `NaN`?

`NaN` means:

> **Not-a-Number**

It represents a numeric value indicating that an operation did not produce a meaningful numeric result.

Examples:

```js
Number("hello");
// NaN

0 / 0;
// NaN
```

Interestingly, `NaN` itself belongs to JavaScript's `number` type. 

### 33. Why is `typeof NaN === "number"`?

Because `NaN` isn't a separate JavaScript type.

It is a **special value within the `number` type**.

```js
typeof NaN;
// "number"
```

Think:

```text
number
├── normal numbers
├── Infinity
├── -Infinity
├── NaN
└── -0
```

This follows from JavaScript's IEEE-754 numeric model. 

### 34. Why does `NaN === NaN` return false?

Because `NaN` has special equality semantics.

```js
NaN === NaN;
// false
```

Even:

```js
NaN == NaN;
// false
```

So never do:

```js
value === NaN
```

to detect NaN. 

### 35. How do you correctly check for NaN?

Use:

```js
Number.isNaN(value);
```

Example:

```js
Number.isNaN(NaN);
// true

Number.isNaN("hello");
// false
```

This is preferable to the global `isNaN()` because the global version performs coercion.

```js
Number.isNaN(value)
```

is therefore the safer, more precise check. 

### 36. What is `Infinity`?

`Infinity` represents positive infinity.

```js
1 / 0;
// Infinity
```

And:

```js
-1 / 0;
// -Infinity
```

They are still values of type `number`:

```js
typeof Infinity;
// "number"
```

You can check whether a number is finite using:

```js
Number.isFinite(value);
```



### 37. What is `-0`?

JavaScript has both:

```text
+0
-0
```

Because its numbers follow IEEE-754 floating-point semantics.

Interestingly:

```js
-0 === 0;
// true
```

But:

```js
Object.is(-0, 0);
// false
```

So strict equality treats them as equal, while `Object.is()` distinguishes them. 

### 38. Difference between `===` and `Object.is()`?

For most ordinary values, they behave similarly.

The important differences are:

```js
NaN === NaN
// false

Object.is(NaN, NaN)
// true
```

And:

```js
-0 === 0
// true

Object.is(-0, 0)
// false
```

So:

```text
===

NaN vs NaN → false
-0 vs +0   → true
```

while:

```text
Object.is()

NaN vs NaN → true
-0 vs +0   → false
```



**Interview answer:**

> `Object.is()` provides a slightly different notion of sameness than strict equality, particularly for `NaN` and signed zero.

### 39. Why does `0.1 + 0.2` not exactly equal `0.3`?

Because JavaScript's ordinary `number` values use **finite-precision binary floating-point representation**.

Many decimal fractions cannot be represented exactly in binary.

Conceptually:

```text
0.1
 ↓
binary floating-point
 ↓
approximation

0.2
 ↓
binary floating-point
 ↓
approximation

approximation + approximation
 ↓
0.30000000000000004
```

Therefore:

```js
0.1 + 0.2 === 0.3;
// false
```

This isn't a JavaScript bug; it's a consequence of binary floating-point representation. 

### 40. What is IEEE-754 and how does it affect JavaScript numbers?

**IEEE-754** is the standard used for floating-point representation. JavaScript's ordinary `number` uses **double-precision IEEE-754 floating-point semantics**. 

The SDE-2 mental model is:

```text
JavaScript number
       ↓
IEEE-754 double precision
       ↓
finite binary representation
       ↓
some decimal fractions cannot be represented exactly
       ↓
rounding / approximation
       ↓
precision issues
```

This explains:

```js
0.1 + 0.2
// 0.30000000000000004
```

and also contributes to the safe-integer limitation:

```js
Number.MAX_SAFE_INTEGER
// 9007199254740991
```

Beyond that range, not every integer can be represented exactly. 

For production systems involving money or other precision-sensitive values, common approaches include **integer minor units, decimal arithmetic libraries, or database `DECIMAL`/`NUMERIC` types**, depending on the domain. 

# 🎯 SDE-2 Mental Model

Don't memorize 40 isolated facts. Connect them:

```text
                 JAVASCRIPT VALUES
                        │
             ┌──────────┴──────────┐
             ↓                     ↓
        PRIMITIVES              OBJECTS
             │                     │
             ↓                     ↓
       runtime type          prototype rules
             │
             ↓
         typeof /
       instanceof /
       toString.call()
             │
             ↓
          COERCION
             │
       ┌─────┼─────┐
       ↓     ↓     ↓
    String Number Boolean
       │     │     │
       └─────┼─────┘
             ↓
        OPERATOR RULES
             │
             ↓
           RESULT
```

And for numbers:

```text
                  number
                    │
                    ↓
                IEEE-754
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
      precision     NaN    Infinity
          │
          ↓
         -0
```

This is the real SDE-2 understanding: **JavaScript isn't producing "weird" results randomly. Runtime types, coercion rules, operator semantics, prototype relationships, and IEEE-754 representation are interacting according to defined rules.** 

The natural next step is **Operators & Expressions**, because it lets you derive the nasty interview questions instead of memorizing them:

```js
[] == false
"0" == false
null == 0
undefined == 0
[] + []
{} + []
1 + true
1 + "2" + 3
```

Those are exactly where your understanding of **type conversion + `ToPrimitive` + operator-specific rules + equality algorithms** gets tested. 
