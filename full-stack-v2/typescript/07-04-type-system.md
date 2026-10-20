# Summary: TypeScript — Deep Revision Table

| Concept                      | What it means                                    | Example                    | Remember                         |
| ---------------------------- | ------------------------------------------------ | -------------------------- | -------------------------------- |
| **Type Annotation**          | Explicitly specify a type                        | `let age: number = 20`     | You tell TS                      |
| **Type Inference**           | TS figures out the type                          | `let age = 20`             | TS tells itself                  |
| **`any`**                    | Disables most type checking                      | `let x: any`               | ⚠️ Escape hatch                  |
| **`unknown`**                | Value of unknown type; must narrow before use    | `let x: unknown`           | Safe alternative to `any`        |
| **`never`**                  | A value that can never occur/return              | `function fail(): never`   | No successful completion         |
| **`void`**                   | Function doesn't return a useful value           | `function log(): void`     | Function can finish              |
| **Union `\|`**               | One of multiple possibilities                    | `string \| number`         | **OR**                           |
| **Intersection `&`**         | Must satisfy multiple types                      | `A & B`                    | **AND**                          |
| **Literal Type**             | Exact allowed value                              | `"success" \| "error"`     | Exact values                     |
| **Narrowing**                | Make a broad type more specific                  | `typeof x === "string"`    | Prove the type                   |
| **`typeof`**                 | Narrow primitive types                           | `typeof x === "number"`    | Runtime check                    |
| **`instanceof`**             | Narrow using class/constructor                   | `x instanceof User`        | Instance check                   |
| **`in`**                     | Narrow based on property existence               | `"email" in user`          | Property check                   |
| **Discriminated Union**      | Union distinguished by a common literal property | `status: "success"`        | Great for state/results          |
| **Exhaustiveness**           | Ensure every union case is handled               | `default: assertNever(x)`  | Catch missing cases              |
| **Type Alias**               | Give a name to a type                            | `type User = {...}`        | Flexible type definition         |
| **Interface**                | Describe an object contract                      | `interface User {...}`     | Great for object/class contracts |
| **Structural Typing**        | Compatibility is based on shape                  | `{name: string}`           | Shape matters, not name          |
| **Generics**                 | Preserve relationships between types             | `function id<T>(x: T): T`  | Type parameter                   |
| **Generic Constraint**       | Restrict generic types                           | `<T extends User>`         | T must satisfy constraint        |
| **`keyof`**                  | Gets keys of a type                              | `keyof User`               | `"name" \| "age"`                |
| **`typeof` (type position)** | Gets type of a value                             | `type User = typeof user`  | Value → type                     |
| **`keyof typeof`**           | Gets keys from a value                           | `keyof typeof roles`       | Very common pattern              |
| **`as const`**               | Preserves literal + readonly types               | `{role: "ADMIN"} as const` | Don't widen literals             |
| **`Partial<T>`**             | Makes all properties optional                    | `Partial<User>`            | Everything `?`                   |
| **`Required<T>`**            | Makes all properties required                    | `Required<User>`           | Remove `?`                       |
| **`Readonly<T>`**            | Makes properties readonly                        | `Readonly<User>`           | Can't reassign                   |
| **`Pick<T, K>`**             | Select specific properties                       | `Pick<User, "name">`       | Keep these                       |
| **`Omit<T, K>`**             | Remove specific properties                       | `Omit<User, "password">`   | Remove these                     |
| **`Record<K, T>`**           | Creates object type from keys                    | `Record<string, number>`   | Key → value mapping              |
| **`Exclude<T, U>`**          | Removes members from a union                     | `Exclude<A, B>`            | Remove matching types            |
| **`Extract<T, U>`**          | Keeps matching members                           | `Extract<A, B>`            | Keep matching types              |
| **`ReturnType<T>`**          | Gets function return type                        | `ReturnType<typeof fn>`    | Function → return type           |
| **`Parameters<T>`**          | Gets function parameter tuple                    | `Parameters<typeof fn>`    | Function → parameters            |
| **Mapped Type**              | Transforms properties of a type                  | `[K in keyof T]`           | Loop over keys                   |
| **Conditional Type**         | Type-level `if/else`                             | `T extends U ? X : Y`      | Type-level condition             |
| **`infer`**                  | Extracts a type inside a conditional type        | `infer R`                  | "Figure out R"                   |

### 🔥 The Most Important Relationships

