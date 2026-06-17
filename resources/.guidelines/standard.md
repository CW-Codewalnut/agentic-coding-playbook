---
name: standard
description: Universal software engineering rules for naming, structure, testing, security, and refactoring. Always include this.
---

# Universal Software Engineering Rules

Rules for writing clear, maintainable code in any language, framework, or project.

## Naming

- Check that names reveal clear and correct intent.
- Avoid single letters even in tiny scopes like one-liner functions/loops.
- Be consistent by using the same term for the same concept everywhere.
- Avoid generic variable names like `data`, `info`, `temp`, `misc`, `item`, `value`, `val`, `event` and similar vague words. These are banned variable names.
- Prefer positive conditions over negatives for readability. Use positive boolean names like `isActive` not `isNotInactive`.
- Prefix booleans with `is`, `has`, `can`, `should`, `was`, `will`.
- Name functions as verbs or verb phrases: `calculateTotal`, `validateInput`.
- Name predicates as questions: `isEmpty`, `hasPermission`, `canExecute`.

## Functions

- Ensure each function does exactly one thing.
- Limit parameters to 2 or fewer; use objects for more.
- Prefer pure functions.
- Return early to avoid deep nesting.
- Avoid optional parameters that drastically change behavior.
- Validate parameters at boundaries, trust them internally.
- Break complex functions into smaller ones.
- Use early returns to reduce nesting.
- Extract complex conditions into named variables.

## Code Organization

- Keep one primary concept per file.
- Name files after their primary export or purpose.
- Organize by feature, not by technical layer.
- Ensure each module does one job well.
- Avoid modules that mix unrelated responsibilities.

## Comments & Documentation

- Comment the "why", not the "what"; code already shows the "what".
- Use JSDoc or similar docs for public APIs and complex reusable utility functions.
- Warn about consequences or gotchas.
- Avoid using comments as a substitute for clear code.
- Clarify complex algorithms or formulas.
- Explain when a team rule is violated or linting is disabled.
- Keep comments up-to-date with code changes.
- Avoid commented-out code; if needed, explain why it exists.
- Document workarounds for bugs.

## Error Handling

- Treat errors as part of the system and design for them.
- Fail fast by detecting and reporting errors early.
- Handle errors at the appropriate level.
- Clean up resources in error paths.
- Never swallow errors silently.
- Use defensive programming at system boundaries.

## Testing

- Treat tests as documentation for how code behaves.
- Test behavior, not implementation.
- Keep tests trustworthy; flaky tests erode confidence.
- Ensure each test verifies one behavior.
- Use descriptive test names.
- Follow Arrange-Act-Assert structure.
- Add unit tests for individual units in isolation.
- Add integration tests for units working together.
- Test boundaries, nulls, empty collections, and error paths.

## Security Basics

- Validate all external input: user inputs, APIs, files, etc.
- Sanitize data before use to prevent injection attacks.
- Trust internal inputs only after validation at boundaries.
- Log security events but never log sensitive data.
- Fail securely; errors shouldn’t reveal system details.

## Universal Anti-Patterns

- Avoid magic numbers and strings; use named constants.
- Avoid spaghetti code with tangled logic.
- Avoid copy-paste programming; abstract duplication.
- Avoid golden hammer solutions; pick the right tool.
- Avoid premature optimization; measure first.
- Avoid boat anchors by removing unused code.
- Avoid complex solutions when simple solutions exist.

## Shared Code Principles

1. DRY (Don't Repeat Yourself)
2. AHA (Avoid Hasty Abstractions)
3. SRP (Single Responsibility Principle)
4. KISS (Keep It Simple, Stupid)
5. YAGNI (You Aren't Gonna Need It)
