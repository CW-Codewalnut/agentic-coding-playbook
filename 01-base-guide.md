# Base Guide — Agentic Coding Playbook

> **The default workflow for coding with AI agents.** Read end-to-end once. After that, treat as quick-reference.
>
> **Audience:** Developers with 1+ year of experience.
>
> **Scope:** Reasonably-sized feature work — anything from "add OAuth" to "build a multi-step wizard." For one-line bug fixes or trivial tasks, use judgment and skip phases.
>
> **How to read this playbook:** This Base Guide is your default. The four variant docs — [Greenfield](./02-greenfield.md), [Brownfield](./03-brownfield.md), [Refactoring Legacy](./04-refactoring-legacy.md), [Enterprise](./05-enterprise.md) — follow the same outline. Under each phase, variants either say `→ Same as Base §N.` (one line, no change) or list `Plus` / `Instead` deltas. Open the variant doc matching your project shape, follow the deltas where present, otherwise follow this Base. Reusable snippets, model picks, and tool refs live in [Resources](./06-resources.md).
>
> **This is opinionated.** It's one team's workflow, refined across real projects — not the One True Way. Steal what works.

---

## A. Pre-flight (one-time per project)

### A.1 Tools & environment

- **Coding agents:** Claude Code + Codex. Both are used — see _Council of Agents_ (B.2) and _Cross-agent review_ (B.9). Run each in your terminal of choice or its desktop app — pick one surface per agent and stick to it.
- **Editor:** VS Code (or any IDE) with **all AI features off** — no Copilot, no in-editor agents, no autocomplete. Use the editor for reading code and viewing diffs only.
- **Single source of truth for changes:** Git. The diff viewer is your ground truth for what an agent changed.
- **Skills:** see [Resources → Agent Skills](./06-resources.md#agent-skills).

> **Why no AI in the editor?** Pick one surface for AI work — terminal or desktop app — and stay there. Editor AI fragments your attention and pollutes the diff with un-tracked tweaks. The editor's job is managing code and git, nothing more.

### A.2 The agent rules file (AGENTS.md / CLAUDE.md / .guidelines)

The most important setup step. Pays back across every session.

- **`AGENTS.md`** at repo root — single source of truth for agent rules.
- **`CLAUDE.md`** is a symlink to `AGENTS.md`: `ln -s AGENTS.md CLAUDE.md`
- **`.guidelines/`** directory — your team's coding standards split by language/framework (e.g. `typescript.md`, `react.md`, `python.md`, `db.md`).

What goes in `AGENTS.md`:

- 3–4 line project overview
- Available scripts (format, check/lint, type-check, test, e2e, package management)
- Folder structure and file conventions
- Naming conventions (kebab-case files, camelCase exports, PascalCase components — your team's choice)
- Final line: "Always follow conventions in `.guidelines/`"

> **Pro Tip:** Keep the `AGENTS.md` overview short. It's loaded into every agent session — long overviews burn context budget without payback.

> **Reference templates:** Opinionated TS-stack scaffolds for `AGENTS.md` and `.guidelines/` live in [`resources/`](./resources/) — copy them into your repo and fill the placeholders. See [Resources → Reference templates](./06-resources.md#reference-templates-agentsmd--guidelines).

### A.3 Scripts contract

Every agent turn auto-runs your formatter, linter, type-checker, tests, and e2e suite — _if_ the scripts are listed in `AGENTS.md` by name and runnable from where the agent is running.

| Script type  | Examples            |
| ------------ | ------------------- |
| Format       | `bun run format`    |
| Lint / check | `bun run check`     |
| Type-check   | `bun run typecheck` |
| Test         | `bun run test`      |
| E2E          | `bun run e2e`       |

**The contract:** if the script exists and is referenced in `AGENTS.md`, the agent runs it at the end of every turn and auto-fixes anything it surfaces. You don't need to ask manually.

### A.4 Optional: Playwright auto-screenshots for UI work

Add this line to `AGENTS.md`:

> When making UI changes, spin up the dev server (if not running already), then use the Playwright CLI to navigate, take screenshots, and verify both layout and behavior.

With this in place + the Playwright CLI skill installed, _every_ UI turn ends with verification automatically. No need to mention it in the prompt.

---

## B. The Workflow (per feature)

### B.1 Requirement intake

- Open a separate plain-text editor (notepad, sublime, even a fresh editor window — _not_ the agent composer).
- Write the requirement in plain language.
- Capture **acceptance criteria**
- Capture **edge cases and nuances** you already know.
- Make every requirement **verifiable** — if you can't test it, it's not a requirement.

> **Why a separate editor?** You re-read your prompts. You add context you forgot. You catch ambiguity. The 5–10 minutes spent here cuts an hour of post-implementation correction. **Single biggest leverage habit in the playbook.**

**Outcome:** A plain-text requirement file with acceptance criteria + known edge cases.

### B.2 Council of Agents brainstorm

Run the same brainstorm in **three separate fresh sessions** — typically 2× Claude + 1× Codex (or vice-versa). Scale up or down with problem complexity.

In each session, in **plan mode**, use a prompt that:

- Lays down the requirement + acceptance criteria + known edge cases
- States your high-level approach (or says "I have no clue, propose options")
- Asks the agent to **research industry best practices** (web search) for this class of problem
- Asks for **2–3 approaches with trade-offs**
- Explicitly tells the agent: _"Use the AskUserQuestion tool to ask me questions. Use the `grill-me` skill."_

After all three sessions complete:

- Pick one approach (or a hybrid).
- Send a follow-up in the _same_ session: _"Now identify edge cases, nuances, and constraints for the chosen approach."_
- Consolidate answers from all three sessions. Most overlap; the non-overlap is gold.

> **Why three sessions?** LLMs are probabilistic. Same prompt, different sessions, different ideas. You're turning non-determinism — usually a flaw — into wider coverage. Hence: _Council of Agents._

> **Pro Tip:** If the problem is genuinely unknown to you, say so explicitly: _"I have only a vague idea of how to implement this. Lay down options and rank them."_ The agent calibrates better when it knows your knowledge level.

**Outcome:** A consolidated set of trade-off-ranked approaches + edge cases + constraints. Your chosen approach, written down.

### B.3 Consolidated prompt prep

- Back to your text editor.
- Combine: requirement + acceptance criteria + edge cases + chosen approach + nuances/constraints from B.2.
- **Manually** decide whether to split the work into smaller chunks. If yes, define the chunks.

> **On chunk size:** Modern agents handle long-running tasks well, with auto-compaction. Don't over-split. But don't ask for "build the whole platform" in one turn either. **Your intuition is the splitter — not the AI's.**

> **Pro Tip:** Re-read the consolidated prompt twice before sending. The cost of re-reading is minutes; the cost of missing context is hours.

**Outcome:** A single prompt (or ordered list of prompts, one per chunk) ready to paste.

### B.4 Plan mode → review → finalize

- Submit the consolidated prompt in **plan mode**.
- Most questions should be pre-answered from B.2; the agent may still ask a few — answer them.
- Read the produced plan **thoroughly**. Don't skim.
- Reply with notes: what to change, what to clarify, what to remove.
- Let the agent produce the final plan.
- Then: start implementation.

**Outcome:** An approved plan that you've read end-to-end and corrected.

### B.5 Implementation

- Let it work. The session will:
  - Implement the plan
  - Auto-run lint, type-check, tests, e2e (because they're in `AGENTS.md`)
  - Auto-fix anything the checks surface
  - Run a Playwright pass if you set up A.4

> **Pro Tip — Caffeinate (macOS):** Long agent runs get killed by sleep settings. Wrap the agent: `caffeinate -i claude`. Already running and you forgot? Attach to its PID: `caffeinate -i -w <PID>` or for a fixed timer (e.g. 2 hours = 7200s): `caffeinate -i -t 7200`. Stash an alias in your shell profile so you never forget.

**Outcome:** Code is written, all automated checks pass, screenshots exist for UI work.

### B.6 Manual verification & stage

- Verify the requirements yourself — does it actually do what you asked?
- For UI: open the running app, exercise the feature manually. Even with screenshots, click around.
- If you have an external E2E suite, run it.
- **Stage the files. Do not commit yet.**

> **Pro Tip:** Pre-AI, you'd manually test before committing. That habit doesn't go away — agents replace typing, not testing.

**Outcome:** Staged changes that you've verified meet the requirements.

### B.7 Code-quality audit (same session)

- Stay in the implementation session.
- Prompt: _"I have staged all your changes. Audit the staged diff for code quality and consistency with the rest of the codebase. Check naming, folder structure, conventions, and the rules in `AGENTS.md` & `.guidelines/`."_
- The agent returns a list of nits. Have it fix them.
- Do a quick at-a-glance review yourself — naming, folder placement, file size, obvious smell. Don't deep-dive.

> **Why not deep-dive yourself?** Functional review happens in B.9 with a different agent. This step is just code-quality polish.

**Outcome:** Clean staged diff matching team conventions.

### B.8 Atomic commits

- Open a fresh session at the **Standard tier** ([what's Standard?](./06-resources.md#model-picks-per-task)).
- Prompt: _"strategically & meaningfully split, stage and commit the changes in the repo, use conventional commits"_

**Outcome:** Clean atomic commits on your feature branch.

### B.9 Cross-agent review (`/review`)

The high-leverage review step. **The agent that did the implementation does NOT review its own work.**

- If implementation was Claude → review with Codex (use Codex `/review`).
- If implementation was Codex → review with Claude (use Claude `/review`).
- Fresh session, opposite agent.
- Prompt: _"/review this branch against [base-branch], check for regressions, breakages, intent mismatch, functional issues, perf issues, system design improvements etc. [high-level-overview-req]"_
- The review returns a prioritized list of issues.
- **You** triage — which to fix, which to defer, which to ignore.
- In the same session: _"Fix issues #1, #3, #5."_
- After fixes: re-run B.7 (code-quality audit on new changes), stage, commit (B.8).
- **Re-run `/review` with the same prompt** in a fresh session — should come back clean (or close to).

> **Pro Tip:** The review prompt doesn't need full requirement detail — the code itself communicates intent. Give a high-level overview + constraints only.

**Outcome:** Functional issues caught and fixed; review re-run is clean.

### B.10 Fresh-mind manual review

After cross-agent review and commits, **sleep on it**. Open the diff the next morning with fresh eyes. This is a pre-AI habit — still essential.

- Review the feature end-to-end, not just the diff. Walk the flow from entry point to outcome, scan adjacent functions, callers, and related modules. The diff is just where the change landed — your job is to understand what the feature actually _does_ in the system. Same way you did reviews pre-AI.
- Aim for fingertip familiarity with your own code. Pre-AI, wake you at 3am with a stack trace and you'd land on the offending line from memory — logs in, line of code out. That bar hasn't moved. Agents write the code; _you_ still own it. If you can't answer a question about your own feature without re-opening the file, you haven't really reviewed it.
- The point isn't catching what the agents missed — they probably caught more than you will. The point is _you_ can **explain the PR** — explain decisions, justify trade-offs, answer reviewer pushback, reason about on-call implications. Knowing what shipped is just the floor.
- Anything material to change: hand notes to your coding agent — _"Apply these changes: [list]"_ — then re-run §B.7 (audit), §B.8 (commits), and a quick `/review` re-run.

> **Why this step?** (1) You'll own this code in production — reviewers, on-call, post-mortem readers all assume you understand it. (2) Tired-you misses things fresh-you catches. Sleeping on a feature is the cheapest pair of glasses you'll buy.

**Outcome:** You've personally read the change and the surrounding code. You can explain the PR.

### B.11 Open PR via `gh` CLI

- **Standard tier** is fine here ([tier reference](./06-resources.md#model-picks-per-task)).
- Prompt: _"Open a PR for this branch against [base-branch]. Use the team PR title format and description template. The feature is: [one-paragraph overview]. Understand the changes yourself and fill in the template. Use `gh` cli."_
- If the change set is genuinely large: _"Split the work across multiple PRs along meaningful boundaries before opening. Suggest the split first; I'll approve before you create the PRs."_

**Outcome:** PR(s) open with proper title, description, and template fields filled.

### B.12 AI code-reviewer feedback loop

External AI reviewer (MergeMitra / Greptile / CodeRabbit / similar) posts comments on the PR.

- Open a coding-agent session.
- Prompt: _"Read the PR comments using `gh` cli. Validate each comment against the actual code. Fix the valid ones, leave a reply explaining any you disagree with. Push the fixes to the same branch."_
- Verify the agent's responses to disputed comments before pushing.

**Outcome:** PR addresses external reviewer feedback; ready for human review.

---

## C. Operating principles (ambient rules)

These aren't phases — they apply across the whole workflow.

- **Always draft prompts in a separate text editor.** Re-read before sending. Single biggest leverage habit.
- **Council of Agents = 2× Claude + 1× Codex** in fresh sessions for any non-trivial brainstorm. Exploit LLM probabilism.
- **Same session for code-quality audit; opposite agent for functional review.** Different lenses, different sessions. Don't conflate.
- **Right tier for the task.** A "tier" is model + reasoning effort, not just model size. See [Resources → Model picks](./06-resources.md#model-picks-per-task) for the legend and full mapping. Quick version:
  - _Planning, implementation, review, audits, AI-reviewer fix loop:_ **Heavy**
  - _PR creation, Atomic commits, mechanical greps:_ **Standard**
- **AGENTS.md does the boring enforcement.** Lint, type-check, test, e2e, screenshots — all auto. Don't ask manually.
- **Compact the conversation regularly.** Long sessions accumulate stale context. `/compact` periodically — especially before a tricky follow-up turn.
- **Git is the source of truth, not the agent's claims.** Always verify changes via the diff viewer.
- **Test data quality decides app quality** in data-driven apps. Spend time generating realistic synthetic data with edge cases — not just happy-path fixtures.
- **Manual verification doesn't go away.** Agents replace typing, not testing. Always exercise the feature yourself before opening a PR.

---

> **Scope reminder:** This Base Guide is for reasonably-sized feature work. For one-line bug fixes, hot-fixes, or trivial tasks: skip phases that don't add value. The framework is the default, not the law.
