# Enterprise (High-NFR): Agentic Coding Playbook

> [!NOTE]
> **What this is:** an extra layer on top of Greenfield, Brownfield, or Refactoring Legacy work.
>
> Use it when performance, accessibility, security, compliance, observability, sign-offs, or audit trails matter enough that "the code works" is not a complete answer.
>
> **How to read:** first open the variant that matches your project shape: [Greenfield](./02-greenfield.md), [Brownfield](./03-brownfield.md), or [Refactoring Legacy](./04-refactoring-legacy.md). Then apply the additions below.

If a feature touches payments, patient data, authentication, financial reporting, or a high-traffic production path, you need more than a passing unit test. You need to know who approved the decision, what risk was checked, how the system will be observed, and how the team will roll back if needed.

Agents can help produce the work, but humans still govern the risk.

---

## A. Pre-flight: expanded

In addition to your base variant's pre-flight, complete the **Preconditions Checklist** before the first feature.

### A.0a Preconditions checklist (one-time per project)

A feature built without this context will usually create rework later. Confirm each:

- [ ] **Business and product context:** vision, personas, workflows, domain glossary, success metrics, regulatory scope, performance expectations, security expectations
- [ ] **Architecture foundation:** high-level diagram, service boundaries, frontend and backend responsibilities, event flows, API contracts, state strategy, auth model, deployment topology, scalability plan, multi-tenancy decisions, observability approach
- [ ] **Approved technology stack:** frontend and backend frameworks, language versions, DB, cloud platform, UI component system, state libraries, API standards, infra tooling, CI/CD, testing frameworks
- [ ] **Engineering standards and governance:** `.guidelines/`, coding standards, naming, folder structure, API conventions, PR standards, testing thresholds, security policies, accessibility, logging, error handling, performance budgets, documentation expectations
- [ ] **Design system and UX constraints:** design system, component library, brand guidelines, UX patterns, accessibility standards, breakpoints, interaction and animation guidelines
- [ ] **Agent operating rules:** what agents may modify, what needs human approval, autonomy levels, refactor permissions, dependency-install rules, secret-handling rules, PR-creation policy, branching strategy
- [ ] **Repo readiness:** clean structure, README, architecture docs, setup scripts, env scripts, test harnesses, seed data, API mocks, lint setup, type enforcement
- [ ] **Project memory:** ADR directory, historical trade-offs log, known-bugs list, internal libraries doc, common pitfalls, glossary, reusable prompts, spec templates
- [ ] **Decision owners:** owners assigned for requirement clarity, architecture, risk, PR rules, trade-off decisions, and quality judgment
- [ ] **Quality and observability:** monitoring, tracing, logging, error aggregation, feature flags, rollback strategy, SLO/SLA, incident workflows, production diagnostics
- [ ] **Security and compliance:** secure coding, PII rules, secrets management, dependency scanning, vulnerability policy, audit logging, SOC2/ISO/GDPR scope, auth standards

### A.0b Definition of Done

For Enterprise work, a feature is done when all relevant evidence exists:

- [ ] Spec written or updated
- [ ] Code generated and reviewed by another agent
- [ ] Human review completed
- [ ] Unit, integration, page, and E2E tests pass where applicable
- [ ] AI reviewer comments triaged
- [ ] Security checks
- [ ] Accessibility validated through automated and manual checks where applicable
- [ ] Observability added
- [ ] Documentation updated
- [ ] Feature flag wired where applicable
- [ ] Rollout plan documented
- [ ] Rollback path documented

> [!TIP]
> Put this list into your PR template. Reviewers should see the evidence without asking for it in comments.

---

## B. The Workflow: stacked additions

### B.1 Requirement intake

**Plus:** identify non-functional requirements for the feature. Pull from the project-wide ceilings set in A.0a and write only what applies.

- **Performance:** latency, throughput, memory, or bundle size
- **Security:** authentication, authorization, PII, audit logging, threat-model touch points
- **Accessibility:** WCAG level, keyboard navigation, screen reader behavior
- **Observability:** metrics to emit, logs to add, traces to link, alerts to consider
- **Compliance:** SOC2 controls, GDPR data flows, or sector-specific rules

Acceptance criteria should include how each applicable NFR will be validated.

