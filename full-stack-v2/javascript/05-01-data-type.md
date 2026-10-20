# 3. JavaScript Data Types & Type Coercion

This is one of the **highest-value JavaScript fundamentals for an SDE-2 interview** because it sits underneath equality, operators, conditionals, APIs, validation, serialization, and a lot of "tricky" JavaScript behavior.

The goal isn't to memorize:

> "JavaScript has 7 primitive types."

We want to understand the model:

```text
                    JAVASCRIPT VALUES
                           │
              ┌────────────┴────────────┐
              ↓                         ↓
          PRIMITIVES                  OBJECTS
              │                         │
     ┌────────┼────────┐               │
     ↓        ↓        ↓               ↓
  string   number   boolean          objects
  bigint   symbol   null
  undefined
```

Then:

```text
Value
  ↓
Type
  ↓
Conversion / Coercion
  ↓
Operators
  ↓
Result
```

# 3.1 What Is a Type in JavaScript?

A **type** describes what kind of value you're dealing with and what operations/semantics apply to it.

For example:

```js
10
```

is a number.

```js
"10"
```

is a string.

```js
true
```

is a boolean.

```js
10n
```

is a BigInt.

The important point is:

> **JavaScript variables don't have fixed types in the same way statically typed languages do; values have runtime types.**

For example:

```js
let value = 10;

value = "hello";
```

The binding `value` now refers to a string instead of a number.

This is the **dynamic typing** we discussed in Part 1.

# 3.2 The Seven Primitive Types

JavaScript has seven primitive types:

```text
string
number
bigint
boolean
undefined
symbol
null
```

Let's understand each one rather than just memorizing the list.

# 3.3 `string`

Strings represent textual data.

```js
const name = "Danishan";
const city = 'Delhi';
```

JavaScript also supports template literals:

```js
const message = `Hello ${name}`;
```

Strings are **primitive values** and are immutable.

For example:

```js
let name = "Danish";
```

You cannot mutate the existing string character-by-character:

```js
name[0] = "X";
```

Instead, operations produce new string values.

```js
name = name.toUpperCase();
```

Conceptually:

```text
"Danish"
   ↓
"DANISH"
```

# 3.4 `number`

JavaScript's `number` type represents both integers and floating-point numbers.

So these are all `number`:

```js
10
-20
3.14
0
-0
Infinity
-Infinity
NaN
```

This is an important difference from languages that have separate types such as:

```text
int
float
double
```

JavaScript fundamentally has one `number` type for ordinary numeric values.

# 3.5 JavaScript Numbers Are IEEE-754 Floating Point

This is where things become interesting.

JavaScript `number` is generally represented using **IEEE-754 double-precision floating-point** semantics.

This leads to one of the most famous examples:

```js
0.1 + 0.2
```

Result:

```js
0.30000000000000004
```

Not:

```text
0.3
```

Why?

Because many decimal fractions cannot be represented exactly in binary floating-point.

Conceptually:

```text
Decimal
  ↓
Binary floating-point representation
  ↓
Approximation
```

So:

```text
0.1
```

is not necessarily represented internally as exactly mathematical `0.1`.

This is not a JavaScript bug.

It's a consequence of finite binary floating-point representation.

# 3.6 Safe Integer Range

Because ordinary numbers use floating-point representation, integer precision isn't unlimited.

JavaScript defines:

```js
Number.MAX_SAFE_INTEGER
```

which is:

```text
9007199254740991
```

And:

```js
Number.MIN_SAFE_INTEGER
```

is:

```text
-9007199254740991
```

Beyond this range, integer arithmetic may no longer represent every integer exactly.

For example:

```js
9007199254740992 === 9007199254740993
```

can evaluate to:

```text
true
```

because the two mathematical integers collapse to the same representable `number` value.

This is where **BigInt** becomes important.

# 3.7 `bigint`

BigInt is used for integers that require precision beyond the safe integer range of `number`.

