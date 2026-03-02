# HTML5 & Web Fundamentals: Deep Dive

## 1. Semantic HTML5

Semantic HTML means using elements that clearly describe their meaning to both the browser and the developer.

* **Structure Tags:** `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`.
* **Why it matters:**
* **SEO:** Search engines prioritize semantic content to understand the page hierarchy.
* **Accessibility (A11y):** Screen readers use these tags to help visually impaired users navigate (e.g., "jumping to the main content").
* **Maintainability:** Makes code much easier to read than a "Div Soup" (nested `<div>` everywhere).



---

## 2. The Critical Rendering Path (CRP)

This is the sequence of steps the browser takes to render a page. Optimizing this is key to **Web Performance**.

1. **Construct DOM:** Browser parses HTML and builds the Document Object Model.
2. **Construct CSSOM:** Browser parses CSS and builds the CSS Object Model.
3. **Render Tree:** Combines DOM and CSSOM (ignoring elements like `display: none`).
4. **Layout (Reflow):** Calculates the exact size and position of each element.
5. **Paint:** Fills in pixels (colors, images, borders).

---

## 3. Script Loading: `async` vs `defer`

How you load JavaScript affects the **First Contentful Paint (FCP)**.

* **Normal `<script>`:** HTML parsing stops, script is fetched and executed, then HTML parsing resumes. (Blocks the DOM).
* **`async`:** Script is fetched in parallel with HTML parsing but **executes as soon as it's ready**, pausing the HTML parser. (Good for independent scripts like Analytics).
* **`defer`:** Script is fetched in parallel and **only executes after HTML parsing is complete**. (Best for scripts that depend on the DOM).

[Image comparing script loading execution flow for normal, async, and defer scripts]

---

## 4. Web Storage API

How to store data on the client side without using a database.

| Feature | **LocalStorage** | **SessionStorage** | **Cookies** |
| --- | --- | --- | --- |
| **Capacity** | ~5-10MB | ~5MB | ~4KB |
| **Expiration** | Never (until deleted) | When tab/window closes | Set manually (Max-Age) |
| **Sent to Server** | No | No | Yes (with every request) |
| **Use Case** | User settings, Themes | Form data, temporary state | Auth tokens, Session IDs |

---

## 5. Modern HTML5 Features

* **Form Validation:** Built-in attributes like `required`, `pattern` (regex), `type="email"`, and `min/max`.
* **Graphics:** * **SVG:** Vector-based, scalable, part of the DOM (interactive).
* **Canvas:** Pixel-based, high performance (good for games), not part of the DOM.


* **Audio/Video:** Native tags `<audio>` and `<video>` without needing third-party plugins (Flash).

---

## 6. Important "Head" Metadata

* **Viewport Meta Tag:** `<meta name="viewport" content="width=device-width, initial-scale=1.0">` — Essential for **Responsive Design**.
* **Character Set:** `<meta charset="UTF-8">` — Prevents text rendering issues.
* **Open Graph (OG) Tags:** `<meta property="og:title" ...>` — Controls how your link looks when shared on social media (Facebook, LinkedIn).

---

### Interview "Pro-Tips":

* **Avoid "Div-itis":** If an interviewer sees you using `<div class="header">` instead of `<header>`, it’s a red flag.
* **The "Script at the bottom" myth:** Older tutorials say put scripts at the end of `<body>`. Modern best practice is putting them in the `<head>` with the `defer` attribute.
* **Self-Closing Tags:** Mention that in HTML5, tags like `<img>` or `<br>` don't strictly require a trailing slash (unlike XHTML/XML).

**Would you like me to move on to a detailed "Deep Dive" for CSS (Box Model, Flex vs. Grid) next?**