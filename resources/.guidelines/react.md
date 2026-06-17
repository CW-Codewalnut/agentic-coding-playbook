---
name: react
description: Team coding guidelines for React applications
---

## Component Structure

- Use functional components; avoid class components for new code unless they are necessary, such as Error Boundaries.
- Keep components simple and small; split them when they become large or complex.
- Do not mix business logic, configuration, and rendering in one component. Move that work into helpers, utilities, hooks, or config files.
- Avoid unnecessary wrapper divs.
- Write components that beautifully compose and come together.

## Accessibility

- Never hardcode element IDs in reusable components; use `useId()` hook or similar or accept an ID prop to prevent duplicate IDs when multiple instances render.
- Use semantic HTML elements over generic ones like `div` and `span` wherever possible.
- Ensure at least basic accessibility compliance.

## Props

- Define prop types with TypeScript interfaces or types; be consistent throughout the codebase.
- Avoid passing too many props; consider composition instead.
- Avoid prop drilling; use context or composition.

## State Management

- Keep state as local as possible.
- Derive values from state instead of storing derived state.
- Use functional state updates (`setX(prev => !prev)`) instead of direct value (`setX(!x)`).

## Hooks

- Keep hooks focused; one responsibility per hook.
- Document reusable custom hooks with JSDoc.
- Extract repeated hook logic in multiple places into custom hooks.
- Use custom hooks to share stateful logic.

## useEffect

- Keep one effect per concern; don’t combine unrelated logic.
- Don’t lie about dependencies; if avoiding a dep, explain why.
- Move non-reactive code outside the effect.
- Think twice if the useEffect is even needed.

## useMemo & useCallback

- Use `useMemo` for expensive calculations, not everything.
- Use `useCallback` for functions passed to optimized children or for those put in deps.

## Conditional Rendering

- Extract complex conditions into variables before JSX.
- Use early returns for loading/error states.
- Create separate components for complex conditional branches.
- Avoid deeply nested ternaries in JSX.

## Lists & Keys

- Never use array index as key unless list is truly static.
- Use unique IDs from data as keys.
- Extract list items into separate components for complex renders.

## Forms

- Prevent double submissions and race conditions.
- Keep a single source of truth for form validation logic, ideally the schema.
- Use form libraries to build complex cross-field validation forms.

## Context

- Split contexts by concern; don’t put everything in one context.
- Memoize context values to prevent unnecessary re-renders.

## Testing

- Test component behavior from the user perspective, not implementation.
- Prioritize queries matching user and assistive tech interaction.
- Query order preference: `role`, `label`, `placeholder`, `text`, `displayValue`, `alt`, `title`, then last resort `testId`.
- Test user interactions: clicks, typing, form submission; always use `userEvent`.
- Follow `renderComponent()` pattern at the top of the test file for component unit tests.
- Create fresh `userEvent.setup()` per test (or in `beforeEach`), not at suite scope, to prevent interaction state from leaking between tests.
- Create fresh mock functions (`vi.fn()`) per test or reset them in `beforeEach` to prevent call history from leaking.
- Avoid index-based selectors (`getAllByRole(...)[0]`); query by accessible name/label for resilient tests.
- Assert callback arguments, not just that the callback was called, to catch logic regressions.
- Avoid flaky tests.
- Use `waitFor` wherever needed.

## Misc

- Use portals for modals, tooltips, popovers, etc.
- Use Error Boundaries to catch rendering errors.
- Avoid inline object/function/array creation in JSX props.
- Move object/function/array creation outside the component body when closures aren’t needed.
- Avoid inline styles unless the value must be computed at render time.
- Do not hardcode CSS colors. Use constants, themes, or CSS variables.
- Name event handlers with `handle` prefix (e.g., `handleClick`, `handleSubmit`).
- Name event handler callbacks with `on` prefix when passed as props (e.g., `onClick`, `onSuccess`).
- Use TanStack Query or similar for async operations rather than hand-rolling.
