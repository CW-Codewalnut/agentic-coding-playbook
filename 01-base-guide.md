# Base Guide: Agentic Coding Playbook

> [!NOTE]
> **What this is:** the default workflow for building software with coding agents.
>
> **Audience:** capable developers who already know the value of clean code, tests, reviews, and production ownership.
>
> **Scope:** reasonably sized feature work, such as adding OAuth or building a multi-step wizard. For tiny fixes, skip the steps that do not help.
>
> **How to read this playbook:** read this Base Guide once. Then open the variant that matches your work: [Greenfield](./02-greenfield.md), [Brownfield](./03-brownfield.md), [Refactoring Legacy](./04-refactoring-legacy.md), or [Enterprise](./05-enterprise.md). The variant docs follow the same outline and call out only what changes.

Before agents, your leverage came from writing the code well yourself.

With agents, your leverage comes from shaping the work well enough that another system can write useful code under your direction. That shift can feel uncomfortable at first. It can feel like the craft is being moved away from your hands.

It is not.

Your engineering foundation becomes the filter. You know when a requirement is vague. You know when an abstraction is premature. You know when a diff passes tests but still smells wrong.

The workflow below turns those instincts into a repeatable operating model:

1. Write the requirement clearly.
2. Use agents to widen the option space.
3. Consolidate the plan yourself.
4. Let an agent implement inside clear guardrails.
5. Verify manually.
6. Review with a different agent.
7. Own the final diff.

---

## Table of Contents

