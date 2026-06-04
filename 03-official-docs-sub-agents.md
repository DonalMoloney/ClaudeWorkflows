# Official Docs: Create Custom Subagents

> Source: https://code.claude.com/docs/en/sub-agents

---

## What Subagents Are

Subagents are specialized AI assistants that handle specific tasks in their own context window and return only the summary. Use one when a side task would flood your main conversation with search results, logs, or file contents you won't reference again.

Each subagent runs with:
- Its own context window
- A custom system prompt
- Specific tool access
- Independent permissions

**Benefits:**
- **Preserve context** — keep exploration out of your main conversation
- **Enforce constraints** — limit which tools a subagent can use
- **Reuse configurations** — user-level subagents work across all projects
- **Specialize behavior** — focused system prompts for specific domains
- **Control costs** — route tasks to faster/cheaper models like Haiku

> Subagents work within a single session. For many independent sessions monitored from one place, see Agent View. For sessions that communicate with each other, see Agent Teams.

---

## Built-in Subagents

| Name | Model | Tools | When used |
|---|---|---|---|
| **Explore** | Haiku | Read-only | Codebase search and context gathering (skips CLAUDE.md) |
| **Plan** | Inherited | Read-only | Research during plan mode (prevents infinite nesting) |
| **General-purpose** | Inherited | All | Complex multi-step tasks requiring exploration + modification |
| `statusline-setup` | Sonnet | — | When you run `/statusline` |
| `claude-code-guide` | Haiku | — | When you ask questions about Claude Code features |

---

## Quickstart: Create Your First Subagent

1. Run `/agents`
2. Switch to **Library** tab → **Create new agent** → **Personal** (saves to `~/.claude/agents/`)
3. Select **Generate with Claude** → describe the subagent
4. Select tools (e.g. Read-only for a reviewer)
5. Choose model (Sonnet balances capability and speed)
6. Pick a display color
7. Configure memory (User scope = persistent learning across conversations)
8. Press `s` or `Enter` to save; `e` to save and open in editor

---

## Subagent Scope and Priority

| Location | Scope | Priority |
|---|---|---|
| Managed settings | Organization-wide | 1 (highest) |
| `--agents` CLI flag | Current session only | 2 |
| `.claude/agents/` | Current project | 3 |
| `~/.claude/agents/` | All your projects | 4 |
| Plugin `agents/` directory | Where plugin is enabled | 5 (lowest) |

When names collide, higher-priority location wins.

- **Project subagents** (`.claude/agents/`) — check into version control for team sharing
- **User subagents** (`~/.claude/agents/`) — personal, available in all projects
- Both scopes are scanned recursively (organize into subdirectories)
- **`name` frontmatter is identity** — keep it unique across the whole tree

---

## Subagent File Format

```markdown
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Glob, Grep
model: sonnet
---

You are a code reviewer. When invoked, analyze the code and provide
specific, actionable feedback on quality, security, and best practices.
```

> Subagents are loaded at session start. If you edit a file on disk, restart your session to load it. Subagents created via `/agents` take effect immediately.

---

## All Frontmatter Fields

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Unique ID using lowercase + hyphens |
| `description` | Yes | When Claude should delegate to this subagent |
| `tools` | No | Tools the subagent can use; inherits all if omitted |
| `disallowedTools` | No | Tools to deny from inherited/specified list |
| `model` | No | `sonnet`, `opus`, `haiku`, full model ID, or `inherit` |
| `permissionMode` | No | `default`, `acceptEdits`, `auto`, `dontAsk`, `bypassPermissions`, `plan` |
| `maxTurns` | No | Max agentic turns before subagent stops |
| `skills` | No | Skills to preload into context at startup |
| `mcpServers` | No | MCP servers available to this subagent |
| `hooks` | No | Lifecycle hooks scoped to this subagent |
| `memory` | No | `user`, `project`, or `local` — enables cross-session learning |
| `background` | No | `true` to always run as background task |
| `effort` | No | `low`, `medium`, `high`, `xhigh`, `max` — overrides session effort |
| `isolation` | No | `worktree` — runs in isolated git worktree; auto-cleaned if no changes |
| `color` | No | `red`, `blue`, `green`, `yellow`, `purple`, `orange`, `pink`, `cyan` |
| `initialPrompt` | No | Auto-submitted as first user turn when agent runs as main session agent |

> Plugin subagents cannot use `hooks`, `mcpServers`, or `permissionMode` — those fields are ignored for security reasons.

---

## Model Resolution Order

1. `CLAUDE_CODE_SUBAGENT_MODEL` environment variable
2. Per-invocation `model` parameter from Claude
3. Subagent definition's `model` frontmatter
4. Main conversation's model

---

## CLI-Defined Subagents (`--agents` flag)

Session-only, not saved to disk. Useful for quick testing or automation:

```bash
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer. Use proactively after code changes.",
    "prompt": "You are a senior code reviewer. Focus on code quality, security, and best practices.",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  },
  "debugger": {
    "description": "Debugging specialist for errors and test failures.",
    "prompt": "You are an expert debugger. Analyze errors, identify root causes, and provide fixes."
  }
}'
```

---

## What Loads at Startup

- Project and user CLAUDE.md files
- MCP servers from project + user settings
- Skills from project + user settings
- The subagent's own system prompt (frontmatter body)
- Basic environment details (working directory)
- **Not** the full Claude Code system prompt
- **Not** the parent session's conversation history

`Explore` and `Plan` skip CLAUDE.md and git status to stay fast and cheap.

---

## Forking the Current Conversation

A forked subagent inherits your full conversation context instead of starting fresh. Useful when the subagent needs everything you've discussed. Different from a normal subagent which starts with only its system prompt.

---

## Using Subagents in Agent Teams

Subagent definitions (from any scope) are also available as teammate types in Agent Teams. When spawning a teammate, reference the subagent type and the teammate uses its `tools` and `model`, with the definition's body appended to the teammate's system prompt. Note: `skills` and `mcpServers` frontmatter fields are NOT applied when running as a teammate.
