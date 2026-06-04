---
name: workflow-review
description: Review a Claude Code workflow script for correctness, pattern misuse, and quality issues. Reads the script at the given path, delegates critique to the workflow-architect agent across six dimensions, and returns a structured findings list with severity and suggested fixes.
version: 1.0.0
---

When the user runs `/workflow-review <path>`:

## Step 1 — Read the script

Read the file at `<path>`. If the path does not exist or was not provided, ask the user to supply the path or paste the script inline.

## Step 2 — Delegate to workflow-architect

Pass the full script content to the `workflow-architect` agent with this exact review brief:

---

Review this Claude Code workflow script across six dimensions. For each dimension, list findings as:
- **severity**: `error` (will cause failure), `warning` (degrades quality/performance), or `suggestion` (improves clarity/cost)
- **what**: one sentence describing the problem
- **fix**: one line describing the correction

**Dimension 1 — Meta declaration**
- Is `export const meta` the first statement in the script?
- Is it a pure literal — no variables, spreads, function calls, or template interpolation in its values?
- Do the entries in `meta.phases` match the `phase()` calls in the script body (same title strings)?

**Dimension 2 — Pipeline vs parallel correctness**
- Is `pipeline()` the default for multi-stage work over a list of items?
- Is `parallel()` used only where a barrier is genuinely needed — i.e., stage N needs ALL of stage N-1's results before it can proceed?
- Smell test: does any `parallel()` call have only a `.filter()` / `.map()` / `.flatMap()` immediately after it, then feed into another `parallel()`? That's a `pipeline()` in disguise.

**Dimension 3 — Label and phase assignment**
- Does every `agent()` call have a `label` option?
- Do `agent()` calls inside `pipeline()` or `parallel()` stages have `phase` explicitly set? (Without it, concurrent stages race on the global phase state and produce garbled progress output.)

**Dimension 4 — Schema quality**
- Are all schemas tight? Check: `required` array lists every field the downstream code accesses; `enum` used for bounded strings; no nested objects as field values.
- Are there `agent()` calls that clearly return structured data but lack `schema:`?

**Dimension 5 — No-random / no-Date compliance**
- Does the script use `Date.now()`, `Math.random()`, or `new Date()` with no arguments? (All three break deterministic resume.)
- Does the script use TypeScript syntax — type annotations (`: string`), interfaces (`interface Foo {}`), or generics (`Array<T>`)? (Causes a parse error before any agent runs.)

**Dimension 6 — Agent count and loop safety**
- Are there `while` loops without a dry-streak exit condition (e.g., `dry < 2`) or a `budget.remaining()` guard?
- Does the estimated total agent count look realistic given the 1,000-agent-per-run cap?

---

## Step 3 — Present findings

Format the output as:

```
ERRORS  (must fix — will cause failures)
  [E1] <Dimension> — <what> → <fix>

WARNINGS  (should fix — degrades quality or performance)
  [W1] <Dimension> — <what> → <fix>

SUGGESTIONS  (consider — improves clarity or cost)
  [S1] <Dimension> — <what> → <fix>

PASSED
  ✓ <Dimension> — no issues found
```

If there are no errors or warnings, say so explicitly. Do not invent findings to fill the report.
