# JavaScript — SDE-2 Revision Map

- **Perspective:** ~3 years experience
- **Purpose:** Interview + production-level revision
- **Approach:** Conceptual grouping, not learning order
- **Depth:** Fundamentals → language behavior → runtime → production implications

## 1. JavaScript Language Fundamentals

The foundation of the language itself.

* JavaScript as a language
* ECMAScript
* JavaScript runtime vs JavaScript language
* Browser vs Node.js environment
* Interpreted vs compiled execution
* Dynamic typing
* Weak vs strong typing
* Expressions vs statements
* Values and references
* Primitive vs non-primitive values
* Mutable vs immutable values
* Strict mode
* JavaScript execution at a high level
* ECMAScript versions and language evolution

## 2. Variables, Declarations & Keywords

Everything related to declaring and binding variables.

* `var`
* `let`
* `const`
* Variable declarations
* Initialization
* Assignment
* Reassignment
* Redeclaration
* Declaration vs definition
* Scope of declarations
* Hoisting
* Temporal Dead Zone
* Global declarations
* `var` behavior
* `let` / `const` behavior
* Variable shadowing
* Reserved words / keywords
* Identifier rules

## 3. Data Types & Type Coercion

Everything related to JavaScript's type system at runtime.

* Primitive types

  * `string`
  * `number`
  * `bigint`
  * `boolean`
  * `undefined`
  * `null`
  * `symbol`
* Objects
* `typeof`
* `instanceof`
* `Object.prototype.toString`
* Truthy / falsy values
* Type conversion
* Explicit coercion
* Implicit coercion
* String conversion
* Number conversion
* Boolean conversion
* `null` vs `undefined`
* `NaN`
* `Infinity`
* `-0`
* Numeric precision
* IEEE-754 basics

## 4. Operators & Expressions

Everything concerned with evaluating values and expressions.

* Arithmetic operators
* Assignment operators
* Comparison operators
* Equality operators
* Strict equality
* Loose equality
* Logical operators
* Short-circuit evaluation
* Nullish coalescing
* Optional chaining
* Unary operators
* Increment/decrement
* Bitwise operators
* Conditional/ternary operator
* `delete`
* `void`
* `typeof`
* Operator precedence
* Associativity
* Expression evaluation

# 5. Control Flow & Iteration

All mechanisms controlling **what executes and when**.

* `if`
* `else`
* `else if`
* `switch`
* `for`
* `while`
* `do...while`
* `for...in`
* `for...of`
* `break`
* `continue`
* `return`
* Labeled statements
* Conditional execution
* Nested control flow
* Iteration patterns

# 6. Functions

Everything fundamentally related to JavaScript functions.

* Function declarations
* Function expressions
* Arrow functions
* Anonymous functions
* Named functions
* Parameters
* Arguments
* Default parameters
* Rest parameters
* Return values
* First-class functions
* Higher-order functions
* Callback functions
* Function composition
* Nested functions
* Immediately Invoked Function Expressions
* Recursive functions
* Pure vs impure functions
* Function objects

And because we're revising for SDE-2, this topic should also include:

* Closures
* Lexical capture
* Function scope
* Function identity
* Function context
* `this`
* `call`
* `apply`
* `bind`

These belong together conceptually even though some of them could technically be separated.

# 7. Scope, Lexical Environment & Closures

This is the **language-behavior side** of functions and variables.

* Global scope
* Function scope
* Block scope
* Module scope
* Lexical/static scoping
* Dynamic scoping — comparison
* Lexical environment
* Environment records
* Scope chain
* Variable resolution
* Shadowing
* Closures
* Closure lifetime
* Captured variables
* Closure use cases
* Common closure pitfalls

This topic connects heavily with Functions, but its primary concern is:

> **How does JavaScript resolve and retain variables?**

# 8. Objects & Object Model

Everything concerning JavaScript objects.

