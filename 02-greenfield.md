# Greenfield: Agentic Coding Playbook

> [!NOTE]
> **What this is:** a brand-new project with no existing code. You are choosing the architecture, runtime, database, deploy target, and engineering conventions.
>
> **How to read:** start with the [Base Guide](./01-base-guide.md). This file follows the same outline and calls out what changes for greenfield work.

Greenfield work is tempting because the agent can generate a lot quickly. That speed is useful, but it can also create a messy foundation fast.

Use agents as thought partners before you use them as builders. First decide the shape of the system: frontend and backend boundaries, API contracts, data model, auth, deployment, and observability. Then ask the agent to implement inside that shape.

Example: if you are building a new internal approval app, do not start with "build the app." Start by asking agents to compare a full-stack app, a separate API plus frontend, and a workflow-backed approach. Decide the interfaces. Write the first ADR. Then generate code.

---

## A. Pre-flight

### A.0 Architecture brainstorm: NEW phase, do first

Before code or `AGENTS.md` exists, decide the architecture shape.

- Open a plain-text editor. Write the product vision in 3 lines, expected user load, core domain entities, and must-have constraints.
- Run a **Council of Agents** brainstorm using your chosen tools from Claude Code, Codex, and Cursor. Use separate fresh sessions. Ask each session to research and propose options for:
  - Frontend, backend, or full-stack framework
  - Runtime, such as Bun, Node, Python, or Go
  - Database, such as Postgres, MySQL, SQLite, or a managed service
  - Auth model
  - Deployment topology, such as monolith, services, or serverless
  - Observability stack
- Consolidate the answers. Choose one architecture shape.
- Write **ADR-001** using [Michael Nygard's ADR template](https://github.com/joelparkerhenderson/architecture-decision-record/blob/main/locales/en/templates/decision-record-template-by-michael-nygard/index.md). Capture the decision and why you chose it.

**Outcome:** `docs/adr/` contains ADR-001. You know what to put in `AGENTS.md`.

### A.1 Tools and environment

Same as [Base A.1](./01-base-guide.md#a1-tools-and-environment).

### A.2 The agent rules file

**Plus:** you are bootstrapping from scratch. Use the reference templates (see [Resources: Reference templates](./06-resources.md#reference-templates-agentsmd-and-guidelines)) as a starting point.

- Copy the reference `.guidelines/` directory.
- Adopt files for languages or frameworks in your stack.
- Keep the general team conventions that apply.
- Write a 3 to 4 line `AGENTS.md` overview section for the new project.
- Add scripts as you create them.
- Start minimal. Let conventions grow as the project reveals them.

### A.3 Scripts contract

Same as [Base A.3](./01-base-guide.md#a3-scripts-contract). Set these up early because the agent needs a repeatable way to check its work.

### A.4 Optional: Playwright auto-screenshots

Same as [Base A.4](./01-base-guide.md#a4-optional-playwright-auto-screenshots-for-ui-work). If the project has a UI, set this up before the first UI feature.

---

## B. The Workflow

### B.1 Requirement intake

Same as [Base B.1](./01-base-guide.md#b1-requirement-intake).

**Plus:** greenfield has no existing patterns to anchor decisions. Be explicit about success criteria, data boundaries, and what you are not building yet.

### B.2 Council of Agents brainstorm

Same as [Base B.2](./01-base-guide.md#b2-council-of-agents-brainstorm).

**Plus:** because the codebase does not constrain you yet, ask the council to compare documented approaches, trade-offs, and failure modes in your chosen stack.

Prompt shape:

```text
I have full freedom on implementation inside this stack: [stack].
Research how teams usually solve [problem] today.
Compare 3 approaches.
Call out what each approach makes easier, what it makes harder, and what decision we would regret later.
```

### B.3 Consolidated prompt prep

Same as [Base B.3](./01-base-guide.md#b3-consolidated-prompt-prep).

### B.4 Plan mode, review, finalize

Same as [Base B.4](./01-base-guide.md#b4-plan-mode-review-finalize).

### B.5 Implementation

Same as [Base B.5](./01-base-guide.md#b5-implementation).

### B.6 Manual verification and stage

Same as [Base B.6](./01-base-guide.md#b6-manual-verification-and-stage).

### B.7 Code-quality audit

Same as [Base B.7](./01-base-guide.md#b7-code-quality-audit-same-session).

**Plus:** early greenfield features often create the project's first conventions. When the audit surfaces a convention worth keeping, add it to `.guidelines/`.

### B.8 Atomic commits

Same as [Base B.8](./01-base-guide.md#b8-atomic-commits).

### B.9 Cross-agent review

Same as [Base B.9](./01-base-guide.md#b9-cross-agent-review-review).

### B.10 Fresh-mind manual review

Same as [Base B.10](./01-base-guide.md#b10-fresh-mind-manual-review).

### B.11 Open PR via `gh` CLI

Same as [Base B.11](./01-base-guide.md#b11-open-pr-via-gh-cli).

### B.12 AI code-reviewer feedback loop

Same as [Base B.12](./01-base-guide.md#b12-ai-code-reviewer-feedback-loop).

**Plus:** set up MergeMitra, CodeRabbit, Greptile, or an equivalent reviewer before the repo starts accumulating many PRs.

---

## C. Operating principles

Same as [Base C](./01-base-guide.md#c-operating-principles-ambient-rules).

---

## Greenfield-specific reminders

- **Capture decisions as ADRs.** Every meaningful "we picked X over Y because Z" belongs in `docs/adr/NNN-title.md`.
- **Start with architecture shape, not generated code.** Let agents help you think before they help you type.
- **Refine `.guidelines/` as conventions emerge.** The first month is when the project teaches you what matters.
- **The first PR sets the tone.** Make it small, reviewed, tested, and easy to explain.
