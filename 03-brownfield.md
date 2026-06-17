# Brownfield: Agentic Coding Playbook

> [!NOTE]
> **What this is:** an existing project with existing code. You are adding features without pretending every current pattern is worth copying.
>
> **How to read:** start with the [Base Guide](./01-base-guide.md). This file follows the same outline and calls out what changes for brownfield work.
>
> _Scope: reasonably sized feature work. See the Base Guide intro._

Brownfield work is where good engineering judgment matters most.

Existing code is useful context, but it is not automatically a source of good conventions. Some projects are healthy: the architecture is coherent, tests catch real regressions, and current patterns are worth following. Some projects need stabilization before fast feature work makes sense.

Your first decision is simple: is this repo ready for feature work, or do we need to map behavior and establish gates first?

---

## A. Pre-flight

### A.0 Brownfield readiness pass: do once per project or target area

Before starting, decide whether the existing repo is healthy enough to use as a guide.

Every brownfield target gets a minimum behavior map, a human verdict on that behavior, and a list of gates that prove the behavior did not drift.

If the project is healthy, keep the pass lightweight: map only the relevant flows, confirm which behaviors are correct, list the scripts and conventions worth following, then move into feature work.

If the project is risky, inconsistent, or poorly understood, run the readiness pass first:

1. **Map behavior.** Walk through current user-visible behavior step by step. Include happy paths, edge cases, error paths, audit logs, background jobs, and integration points.
2. **Confirm behavior.** Mark each behavior as `correct`, `wrong`, `unknown`, or `intentionally changing`. Do not let the agent decide this alone.
3. **Add validation gates.** Use the gates that fit the project: existing tests, characterization tests, E2E tests, contract tests between services, mobile E2E where applicable, and StyleProof for UI or style surfaces where visual drift matters.
4. **Audit internals.** Check architecture shape, large modules, duplicated patterns, styling-system consistency, theming and config placement, test gaps, and the desired end state.
5. **Write the context files last.** Create `AGENTS.md` and `.guidelines/` from the target conventions you want, not from every legacy pattern that happens to exist. Start from the team guidelines where they fit, then add repo-specific `preserve`, `target`, and `avoid` notes.

Use this caveat near the top of the `AGENTS.md` file when the codebase is in transition:

```text
This codebase is in transition. Follow the target conventions below for new work.
Do not copy legacy patterns unless this file explicitly says they are still preferred.
```

> [!TIP]
> Mapping behavior comes before setting conventions. If the current conventions are part of the problem, copying them into `AGENTS.md` teaches the agent to preserve the wrong thing.

> [!TIP]
> Code complexity should trend down in stabilization work. If a change makes a module harder to understand, the plan should explain why that trade-off is temporary or necessary.

**Outcome:** behavior is mapped, validation gates are known, target conventions are documented, and the agent has useful context files.

### A.1 Tools and environment

