# Claude Code: Parallel Agents Reference

> Scraped from official docs at `code.claude.com/docs/en/` — June 2026  
> Covers: Dynamic Workflows (research preview) · Agent Teams (experimental) · Comparison & Best Practices

---

## Table of Contents

1. [When to Use What — Comparison](#when-to-use-what)
2. [Dynamic Workflows](#dynamic-workflows)
   - [Triggering a Workflow](#triggering-a-workflow)
   - [Bundled Workflows](#bundled-workflows)
   - [Approval Flow](#approval-flow)
   - [Saving & Reuse](#saving--reuse)
   - [How a Workflow Runs](#how-a-workflow-runs)
   - [Runtime Constraints](#runtime-constraints)
   - [Managing Runs](#managing-runs)
   - [Cost & Disabling](#cost--disabling)
3. [Agent Teams (Experimental)](#agent-teams-experimental)
   - [Enabling Agent Teams](#enabling-agent-teams)
   - [Starting a Team](#starting-a-team)
   - [Controlling a Team](#controlling-a-team)
   - [Architecture & Internals](#architecture--internals)
   - [Subagents vs Agent Teams](#subagents-vs-agent-teams)
   - [Limitations](#limitations)
4. [Real-World Case Study: Building a C Compiler with 16 Agents](#real-world-case-study)
5. [Best Practices](#best-practices)

---

## When to Use What

All four approaches run multi-step tasks. The difference is **who holds the plan**:

| | Subagents | Skills | Agent Teams | Dynamic Workflows |
|---|---|---|---|---|
| **What it is** | A worker Claude spawns | Instructions Claude follows | A lead agent supervising peer sessions | A script the runtime executes |
| **Who decides next** | Claude, turn by turn | Claude, following the prompt | The lead agent, turn by turn | The script |
| **Intermediate results** | Claude's context window | Claude's context window | A shared task list | Script variables |
| **What's repeatable** | The worker definition | The instructions | The team definition | The orchestration itself |
| **Scale** | A few delegated tasks per turn | Same as subagents | A handful of long-running peers | Dozens to hundreds of agents per run |
| **Interruption** | Restarts the turn | Restarts the turn | Teammates keep running | Resumable in the same session |
| **Inter-agent comms** | None — results only | None | Direct messaging + shared task list | None — script coordinates |

**Decision framework:**

- **Claude delegates inside one conversation** → Subagents
- **You hand off independent tasks and check back** → Agent View (`claude agents`)
- **Claude plans, assigns, and supervises workers** → Agent Teams *(experimental)*
- **A script holds the plan, not Claude's turn-by-turn judgment** → Dynamic Workflows

**Do workers need to talk to each other?**
- Subagents + Agent View: report results only, no inter-agent comms
- Agent Teams: share a task list and message each other directly

---

## Dynamic Workflows

> **Status:** Research preview — requires Claude Code v2.1.154+  
> **Available on:** Pro, Max, Team, Enterprise · Anthropic API · Amazon Bedrock · Google Vertex AI · Microsoft Foundry  
> **On Pro:** enable from the "Dynamic workflows" row in `/config`

A **dynamic workflow** is a JavaScript script that orchestrates subagents at scale. Claude writes the script for the task you describe, and a runtime executes it in the background while your session stays responsive.

Use a workflow when:
- A task needs more agents than one conversation can coordinate
- You want orchestration codified as a script you can read and rerun
- Examples: codebase-wide bug sweep, 500-file migration, cross-checked research, plan drafted from several angles

A workflow moves the plan **into code** — Claude's context holds only the final answer, not every intermediate result. It also lets you apply repeatable quality patterns: independent agents adversarially reviewing each other's findings, or a plan drafted from several angles and weighed.

---

### Triggering a Workflow

#### 1. `ultracode` keyword in your prompt

```
ultracode: audit every API endpoint under src/routes/ for missing auth checks
```

- Claude Code highlights the keyword; Claude writes a workflow script instead of working turn by turn
- Press `Option+W` (macOS) / `Alt+W` (Win/Linux) to cancel for this prompt only
- Backspace while cursor is right after the highlighted keyword also cancels
- Turn off entirely: `/config` → "Ultracode keyword trigger"
- Natural language also works: "use a workflow", "run a workflow" (v2.1.160+)
- Before v2.1.160, the trigger keyword was `workflow`

#### 2. Session-level ultracode

```
/effort ultracode
```

- Combines `xhigh` reasoning effort + automatic workflow orchestration
- Claude plans a workflow for every substantive task in the session
- A single request can produce several workflows in sequence (understand → change → verify)
- Resets at session end; drop back with `/effort high`
- Only available on models that support `xhigh` effort

---

### Bundled Workflows

| Command | What it does |
|---|---|
| `/deep-research <question>` | Fans out web searches across angles → fetches and cross-checks sources → votes on each claim → returns a cited report with failed claims filtered out. Requires the WebSearch tool |

Workflows you save yourself become commands the same way and appear in `/` autocomplete alongside built-ins.

**Running `/deep-research`:**

1. `/deep-research What changed in the Node.js permission model between v20 and v22?`
2. Allow the workflow when prompted
3. Watch progress: `/workflows` → select run → Enter (or use the task panel below the input box)
4. Read the cited report when the run finishes

---

### Watching Runs — `/workflows` Keyboard Shortcuts

```
/workflows
```

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

A one-line progress summary also appears in the task panel below the input box while running.

---

### Approval Flow

**CLI per-run prompt options:**

- **Yes, run it** — start the run
- **Yes, and don't ask again for `<name>` in `<path>`** — start + skip prompt for this workflow in this project
- **View raw script** — read before deciding
- **No** — cancel

`Ctrl+G` opens the script in your editor. `Tab` adjusts the prompt before the run starts.

**Per permission mode:**

| Permission mode | When prompted |
|---|---|
| Default / Accept Edits | Every run (unless "don't ask again" selected) |
| Auto | First launch only; consent saved to user settings; skipped entirely when ultracode is on |
| Bypass / `claude -p` / Agent SDK | Never — starts immediately |

> **Important:** The workflow's subagents always run in `acceptEdits` mode and inherit your tool allowlist regardless of session mode. File edits are auto-approved. Shell commands, web fetches, and MCP tools not in your allowlist can still prompt mid-run — add them to your allowlist before a long run.

**Desktop app:** An approval card shows workflow name, phase list, and a token-usage caution, with **Once**, **Always**, and **Deny** actions. Progress appears in the Background tasks side pane.

---

### Saving & Reuse

From `/workflows` → select run → press `s`:

| Save location | Scope |
|---|---|
| `.claude/workflows/` in project | Shared with everyone who clones the repo |
| `~/.claude/workflows/` in home dir | Personal, available in every project |

Saved workflows become `/<name>` in autocomplete. If names collide, the project workflow wins.

**Passing input to a saved workflow:**

```
> Run /triage-issues on issues 1024, 1025, and 1030
```

Claude passes structured data as the `args` global — array/object methods work directly, no parsing needed. `args` is `undefined` if omitted.

---

### How a Workflow Runs

- Script executes in an **isolated environment** separate from your conversation
- Intermediate results live in **script variables**, not Claude's context
- Every run writes its script to `~/.claude/projects/<session>/` — you can read, diff, or edit it and ask Claude to relaunch from the edited version
- The runtime journals each agent's result, enabling **resumption**
- Scripts are plain JavaScript (not TypeScript) running in an async context

---

### Runtime Constraints

| Constraint | Why |
|---|---|
| No mid-run user input | Only permission prompts can pause; for sign-off between stages, run each stage as its own workflow |
| No direct filesystem/shell from the script itself | Agents read/write/run commands; script coordinates |
| Up to 16 concurrent agents (fewer on low-CPU machines) | Bounds local resource use |
| 1,000 agents total per run | Prevents runaway loops |

---

### Managing Runs

**Resuming after interruption:**
- Completed agents return cached results instantly; remaining agents run live
- Resume from `/workflows` → select run → press `p`
- Or ask Claude to relaunch the workflow with the same script path
- **Session-bound:** exiting Claude Code means the next session starts fresh

---

### Cost & Disabling

**Cost management:**
- Workflow runs count toward your plan's usage and rate limits
- Test on a small slice first (one directory, narrow question)
- `/workflows` shows per-agent token usage live — stop anytime without losing completed work
- Every agent uses your session's model unless the script routes a stage to a different one
- Ask Claude to use a smaller model for stages that don't need the strongest one

**Disabling per-user:**
- Toggle "Dynamic workflows" off in `/config` (persists across sessions)
- `"disableWorkflows": true` in `~/.claude/settings.json`
- `CLAUDE_CODE_DISABLE_WORKFLOWS=1` environment variable

**Disabling org-wide:**
- `"disableWorkflows": true` in managed settings
- Toggle on `claude.ai/admin-settings/claude-code`

When disabled: bundled workflow commands unavailable, `ultracode` keyword inert, `ultracode` removed from `/effort` menu.

---

## Agent Teams (Experimental)

> **Status:** Experimental — disabled by default  
> **Requires:** Claude Code v2.1.32+  
> **Warning:** Known limitations around session resumption, task coordination, and shutdown behavior

Agent teams let you coordinate multiple Claude Code instances working together. One session acts as the **team lead**, coordinating work, assigning tasks, and synthesizing results. Teammates work independently, each in its own context window, and can communicate directly with each other.

Unlike subagents (which run within a single session and report only to the main agent), you can interact with individual teammates directly without going through the lead.

---

### Enabling Agent Teams

Add to `settings.json` or your shell environment:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

Or set `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in your environment.

---

### Starting a Team

Tell Claude to create an agent team in natural language:

```
I'm designing a CLI tool that helps developers track TODO comments across
their codebase. Create an agent team to explore this from different angles: one
teammate on UX, one on technical architecture, one playing devil's advocate.
```

Claude creates a team with a shared task list, spawns teammates for each perspective, has them explore the problem, synthesizes findings, and attempts to clean up the team when finished.

**Two ways a team gets started:**
1. **You request it** — explicitly ask for an agent team
2. **Claude proposes it** — Claude determines your task would benefit from parallel work and suggests a team (requires your confirmation)

---

### Controlling a Team

#### Display Modes

| Mode | Description | Requirements |
|---|---|---|
| **In-process** (default) | All teammates run inside your main terminal. Use `Shift+Down` to cycle through teammates. Works in any terminal. | None |
| **Split panes** | Each teammate gets its own pane. See everyone's output at once. Click into a pane to interact directly. | tmux or iTerm2 |

Configure in `~/.claude/settings.json`:
```json
{
  "teammateMode": "in-process"
}
```

Or per-session:
```bash
claude --teammate-mode in-process
```

**tmux tip:** `tmux -CC` in iTerm2 is the suggested entrypoint. iTerm2 also requires the `it2` CLI and Python API enabled in settings.

Default is `"auto"`: split panes if already in tmux, in-process otherwise.

#### Specifying Teammates and Models

```
Create a team with 4 teammates to refactor these modules in parallel.
Use Sonnet for each teammate.
```

Teammates don't inherit the lead's `/model` by default. Set **Default teammate model** in `/config`, or choose **Default (leader's model)** to follow the lead's current model.

#### Requiring Plan Approval

```
Spawn an architect teammate to refactor the authentication module.
Require plan approval before they make any changes.
```

The teammate works in read-only plan mode until the lead approves. The lead reviews the plan and either approves or rejects with feedback. Rejected plans get revised and resubmitted. Once approved, the teammate begins implementation.

#### Talking to Teammates Directly

- **In-process mode:** `Shift+Down` to cycle through teammates, then type to send a message. `Enter` to view a teammate's session, `Escape` to interrupt their current turn. `Ctrl+T` toggles the task list.
- **Split-pane mode:** click into a teammate's pane to interact directly.

#### Task Assignment

The shared task list coordinates work. Tasks have three states: **pending**, **in progress**, **completed**. Tasks can have dependencies (a task with unresolved dependencies cannot be claimed).

- **Lead assigns:** tell the lead which task to give to which teammate
- **Self-claim:** after finishing, a teammate picks up the next unassigned, unblocked task autonomously

Task claiming uses **file locking** to prevent race conditions.

#### Shutting Down Teammates

```
Ask the researcher teammate to shut down
```

The lead sends a shutdown request. The teammate can approve (exit gracefully) or reject with an explanation.

#### Cleaning Up the Team

```
Clean up the team
```

> **Warning:** Always use the lead to clean up. Teammates running cleanup may leave resources in an inconsistent state.

#### Quality Gates with Hooks

| Hook | Trigger | Exit code 2 effect |
|---|---|---|
| `TeammateIdle` | Teammate about to go idle | Send feedback and keep the teammate working |
| `TaskCreated` | Task being created | Prevent creation and send feedback |
| `TaskCompleted` | Task being marked complete | Prevent completion and send feedback |

---

### Architecture & Internals

| Component | Role |
|---|---|
| **Team lead** | The main Claude Code session — creates the team, spawns teammates, coordinates work |
| **Teammates** | Separate Claude Code instances, each working on assigned tasks |
| **Task list** | Shared list of work items that teammates claim and complete |
| **Mailbox** | Messaging system for direct communication between agents |

**Storage:**
- Team config: `~/.claude/teams/{team-name}/config.json`
- Task list: `~/.claude/tasks/{team-name}/`

Both are auto-generated. Do not edit the team config by hand — it holds runtime state (session IDs, tmux pane IDs) and is overwritten on each state update.

**Context and communication:**
- Each teammate has its own context window; the lead's conversation history does not carry over
- Teammates load the same project context as a regular session: CLAUDE.md, MCP servers, skills
- Messages between agents are delivered automatically (no polling by lead)
- When a teammate finishes, it automatically notifies the lead
- Any agent can message any other by name using `SendMessage`
- Task dependencies unblock automatically when their prerequisite completes

**Permissions:**
- Teammates start with the lead's permission settings
- If lead runs with `--dangerously-skip-permissions`, all teammates do too
- You can change individual teammate modes after spawning, but not at spawn time

**Using subagent definitions for teammates:**

Reference a named subagent type when spawning:
```
Spawn a teammate using the security-reviewer agent type to audit the auth module.
```

The teammate honors that definition's `tools` allowlist and `model`. The definition's body is appended to the teammate's system prompt. Team coordination tools (`SendMessage`, task management tools) are always available regardless of `tools` restrictions.

> Note: `skills` and `mcpServers` frontmatter fields in a subagent definition are NOT applied when running as a teammate — teammates load skills and MCP servers from project/user settings.

---

### Subagents vs Agent Teams

| | Subagents | Agent Teams |
|---|---|---|
| **Context** | Own context window; results return to caller | Own context window; fully independent |
| **Communication** | Report results to main agent only | Teammates message each other directly |
| **Coordination** | Main agent manages all work | Shared task list with self-coordination |
| **Best for** | Focused tasks where only the result matters | Complex work requiring discussion and collaboration |
| **Token cost** | Lower: results summarized back | Higher: each teammate is a separate Claude instance |

**Rule of thumb:** Use subagents when you need quick, focused workers that report back. Use agent teams when teammates need to share findings, challenge each other, and coordinate on their own.

---

### Limitations

Agent teams are experimental. Current known limitations:

| Limitation | Detail |
|---|---|
| **No session resumption with in-process teammates** | `/resume` and `/rewind` do not restore in-process teammates. If the lead messages non-existent teammates after resume, spawn new ones. |
| **Task status can lag** | Teammates sometimes fail to mark tasks complete, blocking dependent tasks. Check if work is done and update status manually or nudge via lead. |
| **Shutdown can be slow** | Teammates finish their current request/tool call before shutting down. |
| **One team at a time** | A lead can only manage one team. Clean up before creating a new one. |
| **No nested teams** | Teammates cannot spawn their own teams or teammates. Only the lead can manage the team. |
| **Lead is fixed** | The session that creates the team is the lead for its lifetime. No leadership transfer. |
| **Permissions set at spawn** | All teammates start with the lead's permission mode. Can change individually after spawn, but not at spawn time. |
| **Split panes require tmux or iTerm2** | Not supported in VS Code integrated terminal, Windows Terminal, or Ghostty. |

---

## Real-World Case Study

### Building a C Compiler with 16 Parallel Claude Agents

> Source: [anthropic.com/engineering/building-c-compiler](https://www.anthropic.com/engineering/building-c-compiler)

A stress-test of agent teams at scale: 16 agents tasked with writing a Rust-based C compiler from scratch, capable of compiling the Linux kernel. Result: a 100,000-line compiler across ~2,000 Claude Code sessions.

#### Core Execution Loop

```bash
while true; do
    claude --dangerously-skip-permissions \
           -p "$(cat AGENT_PROMPT.md)" \
           --model claude-opus-X-Y
done
```

A simple loop where Claude finishes one task, then immediately picks up the next.

#### Coordination Mechanism

- **File-based task locking:** Agents take a lock by writing to `current_tasks/` — git handles conflicts and forces a second agent to pick a different task
- **Containerized parallelism:** Each agent ran in Docker with a mounted git repo; agents cloned locally to `/workspace` and pushed back when done

#### Team Specialization (16 agents)

- Core compiler developers (main functionality)
- Dedicated "coalescing" agent (merging duplicate code)
- Performance optimization specialist
- Code efficiency specialist
- Design critic (structural improvements)
- Documentation maintainer

#### Key Lessons

| Lesson | Detail |
|---|---|
| **High-quality tests are non-negotiable** | "The task verifier is nearly perfect, otherwise Claude will solve the wrong problem." Claude relies entirely on tests to self-orient autonomously. |
| **Avoid context flooding** | Avoid "printing thousands of useless bytes." Maintain "extensive READMEs and progress files that should be updated frequently." |
| **Intelligent task subsetting** | Used GCC as an online oracle to enable parallel debugging of different files; prevents all agents from hitting identical blockers. |
| **Design for time blindness** | Include progress indicators; implement deterministic subsample testing so each agent can identify regressions independently. |
| **Parallel work requires decomposition** | Agents excel on independent failing tests; struggle with monolithic tasks requiring orchestrated changes across files. |
| **Git conflict resilience** | Claude can resolve merge conflicts autonomously during parallel development. |

#### Outcomes & Limits

- 100,000-line Rust compiler, $20,000 in API costs
- Supports Linux 6.9 on x86, ARM, RISC-V
- Practical limits: no efficient 16-bit x86 code, no own assembler/linker, less optimized output than mature compilers
- New features and bugfixes frequently broke existing functionality — Claude Opus 4.6 approached capability boundaries at this complexity level

---

## Best Practices

### For Dynamic Workflows

- **Start small:** Test on a slice (one directory, narrow question) before a full run
- **Check your model:** Run `/model` before a large workflow run; ask Claude to use a smaller model for stages that don't need the strongest
- **Pre-allowlist tools:** Shell commands, web fetches, and MCP tools not in your allowlist can prompt mid-run — add them before starting a long run
- **Save good runs as commands:** If a workflow does what you wanted, press `s` in `/workflows` to turn it into a reusable `/<name>` command
- **Favor pipeline over parallel in scripts:** `pipeline()` is the default — items process through stages independently without waiting for all items to finish a stage

### For Agent Teams

- **Give teammates enough context in spawn prompts:** Teammates don't inherit lead conversation history. Include task-specific details:
  ```
  Spawn a security reviewer teammate with the prompt: "Review the authentication module
  at src/auth/ for security vulnerabilities. Focus on token handling, session
  management, and input validation..."
  ```
- **Optimal team size is 3–5 teammates:** Token costs scale linearly; coordination overhead increases with team size; start here and scale only when the work genuinely benefits
- **5–6 tasks per teammate** keeps everyone productive without excessive context switching
- **Size tasks appropriately:** Too small = coordination overhead exceeds benefit. Too large = teammates work too long without check-ins. Just right = self-contained units with a clear deliverable (a function, a test file, a review)
- **Avoid file conflicts:** Two teammates editing the same file leads to overwrites. Break work so each teammate owns different files
- **Wait for teammates to finish:** If the lead starts implementing tasks itself, tell it: `"Wait for your teammates to complete their tasks before proceeding"`
- **Start with research and review:** If new to agent teams, begin with clear-boundary tasks that don't require writing code
- **Use hooks for quality gates:** `TeammateIdle`, `TaskCreated`, and `TaskCompleted` hooks enforce standards deterministically
- **Monitor and steer:** Check in on teammates' progress; letting a team run unattended too long increases wasted effort risk

### General Multi-Agent Patterns

- **Writer/Reviewer pattern:** Session A implements → Session B reviews in fresh context → Session A fixes. A reviewer without the implementation context evaluates the result on its own terms.
- **Adversarial verify:** For important findings, have independent agents try to *refute* each finding. What survives adversarial review is far more likely to be correct.
- **Competing hypotheses:** When root cause is unclear, assign different theories to different agents and have them actively try to disprove each other's theories. The theory that survives is more likely to be real.
- **Subagents for context hygiene:** Use subagents for investigation-heavy tasks so exploration doesn't fill your main context window

---

## Related Documentation

- [Run agents in parallel](https://code.claude.com/docs/en/agents) — Compare all four approaches
- [Dynamic workflows](https://code.claude.com/docs/en/workflows) — Full workflow reference
- [Agent teams](https://code.claude.com/docs/en/agent-teams) — Full agent teams reference
- [Subagents](https://code.claude.com/docs/en/sub-agents) — Configure reusable specialist agents
- [Worktrees](https://code.claude.com/docs/en/worktrees) — Git worktree isolation for parallel sessions
- [Costs](https://code.claude.com/docs/en/costs) — Token usage and rate limits for multi-agent runs
- [C Compiler Case Study](https://www.anthropic.com/engineering/building-c-compiler) — Real-world 16-agent team at scale
