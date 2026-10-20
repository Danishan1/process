# 22. Regular Expressions — SDE-II Revision

Regex is a **small language for describing patterns in text**.

For SDE-II, the goal isn't to memorize hundreds of patterns. You should understand:

> **How the regex engine interprets a pattern, how matching works, how groups/lookarounds work, and when regex becomes a performance or security problem.**

---

# 22.1 The Basic Mental Model

Suppose:

```js
const regex = /cat/;
```

and:

```js
const text = "The cat is sleeping";
```

Then:

```js
regex.test(text);
```

returns:

```text
true
```

Think of it as:

```text
Regex pattern
     ↓
"cat"
     ↓
Regex engine scans input
     ↓
Finds matching sequence
     ↓
true
```

Regex does **not** generally mean "the entire string must look like this."

It means:

> **Find a substring that satisfies this pattern.**

That's why anchoring matters.

---

# 22.2 Regex in JavaScript

You can create regex in two ways.

### Literal

```js
const regex = /hello/;
```

### Constructor

```js
const regex = new RegExp("hello");
```

The literal form is generally cleaner when the pattern is known statically.

The constructor becomes useful when the pattern is dynamically constructed.

---

# 22.3 Basic Regex Syntax

Literal characters:

```text
abc
```

matches:

```text
abc
```

Special characters have special meanings:

```text
. ^ $ * + ? ( ) [ ] { } | \
```

If you want their literal meaning, you often need escaping.

For example:

```js
/\./
```

matches an actual period.

---

# 22.4 Character Classes

A character class is written with:

```text
[...]
```

Example:

```js
/[abc]/
```

means:

> Match one character that is either `a`, `b`, or `c`.

Therefore:

```text
apple   → match
banana  → match
cat     → match
dog     → no match
```

---

# 22.5 Ranges

Instead of:

```js
/[0123456789]/
```

you can write:

```js
/[0-9]/
```

Similarly:

```js
/[a-z]/
[A-Z]
```

---

# 22.6 Negated Character Class

Put `^` immediately after `[`:

```js
/[^0-9]/
```

means:

> Match one character that is **not** a digit.

Important:

```text
^
```

has different meanings depending on position.

```js
/^hello/
```

means beginning of string.

But:

```js
/[^0-9]/
```

means "not a digit."

---

# 22.7 Common Character Classes

JavaScript regex provides shorthand classes.

### `\d`

Digit:

```js
/\d/
```

roughly:

```text
[0-9]
```

### `\D`

Not a digit.

### `\w`

Word character:

```text
letters
digits
underscore
```

Conceptually:

```text
[A-Za-z0-9_]
```

### `\W`

Not a word character.

### `\s`

Whitespace.

### `\S`

Non-whitespace.

---

# 22.8 The Dot `.`

```js
/a.c/
```

means:

```text
a
any appropriate character
c
```

For example:

```text
abc
axc
a1c
```

can match.

But be careful:

`.` doesn't literally mean "anything in every possible context"; newline handling and flags affect its behavior.

---

# 22.9 Quantifiers

Quantifiers specify **how many times** something can occur.

Important ones:

```text
*
+
?
{n}
{n,}
{n,m}
```

---

## `*`

Zero or more.

```js
/a*/
```

Matches:

```text
""
"a"
"aa"
"aaa"
```

---

## `+`

One or more.

```js
/a+/
```

Matches:

```text
"a"
"aa"
"aaa"
```

but not:

```text
""
```

---

## `?`

Zero or one.

```js
/colou?r/
```

matches:

```text
color
colour
```

---

# 22.10 Exact Quantities

```js
/a{3}/
```

means exactly three `a`s.

```js
/a{2,}/
```

means two or more.

```js
/a{2,5}/
```

means between two and five.

---

# 22.11 Greedy vs Lazy Quantifiers

This is an important SDE-II concept.

By default, quantifiers are generally **greedy**.

Consider:

```js
const regex = /".*"/;
```

Input:

```text
"hello" and "world"
```

A greedy `.*` can consume as much as possible while still allowing the complete pattern to match.

A lazy version:

```js
/".*?"/
```

tries to consume as little as possible.

So:

```text
.*   → greedy
.*?  → lazy
```

This distinction becomes very important when extracting data.

---

# 22.12 Anchors

Anchors don't consume characters.

They describe **positions**.

### `^`

Beginning of input/line depending on flags.

```js
/^hello/
```

matches:

```text
hello world
```

but not:

```text
say hello
```

