# Resources: Agentic Coding Playbook

> [!NOTE]
> **What this is:** reference material for the workflow: templates, tool links, model guidance, and skills.
>
> **How to use:** copy what helps, replace placeholders with your team's conventions and preferences.

---

## Reference templates: AGENTS.md and .guidelines

- [`resources/AGENTS.md`](./resources/AGENTS.md): drop at your repo root. Edit the placeholders and your team conventions. See [Base A.2](./01-base-guide.md#a2-the-agent-rules-file-agentsmd-claudemd-guidelines).
- [`resources/.guidelines/`](./resources/.guidelines/): drop at your repo root. It includes our current team rules for `standard.md`, `javascript.md`, `typescript.md`, and `react.md`. Add files as your stack grows, such as `python.md`, `postgres.md`.

> [!TIP]
> Do not try to finish every rule in one sitting. Start with what you know. Add rules when real agent sessions reveal repeat mistakes.

---

## Tools and services

### Coding agents

Choose any two as your regular pair. Use one to plan and implement and the other for a second-pass review/critique. The point is independent models and harnesses.

- **[Claude Code](https://claude.com/claude-code)**
- **[Codex](https://openai.com/codex/)**
- **[Cursor](https://cursor.com/)**

### PR reviewer tools

- [MergeMitra](https://mergemitra.com/)
- [CodeRabbit](https://coderabbit.ai/)
- [Greptile](https://greptile.com/)

### Visual and style feedback tools

- [StyleProof](https://www.npmjs.com/package/styleproof).
- [Playwright CLI](https://playwright.dev/agent-cli/introduction)
- [Agent Browser](https://agent-browser.dev/)

---

## Model picks per task

> [!NOTE]
> Model names change. Keep the table concrete anyway, then refresh it when your tools change.

| Task                              | Good default                                      | Why                                        |
| --------------------------------- | ------------------------------------------------- | ------------------------------------------ |
| Architecture brainstorm           | Claude Opus 4.5 high or GPT-5.5 xhigh             | Reasoning depth shapes the whole project   |
| Council of Agents                 | Claude Opus 4.5 high plus GPT-5.5 xhigh           | Different models expose different options  |
| Plan mode                         | Claude Opus 4.5 high or GPT-5.5 xhigh             | Bad plan creates bad implementation        |
| Implementation                    | Claude Opus 4.5 high, GPT-5.5 high/xhigh, or Cursor with its strongest coding model | Code quality is the deliverable |
| Code-quality audit (same session) | Same model and session used for implementation    | Reuses implementation context              |
| Cross-agent `/review`             | Fresh reviewer on the other surface: Claude Opus 4.5 high, GPT-5.5 xhigh, or Cursor review | Fresh reasoning catches missed behavior |
| PR creation                       | Claude Sonnet 4.6 medium or GPT-5.4 medium        | Template filling is structured work        |
| AI reviewer fix loop              | Claude Opus 4.5 high or GPT-5.5 high/xhigh        | Requires validation against real code      |
| Atomic commit splitter            | Claude Sonnet 4.6 medium or GPT-5.4 medium        | Works over a known diff                    |

Use Claude Haiku 4.5 or GPT-5.4-mini only for mechanical searches, quick summaries, or low-risk cleanup. Do not use them for architecture, behavior review, or risky refactors.

---

## Agent Skills

> [!NOTE]
> Skills can save repeated prompting. Each link points to [skills.sh](https://skills.sh/).

- **[Superpowers](https://skills.sh/obra/superpowers):** meta-skill bundle for brainstorming, planning, debugging, and related workflows.
- **[Frontend Design](https://skills.sh/anthropics/skills/frontend-design):** UI and frontend implementation guidance.
- **[Playwright CLI](https://skills.sh/microsoft/playwright-cli/playwright-cli):** browser automation and UI verification.
- **[PR Document Writer](https://skills.sh/cw-codewalnut/agent-skills/pr-document-writer):** generates Enterprise-grade PR titles and descriptions.
- **[Awesome Copilot](https://skills.sh/github/awesome-copilot):** curated collection of agent skills and assets.
- **[Grill Me](https://skills.sh/mattpocock/skills/grill-me):** questioning skill that stress-tests plans and designs.
