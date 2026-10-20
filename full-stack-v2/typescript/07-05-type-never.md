# TypeScript `never`

## 1. What is `never`?

`never` is a special TypeScript type that represents a value that **can never exist**.

The easiest mental model is:

> **`never` means there is no possible value.**

For example:

```ts
function fail(message: string): never {
  throw new Error(message);
}
```

This function can never return normally.

It either:

* throws an error, or
* exits through some other non-normal control flow.

Therefore, there is no return value that could be produced by the function.

# 2. Why does TypeScript need `never`?

At first, `never` can look unnecessary.

You might think:

> "If a function throws an error, why not just use `void`?"

Because `void` and `never` describe **different situations**.

```ts
function logMessage(): void {
  console.log("Hello");
}
```

This function:

```text
starts
  ↓
executes
  ↓
finishes normally
```

It doesn't return a useful value, so its return type is `void`.

But:

```ts
function fail(): never {
  throw new Error("Failed");
}
```

behaves like:

```text
starts
  ↓
throws
  ↓
never reaches normal return
```

So:

```text
void  → function finishes without a useful value

never → function cannot complete normally
```

More importantly, `never` is not only about functions.

Its deeper purpose is to represent **impossible states in the type system**.

# 3. `never` as an impossible value

Consider:

```ts
type Status =
  | "loading"
  | "success"
  | "error";
```

There are exactly three possible values:

```ts
"loading"
"success"
"error"
```

Now handle all of them:

```ts
function handleStatus(status: Status) {
  switch (status) {
    case "loading":
      return "Loading...";

    case "success":
      return "Success!";

    case "error":
      return "Something went wrong";

    default:
      const impossible: never = status;
      return impossible;
  }
}
```

After TypeScript removes:

```text
"loading"
"success"
"error"
```

from the possible values, nothing remains.

Therefore:

```ts
status: never
```

The `default` branch represents a situation that **should be impossible**.

This is one of the most useful applications of `never`.

# 4. Exhaustive checking

The previous technique is called **exhaustiveness checking**.

A cleaner reusable version is:

```ts
function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}
```

Then:

```ts
type Status =
  | "loading"
  | "success"
  | "error";

function handleStatus(status: Status) {
  switch (status) {
    case "loading":
      return "Loading...";

    case "success":
      return "Success!";

    case "error":
      return "Something went wrong";

    default:
      return assertNever(status);
  }
}
```

This gives TypeScript a guarantee:

> Every possible `Status` must be handled.

# 5. Why exhaustive checking matters

Imagine the type changes later:

```ts
type Status =
  | "loading"
  | "success"
  | "error"
  | "cancelled";
```

But you forget to update `handleStatus()`.

Now:

```ts
default:
  return assertNever(status);
```

causes a TypeScript error.

Why?

Because `status` is no longer `never`.

It can be:

```ts
"cancelled"
```

Therefore:

```ts
assertNever(status);
```

is invalid.

This is extremely useful in large applications because adding a new union member forces developers to update all relevant logic.

# 6. `never` from a function that always throws

The simplest runtime use case is an error function:

```ts
function fail(message: string): never {
  throw new Error(message);
}
```

Usage:

```ts
function getUser(id: number) {
  if (id <= 0) {
    fail("Invalid user ID");
  }

  return {
    id,
    name: "Danish"
  };
}
```

TypeScript understands that after:

```ts
fail("Invalid user ID");
```

execution cannot continue normally.

Therefore, it can correctly reason about the code after that point.

# 7. `never` with infinite loops

A function that never terminates can also return `never`:

```ts
function runForever(): never {
  while (true) {
    console.log("Running...");
  }
}
```

The function never reaches a normal return statement.

Therefore:

```ts
runForever(); // never returns
```

This is less common in application code but demonstrates the meaning of `never`.

# 8. `never` vs `void`

This distinction is important.

## `void`

```ts
function saveUser(): void {
  console.log("User saved");
}
```

The function finishes.

There simply isn't a meaningful return value.

