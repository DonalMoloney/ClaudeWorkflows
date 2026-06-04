# ClaudeWorkflows

Reference repository of official Claude Code documentation on dynamic workflows, subagents, and parallel agent patterns. Pure markdown — no build, test, or lint commands.

## Purpose

Teach how to harness Claude Code's dynamic workflow system. Every file is a curated, formatted extract from official docs at `code.claude.com/docs/en/`.

## File conventions

- Name new reference files with a zero-padded numeric prefix matching the sequence: `04-official-docs-*.md`, `05-official-docs-*.md`, etc.
- Every file must include a `> Source:` line at the top citing the exact docs URL.
- `workflow.md` is the raw scraped source; the numbered files are the formatted references — prefer editing the numbered files, not `workflow.md`.

## Content rules

- Never invent API details, flag names, or behavior. Copy from the official docs only.
- Preserve the source's version caveats (e.g., "research preview", "requires v2.1.154+").
- Use tables for comparison information — the official docs use this pattern consistently.
- Do not add opinions or subjective guidance; this is a reference, not a tutorial.

## Plugin & skills

- `plugin/` contains a Claude Code plugin for this repo.
- `plugin/skills/parallel-patterns/SKILL.md` — the **Parallel Orchestration Patterns** skill: adversarial verify, loop-until-dry, judge panel, multi-modal sweep, completeness critic. Reference this when writing or reviewing workflow scripts that need to pick an orchestration pattern.
- When adding a new skill, place it under `plugin/skills/<kebab-name>/SKILL.md` following the same frontmatter schema (`name`, `description`, `version`).

## Restrictions

- Only create or add files inside this folder (`/Users/donalmoloney/PycharmProjects/ClaudeWorkflows`). Do not write or modify files outside this directory.

## Gotchas

- Dynamic workflows require Claude Code v2.1.154+ and must be enabled in `/config` for Pro users.
- The `ultracode` keyword triggers a workflow in prompts; natural language ("use a workflow") works from v2.1.160+.
- Workflow subagents always run in `acceptEdits` mode regardless of session permission mode.
- Resume only works within the same Claude Code session — exiting and re-entering starts fresh.
