## TypeScript Interview Cheat Sheet

|  # | Topic                       | What it means                                     | Key syntax / example                               | Interview point                                         |
| -: | --------------------------- | ------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------- |
|  1 | **Basic Types**             | Define the type of a value                        | `string`, `number`, `boolean`, `null`, `undefined` | TS checks types at compile time                         |
|  2 | **`any`**                   | Disables most type checking                       | `let x: any`                                       | Avoid when possible                                     |
|  3 | **`unknown`**               | Safe alternative to `any`                         | `let x: unknown`                                   | Must narrow before using                                |
|  4 | **`never`**                 | Value never successfully occurs/returns           | `function fail(): never { throw ... }`             | Useful for impossible states/errors                     |
|  5 | **Type Inference**          | TS automatically determines types                 | `const age = 30`                                   | Don't unnecessarily annotate obvious types              |
|  6 | **Union**                   | Value can be one of multiple types                | `string \| number`                                 | Requires narrowing for type-specific operations         |
|  7 | **Literal Union**           | Value must be one of specific values              | `"pending" \| "success"`                           | Great for states/statuses                               |
|  8 | **Intersection**            | Combines multiple types                           | `Person & Employee`                                | Must satisfy **both** types                             |
|  9 | **Interface**               | Defines object/class contract                     | `interface User { name: string }`                  | Can extend and declaration-merge                        |
| 10 | **`type`**                  | Creates type aliases                              | `type ID = string \| number`                       | More flexible for unions/intersections                  |
| 11 | **Interface vs Type**       | Two ways to model types                           | `interface User` / `type User =`                   | Interface → object contracts; type → composition/unions |
| 12 | **Optional Property**       | Property may be absent                            | `email?: string`                                   | Value can effectively be `string \| undefined`          |
| 13 | **`readonly`**              | Prevents reassignment through that type           | `readonly id: number`                              | Compile-time protection                                 |
| 14 | **Function Types**          | Describes function signature                      | `(a: number) => number`                            | Useful for callbacks                                    |
| 15 | **Optional Parameter**      | Parameter doesn't have to be supplied             | `age?: number`                                     | Usually `number \| undefined`                           |
| 16 | **Default Parameter**       | Provides fallback value                           | `age = 18`                                         | Parameter gets default when omitted/undefined           |
| 17 | **Rest Parameter**          | Accepts multiple arguments                        | `(...nums: number[])`                              | Collects arguments into array                           |
| 18 | **Function Overloading**    | Multiple call signatures for one function         | `fn(x: string): A; fn(x: number): B`               | Implementation handles actual logic                     |
| 19 | **Type Narrowing**          | Makes a broad type more specific                  | `typeof`, `in`, `instanceof`                       | Essential with unions                                   |
| 20 | **Type Guard**              | Custom function that narrows a type               | `x is Admin`                                       | Tells TS what type something is                         |
| 21 | **Discriminated Union**     | Union distinguished by common property            | `{status: "success"} \| {status: "error"}`         | Excellent for API/state modeling                        |
| 22 | **Generics**                | Preserve type information dynamically             | `function id<T>(x: T): T`                          | One of the most important interview topics              |
| 23 | **Generic Constraint**      | Restricts generic types                           | `<T extends { length: number }>`                   | Says what `T` must support                              |
| 24 | **`keyof`**                 | Gets keys of a type                               | `keyof User`                                       | Produces a union of property names                      |
| 25 | **Indexed Access**          | Gets a property's type                            | `User["name"]`                                     | Useful in generic utilities                             |
| 26 | **`typeof` (type-level)**   | Gets type from existing value                     | `type User = typeof user`                          | Derive types instead of duplicating them                |
| 27 | **Mapped Types**            | Creates type by iterating over keys               | `{ [K in keyof T]?: T[K] }`                        | Foundation of utilities like `Partial`                  |
| 28 | **Conditional Types**       | Type-level if/else                                | `T extends U ? X : Y`                              | Used in advanced generic types                          |
| 29 | **`Partial<T>`**            | Makes all properties optional                     | `Partial<User>`                                    | Common for update/patch APIs                            |
| 30 | **`Required<T>`**           | Makes all properties required                     | `Required<User>`                                   | Opposite of `Partial`                                   |
| 31 | **`Readonly<T>`**           | Makes properties readonly                         | `Readonly<User>`                                   | Prevents reassignment                                   |
| 32 | **`Pick<T, K>`**            | Selects specific properties                       | `Pick<User, "name">`                               | Create smaller type                                     |
| 33 | **`Omit<T, K>`**            | Removes specific properties                       | `Omit<User, "password">`                           | Useful for DTOs                                         |
| 34 | **`Record<K, V>`**          | Creates key/value object type                     | `Record<string, string[]>`                         | Useful for dictionaries/maps                            |
| 35 | **`Exclude<T, U>`**         | Removes types from union                          | `Exclude<Status, "loading">`                       | Union manipulation                                      |
| 36 | **`Extract<T, U>`**         | Keeps matching types                              | `Extract<A, B>`                                    | Opposite idea of `Exclude`                              |
| 37 | **`ReturnType<T>`**         | Gets function return type                         | `ReturnType<typeof getUser>`                       | Avoid duplicate type definitions                        |
| 38 | **`Parameters<T>`**         | Gets function parameters as tuple                 | `Parameters<typeof fn>`                            | Useful for reusable function types                      |
| 39 | **`Awaited<T>`**            | Gets resolved Promise value                       | `Awaited<Promise<User>>` → `User`                  | Useful with async types                                 |
| 40 | **Tuple**                   | Fixed number/order of types                       | `[number, string]`                                 | Position has meaning                                    |
| 41 | **Enum**                    | Named set of constants                            | `enum Role { ADMIN }`                              | Often replaced by literal unions                        |
| 42 | **Class**                   | OOP structure with methods/properties             | `class User {}`                                    | Supports access modifiers, inheritance                  |
| 43 | **`extends`**               | Inherits from class/interface                     | `class Admin extends User`                         | Inheritance                                             |
| 44 | **`implements`**            | Class follows interface contract                  | `class Logger implements ILogger`                  | Checks class structure                                  |
| 45 | **Abstract Class**          | Base class that can't be instantiated             | `abstract class Animal`                            | Child classes implement abstract members                |
| 46 | **`async`**                 | Function always returns Promise                   | `async function getUser()`                         | Return type is `Promise<T>`                             |
| 47 | **`Promise.all`**           | Waits for multiple promises                       | `Promise.all([a(), b()])`                          | Fails if one promise rejects                            |
| 48 | **`Promise.allSettled`**    | Gets result of every promise                      | `Promise.allSettled([...])`                        | Doesn't stop because one fails                          |
| 49 | **API Response Typing**     | Defines API response contract                     | `ApiResponse<User[]>`                              | Generics are very useful here                           |
| 50 | **Modules**                 | Split code into files                             | `export` / `import`                                | `import type` for type-only imports                     |
| 51 | **`strict`**                | Enables strict type checking                      | `"strict": true`                                   | Highly recommended                                      |
| 52 | **`strictNullChecks`**      | Distinguishes `null`/`undefined`                  | `string` ≠ `string \| null`                        | Prevents many runtime bugs                              |
| 53 | **Optional Chaining**       | Safely accesses nested values                     | `user?.profile?.name`                              | Avoids errors from null/undefined                       |
| 54 | **Nullish Coalescing**      | Fallback only for null/undefined                  | `name ?? "Unknown"`                                | Different from `\|\|`                                   |
| 55 | **Type Assertion**          | Tell TS what you believe type is                  | `value as HTMLDivElement`                          | Doesn't validate at runtime                             |
| 56 | **`satisfies`**             | Validates against type while preserving inference | `config satisfies Config`                          | Modern TS feature                                       |
| 57 | **Template Literal Types**  | Builds string types from patterns                 | `` `/api/${string}` ``                             | Useful for type-safe APIs                               |
| 58 | **Declaration Merging**     | Multiple interfaces combine                       | `interface User {}` twice                          | Interfaces can merge                                    |
| 59 | **Runtime vs Compile Time** | TS types disappear at runtime                     | `interface User {}`                                | TS does not validate API JSON automatically             |
| 60 | **Generic Property Access** | Type-safe dynamic property access                 | `K extends keyof T`                                | Very common interview question                          |

