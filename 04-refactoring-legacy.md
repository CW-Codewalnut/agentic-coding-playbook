# Refactoring Legacy — Agentic Coding Playbook

> **What this is:** Existing code, possibly old, possibly poorly understood. You're improving structure, performance, or readability — _not_ adding new functionality. **Behavior preservation is the #1 success criterion.**
>
> **How to read:** Outline mirrors the [Base Guide](./01-base-guide.md). Under each phase, you'll see either `→ Same as Base §N.` or `Plus` / `Instead` deltas. This variant has the most divergence — read it carefully.

---

## A. Pre-flight

### A.0 Spec-from-code → tests → baseline — NEW phase, do once per refactor target

Before any refactor, build a safety net.

1. **Generate spec from code.** In a fresh session: _"Read the code in [module/path]. Produce a behavior spec: what does this code do, including edge cases, error paths, and observable side effects? Don't recommend changes — describe what's there."_
2. **Validate the spec.** Read it. Compare with reality. Edit. **You** are the authority on what behavior must be preserved.
3. **Decide retain vs change.** For each spec item: `must preserve` / `intentionally changing` / `actually a bug — change`. Mark each.
4. **Generate characterization tests.** _"Generate tests for the 'must preserve' items in this spec. Tests should fail if the behavior changes."_
5. **Run the tests against the current code.** All "must preserve" tests must pass _before_ any refactor begins. This is your baseline.

> **Pro Tip:** If a "must preserve" test fails on the current code, you've found existing behavior that disagrees with the spec — a hidden bug. Decide: fix-and-update-spec, or document-as-known-quirk-and-update-test.

> **Pro Tip:** Characterization tests don't have to be pretty — they just have to fail when behavior changes. Don't over-engineer them.

**Outcome:** Validated spec, characterization test suite, all green against current code. This is your safety net.

### A.1 Tools & environment

→ Same as Base §A.1.

### A.2 The agent rules file

**Instead:** Don't blindly extract existing conventions — the code you're refactoring is likely the reason those conventions are suspect. Scope the rules to the refactor target (module-level, not whole-project) and split them into three buckets:

- **Preserve** — external contracts the refactor must not break: public APIs, DB schemas, message formats, file/route names other systems depend on, naming that leaks into logs/metrics/dashboards.
- **Target** — the conventions the refactored code should move _toward_: the patterns, structure, naming, and idioms you want to see post-refactor. These often come from the quality goal in B.1 or from a known-good module elsewhere in the repo.
- **Avoid** — anti-patterns present in the current code that the agent should not propagate when moving or extracting it (god objects, hidden globals, mixed concerns, swallowed errors, whatever's specific to your mess).

> **Pro Tip:** If the wider repo has a healthy module you're refactoring _toward_, point the agent at it explicitly: _"Match the structure and conventions of `[good/module]`."_ Concrete exemplar beats abstract rule every time.

### A.3 Scripts contract

→ Same as Base §A.3. Make sure your characterization tests are part of the test script.

### A.4 Optional: Playwright auto-screenshots

→ Same as Base §A.4. Critical for UI refactors — before/after screenshots are evidence.

---

## B. The Workflow

### B.1 Requirement intake

**Instead:** Requirement is _not_ "build feature X." It's "improve [perf / readability / structure / debt] of [module] while preserving behavior."

Capture:

- **Quality criteria** — measurable. E.g. "p95 latency < 50ms", "cyclomatic complexity < 10 per function", "extract domain logic from controller layer".
- **Behavior contract** — pointer to the spec from A.0.
- **Acceptance:** characterization tests stay green; quality criteria are measurably met.

### B.2 Council of Agents brainstorm

**Instead, use this prompt focus:** _"Here's the code [path]. Here's the quality goal [paste]. Propose 2–3 refactor strategies. Compare: incremental vs big-bang, branch-by-abstraction, strangler-fig, parallel-implementation. Which best preserves behavior given our test coverage?"_

> **Pro Tip:** For risky refactors, prefer strategies that allow rollback at every step (incremental + small commits). Big-bang refactors look clean in retrospect; they're terrifying mid-execution.

### B.3 Consolidated prompt prep

**Instead:** Chunk = atomic refactor step where the test suite stays _green_ before and after. If a chunk leaves tests red, it's two chunks.

> **Pro Tip:** "Move method", "extract function", "rename" are good chunk shapes. "Rewrite entire module" is not.

### B.4 Plan mode → review → finalize

→ Same as Base §B.4.

### B.5 Implementation

→ Same as Base §B.5.

**Plus:** After every chunk, the agent must run the full test suite (including characterization tests). If any test fails, the chunk is wrong — revert and reconsider, **don't patch the test.**

### B.6 Manual verification & stage

→ Same as Base §B.6.

**Plus: Behavior diff.**

- For UI: side-by-side screenshots before/after.
- For backend: run the app against representative inputs, capture outputs, compare.

The test suite gives you confidence; the manual diff gives you certainty.

### B.7 Code-quality audit

→ Same as Base §B.7.

### B.8 Atomic commits

→ Same as Base §B.8. Each commit must leave tests green — confirm before pushing.

### B.9 Cross-agent review

**Instead, the primary lens is:** _Behavior preservation._

Prompt: _"Review this refactor branch against [base]. Primary question: does the behavior change anywhere it shouldn't? Look for: subtle semantic shifts, error path changes, ordering changes, state-management changes. Code quality is **not** the focus."_

### B.10 Fresh-mind manual review

→ Same as Base §B.10.

**Plus:** With fresh eyes, run a final behavior comparison against the original — re-execute representative inputs and compare outputs to the spec from A.0. Catch any silent semantic drift now, not in the PR review.

### B.11 Open PR via `gh` CLI

→ Same as Base §B.11.

**Plus:** PR description must explicitly state _what's preserved_ vs _what's changed_. Include the spec from A.0 as the contract. Reviewers (human or AI) need to know the intent.

### B.12 AI code-reviewer feedback loop

→ Same as Base §B.12.

---

## C. Operating principles

→ Same as Base §C.

---

## Refactor-specific reminders

- **The test suite is the contract.** If you find yourself updating "must preserve" tests during a refactor, stop. Either the test was wrong (update it deliberately, document why) or you broke behavior.
- **Tests stay green at every chunk.** Always. No "I'll fix it in the next commit."
- **Behavior preservation > local elegance.** A "cleaner" rewrite that subtly changes semantics is a regression, not a refactor.
- **Document intentional changes** in the PR description, separate from preservation. "We changed X behavior intentionally because Y" should be its own bullet.
- **Big-bang feels heroic, incremental feels boring.** Boring wins.