| Concept          | Mental Model                       |
| ---------------- | ---------------------------------- |
| `any`            | **Trust me, don't check**          |
| `unknown`        | **You don't know yet; prove it**   |
| `never`          | **This cannot happen**             |
| `void`           | **Nothing useful returned**        |
| `A \| B`         | **A OR B**                         |
| `A & B`          | **A AND B**                        |
| Generic `<T>`    | **Remember this type**             |
| `T extends U`    | **T must satisfy U**               |
| `keyof T`        | **Give me T's keys**               |
| `T[K]`           | **Give me T's property type at K** |
| `typeof value`   | **Give me value's type**           |
| `Partial<T>`     | **Make everything optional**       |
| `Pick<T, K>`     | **Keep these keys**                |
| `Omit<T, K>`     | **Remove these keys**              |
| Mapped Type      | **Loop over type properties**      |
| Conditional Type | **Type-level if/else**             |
| `infer`          | **Extract/figure out a type**      |

### 🧩 One Example Connecting Everything

```ts
const users = {
  admin: {
    name: "John",
    age: 25
  },
  guest: {
    name: "Alice",
    age: 20
  }
} as const;
```

#### Derive the type:

```ts
type Users = typeof users;
```

#### Get the keys:

```ts
type Role = keyof Users;
// "admin" | "guest"
```

#### Generic property access:

```ts
function getUser<T, K extends keyof T>(
  obj: T,
  key: K
): T[K] {
  return obj[key];
}
```

#### Use it:

```ts
const admin = getUser(users, "admin");
```

Now TypeScript knows `admin` precisely.

The concepts connect like this:

```text
value
  ↓
typeof
  ↓
type
  ↓
keyof
  ↓
keys
  ↓
generics
  ↓
constraints
  ↓
T[K]
  ↓
precise type
```

#### 🚀 TypeScript progression to remember

```text
Basic Types
    ↓
Union / Intersection
    ↓
Narrowing
    ↓
Generics
    ↓
keyof / typeof
    ↓
Utility Types
    ↓
Mapped Types
    ↓
Conditional Types
    ↓
infer
    ↓
Type-Level Programming
```

This is the **cheat-sheet version** to revisit before practicing questions.



# 🔥 Deep Dive: TypeScript Type System

The biggest mindset shift:

> **JavaScript types exist at runtime. TypeScript types mostly exist at compile time.**

TypeScript helps you catch mistakes **before JavaScript runs**.

## 1. Type Annotation vs Type Inference

You can explicitly tell TypeScript a type:

```ts
let age: number = 25;
```

Or let TypeScript infer it:

```ts
let age = 25;
```

TypeScript understands:

```ts
age: number
```

So:

```text
Annotation
→ You tell TypeScript

Inference
→ TypeScript figures it out
```

Usually, prefer inference when the type is obvious.

## 2. Primitive Types

```ts
let name: string;
let age: number;
let active: boolean;
let value: null;
let missing: undefined;
let big: bigint;
let id: symbol;
```

The common ones:

```text
string
number
boolean
null
undefined
bigint
symbol
```

## 3. `any` ⚠️

`any` essentially tells TypeScript:

> "Stop checking this value."

```ts
let value: any = "hello";

value.toUpperCase();
value.foo.bar.baz();
value();
```

TypeScript may allow all of these.

You've effectively opted out of type safety.

#### Mental model:

```text
any
 ↓
TypeScript stops protecting you
```

Use it sparingly.

## 4. `unknown` ⭐⭐⭐

`unknown` is the **safe alternative to `any`**.

```ts
let value: unknown = "hello";
```

You cannot directly do:

```ts
value.toUpperCase(); // ❌
```

You must first establish what it is:

```ts
if (typeof value === "string") {
  value.toUpperCase(); // ✅
}
```

Mental model:

```text
any
→ "Trust me."

unknown
→ "You don't know yet. Prove it."
```

This distinction is extremely important.

## 5. `never` ⭐⭐⭐

`never` represents a value that **can never exist**.

Example:

```ts
function fail(message: string): never {
  throw new Error(message);
}
```

The function never successfully returns.

Another example:

```ts
function infiniteLoop(): never {
  while (true) {}
}
```

Think:

```text
void
→ returns nothing useful

never
→ cannot return at all
```

## 6. `void`

Usually used when a function doesn't return a meaningful value:

```ts
function logMessage(message: string): void {
  console.log(message);
}
```

It doesn't mean:

> "This function can never return."

It means:

> "Don't expect a useful return value."

## 7. `void` vs `never`

| Type    | Meaning                                  |
| ------- | ---------------------------------------- |
| `void`  | Function completes without useful result |
| `never` | Function never successfully completes    |

