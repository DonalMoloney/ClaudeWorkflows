---
name: Agent Error Handling
description: Use this skill when writing or reviewing workflow scripts that need to handle agent failures, design defensive schemas, or correctly place .filter(Boolean) after parallel and pipeline calls. Reference whenever a workflow will run at scale where individual agent failures are expected.
version: 1.0.0
---

## Why agents fail

An `agent()` call returns `null` when the agent:
- Exceeds its token budget
- Times out during a long tool call
- Has a required tool denied mid-run
- Exhausts schema validation retries without producing a valid response

Failures are expected at scale. A 200-agent workflow must tolerate individual failures without aborting the whole run.

## Always `.filter(Boolean)` after `parallel()`

After every `parallel()` call, filter before iterating:

```js
const results = await parallel(items.map(item => () =>
  agent(`Analyse ${item}`, {schema: RESULT_SCHEMA})
))
const valid = results.filter(Boolean)
log(`${valid.length}/${items.length} agents completed`)
```

Without `.filter(Boolean)`, a single `null` crashes any subsequent `.map()`, `.flatMap()`, or property access.

## Dropping failures inside `pipeline()` stages

A pipeline stage that throws drops that item to `null` and skips its remaining stages. Use `try/catch` to handle gracefully and log what was dropped:

```js
const results = await pipeline(
  items,
  async item => {
    try {
      return await agent(`Process ${item}`, {schema: RESULT_SCHEMA})
    } catch (err) {
      log(`Dropped ${item}: ${err.message}`)
      return null
    }
  },
  result => result
    ? agent(`Verify ${result.finding}`, {schema: VERDICT_SCHEMA})
    : null
)
const valid = results.filter(Boolean)
```

## Defensive schema design

Loose schemas cause agents to return partial data that breaks downstream aggregation. Four rules:

1. **Always list `required` fields** — don't rely on the model to include optional ones
2. **Use `enum` for bounded strings** — `{ type: 'string', enum: ['critical','high','medium','low'] }` prevents free-text values
3. **No nested objects as field values** — use flat fields (`authorName`, `authorEmail`) not `{ author: { name, email } }`
4. **Keep array item schemas flat** — only include fields your code will actually access

```js
const FINDING_SCHEMA = {
  type: 'object',
  properties: {
    file:        { type: 'string' },
    line:        { type: 'number' },
    severity:    { type: 'string', enum: ['critical', 'high', 'medium', 'low'] },
    description: { type: 'string' },
    fix:         { type: 'string' }
  },
  required: ['file', 'line', 'severity', 'description']
}
```

## Schema retry behaviour — do not add manual retries

When a `schema:` is provided, the model retries automatically on mismatch. The retry loop is built into the runtime. Adding a manual retry wrapper creates double-retry amplification: the agent retries internally, then your wrapper retries the whole agent call, multiplying token spend.

If an agent consistently fails schema validation, fix the schema or the prompt — not with a retry loop.

## Log what you drop

Never silently continue past a `null` result. A log line makes dropped items visible in the `/workflows` progress view:

```js
const valid = results.filter(Boolean)
if (valid.length < results.length) {
  log(`[warn] ${results.length - valid.length} agents returned null — coverage incomplete`)
}
```

This ensures the final report's consumer knows the result set may be incomplete.
