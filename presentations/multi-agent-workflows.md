---
marp: true
theme: default
paginate: true
html: true
backgroundColor: #faf9f5
color: #141413
style: |
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&family=Lora:ital,wght@0,400;0,500;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap');

  /* ── Exact Anthropic brand palette ── */
  :root {
    --dark:       #141413;
    --light:      #faf9f5;
    --mid-gray:   #b0aea5;
    --light-gray: #e8e6dc;
    --orange:     #d97757;   /* primary accent */
    --blue:       #6a9bcc;   /* secondary accent */
    --green:      #788c5d;   /* tertiary accent */
    --orange-mute: rgba(217,119,87,0.12);
    --blue-mute:   rgba(106,155,204,0.12);
    --green-mute:  rgba(120,140,93,0.12);
    --ink:        #1A1208;   /* warm near-black */
    --ink-mid:    #3D2E1E;
    --cream:      #F5F0E8;   /* Anthropic off-white */
    --cream-warm: #EDE5D4;
    --muted:      #8C7360;
    --rule:       #DDD3C0;
  }

  /* ── DEFAULT SLIDE (cream) ── */
  section {
    font-family: 'DM Sans', sans-serif;
    font-weight: 400;
    background: var(--cream);
    color: var(--ink);
    padding: 60px 80px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  /* remove default left-bar from previous version */
  section::before { display: none; }

  /* subtle page number */
  section::after {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.62em;
    color: var(--muted);
    letter-spacing: 0.08em;
  }

  /* ── HEADINGS ── */
  h1 {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 2.05em;
    color: var(--coral);
    letter-spacing: -0.025em;
    line-height: 1.1;
    margin-bottom: 0.25em;
    border: none;
  }

  h2 {
    font-family: 'DM Sans', sans-serif;
    font-weight: 400;
    font-size: 1.1em;
    color: var(--ink-mid);
    letter-spacing: 0.01em;
    margin-bottom: 1em;
    border: none;
    padding: 0;
  }
  h2::after { display: none; }

  h3 {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.8em;
    color: var(--coral);
    text-transform: uppercase;
    letter-spacing: 0.18em;
    margin-bottom: 0.5em;
  }

  p  { color: var(--ink-mid); line-height: 1.65; }
  li { color: var(--ink-mid); margin: 0.3em 0; line-height: 1.55; }
  strong { color: var(--ink); font-weight: 600; }

  blockquote {
    border-left: 3px solid var(--coral);
    background: var(--coral-mute);
    padding: 14px 20px;
    margin: 14px 0;
    color: var(--ink);
    font-style: normal;
    border-radius: 0 6px 6px 0;
  }
  blockquote strong { color: var(--coral); }

  /* ── CODE ── */
  code {
    font-family: 'JetBrains Mono', monospace;
    background: var(--cream-warm);
    color: var(--coral-dark);
    padding: 2px 7px;
    border-radius: 4px;
    font-size: 0.82em;
  }
  pre {
    background: var(--ink);
    border-radius: 8px;
    padding: 20px 24px;
    font-size: 0.72em;
    line-height: 1.6;
    border: none;
  }
  pre code {
    background: transparent;
    color: #D8CDBE;
    padding: 0;
    font-size: 1em;
  }

  /* ── TABLES ── */
  table { font-size: 0.8em; border-collapse: collapse; width: 100%; }
  th {
    background: var(--coral);
    color: var(--cream);
    padding: 9px 16px;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    letter-spacing: 0.04em;
    text-align: left;
    border: none;
  }
  td {
    padding: 7px 16px;
    border-bottom: 1px solid var(--rule);
    color: var(--ink-mid);
  }
  tr:last-child td { border-bottom: none; }
  tr:nth-child(even) td { background: rgba(203,107,81,0.04); }

  /* ── IMPACT SLIDES — coral bg, ink text, like the brand image ── */
  section.impact {
    background: var(--coral);
    color: var(--ink);
    justify-content: center;
    align-items: flex-start;
  }
  section.impact::before { display: none; }
  section.impact::after  { color: rgba(26,18,8,0.35); }
  section.impact h1 {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 3.6em;
    color: var(--ink);
    letter-spacing: -0.04em;
    line-height: 0.97;
    margin-bottom: 0.3em;
  }
  section.impact h2 {
    color: rgba(26,18,8,0.55);
    font-size: 1em;
    margin-top: 0.6em;
  }
  section.impact p, section.impact li { color: rgba(26,18,8,0.7); }

  /* ── LEAD (dark break slides) ── */
  section.lead {
    background: var(--ink);
    color: var(--cream);
    justify-content: center;
    align-items: flex-start;
  }
  section.lead::after { color: rgba(245,240,232,0.25); }
  section.lead h1 {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 2.6em;
    color: var(--cream);
    letter-spacing: -0.03em;
    line-height: 1.05;
  }
  section.lead h2 {
    color: var(--coral);
    font-size: 1em;
    letter-spacing: 0.03em;
    margin-top: 0.4em;
  }
  section.lead p, section.lead li { color: rgba(245,240,232,0.65); }
  section.lead strong { color: var(--cream); }
  section.lead td { color: rgba(245,240,232,0.75); border-color: rgba(245,240,232,0.1); }
  section.lead th { background: var(--coral); }
  section.lead code {
    background: rgba(245,240,232,0.1);
    color: var(--coral);
  }
  section.lead pre {
    background: rgba(245,240,232,0.06);
    border: 1px solid rgba(245,240,232,0.1);
  }
  section.lead pre code { color: #D8CDBE; background: transparent; }
  section.lead blockquote {
    border-color: var(--coral);
    background: rgba(203,107,81,0.15);
    color: var(--cream);
  }

  /* ── UTILITY CLASSES ── */
  .kicker {
    font-family: 'Syne', sans-serif;
    font-size: 0.66em;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--coral);
    display: block;
    margin-bottom: 10px;
  }

  .tag {
    display: inline-block;
    background: var(--coral);
    color: var(--cream);
    border-radius: 3px;
    padding: 1px 9px;
    font-size: 0.7em;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    letter-spacing: 0.06em;
    margin-right: 5px;
    vertical-align: middle;
  }

  .tree {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72em;
    line-height: 1.85;
    background: var(--ink);
    color: #B8A896;
    padding: 22px 28px;
    border-radius: 8px;
  }

  .arch-row {
    display: flex; gap: 10px;
    margin: 8px 0; align-items: stretch;
  }
  .arch-box {
    flex: 1;
    background: var(--cream-warm);
    border: 1px solid var(--rule);
    border-radius: 6px;
    padding: 12px 16px; font-size: 0.77em;
  }
  .arch-box.hi {
    border-color: var(--coral);
    background: rgba(203,107,81,0.08);
  }
  .arch-label {
    font-family: 'Syne', sans-serif;
    font-weight: 700; font-size: 0.95em;
    color: var(--ink); display: block; margin-bottom: 4px;
  }
  .arch-box.hi .arch-label { color: var(--coral); }
  .arch-sub { color: var(--muted); font-size: 0.9em; line-height: 1.45; }
  .arrow {
    display: flex; align-items: center;
    color: var(--coral); font-size: 1.3em;
    flex: 0 0 auto; padding: 0 4px;
  }

  /* lead variants of arch boxes */
  section.lead .arch-box {
    background: rgba(245,240,232,0.06);
    border-color: rgba(245,240,232,0.12);
  }
  section.lead .arch-box.hi {
    background: rgba(203,107,81,0.2);
    border-color: var(--coral);
  }
  section.lead .arch-label { color: var(--cream); }
  section.lead .arch-box.hi .arch-label { color: var(--coral); }
  section.lead .arch-sub { color: rgba(245,240,232,0.55); }
---

<!-- _class: impact -->

# Multi-Agent<br>Workflows<br>in Claude

## From turn-by-turn conversations to orchestrated systems

<!-- Speaker notes:
OPEN COLD. Don't introduce yourself. Let the visual hold.
"A year ago, you prompted Claude. Today, you choose an execution model."
The shift from prompting skill to architectural thinking is the thesis of everything that follows.
Pause three full seconds before speaking.
-->

---

## The Question Has Changed

<span class="kicker">The shift</span>

> **Before:** "How do I prompt Claude to do this?"

> **Now:** "What execution model best fits the structure of this problem?"

Multi-agent workflows aren't a feature update. They're a new mental model for what AI assistance means at scale.

**This deck answers:**
- What Claude was already doing before workflows existed
- Where that model breaks — and why
- The four execution models and what each unlocks
- How to choose the right one for any problem

<!-- Speaker notes:
HOOK — If time is short, this is your starting slide.
The old question was about prompting skill. The new question is about architecture.
Same shift developers made from writing scripts to designing systems.
Claude is no longer just a tool you invoke — it's a runtime you orchestrate.
-->

---

<!-- _class: lead -->

# Part 1
## How Claude was already working

---

## Claude Was Always An Orchestrator

<span class="kicker">The hidden pattern</span>

Even a standard chat session runs implicit orchestration:

```
User: "Refactor the auth module and update the tests"

Claude's hidden steps:
  1. Read the auth module    → accumulates in context
  2. Understand it           → reasons over context
  3. Plan the refactor       → reasons over context
  4. Write new code          → writes into context
  5. Read test files         → MORE context consumed
  6. Update tests            → writes into context
  7. Check consistency       → reasons over ALL of the above
```

Every step consumed the same window. Every result accumulated. No isolation, no parallelism, no independent verification.

<!-- Speaker notes:
REFRAME — Help the audience see something familiar in a new way.
"You've been watching orchestration happen without knowing it. Claude was always the orchestrator."
"The question is whether that plan lives in Claude's context — or in code."
This is the seed for the paradigm shift coming in Part 4.
-->

---

## The Context Window Is the Bottleneck

<span class="kicker">Where it breaks</span>

| Failure mode | What actually happens |
|---|---|
| **Scale** | 500 files can't all be read and reasoned about at once |
| **Accumulation** | Grep output, logs, tool calls crowd out reasoning space |
| **Self-verification** | Claude cannot independently verify its own findings |
| **Repeatability** | The plan lives in a turn — no artifact, no rerun |
| **Cost** | Every intermediate result burns tokens you never see again |

The single-session model excels at **bounded, sequential work.**  
It collapses under exploratory, large-scale, or verification-critical tasks.

<!-- Speaker notes:
TENSION — Let the audience feel the pain points.
"Raise your hand if Claude's context filled up mid-task."
"Or you ran a review, re-ran it, and got different results."
These aren't prompt engineering failures — they're structural failures.
The solution isn't better prompts. It's a different execution model.
-->

---

<!-- _class: lead -->

# Part 2
## The four execution models

---

## Four Models. One Framework.

<span class="kicker">The decision space</span>

| Model | Plan lives in | Coordination by | Scale |
|---|---|---|---|
| **Subagents** | Claude's context | Claude, per turn | A few per turn |
| **Agent View** | Your attention | You, checking in | Independent sessions |
| **Agent Teams** | Shared task list | Claude + teammates | 3–10 collaborating peers |
| **Dynamic Workflows** | Script variables | A generated JS script | Tens to hundreds |

These are not **levels.** They are **shapes** — each one fits a different problem structure.

<!-- Speaker notes:
CLARITY — The insight is where the plan lives.
"In subagents, Claude holds the plan in its head. In workflows, the plan is code."
"Code is inspectable, resumable, version-controllable. Claude's context is not."
Spend 30 seconds on the last row. That's where the talk is going.
-->

---

## Architecture of the Four Models

<div class="arch-row">
  <div class="arch-box">
    <span class="arch-label">Subagents</span>
    <span class="arch-sub">Worker spawned per turn. Returns a summary only. The caller never sees intermediate noise.</span>
  </div>
  <div class="arch-box">
    <span class="arch-label">Agent View</span>
    <span class="arch-sub">Independent background sessions. You dispatch, monitor, and step in when needed.</span>
  </div>
</div>
<div class="arch-row" style="margin-top:10px">
  <div class="arch-box">
    <span class="arch-label">Agent Teams</span>
    <span class="arch-sub">Peers with a shared task list. Direct inter-agent messaging. Aware of each other's progress.</span>
  </div>
  <div class="arch-box hi">
    <span class="arch-label">Dynamic Workflows</span>
    <span class="arch-sub">JS script as orchestrator. Results in variables, not context. Resumable. Up to <strong>1,000 agents</strong> per run.</span>
  </div>
</div>
<p style="margin-top:16px; font-size:0.72em; color:var(--muted); font-family:'JetBrains Mono',monospace; letter-spacing:0.04em;">← simpler · cheaper · deterministic &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; more powerful · adaptive · higher cost →</p>

<!-- Speaker notes:
VISUAL — Let the diagram breathe.
"Think of these as four different organisational structures — not power levels."
"Subagents: solo dev with assistants. Workflows: a coordinated factory floor."
The highlighted Workflows box is intentional — that's the headline of this talk.
-->

---

<!-- _class: impact -->

# Your context.<br>Protected.

## Subagents keep exploration out of your main session

<!-- Speaker notes:
IMPACT BEAT — Pause fully after this slide loads.
"Every codebase search returns hundreds of lines. Without subagents, all of it hits your context."
"With subagents, only the answer does."
The simplest and most immediately useful primitive. Start here.
-->

---

## What Subagents Actually Solve

<span class="kicker">Context isolation</span>

```
"Find all usages of the deprecated API across the codebase"

→ Without a subagent:  200 grep results poured into your context
→ With a subagent:     "Found 47 usages in 12 files" — that's it
```

A subagent gets its own context window, does the work, and **returns only the summary.**

| Built-in agent | Model | What it protects you from |
|---|---|---|
| `Explore` | Haiku | Search result floods (read-only) |
| `Plan` | Inherited | Context pollution during planning |
| `General-purpose` | Inherited | Long exploration + modification runs |

<!-- Speaker notes:
PRACTICAL — Weight this slide.
"Explore runs on Haiku. Fast, cheap, reads 40 files, sends you a summary."
"Your main session never saw those 40 files. Your reasoning stays clean."
This is the entry point for most teams. Easiest win.
-->

---

## Custom Subagents: Specialists You Define

<span class="kicker">Reusable roles</span>

```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities. Use when touching
  auth, session handling, input validation, or API boundaries.
tools: Read, Grep, Glob
model: sonnet
---

You are a security reviewer. Analyze code for OWASP Top 10,
injection risks, improper auth, and data exposure. Cite file:line.
```

The **`description` field is a routing rule** — Claude reads it to decide when to invoke the agent automatically. Precision here determines how reliably delegation happens.

<!-- Speaker notes:
KEY INSIGHT — The description is a trigger condition, not a docstring.
"Write it like a conditional: invoke this when X."
"Notice the tools list: no Write. That's enforcement, not an oversight."
A reviewer that cannot write cannot accidentally touch production code.
-->

---

## When Subagents Excel

<span class="kicker">The right jobs</span>

<span class="tag">✓</span> **Repeatable tasks** — define once, invoke on every PR  
<span class="tag">✓</span> **Predictable cost** — same prompt, model, tools every time  
<span class="tag">✓</span> **Context isolation** — offload long reads, keep sessions clean  
<span class="tag">✓</span> **Least-privilege** — a reviewer with no `Write` tool *cannot* edit files  
<span class="tag">✓</span> **Parallelism** — multiple subagents running concurrently per turn

```
Code review enforcing a team style guide  →  define once, invoke on every PR
Test generation against known frameworks  →  deterministic, auditable output
Documentation on fixed templates          →  consistent across contributors
```

<!-- Speaker notes:
ACTIONABLE — First concrete takeaway.
"If you do nothing else from this talk, write one custom subagent."
"Pick your most repeated review task. Give it focused tools. Ship it."
"You'll never run that task manually again — and it'll never miss a step."
-->

---

<!-- _class: impact -->

# Specialists<br>who talk<br>to each other.

## Agent Teams — when the work is truly collaborative

<!-- Speaker notes:
SHIFT — Mark the transition.
"Subagents are great when tasks are independent. But some work isn't."
"A feature spanning frontend and backend — those agents need to be aware of each other."
That's Agent Teams.
-->

---

## Subagents vs. Agent Teams

<span class="kicker">The critical difference</span>

| | Subagents | Agent Teams |
|---|---|---|
| **Awareness** | Fully isolated | Each teammate watches others' progress |
| **Communication** | Results back to caller only | Direct messaging between any teammates |
| **Coordination** | Main agent manages all | Shared task list — teammates self-claim |
| **Persistence** | Disappear when done | Stay running, claim next available task |
| **File access** | Sequential by design | Partition files strictly to avoid conflicts |

> A backend teammate can *know* when the frontend teammate changes an API contract — because they share a git workspace.

<!-- Speaker notes:
CONTRAST — Make the table visceral.
"Every row represents a different class of problem."
"Subagents are contractors who report in. Teammates are colleagues who collaborate."
"The git workspace is the key — that's how they share state without stepping on each other."
Best use: a PR that touches backend, frontend, and tests simultaneously.
-->

---

<!-- _class: impact -->

# The script<br>is the<br>orchestrator.

## Dynamic Workflows — Claude's biggest leap

<!-- Speaker notes:
THE BIG REVEAL — Everything before this slide was setup.
"Dynamic Workflows are not a better way to prompt Claude."
"They are a fundamentally different execution model."
Full stop. Let it land.
-->

---

## The Paradigm Shift

<span class="kicker">Standard vs Workflows</span>

**Standard Claude Code:**
> Claude IS the orchestrator — turn-by-turn decisions, intermediate results accumulating in context

**Dynamic Workflows:**
> A generated JavaScript script IS the orchestrator — Claude's context holds only the final answer

```
ultracode: audit every API endpoint in src/routes/ for missing auth checks
```

Claude writes the script. The runtime executes it. Your session stays responsive.  
Agents run in parallel. Results are **verified against each other.**

<!-- Speaker notes:
REFRAME — The context bottleneck we identified in slide 4 is eliminated.
"Intermediate results live in script variables. The context window never sees them."
"And because the plan is code, it's inspectable, editable, and resumable."
This is the structural why behind every Dynamic Workflows design decision.
-->

---

## What the Script Enables

<span class="kicker">Real orchestration code</span>

```javascript
const files = await agent("Find all route files", { schema: FILES_SCHEMA })

const audits = await pipeline(
  files.routes,

  // Stage 1: audit each file — items flow independently between stages
  file  => agent(`Audit ${file} for auth checks`, { schema: AUDIT_SCHEMA }),

  // Stage 2: a separate agent tries to disprove each finding
  audit => agent(`Adversarially verify: ${audit.finding}`, { schema: VERDICT })
)

const confirmed = audits.filter(a => a.verdict.isReal)
```

Intermediate results live in **script variables** — not Claude's context.

<!-- Speaker notes:
TECHNICAL — Make it accessible.
"The pipeline call: item A moves to stage 2 while item B is still in stage 1."
"Wall-clock = slowest single item, not sum of all stages."
"Stage 2 is adversarial — a fresh agent with no memory of stage 1, trying to find the flaw."
Only claims that survive the attack make it into the output.
-->

---

## Adversarial Verification

<span class="kicker">The killer feature</span>

Standard Claude cannot independently verify its own findings. Workflows can:

```javascript
// Three independent agents each try to REFUTE a finding
const votes = await parallel(Array.from({ length: 3 }, () => () =>
  agent(`Try to refute: ${claim}. Default to refuted=true if uncertain.`)
))

// A finding survives only if ≥ 2 of 3 agents fail to refute it
const survives = votes.filter(v => !v.refuted).length >= 2
```

This is how **`/deep-research`** works — fans out, cross-checks sources, votes internally, returns only findings that survived independent challenge.

<!-- Speaker notes:
HIGH IMPACT — The moment audiences lean forward.
"This is the scientific method, implemented in JavaScript."
"You're not asking Claude to check its own work. Three independent critics challenge every finding."
"The claim has to survive a majority attack. It's not plausible-sounding — it's adversarially tested."
No single-session AI can do this. It requires multiple independent contexts.
-->

---

## The Three Orchestration Primitives

<span class="kicker">Building blocks</span>

| Primitive | Use when | Wall-clock |
|---|---|---|
| `pipeline(items, ...stages)` | Multi-stage — default choice. Items flow through without barriers. | Slowest single item |
| `parallel(thunks)` | All of stage N must finish before stage N+1 can start | Slowest in the batch |
| `agent(prompt, opts)` | Single worker — add `schema` for validated structured output | One agent's runtime |

**Default to `pipeline()`.** Only reach for `parallel()` when cross-item context is genuinely required before proceeding.

<!-- Speaker notes:
PRACTICAL — The biggest beginner mistake.
"The biggest error: wrapping everything in parallel() because it sounds fast."
"parallel() is a barrier — every item must finish before you proceed."
"pipeline() is a conveyor belt — items flow through independently."
Choose pipeline by default. Add barriers only when the math requires it.
-->

---

<!-- _class: lead -->

# Part 3
## The decision framework

---

## Three Questions to Find Your Model

<span class="kicker">The routing logic</span>

**1 — Who holds the plan?**

| Answer | Model |
|---|---|
| You direct Claude turn by turn | Subagents |
| You hand off and monitor independently | Agent View |
| Claude coordinates workers via shared list | Agent Teams |
| A script holds the orchestration | Dynamic Workflows |

**2 — Do workers need to communicate?**  
Results only → Subagents / Agent View / Workflows &nbsp;·&nbsp; Direct messaging → **Agent Teams**

**3 — Is the partition known in advance?**  
Yes → **Custom Subagents** (repeatable) &nbsp;·&nbsp; No → **Dynamic Workflows** (script discovers it)

<!-- Speaker notes:
DECISION TOOL — Something the audience can use tomorrow.
"These three questions route you to the right model for almost any task."
"Write these on a post-it. Put it on your monitor."
"The third question is the most underrated: if you don't know how to split the work, a workflow finds the split for you."
-->

---

## The Decision Matrix

<span class="kicker">Ready-to-use routing</span>

| Situation | Reach for |
|---|---|
| Side task would flood my context | **Subagents** |
| Several independent long-running sessions | **Agent View** |
| Repeatable, cost-consistent execution | **Custom Subagents** |
| Complex interdependent work across layers | **Agent Teams** |
| Exceeds context, partition unknown | **Dynamic Workflows** |
| Codebase-wide security audit | **Dynamic Workflows** |
| 500-file migration | **Dynamic Workflows** |
| Exploratory bug hunt, unfamiliar codebase | **Dynamic Workflows** |

<!-- Speaker notes:
RAPID FIRE — Move through quickly. Let the pattern land.
"The last five rows all point to workflows — they're all 'I don't know the shape yet.'"
"Unknown shape + high quality requirement = Dynamic Workflows. Every time."
-->

---

## When NOT to Use Dynamic Workflows

<span class="kicker">The honest trade-off</span>

❌ **Repeatable tasks with predictable token budgets**  
→ Custom subagents are cheaper, faster, and more auditable

❌ **Small, bounded tasks needing a single pass**  
→ Script overhead isn't worth it for a one-agent job

❌ **Known workflows where consistency matters**  
→ A fixed subagent is more deterministic

**Use workflows when all three are true:**
1. Task exceeds a single context window
2. The partition strategy isn't known in advance
3. Quality matters more than token economy

<!-- Speaker notes:
CREDIBILITY — Knowing limits builds trust.
"Workflows are not always the right answer. Knowing when NOT to use a tool is half the skill."
"If you can write down exactly what agents will do before you run — use a custom subagent instead."
"Workflows earn their cost only when the problem's shape is genuinely unknown."
-->

---

## This Project's Structure

<span class="kicker">ClaudeWorkflows — the repo behind this deck</span>

<div class="tree">
<span style="color:#CB6B51;font-weight:700">ClaudeWorkflows/</span>
├── <span style="color:#F5F0E8">CLAUDE.md</span>  <span style="color:#6B5040">· project rules and file conventions</span>
├── <span style="color:#CB6B51">01–08-official-docs-*.md</span>  <span style="color:#6B5040">· curated reference extracts from code.claude.com</span>
├── <span style="color:#F5F0E8">claude-code-parallel-agents-reference.md</span>
│
├── <span style="color:#CB6B51">plugin/</span>
│   ├── <span style="color:#B8A896">agents/</span>  <span style="color:#6B5040">workflow-architect · sub-agent-specialist</span>
│   ├── <span style="color:#B8A896">commands/</span>  <span style="color:#6B5040">orchestrate · parallel-sweep</span>
│   └── <span style="color:#B8A896">skills/</span>  <span style="color:#6B5040">dynamic-workflows · parallel-patterns · headless-orchestration</span>
│
├── <span style="color:#CB6B51">.claude/agents/</span>  <span style="color:#6B5040">← session-scoped specialists</span>
│   ├── <span style="color:#D4C5B0">analyze.md</span> · <span style="color:#D4C5B0">planning.md</span> · <span style="color:#D4C5B0">html-css.md</span> · <span style="color:#D4C5B0">marp.md</span>
│
└── <span style="color:#CB6B51">presentations/</span>
    └── <span style="color:#D4C5B0">multi-agent-workflows.md</span>  <span style="color:#6B5040">← you are here</span>
</div>

<!-- Speaker notes:
ORIENTATION — Ground the abstract in something concrete.
"This is the actual repo. The reference docs feed the plugin. The agents generate the presentations."
"The marp.md agent in .claude/agents/ wrote this deck."
"The analyze and planning agents are available in every session in this project."
Self-referential is intentional — it demonstrates the model in action.
-->

---

## How the Pieces Connect

<div class="arch-row">
  <div class="arch-box hi" style="flex:2">
    <span class="arch-label">Reference Docs (01–08)</span>
    <span class="arch-sub">Official Claude Code documentation, curated and formatted. Every claim has a source URL. Nothing invented.</span>
  </div>
  <div class="arrow">→</div>
  <div class="arch-box">
    <span class="arch-label">Plugin</span>
    <span class="arch-sub">Skills, agents, and commands that operationalise the patterns from the docs.</span>
  </div>
</div>
<div class="arch-row" style="margin-top:10px">
  <div class="arch-box">
    <span class="arch-label">.claude/agents/</span>
    <span class="arch-sub">Session specialists: <strong>analyze</strong>, <strong>planning</strong>, <strong>html-css</strong>, <strong>marp</strong>. Loaded automatically in every Claude Code session here.</span>
  </div>
  <div class="arrow">→</div>
  <div class="arch-box hi" style="flex:2">
    <span class="arch-label">presentations/</span>
    <span class="arch-sub">Human-readable synthesis sourced from the reference docs. Generated by the marp agent. The deck you're reading now.</span>
  </div>
</div>

<!-- Speaker notes:
META — Audiences appreciate the self-referential.
"The tool used to make this presentation is documented in the presentation."
"This is what a well-structured Claude Code project looks like — agents are part of the codebase."
-->

---

<!-- _class: lead -->

# Part 4
## Patterns that work

---

## Understand → Change → Verify

<span class="kicker">The canonical engineering workflow</span>

```javascript
// Stage 1: map what needs changing (cheap — run on Haiku)
const map = await agent("Map all modules touching auth", { schema: MAP_SCHEMA })

// Stage 2: change each file in parallel, isolated worktrees prevent conflicts
const changes = await pipeline(
  map.files,
  file => agent(`Refactor auth in ${file}`, { isolation: 'worktree' })
)

// Stage 3: independent agents verify each change — no shared memory with stage 2
const verified = await parallel(changes.map(c => () =>
  agent(`Verify ${c.file} is correct after refactor`, { schema: VERDICT })
))
```

With `/effort ultracode`, Claude plans and runs this entire loop automatically.

<!-- Speaker notes:
PATTERN TRANSFER — Help the audience see how to apply this.
"Stage 1 is cheap: read-only exploration. Run on Haiku."
"Stage 2 is isolated: worktrees mean two agents can't corrupt the same file."
"Stage 3 is adversarial: the verifier doesn't know what the changer wrote. It checks correctness independently."
This three-stage loop is the foundation of every serious engineering workflow.
-->

---

## Loop Until Dry

<span class="kicker">For unknown-scope discovery</span>

```javascript
const bugs = [], seen = new Set()
let dry = 0

while (dry < 2) {                       // two consecutive empty rounds = done
  const found = await agent("Find bugs not yet in seen set", { schema: BUGS })
  const fresh = found.bugs.filter(b => !seen.has(b.id))

  if (!fresh.length) { dry++; continue }
  dry = 0
  fresh.forEach(b => seen.add(b.id))   // dedup against seen, not confirmed
  bugs.push(...fresh)
}
```

Runs until **two consecutive rounds find nothing new** — not just until a count is hit. Catches the tail `while (count < N)` always misses.

<!-- Speaker notes:
SUBTLE BUT CRITICAL — Easy to get wrong.
"The naive version stops when count reaches N. What if there are N+3 bugs?"
"Loop-until-dry keeps going until the space is genuinely exhausted."
"Dedup against `seen`, not `confirmed` — if a rejected finding isn't tracked, it re-appears every round and the loop never converges."
-->

---

## Getting Started

<span class="kicker">Four entry points, in order of investment</span>

**1 — Bundled workflow (zero setup)**
```
/deep-research What changed in the Node.js permission model between v20 and v22?
```

**2 — Keyword trigger**
```
ultracode: audit every API endpoint under src/routes/ for missing auth checks
```

**3 — Natural language (v2.1.160+)**
```
Use a workflow to migrate all fetch() calls to the new HTTP client.
```

**4 — Session-wide orchestration**
```
/effort ultracode
```

Start at 1. Graduate to 4. Build custom subagents along the way.

<!-- Speaker notes:
CALL TO ACTION — Give people something to do today.
"You can run step 1 right now, in this session, on any question you have."
"Don't skip to 4 before you've run 1 a few times — understand what workflow output looks like before you rely on it."
-->

---

## The Rule That Governs All of This

<span class="kicker">Summary</span>

| Shape | Solves |
|---|---|
| **Subagents** | Context flooding · task isolation · least-privilege |
| **Agent Teams** | Interdependent parallel work · inter-agent awareness |
| **Dynamic Workflows** | Scale · adversarial verification · resumability |
| **Agent View** | Independent long-running sessions you monitor |

The shift isn't about using **more AI.**  
It's about choosing the **execution model that fits the structure of your problem.**

If the problem's shape is unknown — let the script discover it.  
If the shape is known — encode it in a custom subagent and never think about it again.

<!-- Speaker notes:
SYNTHESIS — Bring it home.
"Four models. Each one solves a different structural problem."
"The skill isn't knowing all four. It's knowing which question finds the right one."
"Who holds the plan? Do workers need to talk? Is the partition known?"
Answer those three questions. The model selects itself.
-->

---

<!-- _class: impact -->

# The right tool<br>for the<br>problem's shape.

## That's the whole framework.

<!-- Speaker notes:
CLOSE ON THE THEME.
"You will forget the names of the primitives."
"You will not forget this question: does my problem have a known shape?"
"Yes: encode it. No: orchestrate it."
"Thank you."
-->

---

<!-- _class: impact -->

# Thank you

## code.claude.com/docs/en/workflows
## code.claude.com/docs/en/agents
## code.claude.com/docs/en/sub-agents

*Sourced entirely from official Claude Code documentation.*

<!-- Speaker notes:
BRIEF. They have the deck.
"All three URLs are in the reference docs in this repo. Questions?"
-->
