# 11. Strings, Numbers, Dates & Built-in Objects

This chapter is slightly different from the previous ones.

Previously, we studied **language mechanisms** such as:

```text
Types → Operators → Control Flow → Functions → Scope → Objects → Classes → Arrays
```

Now we're moving into JavaScript's **standard built-in objects and APIs**.

For SDE-2, don't try to memorize every method. The goal is to understand:

> **What abstraction does the built-in provide, what type of value does it operate on, what does it return, does it mutate, and what edge cases does it have?**

A useful structure is:

```text
JavaScript Built-ins
│
├── Text
│   └── String
│
├── Numbers
│   ├── Number
│   └── Math
│
├── Time
│   └── Date
│
└── General-purpose collections/data
    ├── RegExp
    ├── JSON
    ├── Map
    ├── Set
    ├── WeakMap
    └── WeakSet
```

---

# 11.1 Strings

A JavaScript string represents a sequence of UTF-16 code units.

```js
const name = "Danishan";
```

Strings are **primitive values**.

This is important because although strings have methods:

```js
name.toUpperCase();
```

`name` itself isn't an instance of a mutable string object.

JavaScript temporarily provides the corresponding object behavior when you access string methods.

---

# 11.2 Creating Strings

The most common approach is a string literal:

```js
const a = "hello";
const b = 'hello';
```

Template literals use backticks:

```js
const c = `hello`;
```

You can technically create a String object:

```js
const d = new String("hello");
```

But this produces an object rather than a primitive string.

```js
typeof a; // "string"
typeof d; // "object"
```

In normal application code, prefer primitive strings.

---

# 11.3 Template Literals

Template literals allow interpolation:

```js
const name = "Danishan";
const age = 25;

const message = `My name is ${name} and I am ${age}.`;
```

Instead of:

```js
const message =
    "My name is " + name + " and I am " + age + ".";
```

Template literals also support multiline strings:

```js
const text = `
Hello
World
`;
```

The important conceptual point:

```text
${expression}
       ↓
expression is evaluated
       ↓
converted into string representation
```

So this isn't limited to simple variable insertion.

```js
`${2 + 3}`;
```

produces:

```text
"5"
```

---

# 11.4 Strings Are Immutable

This is extremely important.

```js
const name = "Danishan";

name[0] = "X";
```

doesn't modify the string.

Strings are immutable primitive values.

Methods such as:

```js
toUpperCase()
slice()
replace()
trim()
```

return new string values rather than modifying the original string.

For example:

```js
const name = "danishan";

const upper = name.toUpperCase();

console.log(name);  // "danishan"
console.log(upper); // "DANISHAN"
```

Mental model:

```text
"danishan"
     │
     └── toUpperCase()
              ↓
          "DANISHAN"
```

The original remains unchanged.

---

# 11.5 String Length

```js
const text = "hello";

text.length;
```

returns:

```text
5
```

But there is an important Unicode caveat.

`length` counts **UTF-16 code units**, not necessarily human-perceived characters.

For example:

```js
const emoji = "😀";

emoji.length;
```

is:

```text
2
```

even though the user sees one Unicode character.

This becomes important when dealing with internationalized text.

---

# 11.6 Searching Strings

Common methods include:

```js
includes()
startsWith()
endsWith()
indexOf()
lastIndexOf()
```

Example:

```js
const text = "JavaScript is powerful";

text.includes("Script");
```

returns:

```text
true
```

`indexOf()` returns the position:

```js
text.indexOf("Script");
```

while:

```js
text.indexOf("Python");
```

returns:

```text
-1
```

A common mistake is treating `indexOf()` as boolean:

```js
if (text.indexOf("Java")) {
    // ...
}
```

Because index `0` is falsy.

Prefer:

```js
if (text.includes("Java")) {
}
```

or explicitly:

```js
if (text.indexOf("Java") !== -1) {
}
```

---

# 11.7 Splitting Strings

`split()` converts a string into an array.

```js
const text = "apple,banana,orange";

const fruits = text.split(",");
```

Result:

```js
["apple", "banana", "orange"]
```

This connects directly to our previous **Arrays** chapter.

```text
String
  ↓
split()
  ↓
Array
```

You can also split into individual UTF-16 code units:

```js
"hello".split("");
```

But remember that this does not necessarily correspond to user-perceived Unicode characters.

---

# 11.8 Replacing Strings

`replace()` replaces a match:

```js
const text = "Hello World";

const result = text.replace("World", "JavaScript");
```

Result:

```text
"Hello JavaScript"
```

A major distinction:

```text
replace()
replaceAll()
```

`replace()` generally replaces the first matching string occurrence when given a string search value.

