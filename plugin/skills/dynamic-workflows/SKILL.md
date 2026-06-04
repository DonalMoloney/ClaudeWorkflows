---
name: Dynamic Workflows
description: Use this skill when the user wants to understand, trigger, write, manage, or debug a Claude Code dynamic workflow. Covers when workflows beat subagents, how to trigger them, what the script primitives do, how to save and resume runs, and cost management.
version: 1.0.0
---

## What a dynamic workflow is

A dynamic workflow is a JavaScript script that orchestrates subagents at scale. Claude writes the script, and a runtime executes it in the background while your session stays responsive. Intermediate results live in script variables — not in Claude's context window.

## When to reach for a workflow

| Situation | Best tool |
|---|---|
| A few delegated tasks per turn | Subagents |
| Independent sessions you want to hand off and check | Agent View |
| A plan Claude assigns and supervises | Agent Teams |
| Dozens–hundreds of agents, repeatable orchestration, cross-checked results | **Dynamic Workflow** |

The key shift: with subagents, Claude decides turn-by-turn what to spawn next. With a workflow, **the script holds the plan** — Claude's context holds only the final answer.

## How to trigger a workflow

**Single task:** Include `ultracode` in the prompt, or say "use a workflow" / "run a workflow":
```
ultracode: audit every API endpoint under src/routes/ for missing auth checks
```

**Session-wide:** `/effort ultracode` — Claude plans a workflow for every substantive task. Resets at session end.

**Run a saved workflow:** `/deep-research <question>` is the built-in example. Saved workflows appear in `/` autocomplete.

**Cancel the trigger:** Press `Option+W` (macOS) or `Alt+W` (Win/Linux) to dismiss the keyword highlight for that prompt.

## Workflow script primitives

```js
// Spawn a sub-agent. Returns text without schema, validated object with schema.
agent(prompt, {
  label: 'display-label',   // shown in /workflows progress view
  phase: 'Phase Name',      // groups this agent under a progress heading
  schema: JSON_SCHEMA,      // forces structured output + retries on mismatch
  model: 'haiku',           // override model for this agent
  isolation: 'worktree'     // run in isolated git worktree (for parallel file edits)
})

// Pipeline: no barrier between stages. Wall-clock = slowest single-item chain.
// stage callback receives (prevResult, originalItem, index)
pipeline(items, stage1, stage2, ...)

// Parallel: BARRIER — all thunks complete before returning.
// Use ONLY when next stage needs ALL prior results together.
parallel([ () => agent(...), () => agent(...) ])

// Progress grouping
phase('Phase Title')   // subsequent agent() calls appear under this heading
log('message')         // narrator line in progress view

// Token budget (when user sets +Nk target)
budget.total           // null if no target set
budget.spent()         // output tokens spent this turn
budget.remaining()     // max(0, total - spent()), or Infinity if no target

// Run a child workflow inline
workflow('saved-name', args)
workflow({scriptPath: '/path/to/script.js'}, args)
```

## Pipeline vs parallel — the decision rule

**Default: `pipeline()`**. Items flow through stages independently — item A can be in stage 3 while item B is still in stage 1.

**Reach for `parallel()` ONLY when:**
- Stage N needs ALL of stage N-1's results (dedup, early-exit on zero count, cross-comparison)

**Smell test:** If your `parallel()` is followed by `.filter()` / `.map()` and then another `parallel()`, rewrite as a `pipeline()` with the transform inside a stage.

## Managing runs

```
/workflows           — list running and completed workflows
↑ / ↓               — select phase or agent
Enter / →            — drill into phase → agent (prompt, tool calls, result)
p                    — pause or resume
x                    — stop selected agent or whole workflow
r                    — restart selected running agent
s                    — save script as a reusable command
```

**Resume:** Completed agents return cached results instantly; remaining agents run live. Works within the same session only — exiting Claude Code starts fresh.

## Saving for reuse

From `/workflows`, select the run and press `s`. Two save locations:
- `.claude/workflows/` — shared with everyone who clones the repo
- `~/.claude/workflows/` — personal, available in every project

Saved workflows become `/<name>` in autocomplete. Project workflow wins on name collision.

## Cost management

- Test on a small slice first (one directory, narrow question)
- `/workflows` shows per-agent token usage live — stop anytime without losing completed work
- Ask Claude to route cheap stages to `haiku` or `sonnet`
- The 1,000-agent-per-run cap bounds runaway scripts

## Constraints

| Constraint | Why |
|---|---|
| No mid-run user input | For sign-off between stages, run each as its own workflow |
| No direct filesystem/shell access from script | Agents do the file work; script coordinates |
| Max 16 concurrent agents | Bounds local CPU/resource use |
| Max 1,000 agents per run | Prevents runaway loops |
| No `Date.now()`, `Math.random()`, `new Date()` | Would break deterministic resume |
| Sub-agents inherit session tool allowlist | Shell commands, web fetches, and MCP tools not on the allowlist can still prompt mid-run — add them to your allowlist before a long run |
| Plain JavaScript only (no TypeScript) | Runtime executes JS, not TS |

## Troubleshooting

| Mistake | Symptom | Fix |
|---|---|---|
| `Date.now()` / `Math.random()` / `new Date()` | Resume broken — cached results don't replay correctly | Remove entirely; pass timestamps via `args` or stamp results after the workflow returns |
| TypeScript syntax (`: string`, `interface Foo`, `<T>`) | Runtime parse error before any agent runs | Plain JavaScript only — remove all type annotations |
| Missing `.filter(Boolean)` after `parallel()` | `null` in results array crashes downstream `.map()` or property access | Always `.filter(Boolean)` before iterating parallel results |
| Using `parallel()` as default | Long wall-clock time — all items wait for the slowest stage | Default to `pipeline()`; use `parallel()` only when stage N needs ALL prior results |
| Manual retry on schema mismatch | Double-retry amplification, inflated token spend | Remove manual retry; the runtime retries internally on schema mismatch |
