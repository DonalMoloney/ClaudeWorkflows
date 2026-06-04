# Official Docs: Orchestrate Subagents at Scale with Dynamic Workflows

> Source: https://code.claude.com/docs/en/workflows
> Status: Research preview — requires Claude Code v2.1.154+
> Available on: Pro, Max, Team, Enterprise, Anthropic API, Amazon Bedrock, Google Vertex AI, Microsoft Foundry

---

## What They Are

A **dynamic workflow** is a JavaScript script that orchestrates subagents at scale. Claude writes the script for the task you describe, and a runtime executes it in the background while your session stays responsive.

Use a workflow when:
- A task needs more agents than one conversation can coordinate
- You want the orchestration codified as a script you can read and rerun
- Examples: codebase-wide bug sweep, 500-file migration, cross-checked research, plan drafted from several independent angles

---

## When to Use a Workflow

| | Subagents | Skills | Agent Teams | Workflows |
|---|---|---|---|---|
| What it is | A worker Claude spawns | Instructions Claude follows | A lead agent supervising peer sessions | A script the runtime executes |
| Who decides what runs next | Claude, turn by turn | Claude, following the prompt | The lead agent, turn by turn | The script |
| Where intermediate results live | Claude's context window | Claude's context window | A shared task list | Script variables |
| What's repeatable | The worker definition | The instructions | The team definition | The orchestration itself |
| Scale | A few delegated tasks per turn | Same as subagents | A handful of long-running peers | Dozens to hundreds of agents per run |
| Interruption | Restarts the turn | Restarts the turn | Teammates keep running | Resumable in the same session |

A workflow moves the plan into code — Claude's context holds only the final answer, not every intermediate result.

---

## Run a Bundled Workflow

### `/deep-research`

```
/deep-research What changed in the Node.js permission model between v20 and v22?
```

Fans out web searches across angles → fetches and cross-checks sources → votes on each claim → returns a cited report with failed claims filtered out.

**Steps:**
1. Run `/deep-research <question>`
2. Allow the workflow when prompted
3. Watch progress with `/workflows` (or the task panel below the input box)
4. Read the cited report when the run finishes

### `/workflows` Progress View — Keyboard Shortcuts

| Key | Action |
|---|---|
| `↑` / `↓` | Select a phase or agent |
| `Enter` or `→` | Drill into phase → agent (prompt, tool calls, result) |
| `Esc` | Back out one level |
| `j` / `k` | Scroll within agent detail |
| `p` | Pause or resume the run |
| `x` | Stop selected agent, or whole workflow when focus is on the run |
| `r` | Restart selected running agent |
| `s` | Save run's script as a command |

---

## How to Trigger a Workflow

### 1. Keyword in prompt — `ultracode`

```
ultracode: audit every API endpoint under src/routes/ for missing auth checks
```

- Claude Code highlights the keyword in your input
- Press `Option+W` (macOS) / `Alt+W` (Win/Linux) to cancel for this prompt
- Backspace while cursor is right after the keyword also cancels
- Turn off entirely in `/config` → "Ultracode keyword trigger"
- Natural language also works: "use a workflow", "run a workflow" (v2.1.160+)
- Before v2.1.160, the trigger keyword was `workflow`

### 2. Session-level ultracode

```
/effort ultracode
```

- Combines `xhigh` reasoning effort + automatic workflow orchestration
- Claude plans a workflow for every substantive task
- A single request can produce several workflows in a row (understand → change → verify)
- Resets at session end; drop back with `/effort high`
- Only available on models that support `xhigh` effort

---

## Approval Flow

### CLI options (per-run prompt)

- **Yes, run it** — start the run
- **Yes, and don't ask again for `<name>` in `<path>`** — start + skip prompt for this workflow in this project
- **View raw script** — read before deciding
- **No** — cancel

`Ctrl+G` opens the script in your editor. `Tab` adjusts the prompt before the run starts.

### Per permission mode

| Permission mode | When prompted |
|---|---|
| Default / Accept Edits | Every run (unless "don't ask again" selected) |
| Auto | First launch only; consent saved to user settings; skipped entirely when ultracode is on |
| Bypass / `claude -p` / Agent SDK | Never — starts immediately |

**Important:** The workflow's subagents always run in `acceptEdits` mode and inherit your tool allowlist regardless of session mode. File edits are auto-approved. Shell commands, web fetches, and MCP tools not in your allowlist can still prompt mid-run — add them to your allowlist before a long run.

---

## Saving Workflows for Reuse

From `/workflows` → select run → press `s`:

| Save location | Scope |
|---|---|
| `.claude/workflows/` in project | Shared with everyone who clones the repo |
| `~/.claude/workflows/` in home dir | Personal, available in every project |

Saved workflows become `/<name>` in autocomplete. Project workflow wins if names collide.

### Passing input to a saved workflow

```
> Run /triage-issues on issues 1024, 1025, and 1030
```

Claude passes structured data as the `args` global — array/object methods work directly, no parsing needed. `args` is `undefined` if omitted.

---

## How a Workflow Runs

- Script executes in an **isolated environment** separate from your conversation
- Intermediate results live in **script variables**, not Claude's context
- Every run writes its script to `~/.claude/projects/<session>/` — you can read, diff, or edit it and ask Claude to relaunch from the edited version
- The runtime journals each agent's result → enables **resumption**

### Constraints

| Constraint | Reason |
|---|---|
| No mid-run user input | Only permission prompts can pause; for sign-off between stages, run each stage as its own workflow |
| No direct filesystem/shell access from the script | Agents read/write/run commands; script coordinates |
| Up to 16 concurrent agents (fewer on low-CPU machines) | Bounds local resource use |
| 1,000 agents total per run | Prevents runaway loops |

---

## Resuming After Interruption

- Completed agents return cached results instantly; remaining agents run live
- Resume from `/workflows` → select run → press `p`
- Or ask Claude to relaunch the workflow with the same script path
- **Session-bound:** exiting Claude Code means the next session starts fresh

---

## Cost Management

- Workflow runs count toward your plan's usage and rate limits
- Test on a small slice first (one directory, narrow question)
- `/workflows` shows per-agent token usage live — stop at any time without losing completed work
- Every agent uses your session's model unless the script routes a stage to a different one
- Ask Claude to use a smaller model for stages that don't need the strongest one

---

## Disabling Workflows

**Per-user:**
- Toggle "Dynamic workflows" off in `/config` (persists across sessions)
- `"disableWorkflows": true` in `~/.claude/settings.json`
- `CLAUDE_CODE_DISABLE_WORKFLOWS=1` environment variable

**Org-wide:**
- `"disableWorkflows": true` in managed settings
- Toggle on claude.ai/admin-settings/claude-code

When disabled: bundled workflow commands unavailable, `ultracode` keyword inert, `ultracode` removed from `/effort` menu.

---

## Related Docs

- `/en/agents` — Compare subagents, agent view, agent teams, workflows
- `/en/sub-agents` — Subagent configuration reference
- `/en/agent-teams` — Agent teams guide
- `/en/costs` — Multi-agent token costs and rate limits