Example:

```ts
function log(): void {
  console.log("hello");
}

function crash(): never {
  throw new Error();
}
```

## 8. Union Types ⭐

A union means:

> A value can be one of several types.

```ts
let id: string | number;
```

So:

```ts
id = "abc"; // ✅
id = 123;   // ✅
id = true;  // ❌
```

Think:

```text
string OR number
```

## 9. Union Narrowing

Suppose:

```ts
function printId(id: string | number) {
  console.log(id);
}
```

Can you do:

```ts
id.toUpperCase();
```

No, because `id` might be a number.

So narrow it:

```ts
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed());
  }
}
```

Now TypeScript understands the branch-specific type.

## 10. Type Narrowing ⭐⭐⭐

TypeScript can use runtime checks to make the type more specific.

Common narrowing tools:

```ts
typeof
instanceof
in
equality checks
custom type guards
```

Example:

```ts
function process(value: string | number) {
  if (typeof value === "string") {
    // value: string
  } else {
    // value: number
  }
}
```

Think:

```text
Broad type
   ↓
runtime check
   ↓
narrower type
```

## 11. `typeof` Narrowing

```ts
function print(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase();
  }

  return value.toFixed(2);
}
```

`typeof` is particularly useful for primitives.

## 12. `instanceof`

Useful with classes/constructors:

```ts
class Dog {
  bark() {}
}

class Cat {
  meow() {}
}

function speak(animal: Dog | Cat) {
  if (animal instanceof Dog) {
    animal.bark();
  } else {
    animal.meow();
  }
}
```

## 13. `in` Operator

Useful for distinguishing object shapes.

```ts
type Dog = {
  bark(): void;
};

type Cat = {
  meow(): void;
};

function speak(animal: Dog | Cat) {
  if ("bark" in animal) {
    animal.bark();
  } else {
    animal.meow();
  }
}
```

## 14. Literal Types ⭐

A type can be an exact value:

```ts
let direction: "left" | "right";
```

Only:

```text
"left"
"right"
```

are allowed.

Not:

```ts
"up"
```

Literal types are extremely useful for APIs and state machines.

## 15. Discriminated Unions ⭐⭐⭐

One of TypeScript's most powerful practical patterns.

```ts
type Result =
  | {
      status: "success";
      data: string;
    }
  | {
      status: "error";
      error: string;
    };
```

Now:

```ts
function handle(result: Result) {
  if (result.status === "success") {
    console.log(result.data);
  } else {
    console.log(result.error);
  }
}
```

The `status` property acts as the **discriminator**.

Mental model:

```text
Result
 ├── status = "success" → data
 └── status = "error"   → error
```

## 16. Exhaustiveness Checking ⭐⭐⭐

This is where `never` becomes extremely useful.

```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; size: number };

function area(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;

    case "square":
      return shape.size ** 2;

    default:
      return assertNever(shape);
  }
}

function assertNever(value: never): never {
  throw new Error("Unexpected value");
}
```

If you later add:

```ts
{ kind: "triangle"; ... }
```

TypeScript can detect that your switch isn't exhaustive.

## 17. Intersection Types

An intersection combines types:

```ts
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type EmployeePerson = Person & Employee;
```

Now:

```ts
const employee: EmployeePerson = {
  name: "John",
  employeeId: 123
};
```

Think:

```text
A & B
 ↓
must satisfy A AND B
```

Whereas:

```text
A | B
 ↓
can satisfy A OR B
```

## 18. Union vs Intersection

| | Union `|` | Intersection `&` |
|---|---|---|
| Meaning | OR | AND |
| Example | `string \| number` | `A & B` |
| Value must satisfy | One possibility | All combined requirements |
| Common use | Alternatives | Combining capabilities |

## 19. Type Aliases

```ts
type User = {
  name: string;
  age: number;
};
```

You can also create aliases for unions:

```ts
type ID = string | number;
```

Or functions:

```ts
type Handler = (value: string) => void;
```

## 20. Interfaces

```ts
interface User {
  name: string;
  age: number;
}
```

A class can implement it:

```ts
class Admin implements User {
  constructor(
    public name: string,
    public age: number
  ) {}
}
```

A useful mental distinction:

```text
interface
→ describes an object contract

type
→ more general type composition mechanism
```

Modern TypeScript lets both handle many overlapping use cases.

## 21. Structural Typing ⭐⭐⭐

This is one of the most important TypeScript concepts.

TypeScript mostly cares about **shape**, not the declared name of the type.