* Object creation
* Object literals
* Properties
* Property keys
* Property values
* Methods
* Property access
* Dot notation
* Bracket notation
* Computed properties
* Adding/removing properties
* Updating properties
* Nested objects
* Object references
* Object copying
* Shallow copy
* Deep copy
* Object destructuring
* Spread syntax
* Property descriptors
* Getters/setters
* Enumerability
* Property ownership
* Object immutability

And the deeper object model:

* Prototypes
* Prototype chain
* `Object.prototype`
* `__proto__`
* `Object.create`
* Constructor functions
* `new`
* Prototype inheritance

# 9. Classes & Object-Oriented JavaScript

Everything related to JavaScript's class-based abstraction.

* `class`
* Constructor
* Instance methods
* Static methods
* Instance properties
* Static properties
* Private fields
* Getters/setters
* Class inheritance
* `extends`
* `super`
* Method overriding
* Class vs constructor function
* Class vs prototype-based inheritance
* Encapsulation
* Polymorphism
* Composition vs inheritance

# 10. Arrays & Collection Operations

Everything around array-based data manipulation.

* Array creation
* Indexing
* Sparse arrays
* Array length
* Mutating operations
* Non-mutating operations
* Iteration
* `map`
* `filter`
* `reduce`
* `find`
* `findIndex`
* `some`
* `every`
* `forEach`
* `sort`
* `slice`
* `splice`
* `concat`
* `flat`
* `flatMap`
* Array destructuring
* Spread
* Array-like objects
* Converting between collections

# 11. Strings, Numbers, Dates & Built-in Objects

Rather than scattering JavaScript's standard library across multiple topics, group the core built-ins together.

### Strings

* String creation
* Template literals
* String methods
* Searching
* Splitting
* Replacing
* Formatting

### Numbers

* Number methods
* Parsing
* Precision
* `NaN`
* `Infinity`
* `Math`

### Dates

* `Date`
* Timestamps
* Date manipulation
* Time-zone considerations

### Other built-ins

* `RegExp`
* `JSON`
* `Map`
* `Set`
* `WeakMap`
* `WeakSet`

# 12. Iterators, Iterables & Generators

This deserves its own boundary because it represents a distinct abstraction in JavaScript.

* Iterable protocol
* Iterator protocol
* `Symbol.iterator`
* `next()`
* `for...of`
* Custom iterables
* Generators
* `yield`
* Generator functions
* Generator delegation
* Lazy evaluation
* Iteration vs enumeration
* Iterable vs array-like

# 13. Modules & Dependency Management

Everything related to code organization and dependencies.

* ES Modules
* `import`
* `export`
* Named exports
* Default exports
* Namespace imports
* Dynamic imports
* Module scope
* Module evaluation
* Module dependencies
* Circular dependencies
* CommonJS
* `require`
* `module.exports`
* ESM vs CommonJS
* Package boundaries
* Dependency resolution
* Tree shaking at a high level

# 14. Error Handling

Everything concerning failures and error propagation.

* `Error`
* Built-in error types
* Custom errors
* `throw`
* `try`
* `catch`
* `finally`
* Error propagation
* Stack traces
* Synchronous errors
* Asynchronous errors
* Promise rejection
* Error handling strategies
* Fail-fast vs recovery

# 15. Asynchronous JavaScript

The complete asynchronous programming model.

* Synchronous vs asynchronous execution
* Blocking vs non-blocking
* Callbacks
* Callback-based APIs
* Callback nesting
* Callback hell
* Promises
* Promise states
* Promise creation
* Promise resolution
* Promise rejection
* Promise chaining
* `then`
* `catch`
* `finally`
* `async`
* `await`
* Async functions
* Error handling in async code
* Sequential vs concurrent operations
* `Promise.all`
* `Promise.allSettled`
* `Promise.race`
* `Promise.any`

# 16. Event Loop & Concurrency Model

