---
name: orchestrate
description: Design and launch a dynamic workflow for a complex task. Claude writes a JavaScript orchestration script, fans it out across parallel agents, and synthesizes results — compressing sequential hours into minutes.
---

When the user runs `/orchestrate <task description>`, follow these steps.

## Example

`/orchestrate audit every API endpoint under src/routes for missing auth checks`

→ Claude designs a discover → execute → verify → synthesize workflow, shows you the script with each phase explained, and runs it across parallel agents once you add `ultracode` to your next message.

## Step 1 — Clarify scope (if not obvious)

Ask at most two questions:
1. What are the inputs? (paths, directories, questions, issue numbers)
2. What is the expected output? (audit report, migrated files, research synthesis)

If the task is large and scope is clear, skip straight to Step 2.

## Step 2 — Design the workflow phases

Every workflow should have:
- **Discover** — map the work surface (list files, enumerate issues, gather sources)
- **Execute** — fan out over discovered items in parallel
- **Verify** — adversarially check high-stakes findings before reporting
- **Synthesize** — produce the final deliverable

## Step 3 — Write the workflow script

Produce a valid Claude Code workflow script. Rules:
- Start with `export const meta = { name, description, phases }` as a pure literal (no computed values)
- Use `agent()`, `pipeline()`, `parallel()`, `phase()`, and `log()` as primitives
- Add `schema:` to `agent()` calls that must return structured data
- Never use `Date.now()`, `Math.random()`, or `new Date()` (breaks resume)
- Plain JavaScript only — no TypeScript

For detailed pattern templates — pipeline default, parallel barrier, adversarial verify, loop-until-dry, judge panel, multi-modal sweep — see the **parallel-patterns** skill. Use those templates directly in the script body.

## Step 4 — Show and launch

Present the script and explain each phase in one sentence. Then tell the user:
- Include `ultracode` in the next prompt to trigger the workflow runtime
- Or use `/effort ultracode` for session-wide workflow mode
- After a successful run, press `s` in `/workflows` to save it as a reusable command
