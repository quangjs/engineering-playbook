# AI-Assisted Coding — Best Practices Guide

---

## Table of Contents

1. [Write Clear, Specific Prompts](#1-write-clear-specific-prompts)
2. [Work Iteratively, Not All at Once](#2-work-iteratively-not-all-at-once)
3. [Manage Context Effectively](#3-manage-context-effectively)
4. [Own the Code — Never Blindly Trust AI](#4-own-the-code--never-blindly-trust-ai)
5. [Use AI to Write Tests, Not Skip Them](#5-use-ai-to-write-tests-not-skip-them)
6. [Establish Team-Wide AI Conventions](#6-establish-team-wide-ai-conventions)
7. [Demonstrating AI Fluency in Interviews](#7-demonstrating-ai-fluency-in-interviews)
8. [Quick Reference by Tool](#8-quick-reference-by-tool)
9. [The 8 Golden Rules](#the-8-golden-rules)

---

## 1. Write Clear, Specific Prompts

The single most impactful skill when working with any AI tool is prompt quality. Vague prompts produce vague outputs. Specific, context-rich prompts produce accurate, actionable code.

### 1.1 Always provide context

AI has no memory of your codebase. Tell it the language, framework, existing patterns, and constraints up front.

#### ❌ Bad

```
Write me a hook to fetch a user.
```

#### ✅ Good

```
Write a React hook that fetches user data from /api/users/:id,
uses SWR for caching, handles loading and error states, and
returns { user, isLoading, error }.
TypeScript. No class components.
```

---

### 1.2 Specify the output format

Tell the AI exactly what you want back: a function, a class, a module with tests, a diff, pseudo-code, etc.

#### ❌ Bad

```
Test my email validator.
```

#### ✅ Good

```
Generate a Jest test file for the validateEmail() utility in src/utils.ts.
Include tests for: valid emails, missing @, missing domain, empty string, null input.
Use describe/it blocks. No test runner config.
```

---

### 1.3 Constrain the scope

AI can go too far and rewrite unrelated code. Explicitly state what should NOT change.

#### ❌ Bad

```
Make this code faster.
```

#### ✅ Good

```
Refactor the sortUsers() function only.
Do NOT change the calling code, types, or file structure.
Keep existing JSDoc.
Goal: reduce time complexity from O(n²) to O(n log n).
```

---

> 💡 **Pro tip: Use a prompt template for your team**
>
> ```
> Context:     [language, framework, file, existing pattern]
> Task:        [what you need]
> Constraints: [what NOT to change, style rules]
> Output:      [function / file / diff / explanation]
> ```

---

## 2. Work Iteratively, Not All at Once

One of the most common mistakes is asking AI to build an entire feature in one shot. This produces massive diffs that are hard to review, test, and debug. Treat AI like a pair programmer: go step by step.

### 2.1 Break large tasks into small steps

#### ❌ Bad

```
Build me a full shopping cart feature with products,
discounts, tax, checkout, and tests.
```

#### ✅ Good

```
Step 1: Define the TypeScript interface for the ShoppingCart model.

Step 2: Write addItem() with quantity logic.

Step 3: Write removeItem() and clearCart().

Step 4: Add calculateTotal() with discount support.

Step 5: Write unit tests for each method.
```

---

### 2.2 Review output before continuing

Never blindly accept AI output and build on top of it. Pause, read the code, run it, and confirm it does what you expect before asking for the next step.

> 💡 **Review Checklist (before accepting AI output)**
>
> - Does it match what I asked for?
> - Are there any hardcoded values that should be config?
> - Does it handle edge cases (null, empty, large input)?
> - Are there obvious security issues (SQL injection, no auth check)?
> - Does it follow the project's existing code style?

---

### 2.3 Use AI for refinement, not just generation

After writing code yourself, use AI to review, refactor, and improve it. This often produces better results than asking AI to start from scratch.

#### ❌ Bad

```
Write a debounce function.
```

#### ✅ Good

```
Here is my implementation of debounce().
Review it for: correctness, edge cases, readability, and TypeScript best practices.
Suggest improvements with explanations.

[paste your code here]
```

---

## 3. Manage Context Effectively

AI tools only know what you tell them in the current conversation or context window. Effective engineers learn to load the right context and keep conversations focused.

### 3.1 Paste relevant code, not the whole file

Share the specific functions, types, and interfaces the AI needs. More irrelevant code means more noise and a higher chance of wrong assumptions.

#### ❌ Bad

```
[pastes 400-line file]

Add order validation to my code.
```

#### ✅ Good

```
Here is the relevant interface:
  interface Order { id: string; items: Item[]; }

And the existing processPayment() signature:
  async function processPayment(order: Order)

Now write a validateOrder() function that checks:
- items array is non-empty
- order.id exists and is a non-empty string
```

---

### 3.2 Use @ mentions and file references (Cursor)

In Cursor, use `@filename`, `@symbol`, or `@docs` to give the AI precise context from your codebase without manual copy-paste.

#### ❌ Bad

```
[In Cursor, no file references]

Update login to use the new session type.
```

#### ✅ Good

```
[In Cursor]

@src/models/user.ts @src/api/auth.ts
Update the login endpoint to use the new UserSession
type defined in the model.
```

---

### 3.3 Start fresh for unrelated tasks

Long conversation history can confuse the AI and cause hallucinations based on earlier context.

> 💡 **When to start a new conversation**
>
> - Switching between different features or modules
> - After the AI starts giving inconsistent or confused answers
> - When a task is completely different from what you've been discussing
> - Before asking for a code review (clean context = cleaner feedback)

---

## 4. Own the Code — Never Blindly Trust AI

AI-generated code can look correct but contain subtle bugs, security vulnerabilities, or outdated patterns. You are responsible for every line that goes into production.

### 4.1 Always review for security issues

AI commonly produces code with SQL injection, missing auth checks, exposed secrets, or improper input sanitization.

#### ❌ Bad — AI-generated, vulnerable

```javascript
// String interpolation — SQL injection risk!
const user = await db.query(`SELECT * FROM users WHERE id = ${userId}`);
```

#### ✅ Good — parameterized query

```javascript
// Safe: parameterized query
const user = await db.query("SELECT * FROM users WHERE id = $1", [userId]);
```

---

### 4.2 Explicitly ask AI to handle edge cases

By default, AI generates happy-path code. Ask it to handle errors, null inputs, empty arrays, rate limits, and network failures explicitly.

#### ❌ Bad

```
Write a fetchUserProfile function.
```

#### ✅ Good

```
Write fetchUserProfile(id: string).
Handle these cases:
- id is null or empty string → throw Error
- Network timeout → retry once, then throw
- 404 response → return null
- Non-2xx → throw with status code
Use async/await with try/catch.
```

---

### 4.3 Ask AI to explain its own output

If you don't fully understand AI-generated code, ask it to explain before shipping it.

#### ❌ Bad

```
[pastes AI code directly into codebase without understanding it]
[ships]
```

#### ✅ Good

```
Explain this code line by line.
Especially: why is useMemo used here, what does the
dependency array do, and what would happen if we removed it?

[paste code]
```

---

> ⚠️ **Security red flags in AI output**
>
> - String interpolation in SQL queries
> - Hardcoded credentials or API keys (even as placeholders)
> - Missing input validation or sanitization
> - `eval()`, `innerHTML`, or `dangerouslySetInnerHTML` without explanation
> - No authentication/authorization checks on sensitive endpoints

---

## 5. Use AI to Write Tests, Not Skip Them

AI excels at generating test cases — often thinking of edge cases you would have missed. Use this to your advantage.

### 5.1 Generate tests alongside implementation

#### ❌ Bad

```
Write formatCurrency().
[writes tests separately later, or not at all]
```

#### ✅ Good

```
Implement formatCurrency(amount, currency).
Then write Jest tests covering:
- Standard formatting (USD, EUR, JPY)
- Zero and negative amounts
- Very large numbers
- Invalid currency code
- Non-numeric input
Put implementation and tests in separate code blocks.
```

---

### 5.2 Use AI to debug by sharing full error context

Don't just share the error message. Share the code, the error, what you expected, and what actually happened.

#### ❌ Bad

```
Why isn't my map working? I keep getting a TypeError.
```

#### ✅ Good

```
I'm getting this error:
  TypeError: Cannot read properties of undefined (reading 'map') at line 24.

Expected: render a list of users
Actual: crash on component mount

Here is the component: [paste code]
And the API response structure: [paste JSON]
```

---

### 5.3 Ask AI to find gaps in your test suite

After writing your tests, ask AI to review them and suggest missing cases.

#### ✅ Good

```
Here is my test suite for parseDate().
Review it and tell me what edge cases or scenarios I might have missed.
Do not rewrite the tests, just list gaps.

[paste existing tests]
```

---

## 6. Establish Team-Wide AI Conventions

Teams that define shared AI conventions see dramatically better outcomes than teams where every engineer uses AI differently.

### 6.1 Create a shared prompt library

Keep a team repo or wiki page of high-quality prompts that work well for your codebase.

> 💡 **What to include in your team prompt library**
>
> - New API endpoint scaffold (with auth, validation, error handling)
> - Database migration template
> - React component with types and Storybook story
> - Unit test scaffold for your specific testing setup
> - PR description generator (given a diff)
> - Architecture review checklist prompt

---

### 6.2 Define what AI should and should not do

| ✅ AI-Encouraged            | ⚠️ Requires Human Review   |
| --------------------------- | -------------------------- |
| Boilerplate & scaffolding   | Security-critical logic    |
| Unit test generation        | Auth & permissions         |
| Documentation & JSDoc       | Database schema changes    |
| Refactoring for readability | Business logic decisions   |
| Explaining unfamiliar code  | Architecture choices       |
| Regex & data transformation | Performance-critical paths |
| PR description first drafts | Legal / compliance code    |

---

### 6.3 Include AI usage in code review

Encourage engineers to note in PRs when code was AI-assisted.

#### ❌ Bad PR description

```
Added email validator.
```

#### ✅ Good PR description

```
This adds the email validation utility.

AI-assisted: initial implementation and tests.
Manually reviewed for: edge cases, sanitization, and
alignment with existing utils patterns.
Added: null check (AI missed this).
```

---

## 7. Demonstrating AI Fluency in Interviews

AI literacy is increasingly evaluated in engineering interviews. Interviewers want to see that you can leverage AI effectively without becoming dependent on it.

### 7.1 Articulate your AI workflow

#### ❌ Weak answer

```
Interviewer: How do you use AI in your workflow?

"I use ChatGPT and Copilot sometimes.
They're pretty helpful for autocomplete."
```

#### ✅ Strong answer

```
Interviewer: How do you use AI in your workflow?

"I use Cursor for in-editor completions and refactoring,
and Claude for architecture discussions and explaining
unfamiliar code. I always review AI output for security
issues and missing error handling, and I ask AI to generate
unit tests alongside implementation. For our team, I
maintain a shared prompt library."
```

---

### 7.2 Show critical thinking about AI limitations

#### ❌ Weak

```
"AI is usually right so I just copy the output and run it."
```

#### ✅ Strong

```
"I always check AI-generated SQL for injection vulnerabilities —
AI often uses string interpolation instead of parameterized queries.
I also verify any library/API suggestions because AI training
data can be out of date."
```

---

### 7.3 Quantify your productivity improvements

> 💡 **Example statements that impress interviewers**
>
> - _"I use AI to scaffold tests, which cuts my test-writing time by roughly 50%"_
> - _"Our team's PR prompt template reduced average PR description time from 15 min to 2 min"_
> - _"I use Claude to review my code before PR — it catches about 2-3 issues per PR on average"_
> - _"I build AI prompts iteratively the same way I build code: test, refine, document"_

---

## The 8 Golden Rules

| #   | Rule                                                                            |
| --- | ------------------------------------------------------------------------------- |
| 1   | **Be specific** — context, constraints, and output format in every prompt       |
| 2   | **Work iteratively** — small steps, review at each stage                        |
| 3   | **Own the code** — review every line before shipping                            |
| 4   | **Ask for edge cases** and error handling explicitly                            |
| 5   | **Generate tests alongside implementation**, not after                          |
| 6   | **Start fresh conversations** for unrelated tasks                               |
| 7   | **Establish team conventions**: prompt library, PR notes, clear guidelines      |
| 8   | **Stay fluent** — understand limitations, hallucinations, and when to go manual |

---

> _The best AI-assisted engineers are not those who use AI the most —_  
> **_they are those who use it most thoughtfully._**
