# TypeScript Advanced Types — Dynamic Type Creation & Transformation

## Quick Summary

| Feature               | What it does                                           | Mental Model                                | Example                      |
| --------------------- | ------------------------------------------------------ | ------------------------------------------- | ---------------------------- |
| `typeof`              | Creates a type from an existing value                  | "Give me the type of this value"            | `type User = typeof user`    |
| `keyof`               | Gets all property names of a type                      | "Give me the keys"                          | `keyof User`                 |
| Indexed Access        | Gets the type of a property                            | "Give me the value type at this key"        | `User["name"]`               |
| `typeof + keyof`      | Derives keys/values directly from an object            | "Object → keys/values"                      | `keyof typeof user`          |
| `as const`            | Preserves literal values and makes properties readonly | "Keep exact values"                         | `{ role: "admin" } as const` |
| Mapped Type           | Transforms properties of an existing type              | "Loop over properties"                      | `[K in keyof T]`             |
| Conditional Type      | Chooses a type based on a condition                    | "if / else for types"                       | `T extends X ? A : B`        |
| `never`               | Represents an impossible value/type                    | "No possible value"                         | `T extends X ? T : never`    |
| `infer`               | Extracts a type from another type                      | "Capture this type"                         | `Promise<infer R>`           |
| Template Literal Type | Dynamically constructs string literal types            | "Build strings from types"                  | `` `on${Capitalize<T>}` ``   |
| Utility Types         | Ready-made type transformations                        | "Reusable type operations"                  | `Partial<User>`              |
| `Partial<T>`          | Makes all properties optional                          | "Everything becomes optional"               | `Partial<User>`              |
| `Required<T>`         | Makes all properties required                          | "Remove optionality"                        | `Required<User>`             |
| `Readonly<T>`         | Makes all properties readonly                          | "Prevent reassignment"                      | `Readonly<User>`             |
| `Pick<T, K>`          | Selects specific properties                            | "Keep these keys"                           | `Pick<User, "id">`           |
| `Omit<T, K>`          | Removes specific properties                            | "Remove these keys"                         | `Omit<User, "password">`     |
| `Record<K, T>`        | Creates an object type from keys and a value type      | "These keys map to this type"               | `Record<Role, Permission>`   |
| `ReturnType<T>`       | Extracts a function's return type                      | "What does this function return?"           | `ReturnType<typeof fn>`      |
| `Parameters<T>`       | Extracts function parameters                           | "What arguments does this function accept?" | `Parameters<typeof fn>`      |
| `Awaited<T>`          | Extracts the resolved value of a Promise               | "What does this Promise resolve to?"        | `Awaited<Promise<User>>`     |

---

# 1. Why Dynamically Create Types?

Consider this object:

```ts
const user = {
  id: 101,
  name: "Danish",
  email: "danish@example.com",
  isActive: true
};
```

We could manually create its type:

```ts
type User = {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
};
```

But now we have the same information in two places.

If the object changes:

```ts
const user = {
  id: 101,
  name: "Danish",
  email: "danish@example.com",
  isActive: true,
  age: 25
};
```

the manually written `User` type is now outdated.

TypeScript allows us to derive the type directly:

```ts
type User = typeof user;
```

Now the object becomes the **source of truth**.

This is the fundamental idea behind dynamic type creation:

> **Don't duplicate information when TypeScript can derive the type automatically.**

---

# 2. `typeof` — Create a Type from a Value

Given:

```ts
const user = {
  id: 101,
  name: "Danish",
  email: "danish@example.com"
};
```

We can write:

```ts
type User = typeof user;
```

TypeScript derives:

```ts
type User = {
  id: number;
  name: string;
  email: string;
};
```

The flow is:

```text
existing value
      ↓
    typeof
      ↓
   its type
```

## Important distinction

JavaScript:

```ts
typeof user
```

returns a runtime string such as:

```ts
"object"
```

But TypeScript:

```ts
type User = typeof user;
```

uses `typeof` in a **type position** to obtain the compile-time type.

---

# 3. `keyof` — Get the Keys of a Type

Given:

```ts
type User = {
  id: number;
  name: string;
  email: string;
};
```

