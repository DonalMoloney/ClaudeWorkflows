---
marp: true
theme: default
paginate: true
html: true
backgroundColor: #faf9f5
color: #141413
style: |
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&family=Lora:ital,wght@0,400;0,500;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap');

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

  section {
    font-family: 'Lora', Georgia, serif;
    background: var(--light);
    color: var(--dark);
    padding: 52px 72px 48px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    line-height: 1.65;
  }
  section::before { display: none; }
  section::after {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.6em; color: var(--mid-gray); letter-spacing: 0.1em;
  }

  h1 {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 800; font-size: 2em; color: var(--orange);
    letter-spacing: -0.02em; line-height: 1.1;
    margin-bottom: 0.2em; border: none; padding: 0;
  }
  h2 {
    font-family: 'Lora', Georgia, serif; font-style: italic;
    font-size: 1.05em; color: var(--mid-gray);
    margin-bottom: 1em; border: none; padding: 0;
  }
  h2::after { display: none; }

  p  { color: var(--dark); line-height: 1.7; font-size: 0.93em; }
  li { color: var(--dark); margin: 0.22em 0; line-height: 1.55; font-size: 0.91em; }
  strong { color: var(--dark); font-weight: 600; }
  em { color: var(--mid-gray); }

  blockquote {
    border-left: 4px solid var(--orange);
    background: var(--orange-mute);
    padding: 11px 18px; margin: 10px 0;
    color: var(--dark); font-style: italic;
    border-radius: 0 8px 8px 0;
  }
  blockquote strong { color: var(--orange); font-style: normal; }

  code {
    font-family: 'JetBrains Mono', monospace;
    background: var(--light-gray); color: var(--orange);
    padding: 2px 7px; border-radius: 4px; font-size: 0.79em;
  }
  pre {
    background: var(--dark); border-radius: 10px;
    padding: 16px 20px; font-size: 0.65em; line-height: 1.55;
    box-shadow: 0 4px 20px rgba(20,20,19,0.18);
  }
  pre code { background: transparent; color: #c8c0b4; padding: 0; font-size: 1em; }

  table {
    font-size: 0.79em; border-collapse: separate; border-spacing: 0;
    width: 100%; border-radius: 8px; overflow: hidden;
    box-shadow: 0 2px 12px rgba(20,20,19,0.08);
    font-family: 'Lora', Georgia, serif;
  }
  th {
    background: var(--orange); color: var(--light); padding: 8px 14px;
    font-family: 'Poppins', Arial, sans-serif; font-weight: 700;
    font-size: 0.88em; text-align: left; border: none;
  }
  td { padding: 7px 14px; border-bottom: 1px solid var(--light-gray); background: var(--light); }
  tr:last-child td { border-bottom: none; }
  tr:nth-child(even) td { background: #f3f1eb; }

  section.impact {
    background: var(--orange) !important; justify-content: center;
  }
  section.impact::after { color: rgba(20,20,19,0.3); }
  section.impact h1 {
    font-family: 'Poppins', Arial, sans-serif; font-weight: 800;
    font-size: 3.6em; color: var(--dark);
    letter-spacing: -0.04em; line-height: 0.95; margin-bottom: 0.35em;
  }
  section.impact h2 {
    font-family: 'Lora', Georgia, serif; font-style: italic;
    color: rgba(20,20,19,0.55); font-size: 1em; margin-top: 0.5em;
  }
  section.impact p { color: rgba(20,20,19,0.7); font-size: 0.93em; }

  section.lead {
    background: var(--dark) !important; color: var(--light); justify-content: center;
  }
  section.lead::after { color: rgba(250,249,245,0.2); }
  section.lead h1 {
    font-family: 'Poppins', Arial, sans-serif; font-weight: 800;
    font-size: 2.6em; color: var(--light); letter-spacing: -0.03em; line-height: 1.05;
  }
  section.lead h2 {
    font-family: 'Lora', Georgia, serif; font-style: italic;
    color: var(--orange); font-size: 1em; margin-top: 0.5em;
  }
  section.lead p, section.lead li { color: var(--mid-gray); }
  section.lead strong { color: var(--light); }
  section.lead code { background: rgba(250,249,245,0.1); color: var(--orange); }

  .kicker {
    font-family: 'Poppins', Arial, sans-serif; font-size: 0.63em;
    font-weight: 700; letter-spacing: 0.22em; text-transform: uppercase;
    color: var(--orange); display: block; margin-bottom: 9px;
  }

  .diagram-grid { display: grid; gap: 9px; margin: 10px 0; }
  .diagram-grid.cols-2 { grid-template-columns: 1fr 1fr; }
  .diagram-grid.cols-3 { grid-template-columns: 1fr 1fr 1fr; }
  .diagram-grid.cols-4 { grid-template-columns: 1fr 1fr 1fr 1fr; }

  .d-box {
    border-radius: 8px; padding: 12px 16px; font-size: 0.76em;
    box-shadow: 0 2px 8px rgba(20,20,19,0.07);
    border: 1.5px solid transparent; font-family: 'Lora', Georgia, serif;
  }
  .d-box.orange { background: var(--orange-mute); border-color: var(--orange); }
  .d-box.blue   { background: var(--blue-mute);   border-color: var(--blue);   }
  .d-box.green  { background: var(--green-mute);  border-color: var(--green);  }
  .d-box.gray   { background: var(--light-gray);  border-color: var(--mid-gray); }
  .d-box.dark   { background: var(--dark); color: var(--light); border-color: var(--dark); }

  .d-label {
    font-family: 'Poppins', Arial, sans-serif; font-weight: 700;
    font-size: 0.94em; display: block; margin-bottom: 4px;
  }
  .d-box.orange .d-label { color: var(--orange); }
  .d-box.blue   .d-label { color: var(--blue);   }
  .d-box.green  .d-label { color: var(--green);  }
  .d-box.gray   .d-label { color: var(--dark);   }
  .d-box.dark   .d-label { color: var(--orange); }
  .d-sub { color: var(--mid-gray); line-height: 1.42; }
  .d-box.dark .d-sub { color: rgba(250,249,245,0.55); }

  .d-flow { display: flex; align-items: stretch; gap: 8px; margin: 8px 0; }
  .d-flow > .d-box { flex: 1; }

  .arch-row { display: flex; align-items: stretch; gap: 12px; }
  .arch-box {
    border-radius: 8px; padding: 13px 17px;
    background: var(--blue-mute); border: 1.5px solid var(--blue);
    display: flex; flex-direction: column; gap: 5px;
  }
  .arch-box.hi { background: var(--orange-mute); border-color: var(--orange); }
  .arch-label {
    font-family: 'Poppins', Arial, sans-serif; font-weight: 700;
    font-size: 0.88em; color: var(--blue); display: block;
  }
  .arch-box.hi .arch-label { color: var(--orange); }
  .arch-sub { font-size: 0.77em; color: var(--mid-gray); line-height: 1.48; display: block; }
  .arrow {
    display: flex; align-items: center; justify-content: center;
    color: var(--mid-gray); font-size: 1.3em; padding: 0 3px; flex-shrink: 0;
  }

  .rule {
    border-left: 3px solid var(--orange);
    padding: 7px 13px; margin: 5px 0; font-size: 0.84em; line-height: 1.52;
  }
  .rule-title {
    font-family: 'Poppins', Arial, sans-serif; font-weight: 700;
    color: var(--dark); display: block; margin-bottom: 2px;
  }
  .rule-body { color: var(--mid-gray); }

  .plugin-cmd {
    font-family: 'JetBrains Mono', monospace;
    background: var(--dark); color: var(--orange);
    padding: 6px 12px; border-radius: 6px; font-size: 0.82em;
    display: inline-block; margin-bottom: 5px;
  }
---

<!-- _class: impact -->

# You've Been<br>Using Workflows<br>for Years.

## Here's what was always missing. — v9 · 15 min

<!--
OPEN COLD. Three full seconds of silence.

"Before we start — raise your hand if your team runs a CI pipeline. GitHub Actions. Jenkins. CircleCI. Anything."

Pause. Most hands up.

"Keep it up if your team uses something like Zapier, Make, or n8n to connect tools together."

More hands.

"Keep it up if you've ever written an Airflow DAG, a LangChain chain, or a CrewAI crew."

"Now put your hands down — and notice that almost everyone's hand was up for at least one of those. Every single one of those is a workflow. You've been building them for years. This talk is not about a new category. It's about the one constraint every single one of those tools shared — and what happens when you remove it."
-->

---

## Your Team Is Already Running Workflows

<span class="kicker">The tools change. The constraint doesn't.</span>

<div class="diagram-grid cols-2" style="margin: 14px 0">
  <div class="d-box orange">
    <span class="d-label">GitHub Actions / Jenkins / CircleCI</span>
    <span class="d-sub">CI/CD pipelines. You write the jobs, the dependencies, the triggers — before the first commit lands. New environment or compliance step: edit the YAML, push, wait for the run.<br><br><em>The pipeline doesn't discover it needs an extra check. You tell it.</em></span>
  </div>
  <div class="d-box blue">
    <span class="d-label">Zapier / Make.com / n8n</span>
    <span class="d-sub">Trigger → action chains. Form submitted → Slack alert → log to Notion → create task in Linear. You design the flow before it handles a single event. New tool in the stack: rebuild the chain.<br><br><em>The automation doesn't decide mid-run to add a step. You do — before.</em></span>
  </div>
  <div class="d-box green">
    <span class="d-label">Airflow / Prefect / dbt</span>
    <span class="d-sub">Data pipelines. Every node, every dependency, every schedule written before the first byte moves. New data source: add a task node, redefine the schema, redeploy the DAG.<br><br><em>The DAG doesn't discover new sources. You enumerate them.</em></span>
  </div>
  <div class="d-box gray">
    <span class="d-label">LangChain / CrewAI / AutoGen</span>
    <span class="d-sub">LLM chains and agent crews. Prompt templates, routing logic, agent roles — all defined before the first token fires. New routing rule or agent: rewrite the chain topology.<br><br><em>The crew doesn't spawn more agents when the work grows. You redesign it.</em></span>
  </div>
</div>

> Every tool. Different era. Same last column: **you write the shape before anything runs.**

<!--
RECOGNITION FIRST. The audience should see their own tool in one of these boxes.

"GitHub Actions — every developer in this room has written this YAML. You know exactly what I mean when I say: new environment, edit the file."

"Zapier — if you're in ops or marketing, this is your workflow tool. You built the zap. You reconnect it every time the stack changes."

"Airflow — if you work in data engineering, this is your world. The DAG is your job description."

"LangChain or CrewAI — if you've been building with LLMs over the last two years, you've hit this wall: the chain is written before the first token fires."

Pause.

"The tools are completely different. The italicised line at the bottom of each box is identical. Every single one requires you to define the shape before the work begins. That is not a coincidence. It is architecture."
-->

---

<!-- _class: impact -->

# The topology<br>was always<br>frozen.

## You couldn't orchestrate what you hadn't already mapped.

<!--
THESIS. Say this once, clearly, slowly.

"I want to name this before we go any further, because it's easy to live inside a constraint so long you stop seeing it."

Two-second pause.

"Every tool we just saw — GitHub Actions, Zapier, Airflow, CrewAI — requires the same thing from you. Before the first job runs, before the first event fires, before the first agent wakes up: you have written the complete graph of what will happen. Every node. Every edge. Every branch condition."

Pause.

"That sounds like a limitation of implementation — something a smarter version of the tool might fix. It is not. It is a limitation of architecture. The human holds the graph. The graph is frozen. And if the work turns out to have a different shape than you expected — you rewrite it. Every time. That is the constraint this talk is about."
-->

---

## What Happens When the Shape Is Wrong

<span class="kicker">The two failure modes no tool escapes</span>

<div class="diagram-grid cols-2" style="margin: 12px 0">
  <div class="d-box orange">
    <span class="d-label">The maintenance trap</span>
    <span class="d-sub">
      Your API grows from 3 routes to 400. Your pipeline now has 400 hand-written task nodes — each one a code change and a redeploy.<br><br>
      Compliance requires verification of findings: 400 more nodes, by hand.<br>
      New microservice ships Wednesday: rewrite, redeploy.<br><br>
      <strong>The graph mirrors the complexity it was supposed to manage.</strong>
    </span>
  </div>
  <div class="d-box blue">
    <span class="d-label">The context wall</span>
    <span class="d-sub">
      You try a single Claude session instead. Smarter about unknown shapes — until context fills at ~30 files.<br><br>
      It cannot independently verify its own output. A model cannot audit itself.<br>
      Interrupted: starts from zero.<br><br>
      <strong>Better than nothing. Breaks the same way at scale.</strong>
    </span>
  </div>
</div>

> Neither tool fails because it's badly built. Both fail because **the human must hold the graph.**
> The topology problem isn't a bug. It's the architecture.

<!--
BOTH FAILURE MODES COME FROM THE SAME ROOT.

"Left box: imagine your team's GitHub Actions workflow is now 400 jobs. Every new service, every new environment check, every new compliance requirement — added by hand, reviewed by hand, deployed by hand. The workflow is no longer a solution. It's a second codebase you maintain in parallel."

"Right box: you try a single Claude session. It's better than Airflow for unknown shapes — until the context window fills. At about 30 files, it stops. It can't check its own work. If you close the tab, it starts over from zero."

"Both fail for the same architectural reason. The human holds the graph. At scale, that means every graph change is a human change. And the graph changes constantly."

Pause.

"So: what does an architecture look like where the human doesn't hold the graph?"
-->

---

## How Claude's Approach Is Different

<span class="kicker">One inversion. Every prior constraint lifted.</span>

```python
# Every prior tool — you write 400 task nodes before a single file is read
audit_1 = PythonOperator(task_id="audit_route_1", ...)
audit_2 = PythonOperator(task_id="audit_route_2", ...)
# ... 398 more before execution begins ...
```

```javascript
// Claude dynamic workflows — the script discovers its own shape
const map = await agent("Find every route file in src/", { schema: FILES_SCHEMA })
// map.routes = whatever exists — 400 today, 412 next Tuesday — zero code change

const audits = await pipeline(map.routes,
  // Stage 1: audit each file — agents run concurrently
  route => agent(`Audit ${route.file} for missing auth`, { schema: AUDIT }),

  // Stage 2: adversarial verify — a FRESH agent with no memory of stage 1
  // tries to DISPROVE each finding. Default: refuted=true.
  // 3 skeptics per finding. 2/3 must confirm it to survive.
  audit => parallel(Array.from({ length: 3 }, () => () =>
    agent(`Refute: "${audit.finding}". Default refuted=true.`, { schema: VERDICT })
  )).then(votes => ({ ...audit, confirmed: votes.filter(v => !v.refuted).length >= 2 }))
)
```

<!--
THE CONTRAST IS EVERYTHING. Let both code blocks sit on screen together.

"Top: the prior approach. 400 task nodes written before a single file is read. Change the number of routes: edit the file. Add verification: add nodes. Every change is yours to make."

"Bottom: phase one — one agent, one line. It finds however many files exist. The pipeline handles them all. No code change when the number changes."

"But look at stage two. This is adversarial verification. A fresh agent — zero memory of stage one — is asked to disprove the finding. Its default answer is: refuted. It has to actively decide the finding is real. Three of these run per finding. Two of three must confirm it."

"This eliminates roughly 30 to 40 percent of false positives in practice. It is structurally impossible in a static pipeline — because you cannot write a verification stage before you know what findings will exist. The whole point is that it runs after."
-->

---

<!-- _class: impact -->

# The script<br>discovers its<br>own shape.

## Describe the work. Claude writes the graph. The runtime runs it.

<!--
TRANSITION. Pause before speaking.

"We've seen the principle. The architecture inverted — the graph writes itself. Now let's see what happens when the people who built this tool decided to find out exactly how far that inversion goes."

Pause.

"They didn't run a demo. They didn't build a tutorial. They built a C compiler."
-->

---

<!-- _class: lead -->

# The Hardest Test<br>They Could Think Of

## Anthropic's own engineers. Their own tool. Published results — including the failures.
## source: anthropic.com/engineering/building-c-compiler

<!--
SET THE CREDIBILITY ANCHOR BEFORE THE CONTENT.

"The rest of this talk is about one project. Before I describe what they built, let me tell you who ran it."

"Anthropic's engineering team — the people who designed and built Claude — decided to test their own tool by doing something they knew would be extremely hard. They didn't commission it to a customer. They didn't hire a research firm. They ran it themselves, on their own infrastructure, with their own engineers watching."

"And then they published everything. The architecture. The failures. The exact point where the system found its ceiling. This is the most authoritative test case that exists for multi-agent orchestration: the tool's creators, testing the tool's limits."

"The goal: build a working C compiler in Rust capable of compiling the Linux kernel."
-->

---

## What Anthropic's Engineers Set Out To Build

<span class="kicker">The goal — and why it required multi-agent from the first line of code</span>

A **C compiler** turns C source code into machine code your processor executes. GCC — the industry reference — represents **fifty years of engineering work**. The target: compile **Linux kernel 6.9** across x86, ARM, and RISC-V.

<div class="diagram-grid cols-3" style="margin: 11px 0">
  <div class="d-box orange">
    <span class="d-label">Too large for one context</span>
    <span class="d-sub">100,000 lines of Rust cannot be held as coherent design intent in any single context window. There is no single-session path. Distribution is not optional — it's the only option.</span>
  </div>
  <div class="d-box blue">
    <span class="d-label">Unknown partition</span>
    <span class="d-sub">You cannot pre-write the task graph. Failing tests discover themselves as the compiler grows. The GitHub Actions YAML for this project cannot be written before the first line exists.</span>
  </div>
  <div class="d-box green">
    <span class="d-label">Binary verifier</span>
    <span class="d-sub">Does the Linux kernel compile? Yes or no. This unambiguous signal is what makes agents self-sufficient. Without it, 16 agents confidently build the wrong thing for weeks.</span>
  </div>
</div>

| Agents | Sessions | Lines of Rust | API cost | Targets |
|---|---|---|---|---|
| 16 concurrent | ~2,000 total | 100,000 | ~$20,000 | Linux 6.9 — x86, ARM, RISC-V |

<!--
THREE BOXES ARE THREE PRIOR FAILURES ANSWERED DIRECTLY.

"Orange box: single Claude session hits the context wall at about 30 files. This project is 100,000 lines. There is no single-session path — it's not a workaround, it's arithmetic."

"Blue box: you cannot pre-write the GitHub Actions YAML for this project. The tasks don't exist until the code exists. The work discovers itself."

"Green box — and this one is the key to everything that follows. Does the Linux kernel compile? That question has a definitive answer. Yes or no. That binary signal is what makes 16 agents self-sufficient. Agents without a strong verifier don't fail loudly — they succeed quietly at the wrong thing."

"Now look at the numbers. Before they land, I want to give you a human reference point."
-->

---

## What 100,000 Lines Actually Means

<span class="kicker">A human reference point — before the numbers land</span>

<div class="diagram-grid cols-2" style="margin: 12px 0">
  <div class="d-box gray">
    <span class="d-label">The human equivalent</span>
    <span class="d-sub">
      A senior engineering team of 6–8 people producing a focused, production-quality 100,000-line codebase: <strong>6–12 months</strong>.<br><br>
      At $200k average fully-loaded cost per engineer: <strong>$600,000 – $1,600,000</strong>.<br><br>
      That budget buys architecture reviews, sprint planning, code reviews, on-call, and the institutional memory that stops this quarter's feature from breaking last quarter's.
    </span>
  </div>
  <div class="d-box orange">
    <span class="d-label">What 16 agents produced</span>
    <span class="d-sub">
      100,000 lines across ~2,000 sessions<br>
      <strong>Weeks, not months</strong><br>
      <strong>~$20,000 in API cost</strong><br><br>
      No architecture reviews.<br>
      No sprint planning.<br>
      No institutional memory.<br><br>
      Merge conflicts: resolved autonomously.<br>
      Task locking: automated via git.<br>
      Test verification: continuous.
    </span>
  </div>
</div>

> The agents found the ceiling where a human team finds it too: **new features started breaking existing ones** — when no single context could hold the architecture.

<!--
THE HUMAN REFERENCE POINT IS WHAT MAKES THE NUMBERS LAND.

"Before $20,000 means anything, you need to hear $600,000 to $1,600,000. That's the fully-loaded cost of a human engineering team producing 100,000 lines of focused, production-quality code. Six to eight senior engineers. Six to twelve months. Architecture reviews. Sprint planning. The person who remembers why the compiler's type system was designed the way it is."

"The agents produced 100,000 lines in weeks at $20,000."

Pause.

"And they hit exactly the same ceiling a human team hits at this scale: new features started breaking existing ones. Not because the agents were poorly designed. Because the system's complexity exceeded what any single context could hold as coherent design intent. The same thing that forces human teams to hold architecture reviews. The ceiling is the same. The distance to it is not."
-->

---

## The Coordination Architecture

<span class="kicker">How 16 agents shared one codebase — with four lines of bash</span>

```bash
# The entire orchestration loop — four lines, running ~2,000 times
while true; do
    claude --dangerously-skip-permissions -p "$(cat AGENT_PROMPT.md)" --model claude-opus-4-6
done
```

<div class="arch-row" style="margin: 11px 0">
  <div class="arch-box" style="flex: 3">
    <span class="arch-label">What one agent does each loop</span>
    <span class="arch-sub">
      1. Read <code>current_tasks/</code> — find an unclaimed failing test<br>
      2. Write a lock file → <strong>git commit</strong> (atomic: if two agents race, git rejects one — it picks a different task)<br>
      3. Work in a Docker container with its own <code>/workspace</code> clone of the repo<br>
      4. Implement a fix → run the full test suite<br>
      5. Green: <code>git push</code>. Conflict: resolve autonomously. Fail: abandon, pick new task<br>
      6. Loop — next unclaimed task
    </span>
  </div>
  <div class="arch-box hi" style="flex: 2">
    <span class="arch-label">Git as the entire scheduler</span>
    <span class="arch-sub">
      Git's atomic commit IS the distributed mutex.<br><br>
      No Redis. No Celery. No task queue. No central coordinator. No ops overhead.<br><br>
      <strong>The simpler the coordination, the fewer failure modes.</strong>
    </span>
  </div>
</div>

<!--
LET THE FOUR LINES LAND BEFORE YOU EXPLAIN THEM.

"This is the entire orchestration. Four lines of bash. That ran approximately 2,000 times over weeks. The sophistication is not in the loop — it is in the agent prompt file and the quality of the test suite."

"The execution cycle: wake up, find an unclaimed failing test, commit a lock file, do the work in an isolated Docker container, push if green, abandon if stuck. Loop. That's all 16 agents doing, in parallel, for weeks."

Pause. Point to the right box.

"Now: git's atomic commit is the distributed mutex. Not Redis. Not a task queue. Just git, doing what git already does — guaranteeing exactly one writer can hold a lock at a time. The second agent to try gets a rejection, picks a different task, and moves on."

"No infrastructure overhead. No operations burden. The coordination mechanism is so simple it barely qualifies as a system. That simplicity is the point."
-->

---

## The Team of 16 — Roles, Not Just Headcount

<span class="kicker">Why the composition mattered as much as the scale</span>

| Role | Why it exists |
|---|---|
| **Core compiler developers** (~11) | Fix failing tests independently — GCC used as oracle so no two agents ever block on the same issue simultaneously. |
| **Coalescing agent** (1) | 11 agents working in parallel will independently write similar utility functions. A dedicated merger turns duplication from a problem into an operation — and it scales. |
| **Performance + efficiency specialists** (2) | Without dedicated roles, optimisation is always deprioritised. These run continuously against the live codebase state. |
| **Design critic** (1) | Architectural debt accumulates invisibly when every agent chases passing tests. This role sees structure, not features. |
| **Docs maintainer** (1) | Stale READMEs degrade every other agent. Clean context in, clean output out. This role maintains the feedback loop. |

> **The coalescing agent is the key insight.** Don't prevent duplication — clean it up. Cheaper, parallelisable, scales with team size.

<!--
WALK EACH ROLE. EACH WAS A DELIBERATE DECISION.

"Eleven core developers — the word independent is load-bearing. GCC is used as an oracle: any C code GCC compiles is a valid test. Each agent picks from a pool of independent failing tests. No two agents block on the same issue."

"The coalescing agent: you cannot prevent two independently-running agents from writing the same utility function. If you try to prevent it, you need coordination, which kills parallelism. So instead — accept it, and dedicate one agent to merging continuously. Duplication becomes maintenance, not a crisis."

"Performance specialists and design critic: without dedicated roles, these are always deprioritised. Every feature-focused agent has more urgent work. Dedicated roles run them continuously."

"Docs maintainer — the most underrated role. Stale READMEs aren't a cosmetic problem. An agent reading 3,000 tokens of outdated context produces worse work than an agent reading a clean 200-word summary. The docs maintainer is infrastructure."
-->

---

## Five Rules Paid for by 2,000 Sessions

<span class="kicker">Not best practices — hard limits discovered by running the system into failure</span>

<div class="rule">
  <span class="rule-title">Your verifier is your product.</span>
  <span class="rule-body">If the verifier has gaps, 16 agents optimise for the gaps. 16× the wrong answer. The test suite is not supporting infrastructure — it is the entire signal that makes autonomous work possible.</span>
</div>
<div class="rule">
  <span class="rule-title">Context flooding kills quality.</span>
  <span class="rule-body">An agent reading thousands of tokens of noise underperforms one reading a clean summary. Maintain READMEs and progress files. The docs maintainer role exists because this was discovered from real degradation, not anticipated in advance.</span>
</div>
<div class="rule">
  <span class="rule-title">Decompose for independence.</span>
  <span class="rule-body">Agents excel at bounded tasks with verifiable outputs. Tasks that require coordinating changes across multiple files simultaneously need a human architectural decision first.</span>
</div>
<div class="rule">
  <span class="rule-title">Git conflict resilience is real.</span>
  <span class="rule-body">Claude resolves merge conflicts autonomously. Do not reduce parallelism to avoid them. Design for conflicts. Expect them. Let the system handle them.</span>
</div>
<div class="rule">
  <span class="rule-title">Orchestration scales execution. It does not scale understanding.</span>
  <span class="rule-body">At some complexity threshold, no agent holds coherent design intent across the full system. Parallel agents cannot substitute for a human who understands the architecture. That ceiling is real — and worth knowing before you hit it.</span>
</div>

<!--
EACH RULE WAS PAID FOR WITH A REAL FAILURE.

"Rule one: the adversarial verify pattern from the earlier slide is the same principle applied to findings. If the verifier is weak, agents confidently optimise for the wrong thing. You need a strong signal."

"Rule two: not anticipated. Discovered. When output quality degraded as the codebase grew, Anthropic's engineers traced it back to context flooding. Clean summaries became a performance requirement."

"Rule three: agents excel on bounded tasks. Architecture that spans multiple files — that's a human decision. Don't delegate it."

"Rule four: don't reduce parallelism to avoid git conflicts. The system handles them. Design for it."

Full pause before rule five.

"Rule five. Write this down: orchestration scales execution. It does not scale understanding. I'm going to show you exactly where that line is."
-->

---

## What They Built — and Where the Ceiling Was

<span class="kicker">The honest ledger — published by Anthropic's engineering team</span>

<div class="d-flow" style="margin: 13px 0">
  <div class="d-box dark" style="flex: 1">
    <span class="d-label">Built</span>
    <span class="d-sub">
      100,000-line Rust C compiler<br>
      Compiles Linux 6.9 — x86, ARM, RISC-V<br>
      ~2,000 sessions over weeks of parallel work<br>
      Autonomous merge conflict resolution<br>
      Git-based task locking across 16 agents<br>
      Zero human intervention per individual task
    </span>
  </div>
  <div class="d-box orange" style="flex: 1">
    <span class="d-label">Ceiling found</span>
    <span class="d-sub">
      No efficient 16-bit x86 code generation<br>
      No own assembler or linker<br>
      Less optimised output than mature compilers<br>
      <strong>New features frequently broke existing ones</strong><br>
      Design coherence degraded past ~100,000 lines<br>
      Claude Opus 4.6 approached capability limits here
    </span>
  </div>
</div>

> **New features breaking existing ones is the signal.** The system's complexity exceeded what any agent could hold as design intent. That is not a bug — it is the honest boundary of what orchestration can do.

<!--
READ BOTH COLUMNS WITH EQUAL WEIGHT. The ceiling is as important as the capability.

"Left: a working C compiler that compiles the Linux kernel across three processor architectures. 100,000 lines of Rust. Zero human intervention per individual task. That is extraordinary."

"Right: and Anthropic's engineers published this column too. New features frequently broke existing ones. Design coherence degraded past 100,000 lines. Claude Opus 4.6 approached its limits here."

Pause.

"The bolded line in the right column — new features breaking existing ones — is the signal. The same thing that forces human engineering teams to hold architecture reviews and maintain institutional memory happened here. No single context could hold the full architecture of a 100,000-line compiler."

"Orchestration scales execution. Understanding still needs to come from somewhere. The ceiling is real. It is also a very high ceiling — one that the vast majority of problems you're working on will never reach."
-->

---

<!-- _class: impact -->

# 100,000 lines.<br>16 agents.<br>$20,000.

## Anthropic's engineers built this. The ceiling found. The results published.

<!--
PAUSE. Three full seconds.

"A hundred thousand lines of Rust."

"Sixteen agents."

"Twenty thousand dollars."

"The Linux kernel compiled."

Pause again.

"And then — exactly as predicted — the ceiling was found. When the system's complexity exceeded what any single context could hold, new features started breaking old ones. The same ceiling any large human engineering team finds."

"Except the human team takes twelve months and a million dollars to find it."

"The ceiling is the same. The distance to it is not."
-->

---

## The Plugin That Makes It Yours

<span class="kicker">parallel-workflows — built for Claude Code, available today</span>

<div class="diagram-grid cols-3" style="margin: 14px 0">
  <div class="d-box orange">
    <span class="d-label">/orchestrate &lt;task&gt;</span>
    <span class="d-sub">
      One command. Claude designs the full workflow for your specific problem — discover → execute → verify → synthesize — shows you the script, explains each phase in one sentence, and launches it when you say <code>ultracode</code>.<br><br>
      <em>The C compiler team engineered this from scratch. You run one command.</em>
    </span>
  </div>
  <div class="d-box blue">
    <span class="d-label">parallel-patterns skill</span>
    <span class="d-sub">
      Five orchestration patterns, copy-paste ready:<br><br>
      <strong>Adversarial verify</strong> — 3 skeptics per finding<br>
      <strong>Loop-until-dry</strong> — unknown-size discovery<br>
      <strong>Judge panel</strong> — wide solution spaces<br>
      <strong>Multi-modal sweep</strong> — parallel search angles<br>
      <strong>Completeness critic</strong> — final gap check
    </span>
  </div>
  <div class="d-box green">
    <span class="d-label">workflow-architect agent</span>
    <span class="d-sub">
      Describe the work in plain English. The agent selects the right pattern, writes the phases, handles the boilerplate, and explains every design decision.<br><br>
      <em>You describe the problem. The agent designs the script. The runtime executes it.</em>
    </span>
  </div>
</div>

> The gap between "I understand dynamic workflows" and "I'm running one in ten minutes" is this plugin.

<!--
THE PLUGIN IS THE BRIDGE.

"Everything we've seen today — the inversion, the adversarial verify, the discover-then-execute pattern — you now have a working mental model of how it works. But there's a gap between understanding a pattern and running it."

"This plugin closes that gap."

"Slash orchestrate followed by any task description: Claude designs the full workflow for your specific problem. Not a generic template — a workflow designed for what you're trying to do. It shows you the script, explains each phase, and launches when you add ultracode."

"The parallel-patterns skill is the pattern library: five orchestration patterns, each with working code, each with an explanation of when to use it. Adversarial verify, loop-until-dry, judge panel, multi-modal sweep, completeness critic. The Anthropic engineering team figured these out by running 2,000 sessions. They're pre-built for you."

"The workflow-architect agent: describe the work in plain English. The agent picks the right pattern, writes the script, explains the design."

"The gap between understanding this and running this is one command."
-->

---

## Your First Dynamic Workflow

<span class="kicker">From principle to running code — in under ten minutes</span>

<div style="display:grid; grid-template-columns: 1.5fr 1fr; gap: 16px; margin-top: 8px;">
<div>

```javascript
// Phase 1: one agent discovers the shape of the work
phase('Discover')
const map = await agent(
  "List every TypeScript file in src/",
  { schema: FILES_SCHEMA }
)

// Phase 2: one agent per file, concurrently
// 4 files today — 400 tomorrow — no code change
phase('Review')
const reviews = await pipeline(map.files,
  file => agent(
    `Review ${file} for security issues`,
    { schema: REVIEW_SCHEMA }
  )
)
return reviews.filter(r => r.issues.length > 0)
```

</div>
<div style="display:flex; flex-direction:column; gap:7px; padding-top:4px; font-size:0.9em;">

<div class="d-box orange">
  <span class="d-label">Fastest start</span>
  <span class="d-sub"><code>/orchestrate &lt;your task&gt;</code><br>Claude writes this script for your specific problem. Add <code>ultracode</code> to launch.</span>
</div>

<div class="d-box blue">
  <span class="d-label">When to reach for it</span>
  <span class="d-sub">
    <strong>Partition unknown?</strong> → Dynamic Workflow<br>
    <strong>Scale past one context?</strong> → Dynamic Workflow<br>
    <strong>Quality over speed?</strong> → Add adversarial verify
  </span>
</div>

<div class="d-box green">
  <span class="d-label">See it live first</span>
  <span class="d-sub"><code>/deep-research &lt;question&gt;</code><br>Bundled workflow. Live in under a minute. See phases, agents, drill-down — before writing a line.</span>
</div>

</div>
</div>

<!--
ACTIONABILITY. This slide should feel like permission to start today, not a reference doc.

"Here is the minimal pattern. Two phases, three primitives. Phase one discovers the shape of the work. Phase two fans out — one agent per item, running concurrently. The C compiler ran 2,000 variations of this across 16 agents. You can run your first variation in the next ten minutes."

"Fastest path: slash orchestrate, describe your task. Claude writes the script for your specific problem. Add ultracode to launch. That's it."

"If you want to see the system before you write anything: slash deep-research followed by any question you'd actually want answered. Bundled workflow, zero setup, shows you the phases and agents live."

"The three questions in the middle are your decision guide. Is the partition unknown — does the work discover its own shape at runtime? Does the scale exceed one context window? Does quality require independent verification? If any of those is yes, you're in dynamic workflow territory."
-->

---

<!-- _class: impact -->

# You describe it.<br>Claude encodes it.<br>The runtime runs it.

## code.claude.com/docs/en/workflows

<!--
CLOSE. Full silence first. Three seconds.

"The plan used to be yours to write. You designed the graph. You wrote every job in the YAML. You defined every trigger, every action, every branch. And every time the shape of the work changed — you rewrote it."

Pause.

"Now: describe the problem. Claude writes the graph. The runtime executes it."

Pause.

"We just watched Anthropic's own engineers push that one change as hard as it would go. Sixteen agents. Two thousand sessions. A hundred thousand lines. A ceiling found at exactly the complexity threshold any large engineering team finds — except in weeks, at $20,000, with published results."

"The ceiling is the same. The distance to it is not."

Full pause.

"Thank you."
-->
