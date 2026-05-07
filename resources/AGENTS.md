> **Opinionated `AGENTS.md` template for TypeScript projects.**
> Copy this to your repo root, fill in the `_[…]_` placeholders, and trim sections that don't apply to your stack.
> Sections **without** placeholders (Code Style, Available Scripts, Tech Stack → Global) are sane defaults for a Bun + TypeScript + Biome + Zod stack — keep, edit, or replace as needed.
> See [Base §A.2](../01-base-guide.md#a2-the-agent-rules-file-agentsmd--claudemd--guidelines) for the full setup notes.

## Product Overview

_[1-paragraph description: what this project is, who it's for, and the core value prop. Keep it tight — this loads into every agent session.]_

### How It Works

_[Numbered list (3–5 steps) describing the high-level flow: trigger → processing → output. Example shape:]_

1. _[A trigger occurs — a request, a webhook, a cron]_
2. _[The system runs pre-checks / validation gates]_
3. _[The core pipeline executes its stages — list them]_
4. _[Results stream back / are persisted / are surfaced to the user]_

### What It Does

_[Bulleted list of major capabilities or domains this project covers. One bullet per capability.]_

- _[Capability 1 — one-line description]_
- _[Capability 2 — one-line description]_
- _[Capability 3 — one-line description]_

## Project Structure

```
_[your-project]_/
├── apps/                          # delete if not a monorepo
│   └── _[app-name]_/              # _[what this app does]_
├── packages/                      # delete if not a monorepo
│   └── _[package-name]_/          # _[the package's role]_
└── scripts/                       # Repo-level scripts (setup, etc.)
```

_[For monorepos, add per-package subsections like the ones below describing each package's responsibilities and submodules. For single-package projects, replace with a `src/` layout and explain key directories.]_

### `_[packages/example-package]_` — _[Short description]_

_[Bullet the key submodules and what each one owns. Keep paths concrete — agents grep these.]_

- **`_[lib/foo/]_`** — _[what this submodule does]_
- **`_[lib/bar/]_`** — _[what this submodule does]_
- **`_[workflows/baz/]_`** — _[what this workflow orchestrates]_

## Tech Stack

### Global

- **Runtime & Package Manager**: Bun
- **Language**: TypeScript (strict mode)
- **Monorepo**: Turborepo with Bun workspaces
- **Linting/Formatting**: Biome
- **Validation**: Zod

### `_[apps/example-app]_` — _[Role]_

- **Framework**: _[Hono / Express / Next.js / …]_
- **_[Other key library]_**: _[purpose]_
- **_[Integration]_**: _[purpose]_

### `_[packages/example-package]_` — _[Role]_

- **_[Library / pattern]_**: _[purpose]_

## Available Scripts

- `bun run dev` - Start all apps in development mode
- `bun run dev:web` - Start only the web app
- `bun run dev:server` - Start only the api server
- `bun run check` - Lint & Format all packages
- `bun run check-types` - Typecheck all packages
- `bun run fun` - Lint, Format & Typecheck all packages
- `bun test` - Run unit & integration tests
- `bun run e2e` - Run end-to-end tests
- `bun run db:push` - Push schema changes to the database
- `bun run db:generate` - Generate SQL migration files
- `bun run db:migrate` - Run pending migrations
- `bun run db:studio` - Open Drizzle Studio GUI

## Code Style

### TypeScript

- Use `type` over `interface`
- Never use `any`
- Explicit return types for exported functions
- Use Zod for external data validation

### Naming

| Element   | Convention       | Example                 |
| --------- | ---------------- | ----------------------- |
| Files     | kebab-case       | `get-user-profile.ts`   |
| Functions | camelCase        | `getUserProfile`        |
| Variables | camelCase        | `userContext`           |
| Constants | UPPER_SNAKE_CASE | `DEFAULT_CONFIG`        |
| Types     | PascalCase       | `UserConfig`            |

When adding dependencies, use `bun add <pkg>` inside the appropriate package directory.

### Code Quality Principles

Follow **SOLID Principles** and **Clean Code** practices for all functions:

- **Single Responsibility**: Each function does one thing well
- **Small Functions**: Aim for 5-30 lines per function
- **Descriptive Names**: Function names should explain what they do without needing comments
- **No Magic Values**: Extract constants, no hardcoded strings/numbers
- **Explicit over Implicit**: Prefer clarity over cleverness

## Environment Variables

_[Document required env vars in a table. Refer to `.env.example` if you keep one. Note any cloud-only vs self-hosted differences.]_

| Variable          | Required | Default | Purpose                                |
| ----------------- | -------- | ------- | -------------------------------------- |
| _[`VAR_NAME`]_    | Yes      | —       | _[What it controls / where it's used]_ |
| _[`OTHER_VAR`]_   | No       | _[…]_   | _[Purpose]_                            |

## Best Practices for This Codebase

1. **Explore Before Implementing**: Study existing patterns before adding new ones.
   - _[Pointer to a representative directory: e.g. orchestration logic at `path/to/dir/`]_
   - _[Pointer to another representative pattern]_
2. **Consistency**: Follow established patterns for similar operations.
3. **Modularity**: Keep functions small and single-purpose.

_[Project-specific dos and don'ts. Common ones:]_

- _[Database migrations: who runs them, who generates them, never edit generated files by hand]_
- _[Network access: when to use which client — e.g. `gh` cli over raw API for GitHub to avoid rate limits]_
- _[Secret handling, deployment gates, branch protection rules]_

Always follow the conventions in `.guidelines/`.