We can write:

```ts
type UserKeys = keyof User;
```

The result is:

```ts
type UserKeys =
  | "id"
  | "name"
  | "email";
```

Think:

```text
keyof T
   ↓
"Give me all valid property names of T."
```

This becomes extremely powerful when combined with `typeof`.

---

# 4. `typeof` + `keyof`

Given:

```ts
const user = {
  id: 101,
  name: "Danish",
  email: "danish@example.com"
};
```

We can derive its keys without manually writing them:

```ts
type UserKeys = keyof typeof user;
```

Result:

```ts
"id" | "name" | "email"
```

The process is:

```text
user
 ↓
typeof user
 ↓
{
  id: number;
  name: string;
  email: string;
}
 ↓
keyof
 ↓
"id" | "name" | "email"
```

This pattern is extremely common:

```ts
keyof typeof object
```

---

# 5. Indexed Access Types

Suppose:

```ts
type User = {
  id: number;
  name: string;
  email: string;
};
```

We can extract a property's type:

```ts
type UserId = User["id"];
```

Result:

```ts
number
```

Similarly:

```ts
type UserName = User["name"];
```

Result:

```ts
string
```

We can also access multiple properties:

```ts
type UserPrimitive =
  User["id" | "name"];
```

Result:

```ts
number | string
```

And we can get all property value types:

```ts
type UserValues =
  User[keyof User];
```

Result:

```ts
number | string
```

The mental model is:

```text
T["property"]
     ↓
type of that property
```

---

# 6. Deriving Values from an Object

Consider:

```ts
const roles = {
  admin: "ADMIN",
  user: "USER",
  guest: "GUEST"
} as const;
```

We can derive the keys:

```ts
type RoleName =
  keyof typeof roles;
```

Result:

```ts
"admin" | "user" | "guest"
```

We can derive the values:

```ts
type Role =
  typeof roles[keyof typeof roles];
```

Result:

```ts
"ADMIN" | "USER" | "GUEST"
```

This is one of the most useful TypeScript patterns:

```ts
type Values<T> = T[keyof T];
```

For example:

```ts
type Role = Values<typeof roles>;
```

---

# 7. `as const` — Preserve Exact Values

Consider:

```ts
const roles = {
  admin: "ADMIN",
  user: "USER",
  guest: "GUEST"
};
```

TypeScript generally sees the property values as:

```ts
{
  admin: string;
  user: string;
  guest: string;
}
```

But:

```ts
const roles = {
  admin: "ADMIN",
  user: "USER",
  guest: "GUEST"
} as const;
```

preserves the exact literal values:

```ts
{
  readonly admin: "ADMIN";
  readonly user: "USER";
  readonly guest: "GUEST";
}
```

Therefore:

```ts
type Role =
  typeof roles[keyof typeof roles];
```

becomes:

```ts
"ADMIN" | "USER" | "GUEST"
```

instead of simply:

```ts
string
```

---

# 8. Creating a Type from an Array

Consider:

```ts
const roles = [
  "admin",
  "user",
  "guest"
] as const;
```

We can derive the union of its values:

```ts
type Role = typeof roles[number];
```

Result:

```ts
"admin" | "user" | "guest"
```

Why `number`?

Because arrays are indexed using numbers:

```ts
roles[0]
roles[1]
roles[2]
```

So:

```ts
typeof roles[number]
```

means:

> "Give me the type of any element in this array."

---

# 9. Mapped Types

A **mapped type** allows us to iterate over the properties of an existing type and transform them.

Given:

```ts
type User = {
  id: number;
  name: string;
  email: string;
};
```

Suppose we want every property to become optional.

We can write:

```ts
type OptionalUser = {
  [Key in keyof User]?: User[Key];
};
```

Result:

```ts
type OptionalUser = {
  id?: number;
  name?: string;
  email?: string;
};
```

The important part is:

```ts
[Key in keyof User]
```

This means:

> For every key in `User`, create a corresponding property.

---

# 10. Mapped Type Mental Model

Think of:

```ts
type Optional<T> = {
  [K in keyof T]?: T[K];
};
```

as a type-level loop:

```text
for every K in keyof T
        ↓
create property K
        ↓
use T[K] as its type
        ↓
make it optional
```

If:

```ts
type User = {
  id: number;
  name: string;
};
```

then conceptually:

```text
K = "id"
→ id?: number

K = "name"
→ name?: string
```

Result:

```ts
{
  id?: number;
  name?: string;
}
```

---

# 11. Mapped Type — Transform Values

Mapped types don't have to preserve the original property types.

For example:

```ts
type UserFlags = {
  [K in keyof User]: boolean;
};
```

Result:

```ts
type UserFlags = {
  id: boolean;
  name: boolean;
  email: boolean;
};
```

The keys remain the same, but the values are transformed.

So:

```text
Mapped Type

keys
 ↓
iterate
 ↓
transform
 ↓
new type
```

---

# 12. Mapped Type — `readonly`

```ts
type ReadonlyUser = {
  readonly [K in keyof User]: User[K];
};
```

Result:

```ts
type ReadonlyUser = {
  readonly id: number;
  readonly name: string;
  readonly email: string;
};
```

---

# 13. Mapped Type — Removing Modifiers

You can also remove modifiers.

Remove optional:

```ts
type RequiredUser = {
  [K in keyof User]-?: User[K];
};
```

Remove `readonly`:

```ts
type MutableUser = {
  -readonly [K in keyof User]: User[K];
};
```

The syntax:

```text
?           → add optional
-?          → remove optional

readonly    → add readonly
-readonly   → remove readonly
```

---

# 14. `Partial`

TypeScript already provides a built-in utility type:

```ts
type UserUpdate = Partial<User>;
```

Conceptually:

```ts
type UserUpdate = {
  [K in keyof User]?: User[K];
};
```

Result:

```ts
{
  id?: number;
  name?: string;
  email?: string;
}
```

Common use case:

```ts
function updateUser(
  id: number,
  updates: Partial<User>
) {
  // update only provided properties
}
```

---

# 15. `Required`

```ts
type RequiredUser = Required<User>;
```

If:

```ts
type User = {
  id?: number;
  name?: string;
};
```

then:

```ts
Required<User>
```

becomes:

```ts
{
  id: number;
  name: string;
}
```

---

# 16. `Readonly`

```ts
type ReadonlyUser = Readonly<User>;
```

Result:

```ts
{
  readonly id: number;
  readonly name: string;
};
```

This prevents reassignment:

```ts
user.id = 10;
// ❌ Cannot assign to readonly property
```

---

# 17. `Pick`

Suppose:

```ts
type User = {
  id: number;
  name: string;
  email: string;
  password: string;
};
```

We only want public fields:

```ts
type PublicUser =
  Pick<User, "id" | "name" | "email">;
```

Result:

```ts
{
  id: number;
  name: string;
  email: string;
}
```

Mental model:

```text
Pick
 ↓
"Keep these properties."
```

---

# 18. `Omit`

Instead of specifying what to keep:

```ts
type PublicUser =
  Omit<User, "password">;
```

Result:

```ts
{
  id: number;
  name: string;
  email: string;
}
```

Mental model:

```text
Omit
 ↓
"Remove these properties."
```

---

# 19. `Record`

`Record` creates an object type from a set of keys.

```ts
type Role =
  | "admin"
  | "user"
  | "guest";
```

We can create:

```ts
type Permissions =
  Record<Role, string>;
```

Result:

```ts
type Permissions = {
  admin: string;
  user: string;
  guest: string;
};
```

For example:

```ts
const permissions: Permissions = {
  admin: "ALL",
  user: "READ",
  guest: "NONE"
};
```

Mental model:

```text
Record<K, T>

K → keys
T → value type
```

---

# 20. Conditional Types

A conditional type is essentially an `if / else` for the type system.

Syntax:

```ts
T extends X
  ? A
  : B
```

Meaning:

```text
Does T satisfy X?

YES → A
NO  → B
```

Example:

```ts
type IsString<T> =
  T extends string
    ? true
    : false;
```

Then:

```ts
type A = IsString<string>;
// true

type B = IsString<number>;
// false
```

---

