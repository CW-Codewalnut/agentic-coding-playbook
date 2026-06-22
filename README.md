# Agentic Coding Playbook

You already know how to build software.

You can take a rough requirement, ask better questions, split risk, read a diff, write tests, debug a production issue, and explain a trade-off to another engineer. Agentic coding does not make those skills obsolete. It makes them matter more.

We had the same first reaction many good engineers have: if an agent writes the code, what is left of the craft? What we found was less dramatic. The craft moves into the setup and review: a crisp requirement, the right files in context, a small plan, a diff you can defend, and tests that check behavior users will notice.

This repo is the workflow we wish we had on day one. It will not teach you software engineering from scratch. It will help a capable engineer adopt agentic coding faster, especially if your current company has not yet given you the tools or time to learn it deeply. Treat it as a focused weekend learning path that can save weeks of trial and error.

## Table of Contents

- [Agentic Coding Playbook](#agentic-coding-playbook)
  - [Table of Contents](#table-of-contents)
  - [Who it's for](#who-its-for)
  - [Scope](#scope)
  - [What's in here](#whats-in-here)
  - [How to read](#how-to-read)
  - [What you'll need](#what-youll-need)
  - [Conventions](#conventions)
  - [The core idea](#the-core-idea)
  - [Credits](#credits)
  - [License](#license)

## Who it's for

- **Good engineers learning agentic coding** who already care about code quality, testing, maintainability, and production behavior.
- **Developers whose teams do not have proper Agentic tools yet** who want a practical way to learn the workflow on their own time.
- **Architects and tech leads** who need a repeatable way to guide agent-assisted work across a team.

## Scope

Reasonably sized feature work. For tiny fixes, use judgment and skip certain steps that may not need them.

## What's in here

- [README.md](./README.md): start here.
- [01-base-guide.md](./01-base-guide.md): the default workflow.
- [02-greenfield.md](./02-greenfield.md): brand-new projects.
- [03-brownfield.md](./03-brownfield.md): existing codebases.
- [04-refactoring-legacy.md](./04-refactoring-legacy.md): behavior-preserving refactors.
- [05-enterprise.md](./05-enterprise.md): high-NFR and regulated work.
- [06-resources.md](./06-resources.md): model guidance, tool links, and skill links.
- [07-token-economics.md](./07-token-economics.md): cut token (and dollar) costs without losing quality.
- [resources/](./resources/): TypeScript-stack templates you can copy into your own repo and adapt.

## How to read

1. Read the **[Base Guide](./01-base-guide.md)** end-to-end once. It explains the default workflow.
2. Open the variant that matches your project:
   - **[Greenfield](./02-greenfield.md)**: brand-new project, no existing code.
   - **[Brownfield](./03-brownfield.md)**: adding features to an existing codebase.
   - **[Refactoring Legacy](./04-refactoring-legacy.md)**: improving structure without changing behavior.
   - **[Enterprise (High-NFR)](./05-enterprise.md)**: extra checks for performance, accessibility, compliance, security, observability, and audit trails.
3. Keep **[Resources](./06-resources.md)** open for tool links, model guidance, and reusable skills.

## What you'll need

- **Two coding agents:** choose any two from Claude Code, Codex, and Cursor. Use one to implement and the other for a second-pass review. The value is the second opinion: different models and harnesses notice different risks.
- **Editor:** VS Code or any IDE you like. Use it for reading code and reviewing diffs.
- **Team conventions:** `.guidelines/`, `AGENTS.md`, PR format, test commands, and release rules. In this repo, the reference `.guidelines` files capture our usual JS, TS, and React rules.

## Conventions

- The guide stays practical and scannable, but it explains why each step exists.
- Tips and rationale notes appear as GitHub alerts (`[!TIP]`, `[!IMPORTANT]`, `[!NOTE]`).
- Prompt snippets appear as fenced code.
- Stable section labels (`A.1`, `B.7`, etc.) let the variant docs reference the Base Guide.
- Model and reasoning guidance lives in [Resources](./06-resources.md#model-picks-per-task).

## The core idea

> [!IMPORTANT]
> Agents replace typing. They do not replace engineering judgment.
>
> Your job moves up a level: clarify the requirement, teach the agent the codebase, choose the shape of the solution, review the diff, verify behavior, and own the outcome in production.

## Credits

Published by **CodeWalnut**. Issues and PRs welcome.

## License

[MIT](./LICENSE) © CodeWalnut.
