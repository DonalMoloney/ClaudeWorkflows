# Ken Huang Substack: Claude Code Orchestration — Dynamic Workflows · Subagents · Agent Teams

> Source: https://kenhuangus.substack.com/p/claude-code-orchestration-dynamic
> Publication: Agentic AI by Ken Huang

---

## Core Thesis

> "The question is no longer 'How do I prompt Claude for this?' — it is 'What execution model best fits the structure of this problem?'"

This guide frames Claude Code's three orchestration primitives as execution models with distinct use cases, rather than interchangeable approaches.

---

## Dynamic Workflows

**Launch:** Research preview May 28, 2026
**Availability:** Max, Team, Enterprise plans + Claude API (Bedrock, Vertex AI)

### The Paradigm Shift

> "In standard Claude Code, Claude IS the orchestrator — making turn-by-turn decisions whose intermediate results accumulate in the context window. In Dynamic Workflows, a generated JavaScript script IS the orchestrator."

When `ultracode` appears in a prompt or `/effort ultracode` is active, Claude generates a JavaScript orchestration script rather than responding directly. The script manages loops, branching, variables, and fans work across tens to hundreds of parallel subagents.

### Key Technical Properties

| Property | Detail |
|---|---|
| Scale | Up to 1,000 agents per run (16 concurrent to bound local resources) |
| Adversarial verification | Independent agents attempt to refute findings; only survivor claims surface |
| Convergence iteration | Spawning continues until answers stabilize |
| Resumability | Caching-backed; paused runs resume with cached results instantly |
| Background execution | Runs don't block CLI |
| Inspection | `/workflows` drills into phases, inspects prompts, examines tool calls |

### `/deep-research`

Unlike standard search that accepts first plausible answers, `/deep-research` is explicitly designed to disprove its own findings. It fans searches across multiple angles, cross-checks sources, votes on claims internally, and includes only adversarially-tested claims.

### `/effort ultracode`

Claude evaluates each request and independently decides whether to deploy a workflow. When appropriate, it plans an understand-change-verify loop automatically:
1. One subagent cluster maps architecture
2. A second executes changes
3. A third verifies results

Higher token cost and time, but substantially improved success rates on multi-stage engineering tasks.

### When to Use Dynamic Workflows

Use when **all three** conditions align:
1. Task exceeds single context window
2. Split strategy is unknown in advance
3. Quality matters more than token economy

**Canonical use cases:**
- Large-scale migrations: consistent transformation across 500+ files
- Codebase-wide security audits: hunting specific vulnerability classes
- Exploratory bug hunts in unfamiliar codebases (no prior partition knowledge)
- Plans requiring stress-testing from multiple angles before commitment
- Tasks requiring independent adversarial review before results reach users

**When NOT to use:**
- Repeatable, well-defined tasks with predictable token budgets
- Known workflows where exact execution and cost consistency matter
- Small, bounded tasks needing a single agent pass

---

## Subagents

**Definition:** Temporary worker instances launched from the current session. Each gets an isolated context window, executes a task, and returns only a summary result. The calling session absorbs only final answers, never intermediate logs or raw output — keeping the orchestrating session clean and focused.

### Built-in Types

| Type | Access | Purpose |
|---|---|---|
| Explore | Read-only | Rapid codebase search without write risks |
| Plan | Read-only | Background context gathering during plan mode |
| General-purpose | Read + write | Mixed exploration and execution tasks |

### Custom Subagent Configuration

Markdown files with YAML frontmatter stored at:
- `.claude/agents/` — project-scoped
- `~/.claude/agents/` — user-scoped, cross-project

**Key fields:**
- `description` — primary trigger signal; Claude reads this to decide automatic invocation. **Precision here determines reliability.**
- `tools` — constrains which tools subagent can use (least-privilege enforcement)
- `model` — `sonnet`, `opus`, `haiku`, or `inherit`
- `permissionMode` — controls file-edit permissions and prompt behavior
- `memory` — optionally provides cross-conversation memory directory

### Subagents vs. Dynamic Workflows

| Aspect | Subagents | Dynamic Workflows |
|---|---|---|
| Who holds the plan | Human or Claude's reasoning | The generated JavaScript script |
| Split strategy | Decided by user/Claude | Script decides automatically |
| Orchestration burden | Human-directed | Shifts to the machine |
| Adversarial verification | Manual if at all | Built in; agents refute each other |

Dynamic Workflows internally spin up subagents composably. Subagents are execution units; Dynamic Workflows are the coordination layer above them.

### When to Use Subagents

Excel when consistent, cost-predictable execution of well-defined tasks is needed:
- Code review enforcing specific team style guides — define once, invoke on every PR
- Test generation against known frameworks — same prompt, tools, model every time
- Documentation updates following fixed templates — deterministic, auditable output
- Parallelizing independent tasks with no ordering dependencies
- Context isolation — offload long-running reads to keep main session clean

---

## Agent Teams

**Launch:** Research preview February 2026
**Requires:** Claude Code v2.1.32+, Opus 4.6 minimum

### Purpose

Enable multiple Claude instances working in parallel on different subtasks while coordinating through a git-based shared workspace. Unlike manual multi-instance setups, Agent Teams provide built-in coordination.

**Design intent:** Complex, interdependent work where multiple specialists produce output that eventually unifies into a single coherent artifact — typically a codebase.

### Architecture

Each member is a fully independent Claude session with its own context, tool access, and responsibility area. Coordination happens through shared git repository:
- Agents claim work items (avoiding duplication)
- Changes merge continuously as agents complete subtasks
- Conflicts resolve automatically via coordination layer
- Team keeps running until all claimed work completes and merges

> "Agents are not merely parallel workers — they're aware of each other's progress and output. Backend agents know when API contracts they conform to update from frontend agents."

### Agent Teams vs. Subagents

**Critical difference:** coordination model.

- **Subagents** are independent — no inter-communication or state-sharing. They receive tasks, complete, return results, disappear.
- **Agent Teams** are collaborative — members share working environment, observe each other's changes, coordinate around common artifacts over time.

---

## Decision Matrix

| Situation | Best choice |
|---|---|
| Task exceeds context window, split strategy unknown, quality > cost | Dynamic Workflows |
| Repeatable task, defined workflow, cost consistency matters | Custom Subagents |
| Complex interdependent work across multiple files/layers | Agent Teams |
| Simple delegation, context isolation, one-off task | Built-in Subagents (Explore/Plan) |
