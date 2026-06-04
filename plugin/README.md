# parallel-workflows

A Claude Code plugin that helps you design, write, and run dynamic parallel workflows — compressing hours of sequential work into minutes.

## Install

```text
/plugin marketplace add DonalMoloney/ClaudeWorkflows
/plugin install parallel-workflows@claude-workflows
```

Requires Claude Code **v2.1.154+** (dynamic workflows). Pro users: enable workflows in `/config`.

## Components

**Commands** (slash commands in Claude Code):
- `/orchestrate <task>` — design and launch a dynamic workflow for any complex task
- `/parallel-sweep <target>` — fan out agents across a codebase and synthesize findings
- `/workflow-review <path>` — critique a workflow script for correctness and quality

**Agents** (specialist workers):
- `workflow-architect` — writes and debugs workflow orchestration scripts
- `sub-agent-specialist` — scopes sub-agent prompts, schemas, and model routing

**Skills** (auto-activating context):
- `dynamic-workflows` — when/how to trigger, all primitives, run management, troubleshooting
- `parallel-patterns` — adversarial verify, loop-until-dry, judge panel, multi-modal sweep
- `headless-orchestration` — shell-level parallelism with `claude -p`
- `error-handling` — agent failure handling, defensive schemas, `.filter(Boolean)` placement

## Typical flow

1. `/orchestrate` describes what you want → Claude writes a workflow script
2. Include `ultracode` in your next prompt to trigger the runtime
3. Watch progress in `/workflows`, drill into agents, pause/stop as needed
4. Press `s` in `/workflows` to save the script as a reusable command