Example:

```js
const x = 9007199254740993n;
```

Notice:

```text
n
```

at the end.

That's a BigInt literal.

You can also construct one:

```js
const x = BigInt("9007199254740993");
```

BigInt and Number are **different types**.

Therefore:

```js
10n === 10
```

is:

```text
false
```

And you generally cannot directly mix them in ordinary arithmetic:

```js
10n + 10
```

throws a `TypeError`.

You need explicit conversion:

```js
10n + BigInt(10)
```

or:

```js
Number(10n) + 10
```

The latter can lose precision for sufficiently large BigInts.

# 3.8 `boolean`

Boolean has exactly two values:

```text
true
false
```

Example:

```js
const isAuthenticated = true;
```

Booleans are heavily involved in conditional logic:

```js
if (isAuthenticated) {
    // ...
}
```

But JavaScript also allows **non-boolean values** inside conditions because of truthiness.

For example:

```js
if ("hello") {
    console.log("runs");
}
```

We'll come back to this under **truthy/falsy**.

# 3.9 `undefined`

`undefined` represents an absence of an assigned value in several JavaScript situations.

For example:

```js
let x;

console.log(x);
```

Output:

```text
undefined
```

A function that doesn't explicitly return a value:

```js
function test() {}

console.log(test());
```

also produces:

```text
undefined
```

An absent object property:

```js
const user = {};

console.log(user.name);
```

produces:

```text
undefined
```

So `undefined` appears naturally from several language operations.

# 3.10 `null`

`null` represents an intentional absence of an object/value.

For example:

```js
const user = null;
```

This communicates:

> "There is intentionally no user value here."

A common conceptual distinction is:

```text
undefined
    ↓
value hasn't been provided / doesn't exist in this context

null
    ↓
absence has been explicitly represented
```

But don't overstate this as an absolute universal rule. JavaScript's historical semantics aren't always perfectly consistent.

# 3.11 The Famous `typeof null`

Now the classic interview question:

```js
typeof null
```

returns:

```text
"object"
```

This is a historical language behavior.

But:

```js
null
```

is a **primitive value**, not an object.

So:

```js
typeof null === "object"
```

is true, even though:

```text
null ≠ object
```

This is one of the reasons `typeof` alone is not a perfect type-testing mechanism.

# 3.12 `symbol`

A Symbol is a primitive value whose primary purpose is to provide unique identifiers.

Example:

```js
const id1 = Symbol("id");
const id2 = Symbol("id");

console.log(id1 === id2);
```

Output:

```text
false
```

Even though both were created with the same description.

Conceptually:

```text
Symbol("id") → unique symbol
Symbol("id") → another unique symbol
```

Symbols are particularly useful as object property keys.

```js
const id = Symbol("id");

const user = {
    [id]: 123
};
```

Symbols also play an important role in JavaScript protocols.

For example:

```js
Symbol.iterator
```

is central to iterables and `for...of`.

We'll study that in the **Iterators & Generators** section.

# 3.13 Objects

Everything that isn't one of the primitive types is generally an object value.

Examples:

```js
const user = {};
const numbers = [];
const date = new Date();
const regex = /abc/;
```

Functions are also objects in JavaScript, while having special callable behavior.

This gives us:

```text
Primitive
├── string
├── number
├── bigint
├── boolean
├── undefined
├── symbol
└── null

Object
├── ordinary objects
├── arrays
├── functions
├── dates
├── maps
├── sets
└── many other object types
```

# 3.14 Primitive vs Object: The Fundamental Difference

Consider:

```js
let a = 10;
let b = a;

b = 20;
```

The values are independent.

But:

```js
const a = {
    count: 10
};

const b = a;

b.count = 20;
```

Now:

```js
console.log(a.count);
```

produces:

```text
20
```

because both bindings contain a reference value pointing to the same object.

Conceptually:

```text
a ──────┐
        │
        ↓
      Object
        ↑
        │
b ──────┘
```

