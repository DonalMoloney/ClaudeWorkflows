---
marp: true
theme: default
paginate: true
html: true
backgroundColor: #FAF9F5
color: #1C1917
style: |
  /* ── Anthropic brand palette ── */
  :root {
    --coral:      #D97757;
    --coral-light:#F5C4A8;
    --coral-dark: #A0451F;
    --cream:      #FAF9F5;
    --ink:        #1C1917;
    --ink-mid:    #44403C;
    --ink-soft:   #78716C;
    --warm-rule:  #E7D9CC;
    --code-bg:    #F0E5D8;
  }

  section {
    font-family: 'Georgia', serif;
    background: var(--cream);
    color: var(--ink);
    padding: 52px 64px;
  }

  /* headings */
  h1 { color: var(--coral); font-size: 2em; margin-bottom: 0.2em; }
  h2 {
    color: var(--coral-dark);
    font-size: 1.35em;
    border-bottom: 2px solid var(--coral);
    padding-bottom: 6px;
    margin-bottom: 0.6em;
  }
  h3 { color: var(--coral); font-size: 1.1em; }

  /* lead (dark) slides */
  section.lead {
    background: var(--ink);
    color: var(--cream);
    text-align: center;
    justify-content: center;
  }
  section.lead h1 { color: var(--coral); font-size: 2.4em; }
  section.lead h2 { color: var(--coral-light); border-color: var(--coral); }
  section.lead p  { color: #C8BDB5; }

  /* inline code + fenced blocks */
  code {
    background: var(--code-bg);
    color: var(--coral-dark);
    padding: 1px 6px;
    border-radius: 4px;
    font-size: 0.88em;
  }
  pre {
    background: #F5EDE4;
    border-left: 4px solid var(--coral);
    border-radius: 0 6px 6px 0;
    padding: 16px 20px;
  }
  pre code { background: transparent; color: var(--ink-mid); }

  /* tables */
  table { font-size: 0.82em; border-collapse: collapse; width: 100%; }
  th {
    background: var(--coral);
    color: white;
    padding: 7px 12px;
    text-align: left;
  }
  td { padding: 6px 12px; border-bottom: 1px solid var(--warm-rule); }
  tr:nth-child(even) td { background: #F7EEE6; }

  /* blockquote */
  blockquote {
    border-left: 4px solid var(--coral);
    background: #F7EEE6;
    padding: 12px 20px;
    margin: 12px 0;
    color: var(--ink-mid);
    font-style: italic;
  }

  /* lists */
  li { margin: 4px 0; }

  /* pagination */
  section::after { color: var(--ink-soft); font-size: 0.75em; }

  /* diagram utilities */
  .tree {
    font-family: 'Menlo', 'Courier New', monospace;
    font-size: 0.78em;
    line-height: 1.9;
    background: var(--ink);
    color: var(--cream);
    padding: 22px 28px;
    border-radius: 8px;
    border-left: 5px solid var(--coral);
  }
  .arch-row {
    display: flex;
    gap: 12px;
    margin: 10px 0;
    align-items: stretch;
  }
  .arch-box {
    flex: 1;
    background: var(--code-bg);
    border: 2px solid var(--warm-rule);
    border-radius: 8px;
    padding: 12px 14px;
    font-size: 0.78em;
  }
  .arch-box.highlight {
    border-color: var(--coral);
    background: #FDF3EC;
  }
  .arch-label {
    font-weight: bold;
    color: var(--coral-dark);
    display: block;
    margin-bottom: 4px;
    font-size: 1em;
  }
  .arch-sub { color: var(--ink-soft); font-size: 0.9em; }
  .arrow {
    display: flex;
    align-items: center;
    font-size: 1.4em;
    color: var(--coral);
    flex: 0 0 auto;
  }
  .tag {
    display: inline-block;
    background: var(--coral);
    color: white;
    border-radius: 4px;
    padding: 1px 8px;
    font-size: 0.75em;
    font-weight: bold;
    margin-right: 4px;
  }
---

<!-- _class: lead -->

# Multi-Agent Workflows in Claude

## From turn-by-turn conversations to orchestrated systems

How Claude Code evolved from a smart assistant into a programmable agent runtime

---

## The Question Has Changed

> **Before:** *"How do I prompt Claude to do this?"*

> **Now:** *"What execution model best fits the structure of this problem?"*

Multi-agent workflows aren't a feature — they're a **fundamentally different mental model** for how AI assists with complex work.

This deck covers:
- What Claude was doing before workflows existed
- The problems that made a new model necessary
- The four execution models Claude Code provides
- When to use which — and why it matters

---

<!-- _class: lead -->

# Part 1
## How Claude Was Already Working

*The implicit orchestration nobody talked about*

---

## Claude Was Always An Orchestrator

Even in a standard conversation, Claude was doing orchestration:

```
User: "Refactor the auth module and update the tests"

Claude's implicit execution:
  1. Read the auth module
  2. Understand what it does
  3. Plan the refactor
  4. Write the new code
  5. Read the test files
  6. Update the tests
  7. Check for consistency
```

**The difference:** all intermediate results lived in Claude's context window. Every step consumed tokens. Every step was visible to every subsequent step.

---

## Where the Single-Session Model Breaks

The context window is the bottleneck:

| Problem | What happens |
|---|---|
| **File count** | 500 files can't all fit and be reasoned about simultaneously |
| **Result accumulation** | Grep output, logs, tool calls pile up and crowd out reasoning |
| **Verification** | Claude can't independently verify its own findings |
| **Scale** | "Audit every endpoint" is qualitatively different from "audit this one" |
| **Repeatability** | The plan lives only in Claude's turn — no artifact, no rerun |

The single-session model is **great for bounded, sequential tasks.** It becomes a liability when a task is exploratory, large, or needs cross-checking.

---

<!-- _class: lead -->

# Part 2
## The Four Execution Models

*Each solves a different structural problem*

---

## The Four Models at a Glance

| Model | Coordination by | Plan lives in | Scale |
|---|---|---|---|
| **Subagents** | Claude, delegating per turn | Context window | A few per turn |
| **Agent View** | You, checking in | Your attention | Independent sessions |
| **Agent Teams** | Claude + teammates | Shared task list | 3–10 collaborating peers |
| **Dynamic Workflows** | A generated script | Script variables | Tens to hundreds |

These aren't levels of power — they're **different shapes** for different problems.

---

## Architecture of the Four Models

<div class="arch-row">
  <div class="arch-box">
    <span class="arch-label">🔹 Subagents</span>
    <span class="arch-sub">Worker spawned per turn. Returns summary only. Context-isolated.</span>
  </div>
  <div class="arch-box">
    <span class="arch-label">🔍 Agent View</span>
    <span class="arch-sub">Independent background sessions. You monitor and steer.</span>
  </div>
</div>
<div class="arch-row">
  <div class="arch-box">
    <span class="arch-label">🤝 Agent Teams</span>
    <span class="arch-sub">Peers with shared task list. Direct inter-agent messaging. Git workspace.</span>
  </div>
  <div class="arch-box highlight">
    <span class="arch-label">⚡ Dynamic Workflows</span>
    <span class="arch-sub">JavaScript script as orchestrator. Script variables hold results. Resumable. Up to 1,000 agents per run.</span>
  </div>
</div>
<div style="margin-top:14px; font-size:0.8em; color:#78716C;">
  ← simpler, cheaper, more deterministic &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; more powerful, more flexible, higher cost →
</div>

---

<!-- _class: lead -->

# Subagents
## Keeping your context clean

---

## What Subagents Actually Solve

**The problem:** a side task would flood your conversation with output you'll never reference again.

```
"Find all usages of the deprecated API across the codebase"

→ Without a subagent: 200 grep results in your context
→ With a subagent:    "Found 47 usages in 12 files" in your context
```

A subagent gets its own context window, does the work, and **returns only the summary.** The noise never reaches your main session.

**Built-in subagents you're already using:**
- `Explore` — read-only codebase search (Haiku model — fast + cheap)
- `Plan` — background context gathering during plan mode
- `General-purpose` — exploration + modification tasks

---

## Custom Subagents: Specialists You Define

```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities. Use when
  touching auth, session handling, input validation, or API boundaries.
tools: Read, Grep, Glob
model: sonnet
---

You are a security reviewer. Analyze code for OWASP Top 10,
injection risks, improper auth, and data exposure. Cite file:line.
```

Save to `.claude/agents/` — Claude reads the `description` to decide when to invoke it automatically.

**The `description` field is a routing rule, not documentation.** Precision here determines how reliably Claude delegates to this agent.

---

## When Subagents Excel

<span class="tag">✓</span> **Repeatable tasks** — define once, invoke on every PR  
<span class="tag">✓</span> **Cost-predictable execution** — same prompt, model, and tools every time  
<span class="tag">✓</span> **Context isolation** — offload long reads to keep the main session focused  
<span class="tag">✓</span> **Least-privilege enforcement** — a reviewer with no `Write` tool *cannot* edit files  
<span class="tag">✓</span> **Parallelising independent tasks** — multiple subagents running concurrently per turn

```
Code review enforcing a specific style guide  → define it, reuse it
Test generation against known frameworks      → deterministic, auditable
Documentation updates on fixed templates      → consistent every time
```

---

<!-- _class: lead -->

# Agent Teams
## Multiple Claude instances collaborating

---

## Subagents vs. Agent Teams: The Key Difference

| | Subagents | Agent Teams |
|---|---|---|
| **Independence** | Fully isolated, no inter-communication | Aware of each other's progress |
| **Context** | Results summarised back to caller | Each teammate has its own full context |
| **Coordination** | Main agent manages all work | Shared task list, teammates self-coordinate |
| **Persistence** | Disappear after task completes | Keep running, claim new work |
| **File access** | Sequential by design | Partition files to avoid conflicts |

**Subagents** receive a task, complete it, disappear.  
**Teammates** receive work, produce output, observe each other's changes, and adapt.

---

## What Agent Teams Enable

A backend teammate can **know when the frontend teammate changes an API contract** — because they share a git workspace and task list.

```
"Create a team with 4 teammates to review PR #142:
 - One focused on security implications
 - One checking performance impact
 - One validating test coverage
 - One as devil's advocate"
```

The lead Claude:
1. Spawns teammates with scoped tasks
2. Shares task list with dependency tracking
3. Merges findings as teammates complete work
4. Manages shutdown and cleanup

---

<!-- _class: lead -->

# Dynamic Workflows
## The paradigm shift

---

## The Fundamental Change

**Standard Claude Code:**
> Claude IS the orchestrator — making turn-by-turn decisions whose intermediate results accumulate in the context window.

**Dynamic Workflows:**
> A generated JavaScript script IS the orchestrator. Claude's context holds only the final answer.

```
ultracode: audit every API endpoint in src/routes/ for missing auth checks
```

Claude writes a script. The runtime executes it. Dozens of subagents run in parallel. Results are verified against each other. Your session stays responsive throughout.

---

## What the Script Enables

```javascript
const files = await agent("Find all route files", { schema: FILES_SCHEMA })

const audits = await pipeline(
  files.routes,
  file  => agent(`Audit ${file} for auth checks`, { schema: AUDIT_SCHEMA }),
  audit => agent(`Adversarially verify: ${audit.finding}`, { schema: VERDICT })
)

const confirmed = audits.filter(a => a.verdict.isReal)
return { confirmed, total: files.routes.length }
```

**Intermediate results live in script variables — not Claude's context.**  
The context-window bottleneck is eliminated.

---

## Adversarial Verification: The Killer Feature

Standard Claude can't independently verify its own findings. Workflows can:

```javascript
// Three independent agents each try to REFUTE a finding
const votes = await parallel(Array.from({ length: 3 }, () => () =>
  agent(`Try to refute: ${claim}. Default refuted=true if uncertain.`)
))

// Finding survives only if ≥ 2 of 3 agents fail to refute it
const survives = votes.filter(v => !v.refuted).length >= 2
```

`/deep-research` uses this pattern: fans out across search angles, cross-checks sources, votes internally, and **returns only adversarially-tested findings** — not the first plausible answer.

---

## The Three Workflow Primitives

| Primitive | Use when |
|---|---|
| `pipeline(items, stage1, stage2)` | Multi-stage work — item A can be in stage 3 while item B is in stage 1. **Default choice.** |
| `parallel(thunks)` | Genuine barrier needed — all of stage N must finish before stage N+1 can start |
| `agent(prompt, opts)` | Single subagent — add `schema` for validated structured output |

**Wall-clock = slowest single-item chain** in a pipeline, not sum of stages.  
Most workflows should default to `pipeline()`.

---

<!-- _class: lead -->

# Part 3
## The Decision Framework

*Which model fits your problem?*

---

## Three Questions to Pick the Right Model

**1. Who holds the plan?**
- You direct Claude turn by turn → **Subagents**
- You hand off independent sessions → **Agent View**
- Claude coordinates workers with a shared list → **Agent Teams**
- A script holds the orchestration logic → **Dynamic Workflows**

**2. Do workers need to communicate?**
- Results only → Subagents / Agent View / Workflows
- Direct messaging, shared state → **Agent Teams**

**3. Is the split strategy known in advance?**
- Yes → **Custom Subagents** (repeatable, cost-consistent)
- No → **Dynamic Workflows** (script discovers the partition)

---

## The Decision Matrix

| Situation | Best choice |
|---|---|
| Side task would flood my context | **Subagents** |
| Several independent long-running tasks | **Agent View** |
| Repeatable task, cost consistency matters | **Custom Subagents** |
| Complex interdependent work across layers | **Agent Teams** |
| Exceeds context, split unknown, quality > cost | **Dynamic Workflows** |
| Codebase-wide audit / 500-file migration | **Dynamic Workflows** |
| Exploratory bug hunt in unfamiliar codebase | **Dynamic Workflows** |

---

## When NOT to Use Dynamic Workflows

Workflows are not always the right answer.

❌ **Repeatable, well-defined tasks with predictable token budgets**  
→ Custom subagents are cheaper and deterministic

❌ **Small, bounded tasks needing a single agent pass**  
→ Script overhead isn't worth it

❌ **Known workflows where exact execution consistency matters**  
→ A custom subagent is more auditable; the script adds variability

**The rule:** use workflows when all three are true:
1. Task exceeds a single context window
2. The split strategy isn't known in advance
3. Quality matters more than token economy

---

<!-- _class: lead -->

# Part 4
## This Project's Structure

*How ClaudeWorkflows is organised*

---

## Project Structure

<div class="tree">
<span style="color:#D97757;font-weight:bold">ClaudeWorkflows/</span>
├── <span style="color:#FAF9F5">CLAUDE.md</span>  <span style="color:#78716C">· project rules and file conventions</span>
├── <span style="color:#FAF9F5">workflow.md</span>  <span style="color:#78716C">· raw scraped source</span>
├── <span style="color:#D97757">01–08-official-docs-*.md</span>  <span style="color:#78716C">· curated, formatted reference extracts</span>
├── <span style="color:#FAF9F5">claude-code-parallel-agents-reference.md</span>
│
├── <span style="color:#D97757">plugin/</span>
│   ├── <span style="color:#A8D8A8">agents/</span>  <span style="color:#78716C">workflow-architect · sub-agent-specialist</span>
│   ├── <span style="color:#A8D8A8">commands/</span>  <span style="color:#78716C">orchestrate · parallel-sweep</span>
│   └── <span style="color:#A8D8A8">skills/</span>  <span style="color:#78716C">dynamic-workflows · parallel-patterns · headless-orchestration</span>
│
├── <span style="color:#D97757">.claude/agents/</span>  <span style="color:#78716C">← project-scoped session agents</span>
│   ├── <span style="color:#F5C4A8">analyze.md</span>  <span style="color:#78716C">· audit code and docs</span>
│   ├── <span style="color:#F5C4A8">planning.md</span>  <span style="color:#78716C">· create implementation plans</span>
│   ├── <span style="color:#F5C4A8">html-css.md</span>  <span style="color:#78716C">· write and review HTML/CSS</span>
│   └── <span style="color:#F5C4A8">marp.md</span>  <span style="color:#78716C">· generate slide decks</span>
│
└── <span style="color:#D97757">presentations/</span>
    └── <span style="color:#F5C4A8">multi-agent-workflows.md</span>  <span style="color:#78716C">← you are here</span>
</div>

---

## How the Pieces Connect

<div class="arch-row">
  <div class="arch-box highlight" style="flex:2">
    <span class="arch-label">Reference Docs (01–08)</span>
    <span class="arch-sub">Official Claude Code documentation, curated and formatted. Never invented — always cited from <code>code.claude.com</code>.</span>
  </div>
  <div class="arrow">→</div>
  <div class="arch-box" style="flex:1">
    <span class="arch-label">Plugin</span>
    <span class="arch-sub">Skills, agents, and commands that apply the patterns from the docs.</span>
  </div>
</div>
<div class="arch-row">
  <div class="arch-box" style="flex:1">
    <span class="arch-label">.claude/agents/</span>
    <span class="arch-sub">Session-level specialists: <strong>analyze</strong>, <strong>planning</strong>, <strong>html-css</strong>, <strong>marp</strong>. Available in every Claude Code session in this project.</span>
  </div>
  <div class="arrow">→</div>
  <div class="arch-box highlight" style="flex:2">
    <span class="arch-label">presentations/</span>
    <span class="arch-sub">Human-readable synthesis of the docs. Generated by the <strong>marp</strong> agent, sourced from the reference docs. No invented content.</span>
  </div>
</div>

---

<!-- _class: lead -->

# Part 5
## Patterns That Work

*What orchestration looks like in practice*

---

## Pattern: Understand → Change → Verify

The canonical multi-stage engineering workflow:

```javascript
// Stage 1: map the architecture
const map = await agent("Map all modules touching auth", { schema: MAP_SCHEMA })

// Stage 2: execute changes in parallel, each in an isolated worktree
const changes = await pipeline(
  map.files,
  file => agent(`Refactor auth in ${file}`, { isolation: 'worktree' })
)

// Stage 3: independent verification
const verified = await parallel(changes.map(c => () =>
  agent(`Verify ${c.file} is correct after refactor`, { schema: VERDICT })
))
```

With `/effort ultracode`, Claude plans and runs this loop automatically.

---

## Pattern: Loop Until Dry

For discovery tasks where the total scope is unknown:

```javascript
const bugs = [], seen = new Set()
let dry = 0

while (dry < 2) {
  const found = await agent("Find bugs not yet in seen set", { schema: BUGS_SCHEMA })
  const fresh = found.bugs.filter(b => !seen.has(b.id))

  if (!fresh.length) { dry++; continue }
  dry = 0
  fresh.forEach(b => seen.add(b.id))
  bugs.push(...fresh)
}
```

Runs until **two consecutive rounds find nothing new** — not just until a count is hit. This catches the tail that `while (count < N)` misses.

---

## Pattern: Multi-Dimensional Review

Different failure modes need different lenses:

```javascript
const DIMENSIONS = [
  { key: 'bugs',     prompt: 'Find logic errors and crashes' },
  { key: 'security', prompt: 'Find OWASP Top 10 vulnerabilities' },
  { key: 'perf',     prompt: 'Find N+1 queries and memory leaks' },
]

const results = await pipeline(
  DIMENSIONS,
  d      => agent(d.prompt, { label: `review:${d.key}`, schema: FINDINGS }),
  review => parallel(review.findings.map(f => () =>
    agent(`Adversarially verify: ${f.title}`, { schema: VERDICT })
  ))
)
// 'bugs' dimension verifies while 'security' is still reviewing — no wasted time.
```

---

<!-- _class: lead -->

# Part 6
## Getting Started

*From zero to your first workflow*

---

## Four Entry Points

**1. Bundled workflow**
```
/deep-research What changed in the Node.js permission model between v20 and v22?
```

**2. Keyword trigger**
```
ultracode: audit every API endpoint under src/routes/ for missing auth checks
```

**3. Natural language (v2.1.160+)**
```
Use a workflow to migrate all fetch() calls to the new HTTP client.
```

**4. Session-level mode**
```
/effort ultracode
```
Claude evaluates every request and deploys a workflow where appropriate.

---

## Create Your First Custom Subagent

Via the UI: `/agents → Library → Create new agent → Personal`

Or write the file directly in `.claude/agents/`:

```markdown
---
name: my-reviewer
description: Reviews pull requests for style and correctness.
  Use when asked to review a PR or check recent changes.
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are a code reviewer. Analyze recent changes for:
- Logic errors and edge cases
- Style guide violations
- Missing tests

Report findings grouped by severity with file:line citations.
```

Restart the session to load it (or use `/agents` to load it immediately).

---

## Cost and Safety

| Practice | Why |
|---|---|
| Test on a small slice first | `one directory` before `entire repo` |
| Watch `/workflows` live | Per-agent token usage; stop without losing completed work |
| Add shell/MCP tools to allowlist before long runs | Mid-run permission prompts can block progress |
| Use `model: haiku` for cheap stages | Search and exploration don't need the strongest model |
| Set `isolation: worktree` for parallel file edits | Prevents agents conflicting on the same files |

Workflow subagents always run in `acceptEdits` mode — file edits are auto-approved, but shell commands outside your allowlist can still prompt.

---

<!-- _class: lead -->

# Summary

---

## What We Covered

**The old model:** Claude as orchestrator, context window as the bottleneck, plan lost at session end.

**The new model:** four execution shapes, each solving a different structural problem.

| Shape | Solves |
|---|---|
| **Subagents** | Context flooding, task isolation, least-privilege |
| **Agent Teams** | Interdependent parallel work with inter-agent awareness |
| **Dynamic Workflows** | Scale, adversarial verification, resumability |
| **Agent View** | Independent long-running sessions you monitor |

The shift isn't about using more AI — it's about choosing the **execution model that fits the structure of your problem**.

---

## The Question to Ask First

> **Does this task have a known partition strategy and a predictable scope?**

<div class="arch-row" style="margin-top:16px">
  <div class="arch-box highlight">
    <span class="arch-label">Yes</span>
    <span class="arch-sub"><strong>Custom subagents.</strong> Repeatable, auditable, cost-consistent.</span>
  </div>
  <div class="arch-box">
    <span class="arch-label">No — but bounded</span>
    <span class="arch-sub"><strong>Agent Teams.</strong> Coordinate specialists over shared artifacts.</span>
  </div>
  <div class="arch-box">
    <span class="arch-label">No — and large</span>
    <span class="arch-sub"><strong>Dynamic Workflows.</strong> Let the script discover the partition.</span>
  </div>
</div>

<br>

> The tool that helps you most isn't always the most powerful one. It's the one that **fits the problem's shape**.

---

<!-- _class: lead -->

# Thank you

**Further reading**

`code.claude.com/docs/en/workflows`  
`code.claude.com/docs/en/agents`  
`code.claude.com/docs/en/sub-agents`  
`code.claude.com/docs/en/agent-teams`

*Sourced entirely from official Claude Code documentation and the ClaudeWorkflows reference repository.*
