# Token Economics: Agentic Coding Playbook

> [!NOTE]
> **What this is:** a practical guide to spending tokens better with coding agents like Claude Code, Codex, and Cursor.
>
> **How to use:** read it once to build the mental model, then keep the [Tool-specific knobs](#tool-specific-knobs) handy. It pairs with the [Base Guide](./01-base-guide.md) workflow and the [Resources](./06-resources.md#model-picks-per-task) model table.

Once coding agents start helping for real, token spend stops being an abstract API metric. You open more sessions, keep longer threads alive, add MCP servers, run reviews, and use sub-agents. The tool gets better, but the bill can still climb, and watching that number rise can feel like the cost of working the way you were told to.

It is not. Spending tokens well rewards the same instincts you already trust as an engineer: say what you mean, keep the workspace tidy, and do not hand the agent more than it needs for the job.

That does not mean the answer is "use fewer tokens everywhere." It means you should spend tokens where they buy judgment and remove the tokens that only add noise.

> [!IMPORTANT]
> **The one principle that ties this whole doc together:** maximize signal per token.
>
> Small costs compound. A few noisy rules, a giant log, a stale thread, or one unnecessary tool schema may look harmless alone. Repeated across turns and sessions, they add up.
>
> The goal is not austerity. Spend tokens on architecture thinking, behavior maps, refactor safety nets, and real review. Cut stale context, repeated prompts, broad file dumps, noisy tools, and verbose output.

## Table of Contents

- [Token Economics: Agentic Coding Playbook](#token-economics-agentic-coding-playbook)
  - [Table of Contents](#table-of-contents)
  - [How you actually pay](#how-you-actually-pay)
    - [Input tokens vs output tokens](#input-tokens-vs-output-tokens)
    - [You pay for the whole context, every turn](#you-pay-for-the-whole-context-every-turn)
    - [Where the tokens go](#where-the-tokens-go)
  - [The levers](#the-levers)
    - [1. Keep the always-on stuff small](#1-keep-the-always-on-stuff-small)
    - [2. Keep the conversation lean](#2-keep-the-conversation-lean)
    - [3. Feed lean inputs](#3-feed-lean-inputs)
    - [4. Right model, right effort](#4-right-model-right-effort)
    - [5. Don't break the cache](#5-dont-break-the-cache)
    - [6. Turn repeated prompts into scripts](#6-turn-repeated-prompts-into-scripts)
    - [7. Stay in the loop](#7-stay-in-the-loop)
  - [Where not to cut](#where-not-to-cut)
  - [Tool-specific knobs](#tool-specific-knobs)
  - [Further reading](#further-reading)

---

## How you actually pay

### Input tokens vs output tokens

Every request has two kinds of tokens, and they are not priced the same.

- **Input tokens** are everything you _send_: the agent's system prompt, tool definitions, MCP definitions, your rules files (`CLAUDE.md` / `AGENTS.md`), skills metadata, the whole conversation so far, every tool result, and your new message.
- **Output tokens** are everything the model _generates_: its hidden reasoning ("thinking"), the arguments for each tool call, and the final reply you read.

**Output tokens usually cost several times more than input tokens.** The exact ratio changes by model, but the pattern is stable enough to matter. Reasoning tokens are output tokens too, so a high-reasoning turn can get expensive quickly.

| Model (June 2026) | Input ($/M tokens) | Output ($/M tokens) | Output is |
| ----------------- | ------------------ | ------------------- | --------- |
| Claude Opus 4.8   | $5                 | $25                 | 5x input  |
| Claude Sonnet 4.6 | $3                 | $15                 | 5x input  |
| OpenAI GPT-5.5    | $5                 | $30                 | 6x input  |

> [!NOTE]
> Prices and model names are a June 2026 snapshot and change often. Treat the **ratios** as the durable lesson: output is the pricey part, and reasoning is also output.

### You pay for the whole context, every turn

The model answers each turn from the context the harness or provider gives it. Some APIs store conversation state server-side, and some harnesses resend the transcript from the client. Either way, prior messages and tool results still count as input when the next step is evaluated.

That means a 100K-token conversation can keep showing up in the bill or usage accounting on later turns. Built-in caching can soften the cost (see [lever 5](#5-dont-break-the-cache)), but the tokens still matter. The habit to build is simple: keep the context relevant and current.

### Where the tokens go

**Sent on every turn (input, the cheaper kind):**

- The agent's own **system prompt** (its built-in instructions). Fixed.
- **Tool definitions**: every built-in tool plus every MCP tool's full schema. Fixed until you change tools.
- Your **rules files**: `CLAUDE.md` / `AGENTS.md` and any "always-on" rules. Fixed per session.
- **Skills metadata**: the name and description of _every_ installed skill (the body loads only when used). Fixed.
- The **entire conversation so far**: your messages, the agent's replies, and every tool result. This one **grows with every turn**.
- Your **new message**.

**Generated by the model (output, the expensive kind):**

- Hidden **reasoning / thinking**.
- **Tool-call arguments** (the JSON to run a tool).
- The **visible reply**.

> [!IMPORTANT]
> Two buckets, two strategies. The fixed input at the top is paid repeatedly, so pruning it once keeps paying back. The growing input is why you start fresh or compact at natural breaks. Output is where model and effort choices matter most.

---

## The levers

### 1. Keep the always-on stuff small

This bucket is loaded in full on **every** turn. A five-minute prune pays off for the rest of the session.

- **Rules files (`CLAUDE.md` / `AGENTS.md`).** Main project rules are usually loaded up front. Keep them tight: a short project overview, the scripts, and the conventions agents most often miss. Move long reference material into skills or dedicated files the agent reads only when needed. Codex exposes `project_doc_max_bytes` if you want to enforce a budget. See [Base A.2](./01-base-guide.md#a2-the-agent-rules-file-agentsmd--claudemd--guidelines).
- **Skills.** Only each skill's name and description sit in context always; the body loads when used. But 40 installed skills still means 40 descriptions on every turn. Keep the skills you actually use and remove the rest.
- **MCP servers.** Large tool sets cost either tokens, attention, or both. Some harnesses load full schemas; others defer tool definitions until needed. Either way, too many enabled tools makes the agent worse at choosing. Audit servers, disable unused tools, and prefer a plain CLI like `gh` when it is simpler than an MCP server.
- **Ignore files.** Keep junk out of reach: build output, `node_modules`, generated files, secrets. Cursor has `.cursorignore` and `.cursorindexingignore`; most agents respect `.gitignore`.

### 2. Keep the conversation lean

The conversation is the part that grows. Left alone, it quietly becomes the biggest line on your bill.

- **One task per session.** Start fresh for unrelated work (`/clear` in Claude Code, a new chat in Cursor, a new thread in Codex). Stale context is re-billed on every later message and pulls the model's attention off-task.
- **Compact early; don't wait for auto-compact.** Auto-compaction usually happens late, when the context is already crowded. Compact at a natural break instead: after a plan is approved, after a chunk lands, or before a long review. Lower is usually better for cost and focus, as long as the summary keeps the facts that matter.
- **Make context visible.** You cannot manage what you cannot see. Configure a status line to show the context percentage (Claude Code: a custom `statusLine`, plus `/context` and `/usage`). Codex shows "% context left" in `/status`; Cursor shows a context ring with a per-category breakdown.
- **For long, unavoidable work, hand off.** Instead of dragging a bloated thread forward, write a short handoff note and start a fresh session that loads only the summary plus file paths and links, not the whole transcript. The [handoff skill](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md) does exactly this.
- **Don't reopen yesterday's giant thread cold by default.** Cache retention differs by provider and plan. Some caches expire in minutes; newer OpenAI models can use longer retention. The practical rule still holds: if the thread contains a lot of stale work, start from a short summary instead of carrying the whole transcript forward.

### 3. Feed lean inputs

Give the model exactly what it needs, in the lightest form.

- **Don't attach whole files or folders.** Attaching in the terminal or IDE usually dumps the entire file (and sometimes the whole tree) into context. Point precisely instead: name the file and line range, or let the agent search for what it needs.
- **Fetch docs as Markdown, not HTML.** A rendered docs page is mostly nav, CSS, and scripts; for an agent that is all noise that costs tokens. Ask for the Markdown: append `.md` to the URL, use a "Copy as Markdown" button, grab the raw file on GitHub, or use a site's `/llms.txt`. Cloudflare measured one page drop from ~16,000 tokens of HTML to ~3,000 of Markdown, about 80% off. (Not every site supports `.md`, so check.)
- **Compress repeated noise at the tool boundary.** If the same noisy commands show up every session, move the filtering into a wrapper, lifecycle hook, or CLI proxy. A tool like [RTK](https://www.rtk-ai.app/) can compact common command output before it reaches the agent. Treat savings numbers as directional and verify them on your own stack. Keep a path to the raw output for failures, and do not hide warnings, stack traces, changed files, or diff hunks the reviewer still needs.
- **Use retrieval before context.** For large codebases, don't start by dumping files into the chat. Give the agent a cheap way to find the right context first: `rg`, symbol search, AST-aware tools like Semgrep or ast-grep, semantic code search, an Aider-style repo map, or a code graph tool like [Graphify](https://graphify.net). Feed paths, symbols, queries, links, and small snippets; pull raw code only when the next decision needs it.

> **Takeaway:** precise inputs are cheaper inputs. A pointer costs a few tokens; a dumped file costs thousands.

### 4. Right model, right effort

This is where the expensive output tokens are won or lost.

- **Reasoning is output, and output is the costly part.** Higher "thinking" or effort settings generate more reasoning tokens at the output price. Don't reach for max effort out of habit. Match it to the task: low or medium for routine edits, high or xhigh for hard design and tricky debugging. There is no prize for over-thinking a rename.
- **Trim output verbosity, too.** Reasoning is not the only output you pay for; the visible reply and the tool-call arguments are output as well, and a coding agent does not need flowery prose. Where the tool allows it, set a lower verbosity (Codex: `model_verbosity = "low"`) and just ask for terse answers. Community skills like [ponytail](https://github.com/DietrichGebert/ponytail/tree/main/skills/ponytail) push in a related direction: it biases the agent toward the smallest solution that works (reach for the standard library or an already-installed dependency before a new abstraction, prefer a one-liner over a framework) and replies code-first, with at most a few lines of explanation. Less code and less prose both trim output now, and leave a smaller surface to read and review later. It makes no token-savings claim of its own, so treat any number you see as a starting point to measure on your own work, not a guarantee. Keep verbosity normal when you actually want a careful explanation; the skill itself refuses to simplify away input validation, error handling, security, or accessibility.
- **Match the model to the job.** Use a strong model for planning, architecture, and review; use a cheaper or faster one for mechanical work. Even when the price gap is only 1.5x to 2x, repeated routine turns add up. See the [model picks table](./06-resources.md#model-picks-per-task). Examples: Opus to plan, Sonnet or Cursor's Composer to implement; or GPT-5.5 xhigh to plan, GPT-5.4 medium to build.
- **Delegate to sub-agents, deliberately.** A sub-agent runs in its own context window, does noisy work such as search or file exploration, and returns a short summary. That can keep your main thread clean. The catch: each sub-agent pays for its own prompt and tools, and parallel agents multiply total spend. Use them when the alternative is polluting a long-lived session, not for every small task.

> **Takeaway:** spend reasoning where judgment is needed; use cheaper settings for routine work.

### 5. Don't break the cache

Coding agents already do a lot of token optimization for you: prompt caching, context reuse, compaction, summaries, retrieval, and tool-output truncation. You are usually not building the cache. You are operating the agent so those built-in savings still work.

- **What the tool already handles.** Providers and agent harnesses try to reuse stable prompt prefixes and bill cached input more cheaply when the prefix matches. OpenAI prompt caching is automatic for long prompts on recent models; Anthropic exposes cache write/read economics; Claude Code, Codex, and Cursor expose usage, context, or status surfaces. The exact rules differ by product, so do not build your daily workflow around one stale number.
- **Your job: keep stable things stable.** Cache-friendly sessions keep the boring setup steady: model, effort, rules, MCP servers, tools, skills, and reusable repo docs. Put volatile task details, logs, diffs, and one-off files after the stable setup.
- **Do not churn the setup mid-task.** Switching model or effort, editing root rules, enabling new MCP servers, changing plugins, or attaching huge docs can destroy cache locality and increase context noise. Do it only when the quality gain is worth the cost.
- **Use fresh sessions at task boundaries.** Caching and clean context pull in different directions. Inside one task, continuity can help. For a new task, stale context is usually more expensive than a cache hit is valuable.
- **Keep cacheable context clean.** Short global rules, scoped skills, small repo indexes, and filtered tool output make the stable prefix useful. Giant logs, full files, generated folders, and broad MCP catalogs make the agent pay attention to the wrong things.
- **Watch the signals.** Use `/status`, `/usage`, `/context`, statuslines, Cursor's context ring, and usage dashboards. If context is bloated or the agent is looping, compact or restart instead of hoping the cache saves it.
- **Bulk, non-interactive jobs are different.** Batch APIs can be cheaper for offline evals or large one-shot rewrites. They are not a fit for live coding loops.

> [!NOTE]
> The agent has optimizations built in. Your job is not to outsmart them; your job is to avoid churn, stale context, and noisy prefixes that stop those optimizations from paying off.

### 6. Turn repeated prompts into scripts

If you do the same deterministic, multi-step thing through the agent over and over, it should not be a prompt at all. **The cheapest LLM call is the one you don't make.** A script does the job for free, the same way every time, with no reasoning tokens and no chance of drift.

Think of a cost ladder, and push work down it whenever you can:

| Where it lives            | Token cost                       |
| ------------------------- | -------------------------------- |
| `CLAUDE.md` / `AGENTS.md` | Paid on **every** session        |
| A skill or slash command  | Loads only **when used**         |
| A hook or plain script    | Runs with **~zero** model tokens |

Let the agent help: ask it to review your recent sessions, spot the workflows you repeat, and turn the deterministic ones into a script, slash command, or hook. Not everything fits, but the ones that do stop costing tokens forever.

### 7. Stay in the loop

Your review is still the best cost control.

- **Draft prompts in a text editor; read twice.** A vague prompt sends the agent down the wrong path, and you pay for the wrong attempt _plus_ the correction, each turn replaying the whole context. Two minutes of editing beats three rounds of fixing. You notice missing context and catch ambiguity before it becomes code. This is already [Base B.1](./01-base-guide.md#b1-requirement-intake); it is also a cost lever.
- **Watch the agent, and codify what you learn.** When it repeats a mistake, such as a GitHub 403, a wrong test command, or a missing convention, do not only fix it in chat. Write the correction into `AGENTS.md` / `CLAUDE.md`, a skill, or a hook so the same issue does not cost another session. This is the same habit as [Base C](./01-base-guide.md#c-operating-principles-ambient-rules): repeated instructions belong in the rules file.

---

## Where not to cut

Token economy is not austerity. Spend tokens when the extra context lowers real engineering risk.

- **Greenfield architecture.** Use strong models and enough context when choosing architecture, interfaces, ADRs, auth, data shape, and deployment direction. A cheap plan can create expensive rework.
- **Brownfield readiness.** Do not shrink the behavior map just to save tokens. Existing behavior, adjacent flows, logs, jobs, contracts, and validation gates are the work.
- **Refactoring safety.** Spend tokens on behavior specs, characterization tests, and review. A smaller refactor prompt that misses behavior is not cheaper.
- **Security, compliance, and production risk.** Use the context needed to check auth, PII, audit logging, rollout, rollback, and observability.
- **Second-opinion review.** A fresh reviewer costs tokens, but it can catch mistakes the implementation session normalized.

Cut low-signal context. Keep the context that lets the agent make a better decision.

---

## Tool-specific knobs

The same levers, with the concrete controls for each tool. Names and defaults are a June 2026 snapshot; verify against current docs.

**Claude Code**

- _See your usage:_ `/context`, `/usage`, and a custom `statusLine` showing context %.
- _Trim context:_ `/compact [focus]`, `/clear`, a lean `CLAUDE.md`, hide skills with `disable-model-invocation` / `skillOverrides`, audit servers with `/mcp`, and `deny` rules for noisy directories.
- _Model & effort:_ `/model` (e.g. `opusplan` to plan on Opus and build on Sonnet), `/effort`, and turn off extended thinking for routine work (`Alt+T`, or `MAX_THINKING_TOKENS=0`).
- _Output limits:_ use hooks or command-output proxies where available to filter verbose command output before it enters the next turn.
- _Caching & resuming:_ automatic, but retention depends on plan and surface. If an old thread is stale, start fresh from a `/compact` summary or a handoff note rather than resuming the full history. Avoid mid-session model / effort / tool churn.

**Codex**

- _Config (`config.toml`):_ `model` (default `gpt-5.5`), `model_reasoning_effort` (default `medium`, set `low` or `minimal` for routine work), `model_reasoning_summary = "none"`, `model_verbosity = "low"`.
- _Rules:_ keep `AGENTS.md` tight; Codex exposes `project_doc_max_bytes` if you want to enforce a project-doc budget.
- _MCP:_ use `enabled_tools` / `disabled_tools` per server. Prefer deferred discovery where available, and avoid exposing broad servers for one small job.
- _Caching & context:_ keep model and effort stable inside a task. Codex exposes `tool_output_token_limit`; read files surgically (`sed -n`, `grep`) rather than `cat`-ing huge files. `/status` shows context left; prefer fresh threads or compaction at natural breaks.

**Cursor**

- _Cost routing:_ default to **Auto** or **Composer** for routine work; reserve frontier models and **Max Mode** for genuinely hard tasks. Avoid "Fast mode" unless latency-critical.
- _Sessions:_ new chat per task (history is re-billed every turn). Use **Plan** mode to front-load research, **Ask** / **Manual** for read-only or surgical work.
- _Context:_ attach precise `@Code` / `@Files`, not `@Folders` / `@Codebase`; watch the context ring's Rules / MCP / Conversation breakdown.
- _Rules & ignore:_ `.cursor/rules/*.mdc`, where only **Always Apply** rules cost tokens every turn, so keep those few and lean. Use `.cursorignore` to block and `.cursorindexingignore` to de-index.
- _Spend control:_ the dashboard **Usage** tab, **spend limits**, or disable on-demand usage for a hard cap.

---

## Further reading

- [Anthropic: Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (the "attention budget" and context-rot framing)
- [Claude Code: Manage costs effectively](https://code.claude.com/docs/en/costs)
- [Anthropic: Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [OpenAI: Codex best practices](https://developers.openai.com/codex/learn/best-practices)
- [OpenAI: Prompt caching guide](https://developers.openai.com/api/docs/guides/prompt-caching)
- ["Lost in the Middle" (Liu et al.)](https://arxiv.org/abs/2307.03172) and [Chroma: Context Rot](https://research.trychroma.com/context-rot)
- [RTK](https://www.rtk-ai.app/): command-output compression before agent context
- [Aider: Repository map](https://aider.chat/docs/repomap.html) and [tree-sitter repo map notes](https://aider.chat/2023/10/22/repomap.html)
- [Graphify](https://graphify.net) and the [handoff skill](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md)
- [ponytail](https://github.com/DietrichGebert/ponytail/tree/main/skills/ponytail): a minimalist (YAGNI) skill that biases the agent toward the smallest solution that works and code-first, low-prose replies — less _output_ now, less code to maintain later

---

> [!NOTE]
> **Scope reminder:** optimize for total cost _and_ quality, not raw token count. Keep the context small where it is noisy, and spend context where it reduces engineering risk. Prices and model names move quickly, so re-check the numbers when your tools change.