```js
"a a a".replace("a", "x");
```

produces:

```text
"x a a"
```

Whereas:

```js
"a a a".replaceAll("a", "x");
```

produces:

```text
"x x x"
```

Regular expressions provide another replacement mechanism.

---

# 11.9 Useful String Formatting Operations

Common methods:

```text
trim()
trimStart()
trimEnd()

toUpperCase()
toLowerCase()

padStart()
padEnd()

repeat()
```

For example:

```js
const value = "42";

value.padStart(5, "0");
```

gives:

```text
"00042"
```

---

# 11.10 Numbers

JavaScript has one primary numeric primitive:

```js
number
```

Unlike languages such as Java or C++, JavaScript's ordinary `number` doesn't distinguish between:

```text
int
float
double
```

for ordinary numeric values.

JavaScript numbers use the IEEE-754 double-precision floating-point format.

---

# 11.11 Number Basics

```js
const a = 10;
const b = 10.5;
const c = -20;
```

All are:

```text
number
```

There is also:

```text
bigint
```

for arbitrary-size integers, which we studied in the Data Types chapter.

---

# 11.12 Number Parsing

JavaScript provides:

```js
Number()
parseInt()
parseFloat()
```

These don't all behave the same way.

### `Number()`

```js
Number("123");
```

returns:

```text
123
```

But:

```js
Number("123abc");
```

returns:

```text
NaN
```

### `parseInt()`

```js
parseInt("123abc", 10);
```

returns:

```text
123
```

It parses an integer from the beginning of the string.

### `parseFloat()`

```js
parseFloat("12.5px");
```

returns:

```text
12.5
```

---

# 11.13 Always Understand the Radix

When using `parseInt`, explicitly provide the radix:

```js
parseInt("101", 2);
```

Result:

```text
5
```

because:

```text
101₂ = 5₁₀
```

For decimal parsing:

```js
parseInt("123", 10);
```

This makes your intention explicit.

---

# 11.14 `Number.isNaN()`

Recall from our type/coercion chapter:

```js
NaN
```

is a special numeric value.

But:

```js
NaN === NaN;
```

is:

```text
false
```

Use:

```js
Number.isNaN(value);
```

when you specifically want to check whether a value is the numeric `NaN`.

Example:

```js
Number.isNaN(NaN);     // true
Number.isNaN("hello"); // false
```

This differs from the global `isNaN()`, which performs coercion.

---

# 11.15 Number Safety

JavaScript numbers have a range of integers that can be represented exactly.

The maximum safe integer is:

```js
Number.MAX_SAFE_INTEGER
```

which is:

```text
9007199254740991
```

Beyond this range, integer arithmetic with `number` can lose precision.

For example:

```js
Number.MAX_SAFE_INTEGER + 1
```

and:

```js
Number.MAX_SAFE_INTEGER + 2
```

can produce the same numeric result.

When exact arbitrary-size integers are required, use:

```js
BigInt
```

---

# 11.16 `Number` Utility Methods

Useful methods include:

```js
Number.isInteger(value)
Number.isFinite(value)
Number.isNaN(value)
Number.isSafeInteger(value)
```

Example:

```js
Number.isInteger(10);   // true
Number.isInteger(10.5); // false
```

And:

```js
Number.isFinite(10);       // true
Number.isFinite(Infinity); // false
```

These methods generally avoid the implicit coercion performed by some older global functions.

---

# 11.17 `toFixed()`

Useful for formatting numbers:

```js
const price = 12.3456;

price.toFixed(2);
```

returns:

```text
"12.35"
```

Notice:

> `toFixed()` returns a **string**, not a number.

That's an important interview detail.

---

# 11.18 `Math`

`Math` is a built-in object containing mathematical constants and functions.

Examples:

```js
Math.round(4.6); // 5
Math.floor(4.6); // 4
Math.ceil(4.1);  // 5
Math.trunc(4.9); // 4
```

Other common operations:

```js
Math.max(...)
Math.min(...)
Math.abs(...)
Math.pow(...)
Math.sqrt(...)
Math.random()
```

---

# 11.19 `Math.floor()` vs `Math.trunc()`

This is a classic negative-number trap.

```js
Math.floor(-4.7);
```

returns:

```text
-5
```

while:

```js
Math.trunc(-4.7);
```

returns:

```text
-4
```

Why?

```text
floor → toward negative infinity
trunc → remove fractional part
```

---

# 11.20 Dates

JavaScript's traditional date/time abstraction is:

```js
Date
```

Example:

```js
const now = new Date();
```

A `Date` represents a particular point in time, internally based on a numeric timestamp.