---

### `$`

End of input/line depending on flags.

```js
/world$/
```

matches:

```text
hello world
```

but not:

```text
world today
```

---

# 22.13 Why Anchors Matter

Suppose you want to validate that a string contains only digits.

This:

```js
/\d+/
```

does **not** necessarily mean the entire string is digits.

For:

```text
123abc
```

it can find:

```text
123
```

For whole-input validation, you need appropriate anchoring, for example:

```js
/^\d+$/
```

Now the complete string must satisfy the pattern.

This distinction is extremely important in validation code.

---

# 22.14 Groups

Parentheses create groups:

```js
/(abc)/
```

Groups allow you to treat multiple characters as a unit.

For example:

```js
/(ab)+/
```

can match:

```text
ab
abab
ababab
```

because the quantifier applies to the entire group.

---

# 22.15 Alternation `|`

```js
/cat|dog/
```

means:

```text
cat OR dog
```

Groups help control scope:

```js
/(cat|dog)s/
```

matches:

```text
cats
dogs
```

Without understanding grouping, alternation can behave differently from what you intended.

---

# 22.16 Capturing Groups

A normal group:

```js
/(\d{4})-(\d{2})-(\d{2})/
```

captures parts of the match.

For:

```text
2026-09-07
```

you can access the captured pieces.

Example:

```js
const match =
    "2026-09-07".match(/(\d{4})-(\d{2})-(\d{2})/);

console.log(match);
```

Conceptually:

```text
full match
   │
   ├── group 1 → 2026
   ├── group 2 → 09
   └── group 3 → 07
```

---

# 22.17 Non-Capturing Groups

Sometimes you need grouping but don't need the captured value.

Use:

```js
(?:...)
```

Example:

```js
/(?:cat|dog)s/
```

This groups `cat|dog`, but doesn't create a capture group.

Why is this useful?

Because capture groups have a purpose: **extracting information**.

If you don't need extraction, non-capturing groups communicate intent and avoid unnecessary capture bookkeeping.

---

# 22.18 Capturing vs Non-Capturing

```text
(...)       → group + capture

(?:...)     → group without capture
```

Interview answer:

> "I use capturing groups when I need the matched subexpression later, and non-capturing groups when I only need grouping for precedence or quantification."

---

# 22.19 Named Capturing Groups

Modern JavaScript supports named groups.

```js
const regex =
    /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
```

Then:

```js
const match = regex.exec("2026-09-07");

match.groups.year;
match.groups.month;
match.groups.day;
```

This is often much clearer than:

```js
match[1]
match[2]
match[3]
```

especially in complex patterns.

---

# 22.20 Lookahead

A lookahead checks what comes **after** the current position without consuming it.

Positive lookahead:

```text
(?=...)
```

Example:

```js
/\d+(?=USD)/
```

Conceptually:

```text
100USD
```

matches:

```text
100
```

because `USD` must follow it, but `USD` itself isn't part of the consumed match.

Think:

```text
MATCH THIS
    +
VERIFY THAT THIS FOLLOWS
```

---

# 22.21 Negative Lookahead

```text
(?!...)
```

means:

> The following pattern must NOT exist here.

Example:

```js
/foo(?!bar)/
```

matches `foo` only when it isn't immediately followed by `bar`.

---

# 22.22 Lookbehind

Lookbehind checks what comes **before** the current position.

Positive:

```text
(?<=...)
```

Example:

```js
/(?<=\$)\d+/
```

For:

```text
$100
```

the match is:

```text
100
```

because it must be preceded by `$`.

---

# 22.23 Negative Lookbehind

```text
(?<!...)
```

means:

> The preceding characters must not match the specified pattern.

For example:

```js
/(?<!\$)\d+/
```

can match digits that aren't immediately preceded by `$`.

---

# 22.24 Lookarounds Mental Model

Don't think of lookarounds as complicated syntax.

Think:

```text
Lookahead
    ↓
"Is this true AFTER me?"

Lookbehind
    ↓
"Is this true BEFORE me?"
```

And:

```text
(?=...)   must exist after
(?!...)   must not exist after

(?<=...)  must exist before
(?<!...)  must not exist before
```

The key idea is:

> **Lookaround checks context without consuming that context as part of the match.**

---

# 22.25 Regex Flags

Flags change matching behavior.

Important JavaScript flags:

```text
g
i
m
s
u
y
d
v
```

You don't need to memorize every obscure flag, but understand the important ones.

---

## `g` — Global