# Deep Down

## 1. Basic Types

```ts
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;

let value: null = null;
let data: undefined = undefined;
```

### Important: `any` vs `unknown` vs `never`

```ts
let a: any = "hello";
a.foo.bar(); // TypeScript won't complain
```

`any` = turns off type safety.

```ts
let value: unknown = "hello";

if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

`unknown` = safer alternative to `any`; you must narrow it before using it.

```ts
function throwError(message: string): never {
  throw new Error(message);
}
```

`never` = something that never successfully returns.

**Interview:**

> Why is `unknown` better than `any`?

Because `unknown` forces type checking/narrowing before operations.

# 2. Type Inference

You don't always need to explicitly specify types.

```ts
const name = "John"; // inferred as string
const age = 30;      // inferred as number
```

TypeScript automatically determines the type.

```ts
function add(a: number, b: number) {
  return a + b;
}
```

Return type is inferred as `number`.

**Good practice:** Let TypeScript infer obvious types; explicitly type function parameters, public APIs, and important contracts.

# 3. Union Types

A variable can have multiple possible types.

```ts
let id: string | number;

id = 101;
id = "101";
```

You need narrowing before using type-specific operations.

```ts
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

### Literal union

```ts
type Status = "pending" | "success" | "failed";

let status: Status = "pending";
```

