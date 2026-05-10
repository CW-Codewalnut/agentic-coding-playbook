# Agentic Coding Playbook

An **opinionated** practitioner's playbook for shipping software with AI coding agents. Bulleted, scannable, decisive — not a tutorial, not a survey of options.

> **Fork it. Hack it. Adopt it.** This repo is a living scaffold, not a finished artifact. Fork it, swap the placeholders for your team's links, snippets, and conventions, and trim what doesn't apply. Disagree with a step? Replace it. The structure is the contribution; the specifics are negotiable.

## Who it's for

- **Developers already using AI coding agents** who want to sharpen their workflow and stop fighting non-determinism.
- **Architects and tech leads** structuring agent-assisted work for a team.

## Scope

Reasonably-sized feature work — anything from "add OAuth" to "build a multi-step wizard." Not for one-line bug fixes (just do them) and not for org-wide architecture programs (different problem).

## What's in here

```
.
├── README.md
├── LICENSE
├── 01-base-guide.md              ← start here
├── 02-greenfield.md              ← variant: brand-new project
├── 03-brownfield.md              ← variant: existing codebase
├── 04-refactoring-legacy.md      ← variant: behavior-preserving refactor
├── 05-enterprise.md              ← multiplier: high-NFR / regulated
├── 06-resources.md               ← model picks, tool refs, skill links
└── resources/                    ← opinionated TS-stack templates — copy into your repo and fill placeholders
    ├── AGENTS.md
    └── .guidelines/
        ├── standard.md
        ├── javascript.md
        ├── typescript.md
        └── react.md
```

## How to read

1. Read the **[Base Guide](./01-base-guide.md)** end-to-end once. It's the default workflow.
2. Open the **variant doc** matching your project shape:
   - **[Greenfield](./02-greenfield.md)** — brand-new project, no existing code.
   - **[Brownfield](./03-brownfield.md)** — adding features to an existing codebase.
   - **[Refactoring Legacy](./04-refactoring-legacy.md)** — improving structure without changing behavior.
   - **[Enterprise (High-NFR)](./05-enterprise.md)** — stacks on top of the variant above when you have hard non-functional requirements (perf, a11y, compliance, observability).
3. Keep **[Resources](./06-resources.md)** open for model tier picks, tool refs, and skill links.

## What you'll need

- **Coding agents:** Claude Code + Codex (terminal or desktop).
- **Standard tooling:** Git, the `gh` CLI, your team's lint / typecheck / test scripts.
- **Optional but recommended:** Playwright skill for UI work — see [Base §A.4](./01-base-guide.md#a4-optional-playwright-auto-screenshots-for-ui-work) and [Resources → Agent Skills](./06-resources.md#agent-skills).

## Conventions

- **Bulleted, imperative, scannable** — designed to be re-read, not read once.
- **Pro Tips** appear as block quotes; **prompt snippets** appear as fenced code.
- **Stable section IDs** (`§A.1`, `§B.7`, …) — variants reference them.
- **Tier vocabulary** (`Heavy` / `Standard` / `Light`) for model picks — defined in [Resources → Model picks](./06-resources.md#model-picks-per-task).

## The spirit

> **Outcome engineering, not output engineering.** "Code merged" is not the goal — "system observably reliable in production" is. Every phase in the playbook serves that.

## Credits

Published by **CodeWalnut**. Issues and PRs welcome.

## License

[MIT](./LICENSE) © CodeWalnut.
