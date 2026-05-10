# Enterprise (High-NFR) — Agentic Coding Playbook

> **What this is:** A _multiplier_ on top of one of the other variants — Greenfield, Brownfield, or Refactoring Legacy. Use when:
>
> - The system has hard non-functional requirements (perf budgets, accessibility, compliance, observability)
> - The team is large enough that conventions, sign-offs, and audit trails matter
> - Production downtime has real cost (regulated industries, user-facing critical paths)
>
> **How to read:** First open the variant matching your project shape — [Greenfield](./02-greenfield.md), [Brownfield](./03-brownfield.md), or [Refactoring Legacy](./04-refactoring-legacy.md). Then apply the additions below. **This doc does not replace those — it stacks on top.**
>
> _Scope: reasonably-sized feature work (see Base intro)._

---

## A. Pre-flight — EXPANDED

In addition to your base variant's pre-flight, complete the **Preconditions Checklist** before the first feature.

### A.0a Preconditions checklist (one-time per project)

A feature implementation built on missing preconditions amplifies the gaps. Confirm each:

- [ ] **Business & product context** — vision, personas, workflows, domain glossary, success metrics, regulatory/compliance scope, perf expectations, security expectations
- [ ] **Architecture foundation** — high-level diagram, service boundaries, FE/BE responsibilities, event flows, API contracts, state strategy, auth model, deployment topology, scalability plan, multi-tenancy decisions, observability approach
- [ ] **Approved technology stack** — frontend / backend frameworks, language versions, DB, cloud platform, UI component system, state libs, API standards, infra tooling, CI/CD, testing frameworks
- [ ] **Engineering standards & governance** — coding standards, naming, folder structure, API conventions, PR standards, testing thresholds, security policies, accessibility, logging, error handling, perf budgets, doc expectations
- [ ] **Design system & UX constraints** — design system, component library, brand guidelines, UX patterns, a11y standards, breakpoints, interaction & animation guidelines
- [ ] **Agent operating rules** — what agents may modify, what needs human approval, autonomy levels, refactor permissions, dependency-install rules, secret-handling rules, PR-creation policy, branching strategy
- [ ] **Repo readiness** — clean structure, README, architecture docs, setup scripts, env scripts, test harnesses, seed data, API mocks, lint setup, type enforcement
- [ ] **Knowledge context layer** — ADR directory, historical trade-offs log, known-bugs list, internal libraries doc, common pitfalls, glossary, reusable prompts, spec templates
- [ ] **Human governance** — owners assigned for: requirement clarity, architecture, risk, PR governance, trade-off decisions, quality judgment
- [ ] **Quality & observability** — monitoring, tracing, logging, error aggregation, feature flags, rollback strategy, SLO/SLA, incident workflows, prod diagnostics
- [ ] **Security & compliance** — secure coding, PII rules, secrets management, dependency scanning, vulnerability policy, audit logging, SOC2/ISO/GDPR scope, auth standards
- [ ] **Definition of Done** — see §A.0b

### A.0b Definition of Done

For Enterprise work, "code exists" ≠ "done." A feature is done when _all_ of:

- [ ] Spec written / updated
- [ ] Code generated and reviewed (cross-agent + human)
- [ ] Unit, integration, page, and E2E tests pass
- [ ] PR reviewed (AI reviewer + human)
- [ ] Security scanned (SAST / dependency / secrets)
- [ ] Accessibility validated (axe / pa11y / manual)
- [ ] Observability hooks added (metrics, logs, traces)
- [ ] Documentation generated / updated
- [ ] Release notes prepared
- [ ] Feature flag wired (if applicable)
- [ ] Rollout plan documented
- [ ] Deployable to production

> **Pro Tip:** Bake this list into your repo's PR template as checkboxes. Reviewers see what hasn't been ticked.

---

## B. The Workflow — STACKED ADDITIONS

### B.1 Requirement intake

**Plus:** NFR identification per feature. Pull from the project-wide ceilings set in A.0a; pick what applies and assign specific budgets. For each feature, list applicable NFRs:

- **Perf budget** (latency, throughput, memory, bundle size)
- **Security** (auth, authz, PII, audit logging, threat-model touch points)
- **Accessibility** (WCAG level, keyboard nav, screen reader)
- **Observability** (metrics to emit, logs to add, alerts to wire)
- **Compliance** (SOC2 controls, GDPR data flows, sector-specific rules)

Acceptance criteria must explicitly include NFR validation.