This distinction becomes critical for:

* function arguments
* object copying
* mutation
* closures
* state management
* React
* caching
* APIs

# 3.15 `typeof`

`typeof` returns a string describing the type category of its operand.

Examples:

```js
typeof "hello"
// "string"

typeof 10
// "number"

typeof true
// "boolean"

typeof 10n
// "bigint"

typeof Symbol()
// "symbol"

typeof undefined
// "undefined"
```

For objects:

```js
typeof {}
// "object"

typeof []
// "object"

typeof null
// "object"
```

And:

```js
typeof function () {}
```

returns:

```text
"function"
```

This is a special `typeof` result even though functions are objects.

# 3.16 Why `typeof` Isn't Enough

Consider:

```js
typeof []
```

returns:

```text
"object"
```

But you may want to know:

> Is this specifically an array?

`typeof` cannot tell you.

Similarly:

```js
typeof new Date()
```

returns:

```text
"object"
```

and:

```js
typeof new Map()
```

also:

```text
"object"
```

So we need other mechanisms.

# 3.17 `instanceof`

`instanceof` tests whether an object's prototype chain contains a constructor's prototype.

For example:

```js
const numbers = [];

numbers instanceof Array;
```

returns:

```text
true
```

Likewise:

```js
numbers instanceof Object;
```

is also:

```text
true
```

because the array's prototype chain ultimately connects to `Object.prototype`.

Another example:

```js
const date = new Date();

date instanceof Date;
```

→ `true`

# 3.18 `instanceof` Is About Prototype Relationships

This is important.

Don't define it merely as:

> "`instanceof` checks the object's type."

More accurately:

> **`instanceof` checks whether the prototype chain of the left-hand object contains the `prototype` property of the right-hand constructor/function according to the `instanceof` semantics.**

Example:

```js
function Animal() {}

const dog = new Animal();

dog instanceof Animal;
```

The relationship is approximately:

```text
dog
 ↓
Animal.prototype
 ↓
Object.prototype
 ↓
null
```

Therefore:

```js
dog instanceof Animal
```

is true.

This will become much clearer when we study **prototypes**.

# 3.19 `instanceof` Has Limitations

`instanceof` is not a universal type-checking mechanism.

For example, values from different JavaScript realms—such as objects created in different iframes—can have different intrinsic prototypes.

So:

```js
value instanceof Array
```

isn't always the best way to determine whether something is an array across realms.

For arrays, prefer:

```js
Array.isArray(value)
```

For example:

```js
Array.isArray([]);
```

→ `true`

and:

```js
Array.isArray({});
```

→ `false`

# 3.20 `Object.prototype.toString`

Another traditional type-identification technique is:

```js
Object.prototype.toString.call(value)
```

Examples:

```js
Object.prototype.toString.call([]);
```

returns:

```text
"[object Array]"
```

And:

```js
Object.prototype.toString.call(new Date());
```

returns:

```text
"[object Date]"
```

And:

```js
Object.prototype.toString.call(null);
```

returns:

```text
"[object Null]"
```

This can provide more specific built-in type information than `typeof`.

# 3.21 Three Type-Checking Mechanisms

So we now have:

```text
typeof
   ↓
basic runtime type category

instanceof
   ↓
prototype-chain relationship

Object.prototype.toString.call(...)
   ↓
built-in object classification
```

For example:

```js
const value = [];
```

Then:

```js
typeof value
// "object"

value instanceof Array
// true

Object.prototype.toString.call(value)
// "[object Array]"
```

Each mechanism answers a somewhat different question.

# 3.22 Truthy and Falsy

JavaScript doesn't require conditions to literally be `true` or `false`.

Consider:

```js
if (value) {
}
```

JavaScript converts `value` according to its boolean semantics.

Some values are **falsy**.

The principal falsy values are:

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

Everything else is truthy, including:

```js
[]
{}
"0"
"false"
```

