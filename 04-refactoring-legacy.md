# Refactoring Legacy: Agentic Coding Playbook

> [!NOTE]
> **What this is:** existing code that needs better structure, readability, or maintainability without changing behavior.
>
> **How to read:** start with the [Base Guide](./01-base-guide.md). This variant has more changes than Greenfield or Brownfield because refactoring has a different success condition.
>
> **Main rule:** behavior preservation is the success criterion.

Refactoring with agents can feel powerful because the agent can move code quickly. That is also the risk. A clean-looking rewrite that changes behavior is not a refactor. It is a bug with better formatting.

Your job is to define what must stay true before any code moves.

---

## A. Pre-flight

### A.0 Behavior safety net: simplified version, do once per refactor target

Before the refactor, build a safety net.

This is the short version. A full "generate tests from existing code" workflow should also cover fixture design, approval of generated specs, edge-case discovery, and cases where awkward behavior must stay because callers depend on it. That deeper workflow is out of scope for this playbook.

1. **Generate spec from code.** In a fresh session, prompt: _"Read the code in [module/path]. Produce a behavior spec: what does this code do, including edge cases, error paths, and observable side effects? Do not recommend changes. Describe what is there."_
2. **Validate the spec.** Read it. Compare with reality. Edit it. You decide what behavior must be preserved.
3. **Decide retain vs change.** For each spec item, mark it as `must preserve`, `intentionally changing`, or `actually a bug, change`.
4. **Generate characterization tests.** Prompt: _"Generate tests for the `must preserve` items in this spec. Tests should fail if the behavior changes."_
5. **Run tests against current code.** All `must preserve` tests should pass before the refactor starts. That is your baseline.

> [!TIP]
> If a `must preserve` test fails against current code, pause. Either the spec is wrong, or you found behavior that already disagrees with the spec. Decide deliberately.

> [!TIP]
> Characterization tests do not need to be beautiful. They need to catch behavior changes. The purpose is confidence, not a perfect test architecture.

**Outcome:** validated spec, characterization tests, and a green baseline.

### A.0b Internal audit: define the target before moving code

Before writing target rules, inspect the internals you are about to change:

- Architecture shape and module boundaries
- Large modules that need to be split
- Duplicated patterns and mixed responsibilities
- Styling system, theming, and config placement for UI code
- Test gaps and validation gates that need to be added first
- The target end state for the refactored area

This audit prevents the agent from turning messy current code into official convention.

### A.1 Tools and environment