This is where we move from **using async JavaScript** to understanding **why it behaves that way**.

* Call stack
* Execution stack
* Heap
* Event loop
* Task queue
* Microtask queue
* Macrotasks/tasks
* Microtasks
* Promise callbacks
* Timers
* Browser event loop
* Node.js event loop
* Non-blocking I/O
* Concurrency vs parallelism
* Starvation
* Event-loop blocking
* Long-running synchronous work

This will be one of the **highest-priority SDE-2 topics**.

# 17. Memory Management

Understanding how JavaScript manages memory.

* Stack vs heap — conceptual model
* Object allocation
* References
* Reachability
* Garbage collection
* Mark-and-sweep
* Memory leaks
* Detached references
* Closures and memory
* Event listeners and memory
* Timers and memory
* Weak references
* `WeakMap`
* `WeakSet`
* `WeakRef`
* `FinalizationRegistry`
* Memory debugging

We don't need engine-level GC internals yet—that belongs in the Staff-level revision.

# 18. Browser JavaScript & Web APIs

Everything specific to JavaScript running inside a browser.

* DOM
* BOM
* Browser environment
* DOM manipulation
* DOM events
* Event listeners
* Event propagation
* Capturing
* Bubbling
* Event delegation
* Browser storage
* Cookies
* `localStorage`
* `sessionStorage`
* Fetch API
* XMLHttpRequest
* Timers
* Web APIs
* Web Workers
* WebSockets
* Browser security model at a high level

# 19. Node.js JavaScript Runtime

Since you're working with JavaScript beyond the browser, this deserves its own boundary.

* Node.js runtime
* V8
* Node APIs
* Event-driven architecture
* Non-blocking I/O
* Node.js event loop
* File system APIs
* Networking
* HTTP
* Streams
* Buffers
* Process
* Environment variables
* Worker threads
* Child processes
* Node module system
* CommonJS vs ESM in Node

# 20. JavaScript Standard Library & Modern Features

A revision bucket for language features that don't deserve to be scattered everywhere.

* Optional chaining
* Nullish coalescing
* Logical assignment
* Destructuring
* Spread/rest
* Template literals
* Symbols
* BigInt
* Private class fields
* Modern array methods
* Modern object methods
* Modern Promise APIs
* `structuredClone`
* Modern ECMAScript features

# 21. JavaScript Functional Programming

The functional style of JavaScript deserves its own conceptual grouping.

* First-class functions
* Higher-order functions
* Pure functions
* Immutability
* Referential transparency
* Function composition
* Currying
* Partial application
* Map/filter/reduce
* Side effects
* Closures in functional programming
* Declarative vs imperative programming

# 22. Regular Expressions

A relatively isolated language feature.

* Regex syntax
* Character classes
* Quantifiers
* Groups
* Capturing
* Non-capturing groups
* Lookahead/lookbehind
* Flags
* Matching
* Replacing
* Regex performance considerations

# 23. Metaprogramming & Reflection

Advanced language capabilities that are still relevant for an experienced JavaScript developer.

* `Proxy`
* `Reflect`
* Symbols
* Property descriptors
* Dynamic property access
* Getters/setters
* Runtime object manipulation
* Metaprogramming use cases

# 24. JavaScript Tooling & Build Ecosystem

Everything surrounding a real JavaScript project.

* Node.js
* npm
* `package.json`
* `package-lock.json`
* Dependencies
* Dev dependencies
* Semantic versioning
* npm scripts
* Package resolution
* Bundlers
* Transpilers
* Babel
* Source maps
* Minification
* Tree shaking
* Code splitting
* Environment configuration
* Development vs production builds

# 25. Testing & Code Quality

For SDE-2, JavaScript revision shouldn't stop at language syntax.

* Unit testing
* Integration testing
* Test doubles
* Mocking
* Stubbing
* Assertions
* Async testing
* Test isolation
* Code coverage
* Linting
* Formatting
* Static analysis
* Debugging
* Error diagnosis

