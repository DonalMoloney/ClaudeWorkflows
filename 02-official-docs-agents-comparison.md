# Official Docs: Run Agents in Parallel — Comparison Guide

> Source: https://code.claude.com/docs/en/agents

---

## The Four Parallelism Approaches

| Approach | What it gives you | Use it when |
|---|---|---|
| **Subagents** | Delegated workers inside one session; do a side task in their own context and return a summary | A side task would flood your main conversation with search results, logs, or file contents you won't reference again |
| **Agent View** | One screen to dispatch and monitor sessions running in the background, opened with `claude agents`. Research preview | You have several independent tasks and want to hand them off, check status at a glance, and step in only when one needs you |
| **Agent Teams** | Multiple coordinated sessions with a shared task list and inter-agent messaging, managed by a lead. Experimental and disabled by default | You want Claude to split a project into pieces, assign them, and keep the workers in sync |
| **Dynamic Workflows** | A script that runs many subagents and cross-checks their results. Research preview | A job outgrows a handful of subagents, or you want findings verified against each other: a codebase-wide audit, a 500-file migration, cross-checked research, or a plan drafted from several angles |

In every approach, the workers are Claude sessions. To involve a different tool, expose it as an MCP server.

---

## Supporting Tools (Not Coordination Styles)

- **Worktrees** — give each session a separate git checkout so parallel sessions never edit the same files. Agent view auto-moves dispatched sessions into their own worktree; subagents can each get one too.
- **`/batch`** — a skill that has Claude split one large change into 5–30 worktree-isolated subagents that each open a PR. Packaged use of subagents + worktrees, not a separate coordination style.

### Features that run Claude without being agent coordination:
- **Background bash command** — runs one shell command without blocking conversation; doesn't spawn an agent
- **Forked subagent** — a subagent that inherits your full conversation context instead of starting fresh
- **Routine** — runs a session on a schedule in Anthropic's cloud; not parallel on your machine

---

## Decision Framework

**Who coordinates the work?**
- Claude delegates inside one conversation → **Subagents**
- You hand off independent tasks and check back → **Agent view**
- Claude plans, assigns, and supervises workers → **Agent teams** *(experimental)*
- A script holds the plan instead of Claude's turn-by-turn judgment → **Dynamic workflows**

**Do workers need to talk to each other?**
- Subagents + Agent view sessions report results only; no inter-agent communication
- Agent team teammates share a task list and message each other directly

**Do tasks touch the same files?**
- Use **Worktrees** to isolate. Agent view auto-does this. Agent teams don't — partition work so each teammate owns different files.

---

## Checking on Running Work

| What's running | Command |
|---|---|
| Background sessions | `claude agents` → opens Agent View |
| Subagents in current session | `/agents` → Running tab + Library tab |
| Background tasks in current session | `/tasks` |
| Dynamic workflows | `/workflows` |

---

## Related Guides

- `/en/sub-agents` — Define reusable specialists, control tool access
- `/en/agent-view` — Dispatch sessions, watch state, attach when needed
- `/en/agent-teams` — Set up lead + teammates, assign tasks, review work
- `/en/workflows` — Run bundled workflows or have Claude write one
- `/en/worktrees` — Git worktree isolation for parallel sessions
- `/en/costs` — Token usage and rate limits for multi-agent runs