# 21. Conditional Types Are Type-Level `if`

Think:

```ts
type Example<T> =
  T extends string
    ? "string"
    : "not string";
```

Conceptually:

```text
if (T is string) {
    return "string";
} else {
    return "not string";
}
```

Except this happens entirely at compile time.

---

# 22. Conditional Types + `never`

`never` becomes especially useful as a filter.

```ts
type OnlyStrings<T> =
  T extends string
    ? T
    : never;
```

Now:

```ts
type Result =
  OnlyStrings<string | number | boolean>;
```

TypeScript distributes the conditional type over the union:

```text
string  → string
number  → never
boolean → never
```

Therefore:

```text
string | never | never
```

becomes:

```ts
string
```

So `never` can effectively mean:

> **"Remove this type from the resulting union."**

---

# 23. `never` and Exhaustive Checking

`never` also represents an impossible state.

```ts
type Status =
  | "loading"
  | "success"
  | "error";
```

We can write:

```ts
function assertNever(value: never): never {
  throw new Error(
    `Unexpected value: ${value}`
  );
}
```

Then:

```ts
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

Once all three possibilities have been handled, TypeScript knows that nothing remains.

Therefore:

```ts
status: never
```

This provides exhaustive checking.

If someone later adds:

```ts
type Status =
  | "loading"
  | "success"
  | "error"
  | "cancelled";
```

TypeScript will report an error because `"cancelled"` isn't handled.

---

# 24. `infer` — Extract a Type

`infer` is used inside conditional types to **capture part of a type**.

Consider:

```ts
type ReturnTypeOf<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;
```

The important part is:

```ts
infer R
```

It means:

> "Whatever the return type is, capture it and call it `R`."

---

# 25. `infer` with Functions

Consider:

```ts
function getUser() {
  return {
    id: 1,
    name: "Danish"
  };
}
```

Now:

```ts
type User =
  ReturnType<typeof getUser>;
```

produces:

```ts
{
  id: number;
  name: string;
}
```

Conceptually, `ReturnType` works like:

```ts
type MyReturnType<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;
```

TypeScript sees:

```text
function
   ↓
what does it return?
   ↓
infer R
   ↓
R = return type
```

---

# 26. `infer` with Promises

We can extract the value inside a Promise:

```ts
type UnwrapPromise<T> =
  T extends Promise<infer R>
    ? R
    : T;
```

Then:

```ts
type A =
  UnwrapPromise<Promise<string>>;
```

Result:

```ts
string
```

And:

```ts
type B =
  UnwrapPromise<Promise<User>>;
```

Result:

```ts
User
```

The mental model:

```text
Promise<User>
      ↓
Promise<infer R>
      ↓
R = User
      ↓
User
```

---

# 27. `infer` with Arrays

We can extract an array's element type:

```ts
type ElementType<T> =
  T extends Array<infer E>
    ? E
    : never;
```

Then:

```ts
type A =
  ElementType<string[]>;
```

Result:

```ts
string
```

And:

```ts
type B =
  ElementType<User[]>;
```

Result:

```ts
User
```

---

# 28. `infer` with Function Parameters

We can extract parameters:

```ts
type MyParameters<T> =
  T extends (...args: infer P) => any
    ? P
    : never;
```

Given:

```ts
function createUser(
  name: string,
  age: number
) {
  return {
    name,
    age
  };
}
```

Then:

```ts
type Params =
  MyParameters<typeof createUser>;
```

Result:

```ts
[string, number]
```

Here:

```ts
infer P
```

captures the parameter tuple.

---

# 29. Template Literal Types

TypeScript can dynamically construct string literal types.

For example:

```ts
type Event =
  | "click"
  | "focus"
  | "blur";
```

We can generate handler names:

```ts
type Handler =
  `on${Capitalize<Event>}`;