## `never`

```ts
function crash(): never {
  throw new Error("Application crashed");
}
```

The function does not finish normally.

### Comparison

```text
void
 ↓
Function completes
 ↓
No useful return value


never
 ↓
Function cannot complete normally
 ↓
No possible return value
```

# 9. `never` vs `unknown`

These types are almost opposites in terms of what the type system knows.

### `unknown`

Means:

> "A value exists, but I don't know its type."

```ts
let value: unknown = "hello";
```

The value exists.

You simply need to narrow it before using it:

```ts
if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

### `never`

Means:

> "There is no possible value."

```ts
let value: never;
```

You cannot normally assign anything to it:

```ts
let value: never;

value = 10;      // ❌
value = "hello"; // ❌
value = true;    // ❌
```

So:

```text
unknown → some value exists, but its type is unknown

never   → no value can exist
```

# 10. `never` vs `any`

`any` essentially tells TypeScript:

> "Don't perform type checking here."

```ts
let value: any = 10;

value.foo.bar.baz();
```

TypeScript allows it.

`never` means the exact opposite:

> "No value is possible here."

```ts
let value: never;

value = 10; // ❌
```

Therefore:

```text
any
 ↓
TypeScript accepts almost anything


never
 ↓
TypeScript accepts no value
```

# 11. `never` inside union types

Consider:

```ts
type A = string | never;
```

This simplifies to:

```ts
type A = string;
```

Why?

Because `never` contributes no possible values.

Conceptually:

```text
string | never
     ↓
string
```

There is nothing new that `never` adds to a union.

# 12. `never` inside intersection types

Now consider:

```ts
type A = string & never;
```

This becomes:

```ts
type A = never;
```

Why?

An intersection means:

> The value must satisfy both types.

But there is no value that can be `never`.

Therefore:

```text
string & never
       ↓
     never
```

This behavior becomes particularly important when working with advanced TypeScript types.

# 13. `never` in conditional types

One of the most powerful uses of `never` is in conditional types.

For example:

```ts
type OnlyStrings<T> =
  T extends string
    ? T
    : never;
```

Now:

```ts
type Result = OnlyStrings<string | number | boolean>;
```

TypeScript distributes the conditional type over the union:

```text
string  → string
number  → never
boolean → never
```

So we get:

```ts
string | never | never
```

which simplifies to:

```ts
string
```

Therefore:

```ts
type Result = string;
```

Here `never` is effectively being used to **remove types that don't match the condition**.

# 14. `never` as a type-level filter

This pattern is extremely common in advanced TypeScript.

```ts
type OnlyNumbers<T> =
  T extends number
    ? T
    : never;
```

Then:

```ts
type Result = OnlyNumbers<
  string | number | boolean
>;
```

The evaluation is approximately:

```text
string  → never
number  → number
boolean → never
```

Final result:

```ts
number
```

So you can think of:

```ts
never
```

as:

> **"Remove this type from the resulting union."**

# 15. Discriminated unions and `never`

`never` is particularly useful with discriminated unions.

Consider:

```ts
type Result =
  | {
      type: "success";
      data: string;
    }
  | {
      type: "error";
      error: string;
    };
```

Now:

```ts
function processResult(result: Result) {
  switch (result.type) {
    case "success":
      return result.data;

    case "error":
      return result.error;

    default:
      return assertNever(result);
  }
}
```

Once `"success"` and `"error"` have been handled, there is no valid `Result` left.

Therefore:

```ts
result
```

is narrowed to:

```ts
never
```

# 16. `never` and control-flow analysis

TypeScript performs control-flow analysis.

For example:

```ts
function process(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase();
  }

  if (typeof value === "number") {
    return value.toFixed();
  }

  return assertNever(value);
}
```

TypeScript understands:

```text
value
 ↓
string | number

string branch
 ↓
string handled

number branch
 ↓
number handled

remaining possibilities
 ↓
none

therefore
 ↓