Very common in production code.

# 4. Intersection Types

Combine multiple types.

```ts
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type EmployeePerson = Person & Employee;

const employee: EmployeePerson = {
  name: "John",
  employeeId: 101
};
```

`&` means **must satisfy both types**.

# 5. Interface

```ts
interface User {
  id: number;
  name: string;
  email?: string;
  readonly createdAt: Date;
}
```

* `?` → optional
* `readonly` → cannot be reassigned

```ts
const user: User = {
  id: 1,
  name: "John",
  createdAt: new Date()
};

// user.createdAt = new Date(); // Error
```

### Extending interfaces

```ts
interface Admin extends User {
  permissions: string[];
}
```

# 6. `type` vs `interface`

Both can describe object shapes.

```ts
type User = {
  id: number;
  name: string;
};
```

```ts
interface User {
  id: number;
  name: string;
}
```

### Interview answer

**Interface** is particularly useful for object contracts and can be extended/merged.

**Type** is more flexible for unions, intersections, primitives, tuples, mapped types, etc.

```ts
type Status = "active" | "inactive";

type ID = string | number;
```

A common practical approach is:

> Use `interface` for extensible object contracts and `type` when you need unions/intersections or more advanced type composition.

# 7. Functions

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

Optional parameter:

```ts
function greet(name: string, age?: number) {
  console.log(name, age);
}
```

Default parameter:

```ts
function greet(name: string, age = 18) {}
```

Rest parameter:

```ts
function sum(...numbers: number[]): number {
  return numbers.reduce((total, n) => total + n, 0);
}
```

# 8. Function Types

```ts
type Calculator = (a: number, b: number) => number;

const add: Calculator = (a, b) => a + b;
```


Useful for callbacks:

```ts
function process(
  value: number,
  callback: (value: number) => void
) {
  callback(value);
}
```

# 9. Function Overloading

When a function supports different input/output combinations.

```ts
function getValue(id: number): User;
function getValue(name: string): User;

function getValue(value: number | string): User {
  // implementation
}
```

The overload signatures provide the API contract; the implementation handles the actual logic.

# 10. Type Narrowing

TypeScript can determine a more specific type.

```ts
function print(value: string | number) {
  if (typeof value === "string") {
    value.toUpperCase();
  } else {
    value.toFixed(2);
  }
}
```

Common narrowing techniques:

```ts
typeof
instanceof
in
=== / !==
```

Example:

```ts
if ("email" in user) {
  console.log(user.email);
}
```

