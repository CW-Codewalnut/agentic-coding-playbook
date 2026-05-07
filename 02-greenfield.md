# Greenfield — Agentic Coding Playbook

> **What this is:** Brand-new project, no existing code. You're free to pick everything: framework, runtime, database, deploy target.
>
> **How to read:** Outline mirrors the [Base Guide](./01-base-guide.md). Under each phase, you'll see either `→ Same as Base §N.` or `Plus` / `Instead` deltas. Where there's no delta, follow Base.

---

## A. Pre-flight

### A.0 Architecture brainstorm — NEW phase, do first

Before any code or AGENTS.md exists, decide the architecture _shape_.

- Open plain-text editor. Write: product vision (3 lines), expected user load, core domain entities, must-have constraints (e.g. "must run on a single VPS", "team only knows TypeScript").
- Run **Council of Agents** brainstorm (2× Claude + 1× Codex, fresh sessions). Each session researches industry best practices and proposes options for:
  - Frontend / backend / full-stack frameworks
  - Runtime (Bun, Node, Python, Go)
  - Database (Postgres, MySQL, SQLite, managed services)
  - Auth model
  - Deployment topology (monolith, microservices, serverless)
  - Observability stack
- Consolidate. Pick. Write **ADR-001** capturing the decision and rationale.

> **Pro Tip:** Don't decide framework versions or specific libraries here — pick those when you actually need them. Decide _shape_ only.

> **Pro Tip:** When in doubt, pick boring. The agent's "innovative" tech-stack suggestion is usually wrong for production.

**Outcome:** ADR-001 in `docs/adr/` describing the architecture shape. You know what to put in `AGENTS.md` next.

### A.1 Tools & environment

→ Same as Base §A.1.

### A.2 The agent rules file

**Plus:** You're bootstrapping from scratch. Use the reference templates (see [Resources → Reference templates](./06-resources.md#reference-templates-agentsmd--guidelines)) as a starting point and adapt:

- Copy your reference `.guidelines/` directory wholesale.
- Trim files for languages/frameworks not in your stack.
- `AGENTS.md` overview = 3–4 lines describing the new project. Scripts get filled in as you set them up.
- Don't over-write `AGENTS.md` early — start minimal, grow as conventions emerge.

### A.3 Scripts contract

→ Same as Base §A.3. Set these up _first_ — your AGENTS.md is mostly empty until they exist.

### A.4 Optional: Playwright auto-screenshots

→ Same as Base §A.4. If you have a UI, set this up before the first feature.

---

## B. The Workflow

### B.1 Requirement intake

→ Same as Base §B.1.

**Plus:** On greenfield, you have no existing patterns to compare against — be extra explicit about success criteria. Ambiguity costs more here because there's nothing to anchor to.

### B.2 Council of Agents brainstorm

→ Same as Base §B.2.

**Plus:** Since there's no existing code constraining choices, ask the council to research industry best practices more aggressively. Phrasing: _"I have full freedom on implementation. Research how teams typically solve this in [stack] in 2025. Compare 3 approaches."_

### B.3 Consolidated prompt prep

→ Same as Base §B.3.

### B.4 Plan mode → review → finalize

→ Same as Base §B.4.

### B.5 Implementation

→ Same as Base §B.5.

### B.6 Manual verification & stage

→ Same as Base §B.6.

### B.7 Code-quality audit

→ Same as Base §B.7.

**Plus:** On early greenfield features, the convention bar _is_ whatever ends up in your `.guidelines/`. Expect the audit to surface "should this be a convention?" questions. Capture them in `.guidelines/` as you go.

### B.8 Atomic commits

→ Same as Base §B.8.

### B.9 Cross-agent review

→ Same as Base §B.9.

### B.10 Fresh-mind manual review

→ Same as Base §B.10.

### B.11 Open PR via `gh` CLI

→ Same as Base §B.11.

**Plus:** On greenfield, the first few PRs may bootstrap your PR template and review process — refine as you go. Don't perfect the template before the first PR.

### B.12 AI code-reviewer feedback loop

→ Same as Base §B.12.

**Plus:** Set up CodeRabbit / MergeMitra / equivalent on the repo before merging the second PR.

---

## C. Operating principles

→ Same as Base §C.

---

## Greenfield-specific reminders

- **Capture decisions as ADRs.** Every "we picked X over Y because Z" goes into `docs/adr/NNN-title.md`. Future-you and future-team will thank you.
- **Don't over-design upfront.** Decide architecture shape, not every library. Leave room to discover.
- **Refine `.guidelines/` as conventions emerge.** It's a living artifact in the first month. After that it stabilizes.
- **First PR sets the tone.** Subsequent contributors (human or agent) mimic the first PR. Make it a good one.