```js
/hello/g
```

Find multiple matches rather than stopping after the first match in APIs where global matching applies.

---

## `i` — Ignore Case

```js
/hello/i
```

matches:

```text
hello
HELLO
Hello
hElLo
```

---

## `m` — Multiline

Changes how:

```text
^
$
```

behave with multiline input.

Without `m`, they're generally relative to the whole input.

With `m`, they can also match line boundaries.

---

## `s` — DotAll

Allows `.` to match line terminators as well.

```js
/foo.bar/s
```

---

## `u` — Unicode

Enables Unicode-aware regex semantics and Unicode code point escapes.

For example:

```js
/\u{1F600}/u
```

---

## `y` — Sticky

Sticky matching requires the match to begin at the regex's current `lastIndex`.

It's more specialized but worth recognizing.

---

# 22.26 Matching APIs

JavaScript provides several ways to use regex.

---

## `test()`

```js
/\d+/.test("123");
```

Returns:

```text
true
```

or:

```text
false
```

Use when you only need to know whether a match exists.

---

## `exec()`

```js
const match = /\d+/.exec("abc123");
```

Returns detailed match information or `null`.

Useful when you need captures.

---

## `match()`

```js
"abc123".match(/\d+/);
```

Returns match information.

Behavior differs depending on flags, especially `g`.

---

## `matchAll()`

Useful when you need **all matches plus capture groups**.

```js
const matches =
    [..."a1 b2 c3".matchAll(/([a-z])(\d)/g)];
```

This is often preferable to manually repeatedly calling `exec()`.

---

# 22.27 Replacing with Regex

```js
const result =
    "hello world".replace(/world/, "JavaScript");
```

Result:

```text
hello JavaScript
```

With `g`:

```js
"cat cat cat".replace(/cat/g, "dog");
```

gives:

```text
dog dog dog
```

---

# 22.28 Replacement Functions

The replacement doesn't have to be a string.

```js
const result = "1 2 3".replace(
    /\d+/g,
    match => String(Number(match) * 2)
);
```

Result:

```text
2 4 6
```

This is extremely useful for transformations.

---

# 22.29 Regex Validation Example

Suppose:

```text
username
```

must contain only:

```text
letters
digits
underscore
```

You could write:

```js
const usernameRegex = /^[A-Za-z0-9_]+$/;
```

Then:

```js
usernameRegex.test(username);
```

But remember:

> Regex validation should be part of input validation, not your only security boundary.

Server-side validation is still required even if the frontend validates.

---

# 22.30 Regex and Security

Regex becomes security-sensitive when:

* input is attacker-controlled
* patterns are complex
* input can be huge
* matching happens frequently

The biggest concern is often:

> **Catastrophic backtracking / ReDoS**

---

# 22.31 Catastrophic Backtracking

Some regex patterns create enormous numbers of possible paths for the engine to explore.

A classic dangerous shape is:

```js
/(a+)+$/
```

against carefully chosen long input.

The engine can repeatedly reconsider different ways of partitioning the input.

Conceptually:

```text
Input
 ↓
many possible match paths
 ↓
backtracking
 ↓
more paths
 ↓
CPU explosion
```

This can turn regex processing into a denial-of-service problem.

---

# 22.32 Why This Matters in Node.js

This is particularly important for Node.js.

Remember Point 19:

> JavaScript execution happens on the main event-loop thread unless you explicitly move work elsewhere.

So if a regex consumes huge CPU time:

```text
HTTP Request
     ↓
Regex
     ↓
CPU-heavy matching
     ↓
Event loop blocked
     ↓
Other requests wait
```

One malicious request can therefore affect unrelated requests.

This connects **regex → performance → event loop → production reliability**.

---

# 22.33 Regex Performance Rules

For production code:

### 1. Keep patterns simple

Prefer:

```js
/^\d+$/
```

over unnecessarily complex constructions.

### 2. Avoid nested ambiguous quantifiers

Patterns involving structures like:

```text
(a+)+
(a*)*
(.+)+
```

deserve careful review.

### 3. Be careful with `.*`

Especially when combined with nested structures and backtracking.

### 4. Bound input

Don't let an API accept arbitrarily large strings if regex processing is expensive.

### 5. Test worst-case input

Don't test only:

```text
"normal@example.com"
```

Test:

```text
very long input
near-match input
malformed input
repeated characters
```

---

# 22.34 Regex Is Not Always the Right Tool

This is an important engineering judgment.

Regex is excellent for:

