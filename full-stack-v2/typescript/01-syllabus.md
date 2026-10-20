Absolutely. For a **3-year experienced developer switching jobs**, I’d recommend revising TypeScript systematically from fundamentals to advanced interview topics, with emphasis on **why/when you use each feature**, not just syntax.

### TypeScript interview roadmap

1. **TypeScript fundamentals**

   * `string`, `number`, `boolean`, `null`, `undefined`
   * `any` vs `unknown` vs `never`
   * Type inference
   * Type assertions
   * `void`
   * Literal types
   * Union and intersection types

2. **Objects, interfaces & type aliases**

   * `interface` vs `type`
   * Optional and readonly properties
   * Index signatures
   * Extending interfaces
   * Intersection types
   * Declaration merging

3. **Functions**

   * Function parameter and return types
   * Optional/default parameters
   * Rest parameters
   * Function overloads
   * Callbacks
   * Generic functions

4. **Advanced types — very important for interviews**

   * Generics
   * `keyof`
   * `typeof`
   * Indexed access types
   * `in`
   * Conditional types
   * Mapped types
   * Template literal types
   * Utility types

5. **Built-in utility types**

   ```ts
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

6. **Type narrowing**

   * `typeof`
   * `instanceof`
   * `in`
   * Equality narrowing
   * Custom type guards
   * Discriminated unions
   * Exhaustiveness checking

7. **Classes & OOP**

   * `public`, `private`, `protected`
   * `readonly`
   * `static`
   * Abstract classes
   * Constructors
   * Getters/setters
   * `implements`
   * `extends`
   * Composition vs inheritance

8. **Enums**

   * Numeric enums
   * String enums
   * `const enum`
   * Why many modern projects prefer union types over enums

9. **Generics in real applications**

   ```ts
   function identity<T>(value: T): T {
     return value;
   }

   function getProperty<T, K extends keyof T>(
     obj: T,
     key: K
   ): T[K] {
     return obj[key];
   }
   ```

10. **Async TypeScript**

    * `Promise<T>`
    * `async/await`
    * Typing API responses
    * Error handling
    * `Promise.all`
    * `Promise.allSettled`
    * Generic API functions

11. **Modules**

    * `import` / `export`
    * Default vs named exports
    * `type` imports
    * Module resolution
    * CommonJS vs ES modules

12. **`tsconfig.json`**
    Know these especially well:

    ```json
    {
      "strict": true,
      "noImplicitAny": true,
      "strictNullChecks": true,
      "noUnusedLocals": true,
      "noUnusedParameters": true,
      "target": "...",
      "module": "..."
    }
    ```

13. **Real-world TypeScript**

    * Typing REST APIs
    * DTOs
    * API error types
    * React/Angular types
    * Form types
    * Event types
    * State management
    * Reusable generic components/functions
    * Avoiding `any`
    * Designing maintainable types

14. **Interview coding questions**
    You should be comfortable implementing:

    * Generic functions
    * `debounce`
    * `throttle`
    * Deep/partial types
    * Type-safe object utilities
    * API wrappers
    * Type guards
    * Generic repositories/services
    * Data transformation utilities

### What I'd focus on for 3 years' experience

Interviewers generally won't be impressed by simply knowing that `interface` exists. They may ask:

> **“When would you use `interface` instead of `type`?”**

> **“Why is `unknown` safer than `any`?”**

> **“Explain `keyof` with a real example.”**

> **“How would you make this API function type-safe?”**

> **“Write a generic function that accepts an object and a key and returns the correct property type.”**

> **“How does TypeScript narrow union types?”**

> **“Create your own `Pick`/`Partial` utility type.”**

> **“What does `T extends keyof U` mean?”**

> **“How do you type an API response where the data differs by status?”**

For your interview, I can take this further and make you a **complete 3-year TypeScript interview revision**, including **concept → example → common interview question → coding question → answer**, in a practical order.