```

Result:

```ts
"onClick"
| "onFocus"
| "onBlur"
```

This is useful when creating strongly typed event APIs.

---

# 30. Combining Mapped Types + Conditional Types + `infer`

This is where advanced TypeScript becomes extremely powerful.

Suppose:

```ts
type API = {
  getUser: () => Promise<User>;
  getPosts: () => Promise<Post[]>;
  deleteUser: () => Promise<void>;
};
```

We want:

```ts
{
  getUser: User;
  getPosts: Post[];
  deleteUser: void;
}
```

We can write:

```ts
type APIResponses<T> = {
  [K in keyof T]:
    T[K] extends (...args: any[]) => Promise<infer R>
      ? R
      : never;
};
```

Now:

```ts
type Responses =
  APIResponses<API>;
```

produces:

```ts
{
  getUser: User;
  getPosts: Post[];
  deleteUser: void;
}
```

Three mechanisms are working together.

### Mapped Type

```ts
[K in keyof T]
```

> Go through every property.

### Conditional Type

```ts
T[K] extends (...) => Promise<infer R>
```

> Check whether the property is a function returning a Promise.

### `infer`

```ts
infer R
```

> Extract the value inside the Promise.

---

# 31. Another Real-World Example

Suppose we define API endpoints:

```ts
const endpoints = {
  users: "/api/users",
  posts: "/api/posts",
  comments: "/api/comments"
} as const;
```

We can derive the endpoint names:

```ts
type EndpointName =
  keyof typeof endpoints;
```

Result:

```ts
"users"
| "posts"
| "comments"
```

We can derive the actual endpoint values:

```ts
type Endpoint =
  typeof endpoints[keyof typeof endpoints];
```

Result:

```ts
"/api/users"
| "/api/posts"
| "/api/comments"
```

Now if we add:

```ts
products: "/api/products"
```

both types automatically update.

The object remains the single source of truth.

---

# 32. Utility Types

Many TypeScript utility types are combinations of the concepts above.

Important ones to understand:

```text
Partial<T>
Required<T>
Readonly<T>
Pick<T, K>
Omit<T, K>
Record<K, T>
Exclude<T, U>
Extract<T, U>
NonNullable<T>
ReturnType<T>
Parameters<T>
Awaited<T>
```

Don't think of these as isolated features.

Think:

```text
Utility Types
      ↓
pre-built type transformations
      ↓
implemented using TypeScript's type-system features
```

Once you understand:

```text
keyof
mapped types
conditional types
never
infer
```

many utility types become much easier to understand.

---

# 33. `Exclude`

Given:

```ts
type Status =
  | "loading"
  | "success"
  | "error";
```

We can remove `"loading"`:

```ts
type FinalStatus =
  Exclude<Status, "loading">;
```

Result:

```ts
"success" | "error"
```

Conceptually:

```ts
type MyExclude<T, U> =
  T extends U
    ? never
    : T;
```

Again, `never` is acting as a filter.

---

# 34. `Extract`

The opposite idea:

```ts
type A =
  Extract<
    "a" | "b" | "c",
    "a" | "b"
  >;
```

Result:

```ts
"a" | "b"
```

Conceptually:

```ts
type MyExtract<T, U> =
  T extends U
    ? T
    : never;
```

So:

```text
Exclude
→ remove matching types

Extract
→ keep matching types
```

---

# 35. `NonNullable`

Given:

```ts
type Value =
  string | null | undefined;
```

We can remove `null` and `undefined`:

```ts
type Clean =
  NonNullable<Value>;
```

Result:

```ts
string
```

Conceptually, it uses the same filtering idea.

---

# 36. `ReturnType`

Given:

```ts
function getUser() {
  return {
    id: 1,
    name: "Danish"
  };
}
```

We can write:

```ts
type User =
  ReturnType<typeof getUser>;
```

Result:

```ts
{
  id: number;
  name: string;
}
```

This prevents duplicating the function's return type.

---

# 37. `Parameters`

Given:

```ts
function createUser(
  name: string,
  age: number
) {
  // ...
}
```

We can write:

```ts
type CreateUserParams =
  Parameters<typeof createUser>;
```

Result:

```ts
[string, number]
```

This is useful when another function needs to reuse the same parameter structure.

---

# 38. `Awaited`

Suppose:

```ts
type Result =
  Promise<User>;
```

We can extract the resolved type:

```ts
type UserResult =
  Awaited<Result>;