You can obtain a timestamp:

```js
const timestamp = Date.now();
```

This is the number of milliseconds since:

```text
1970-01-01T00:00:00.000Z
```

the Unix epoch reference used by JavaScript timestamps.

---

# 11.21 Date Objects vs Timestamps

Compare:

```js
const date = new Date();
```

with:

```js
const timestamp = Date.now();
```

Conceptually:

```text
Date object
    ↓
object representing a time value

timestamp
    ↓
number representing that time value
```

You can convert:

```js
const date = new Date(timestamp);
```

---

# 11.22 Why Date Handling Is Tricky

The biggest issue isn't creating a `Date`.

The difficult part is:

```text
time zones
+
parsing
+
formatting
+
DST
+
local vs UTC
```

For example:

```js
date.getHours();
```

uses local time.

Whereas:

```js
date.getUTCHours();
```

uses UTC.

So always know which time basis your operation uses.

---

# 11.23 UTC vs Local Time

A `Date` represents a specific instant.

Its display can be interpreted in different time zones.

For example:

```text
Same instant
      │
      ├── UTC
      │
      ├── India Standard Time
      │
      └── New York time
```

The underlying instant doesn't change merely because you're displaying it in another time zone.

This distinction is essential for backend development.

---

# 11.24 ISO Strings

For exchanging dates between systems, ISO 8601-style strings are common:

```js
const date = new Date();

date.toISOString();
```

Example output:

```text
"2026-08-31T03:30:00.000Z"
```

The `Z` indicates UTC.

This is generally much safer for API communication than relying on locale-specific date strings.

---

# 11.25 Date Parsing

Be careful with:

```js
new Date(string)
```

String parsing can have surprising behavior depending on the format.

For machine-to-machine communication, prefer standardized representations such as ISO 8601.

For example:

```js
new Date("2026-08-31T10:00:00Z");
```

clearly identifies an instant in UTC.

---

# 11.26 `RegExp`

Regular expressions provide pattern matching.

Example:

```js
const pattern = /^\d+$/;
```

This matches a string consisting entirely of digits.

Usage:

```js
pattern.test("123");
```

returns:

```text
true
```

while:

```js
pattern.test("123abc");
```

returns:

```text
false
```

---

# 11.27 RegExp and Strings Work Together

Regular expressions can be used with string methods:

```js
text.match(regex);
text.replace(regex, replacement);
text.search(regex);
text.split(regex);
```

This makes RegExp particularly useful for:

* validation
* extraction
* replacement
* tokenization
* searching

But regex isn't automatically the best tool for every parsing problem.

For complex structured formats, a dedicated parser is often more appropriate.

---

# 11.28 `JSON`

JSON is a data interchange format, while JavaScript provides the `JSON` built-in object for converting between JavaScript values and JSON text.

Two methods are fundamental:

```js
JSON.stringify()
JSON.parse()
```

---

# 11.29 `JSON.stringify`

Converts a JavaScript value into JSON text.

```js
const user = {
    name: "Danishan",
    age: 25
};

const json = JSON.stringify(user);
```

Result:

```text
'{"name":"Danishan","age":25}'
```

Notice:

```text
JavaScript object
        ↓
JSON text
```

It is a string.

---

# 11.30 `JSON.parse`

The reverse:

```js
const json = '{"name":"Danishan","age":25}';

const user = JSON.parse(json);
```

Now:

```text
JSON text
   ↓
JavaScript value
```

This is fundamental for API communication.

---

# 11.31 JSON Is Not a Deep Clone Mechanism

A common interview answer is:

```js
const copy = JSON.parse(JSON.stringify(object));
```

This can sometimes produce a copy, but it is **not a general-purpose deep-cloning mechanism**.

It has limitations around values such as:

```text
undefined
functions
symbols
BigInt
Date
Map
Set
circular references
```

For supported values and modern environments, `structuredClone()` is often a better general-purpose deep-cloning mechanism.

But even then, the correct question is:

> **Do I actually need a deep clone?**

Blind cloning can be expensive and often indicates that object ownership/design needs reconsideration.

---

# 11.32 Map

`Map` stores key-value pairs.

```js
const users = new Map();

users.set("u1", "Danishan");
users.set("u2", "Alex");
```

Read:

```js
users.get("u1");
```

Check:

```js
users.has("u1");
```

Delete:

```js
users.delete("u1");
```

Size:

```js
users.size;
```

---

# 11.33 Why `Map` Instead of Object?

An object is also capable of storing key-value data:

```js
const users = {
    u1: "Danishan"
};
```

But `Map` is designed specifically as a general-purpose key-value collection.

Important difference:

