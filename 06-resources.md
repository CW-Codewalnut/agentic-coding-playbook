# Resources — Agentic Coding Playbook

> [!NOTE]
> **What this is:** Reference material for the workflow — skill links, tool refs, model picks. Cross-referenced from the [Base Guide](./01-base-guide.md) and the four variant docs.
>
> **How to use:** This is a living scaffold. Fork the repo, fill the placeholders with your team's snippets, links, and conventions, and keep it high-signal as your stack evolves.

---

## Reference templates: AGENTS.md & .guidelines

Opinionated templates for a TypeScript / Bun / Biome / Zod stack live in [`resources/`](./resources/) at the repo root. Copy them into your project and fill the `_[…]_` placeholders:

- [`resources/AGENTS.md`](./resources/AGENTS.md) — drop at your repo root. Project-related sections (overview, structure, env vars) are placeholders; conventions, scripts, and global tech stack are sane defaults. See [Base §A.2](./01-base-guide.md#a2-the-agent-rules-file-agentsmd--claudemd--guidelines) for the setup notes.
- [`resources/.guidelines/`](./resources/.guidelines/) — drop at your repo root. Ships with stub files for `standard.md`, `javascript.md`, `typescript.md`, and `react.md` (front matter only). Add per-framework files as you grow (e.g. `python.md`, `db.md`, `commits.md`, `pr-template.md`).

> [!TIP]
> Don't try to fill these in one sitting. Start with what you know; let the conventions accumulate as your agent sessions surface them.

---

## Tools & services

### PR Reviewer Tools

- [MergeMitra](https://mergemitra.com/)
- [CodeRabbit](https://coderabbit.ai/)
- [Greptile](https://greptile.com/)

### Coding agents

- **[Claude Code](https://claude.com/claude-code)**
- **[Codex](https://openai.com/codex/)**

### Editor

- **VS Code** — AI features off, used for reading, modifying code, and viewing diffs.

### Terminal

- [Ghostty](http://ghostty.org/)

---

## Model picks per task

> [!NOTE]
> **Legend.** Three tiers:
>
> - **Heavy** — Claude Opus 4.5+ high; GPT-5.4+ xhigh.
> - **Standard** — Claude Sonnet 4.6 medium; GPT-5.4+ medium.
> - **Light** — Claude Haiku 4.5; GPT-5.4-mini.
>
> Refresh the examples as new models ship — the **tier mapping for tasks below stays stable.**

| Task                              | Tier     | Why                                                            |
| --------------------------------- | -------- | -------------------------------------------------------------- |
| Architecture brainstorm           | Heavy    | Reasoning depth shapes the whole project                       |
| Council of Agents                 | Heavy    | Quality of options matters more than speed                     |
| Plan mode                         | Heavy    | Bad plan = bad implementation                                  |
| Implementation                    | Heavy    | Code quality is the deliverable                                |
| Code-quality audit (same session) | Heavy    | Reuses implementation session — context loaded                 |
| Cross-agent `/review`             | Heavy    | Use the _opposite_ agent; catch what was missed                |
| PR creation                       | Standard | Template filling — structured, not novel                       |
| AI reviewer fix loop              | Heavy    | Validations and targeted edits responding to specific comments |
| Atomic commit splitter            | Standard | Pattern-match work over a known diff                           |

---

## Agent Skills

> [!NOTE]
> Skills installed across sessions. Each link points to [skills.sh](https://skills.sh/).

- **[Superpowers](https://skills.sh/obra/superpowers)** — meta-skill bundle: brainstorming, writing-plans, debugging, and more.
- **[Frontend Design](https://skills.sh/anthropics/skills/frontend-design)** — UI / frontend implementation guidance.
- **[Playwright CLI](https://skills.sh/microsoft/playwright-cli/playwright-cli)** — browser automation and UI verification.
- **[PR Document Writer](https://skills.sh/cw-codewalnut/agent-skills/pr-document-writer)** — generates Enterprise grade PR titles and descriptions.
- **[Awesome Copilot](https://skills.sh/github/awesome-copilot)** — curated collection of Agent skills and assets.
- **[Grill Me](https://skills.sh/mattpocock/skills/grill-me)** — relentless interviewing skill that stress-tests plans and designs through systematic questioning.