### B.2 Council of Agents brainstorm

**Plus:** Architecture review with senior architect (human). For non-trivial decisions, an ADR is created and signed off _before_ implementation. Council of Agents proposes; human architect approves.

### B.3 Consolidated prompt prep

**Plus:** Spec must reference applicable NFRs and the Definition of Done. Make the connection explicit so the implementing agent doesn't drop them.

### B.4 Plan mode → review → finalize

**Plus:** The plan must include observability hooks, feature-flag plan, rollout plan, security checks, and a11y validation as line items. **If they're not in the plan, they won't be in the code.**

### B.5 Implementation

**Plus:** As part of the implementation turn, the agent must:

- Emit observability hooks at key code paths
- Wire a feature flag for the new path
- Apply security checks (input validation, output encoding, authz at every boundary)
- Apply accessibility (ARIA roles, keyboard handlers, focus management)

### B.6 Manual verification & stage

**Plus: NFR validation pass.**

- **Perf:** run against perf budget. For perf-critical features, capture a heap profile / flamegraph.
- **A11y:** automated audit (axe-core / pa11y) + manual keyboard nav.
- **Security:** SAST scan; secrets scanner; dependency vulnerability scan.
- **Observability:** confirm metrics / logs / traces appear in your dashboards.

### B.7 Code-quality audit

**Plus:** The audit must additionally check:

- Secure coding standards (input handling, error messages don't leak info)
- PII handling (data classification, encryption at rest, access logging)
- Audit logging (every privileged action emits an audit record)
- Authorization at every boundary (no implicit trust)

### B.8 Atomic commits

→ Same as Base §B.8.

### B.9 Cross-agent review

**Plus:** Review must additionally check NFR adherence:

- Perf budgets respected?
- Security boundaries respected?
- Observability hooks present at the right points?
- A11y requirements satisfied?
- Audit logs emitted for privileged actions?

### B.10 Fresh-mind manual review

→ Same as Base §B.10.

**Plus:** Catch NFR gaps now — easier to fix pre-PR than post. Spot-check the forthcoming evidence pack: are the metrics flowing, are the screenshots captured, are the scans clean? Anything missing goes on the morning fix list.

### B.11 Open PR via `gh` CLI

**Plus: PR readiness evidence pack.** Every Enterprise PR description includes:

- Screenshots vs Figma (UI features)
- Test results (unit + integration + E2E pass counts)
- Coverage report (new lines covered ≥ team threshold)
- Heap memory graph / flamegraph (perf-critical features)
- Security scan report (clean or accepted exceptions)
- A11y audit report
- AI reviewer report / merge-readiness score
- Observability dashboard link (proving the new metrics / logs are flowing)
- Feature flag config
- Rollout plan + rollback procedure

> **Pro Tip:** This is what "merge-readiness" looks like in real enterprise teams. It's tedious. Automate as much of the evidence pack as your CI can produce — leave only the human-judgment items for the PR author.

### B.12 AI code-reviewer feedback loop + Human gates

**Plus:**

- **Senior architect review** — gate before merge for any change touching architecture, security, or critical paths
- **Security review** — gate for changes touching auth, PII, secrets, or external interfaces
- **Release notes** committed to the repo as part of the PR
- **Post-merge validation:**
  - Observability dashboards confirm new metrics flow
  - On-call team is aware of the new feature (alert tuning if needed)
  - Feature flag verified in production (off by default; toggle for canary)

---

## C. Operating principles

→ Same as Base §C, plus:

- **Outcome engineering, not output engineering.** "Code merged" is not the goal — "system observably reliable in production" is.
- **Sign-offs are part of the workflow, not friction on top.** Architect approval, security review, accessibility validation are checkpoints, not bureaucracy.
- **The Definition of Done is a contract.** Treat its line items as code. If a line is unchecked, the work is unfinished — same as a failing test.
- **Humans govern outcomes; agents do work.** The human role shifts from typing code to governing requirements, architecture, risk, and quality.

---

## Enterprise-specific reminders

- **Preconditions matter more than the workflow.** A great workflow built on missing preconditions amplifies the gaps. Get §A.0a right before anything else.
- **NFRs are first-class.** They appear in the spec, the plan, the implementation, the review, the PR, and post-merge validation.
- **Evidence beats assertion.** "It works" is not a status — show the test report, the screenshot, the dashboard, the heap graph.
- **Automate the evidence pack.** PR readiness should be a CI artifact, not a human checklist of copy-paste.
