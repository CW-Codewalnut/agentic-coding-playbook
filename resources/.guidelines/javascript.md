---
name: javascript
description: Team coding guidelines for JavaScript projects. Also applicable when a superset of JS, such as TypeScript or CoffeeScript, is used.
---

- Use `camelCase` for variables, functions, and method names.
- Use `PascalCase` for classes and constructor functions.
- Use `SCREAMING_SNAKE_CASE` for constants and environment variables.
- Name event handlers with `handle` prefix (e.g., `handleClick`, `handleSubmit`).
- Name event handler callbacks with `on` prefix when passed as props (e.g., `onClick`, `onSuccess`).
- Avoid magic numbers/strings; extract them into named constants.
- Ensure each function does one thing and does it well. Extract only when it helps, because too many one-line helpers create noise.
- Limit function parameters to 2; use an options object for more.
- Never mutate function arguments; return new values instead.
- Use nullish coalescing (`??`) instead of `||` for default values with falsy possibilities.
- Use optional chaining (`?.`) for safe property access on nullable objects.
- Use `try/catch` for operations that may throw.
- Use `Promise.all()` for parallel independent async operations.
- Avoid mixing `async/await` with `.then()` in the same function.
- Return promises directly; don't `await` then immediately return.
- Mark functions as `async` only if they use `await`.
- Use `AbortController` whenever needed.
- Prefer template literals over string concatenation; ensure those expressions don’t evaluate to `undefined` or `null`.
- Prefer standard APIs over libraries when sufficient. For example, prefer native `fetch` over `axios`.
- Choose the simplest, most readable solution; avoid clever patterns or unusual syntax when straightforward alternatives exist.
- Lazy load non-critical resources.
- Use Zod or a similar library for runtime validation instead of complex manual conditionals.