- [Base Guide: Agentic Coding Playbook](#base-guide-agentic-coding-playbook)
  - [Table of Contents](#table-of-contents)
  - [A. Pre-flight (one-time per project)](#a-pre-flight-one-time-per-project)
    - [A.1 Tools and environment](#a1-tools-and-environment)
    - [A.2 The agent rules file (AGENTS.md / CLAUDE.md / .guidelines)](#a2-the-agent-rules-file-agentsmd--claudemd--guidelines)
    - [A.3 Scripts contract](#a3-scripts-contract)
    - [A.4 Optional: Playwright auto-screenshots for UI work](#a4-optional-playwright-auto-screenshots-for-ui-work)
  - [B. The Workflow (per feature)](#b-the-workflow-per-feature)
    - [B.1 Requirement intake](#b1-requirement-intake)
    - [B.2 Council of Agents brainstorm](#b2-council-of-agents-brainstorm)
    - [B.3 Consolidated prompt prep](#b3-consolidated-prompt-prep)
    - [B.4 Plan mode, review, finalize](#b4-plan-mode-review-finalize)
    - [B.5 Implementation](#b5-implementation)
    - [B.6 Manual verification and stage](#b6-manual-verification-and-stage)
    - [B.7 Code-quality audit (same session)](#b7-code-quality-audit-same-session)
    - [B.8 Atomic commits](#b8-atomic-commits)
    - [B.9 Cross-agent review (`/review`)](#b9-cross-agent-review-review)
    - [B.10 Fresh-mind manual review](#b10-fresh-mind-manual-review)
    - [B.11 Open PR via `gh` CLI](#b11-open-pr-via-gh-cli)
    - [B.12 AI code-reviewer feedback loop](#b12-ai-code-reviewer-feedback-loop)
  - [C. Operating principles (ambient rules)](#c-operating-principles-ambient-rules)

## A. Pre-flight (one-time per project)

### A.1 Tools and environment

- **Coding agents:** choose any two from Claude Code, Codex, and Cursor. Use one to implement and another for second-pass review and opinions.
- **Editor:** use VS Code or any IDE for reading code, checking navigation, and reviewing diffs. If Cursor is one of your chosen agents, treat it as an agent surface.
- **Single source of truth for changes:** Git. The diff viewer tells you what changed. The agent's summary is only a summary.
- **Skills and reusable prompts:** see [Resources: Agent Skills](./06-resources.md#agent-skills).

> [!NOTE]
> **Why two agent surfaces?** Agents miss different things. One may write a clean implementation and still miss an edge case. Another, coming in fresh, is more likely to question the behavior.

### A.2 The agent rules file (AGENTS.md / CLAUDE.md / .guidelines)

The rules file teaches the agent how your repo works. Without it, every session starts from a cold read of the codebase, and the agent may invent patterns that do not belong.

- **`AGENTS.md`** at repo root: the main rule file for agents that support it.
- **`CLAUDE.md`** can be a symlink to `AGENTS.md`: `ln -s AGENTS.md CLAUDE.md`
- **`.guidelines/`** directory: your team's coding standards split by language or framework, such as `typescript.md`, `react.md`, `python.md`.

What goes in `AGENTS.md`:

- 3 to 4 line project overview
- Available scripts: format, check or lint, type-check, test, e2e, package management
- Folder structure and file conventions
- Naming conventions and the team rules agents most often miss
- Final line: "Always follow conventions in `.guidelines/`"

> [!TIP]
> Keep the overview concise. The agent reads it often. Long background text consumes context without helping the next edit.

### A.3 Scripts contract

Give the agent a clear list of commands it can run to check its own work. Put those commands in `AGENTS.md`.

| Script type  | Examples            |
| ------------ | ------------------- |
| Format       | `bun run format`    |
| Lint / check | `bun run check`     |
| Type-check   | `bun run typecheck` |
| Test         | `bun run test`      |
| E2E          | `bun run e2e`       |

The practical rule: if a script exists and is listed in `AGENTS.md`, most agents run it before handing work back. The goal is simple. You do not want to discover basic lint, type, or test failures during your own review. Let the agent run them and fix the errors by itself before finishing its work.

### A.4 Optional: Playwright auto-screenshots for UI work

Add this line to `AGENTS.md`:

```text
When making UI changes, use Playwright CLI to open the changed screen, take screenshots, and verify layout and behavior.
```

With this in place, UI work gets a basic visual check before you review it. Screenshots do not replace manual testing, but they catch obvious layout failures early.

---

## B. The Workflow (per feature)

### B.1 Requirement intake

- Open a separate plain-text editor, not the agent's input field.
- Write the requirement in plain language.
- Capture acceptance criteria.
- Capture edge cases and nuances you already know.
- Make every requirement verifiable. If you cannot test it, it is not ready.

> [!IMPORTANT]
> **Why a separate editor?** You re-read your own prompt. You notice missing context. You catch ambiguity before it becomes code. This short pause saves a lot tokens and time.

> [!TIP]
> Use the Grill Me skill during requirement intake. Ask the agent to interview you before planning, using its structured ask question tool. Most modern coding agents in plan mode can ask structured questions instead of sending a loose list of questions in chat. That gives you a cleaner decision record and makes it easier to confirm or change requirements before code exists.
>
> ```text
> Relentlessly ask me questions so that we are in sync with each other with respect to the requirements.
> ```

**Outcome:** a plain-text requirement with acceptance criteria and known edge cases.

### B.2 Council of Agents brainstorm

For complex tasks, run the same brainstorm in separate fresh sessions. Use your two chosen agents from Claude Code, Codex, and Cursor. For harder problems, use three sessions by running one of the tools twice with a fresh context.

In each session, stay in planning mode and use a prompt that:

- Gives the requirement, acceptance criteria, and known edge cases
- States your high-level approach, or says that you need options
- Asks the agent to research source-backed approaches when the topic benefits from it
- Asks for 2 to 3 approaches with trade-offs
- Asks the agent to question unclear requirements before planning

After the sessions complete:

- Pick one approach, or combine the strongest parts of multiple approaches.
- Ask at least one session: _"Now identify edge cases, nuances, and constraints for the chosen approach."_
- Consolidate the answers yourself. The repeated points are useful. The non-overlapping points are often where bugs hide.

> [!IMPORTANT]
> **Why multiple sessions?** Same prompt, different harnesses, different blind spots. You are using variation to get broader coverage before implementation begins.

> [!TIP]
> If you do not know the implementation space well for the given task in hand, say so. Try: _"I have only a rough idea of how to implement this: <vague_idea>. Lay out options, rank them, and tell me what I should learn before choosing."_

**Outcome:** a chosen approach with trade-offs, edge cases, and constraints written down.

### B.3 Consolidated prompt prep

- Return to your text editor.
- Combine the requirement, acceptance criteria, edge cases, chosen approach, and constraints from B.2.
- Decide whether to split the work into smaller chunks. If yes, define the chunks before you paste the prompt.

> [!IMPORTANT]
> **On chunk size:** Modern agents can handle long tasks, but a vague large task still produces vague large diffs. Split work when the blast radius would be hard to review.

> [!TIP]
> Re-read the consolidated prompt twice. Missing context costs more after implementation starts.

**Outcome:** one implementation prompt, or an ordered list of prompts, ready to paste.

### B.4 Plan mode, review, finalize

- Submit the consolidated prompt in plan mode.
- Answer any remaining questions from the agent.
- Read the plan end-to-end.
- Reply with notes: what to change, clarify, or remove.
- Let the agent produce a final plan.
- Start implementation only after the plan matches your intent.

**Outcome:** an approved plan that you have actually read.

### B.5 Implementation

Let the agent work inside the plan. The session should:

- Implement the approved plan
- Run the checks listed in `AGENTS.md`
- Fix issues surfaced by those checks
- Run a Playwright pass for UI work if you set up A.4

> [!TIP]
> **Caffeinate on macOS:** Long runs can be interrupted by sleep. Wrap the agent: `caffeinate -i claude`. If the process is already running, attach to its PID: `caffeinate -i -w <PID>`.

**Outcome:** code is written, automated checks pass, and screenshots exist for UI work.

### B.6 Manual verification and stage

- Verify the requirement yourself.
- For UI, open the running app and exercise the feature. Click through the flow.
- If you have an external E2E suite, run it.
- Stage the files. Do not commit yet.

> [!TIP]
> Pre-AI, you manually tested before committing. Keep that habit. Agents replace typing, not testing.

**Outcome:** staged changes that you have verified against the requirement.

### B.7 Code-quality audit (same session)

- Stay in the implementation session.
- Prompt: _"I have staged all your changes. Audit the staged diff for code quality and consistency with the rest of the codebase. Check naming, folder structure, conventions, and the rules in `AGENTS.md` and `.guidelines/`."_
- Have the agent fix valid issues.
- Do a quick pass yourself for naming, folder placement, file size, and obvious smells.

> [!NOTE]
> This is not the deep functional review. It is a polish pass while the implementation context is still loaded.

**Outcome:** staged diff matches team conventions.

### B.8 Atomic commits

- Open a fresh session with a medium model or a medium reasoning setting. This is diff organization work, not deep design work.
- Prompt: _"Strategically split, stage and commit the changes in the repo, use conventional commits. First take a snapshot of the current diff, then once you apply all the commits, pls check against this to make sure all the changes are kept intact without missing out."_

**Outcome:** clean atomic commits on your feature branch.

### B.9 Cross-agent review (`/review`)

Treat cross-agent review as a second opinion from a fresh reviewer. The goal is to use LLM variance in your favor: different models and harnesses tend to question different assumptions.

- If Claude Code implemented, review with Codex or Cursor.
- If Codex implemented, review with Claude Code or Cursor.
- If Cursor implemented, review with Claude Code or Codex.
- Fresh session, different agent.
- Prompt: _"/review this branch against [base-branch]. Check for regressions, breakages, intent mismatch, functional issues, performance issues, and system design concerns. Is it ready to merge and deploy or not. Feature overview: [one-paragraph overview]."_
- You triage the findings. Decide what to fix, defer, or ignore.
- In the same review session, ask it to fix the selected issues.
- After fixes, re-run B.7, stage, and commit.
- Run the review again in a fresh session. It should come back clean, or close enough that you understand the remaining risk.

> [!TIP]
> The review prompt does not need every requirement detail. Give a clear overview and constraints. The reviewer can read the diff.

**Outcome:** functional issues are caught by a different agent and fixed before PR.

### B.10 Fresh-mind manual review

After cross-agent review and commits, sleep on it if the change is meaningful. Open the diff later with a fresh mind.

- Review the full feature flow, including the surrounding code.
- Walk the flow from entry point to outcome.
- Scan adjacent functions, callers, and shared modules.
- Make sure you can explain the PR without asking the agent again.
- If you find material changes, hand the notes to your coding agent, then re-run B.7, B.8, and a quick cross-agent review.

> [!IMPORTANT]
> **Why this step?** You own the code in production. Reviewers, on-call engineers, and incident readers will assume you understand what shipped.

**Outcome:** you have personally read the change and the surrounding code. You can explain the PR.

### B.11 Open PR via `gh` CLI

- A medium model or medium reasoning setting is enough for this step. The task is mostly reading the diff and filling the PR template clearly.
- Prompt: _"Open a PR for this branch against [base-branch]. Use the team PR title format and description template. The feature is: [one-paragraph overview]. Read the changes and fill in the template. Use `gh` CLI."_
- If the change set is large, ask the agent to suggest a PR split first. Approve the split before it creates PRs.

**Outcome:** PR is open with a clear title, description, and template fields filled.

### B.12 AI code-reviewer feedback loop

External AI reviewers such as MergeMitra, Greptile, CodeRabbit, or similar tools may post comments on the PR.

- Open a coding-agent session.
- Prompt: _"Read the PR comments using `gh` CLI. Validate each comment against the actual code. Fix the valid ones, and draft a reply for any you disagree with. Push the fixes to the same branch."_
- Review the replies to disputed comments before pushing.

**Outcome:** PR addresses valid external reviewer feedback and is ready for human review.

---

## C. Operating principles (ambient rules)

These are not phases. They apply across the workflow.

- **Draft prompts in a separate text editor.** Re-read before sending.
- **Use multiple agent sessions for non-trivial planning.** Two different agents are enough for most work. Use a third session when the problem is unfamiliar or high risk.
- **Same session for code-quality audit, different agent for functional review.** They catch different classes of problems.
- **Pick model strength based on judgment needed.** Use your strongest reasoning model for planning, implementation, review, audits, and AI-reviewer fixes. Use a medium model or medium reasoning setting for PR descriptions, atomic commits, and mechanical searches.
- **`AGENTS.md` handles repeated instructions.** Checks, screenshots, and conventions should be written down once.
- **Compact long conversations.** Long sessions collect stale context. Use `/compact` before tricky follow-up work.
- **Git is the ground truth.** Verify changes through the diff.
- **Test data quality decides app quality** in data-driven apps. Use realistic synthetic data with edge cases, not only happy-path fixtures.
- **Manual verification stays with you.** Always exercise the feature before opening a PR.

---

> [!NOTE]
> **Scope reminder:** This guide is the default, not a law. Skip steps that do not add value for a tiny change. Do not skip judgment.
