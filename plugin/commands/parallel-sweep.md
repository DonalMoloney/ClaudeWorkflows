---
name: parallel-sweep
description: Fan out N specialized sub-agents across a codebase or file list simultaneously. Each sub-agent works in an isolated context window, then findings are adversarially verified and synthesized into a single report.
version: 1.0.0
---

When the user runs `/parallel-sweep <target> [focus]`:

## Example

`/parallel-sweep src/ security`

→ Discovers the files under `src/`, fans out one agent per chunk to hunt auth gaps / injection / exposed secrets, adversarially verifies the high-severity hits, and returns one ranked report. Swap `security` for `coverage`, `performance`, or `dead-code` to change the focus.

## The core architecture

```
Orchestrator (this session)
  ├── Discovers work items
  ├── Chunks into 5–20 groups
  └── Fans out via pipeline()
         ↓
Sub-agents (each is a fresh Claude Code session)
  ├── Isolated context window
  ├── Only receives its assigned chunk
  ├── Returns structured JSON summary
  └── Never sees other sub-agents' results
         ↓
Verifier agents (adversarial pass)
  ├── Each tries to REFUTE a finding
  ├── Finding survives only if majority confirm
  └── Eliminates false positives before reporting
         ↓
Synthesizer — final structured report
```

Each sub-agent does NOT share memory or state with other sub-agents unless explicitly passed in the task prompt.

## Step 1 — Discover the work surface

List all relevant files/modules in target. Group into logical chunks (by directory, feature area, or file count). Target 5–20 chunks — fewer loses parallelism, more wastes spawn overhead.

## Step 2 — Define focus

If not specified, ask: what are we looking for?
- Security: auth gaps, injection risks, exposed secrets
- Coverage: untested branches, missing edge cases
- Performance: N+1 queries, blocking I/O, unbounded loops
- Dead code: unreachable paths, unused exports

## Step 3 — Fan out and verify

Produce a workflow script that:
1. Scans each chunk with a focused sub-agent
2. Adversarially verifies high-severity findings
3. Synthesizes into a structured report with severity ratings

## Step 4 — Report format

```
CRITICAL  (survived adversarial verify)
  • Finding, file:line, confidence, suggested fix

WARNINGS  (single-agent finding, unverified)
  • Finding, file, why unverified

STATS
  Files scanned: N | Agents spawned: N | Tokens: N
```

## Shell-level alternative (no workflow runtime)

For simpler sweeps, use headless `claude -p` parallelism directly:

```bash
# Fan out 8 parallel claude sessions over TypeScript files
find src/ -name "*.ts" | xargs -P 8 -I{} \
  claude -p "Review {} for missing error handling. Respond as JSON: {\"file\":\"{}\",\"issues\":[]}" \
  --output-format json 2>/dev/null >> sweep-results.jsonl

# Aggregate results
jq -s '[.[] | select(.issues | length > 0)]' sweep-results.jsonl
```

Each `claude -p` is a fully isolated session with its own context budget. Trades the structured resume/verification of the workflow runtime for scripting simplicity.
