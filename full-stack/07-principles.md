# Essential Coding Principles for 4–6 LPA Developers

*(With explanations)*

## 1. **SOLID Principles**

These are object-oriented design principles to make code maintainable, scalable, and testable.

### S – Single Responsibility Principle (SRP)

A class/module should have only one reason to change.
One class = one job.

### O – Open/Closed Principle (OCP)

Software entities should be open for extension but closed for modification.
Add new functionality without changing existing code.

### L – Liskov Substitution Principle (LSP)

Child classes should be usable in place of parent classes without breaking functionality.

### I – Interface Segregation Principle (ISP)

Clients should not depend on interfaces they don’t use.
Prefer multiple small interfaces over one large one.

### D – Dependency Inversion Principle (DIP)

Depend on abstractions, not concrete implementations.
Use interfaces, dependency injection, inversion of control.

---

## 2. **KISS – Keep It Simple, Stupid**

Write simple, clear, understandable code.
Avoid unnecessary complexity, clever tricks, and over-engineering.

---

## 3. **DRY – Don’t Repeat Yourself**

Avoid duplicating logic.
Duplicate code increases bugs and maintenance cost.
Extract reusable components or functions.

---

## 4. **YAGNI – You Aren’t Gonna Need It**

Do not implement features until they are actually required.
Prevents over-engineering and unnecessary complexity.

---

## 5. **SoC – Separation of Concerns**

Divide a system into distinct sections, each handling a separate concern.
Example: Controllers handle requests, services handle logic, repos handle data.

---

## 6. **Law of Demeter (LoD)**

A module/class should only talk to its direct friends.
Avoid chained calls like `obj.a.b.c.doSomething()`
Reduces tight coupling.

---

## 7. **Principle of Least Surprise**

Code should behave in a way that is predictable and intuitive.
Other developers should not be surprised by your design.

---

## 8. **Fail Fast Principle**

Detect errors as early as possible.
Better to crash early during development than silently continue with invalid state.

---

## 9. **Composition Over Inheritance**

Use composition (has-a) instead of inheritance (is-a) unless inheritance is truly needed.
Helps reduce tight coupling.

---

## 10. **Encapsulation**

Keep data hidden and expose only what is necessary.
Improves reliability and maintainability.

---

## 11. **Immutability Principle**

Prefer immutable objects where possible.
Reduces side-effects, supports thread safety, easier debugging.

---

## 12. **Clean Code Principles (Robert C. Martin)**

* Meaningful names
* Small functions
* Minimal arguments
* Consistent coding style
* Comments only when needed
* Avoid deeply nested code
* Write readable code over clever code

---

## 13. **Testability Principles**

Write code that is easy to unit test.
Avoid static dependencies, singletons, and tight coupling.

---

## 14. **Modularity Principle**

Divide the system into independent modules.
Makes features easier to update or replace.

---

## 15. **Idempotency**

An operation can be performed multiple times with the same effect.
Important in backend, APIs, distributed systems.
