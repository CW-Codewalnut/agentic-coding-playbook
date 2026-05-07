# Brownfield — Agentic Coding Playbook

> **What this is:** Existing project with existing code. You're adding new features without rewriting what's there. Architecture is a constraint, not a choice.
>
> **How to read:** Outline mirrors the [Base Guide](./01-base-guide.md). Under each phase, you'll see either `→ Same as Base §N.` or `Plus` / `Instead` deltas.
>
> _Scope: reasonably-sized feature work (see Base intro)._

---

## A. Pre-flight

### A.0 Pattern-extraction pass — NEW phase, do once per project

Before writing any new feature, capture _what's actually there_ into your agent rules file.

- In a fresh session: _"Read this codebase and write me an `AGENTS.md` that describes the actual conventions in use: folder structure, naming patterns, error handling, logging, common abstractions, testing patterns. Don't editorialize — describe what's there."_
- Review the output. Edit it for correctness — agents will hallucinate "conventions" from limited examples.
- Do the same for `.guidelines/` files: ask the agent to extract per-language conventions (TypeScript, React, Python, etc.) into separate files.
- Manually edit. **You** are the source of truth — the agent's first pass is a starting draft.

> **Pro Tip:** This is a one-time cost. Spend a couple of hours getting it right; you'll recoup it in every subsequent feature session.

> **Pro Tip:** If the codebase has multiple inconsistent patterns, document the _preferred_ one and add a note like "legacy modules use [old pattern] — don't follow that for new code."

**Outcome:** AGENTS.md and `.guidelines/` reflecting the existing project's actual conventions.

### A.1 Tools & environment

→ Same as Base §A.1.

### A.2 The agent rules file

→ Same as Base §A.2 — but populated from A.0, not from a template.

### A.3 Scripts contract

→ Same as Base §A.3. The scripts already exist — list them by name in `AGENTS.md`.

### A.4 Optional: Playwright auto-screenshots

→ Same as Base §A.4.

---

## B. The Workflow

### B.1 Requirement intake

→ Same as Base §B.1.

**Plus:** Confirm no conflict with existing features. Phrase explicitly: _"Existing features X, Y, Z must continue to work unchanged."_ Make backward compatibility part of the acceptance criteria.

### B.2 Council of Agents brainstorm

→ Same as Base §B.2.

**Plus:** Each council session must read the _existing relevant code_ before proposing approaches. Phrasing: _"Read [list of relevant files / modules]. Then propose 2–3 approaches that fit the existing architecture. Industry best practice matters less than fit."_

### B.3 Consolidated prompt prep

→ Same as Base §B.3.

**Plus:** Bias chunks smaller. Each chunk should have minimal blast radius — touch as few files as possible. **Brownfield risk = unintended regression in unrelated code.**

### B.4 Plan mode → review → finalize

→ Same as Base §B.4.

### B.5 Implementation

→ Same as Base §B.5.

### B.6 Manual verification & stage

→ Same as Base §B.6.

**Plus:** Manually exercise _adjacent_ features that share modules with your new code. Even with full E2E coverage, eyeball the neighbors.

### B.7 Code-quality audit

→ Same as Base §B.7.

**Plus:** The audit should specifically check that new code matches the _existing_ patterns extracted in A.0 — not the conventions in some external reference.

### B.8 Atomic commits

→ Same as Base §B.8.

### B.9 Cross-agent review

→ Same as Base §B.9.

**Plus:** Review prompt must include: _"Check for regressions in unrelated features that share modules / state with the changes. Flag any place where the new code might inadvertently change behavior elsewhere."_

### B.10 Fresh-mind manual review

→ Same as Base §B.10.

### B.11 Open PR via `gh` CLI

→ Same as Base §B.11.

### B.12 AI code-reviewer feedback loop

→ Same as Base §B.12.

---

## C. Operating principles

→ Same as Base §C.

---

## Brownfield-specific reminders

- **Mimic, don't optimize.** Match existing patterns even if you'd write them differently in greenfield. Consistency > local elegance.
- **Smaller chunks beat bigger ones.** Brownfield risk compounds with diff size.
- **Eyeball the neighbors.** The feature you broke is rarely the feature you changed.
- **A.0 pays back forever.** The hour you spend extracting conventions saves dozens of "why did the agent invent its own pattern?" corrections.