never
```

This is why `never` works so well with narrowing.

# 17. A realistic API example

Suppose an API can return three states:

```ts
type ApiResponse =
  | {
      status: "success";
      data: string;
    }
  | {
      status: "error";
      message: string;
    }
  | {
      status: "loading";
    };
```

We can handle it exhaustively:

```ts
function handleResponse(response: ApiResponse) {
  switch (response.status) {
    case "success":
      return response.data;

    case "error":
      return response.message;

    case "loading":
      return "Loading...";

    default:
      return assertNever(response);
  }
}
```

If another developer later adds:

```ts
{
  status: "cancelled";
}
```

TypeScript forces us to think about that new state.

This prevents a common class of bugs where the type definition changes but business logic isn't updated.

# 18. A practical React-style example

Suppose you have application states:

```ts
type State =
  | { type: "idle" }
  | { type: "loading" }
  | { type: "success"; data: string }
  | { type: "error"; message: string };
```

You can write:

```ts
function renderState(state: State) {
  switch (state.type) {
    case "idle":
      return "Start";

    case "loading":
      return "Loading...";

    case "success":
      return state.data;

    case "error":
      return state.message;

    default:
      return assertNever(state);
  }
}
```

Now the compiler becomes a safety net when the application's state model evolves.

# 19. `never` is not "nothing"

This distinction is important.

`void` is closer to:

> "No useful return value."

`never` is:

> "There is no possible value."

For example:

```ts
function log(): void {
  return;
}
```

A `void` function can return.

But:

```ts
function fail(): never {
  throw new Error();
}
```

cannot return normally.

Therefore, `never` is better understood as an **impossible type**, not simply an empty type.

# 20. The four types together

A useful comparison:

```ts
let a: any;
let b: unknown;
let c: void;
let d: never;
```

### `any`

```text
"I don't want TypeScript to check this."
```

### `unknown`

```text
"I don't know what this value is yet."
```

### `void`

```text
"This function doesn't produce a useful value."
```

### `never`

```text
"There is no possible value here."
```

# 21. The most important use cases

You don't need to memorize every possible use of `never`.

Focus on these:

### 1. Functions that never return

```ts
function fail(message: string): never {
  throw new Error(message);
}
```

### 2. Exhaustive checking

```ts
function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}
```

### 3. Impossible states

```ts
switch (state.type) {
  // all valid states...

  default:
    return assertNever(state);
}
```

### 4. Conditional types / filtering

```ts
type OnlyStrings<T> =
  T extends string ? T : never;
```

These four cover most situations you'll encounter.

# 22. Interview answer

If an interviewer asks:

**"What is `never` in TypeScript?"**

A strong answer would be:

> "`never` represents values that can never occur. It's commonly used as the return type of functions that never complete normally, such as functions that always throw. More importantly, it can be used for exhaustive checking of union types, where after handling all possible cases TypeScript narrows the remaining state to `never`. It's also heavily used in conditional types as a way to filter types out of unions."

Then demonstrate:

```ts
type Status =
  | "loading"
  | "success"
  | "error";

function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}

function handleStatus(status: Status) {
  switch (status) {
    case "loading":
      return "Loading";

    case "success":
      return "Success";

    case "error":
      return "Error";

    default:
      return assertNever(status);
  }
}
```

That example demonstrates the **real value of `never`** much better than simply showing a function that throws.

# Final Mental Model

Remember this:

```text
                    TypeScript Types

any
│
├── "Don't check this."
│
unknown
│
├── "A value exists, but I don't know its type."
│
void
│
├── "The function finishes but has no useful result."
│
never
│
└── "No value is possible."
```

And the most important conceptual relationship is:

```text
never
  │
  ├── impossible values
  │
  ├── impossible states
  │
  ├── exhaustive checking
  │
  ├── control-flow narrowing
  │
  └── conditional-type filtering
```

**Don't memorize `never` as "a function that doesn't return."**

Memorize:

> **`never` is TypeScript's way of representing impossibility.**

The "function never returns" case is just one consequence of that idea.