# 11. Custom Type Guards

Very important interview topic.

```ts
interface User {
  name: string;
}

interface Admin {
  name: string;
  permissions: string[];
}

function isAdmin(user: User | Admin): user is Admin {
  return "permissions" in user;
}
```

Now:

```ts
if (isAdmin(user)) {
  user.permissions;
}
```

The `user is Admin` part tells TypeScript:

> If this function returns true, treat `user` as `Admin`.

# 12. Discriminated Unions

Excellent for API/state modeling.

```ts
type Result =
  | { status: "success"; data: User[] }
  | { status: "error"; error: string }
  | { status: "loading" };
```

Then:

```ts
function handleResult(result: Result) {
  if (result.status === "success") {
    result.data;
  }

  if (result.status === "error") {
    result.error;
  }
}
```

TypeScript automatically narrows based on `status`.

# 13. Generics

One of the **most important topics for a 3-year developer**.

Instead of:

```ts
function identity(value: any): any {
  return value;
}
```

Use:

```ts
function identity<T>(value: T): T {
  return value;
}
```

Now:

```ts
const result = identity("hello");
// result: string

const numberResult = identity(100);
// numberResult: number
```

Generics preserve type information.

# 14. Generic Constraints

You can restrict what `T` can be.

```ts
function getLength<T extends { length: number }>(value: T) {
  return value.length;
}
```

Works with:

```ts
getLength("hello");
getLength([1, 2, 3]);
```

But not:

```ts
getLength(100); // Error
```

# 15. `keyof`

`keyof` gives you the keys of a type.

```ts
interface User {
  id: number;
  name: string;
  email: string;
}

type UserKey = keyof User;
```

Equivalent to:

```ts
"id" | "name" | "email"
```

Very useful with generics:

```ts
function getProperty<T, K extends keyof T>(
  obj: T,
  key: K
): T[K] {
  return obj[key];
}
```

Usage:

```ts
const user = {
  id: 1,
  name: "John"
};

const name = getProperty(user, "name");
// string

const id = getProperty(user, "id");
// number
```

This is a **very good interview example**.

# 16. `typeof` in TypeScript

There are two concepts.

Runtime:

```ts
typeof value === "string"
```

Type-level:

```ts
const user = {
  name: "John",
  age: 30
};

type User = typeof user;
```

Now `User` is derived from the object.

# 17. Indexed Access Types

```ts
type User = {
  id: number;
  name: string;
};

type UserName = User["name"];
```

Result:

```ts
string
```

You can also do:

```ts
type UserValue = User[keyof User];
```

Result:

```ts
string | number
```

# 18. Mapped Types

Used to create new types from existing types.

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

`Optional<User>` becomes approximately:

```ts
{
  name?: string;
  age?: number;
}
```

This is basically the concept behind `Partial<T>`.

# 19. Conditional Types

Syntax:

```ts
T extends U ? X : Y
```

Example:

```ts
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

Think of it as a **type-level if/else**.

# 20. Utility Types

Know these extremely well.

### `Partial`

```ts
type User = {
  name: string;
  age: number;
};

type UpdateUser = Partial<User>;
```

Everything becomes optional.

### `Required`

```ts
type RequiredUser = Required<User>;
```

Everything becomes required.

### `Readonly`

```ts
type ReadonlyUser = Readonly<User>;
```

Properties cannot be reassigned.

### `Pick`

```ts
type UserPreview = Pick<User, "name">;
```

### `Omit`

```ts
type UserWithoutAge = Omit<User, "age">;
```

### `Record`

```ts
type UserRoles = Record<string, string[]>;
```

Example:

```ts
const roles: UserRoles = {
  john: ["admin"],
  jane: ["editor"]
};
```

### `Exclude`

```ts
type Status = "success" | "error" | "loading";

type FinalStatus = Exclude<Status, "loading">;
```

Result:

```ts
"success" | "error"
```

### `Extract`

Opposite idea:

```ts
type A = Extract<"a" | "b", "a" | "c">;
// "a"
```

### `ReturnType`

```ts
function getUser() {
  return {
    id: 1,
    name: "John"
  };
}

