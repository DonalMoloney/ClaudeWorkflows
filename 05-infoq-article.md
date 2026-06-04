# InfoQ Article: Claude Code Dynamic Workflows

> Source: https://www.infoq.com/news/2026/06/dynamic-workflows-claude-code/
> Published: June 2026

---

## What They Are

Dynamic Workflows is a Claude Code capability for handling complex software engineering tasks by coordinating multiple AI agents within a single workflow. Rather than manually configured teams, Claude generates workflows on demand based on user objectives.

---

## How They Work

1. **Dynamic Planning** — Claude generates an orchestration script tailored to the specific task
2. **Task Distribution** — work breaks into subtasks executed in parallel across specialized agents
3. **Verification** — findings are compared and validated before convergence
4. **Iteration** — process continues until results stabilize

> "Allows Claude to dynamically create orchestration scripts, break work into subtasks, run them in parallel, and validate results before presenting a final answer."

---

## Activation Methods

- Explicit request to create a workflow
- "Ultracode" setting — automatically determines when workflow approaches suit the task

---

## Progress Management

- Saves progress throughout execution
- **Interrupted runs can resume without restarting**

---

## Use Cases

- Investigating widespread bugs
- Managing large migrations
- Conducting security audits
- Reviewing performance
- Analyzing complex software architecture

---

## Availability

- Research preview for Claude Code users on Max, Team, and Enterprise plans
- Also available through Claude API and partners: Amazon Bedrock, Google Vertex AI, Microsoft Foundry

---

## Resource Warning

> "Dynamic Workflows can consume substantially more tokens" than typical sessions.

Starting with well-scoped tasks is recommended before committing to large runs.