```text
Object
    → property keys

Map
    → arbitrary JavaScript values as keys
```

For example:

```js
const key = {};

const map = new Map();

map.set(key, "value");
```

The object itself can be the key.

---

# 11.34 Map Preserves Insertion Order

```js
const map = new Map();

map.set("a", 1);
map.set("b", 2);
map.set("c", 3);
```

Iteration follows insertion order.

```js
for (const [key, value] of map) {
    console.log(key, value);
}
```

This makes `Map` useful when you need an ordered key-value collection.

---

# 11.35 Map Keys Use Identity

This is connected to our earlier discussion of references.

```js
const a = {};
const b = {};

const map = new Map();

map.set(a, "A");

map.get(a); // "A"
map.get(b); // undefined
```

Even though:

```js
a
```

and:

```js
b
```

have the same structure, they are different object identities.

This is the same reference/identity concept we studied earlier.

---

# 11.36 Set

A `Set` stores unique values.

```js
const numbers = new Set();

numbers.add(1);
numbers.add(2);
numbers.add(2);
```

Result conceptually:

```text
{1, 2}
```

The duplicate `2` isn't stored twice.

Check:

```js
numbers.has(2);
```

Delete:

```js
numbers.delete(2);
```

Size:

```js
numbers.size;
```

---

# 11.37 Removing Duplicates with Set

A common pattern:

```js
const numbers = [1, 2, 2, 3, 3, 4];

const unique = [...new Set(numbers)];
```

Result:

```text
[1, 2, 3, 4]
```

The flow is:

```text
Array
 ↓
Set
 ↓
unique values
 ↓
Spread
 ↓
Array
```

This combines our knowledge of:

```text
Arrays
+
Iterables
+
Spread
+
Set
```

---

# 11.38 Set Uses Value Identity Semantics

For primitives:

```js
new Set([1, 1, 2]);
```

stores one `1`.

For objects:

```js
const a = {};
const b = {};

const set = new Set([a, b]);
```

both are stored because:

```text
a !== b
```

They are different object identities.

---

# 11.39 WeakMap

`WeakMap` is a key-value collection where keys must be objects or non-registered symbols, and the collection does not keep those object keys alive merely because they are keys.

Example:

```js
const metadata = new WeakMap();

const user = {};

metadata.set(user, {
    lastLogin: Date.now()
});
```

The key is:

```text
user object
```

---

# 11.40 Why "Weak"?

The important concept is garbage collection.

Suppose:

```js
const map = new Map();

let user = {};

map.set(user, "metadata");

user = null;
```

The `Map` still strongly references the original object as a key.

Therefore, that object remains reachable through the `Map`.

With:

```js
const map = new WeakMap();

let user = {};

map.set(user, "metadata");

user = null;
```

there is no strong reference from the `WeakMap` that would by itself keep that object alive.

If nothing else references the object, it becomes eligible for garbage collection.

You don't manually observe the exact moment of collection.

---

# 11.41 WeakMap Use Cases

WeakMap is useful for associating metadata with objects without extending their lifetime.

Common conceptual use cases include:

```text
object metadata
private-ish per-object state
caching associated with object identity
framework/library internals
```

It is particularly useful when the metadata should disappear naturally when the associated object is no longer reachable.

---

# 11.42 WeakSet

`WeakSet` is similar in spirit.

It stores object references without keeping those objects alive merely because they are members.

```js
const processed = new WeakSet();

const request = {};

processed.add(request);
```

You can check:

```js
processed.has(request);
```

and:

```js
processed.delete(request);
```

Like `WeakMap`, it is designed around garbage-collection-friendly object associations.

---

# 11.43 Weak Collections Have Restrictions

`WeakMap` and `WeakSet` intentionally have fewer capabilities than their strong counterparts.

For example, you cannot generally iterate over all entries:

```js
for (const item of weakSet) {
}
```

isn't supported.

Why?

Because garbage collection is nondeterministic.

If enumeration were exposed, the observable contents could change based on when the garbage collector happened to run.

So the API intentionally avoids operations that would expose this nondeterministic behavior.

---

# 11.44 Map vs Object vs Set

A useful comparison:

| Collection | Primary purpose            | Keys/values                       |
| ---------- | -------------------------- | --------------------------------- |
| Object     | Structured properties      | Property keys                     |
| Map        | Key-value collection       | Arbitrary keys                    |
| Set        | Unique values              | Values only                       |
| WeakMap    | Object-associated metadata | Object/non-registered-symbol keys |
| WeakSet    | Track object membership    | Objects/non-registered symbols    |

---

# 11.45 The Built-ins as a System

Don't study these independently.

