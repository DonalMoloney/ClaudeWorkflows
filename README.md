# Claude Code Workflows

This repository is **two things**:

1. **A Claude Code plugin — [`parallel-workflows`](plugin/README.md)** — fans large tasks (audits, migrations, research sweeps) out across parallel agents. Install it:
   ```text
   /plugin marketplace add DonalMoloney/ClaudeWorkflows
   /plugin install parallel-workflows@claude-workflows
   ```
   See **[plugin/README.md](plugin/README.md)** for what it does, the command cheatsheet, and a 30-second first run.

2. **A reference library** — curated, formatted extracts of the official Claude Code docs on dynamic workflows, subagents, and agent orchestration (indexed below).

## Reference library

| File | Source | Content |
|---|---|---|
| `01-official-docs-workflows.md` | code.claude.com/docs/en/workflows | Full official docs on Dynamic Workflows: triggers, approval flow, `/workflows` UI, saving, resumption, limits |
| `02-official-docs-agents-comparison.md` | code.claude.com/docs/en/agents | Comparison of all 4 parallelism approaches: subagents, agent view, agent teams, workflows |
| `03-official-docs-sub-agents.md` | code.claude.com/docs/en/sub-agents | Full subagent reference: built-ins, frontmatter fields, scopes, model resolution, CLI flags |
| `04-official-docs-agent-teams.md` | code.claude.com/docs/en/agent-teams | Agent teams guide: setup, display modes, task list, plan approval, known limitations |
| `05-infoq-article.md` | infoq.com | Overview of Dynamic Workflows launch, use cases, token warning |
| `06-mindstudio-parallel-subagents.md` | mindstudio.ai | Practical guide: orchestrator/subagent model, headless patterns, cost math, 6 failure modes |
| `07-ken-huang-substack.md` | kenhuangus.substack.com | Execution model selection framework: when each primitive fits, decision matrix |
| `08-mindstudio-when-to-use.md` | mindstudio.ai | Counterpoint: why Dynamic Workflows are overused, token cost compounding, 5-step decision framework |

## Quick Decision Guide

| Situation | Use |
|---|---|
| Task outgrows a handful of subagents, split strategy unknown, quality > cost | **Dynamic Workflows** |
| Repeatable, well-defined task, predictable token budget | **Custom Subagents** |
| Multiple specialists need to communicate and share a working codebase | **Agent Teams** (experimental) |
| Several independent sessions to hand off and monitor | **Agent View** |
| Supervised exploratory session with a completion condition | **`/goal`** |

## Key Facts

- Workflows require Claude Code **v2.1.154+**
- Trigger with `ultracode` keyword or "use a workflow" / "run a workflow" (v2.1.160+)
- Max **1,000 agents per run**, **16 concurrent**
- Resumable within the same session; exit = start fresh
- Scripts saved to `~/.claude/projects/<session>/` — readable, diffable, editable
- Save a run as a reusable command: `/workflows` → select run → `s`
- Workflow subagents always run in `acceptEdits` mode regardless of your session mode
- `Date.now()`, `Math.random()`, `new Date()` are banned in workflow scripts (break resumption)