Same as [Base A.1](./01-base-guide.md#a1-tools-and-environment).

### A.2 The agent rules file

Same as [Base A.2](./01-base-guide.md#a2-the-agent-rules-file-agentsmd-claudemd-guidelines), but populated from the A.0 readiness pass.

For a healthy repo, document the conventions currently worth following.

For an unhealthy repo, document north-star conventions: the patterns, boundaries, and quality gates the codebase is moving toward. Name legacy patterns only when the agent needs to avoid them or preserve them for compatibility. If the team `.guidelines/` conflict with the current code, mark the current pattern as legacy instead of letting the agent copy it silently.

### A.3 Scripts contract

Same as [Base A.3](./01-base-guide.md#a3-scripts-contract). List existing scripts by name in `AGENTS.md`. Add missing validation gates before trusting the agent with risky changes.

### A.4 Optional: Playwright auto-screenshots

Same as [Base A.4](./01-base-guide.md#a4-optional-playwright-auto-screenshots-for-ui-work).

For UI-heavy brownfield work, screenshots are the baseline. For style-system or visual-preservation work, consider StyleProof as a stricter computed-style gate.

---

## B. The Workflow

### B.1 Requirement intake

Same as [Base B.1](./01-base-guide.md#b1-requirement-intake).

**Plus:** write down what must not change before writing what should change.

Capture:

- Existing behavior that must continue to work
- Adjacent flows that share modules, state, APIs, or UI surfaces
- Validation gates that must stay green
- Visual regression expectations for UI work
- Any allowed complexity increase, with the reason and rollback plan

### B.2 Council of Agents brainstorm

Same as [Base B.2](./01-base-guide.md#b2-council-of-agents-brainstorm).

**Plus:** each council session reads the relevant existing code and the A.0 readiness notes before proposing approaches.

Prompt shape:

```text
Read [relevant files or modules] and the brownfield readiness notes.
Then propose 2 to 3 approaches for [feature] that preserve existing behavior.
Call out regression risk in adjacent flows.
Call out any current convention that looks unsafe to follow.
Prefer the target conventions in AGENTS.md over legacy patterns.
```

### B.3 Consolidated prompt prep

Same as [Base B.3](./01-base-guide.md#b3-consolidated-prompt-prep).

**Plus:** bias chunks smaller. Each chunk should touch as few files as practical and leave validation gates green. Brownfield risk comes from unintended changes in nearby behavior.

### B.4 Plan mode, review, finalize

Same as [Base B.4](./01-base-guide.md#b4-plan-mode-review-finalize).

**Plus:** the plan should explicitly list existing behaviors to preserve, adjacent flows to verify, and validation gates to run. If the project is in transition, the plan should also explain how the change moves toward the target conventions.

### B.5 Implementation

Same as [Base B.5](./01-base-guide.md#b5-implementation).

**Plus:** after each meaningful chunk, run the validation gates relevant to the touched area. If behavior gates fail, fix the code or revisit the plan before continuing.

### B.6 Manual verification and stage

Same as [Base B.6](./01-base-guide.md#b6-manual-verification-and-stage).

**Plus:** manually exercise adjacent features that share modules with the new code. The feature you break is often next to the feature you changed.

For UI work, compare screenshots. For visual-preservation work, review StyleProof output if configured.

### B.7 Code-quality audit

Same as [Base B.7](./01-base-guide.md#b7-code-quality-audit-same-session).

**Plus:** ask the audit to check that new code follows the target conventions from A.0. It should also flag unnecessary complexity, new duplicated patterns, and places where legacy patterns were copied without a reason.

### B.8 Atomic commits

Same as [Base B.8](./01-base-guide.md#b8-atomic-commits).

### B.9 Cross-agent review

Same as [Base B.9](./01-base-guide.md#b9-cross-agent-review-review).

**Plus:** include this in the review prompt:

```text
Review this as brownfield work.
Use the behavior map and validation output as the contract.
Check for regressions in existing features, especially adjacent flows that share modules or state.
Check for accidental behavior changes, visual or computed-style drift, and architecture drift.
Check whether complexity increased without a clear reason.
Check whether the code follows the target conventions in AGENTS.md rather than copying unsafe legacy patterns.
```

### B.10 Fresh-mind manual review

Same as [Base B.10](./01-base-guide.md#b10-fresh-mind-manual-review).

**Plus:** read the change against the behavior map. You should be able to say which existing behaviors were protected, which gates proved it, and which risks remain.

### B.11 Open PR via `gh` CLI

Same as [Base B.11](./01-base-guide.md#b11-open-pr-via-gh-cli).

**Plus:** include the brownfield safety notes in the PR: preserved behavior, validation gates run, adjacent flows verified, visual checks if relevant, and any intentional convention shift.

### B.12 AI code-reviewer feedback loop

Same as [Base B.12](./01-base-guide.md#b12-ai-code-reviewer-feedback-loop).

---

## C. Operating principles

Same as [Base C](./01-base-guide.md#c-operating-principles-ambient-rules).

---

## Brownfield-specific reminders

- **Map behavior before conventions.** If the current patterns are wrong, do not teach them to the agent as rules.
- **Healthy repos can move faster.** Risky repos need behavior gates and target conventions first.
- **Keep chunks reviewable.** Brownfield risk grows with diff size.
- **Check neighboring flows.** Shared modules are where accidental regressions hide.
- **Complexity should not creep upward.** If it does, make the reason explicit.
