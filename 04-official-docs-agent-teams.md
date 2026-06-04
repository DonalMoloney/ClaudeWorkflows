# Official Docs: Orchestrate Teams of Claude Code Sessions (Agent Teams)

> Source: https://code.claude.com/docs/en/agent-teams
> Status: EXPERIMENTAL — disabled by default

---

## Enable Agent Teams

```json
// settings.json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

Or set the environment variable directly. Requires Claude Code v2.1.32+.

---

## What Agent Teams Are

Multiple Claude Code instances working together. One session is the **team lead**; others are **teammates**. Unlike subagents (which only report back to the main agent), teammates:
- Each have their own full context window
- Communicate directly with each other
- Share a task list and self-coordinate
- Can message the lead or any other teammate by name

---

## Subagents vs. Agent Teams

| | Subagents | Agent Teams |
|---|---|---|
| Context | Own window; results return to caller | Own window; fully independent |
| Communication | Report results back to main agent only | Teammates message each other directly |
| Coordination | Main agent manages all work | Shared task list with self-coordination |
| Best for | Focused tasks where only the result matters | Complex work requiring discussion and collaboration |
| Token cost | Lower (results summarized back) | Higher (each teammate is a separate Claude instance) |

---

## Starting an Agent Team

```text
I'm designing a CLI tool that helps developers track TODO comments across
their codebase. Create an agent team to explore this from different angles:
one teammate on UX, one on technical architecture, one playing devil's advocate.
```

Claude creates the team, spawns teammates, coordinates work, synthesizes findings, and attempts cleanup when finished.

---

## Display Modes

| Mode | Description | Setup required |
|---|---|---|
| **In-process** (default) | All teammates run inside your main terminal; `Shift+Down` to cycle | None |
| **Split panes** | Each teammate gets its own pane; see everyone's output at once | tmux or iTerm2 |

Setting in `~/.claude/settings.json`:
```json
{ "teammateMode": "in-process" }
```
Or per-session:
```bash
claude --teammate-mode in-process
```

**Auto** (default): uses split panes if already in tmux, in-process otherwise.

---

## Control Commands

| Action | How |
|---|---|
| Cycle through teammates | `Shift+Down` (in-process mode) |
| Send message to teammate | Cycle to them, then type |
| Interrupt current turn | Press `Enter` to view session, then `Escape` |
| Toggle task list | `Ctrl+T` |
| Split-pane: interact | Click the pane directly |

---

## Specifying Teammates and Models

```text
Create a team with 4 teammates to refactor these modules in parallel.
Use Sonnet for each teammate.
```

- Teammates don't inherit the lead's `/model` selection by default
- Set **Default teammate model** in `/config` to change this
- **Default (leader's model)** option makes teammates follow the lead's current model

---

## Plan Approval for Teammates

```text
Spawn an architect teammate to refactor the authentication module.
Require plan approval before they make any changes.
```

Flow:
1. Teammate works in read-only plan mode
2. Sends plan approval request to lead
3. Lead reviews: approve or reject with feedback
4. If rejected: teammate revises and resubmits
5. Once approved: teammate exits plan mode and implements

Influence the lead's judgment in your spawn prompt: "only approve plans that include test coverage."

---

## Task List & Coordination

- Shared task list with states: **pending**, **in progress**, **completed**
- Tasks can have dependencies — a pending task with unresolved dependencies cannot be claimed until those are complete
- **Lead assigns** tasks explicitly, or **teammates self-claim** the next unassigned/unblocked task
- File locking prevents race conditions when multiple teammates try to claim the same task

---

## Shutting Down and Cleanup

```text
Ask the researcher teammate to shut down
```
Lead sends shutdown request; teammate can approve (exits gracefully) or reject with explanation.

```text
Clean up the team
```
Removes shared team resources. **Always use the lead to clean up** — teammates may leave resources in inconsistent state. Fails if active teammates are still running (shut them down first).

---

## Architecture

| Component | Role |
|---|---|
| Team lead | Main Claude Code session; creates team, spawns teammates, coordinates |
| Teammates | Separate Claude Code instances working on assigned tasks |
| Task list | Shared work items that teammates claim and complete |
| Mailbox | Messaging system for inter-agent communication |

Storage:
- Team config: `~/.claude/teams/{team-name}/config.json` (auto-generated; don't edit by hand)
- Task list: `~/.claude/tasks/{team-name}/`

---

## Permissions

- Teammates start with the lead's permission settings
- `--dangerously-skip-permissions` on lead → all teammates get it too
- Can change individual teammate modes after spawning
- Cannot set per-teammate modes at spawn time

---

## Using Subagent Definitions as Teammates

Reference any subagent type from project/user/plugin scope:
```text
Spawn a teammate using the security-reviewer agent type to audit the auth module.
```
- Teammate uses that definition's `tools` and `model`
- Definition body is appended to teammate's system prompt (not replacing it)
- Team coordination tools (`SendMessage`, task management) always available even when `tools` restricts others
- `skills` and `mcpServers` frontmatter fields are NOT applied for teammates (loaded from project/user settings instead)

---

## Best Practices

- **Give teammates enough context** in the spawn prompt — they don't inherit the lead's conversation history
- **Team size**: 3–5 teammates for most workflows; start there before scaling up
- **Task sizing**: 5–6 tasks per teammate; self-contained units with clear deliverables
- **Avoid file conflicts**: partition work so each teammate owns different files
- **Start with research/review** before coding tasks (clearer boundaries, lower conflict risk)
- **Monitor and steer**: check in on progress; redirect approaches that aren't working

---

## Use Case Examples

### Parallel Code Review
```text
Create an agent team to review PR #142. Spawn three reviewers:
- One focused on security implications
- One checking performance impact
- One validating test coverage
Have them each review and report findings.
```

### Competing Hypothesis Bug Hunt
```text
Users report the app exits after one message instead of staying connected.
Spawn 5 agent teammates to investigate different hypotheses. Have them talk to
each other to try to disprove each other's theories, like a scientific debate.
Update the findings doc with whatever consensus emerges.
```

---

## Hooks for Quality Gates

| Hook | When it fires | Exit 2 to... |
|---|---|---|
| `TeammateIdle` | Teammate about to go idle | Send feedback and keep teammate working |
| `TaskCreated` | Task being created | Prevent creation and send feedback |
| `TaskCompleted` | Task being marked complete | Prevent completion and send feedback |

---

## Known Limitations

- **No session resumption with in-process teammates**: `/resume` and `/rewind` don't restore in-process teammates
- **Task status can lag**: teammates sometimes fail to mark tasks complete; check manually
- **Shutdown can be slow**: waits for current request/tool call to finish
- **One team at a time**: clean up before creating a new team
- **No nested teams**: teammates cannot spawn their own teams
- **Lead is fixed**: can't promote a teammate to lead
- **Split panes not supported** in VS Code integrated terminal, Windows Terminal, or Ghostty
