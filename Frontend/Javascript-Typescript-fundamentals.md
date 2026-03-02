# JavaScript & TypeScript Mastery Guide (2026 Edition)

## 1. JavaScript Core Fundamentals

Mastering these is non-negotiable for any level of developer.

### Scope & The Temporal Dead Zone (TDZ)

- **Concepts:** Global, Function, and Block scope.
- **Key Difference:** `var` is function-scoped and hoisted with `undefined`. `let` and `const` are block-scoped and hoisted into the **TDZ**—accessing them before declaration throws a `ReferenceError`.

### The Prototype Chain

- **Definition:** Every JS object has a private property pointing to another object called its **prototype**.
- **Inheritance:** When accessing a property, JS looks at the object, then its prototype, then the prototype's prototype, until it reaches `null`.
- **Modern Note:** Use `Object.getPrototypeOf(obj)` instead of the deprecated `__proto__`.

### Closures

- **Definition:** A function bundled together with references to its surrounding state (lexical environment).
- **Use Case:** Data encapsulation (private variables) and functional programming patterns like **Currying**.
- **Example:**

```javascript
function createCounter() {
  let count = 0;
  return () => ++count; // Remembers 'count' even after createCounter finishes
}
```

### The Event Loop

- **Mechanism:** JS is single-threaded but handles concurrency via the Event Loop.
- **Priority Ranking:**

1. **Call Stack:** Synchronous code.
2. **Microtask Queue:** `Promises`, `queueMicrotask`, `MutationObserver` (High priority).
3. **Task Queue (Macrotasks):** `setTimeout`, `setInterval`, I/O (Lower priority).

---

## 2. Asynchronous Mastery

Interviewers focus on how you handle data fetching and race conditions.

### Promise Methods

- **`Promise.all()`**: Fails if **any** promise rejects.
- **`Promise.allSettled()`**: Waits for all to finish, regardless of success or failure.
- **`Promise.race()`**: Settles as soon as the **first** promise settles (win or lose).
- **`Promise.any()`**: Settles as soon as the first **successful** promise settles.

### Error Handling

- Always use `try/catch` with `async/await`.
- Understand the `Unhandled Rejection` event for global error tracking.

---

## 3. TypeScript: Advanced Type System

Shift from "Basic Types" to "Type Logic."

### Interfaces vs. Types

| Feature       | Interface                     | Type Alias                          |
| ------------- | ----------------------------- | ----------------------------------- |
| **Extension** | `extends`                     | Intersection (`&`)                  |
| **Merging**   | Automatic declaration merging | Impossible                          |
| **Usage**     | Best for Object structures    | Best for Unions, Primitives, Tuples |

### Generics

Generics allow you to create reusable components that work with a variety of types while maintaining type safety.

```typescript
function wrapInArray<T>(value: T): T[] {
  return [value];
}
```

### Key Utility Types

- **`Partial<T>`**: All properties optional.
- **`Required<T>`**: All properties required.
- **`Readonly<T>`**: Cannot reassign properties.
- **`Record<K, T>`**: Map keys of type `K` to values of type `T`.
- **`Pick<T, K>`** / **`Omit<T, K>`**: Extract or remove specific keys.

### Type Narrowing & Guards

- **`typeof`**: For primitives.
- **`instanceof`**: For class instances.
- **Type Predicates**: `function isString(val: any): val is string { ... }`

---

## 4. 2026 Modern Features & Best Practices

Demonstrate that your knowledge is current.

- **`structuredClone()`**: The native way to deep-clone objects without losing `Dates` or `RegEx`.
- **Decorators**: Now a standard feature in TS for logging, validation, and Dependency Injection.
- **`satisfies` Operator**: Allows you to validate an object matches a type without losing the specific inferred type of the properties.
- **Explicit Resource Management**: The `using` keyword for automatic cleanup (**Disposables**).

---

## 5. Coding Challenge Topics

Be prepared to implement these on a whiteboard:

1. **Debounce & Throttle:** For performance optimization on scroll/resize events.
2. **Deep Merge/Clone:** Handling nested objects manually.
3. **Flatten Array:** Converting `[1, [2, [3]]]` to `[1, 2, 3]`.
4. **Promisify:** Converting a callback-based function to a `Promise`-based one.