type User = ReturnType<typeof getUser>;
```

### `Parameters`

```ts
function createUser(name: string, age: number) {}

type Params = Parameters<typeof createUser>;
// [string, number]
```

### `Awaited`

```ts
type User = Awaited<Promise<User>>;
```

Gets the resolved value of a Promise.

# 21. Tuples

A tuple has a fixed structure.

```ts
let user: [number, string];

user = [1, "John"];
```

Unlike:

```ts
string[]
```

which allows any number of strings.

Tuples are useful when position has meaning.

# 22. Enums

```ts
enum Role {
  ADMIN = "ADMIN",
  USER = "USER"
}
```

Usage:

```ts
const role: Role = Role.ADMIN;
```

But modern TypeScript code often prefers:

```ts
type Role = "ADMIN" | "USER";
```

because unions are simpler and don't generate enum runtime objects.

# 23. Classes

```ts
class UserService {
  constructor(private apiUrl: string) {}

  public getUser(id: number): void {
    console.log(`${this.apiUrl}/${id}`);
  }
}
```

Access modifiers:

```ts
public
private
protected
```

### `readonly`

```ts
class User {
  readonly id: number;

  constructor(id: number) {
    this.id = id;
  }
}
```

# 24. `extends` vs `implements`

### `extends`

Inheritance:

```ts
class Admin extends User {
}
```

### `implements`

Enforces a contract:

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message);
  }
}
```

A class can implement multiple interfaces.

# 25. Abstract Classes

```ts
abstract class Animal {
  abstract makeSound(): void;

  move() {
    console.log("Moving");
  }
}
```

You cannot instantiate it directly:

```ts
// new Animal(); // Error
```

Child classes must implement abstract members.

# 26. Async/Await

```ts
async function getUser(): Promise<User> {
  const response = await fetch("/api/user");
  return response.json();
}
```

Important:

> An `async` function always returns a Promise.

Error handling:

```ts
try {
  const user = await getUser();
} catch (error) {
  console.error(error);
}
```

# 27. `Promise.all`

When requests can execute independently:

```ts
const [users, products] = await Promise.all([
  getUsers(),
  getProducts()
]);
```

They execute concurrently.

### `Promise.allSettled`

Useful when you want results from **all operations**, even if some fail.

```ts
const results = await Promise.allSettled([
  getUsers(),
  getProducts()
]);
```

# 28. API Response Typing

Instead of:

```ts
const response: any = await fetchData();
```

Define a contract:

```ts
interface ApiResponse<T> {
  data: T;
  success: boolean;
  message?: string;
}
```

Then:

```ts
async function getUsers(): Promise<ApiResponse<User[]>> {
  // ...
}
```

This is how generics become useful in real applications.

# 29. Modules

Named export:

```ts
export function getUser() {}
```

Import:

```ts
import { getUser } from "./user";
```

Default:

```ts
export default UserService;
```

Import:

```ts
import UserService from "./UserService";
```

For type-only imports:

```ts
import type { User } from "./types";
```

# 30. `tsconfig.json`

For a serious project, understand:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}
```

### `strict`

Enables a collection of strict type-checking options.

### `strictNullChecks`

Makes this distinction important:

```ts
string
```

is different from:

```ts
string | null
```

This catches many production bugs.

# 31. `null` Handling

Bad:

```ts
function getName(user: User) {
  return user.name.toUpperCase();
}
```

If `name` can be null, this can crash.

Better:

```ts
function getName(user: User) {
  return user.name?.toUpperCase();
}
```

Optional chaining:

```ts
user?.profile?.address?.city
```

Nullish coalescing:

```ts
const name = user.name ?? "Unknown";
```

Remember:

```ts
|| 
```

checks falsiness, while:

```ts
??
```

only falls back for `null` or `undefined`.

# 32. Type Assertion

```ts
const value = document.getElementById("app") as HTMLDivElement;
```

You're telling TypeScript:

> "I know more about this value than you do."

But assertions **do not perform runtime validation**.

Don't use assertions just to silence errors.

# 33. `satisfies`

Modern TypeScript feature worth knowing.

```ts
type Config = {
  port: number;
  environment: "dev" | "prod";
};