```text
simple lexical patterns
searching
token extraction
basic validation
replacement
format detection
```

But it is often the wrong tool for:

```text
complex nested structures
full programming-language parsing
HTML parsing
JSON parsing
business-rule-heavy validation
```

For example, don't build a giant regex to parse JSON.

Use:

```js
JSON.parse()
```

when appropriate.

---

# 22.35 Regex + Business Validation

Suppose you're validating an email.

A giant regex attempting to perfectly implement every email standard can become difficult to maintain.

A practical application might instead use:

```text
basic format validation
        ↓
normalization
        ↓
business rules
        ↓
verification email
```

Regex should solve the **pattern problem**, not become your entire validation architecture.

---

# 22.36 Regex vs String Methods

Don't use regex automatically.

For:

```js
text.startsWith("hello")
```

don't write:

```js
/^hello/.test(text)
```

unless regex is actually needed.

Similarly:

```js
text.includes("cat")
```

is clearer than:

```js
/cat/.test(text)
```

Use the simplest appropriate tool.

---

# 22.37 SDE-II Mental Model

Think about regex in five layers:

```text
                 REGEX
                   │
       ┌───────────┼───────────┐
       │           │           │
     Syntax      Matching    Extraction
       │           │           │
 [abc]           test()      groups
 \d              exec()      captures
 * + ?           match()     matchAll()
 ^ $
       │
       └──────────────┐
                      │
                 Performance
                      │
             backtracking
                      │
                    ReDoS
                      │
               Event Loop
                      │
               Production Risk
```

---

# 22.38 What You Must Know for SDE-II

| Topic             | Priority | What you should know             |
| ----------------- | -------: | -------------------------------- |
| Basic syntax      |       🔴 | Literals, escapes, special chars |
| Character classes |       🔴 | `[]`, ranges, `\d`, `\w`, `\s`   |
| Quantifiers       |       🔴 | `*`, `+`, `?`, `{n,m}`           |
| Greedy/lazy       |       🔴 | `.*` vs `.*?`                    |
| Anchors           |       🔴 | `^`, `$`                         |
| Groups            |       🔴 | `()`, alternation                |
| Capturing         |       🔴 | Extract matched portions         |
| Non-capturing     |       🔴 | `(?:...)`                        |
| Lookahead         |       🔴 | Positive/negative                |
| Lookbehind        |       🟡 | Positive/negative                |
| Flags             |       🔴 | `g`, `i`, `m`, `s`, `u`          |
| `test()`          |       🔴 | Boolean matching                 |
| `exec()`          |       🔴 | Detailed matching                |
| `match()`         |       🔴 | Extract matches                  |
| `matchAll()`      |       🟡 | All matches + captures           |
| `replace()`       |       🔴 | Regex replacement                |
| ReDoS             |       🔴 | Catastrophic backtracking        |
| Regex vs strings  |       🔴 | Don't overuse regex              |

---

# 22.39 Interview Questions You Should Be Ready For

### Q1. What's the difference between `*`, `+`, and `?`?

```text
* → 0 or more
+ → 1 or more
? → 0 or 1
```

---

### Q2. Capturing vs non-capturing groups?

```text
(...)     → capture
(?:...)   → group without capture
```

---

### Q3. What is a lookahead?

> A zero-width assertion that checks what follows the current position without consuming those characters.

---

### Q4. Why are anchors important?

Because:

```js
/\d+/
```

can match a substring, while:

```js
/^\d+$/
```

requires the entire input to consist of digits.

---

### Q5. What's ReDoS?

> A denial-of-service condition where an inefficient regex causes excessive CPU consumption through catastrophic backtracking, potentially blocking the Node.js event loop.

That last part is particularly strong for a Node.js SDE-II interview.

---

# 22.40 Final Takeaway

You don't need to become a **regex wizard**.

You need to be able to look at:

```js
/^(?:https?):\/\/(?<host>[^/]+)(?:\/.*)?$/i
```

and understand roughly:

```text
^
    beginning

(?:https?)
    http or https, non-capturing group

:\/\/
    literal ://

(?<host>[^/]+)
    named capture
    one or more non-/ characters

(?:\/.*)?
    optional path

$
    end

i
    case-insensitive
```

And, more importantly, you should be able to ask:

> **Is this regex correct? Is it readable? Does it match the entire input or just a substring? Could it backtrack badly? Is regex even the right tool here?**

That is the level of **Point 22 — Regex** I'd consider sufficient for SDE-II.