Same as [Base A.1](./01-base-guide.md#a1-tools-and-environment).

### A.2 The agent rules file

**Refactor-specific guidance:** do not blindly extract the conventions of code you are trying to improve. Scope the rules to the refactor target and split them into three buckets:

- **Preserve:** external contracts the refactor must not break, such as public APIs, DB schemas, message formats, route names, logs, metrics, and dashboards.
- **Target:** the conventions the refactored code should move toward, often from a healthier module in the same repo. If no healthy example exists, define the target standard first in `AGENTS.md` or `.guidelines/`, starting from the team rules that apply to this stack.
- **Avoid:** local anti-patterns that should not be copied, such as hidden globals, mixed concerns, swallowed errors, or god modules.

> [!TIP]
> If the repo has a healthy module that shows the style you want, point the agent at it. If it does not, write the standard down before asking for code. A concrete example is clearer than an abstract rule, but an explicit standard is still better than letting the agent infer one from messy code.

### A.3 Scripts contract

Same as [Base A.3](./01-base-guide.md#a3-scripts-contract). Make sure characterization tests are part of the test command.

### A.4 Optional: Playwright auto-screenshots

Same as [Base A.4](./01-base-guide.md#a4-optional-playwright-auto-screenshots-for-ui-work). For UI refactors, before and after screenshots give useful evidence.

---

## B. The Workflow

### B.1 Requirement intake

**Refactor requirement shape:** the requirement is not "build feature X." It is "improve [readability / structure / maintainability / remove debt] of [module] while preserving behavior."

Capture:

- **Quality criteria:** measurable maintainability goals, such as "extract domain logic from controller layer," "split the 800-line module into focused units," or "remove duplicated branching while keeping the public API unchanged."
- **Behavior contract:** pointer to the spec from A.0.
- **Validation gates:** characterization tests, existing unit or integration tests, E2E tests, contract tests, screenshot comparisons, or StyleProof checks that apply to the refactor.
- **Acceptance:** behavior gates stay green, quality criteria are met, and complexity goes down or any temporary increase is justified.

### B.2 Council of Agents brainstorm

**Refactor prompt focus:**

```text
Here is the code: [path].
Here is the quality goal: [paste].
Propose 2 to 3 refactor strategies.
Compare incremental refactor, branch by abstraction, strangler pattern, and parallel implementation where relevant.
Which approach best preserves behavior given our test coverage?
Which approach reduces complexity without creating architecture drift?
```

> [!WARNING]
> For risky refactors, prefer steps that can be reviewed and rolled back one at a time.

### B.3 Consolidated prompt prep

**Refactor chunking rule:** each chunk should be an atomic refactor step where the test suite is green before and after. If a chunk leaves tests red, split it.

> [!TIP]
> "Move method," "extract function," and "rename" are good chunk shapes. "Rewrite the module" is usually too large.

### B.4 Plan mode, review, finalize

Same as [Base B.4](./01-base-guide.md#b4-plan-mode-review-finalize).

### B.5 Implementation

Same as [Base B.5](./01-base-guide.md#b5-implementation).

**Plus:** after every chunk, the agent runs the full test suite, including characterization tests. If a preservation test fails, the chunk is wrong or the spec decision needs review. Do not patch the test just to make the refactor pass.

For UI or style refactors, run the visual or computed-style gate after the relevant chunk, not only at the end.

### B.6 Manual verification and stage

Same as [Base B.6](./01-base-guide.md#b6-manual-verification-and-stage).

**Plus: behavior comparison.**

- For UI, compare before and after screenshots.
- For CSS, Tailwind, design-system, or styling-library refactors, consider [`styleproof`](https://www.npmjs.com/package/styleproof). It can flag computed-style changes between base and head so you can decide whether the visual diff is intentional.
- For backend, run representative inputs and compare outputs.

The test suite gives confidence. The behavior comparison catches what tests missed.

### B.7 Code-quality audit

Same as [Base B.7](./01-base-guide.md#b7-code-quality-audit-same-session).

### B.8 Atomic commits

Same as [Base B.8](./01-base-guide.md#b8-atomic-commits). Each commit should leave tests green.

### B.9 Cross-agent review

**Refactor review lens:** behavior preservation comes first.

Prompt:

```text
Review this refactor branch against [base].
Use the A.0 behavior spec, A.0b internal audit, and validation output as the contract.
Primary question: does behavior change anywhere it should not?
Look for semantic shifts, error path changes, ordering changes, state-management changes, and observable side effects.
Check that characterization tests and other behavior gates stayed green.
Check that visual or computed-style diffs are intentional.
Check that complexity goes down or any increase is justified.
Check that module boundaries move toward the target architecture.
Check that the change does not introduce new architecture drift.
```

### B.10 Fresh-mind manual review

Same as [Base B.10](./01-base-guide.md#b10-fresh-mind-manual-review).

**Plus:** with fresh eyes, run a final behavior comparison against the original spec from A.0. Catch semantic drift before PR review.

### B.11 Open PR via `gh` CLI

Same as [Base B.11](./01-base-guide.md#b11-open-pr-via-gh-cli).

**Plus:** PR description should state what is preserved and what is intentionally changed. Include the spec from A.0 as the contract.

### B.12 AI code-reviewer feedback loop

Same as [Base B.12](./01-base-guide.md#b12-ai-code-reviewer-feedback-loop).

---

## C. Operating principles

Same as [Base C](./01-base-guide.md#c-operating-principles-ambient-rules).

---

## Refactor-specific reminders

- **The behavior contract comes first.** A cleaner implementation that changes behavior accidentally is a regression.
- **Tests stay green at every chunk.** Do not carry red tests into the next step.
- **Document intentional changes.** Separate them from behavior that should be preserved.
- **Small steps are easier to trust.** Refactors succeed when each move is understandable.