const config = {
  port: 3000,
  environment: "dev"
} satisfies Config;
```

It validates that the object satisfies the required type while preserving more specific inferred information.

# 34. Template Literal Types

```ts
type EventName = `on${Capitalize<string>}`;
```

Example:

```ts
type HTTPMethod = "GET" | "POST";

type Endpoint = `/api/${string}`;
```

Useful for advanced type-safe APIs.

# 35. Declaration Merging

Interfaces can merge:

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}
```

TypeScript treats it roughly as:

```ts
interface User {
  name: string;
  age: number;
}
```

This is one difference from type aliases.

# 36. TypeScript vs JavaScript

**JavaScript**:

* Dynamically typed
* Types checked at runtime
* Runs directly in browser/Node

**TypeScript**:

* Statically typed
* Types checked during development/compilation
* Compiles/transpiles to JavaScript
* Types are generally removed from runtime output

Important interview statement:

> TypeScript improves developer-time safety, but it doesn't magically validate external runtime data.

For example, an API can still return malformed JSON.

# 37. Very Important: Runtime vs Compile Time

TypeScript:

```ts
interface User {
  id: number;
  name: string;
}
```

This doesn't validate incoming JSON at runtime.

If you receive:

```ts
const data = await response.json();
```

you may need runtime validation using a validation library or explicit checks.

This is a common **experienced-developer discussion point**.

# 38. `any` — Interview Trap

Avoid:

```ts
function process(data: any) {
  return data.foo.bar;
}
```

Prefer:

```ts
function process(data: unknown) {
  if (
    typeof data === "object" &&
    data !== null &&
    "foo" in data
  ) {
    // narrow further
  }
}
```

Or define the actual domain type.

# 39. Generic API Wrapper — Real Interview Example

A reusable API function:

```ts
async function request<T>(url: string): Promise<T> {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  return response.json() as Promise<T>;
}
```

Usage:

```ts
const users = await request<User[]>("/api/users");
```

The important idea is:

```ts
request<T>
```

allows the caller to specify the expected response type.

But remember: `T` here is a **compile-time assertion**, not runtime validation.

# 40. Interview-Level Coding Question

You should be able to explain and write this without hesitation:

```ts
function getProperty<T, K extends keyof T>(
  object: T,
  key: K
): T[K] {
  return object[key];
}
```

Given:

```ts
const user = {
  id: 1,
  name: "John",
  active: true
};
```

Then:

```ts
const id = getProperty(user, "id");
// number

const name = getProperty(user, "name");
// string

const active = getProperty(user, "active");
// boolean
```

The interviewer may ask:

**Why not simply use `string` for `key`?**

Because then this would be allowed:

```ts
getProperty(user, "somethingThatDoesNotExist");
```

`K extends keyof T` restricts `key` to actual keys of `T`.

# 🔥 Your 3-Year Interview Priority

If you have limited time, revise these **first**:

| Priority        | Topic                         |
| --------------- | ----------------------------- |
| 🔴 Must know    | `any` vs `unknown` vs `never` |
| 🔴 Must know    | Union/intersection            |
| 🔴 Must know    | Type narrowing                |
| 🔴 Must know    | Interface vs type             |
| 🔴 Must know    | Generics                      |
| 🔴 Must know    | `keyof`                       |
| 🔴 Must know    | Utility types                 |
| 🔴 Must know    | `T extends keyof T` patterns  |
| 🔴 Must know    | Async/Promise typing          |
| 🔴 Must know    | API response typing           |
| 🟠 Important    | Conditional types             |
| 🟠 Important    | Mapped types                  |
| 🟠 Important    | Type guards                   |
| 🟠 Important    | Discriminated unions          |
| 🟠 Important    | `tsconfig` / strict mode      |
| 🟡 Good to know | Template literal types        |
| 🟡 Good to know | Declaration merging           |
| 🟡 Good to know | `satisfies`                   |