```

Result:

```ts
User
```

It is particularly useful for async function return types:

```ts
async function getUser() {
  return {
    id: 1,
    name: "Danish"
  };
}

type User =
  Awaited<ReturnType<typeof getUser>>;
```

Result:

```ts
{
  id: number;
  name: string;
}
```

The transformation is:

```text
async function
      ↓
ReturnType
      ↓
Promise<User>
      ↓
Awaited
      ↓
User
```

---

# 39. The Big Picture

All of these features are connected.

```text
                    Existing Value
                         │
                       typeof
                         │
                         ▼
                    Existing Type
                         │
             ┌───────────┼───────────┐
             │           │           │
           keyof      [Key]      conditional
             │           │           │
             ▼           ▼           ▼
            Keys       Values      if / else
             │                       │
             │                     infer
             │                       │
             │                       ▼
             │                  Extract Type
             │
             ▼
        Mapped Type
             │
             ▼
       Transform Properties
             │
             ▼
      New Derived Type
```

---

# 40. The Three Most Important Advanced Concepts

If you want to reduce everything to three fundamental ideas:

## Mapped Type

```ts
type Example<T> = {
  [K in keyof T]: ...
};
```

Think:

> **"Loop through the properties and transform them."**

---

## Conditional Type

```ts
type Example<T> =
  T extends X
    ? A
    : B;
```

Think:

> **"If the type satisfies this condition, choose A; otherwise choose B."**

---

## `infer`

```ts
T extends Something<infer R>
  ? R
  : never
```

Think:

> **"Extract this part of the type and call it R."**

---

# 41. The Most Useful Mental Models

### `typeof`

```text
value → type
```

### `keyof`

```text
type → keys
```

### Indexed Access

```text
type + key → property type
```

### `as const`

```text
normal value
    ↓
preserve exact literals
```

### Mapped Type

```text
type
 ↓
iterate properties
 ↓
transform properties
 ↓
new type
```

### Conditional Type

```text
type
 ↓
condition
 ↓
true / false
 ↓
selected type
```

### `infer`

```text
complex type
 ↓
extract part
 ↓
give extracted part a name
```

### `never`

```text
no possible value
```

---

# 42. Final Example — Putting Everything Together

Consider:

```ts
const api = {
  getUser: () => Promise.resolve({
    id: 1,
    name: "Danish"
  }),

  getPosts: () => Promise.resolve([
    {
      id: 1,
      title: "TypeScript"
    }
  ])
} as const;
```

First, derive the API type:

```ts
type API = typeof api;
```

Get the method names:

```ts
type Method =
  keyof API;
```

Result:

```ts
"getUser" | "getPosts"
```

Now transform every method:

```ts
type APIResponse<T> = {
  [K in keyof T]:
    T[K] extends (...args: any[]) => Promise<infer R>
      ? R
      : never;
};
```

Finally:

```ts
type Responses =
  APIResponse<API>;
```

TypeScript derives:

```ts
type Responses = {
  getUser: {
    id: number;
    name: string;
  };

  getPosts: {
    id: number;
    title: string;
  }[];
};
```

We didn't manually write the response types.

We derived them from the actual API definition.

---

# Final Takeaway

The most important principle behind advanced TypeScript is:

> **Use existing information to derive new types instead of duplicating type definitions.**

The core tools are:

```text
typeof
    ↓
derive a type from a value

keyof
    ↓
derive keys from a type

Indexed Access
    ↓
derive property/value types

as const
    ↓
preserve exact literal values

Mapped Types
    ↓
transform properties

Conditional Types
    ↓
make type-level decisions

never
    ↓
represent impossible/remove types

infer
    ↓
extract types from structures

Template Literal Types
    ↓
construct string types
```

Once these concepts are understood, TypeScript's built-in utilities are no longer mysterious:

```text
Partial
Required
Readonly
Pick
Omit
Record
Exclude
Extract
NonNullable
ReturnType
Parameters
Awaited
```

They become recognizable as **reusable combinations of a small number of type-system mechanisms**.

The ultimate goal is not to write the most complicated TypeScript possible.

The goal is:

> **Let the type system derive as much information as possible from the source of truth, while keeping the resulting code understandable.**
