> **Starter `AGENTS.md` template for TypeScript projects.**
> Copy this to your repo root, fill in the `_[...]_` placeholders, and trim sections that do not apply to your stack.
> Sections without placeholders are defaults for a modern Bun + TS + Drizzle stack. Keep, edit, or replace them as needed.
> See [Base A.2](../01-base-guide.md#a2-the-agent-rules-file-agentsmd-claudemd-guidelines) for setup notes.

## Product Overview

_[1-paragraph description: what this project is, who uses it, and what job it performs. Keep it concise.]_

### How It Works

_[Numbered list, 3 to 5 steps, describing the high-level flow: trigger, processing, output. Example shape:]_

1. _[A trigger occurs: a request, webhook, or cron]_
2. _[The system runs pre-checks or validation gates]_
3. _[The core pipeline executes its stages. List them]_
4. _[Results stream back, are persisted, or are surfaced to the user]_

### What It Does

_[Bulleted list of major capabilities or domains this project covers. One bullet per capability.]_

- _[Capability 1: one-line description]_
- _[Capability 2: one-line description]_
- _[Capability 3: one-line description]_

## Project Structure

```text
_[your-project]_/
├── apps/                          # delete if not a monorepo
│   └── _[app-name]_/              # _[what this app does]_
├── packages/                      # delete if not a monorepo
│   └── _[package-name]_/          # _[the package's role]_
└── scripts/                       # repo-level scripts
```

_[For monorepos, add per-package subsections like the ones below describing each package's responsibilities and submodules. For single-package projects, replace with a `src/` layout and explain key directories.]_

### `_[packages/example-package]_`: _[Short description]_

_[Bullet the key submodules and what each one owns. Keep paths concrete so agents can search for them.]_

- **`_[lib/foo/]_`**: _[what this submodule does]_
- **`_[lib/bar/]_`**: _[what this submodule does]_
- **`_[workflows/baz/]_`**: _[what this workflow orchestrates]_

## Current vs Target Conventions

Use this section when a brownfield codebase is in transition.

```text
This codebase is in transition. Follow the target conventions below for new work.
Do not copy legacy patterns unless this file explicitly says they are still preferred.
```

- **Current patterns to preserve:** _[Public APIs, schemas, routes, logs, metrics, filenames, or other contracts that must not change]_
- **Target patterns to follow:** _[Architecture, folder structure, naming, error handling, styling, testing, and validation patterns new work should move toward]_
- **Legacy patterns to avoid:** _[Old patterns present in the repo that agents should not copy into new code]_

## Tech Stack

### Global

- **Runtime and package manager:** Bun
- **Language:** TypeScript in strict mode
- **Monorepo:** Turborepo with Bun workspaces
- **Linting and formatting:** Biome
- **Validation:** Zod

### `_[apps/example-app]_`: _[Role]_

- **Framework:** _[Hono / Express / Next.js / etc.]_
- **_[Other key library]_**: _[purpose]_
- **_[Integration]_**: _[purpose]_

### `_[packages/example-package]_`: _[Role]_

- **_[Library or pattern]_**: _[purpose]_

## Available Scripts

- `bun run dev`: Start all apps in development mode
- `bun run dev:web`: Start only the web app
- `bun run dev:server`: Start only the API server
- `bun run check`: Lint and format all packages
- `bun run check-types`: Typecheck all packages
- `bun run fun`: Lint, format, and typecheck all packages
- `bun test`: Run unit and integration tests
- `bun run e2e`: Run end-to-end tests
- `bun run db:push`: Push schema changes to the database
- `bun run db:generate`: Generate SQL migration files
- `bun run db:migrate`: Run pending migrations
- `bun run db:studio`: Open Drizzle Studio GUI

## Code Style

### TypeScript

- Use `type` over `interface`
- Never use `any`
- Add explicit return types for exported functions
- Use Zod for external data validation

### Naming

| Element   | Convention       | Example               |
| --------- | ---------------- | --------------------- |
| Files     | kebab-case       | `get-user-profile.ts` |
| Functions | camelCase        | `getUserProfile`      |
| Variables | camelCase        | `userContext`         |
| Constants | UPPER_SNAKE_CASE | `DEFAULT_CONFIG`      |
| Types     | PascalCase       | `UserConfig`          |

When adding dependencies, use `bun add <pkg>` inside the appropriate package directory.

### Checkable Code Quality Rules

Use rules the agent can verify in a diff:

- Keep exported functions typed.
- Validate external input with Zod.
- Split a function when a branch handles a separate concern.
- Name constants for reused strings and numbers.
- Add tests for edge cases and error paths.

## Environment Variables

_[Document required env vars in a table. Refer to `.env.example` if you keep one]_

| Variable        | Required | Default | Purpose                             |
| --------------- | -------- | ------- | ----------------------------------- |
| _[`VAR_NAME`]_  | Yes      | None    | _[What it controls and where used]_ |
| _[`OTHER_VAR`]_ | No       | _[...]_ | _[Purpose]_                         |

## Working Rules for This Codebase

1. **Explore before implementing:** read the relevant modules before adding new code.
   - _[Pointer to a representative directory, such as orchestration logic at `path/to/dir/`]_
   - _[Pointer to another representative pattern]_
2. **Current vs target:** before following an existing pattern, check whether this file marks it as current, target, or legacy.
3. **Modularity:** keep changed code easy to review and avoid growing large mixed-purpose modules.

_[Project-specific dos and don'ts. Common ones:]_

- _[Database migrations: who runs them, who generates them, and whether generated files may be edited by hand]_
- _[Network access: when to use which client, such as `gh` CLI over raw API for GitHub to avoid rate limits]_
- _[Secret handling, deployment gates, branch protection rules]_

Always follow the conventions in `.guidelines/`.