They connect to everything we've already learned.

```text
String
  ↓
primitive + immutable
  ↓
string methods
  ↓
Array
```

```text
Number
  ↓
IEEE-754
  ↓
NaN / Infinity / precision
  ↓
Math
```

```text
Date
  ↓
timestamp
  ↓
UTC / local time
  ↓
serialization
```

```text
Object
  ↓
Map / Set
  ↓
identity + references
```

```text
Object
  ↓
WeakMap / WeakSet
  ↓
garbage collection
```

```text
JavaScript value
  ↓
JSON.stringify()
  ↓
JSON text
  ↓
JSON.parse()
  ↓
JavaScript value
```

---

# 11.46 High-Probability SDE-2 Questions

I would prioritize these heavily.

### Strings

1. Are JavaScript strings mutable or immutable?
2. What does `String.prototype.length` actually count?
3. Difference between `slice`, `substring`, and `substr`.
4. Difference between `replace` and `replaceAll`.
5. Difference between `includes` and `indexOf`.
6. What are template literals?
7. How does string interpolation work?
8. Why can `"😀".length` be `2`?
9. How would you safely handle Unicode text?

### Numbers

10. How are JavaScript numbers represented?
11. Why does `0.1 + 0.2 !== 0.3`?
12. What is `Number.MAX_SAFE_INTEGER`?
13. When should you use `BigInt`?
14. Difference between `Number()`, `parseInt()`, and `parseFloat()`.
15. Why should you provide a radix to `parseInt()`?
16. Difference between `Number.isNaN()` and global `isNaN()`.
17. Difference between `Math.floor()` and `Math.trunc()`.
18. What does `toFixed()` return?

### Dates

19. How does JavaScript represent a `Date` internally?
20. What is a Unix timestamp?
21. Difference between local time and UTC.
22. Difference between `getHours()` and `getUTCHours()`.
23. Why are date/time bugs common in distributed applications?
24. Why are ISO timestamps commonly used in APIs?
25. What problems can occur when parsing date strings?

### RegExp

26. What is a regular expression?
27. Difference between `RegExp.test()` and string `match()`.
28. How does regex interact with `replace()`?
29. When should you avoid regex?

### JSON

30. Difference between a JavaScript object and JSON.
31. Difference between `JSON.stringify()` and `JSON.parse()`.
32. Why isn't `JSON.parse(JSON.stringify(obj))` a reliable general-purpose deep clone?
33. What happens to unsupported values during JSON serialization?
34. How would you handle circular references during JSON serialization?

### Collections

35. Difference between `Map` and `Object`.
36. When would you choose `Map` over an object?
37. What makes `Set` useful?
38. How would you remove duplicates from an array?
39. How does `Map` determine whether two object keys are the same?
40. What is `WeakMap`?
41. Why does `WeakMap` not support normal iteration?
42. What is the relationship between `WeakMap` and garbage collection?
43. Difference between `Map` and `WeakMap`.
44. Difference between `Set` and `WeakSet`.

---

# 11.47 SDE-2 Mental Model

The chapter can ultimately be reduced to this:

```text
                 BUILT-IN OBJECTS
                        │
       ┌────────────────┼─────────────────┐
       ↓                ↓                 ↓
     String           Number             Date
       │                │                 │
    immutable       IEEE-754          timestamp
       │                │                 │
    text APIs       precision        UTC/local
       │                │                 │
       └────────────────┴─────────────────┘

                        │
                        ↓

                COLLECTIONS / DATA
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
         Map           Set         WeakMap
          │             │             │
      key/value       unique       object
      arbitrary       values       association
                                      │
                                      ↓
                               garbage collection
```

And the most important distinctions are:

```text
String
→ immutable text

Number
→ IEEE-754 floating-point value

Date
→ point-in-time value represented through a timestamp

RegExp
→ pattern matching

JSON
→ serialization/data-interchange format

Map
→ arbitrary key/value collection

Set
→ unique-value collection

WeakMap
→ garbage-collection-friendly object association

WeakSet
→ garbage-collection-friendly object membership
```

The key SDE-2 mindset here is **not memorizing 100 built-in methods**. It is being able to look at a problem and immediately choose the right abstraction:

```text
Need text manipulation?       → String
Need numeric computation?     → Number / Math
Need an instant in time?      → Date / timestamp
Need pattern matching?        → RegExp
Need serialization?           → JSON
Need key/value lookup?        → Map
Need uniqueness?              → Set
Need object-attached metadata
without retaining objects?    → WeakMap
Need object membership
without retaining objects?    → WeakSet
```

That is the level at which these built-ins become useful engineering tools rather than just a list of APIs.