```ts
type User = {
  name: string;
};

const person = {
  name: "John",
  age: 25
};

const user: User = person;
```

This works.

Why?

Because `person` has at least what `User` requires.

Think:

```text
User requires:
    name: string

person provides:
    name: string
    age: number

Therefore:
person satisfies User
```

This is called **structural typing**.

## 22. Excess Property Checking

There is a subtle wrinkle.

```ts
type User = {
  name: string;
};

const user: User = {
  name: "John",
  age: 25
};
```

TypeScript may complain here because you're directly assigning an object literal with an extra property.

But:

```ts
const person = {
  name: "John",
  age: 25
};

const user: User = person;
```

works.

This is a classic TypeScript behavior worth understanding rather than memorizing blindly.

## 23. Generics ⭐⭐⭐⭐⭐

Now we reach one of the most important TypeScript concepts.

Instead of:

```ts
function identity(value: any): any {
  return value;
}
```

we can preserve the relationship between input and output:

```ts
function identity<T>(value: T): T {
  return value;
}
```

Then:

```ts
const a = identity("hello");
// string

const b = identity(123);
// number
```

`T` represents a type parameter.

Think:

```text
Input type
    ↓
    T
    ↓
Output type
```

The relationship is preserved.

## 24. Why Generics Are Better Than `any`

Compare:

```ts
function identity(value: any): any {
  return value;
}
```

with:

```ts
function identity<T>(value: T): T {
  return value;
}
```

With `any`:

```text
type information → lost
```

With generic:

```text
input type
    ↓
   T
    ↓
output type
```

TypeScript remembers the relationship.

## 25. Generic Constraints

Sometimes you don't want **any possible type**.

You want something with a particular property.

```ts
function getLength<T extends { length: number }>(
  value: T
) {
  return value.length;
}
```

Now:

```ts
getLength("hello");     // ✅
getLength([1, 2, 3]);   // ✅
getLength({ length: 5 }); // ✅
```

But:

```ts
getLength(123); // ❌
```

Because number doesn't satisfy:

```ts
{ length: number }
```

Think:

```text
T extends Constraint
        ↓
T must satisfy Constraint
```

## 26. `keyof` ⭐⭐⭐

`keyof` gives you the keys of a type.

```ts
type User = {
  name: string;
  age: number;
};
```

Then:

```ts
type UserKey = keyof User;
```

becomes approximately:

```ts
"name" | "age"
```

This is extremely useful with generics.

## 27. Generic `getProperty`

```ts
function getProperty<T, K extends keyof T>(
  object: T,
  key: K
) {
  return object[key];
}
```

Now:

```ts
const user = {
  name: "John",
  age: 25
};

getProperty(user, "name"); // string
getProperty(user, "age");  // number
```

But:

```ts
getProperty(user, "email"); // ❌
```

This is a beautiful example of TypeScript's type system preserving relationships.

## 28. `typeof` in Type Positions

JavaScript:

```ts
typeof value
```

checks a runtime type.

TypeScript can also use `typeof` in a type position:

```ts
const user = {
  name: "John",
  age: 25
};

type User = typeof user;
```

Now `User` is derived from the value's type.

This is different from runtime `typeof`.

## 29. `keyof typeof`

These become powerful together.

```ts
const roles = {
  admin: "ADMIN",
  user: "USER",
  guest: "GUEST"
} as const;

type Role = keyof typeof roles;
```

Result:

```ts
"admin" | "user" | "guest"
```

And:

```ts
type RoleValue = typeof roles[Role];
```

becomes:

```ts
"ADMIN" | "USER" | "GUEST"
```

This pattern appears constantly in real TypeScript code.

## 30. `as const`

Without:

```ts
as const
```

TypeScript may widen values:

```ts
const role = "ADMIN";
```

Generally inferred as:

```text
string literal "ADMIN"
```

for a `const` variable, but object properties can be widened:

```ts
const config = {
  role: "ADMIN"
};
```

The property may be inferred as:

```text
role: string
```

Using:

```ts
const config = {
  role: "ADMIN"
} as const;
```

preserves literal/read-only information:

```text
readonly role: "ADMIN"
```

## 31. Utility Types

TypeScript provides powerful built-in transformations.

```ts
Partial<User>
Required<User>
Readonly<User>
Pick<User, ...>
Omit<User, ...>
Record<...>
Exclude<...>
Extract<...>
ReturnType<...>
Parameters<...>
```

For example:

```ts
type User = {
  name: string;
  age: number;
};
```

#### `Partial`

