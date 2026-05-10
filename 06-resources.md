# Resources — Agentic Coding Playbook

> **What this is:** Reference material for the workflow — prompt snippets, skill links, tool refs, model picks. Cross-referenced from the [Base Guide](./01-base-guide.md) and the four variant docs.
>
> **How to use:** This is a living scaffold. Fork the repo, fill the placeholders with your team's snippets, links, and conventions, and keep it high-signal as your stack evolves.

---

## Reference templates: AGENTS.md & .guidelines

Opinionated templates for a TypeScript / Bun / Biome / Zod stack live in [`resources/`](./resources/) at the repo root. Copy them into your project and fill the `_[…]_` placeholders:

- [`resources/AGENTS.md`](./resources/AGENTS.md) — drop at your repo root. Project-related sections (overview, structure, env vars) are placeholders; conventions, scripts, and global tech stack are sane defaults. See [Base §A.2](./01-base-guide.md#a2-the-agent-rules-file-agentsmd--claudemd--guidelines) for the setup notes.
- [`resources/.guidelines/`](./resources/.guidelines/) — drop at your repo root. Ships with stub files for `standard.md`, `javascript.md`, `typescript.md`, and `react.md` (front matter only). Add per-framework files as you grow (e.g. `python.md`, `db.md`, `commits.md`, `pr-template.md`).

> **Pro Tip:** Don't try to fill these in one sitting. Start with what you know; let the conventions accumulate as your agent sessions surface them.

---

## Tools & services

### AI code reviewers

- MergeMitra
- CodeRabbit
- Greptile

### Coding agents

- **Claude Code** — terminal
- **Codex** — terminal + desktop app

### Editor

- **VS Code** — AI features off, used for reading, modifying code and viewing diffs

### Terminal

- Ghostty

---

## Model picks per task

> **Legend.** Three tiers:
>
> - **Heavy** — Claude Opus 4.5+ high; GPT-5.4+ xhigh.
> - **Standard** — Claude Sonnet 4.6 medium; GPT-5.4+ medium.
> - **Light** — GPT-5.4-mini medium.
>
> Refresh the examples as new models ship — the **tier mapping for tasks below stays stable.**

| Task                              | Tier     | Why                                             |
| --------------------------------- | -------- | ----------------------------------------------- |
| Architecture brainstorm           | Heavy    | Reasoning depth shapes the whole project        |
| Council of Agents                 | Heavy    | Quality of options matters more than speed      |
| Plan mode                         | Heavy    | Bad plan = bad implementation                   |
| Implementation                    | Heavy    | Code quality is the deliverable                 |
| Code-quality audit (same session) | Heavy    | Reuses implementation session — context loaded  |
| Cross-agent `/review`             | Heavy    | Use the _opposite_ agent; catch what was missed |
| PR creation                       | Standard | Template filling — structured, not novel        |
| AI reviewer fix loop              | Standard | Targeted edits responding to specific comments  |
| Atomic commit splitter            | Light    | Pattern-match work over a known diff            |

---

## Agent Skills

> _Skills installed across sessions. Each link points to [skills.sh](https://skills.sh/)._

- **[Superpowers](https://skills.sh/obra/superpowers)** by `obra` — meta-skill bundle: brainstorming, writing-plans, debugging, and more.
- **[Frontend Design](https://skills.sh/anthropics/skills/frontend-design)** by Anthropic — UI / frontend implementation guidance.
- **[Playwright CLI](https://skills.sh/microsoft/playwright-cli/playwright-cli)** by Microsoft — browser automation and UI verification.
- **[PR Document Writer](https://skills.sh/cw-codewalnut/agent-skills/pr-document-writer)** by CodeWalnut — generates PR titles and descriptions to a template.
- **[Awesome Copilot](https://skills.sh/github/awesome-copilot)** by GitHub — curated collection of Agent skills and assets.
- **[Grill Me](https://skills.sh/mattpocock/skills/grill-me)** by mattpocock — relentless interviewing skill that stress-tests plans and designs through systematic questioning.