This creates famous interview traps.

For example:

```js
Boolean([]);
```

→ `true`

And:

```js
Boolean({});
```

→ `true`

Even though:

```text
[] !== true
{} !== true
```

Truthy/falsy is about **Boolean conversion**, not equality with `true`.

# 3.23 Explicit Type Conversion

Explicit conversion means **you intentionally ask JavaScript to convert a value**.

Examples:

```js
Number("123");
String(123);
Boolean(1);
```

This is generally easier to reason about than implicit coercion.

# 3.24 String Conversion

The most direct mechanism:

```js
String(value)
```

Examples:

```js
String(123);
// "123"

String(true);
// "true"

String(null);
// "null"

String(undefined);
// "undefined"
```

Objects are more interesting:

```js
String({});
```

typically results in:

```text
"[object Object]"
```

Arrays have their own string conversion behavior:

```js
String([1, 2, 3]);
```

produces:

```text
"1,2,3"
```

These conversions become important during `+` operations.

# 3.25 Number Conversion

Use:

```js
Number(value)
```

Examples:

```js
Number("123");
// 123

Number("3.14");
// 3.14

Number(true);
// 1

Number(false);
// 0

Number(null);
// 0

Number("");
// 0
```

But:

```js
Number("hello");
```

produces:

```text
NaN
```

This is extremely important:

> **Failed numeric conversion generally produces `NaN`, not an exception.**

# 3.26 Boolean Conversion

You can explicitly convert using:

```js
Boolean(value)
```

Examples:

```js
Boolean(1);
// true

Boolean(0);
// false

Boolean("hello");
// true

Boolean("");
// false

Boolean(null);
// false

Boolean({});
// true

Boolean([]);
// true
```

This is exactly the mechanism underlying truthy/falsy conditions.

# 3.27 Implicit Coercion

Now we reach one of JavaScript's most important—and controversial—features.

JavaScript sometimes converts values automatically.

Example:

```js
"10" + 5
```

produces:

```text
"105"
```

The number is converted into a string because of the semantics of `+`.

But:

```js
"10" - 5
```

produces:

```text
5
```

because subtraction performs numeric coercion.

This is **implicit type coercion**.

# 3.28 The `+` Operator Is Special

Consider:

```js
1 + 2
```

→ `3`

But:

```js
"1" + 2
```

→ `"12"`

And:

```js
1 + "2"
```

→ `"12"`

And:

```js
"1" + true
```

→ `"1true"`

The `+` operator can perform either:

```text
numeric addition
```

or:

```text
string concatenation
```

depending on the operands after the relevant coercion steps.

This is why the operator is so frequently tested.

# 3.29 Other Arithmetic Operators

Compare:

```js
"10" - 2
```

→ `8`

```js
"10" * 2
```

→ `20`

```js
"10" / 2
```

→ `5`

because these operators require numeric operands, so strings are converted to numbers.

Hence:

```text
+  → potentially string concatenation
-  → numeric operation
*  → numeric operation
/  → numeric operation
```

# 3.30 Loose Equality `==`

Loose equality allows coercion.

For example:

```js
5 == "5"
```

is:

```text
true
```

because coercion occurs during the comparison.

But:

```js
5 === "5"
```

is:

```text
false
```

because strict equality doesn't perform the same type coercion.

For production code, `===` is generally preferred unless you have a deliberate reason to use `==`.

# 3.31 Strict Equality `===`

Strict equality compares values without the coercive behavior associated with loose equality.

```js
5 === 5
// true

5 === "5"
// false

true === 1
// false
```

It is therefore more predictable for most application code.

But there are still interesting cases involving:

```text
NaN
+0
-0
objects
```

which we'll cover shortly.

# 3.32 `null` vs `undefined`

This is an important interview distinction.

```js
let a;
```

gives:

```text
undefined
```

while:

```js
let b = null;
```

gives:

```text
null
```

A useful mental distinction:

