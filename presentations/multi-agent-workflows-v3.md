---
marp: true
theme: default
paginate: true
html: true
backgroundColor: #faf9f5
color: #141413
style: |
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&family=Lora:ital,wght@0,400;0,500;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap');

  /* ═══════════════════════════════════════════
     Anthropic brand palette — exact spec
  ═══════════════════════════════════════════ */
  :root {
    --dark:       #141413;
    --light:      #faf9f5;
    --mid-gray:   #b0aea5;
    --light-gray: #e8e6dc;
    --orange:     #d97757;
    --blue:       #6a9bcc;
    --green:      #788c5d;
    --orange-mute: rgba(217,119,87,0.13);
    --blue-mute:   rgba(106,155,204,0.13);
    --green-mute:  rgba(120,140,93,0.13);
  }

  /* ═══════════════════════════════════════════
     BASE SLIDE — light background
  ═══════════════════════════════════════════ */
  section {
    font-family: 'Lora', Georgia, serif;
    font-weight: 400;
    background: var(--light);
    color: var(--dark);
    padding: 56px 76px 52px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    line-height: 1.65;
  }

  section::before { display: none; }

  section::after {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.6em;
    color: var(--mid-gray);
    letter-spacing: 0.1em;
  }

  /* ═══════════════════════════════════════════
     TYPOGRAPHY
  ═══════════════════════════════════════════ */
  h1 {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 800;
    font-size: 2em;
    color: var(--orange);
    letter-spacing: -0.02em;
    line-height: 1.1;
    margin-bottom: 0.2em;
    border: none;
    padding: 0;
  }

  h2 {
    font-family: 'Lora', Georgia, serif;
    font-weight: 400;
    font-style: italic;
    font-size: 1.05em;
    color: var(--mid-gray);
    letter-spacing: 0;
    margin-bottom: 1em;
    border: none;
    padding: 0;
  }
  h2::after { display: none; }

  h3 {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 700;
    font-size: 0.72em;
    color: var(--orange);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 0.4em;
  }

  p  { color: var(--dark); line-height: 1.7; font-size: 0.95em; }
  li { color: var(--dark); margin: 0.3em 0; line-height: 1.6; font-size: 0.93em; }
  strong { color: var(--dark); font-weight: 600; }
  em { color: var(--mid-gray); }

  blockquote {
    border-left: 4px solid var(--orange);
    background: var(--orange-mute);
    padding: 14px 20px;
    margin: 14px 0;
    color: var(--dark);
    font-style: italic;
    border-radius: 0 8px 8px 0;
    box-shadow: 0 1px 4px rgba(20,20,19,0.06);
  }
  blockquote strong { color: var(--orange); font-style: normal; }

  /* ═══════════════════════════════════════════
     CODE
  ═══════════════════════════════════════════ */
  code {
    font-family: 'JetBrains Mono', monospace;
    background: var(--light-gray);
    color: var(--orange);
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 0.8em;
  }
  pre {
    background: var(--dark);
    border-radius: 10px;
    padding: 20px 24px;
    font-size: 0.7em;
    line-height: 1.6;
    box-shadow: 0 4px 20px rgba(20,20,19,0.18);
  }
  pre code {
    background: transparent;
    color: #c8c0b4;
    padding: 0;
    font-size: 1em;
  }

  /* ═══════════════════════════════════════════
     TABLES
  ═══════════════════════════════════════════ */
  table {
    font-size: 0.8em;
    border-collapse: separate;
    border-spacing: 0;
    width: 100%;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 12px rgba(20,20,19,0.08);
    font-family: 'Lora', Georgia, serif;
  }
  th {
    background: var(--orange);
    color: var(--light);
    padding: 9px 16px;
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 700;
    font-size: 0.9em;
    letter-spacing: 0.03em;
    text-align: left;
    border: none;
  }
  td {
    padding: 8px 16px;
    border-bottom: 1px solid var(--light-gray);
    color: var(--dark);
    background: var(--light);
  }
  tr:last-child td { border-bottom: none; }
  tr:nth-child(even) td { background: #f3f1eb; }

  /* ═══════════════════════════════════════════
     IMPACT SLIDES — orange background (brand image)
  ═══════════════════════════════════════════ */
  section.impact {
    background: var(--orange);
    color: var(--dark);
    justify-content: center;
  }
  section.impact::after { color: rgba(20,20,19,0.3); }
  section.impact h1 {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 800;
    font-size: 3.8em;
    color: var(--dark);
    letter-spacing: -0.04em;
    line-height: 0.95;
    margin-bottom: 0.35em;
  }
  section.impact h2 {
    font-family: 'Lora', Georgia, serif;
    font-style: italic;
    color: rgba(20,20,19,0.55);
    font-size: 1em;
    margin-top: 0.5em;
  }
  section.impact p  { color: rgba(20,20,19,0.7); font-size: 0.9em; }
  section.impact li { color: rgba(20,20,19,0.7); }

  /* ═══════════════════════════════════════════
     LEAD SLIDES — dark background
  ═══════════════════════════════════════════ */
  section.lead {
    background: var(--dark);
    color: var(--light);
    justify-content: center;
  }
  section.lead::after { color: rgba(250,249,245,0.2); }
  section.lead h1 {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 800;
    font-size: 2.8em;
    color: var(--light);
    letter-spacing: -0.03em;
    line-height: 1.05;
  }
  section.lead h2 {
    font-family: 'Lora', Georgia, serif;
    font-style: italic;
    color: var(--orange);
    font-size: 1em;
    margin-top: 0.5em;
  }
  section.lead p, section.lead li { color: var(--mid-gray); }
  section.lead strong            { color: var(--light); }
  section.lead code              { background: rgba(250,249,245,0.1); color: var(--orange); }
  section.lead pre               { background: rgba(250,249,245,0.06); box-shadow: none; }
  section.lead pre code          { color: #c8c0b4; background: transparent; }
  section.lead td                { color: var(--mid-gray); border-color: rgba(250,249,245,0.08); background: transparent; }
  section.lead tr:nth-child(even) td { background: rgba(250,249,245,0.04); }
  section.lead th                { background: var(--orange); color: var(--dark); }
  section.lead blockquote        { border-color: var(--orange); background: rgba(217,119,87,0.15); color: var(--light); }

  /* ═══════════════════════════════════════════
     UTILITY CLASSES
  ═══════════════════════════════════════════ */
  .kicker {
    font-family: 'Poppins', Arial, sans-serif;
    font-size: 0.64em;
    font-weight: 700;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    color: var(--orange);
    display: block;
    margin-bottom: 10px;
  }

  .tag {
    display: inline-block;
    background: var(--orange);
    color: var(--light);
    border-radius: 4px;
    padding: 2px 10px;
    font-size: 0.68em;
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 700;
    letter-spacing: 0.05em;
    margin-right: 6px;
    vertical-align: middle;
  }

  /* ═══════════════════════════════════════════
     DIAGRAM COMPONENTS
  ═══════════════════════════════════════════ */
  .diagram-grid {
    display: grid;
    gap: 10px;
    margin: 12px 0;
  }
  .diagram-grid.cols-2 { grid-template-columns: 1fr 1fr; }
  .diagram-grid.cols-3 { grid-template-columns: 1fr 1fr 1fr; }

  .d-box {
    border-radius: 8px;
    padding: 14px 18px;
    font-size: 0.77em;
    box-shadow: 0 2px 8px rgba(20,20,19,0.07);
    border: 1.5px solid transparent;
    font-family: 'Lora', Georgia, serif;
  }
  .d-box.orange { background: var(--orange-mute); border-color: var(--orange); }
  .d-box.blue   { background: var(--blue-mute);   border-color: var(--blue);   }
  .d-box.green  { background: var(--green-mute);  border-color: var(--green);  }
  .d-box.gray   { background: var(--light-gray);  border-color: var(--mid-gray); }
  .d-box.dark   { background: var(--dark); color: var(--light); border-color: var(--dark); }

  .d-label {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 700;
    font-size: 0.95em;
    display: block;
    margin-bottom: 5px;
  }
  .d-box.orange .d-label { color: var(--orange); }
  .d-box.blue   .d-label { color: var(--blue);   }
  .d-box.green  .d-label { color: var(--green);  }
  .d-box.gray   .d-label { color: var(--dark);   }
  .d-box.dark   .d-label { color: var(--orange); }

  .d-sub { color: var(--mid-gray); line-height: 1.45; }
  .d-box.dark .d-sub { color: rgba(250,249,245,0.55); }

  .d-arrow {
    display: flex; align-items: center; justify-content: center;
    color: var(--mid-gray); font-size: 1.3em;
  }
  .d-flow {
    display: flex; align-items: stretch; gap: 8px;
    margin: 8px 0;
  }
  .d-flow > .d-box { flex: 1; }

  .d-pill {
    display: inline-block;
    border-radius: 100px;
    padding: 3px 12px;
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 600;
    font-size: 0.72em;
    letter-spacing: 0.03em;
  }
  .d-pill.orange { background: var(--orange); color: var(--light); }
  .d-pill.blue   { background: var(--blue);   color: var(--light); }
  .d-pill.green  { background: var(--green);  color: var(--light); }

  .tree {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.71em;
    line-height: 1.9;
    background: var(--dark);
    color: var(--mid-gray);
    padding: 22px 28px;
    border-radius: 10px;
    box-shadow: 0 4px 20px rgba(20,20,19,0.18);
  }

  .scale-bar {
    display: flex;
    align-items: center;
    gap: 0;
    margin-top: 14px;
    font-size: 0.68em;
    font-family: 'Poppins', Arial, sans-serif;
  }
  .scale-bar-track {
    flex: 1;
    height: 4px;
    border-radius: 4px;
    background: linear-gradient(90deg, var(--green) 0%, var(--blue) 50%, var(--orange) 100%);
    margin: 0 10px;
  }
  .scale-bar span { color: var(--mid-gray); }

  /* ═══════════════════════════════════════════
     ARCH DIAGRAM COMPONENTS
  ═══════════════════════════════════════════ */
  .arch-row {
    display: flex;
    align-items: stretch;
    gap: 12px;
  }
  .arch-box {
    border-radius: 8px;
    padding: 14px 18px;
    background: var(--blue-mute);
    border: 1.5px solid var(--blue);
    box-shadow: 0 2px 8px rgba(20,20,19,0.07);
    display: flex;
    flex-direction: column;
    gap: 6px;
  }
  .arch-box.hi {
    background: var(--orange-mute);
    border-color: var(--orange);
  }
  .arch-label {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 700;
    font-size: 0.9em;
    color: var(--blue);
    display: block;
  }
  .arch-box.hi .arch-label {
    color: var(--orange);
  }
  .arch-sub {
    font-size: 0.78em;
    color: var(--mid-gray);
    line-height: 1.5;
    display: block;
  }
  .arrow {
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--mid-gray);
    font-size: 1.3em;
    padding: 0 4px;
    flex-shrink: 0;
  }
---

<!-- _class: impact -->

# Multi-Agent<br>Workflows<br>in Claude

## From turn-by-turn conversations to orchestrated systems — v3

<!-- Speaker notes:
OPEN COLD. Three full seconds of silence before you speak.
"A year ago, you prompted Claude. Today, you choose an execution model."
The shift from prompting skill to architectural thinking is the thesis of everything that follows.
v3: tightened to ~15 min — plugin (Part 3) fully covered, code-heavy deep-dives moved to the docs.
-->

---

## The Question Has Changed

<span class="kicker">The shift</span>

> **Before:** "How do I prompt Claude to do this?"

> **Now:** "What execution model best fits the structure of this problem?"

Multi-agent workflows aren't a feature update. They're a new mental model for what AI assistance means at scale.

<!-- Speaker notes:
HOOK — If time is short, start here.
The old question was about prompting skill. The new question is about architecture.
Same shift developers made from writing scripts to designing systems.
AGENDA (to mention verbally):
• What Claude was already doing
• Where that model breaks
• The four execution models
• The plugin that operationalises them
• How to choose the right one
• Patterns that work
-->

---

<!-- _class: lead -->

# Part 1
## How Claude was already working

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
TENSION — Let the audience feel the pain.
"Raise your hand if Claude's context filled up mid-task."
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

| Model | Plan lives in | Coordination | Scale |
|---|---|---|---|
| **Subagents** | Claude's context | Claude, per turn | A few per turn |
| **Agent View** | Your attention | You, checking in | Independent sessions |
| **Agent Teams** | Shared task list | Claude + teammates | 3–10 collaborating peers |
| **Dynamic Workflows** | Script variables | Generated JS script | Tens to hundreds |

These are not **levels.** They are **shapes** — each fits a different problem structure.

<!-- Speaker notes:
CLARITY — The key insight is where the plan lives.
"In subagents, Claude holds the plan in its head. In workflows, the plan is code."
"Code is inspectable, resumable, version-controllable. Claude's context is not."
Spend 30 seconds on the last row — that's where the talk is going.
-->

---

## Architecture of the Four Models

<div class="diagram-grid cols-2">
  <div class="d-box orange">
    <span class="d-label">Subagents</span>
    <span class="d-sub">Worker spawned per turn. Returns a summary only — intermediate noise never reaches the caller's context.</span>
  </div>
  <div class="d-box blue">
    <span class="d-label">Agent View</span>
    <span class="d-sub">Independent background sessions. You dispatch tasks, monitor status, and step in only when needed.</span>
  </div>
  <div class="d-box green">
    <span class="d-label">Agent Teams</span>
    <span class="d-sub">Peers with a shared task list and direct inter-agent messaging. Each teammate is aware of others' progress.</span>
  </div>
  <div class="d-box dark">
    <span class="d-label">Dynamic Workflows</span>
    <span class="d-sub">A JS script is the orchestrator. Results live in variables, not context. Resumable. Up to <strong>1,000 agents</strong> per run.</span>
  </div>
</div>

<div class="scale-bar">
  <span>simpler · cheaper · deterministic</span>
  <div class="scale-bar-track"></div>
  <span>adaptive · powerful · higher cost</span>
</div>

<!-- Speaker notes:
VISUAL — Let the diagram breathe. Don't narrate every box.
"Green = teams that collaborate. Blue = sessions you monitor. Orange = specialists you define. Dark = the script takes over."
The gradient scale bar reinforces that these are trade-offs, not rankings.
-->

---

<!-- _class: impact -->

# Your context.<br>Protected.

## Subagents keep exploration out of your main session

<!-- Speaker notes:
PAUSE fully after this slide loads.
"Every codebase search returns hundreds of lines. Without subagents, all of it hits your context."
"With subagents, only the answer does."
-->

---

## What Subagents Actually Solve

<span class="kicker">Context isolation in practice</span>

```
"Find all usages of the deprecated API across the codebase"

→ Without a subagent:  200 grep results poured into your context
→ With a subagent:     "Found 47 usages in 12 files" — and nothing else
```

A subagent gets its own context window, does the work, and **returns only the summary.**

| Built-in agent | Model | Protects you from |
|---|---|---|
| `Explore` | Haiku | Search floods (read-only, fast, cheap) |
| `Plan` | Inherited | Context pollution during planning |
| `General-purpose` | Inherited | Long exploration + modification runs |

<!-- Speaker notes:
PRACTICAL — Weight this slide.
"Explore runs on Haiku. It reads 40 files, sends you a summary. Your main session never saw those 40 files."
This is the easiest entry point. Highest immediate ROI.
-->

---

## Custom Subagents: Specialists You Define

<span class="kicker">Reusable roles — saved in .claude/agents/</span>

```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities. Use when touching
  auth, session handling, input validation, or API boundaries.
tools: Read, Grep, Glob
model: sonnet
---

Analyze code for OWASP Top 10, injection risks, improper auth, and
data exposure. Cite every finding with file:line references.
```

The **`description` field is a routing rule** — Claude reads it to decide when to invoke the agent automatically. Precision here determines reliability.

<!-- Speaker notes:
KEY INSIGHT — The description is a trigger condition, not a docstring.
"Write it like a conditional: invoke this when X happens."
"Notice: no Write tool. That's enforcement, not an oversight."
A reviewer that cannot write cannot touch production code by accident.
-->

---

## Subagents vs. Agent Teams

<span class="kicker">The critical difference — coordination model</span>

<div class="d-flow">
  <div class="d-box blue" style="flex:1">
    <span class="d-label">Subagents</span>
    <span class="d-sub">
      Fully isolated — no inter-communication<br>
      Results back to caller only<br>
      Main agent manages all work<br>
      Disappear when done<br>
      Sequential file access
    </span>
  </div>
  <div class="d-box green" style="flex:1">
    <span class="d-label">Agent Teams</span>
    <span class="d-sub">
      Aware of each other's progress<br>
      Direct messaging between any teammates<br>
      Shared task list — teammates self-claim<br>
      Stay running, claim next available task<br>
      Partition files strictly to avoid conflicts
    </span>
  </div>
</div>

<!-- Speaker notes:
CONTRAST — Make the rows visceral.
"Every line represents a different class of problem."
"Subagents: contractors who report in. Teammates: colleagues who collaborate."
The git workspace is the key — that's shared state without stepping on each other.
KEY EXAMPLE: "A backend teammate can *know* when the frontend teammate changes an API contract — because they share a git workspace."
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

<span class="kicker">What actually changes</span>

<div class="arch-row" style="margin-bottom:14px">
  <div class="arch-box" style="flex:1">
    <span class="arch-label">Standard Claude Code</span>
    <span class="arch-sub">Claude IS the orchestrator · turn-by-turn decisions · intermediate results accumulate in context · self-verification impossible</span>
  </div>
  <div class="arrow">→</div>
  <div class="arch-box hi" style="flex:1">
    <span class="arch-label">Dynamic Workflows</span>
    <span class="arch-sub">A JS script IS the orchestrator · results in variables · context stays clean · adversarial verification built-in</span>
  </div>
</div>

```
ultracode: audit every API endpoint in src/routes/ for missing auth checks
```

Claude writes the script. The runtime executes it in the background.  
Agents run in parallel. Results are **verified against each other.**

<!-- Speaker notes:
REFRAME — This is the payoff of everything we've built toward.
"The bottleneck we identified in slide 5 — the context window — is eliminated."
"Intermediate results live in script variables. The context window never sees them."
"Because the plan is code: inspectable, editable, and resumable."
-->

---

## What the Script Enables

<span class="kicker">Real orchestration code</span>

```javascript
const files = await agent("Find all route files", { schema: FILES_SCHEMA })

const audits = await pipeline(
  files.routes,

  // Stage 1: audit each file independently — no barriers between items
  file  => agent(`Audit ${file} for auth checks`, { schema: AUDIT_SCHEMA }),

  // Stage 2: a separate agent with no memory of stage 1 tries to disprove each finding
  audit => agent(`Adversarially verify: ${audit.finding}`, { schema: VERDICT })
)

const confirmed = audits.filter(a => a.verdict.isReal)
return { confirmed, total: files.routes.length }
```

Intermediate results live in **script variables** — the context window sees only the final answer.

<!-- Speaker notes:
TECHNICAL — Make it accessible.
"The pipeline: item A moves to stage 2 while item B is still in stage 1."
"Wall-clock = slowest single item, not sum of all stages."
"Stage 2 is adversarial: a fresh agent with no memory of stage 1 checks the finding independently."
Only claims that survive the attack reach the output.
-->

---

## The Three Orchestration Primitives

<span class="kicker">Building blocks of every workflow</span>

<div class="diagram-grid cols-3" style="margin:14px 0">
  <div class="d-box orange">
    <span class="d-label">pipeline()</span>
    <span class="d-sub">Multi-stage, no barriers. Item A can be in stage 3 while item B is in stage 1. <strong>Default choice.</strong></span>
  </div>
  <div class="d-box blue">
    <span class="d-label">parallel()</span>
    <span class="d-sub">Barrier — all items must finish before the next stage starts. Use only when cross-item context is genuinely required.</span>
  </div>
  <div class="d-box green">
    <span class="d-label">agent()</span>
    <span class="d-sub">Single worker. Add <code>schema</code> for validated structured output. Add <code>isolation: 'worktree'</code> for file-safe parallelism.</span>
  </div>
</div>

<div class="arch-row" style="margin:10px 0 6px 0; font-size:0.8em">
  <div class="arch-box hi" style="flex:2">
    <span class="arch-label" style="font-size:0.9em">pipeline(items, ...stages)</span>
    <span class="arch-sub">A → stage1 → stage2 → done<br>B → stage1 → stage2 → done<br><em>Items flow independently — no barriers</em></span>
  </div>
  <div class="arrow">vs</div>
  <div class="arch-box" style="flex:2">
    <span class="arch-label" style="font-size:0.9em">parallel(thunks)</span>
    <span class="arch-sub">A ─┐<br>B ─┤ → all finish → proceed<br>C ─┘<br><em>Barrier: waits for every item</em></span>
  </div>
</div>

**Default to `pipeline()`.** Wall-clock time = slowest single item, not sum of stages.

<!-- Speaker notes:
PRACTICAL — The biggest beginner mistake is reaching for parallel() first.
"parallel() is a barrier — every item must finish before you proceed."
"pipeline() is a conveyor belt — items flow through independently."
Choose pipeline by default. Add barriers only when the math requires it.
-->

---

<!-- _class: impact -->

# The plugin<br>is the<br>practice layer.

## Skills, agents, and commands — built into every session

<!-- Speaker notes:
BRIDGE — From theory to tool.
"The reference docs tell you what's possible. The plugin makes it the default."
"Four skills, two agents, three commands — each one operationalises a pattern from this talk."
This is what's new in v2.
-->

---

<!-- _class: lead -->

# Part 3
## The plugin — from docs to practice

---

## Plugin Architecture

<span class="kicker">Three layers, one purpose</span>

<div class="tree">
<span style="color:#d97757;font-weight:700">plugin/</span>
├── <span style="color:#6a9bcc">skills/</span>           <span style="color:#6a7560">← cognitive guides — invoked by Skill tool</span>
│   ├── <span style="color:#faf9f5">dynamic-workflows/</span>    <span style="color:#6a7560">when/how to trigger, primitives, constraints</span>
│   ├── <span style="color:#faf9f5">parallel-patterns/</span>    <span style="color:#6a7560">adversarial verify · loop-until-dry · judge panel</span>
│   ├── <span style="color:#faf9f5">error-handling/</span>       <span style="color:#6a7560">filter(Boolean) · schema design · drop logging</span>
│   └── <span style="color:#faf9f5">headless-orchestration/</span> <span style="color:#6a7560">claude -p for CI/CD and shell fan-outs</span>
│
├── <span style="color:#788c5d">agents/</span>           <span style="color:#6a7560">← specialists auto-routed by description</span>
│   ├── <span style="color:#faf9f5">workflow-architect.md</span> <span style="color:#6a7560">designs orchestration scripts end-to-end</span>
│   └── <span style="color:#faf9f5">sub-agent-specialist.md</span> <span style="color:#6a7560">designs agent prompts, schemas, model tiers</span>
│
└── <span style="color:#d97757">commands/</span>         <span style="color:#6a7560">← instant workflows, available as /command</span>
    ├── <span style="color:#faf9f5">orchestrate.md</span>        <span style="color:#6a7560">4-phase recipe for any complex task</span>
    ├── <span style="color:#faf9f5">parallel-sweep.md</span>     <span style="color:#6a7560">fan-out over a list of items</span>
    └── <span style="color:#faf9f5">workflow-review.md</span>    <span style="color:#6a7560">reviews a workflow script for correctness</span>
</div>

<!-- Speaker notes:
ORIENTATION — Three layers, each doing something different.
"Skills are read when Claude needs to decide HOW to do something."
"Agents are invoked automatically when their description matches the task."
"Commands are pre-built workflows you trigger with a slash."
-->

---

## Skills: Your Cognitive Guides

<span class="kicker">Invoked by the Skill tool — tell Claude how to think</span>

<div class="diagram-grid cols-2" style="margin:12px 0">
  <div class="d-box orange">
    <span class="d-label">dynamic-workflows</span>
    <span class="d-sub">Covers: when workflows beat subagents · script primitives · run management · cost controls · troubleshooting. Invoke when writing or debugging any workflow.</span>
  </div>
  <div class="d-box blue">
    <span class="d-label">parallel-patterns</span>
    <span class="d-sub">Five copy-paste patterns: adversarial verify · loop-until-dry · judge panel · multi-modal sweep · completeness critic. Invoke when choosing an orchestration shape.</span>
  </div>
  <div class="d-box green">
    <span class="d-label">error-handling</span>
    <span class="d-sub">Rules: always <code>.filter(Boolean)</code> after <code>parallel()</code> · log dropped agents · defensive schema design · no manual retry wrappers. Invoke at scale.</span>
  </div>
  <div class="d-box gray">
    <span class="d-label">headless-orchestration</span>
    <span class="d-sub">Shell fan-outs via <code>claude -p</code> with <code>xargs</code>, GNU parallel, or Python. Use when integrating with CI/CD or existing shell pipelines.</span>
  </div>
</div>

<!-- Speaker notes:
ACTIONABLE — Skills are the reference layer you pull in on demand.
"You don't memorise all five patterns. You invoke parallel-patterns when you need to pick one."
"error-handling is the most commonly skipped — and the most commonly regretted."
-->

---

## Agents: Specialists in Every Session

<span class="kicker">Auto-routed by description — no manual invocation needed</span>

<div class="d-flow" style="margin:14px 0">
  <div class="d-box orange" style="flex:1">
    <span class="d-label">workflow-architect</span>
    <span class="d-sub">
      Triggered: building, debugging, or optimizing a workflow script<br><br>
      Expertise: pipeline vs parallel decisions · phase structure · adversarial verification patterns · budget-aware scaling<br><br>
      Tools: Read · Grep · Glob · Bash &nbsp;&nbsp; Model: Sonnet
    </span>
  </div>
  <div class="d-box blue" style="flex:1">
    <span class="d-label">sub-agent-specialist</span>
    <span class="d-sub">
      Triggered: defining agent prompts, tool access, or structured schemas<br><br>
      Expertise: Role · Context · Task · Output format · Model routing (Haiku → Sonnet → Opus) · worktree isolation<br><br>
      Tools: Read · Grep · Glob &nbsp;&nbsp; Model: Sonnet
    </span>
  </div>
</div>

> The **description field** is a routing rule, not a docstring. Write it as a trigger condition.

<!-- Speaker notes:
KEY INSIGHT — Agents are invoked by the system, not by you.
"Claude reads the description field and decides whether this specialist fits the task."
"That's why precision matters: vague descriptions get vague routing."
"No Write tool on the sub-agent-specialist — it designs prompts, it doesn't execute them."
-->

---

## Commands: Instant Workflows

<span class="kicker">/orchestrate — the four-phase recipe</span>

```
/orchestrate audit every API endpoint under src/routes for missing auth checks
```

Claude runs four phases automatically:

<div class="diagram-grid cols-2" style="margin:12px 0">
  <div class="d-box orange">
    <span class="d-label">1 — Discover</span>
    <span class="d-sub">Map the work surface. List files, enumerate issues, gather sources. Cheap — route to Haiku.</span>
  </div>
  <div class="d-box blue">
    <span class="d-label">2 — Execute</span>
    <span class="d-sub">Fan out over discovered items in parallel. One agent per item. Use <code>isolation: 'worktree'</code> for file edits.</span>
  </div>
  <div class="d-box green">
    <span class="d-label">3 — Verify</span>
    <span class="d-sub">Adversarially check high-stakes findings. Three independent critics per claim. Only survivors reach output.</span>
  </div>
  <div class="d-box gray">
    <span class="d-label">4 — Synthesize</span>
    <span class="d-sub">Produce the final deliverable. Confirmed findings only. Summary + recommendations.</span>
  </div>
</div>

<!-- Speaker notes:
PRACTICAL — This is the pattern to memorise.
"Discover → Execute → Verify → Synthesize. Every serious workflow is a variation of this."
"/orchestrate writes and launches the script for you. You review it, add ultracode, it runs."
After a successful run, press s in /workflows to save it as a reusable /<name> command.
-->

---

## Sub-Agent Design Principles

<span class="kicker">What every good agent prompt contains</span>

<div class="arch-row" style="margin:12px 0">
  <div class="arch-box hi" style="flex:3">
    <span class="arch-label">Four parts of a scoped prompt</span>
    <span class="arch-sub">
      <strong style="color:var(--orange)">Role</strong> — "You are a security reviewer."<br>
      <strong style="color:var(--orange)">Context</strong> — "Analyze src/auth/middleware.ts"<br>
      <strong style="color:var(--orange)">Task</strong> — "Focus on JWT algorithm confusion and session fixation."<br>
      <strong style="color:var(--orange)">Output</strong> — "Return JSON: {file, vulns: [{line, severity, fix}]}"
    </span>
  </div>
  <div class="arch-box" style="flex:2">
    <span class="arch-label">Model routing</span>
    <span class="arch-sub">
      <code>haiku</code> — discovery, listing, classification<br>
      <code>sonnet</code> — analysis, review, synthesis (default)<br>
      <code>opus</code> — adversarial verify, final synthesis<br><br>
      Route cheap stages to Haiku. Reserve Opus for quality gates.
    </span>
  </div>
</div>

> Sub-agents receive **only what's in their prompt.** They share no memory with siblings.

<!-- Speaker notes:
PRACTICAL — The most common mistake is vague prompts.
"Every piece of context missing from the prompt is context the agent will hallucinate."
"Model routing is the single biggest lever on cost: a Haiku discovery pass on 200 files costs a fraction of a Sonnet pass."
-->

---

## /workflows Interface and Saving

<span class="kicker">Managing runs — and making them permanent</span>

<div class="d-flow" style="margin:12px 0">
  <div class="d-box dark" style="flex:2">
    <span class="d-label">Keyboard controls in /workflows</span>
    <span class="d-sub">
      ↑ / ↓ — select phase or agent<br>
      Enter / → — drill into phase → agent (prompt, tool calls, result)<br>
      p — pause or resume the run<br>
      x — stop selected agent or entire workflow<br>
      r — restart a running agent<br>
      s — <strong style="color:var(--orange)">save as a reusable command</strong>
    </span>
  </div>
  <div class="d-box green" style="flex:1">
    <span class="d-label">Save locations</span>
    <span class="d-sub">
      <code>.claude/workflows/</code><br>
      shared with everyone who clones the repo<br><br>
      <code>~/.claude/workflows/</code><br>
      personal, available in every project<br><br>
      Saved workflows appear as <code>/name</code> in autocomplete.
    </span>
  </div>
</div>

**Resume:** Completed agents return cached results instantly. Works within the same session.

<!-- Speaker notes:
PRACTICAL — The s shortcut is the most underused feature.
"Every workflow you run successfully is one press of s away from becoming a reusable command."
"Resume means a 200-agent run that gets interrupted doesn't start from zero — only incomplete agents re-run."
-->

---

## Headless Orchestration: claude -p

<span class="kicker">For CI/CD pipelines and shell fan-outs</span>

```bash
# Fan out 8 parallel Claude sessions over TypeScript files
find src/ -name "*.ts" | xargs -P 8 -I{} \
  claude -p "Review {} for missing error handling. JSON: {\"file\":\"{}\",\"issues\":[]}" \
  --output-format json >> results.jsonl

# Aggregate: show only files with issues
jq -s '[.[] | select(.issues | length > 0)]' results.jsonl
```

| | Dynamic Workflows | Headless `claude -p` |
|---|---|---|
| **Resume** | Yes — cached results | No — each run independent |
| **Progress** | `/workflows` drill-down | Shell stdout |
| **Verification** | Adversarial, built-in | Manual shell logic |
| **Best for** | Large resumable audits | CI pipelines, simple fan-outs |

<!-- Speaker notes:
BRIDGE — Workflows aren't the only way.
"For a GitHub Actions job that reviews every PR, headless is simpler and integrates cleanly."
"For a 200-file migration you want to resume, the workflow runtime is the right tool."
Choose by resumability and verification needs.
-->

---

<!-- _class: lead -->

# Part 4
## The decision framework

---

## Three Questions to Find Your Model

<span class="kicker">The routing logic</span>

<div class="diagram-grid cols-3" style="margin:14px 0">
  <div class="d-box orange">
    <span class="d-label">1 — Who holds the plan?</span>
    <span class="d-sub">
      Turn by turn → <strong>Subagents</strong><br>
      You monitor → <strong>Agent View</strong><br>
      Shared task list → <strong>Agent Teams</strong><br>
      Script holds it → <strong>Workflows</strong>
    </span>
  </div>
  <div class="d-box blue">
    <span class="d-label">2 — Do workers communicate?</span>
    <span class="d-sub">
      Results only → Subagents / View / Workflows<br><br>
      Direct messaging needed → <strong>Agent Teams</strong>
    </span>
  </div>
  <div class="d-box green">
    <span class="d-label">3 — Is the partition known?</span>
    <span class="d-sub">
      Yes → <strong>Custom Subagents</strong><br>
      (repeatable, cost-consistent)<br><br>
      No → <strong>Dynamic Workflows</strong><br>
      (script discovers the split)
    </span>
  </div>
</div>

<!-- Speaker notes:
DECISION TOOL — Something the audience can use tomorrow.
"Write these three questions on a post-it. Put it on your monitor."
"The third question is the most underrated: if you don't know how to split the work, let a workflow find the split."
Q2: Do workers need to communicate? Results only → Subagents / Agent View / Workflows. Direct messaging → Agent Teams.
Q3: Is the partition known in advance? Yes → Custom Subagents (repeatable). No → Dynamic Workflows (script discovers it).
-->

---

## The Decision Matrix

<span class="kicker">Ready-to-use routing</span>

| Situation | <span class="d-pill orange">Orange</span> Subagents | <span class="d-pill blue">Blue</span> Teams | <span class="d-pill green">Green</span> Workflows |
|---|---|---|---|
| Side task floods context | ✓ | | |
| Repeatable, cost-consistent | ✓ | | |
| Interdependent across layers | | ✓ | |
| Specialists must collaborate | | ✓ | |
| Partition unknown, quality critical | | | ✓ |
| 500-file migration | | | ✓ |
| Codebase-wide security audit | | | ✓ |
| Exploratory bug hunt | | | ✓ |

<!-- Speaker notes:
RAPID FIRE — Move through quickly. Let the pattern land.
"The last four rows all point to Workflows — they're all 'I don't know the shape yet.'"
"Unknown shape + quality critical = Dynamic Workflows, every time."
-->

---

## When NOT to Use Dynamic Workflows

<span class="kicker">The honest trade-off</span>

<div class="d-flow" style="margin:14px 0">
  <div class="d-box orange">
    <span class="d-label">Use Workflows when:</span>
    <span class="d-sub">
      ✓ Task exceeds a single context window<br>
      ✓ Partition strategy is unknown in advance<br>
      ✓ Quality matters more than token economy
    </span>
  </div>
  <div class="d-box gray">
    <span class="d-label">Skip Workflows when:</span>
    <span class="d-sub">
      ✗ Repeatable, predictable token budgets<br>
      ✗ Small tasks needing a single agent pass<br>
      ✗ Exact consistency matters over adaptability
    </span>
  </div>
</div>

All three conditions must be true to justify the workflow overhead.

<!-- Speaker notes:
CREDIBILITY — Knowing limits builds trust.
"Knowing when NOT to use a tool is half the skill."
"If you can write down exactly what agents will do before you run — use a custom subagent instead."
"Workflows earn their cost only when the problem's shape is genuinely unknown."
-->

---

## This Project's Structure

<span class="kicker">ClaudeWorkflows — the repo behind this deck</span>

<div class="tree">
<span style="color:#d97757;font-weight:700">ClaudeWorkflows/</span>
├── <span style="color:#faf9f5">CLAUDE.md</span>  <span style="color:#6a7560">· project rules — never invent, always cite</span>
├── <span style="color:#d97757">01–08-official-docs-*.md</span>  <span style="color:#6a7560">· curated extracts from code.claude.com</span>
├── <span style="color:#faf9f5">claude-code-parallel-agents-reference.md</span>
│
├── <span style="color:#d97757">plugin/</span>
│   ├── <span style="color:#6a9bcc">agents/</span>  <span style="color:#6a7560">workflow-architect · sub-agent-specialist</span>
│   ├── <span style="color:#6a9bcc">commands/</span>  <span style="color:#6a7560">orchestrate · parallel-sweep · workflow-review</span>
│   └── <span style="color:#6a9bcc">skills/</span>  <span style="color:#6a7560">dynamic-workflows · parallel-patterns · error-handling · headless-orchestration</span>
│
├── <span style="color:#d97757">.claude/agents/</span>  <span style="color:#6a7560">← session-scoped specialists</span>
│   ├── <span style="color:#788c5d">analyze.md</span>  · <span style="color:#788c5d">planning.md</span>  · <span style="color:#788c5d">html-css.md</span>  · <span style="color:#788c5d">marp.md</span>
│
└── <span style="color:#d97757">presentations/</span>
    ├── <span style="color:#788c5d">multi-agent-workflows.md</span>  <span style="color:#6a7560">← v1</span>
    └── <span style="color:#788c5d">multi-agent-workflows-v2.md</span>  <span style="color:#6a7560">← you are here</span>
</div>

<!-- Speaker notes:
ORIENTATION — Ground the abstract in something concrete.
"Orange = reference docs. Blue = plugin layer. Green = session agents."
"The plugin is what makes the patterns from the reference docs available as defaults — not extras you have to remember."
-->

---

## How the Pieces Connect

<div class="d-flow" style="margin:8px 0">
  <div class="d-box orange" style="flex:2">
    <span class="d-label">Reference Docs (01–08)</span>
    <span class="d-sub">Official Claude Code documentation, curated and formatted. Every claim has a source URL. Nothing invented.</span>
  </div>
  <div class="d-arrow">→</div>
  <div class="d-box blue">
    <span class="d-label">Plugin</span>
    <span class="d-sub">Skills, agents, and commands that operationalise the patterns from the docs. The practice layer.</span>
  </div>
</div>
<div class="d-flow" style="margin:8px 0">
  <div class="d-box green">
    <span class="d-label">.claude/agents/</span>
    <span class="d-sub">Session specialists loaded automatically in every Claude Code session: <strong>analyze</strong>, <strong>planning</strong>, <strong>html-css</strong>, <strong>marp</strong>.</span>
  </div>
  <div class="d-arrow">→</div>
  <div class="d-box orange" style="flex:2">
    <span class="d-label">presentations/</span>
    <span class="d-sub">Human-readable synthesis sourced entirely from the reference docs. The marp agent in .claude/agents/ generated this deck.</span>
  </div>
</div>

<!-- Speaker notes:
META — Audiences appreciate the self-referential.
"The tool used to make this presentation is documented in this presentation."
"The plugin skills described in Part 3 were active when this deck was generated."
"This is what a well-structured Claude Code project looks like — agents and skills are part of the codebase."
-->

---

<!-- _class: lead -->

# Part 5
## Patterns that work

---

## Understand → Change → Verify

<span class="kicker">The canonical three-stage engineering workflow</span>

```javascript
// Stage 1: map what needs changing (cheap — ideal for Haiku model)
const map = await agent("Map all modules touching auth", { schema: MAP_SCHEMA })

// Stage 2: change each file in parallel, isolated worktrees prevent conflicts
const changes = await pipeline(
  map.files,
  file => agent(`Refactor auth in ${file}`, { isolation: 'worktree' })
)

// Stage 3: independent agents verify — no shared memory with stage 2
const verified = await parallel(changes.map(c => () =>
  agent(`Verify ${c.file} is correct after refactor`, { schema: VERDICT })
))
```

With `/effort ultracode`, Claude plans and runs this entire loop automatically.

<!-- Speaker notes:
PATTERN TRANSFER — The three stages are independently valuable.
"Stage 1 is cheap: read-only, run on Haiku."
"Stage 2 is isolated: worktrees mean two agents can't corrupt the same file."
"Stage 3 is adversarial: the verifier has no memory of what the changer wrote."
This three-stage loop is the foundation of every serious engineering workflow.
-->

---

## Getting Started

<span class="kicker">Four entry points — in order of investment</span>

<div class="diagram-grid cols-2" style="margin:14px 0">
  <div class="d-box orange">
    <span class="d-label">1 — Bundled workflow</span>
    <span class="d-sub"><code>/deep-research What changed in the Node.js permission model between v20 and v22?</code><br>Zero setup. Run now.</span>
  </div>
  <div class="d-box blue">
    <span class="d-label">2 — Plugin command</span>
    <span class="d-sub"><code>/orchestrate &lt;task description&gt;</code><br>Claude designs a 4-phase workflow, shows you the script, and launches it with <code>ultracode</code>.</span>
  </div>
  <div class="d-box green">
    <span class="d-label">3 — Write a custom subagent</span>
    <span class="d-sub">Add <code>.claude/agents/your-specialist.md</code> with a precise description. It auto-invokes when the task matches.</span>
  </div>
  <div class="d-box gray">
    <span class="d-label">4 — Session-wide orchestration</span>
    <span class="d-sub"><code>/effort ultracode</code> — Claude evaluates every request and deploys workflows automatically.</span>
  </div>
</div>

Start at 1. Graduate to 4.

<!-- Speaker notes:
CALL TO ACTION — Something to do today.
"You can run step 1 right now."
"Step 2 uses the plugin you just learned about — /orchestrate is the fastest path to a custom workflow."
"Step 3 is the highest ROI: one subagent definition, used on every PR forever."
"Don't skip to 4 before you've seen a few workflow outputs."
-->

---

## The Rule That Governs All of This

<span class="kicker">Summary</span>

<div class="diagram-grid cols-3" style="margin:14px 0">
  <div class="d-box orange">
    <span class="d-label">Subagents</span>
    <span class="d-sub">Context flooding · task isolation · least-privilege enforcement</span>
  </div>
  <div class="d-box blue">
    <span class="d-label">Agent Teams</span>
    <span class="d-sub">Interdependent parallel work · inter-agent awareness · shared git workspace</span>
  </div>
  <div class="d-box green">
    <span class="d-label">Dynamic Workflows</span>
    <span class="d-sub">Scale · adversarial verification · resumability · unknown-shape discovery</span>
  </div>
</div>

The plugin gives you these patterns **as defaults, not extras** —  
skills you invoke, agents that self-select, commands that write the scripts.

The shift isn't about using **more AI.**  
It's about choosing the **execution model that fits the structure of your problem.**

<!-- Speaker notes:
SYNTHESIS — Bring it home.
"Four models. Each solves a different structural problem."
"The plugin bridges the gap between knowing the patterns and using them by default."
"The skill isn't knowing all four — it's knowing which question finds the right one."
"Who holds the plan? Do workers need to talk? Is the partition known?"
Answer those three. The model selects itself.
-->

---

<!-- _class: impact -->

# The right tool<br>for the<br>problem's shape.

## That's the whole framework.

<!-- Speaker notes:
CLOSE ON THE THEME.
"You will forget the names. You will not forget this question: does my problem have a known shape?"
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
Brief. They have the deck.
"Questions?"
-->