# 26. JavaScript Performance

This is where normal language knowledge starts becoming engineering knowledge.

* Time complexity of common operations
* Array performance
* Object performance
* Map/Set performance
* Memory allocation
* Garbage collection impact
* Event-loop blocking
* Async performance
* Rendering performance
* DOM performance
* Debouncing
* Throttling
* Memoization
* Lazy evaluation
* Code splitting
* Bundle size
* Profiling
* Benchmarking
* Performance bottleneck identification

# 27. Security

JavaScript-specific application security.

* XSS
* CSRF
* Prototype pollution
* Injection
* Unsafe evaluation
* `eval`
* Dependency vulnerabilities
* Supply-chain attacks
* Token/storage considerations
* CORS
* Same-origin policy
* Content Security Policy
* Secure handling of user input

# 28. Design Patterns & JavaScript Architecture

Now we're moving beyond individual language features into application design.

* Module pattern
* Factory pattern
* Singleton
* Observer
* Pub/Sub
* Strategy
* Adapter
* Decorator
* Proxy
* Dependency injection
* Composition
* Event-driven design
* Layered architecture
* Separation of concerns
* SOLID in JavaScript
* Abstraction boundaries
* Coupling vs cohesion

# 29. Production JavaScript Engineering

The final SDE-2 layer.

* Maintainability
* Readability
* API design
* Error handling strategy
* Logging
* Observability
* Configuration
* Dependency management
* Backward compatibility
* Versioning
* Reliability
* Scalability
* Performance trade-offs
* Concurrency concerns
* Resource management
* Technical debt
* Code review considerations

# The Important Part: Our Revision Hierarchy

Now we have a much cleaner structure:

```text
JAVASCRIPT
│
├── LANGUAGE
│   ├── Fundamentals
│   ├── Variables & Keywords
│   ├── Types & Coercion
│   ├── Operators & Expressions
│   └── Control Flow
│
├── FUNCTIONS & EXECUTION
│   ├── Functions
│   ├── Scope & Closures
│   ├── Objects & Object Model
│   ├── Classes & OOP
│   └── Iterators & Generators
│
├── DATA & STANDARD LIBRARY
│   ├── Arrays
│   ├── Strings / Numbers / Dates
│   ├── Collections
│   └── Regular Expressions
│
├── ASYNCHRONOUS JAVASCRIPT
│   ├── Async Programming
│   ├── Promises
│   └── Event Loop & Concurrency
│
├── RUNTIME
│   ├── Memory Management
│   ├── Browser Runtime
│   └── Node.js Runtime
│
├── CODE ORGANIZATION
│   ├── Modules
│   ├── Dependency Management
│   └── Tooling / Build Systems
│
├── ADVANCED LANGUAGE FEATURES
│   ├── Functional Programming
│   ├── Metaprogramming
│   └── Modern ECMAScript
│
└── ENGINEERING
    ├── Testing
    ├── Performance
    ├── Security
    ├── Design Patterns
    └── Production Engineering
```

### And this gives us the exact revision behavior you described.

If we say:

> **"Let's revise JavaScript Functions."**

We don't ask, *"Which function concepts?"*

We simply open the **Functions** bucket and systematically run through its checklist.

If we say:

> **"Let's revise JavaScript scope."**

We know that includes **global/function/block/module scope, lexical scoping, lexical environments, scope chain, variable resolution, shadowing, and closures**.

If we say:

> **"Let's revise asynchronous JavaScript."**

We know we're going through **callbacks → promises → async/await → concurrency patterns**, while **Event Loop & Concurrency Model** remains the deeper runtime-oriented bucket.

That is the distinction I think you're looking for: **the syllabus is the index; the subtopics are the coverage contract.**

And because this is **revision rather than learning**, we can later mark each subtopic as **Strong / Needs Revision / Weak** instead of spending equal time on everything.