```text
undefined
→ absence/default state produced by JavaScript semantics

null
→ explicitly represented absence
```

But they behave differently in conversions.

For example:

```js
Number(null)
```

→ `0`

while:

```js
Number(undefined)
```

→ `NaN`

And:

```js
Boolean(null)
```

→ `false`

```js
Boolean(undefined)
```

→ `false`

# 3.33 Why `null == undefined` Is True

This is one of the deliberate special cases in loose equality:

```js
null == undefined
```

→ `true`

But:

```js
null === undefined
```

→ `false`

This is a perfect demonstration of why `==` and `===` are fundamentally different.

# 3.34 `NaN`

`NaN` means:

> **Not-a-Number**

But the name is somewhat misleading because:

```js
typeof NaN
```

returns:

```text
"number"
```

That's because `NaN` is a special value within JavaScript's `number` type.

It commonly appears when a numeric operation fails to produce a meaningful numeric result.

Example:

```js
Number("hello")
```

→ `NaN`

And:

```js
0 / 0
```

→ `NaN`

# 3.35 `NaN` Is Not Equal to Itself

This is one of the most famous JavaScript facts:

```js
NaN === NaN
```

returns:

```text
false
```

Even:

```js
NaN == NaN
```

is:

```text
false
```

So don't detect NaN using:

```js
value === NaN
```

Use:

```js
Number.isNaN(value)
```

For example:

```js
Number.isNaN(NaN);
// true

Number.isNaN("hello");
// false
```

This is safer than the older global `isNaN()` because the global function performs coercion.

# 3.36 `Infinity`

JavaScript also has:

```js
Infinity
-Infinity
```

For example:

```js
1 / 0
```

produces:

```text
Infinity
```

And:

```js
-1 / 0
```

produces:

```text
-Infinity
```

They are numbers:

```js
typeof Infinity
```

→

```text
"number"
```

You can check finiteness using:

```js
Number.isFinite(value)
```

# 3.37 `-0`

Yes, JavaScript has **negative zero**.

```js
const x = -0;
```

And:

```js
x === 0
```

returns:

```text
true
```

So ordinary strict equality doesn't distinguish them.

But:

```js
Object.is(-0, 0)
```

returns:

```text
false
```

while:

```js
Object.is(-0, -0)
```

returns:

```text
true
```

This is a consequence of IEEE-754 floating-point semantics.

It's rarely important in normal application code, but it's a **classic SDE/interview edge case**.

# 3.38 `Object.is()`

`Object.is()` is useful when you need a slightly different notion of sameness.

Examples:

```js
Object.is(NaN, NaN);
// true
```

while:

```js
NaN === NaN
// false
```

And:

```js
Object.is(-0, 0);
// false
```

while:

```js
-0 === 0
// true
```

For ordinary values, `Object.is()` and `===` often agree.

The differences become important for:

```text
NaN
-0
+0
```

# 3.39 Numeric Precision

Let's examine:

```js
0.1 + 0.2
```

Result:

```text
0.30000000000000004
```

This happens because JavaScript's ordinary numbers are finite-precision binary floating-point values.

Therefore, for financial or other precision-sensitive calculations, blindly relying on binary floating-point arithmetic can be dangerous.

For example, instead of assuming:

```js
price = 10.10;
```

will behave exactly like decimal arithmetic in every operation, production systems often use strategies such as:

```text
integer minor units
decimal arithmetic libraries
database decimal/numeric types
```

depending on the domain.

This is where language knowledge becomes engineering knowledge.

# 3.40 IEEE-754 Mental Model

You don't need to become a floating-point researcher for SDE-2.

The useful model is:

```text
JavaScript number
       ↓
IEEE-754 double precision
       ↓
finite representation
       ↓
many decimal fractions
cannot be represented exactly
       ↓
rounding/approximation
```

Therefore:

```js
0.1 + 0.2 !== 0.3
```

isn't surprising once you understand the representation.

