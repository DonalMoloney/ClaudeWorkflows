# Parallel Workflows

A Claude Code plugin that turns large, sequential tasks into **fan-outs of parallel agents** — audits, migrations, research sweeps, and multi-file reviews that finish in minutes instead of hours. It ships two commands, two specialist agents, and three skills that teach Claude *when* and *how* to orchestrate.

## Install in 30 seconds

```text
/plugin marketplace add DonalMoloney/ClaudeWorkflows
/plugin install parallel-workflows@claude-workflows
```

That's it — the commands, agents, and skills are now available in every session. To confirm, type `/` and look for `/orchestrate` and `/parallel-sweep` in the autocomplete.

> **Requirements:** Claude Code **v2.1.154+** (dynamic workflows). Pro users must enable workflows in `/config`.

## What you get

| Component | Type | Use it when you want to… |
|---|---|---|
| `/parallel-sweep <target> [focus]` | command | Scan a codebase/file list with many agents at once, then adversarially verify findings into one report. **Best first run.** |
| `/orchestrate <task>` | command | Design a custom multi-phase workflow (discover → execute → verify → synthesize) for a task that doesn't fit a simple sweep. |
| `workflow-architect` | agent | Get help writing or debugging a workflow script — `pipeline` vs `parallel`, phases, adversarial verification. |
| `sub-agent-specialist` | agent | Design the prompt, schema, tool access, and model for the agents *inside* a workflow. |
| `Dynamic Workflows` | skill | Learn what workflows are, how to trigger them, and the script primitives. Auto-loads when relevant. |
| `Parallel Orchestration Patterns` | skill | Pick the right pattern: adversarial verify, loop-until-dry, judge panel, multi-modal sweep, completeness critic. |
| `Headless Shell Orchestration` | skill | Fan out `claude -p` sessions from bash/Python for CI pipelines — no workflow runtime needed. |

## Your first run

Point a sweep at a folder and tell it what to look for:

```text
/parallel-sweep src/ security
```

**What happens:**
1. **Discover** — Claude lists the files under `src/` and groups them into 5–20 chunks.
2. **Fan out** — one agent per chunk reviews its slice for auth gaps, injection risks, and exposed secrets — all in parallel, each in its own context window.
3. **Verify** — high-severity findings are sent to independent "refuter" agents; a finding only survives if the majority can't refute it.
4. **Report** — you get a single ranked report: `CRITICAL` (verified) vs `WARNINGS` (unverified), each with file, line, confidence, and a suggested fix.

Swap `security` for `coverage`, `performance`, or `dead-code` to change the focus. Start with one directory to keep the token cost small — `/workflows` shows live per-agent usage and you can stop anytime without losing completed work.

## Which command do I use?

| Your situation | Reach for |
|---|---|
| "Review/audit these files for X" — a known target, one focus | **`/parallel-sweep`** |
| "Do this big multi-step task" — migration, research, anything needing discover→execute→verify→synthesize | **`/orchestrate`** |
| "I just want to understand workflows before running one" | Ask anything — the **Dynamic Workflows** skill loads automatically |

## How it works under the hood

Each agent the plugin spawns is a **fresh, isolated Claude Code session**: its own context window, its own token budget, and no shared memory with its siblings. The orchestrator (your main session) holds only the plan and the final synthesis — intermediate results live in the workflow script's variables, not in your context. That isolation is what lets a single run scale to many agents (up to 16 concurrent, 1,000 per run) without blowing up the context window.

## Learn more

- The bundled skills are the deep dives — invoke **Dynamic Workflows**, **Parallel Orchestration Patterns**, or **Headless Shell Orchestration** for copy-paste patterns and the full primitive reference.
- The [ClaudeWorkflows repository](https://github.com/DonalMoloney/ClaudeWorkflows) hosts a curated reference library (`01`–`08`) extracted from the [official Claude Code docs](https://code.claude.com/docs/en/workflows): the workflow runtime, subagents, agent teams, cost trade-offs, and when *not* to use workflows.

## Troubleshooting

- **Commands don't appear?** Run `/plugin` to confirm `parallel-workflows@claude-workflows` is installed and enabled, then restart Claude Code.
- **"Workflows aren't enabled"?** Update to Claude Code v2.1.154+ and enable workflows in `/config`.
- **Validate the plugin/marketplace locally:** `claude plugin validate .` from the repo root.
