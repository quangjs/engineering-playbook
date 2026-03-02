## 1. Debounce (The "Wait until I'm done" approach)

**The Concept:** Debouncing ensures a function is only triggered **after** a certain amount of silence. Every time the event occurs, the timer resets.

**Real-World Analogy:** An **Elevator**.
The doors are about to close, but someone walks in. The timer resets. Another person walks in; it resets again. The elevator only moves once there has been a steady silence of 5 seconds.

### 🔎 Real Example: Search Bar (Type-ahead)

If you fetch API results on every single keystroke, typing "MacBook" would trigger 7 API calls. With a 300ms debounce, it only fires once you stop typing.

```javascript
function debounce(func, delay) {
  let timeoutId;
  return (...args) => {
    clearTimeout(timeoutId); // Reset the timer on every call
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Usage: Only calls the API 300ms after the user stops typing
const searchAPI = debounce(
  (query) => console.log("Searching for:", query),
  300,
);
```

---

## 2. Throttle (The "Speed Limit" approach)

**The Concept:** Throttling ensures a function is called **at most once** in a specified time period. It "chokes" the execution rate.

**Real-World Analogy:** A **Water Faucet**.
No matter how hard you turn the handle or how fast the water wants to come out, the pipe only allows a steady stream of X liters per minute.

### 🔎 Real Example: Scroll Events (Infinite Scroll)

If you are checking if a user has reached the bottom of a page to load more content, the `scroll` event fires dozens of times per second. This can tank performance. Throttling ensures you only check the scroll position once every 200ms.

```javascript
function throttle(func, limit) {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit); // Unlock after the limit
    }
  };
}

// Usage: Logs scroll position max once every 200ms
const handleScroll = throttle(() => console.log("Checking scroll..."), 200);
window.addEventListener("scroll", handleScroll);
```

---

## Quick Comparison Table

| Feature          | Debounce                               | Throttle                                |
| ---------------- | -------------------------------------- | --------------------------------------- |
| **Best For**     | Heavy tasks (API calls, validation).   | Continuous tasks (Scrolling, resizing). |
| **Execution**    | Executes at the **end** of the flurry. | Executes at **regular intervals**.      |
| **Mental Model** | "Don't do anything until they stop."   | "Only do this once every X seconds."    |

---

> **Bonus Tip:** In modern production environments, you rarely write these from scratch. Use **Lodash** (`_.debounce`) or **RxJS** operators to handle these patterns reliably.

**Would you like to see how to implement a "Leading Edge" debounce, where the function fires immediately on the first click but ignores subsequent fast clicks?**