```ts
type UpdateUser = Partial<User>;
```

becomes:

```ts
{
  name?: string;
  age?: number;
}
```

## 32. `Pick` vs `Omit`

```ts
type User = {
  name: string;
  age: number;
  email: string;
};
```

Pick:

```ts
type PublicUser = Pick<User, "name" | "email">;
```

Result:

```ts
{
  name: string;
  email: string;
}
```

Omit:

```ts
type UserWithoutEmail = Omit<User, "email">;
```

Result:

```ts
{
  name: string;
  age: number;
}
```

Think:

```text
Pick → keep these
Omit → remove these
```

## 33. Mapped Types ⭐⭐⭐⭐

This is where TypeScript becomes a type-programming language.

```ts
type Optional<T> = {
  [K in keyof T]?: T[K];
};
```

Given:

```ts
type User = {
  name: string;
  age: number;
};
```

we get:

```ts
type OptionalUser = Optional<User>;
```

which becomes:

```ts
{
  name?: string;
  age?: number;
}
```

You're essentially **looping over keys at the type level**.

Mental model:

```text
keyof T
   ↓
iterate over keys
   ↓
transform each property
```

## 34. Conditional Types ⭐⭐⭐⭐

Conditional types look like:

```ts
T extends U ? X : Y
```

Think:

```text
if T extends U
    → X
else
    → Y
```

Example:

```ts
type IsString<T> =
  T extends string ? true : false;
```

Then:

```ts
type A = IsString<string>; // true
type B = IsString<number>; // false
```

This is essentially an **if statement at the type level**.

## 35. `infer` ⭐⭐⭐⭐⭐

`infer` lets TypeScript extract a type from another type.

Example:

```ts
type Return<T> =
  T extends (...args: any[]) => infer R
    ? R
    : never;
```

Given:

```ts
type Fn = () => string;

type Result = Return<Fn>;
```

`Result` becomes:

```text
string
```

Think:

```text
infer R
 ↓
"TypeScript, figure out what R is here."
```

## 36. The Type-System Progression

This is the progression I'd remember:

```text
Basic Types
    ↓
Union / Intersection
    ↓
Narrowing
    ↓
Generics
    ↓
keyof / typeof
    ↓
Utility Types
    ↓
Mapped Types
    ↓
Conditional Types
    ↓
infer
    ↓
Type-Level Programming
```

## 🧠 Final TypeScript Mental Model

```text
                    TYPE SYSTEM
                        │
          ┌─────────────┴─────────────┐
          ↓                           ↓
       VALUES                       TYPES
          │                           │
          ↓                           ↓
      inference                  annotations
                                      │
                    ┌─────────────────┼─────────────────┐
                    ↓                 ↓                 ↓
                  Union         Intersection        Generics
                    │                 │                 │
                    ↓                 ↓                 ↓
                Narrowing          Combine          Constraints
                    │                                   │
                    └────────────────┬──────────────────┘
                                     ↓
                              keyof / typeof
                                     ↓
                               Utility Types
                                     ↓
                               Mapped Types
                                     ↓
                              Conditional Types
                                     ↓
                                   infer
                                     ↓
                            Type-Level Programming
```

## 🔴 Deep-Dive Questions

#### Level 1

1. What is the difference between `any` and `unknown`?
2. What is `never`?
3. What is a union?
4. What is an intersection?
5. What does `keyof` do?
6. What does `extends` mean in a generic constraint?
7. What does `Partial<T>` do?

#### Level 2

8. Why is `unknown` safer than `any`?
9. Why does TypeScript narrow a union after `typeof`?
10. Why would you use a generic instead of `any`?
11. What does `K extends keyof T` guarantee?
12. What's the difference between `Pick<T, K>` and `Omit<T, K>`?
13. Why are discriminated unions useful?

#### Level 3 🔴

14. Explain exactly what this does:

```ts
function getProperty<T, K extends keyof T>(
  obj: T,
  key: K
): T[K] {
  return obj[key];
}
```

15. Implement your own:

```ts
type MyPartial<T> = ...
```

16. Implement:

```ts
type MyPick<T, K extends keyof T> = ...
```

17. Explain this:

```ts
type ElementType<T> =
  T extends (infer U)[]
    ? U
    : never;
```

18. What does this become?

```ts
type Result =
  | string
  | number
  | boolean;

type OnlyStrings<T> =
  T extends string ? T : never;

type A = OnlyStrings<Result>;
```

The answer to that last one forces you to understand **conditional types + distributive conditional types**, which is exactly the level we want to reach next.