# 3.41 A Major Coercion Example

Now let's combine several concepts:

```js
console.log([] + []);
console.log([] + {});
console.log({} + []);
```

These are famous interview puzzles.

The reason they're tricky is that JavaScript may perform:

```text
object → primitive conversion
        ↓
string conversion
        ↓
operator behavior
```

For example, arrays have particular primitive conversion behavior.

Rather than memorizing every weird result, understand the process:

```text
Operand
   ↓
Does the operator require coercion?
   ↓
ToPrimitive
   ↓
possible value conversion
   ↓
operator-specific semantics
```

We'll revisit this more deeply when we study **operators and expressions**.

# 3.42 Explicit vs Implicit Conversion

Now we can distinguish:

### Explicit

```js
Number("42");
String(42);
Boolean(42);
```

You asked JavaScript to convert it.

### Implicit

```js
"42" - 1
```

JavaScript performs conversion because the operator requires it.

Or:

```js
if ("hello") {
}
```

JavaScript performs Boolean conversion for the conditional.

A useful production rule:

> **Explicit conversion generally makes intent clearer; implicit coercion should be understood even when you choose not to use it.**

# 3.43 The Type-Coercion Pipeline

Here's the mental model I'd keep:

```text
                  VALUE
                    │
                    ↓
             Operator / Context
                    │
                    ↓
          Does coercion happen?
             /            \
           No              Yes
           │                │
           ↓                ↓
       Use value       Conversion rules
                            │
                    ┌───────┼────────┐
                    ↓       ↓        ↓
                 String   Number   Boolean
                    │       │        │
                    └───────┼────────┘
                            ↓
                        Operation
                            ↓
                          Result
```

The important lesson is:

> **Coercion is not random. JavaScript has defined conversion rules.**

# 3.44 The Type System in One Diagram

```text
                         JAVASCRIPT VALUES
                                │
              ┌─────────────────┴─────────────────┐
              │                                   │
          PRIMITIVES                            OBJECTS
              │                                   │
      ┌───────┼────────┐                 ┌────────┼────────┐
      ↓       ↓        ↓                 ↓        ↓        ↓
   string   number   boolean          Object    Array   Function
   bigint   symbol   undefined         Date      Map     Set
            null
              │
              ↓
        Runtime type
              │
       ┌──────┼───────┐
       ↓      ↓       ↓
    typeof instanceof toString
       │      │       │
       └──────┴───────┘
              ↓
       Type identification
```

Then:

```text
                     VALUE
                       │
                       ↓
                  COERCION
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       String        Number       Boolean
          │            │            │
          └────────────┼────────────┘
                       ↓
                  OPERATION
                       │
                       ↓
                    RESULT
```

# 3.45 High-Probability SDE-2 Questions

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

# The SDE-2 Mental Model

Don't memorize isolated facts like:

```text
typeof null → object
NaN → number
[] → truthy
0.1 + 0.2 → weird
```

Instead, connect them:

```text
                  JAVASCRIPT TYPE SYSTEM
                           │
                           ↓
                    Runtime values
                           │
              ┌────────────┴────────────┐
              ↓                         ↓
         Primitives                   Objects
              │                         │
              ↓                         ↓
       runtime type rules        prototype rules
              │
              ↓
          Conversion
              │
       ┌──────┼──────┐
       ↓      ↓      ↓
    String  Number Boolean
       │      │      │
       └──────┼──────┘
              ↓
         Operator rules
              │
              ↓
            Result
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
       precision    NaN     Infinity
          │
          ↓
         -0
```

Once this model is clear, **Operators & Expressions** becomes the perfect next topic, because that's where we'll take these type/coercion rules and systematically derive the behavior of:

```text
+  -  *  /
== ===
&& || ??
! 
?:
typeof
delete
void
```

including the **really nasty coercion questions** such as:

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

Those are much more valuable to solve by understanding the coercion algorithm than by memorizing their outputs.