### B.2 Council of Agents brainstorm

**Plus:** for non-trivial decisions, include a human architecture review before implementation. Agents can propose options. A responsible human approves the direction.

Create an ADR when the decision changes architecture, data flow, security boundaries, or operational behavior.

### B.3 Consolidated prompt prep

**Plus:** the implementation prompt should reference the applicable NFRs and the Definition of Done. If the agent does not see them in the prompt, it may optimize only for functional code.

### B.4 Plan mode, review, finalize

**Plus:** the plan should include observability hooks, feature-flag handling, rollout plan, security checks, and accessibility validation where relevant.

### B.5 Implementation

**Plus:** as part of implementation, the agent should:

- Emit observability hooks at important code paths
- Wire a feature flag for the new path when needed
- Apply security checks at boundaries
- Avoid leaking sensitive data in errors or logs
- Apply accessibility basics such as ARIA roles, keyboard handlers, and focus management where relevant

### B.6 Manual verification and stage

**Plus: NFR validation pass.**

- **Performance:** run against the budget. For performance-critical features, capture a heap profile or flamegraph.
- **Accessibility:** run an automated audit and manually check keyboard navigation.
- **Security:** run SAST, secrets scanning, and dependency vulnerability checks.
- **Observability:** confirm metrics, logs, or traces appear in the expected dashboards.

### B.7 Code-quality audit

**Plus:** the audit should also check:

- Secure input handling
- Error messages that do not leak sensitive information
- PII handling and data classification
- Audit logging for privileged actions
- Authorization at boundaries

### B.8 Atomic commits

Same as [Base B.8](./01-base-guide.md#b8-atomic-commits).

### B.9 Cross-agent review

**Plus:** review NFR adherence:

- Are performance budgets respected?
- Are security boundaries respected?
- Are observability hooks present at the right points?
- Are accessibility requirements satisfied?
- Are audit logs emitted for privileged actions?

### B.10 Fresh-mind manual review

Same as [Base B.10](./01-base-guide.md#b10-fresh-mind-manual-review).

**Plus:** spot-check the evidence pack before PR. Confirm screenshots, reports, scans, dashboards, and rollout notes are present where relevant.

### B.11 Open PR via `gh` CLI

**Plus: PR readiness evidence pack.** Include applicable items in the PR description:

- Screenshots vs Figma for UI features
- Test results for unit, integration, and E2E checks
- Coverage report when the team requires it
- Heap graph or flamegraph for performance-critical features
- Security scan report, either clean or with accepted exceptions
- Accessibility audit report
- AI reviewer report or merge-readiness score
- Observability dashboard link showing new signals
- Feature flag configuration
- Rollout plan and rollback procedure

> [!TIP]
> Automate as much of this evidence as CI can produce. Leave human judgment for the items that actually need a human.

### B.12 AI code-reviewer feedback loop plus human gates

**Plus:**

- **Senior architect review:** required for changes touching architecture, security, or critical paths
- **Security review:** required for changes touching auth, PII, secrets, or external interfaces
- **Release notes:** committed to the repo when the change needs them
- **Post-merge validation:**
  - Observability dashboards confirm new metrics flow
  - On-call team knows about the new feature when alerts or runbooks change
  - Feature flag is verified in production if applicable

---

## C. Operating principles

Same as [Base C](./01-base-guide.md#c-operating-principles-ambient-rules), plus:

- **Production outcome beats code volume.** Generated code is only useful after the behavior, evidence, and rollout path are checked.
- **Sign-offs happen inside the workflow.** Architecture, security, and accessibility checks should happen before the PR is treated as ready.
- **The Definition of Done is a checklist, not decoration.** If a required line is unchecked, the work is unfinished.
- **Humans own the outcome.** Agents can do the work, but people still own requirements, architecture, risk, and quality judgment.

---

## Enterprise-specific reminders

- **Preconditions matter.** A strong workflow cannot compensate for missing product, architecture, or governance context.
- **NFRs are first-class.** They appear in the spec, plan, implementation, review, PR, and post-merge validation.
- **Evidence beats assertion.** Show the test report, screenshot, dashboard, scan, or rollout note.
- **Automate the evidence pack.** PR readiness should be a CI artifact where possible.
