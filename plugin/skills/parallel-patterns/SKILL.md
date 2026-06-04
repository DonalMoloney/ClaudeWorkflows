---
name: Parallel Orchestration Patterns
description: Use this skill when writing or reviewing a workflow script and needing to pick the right orchestration pattern — adversarial verify, loop-until-dry, judge panel, multi-modal sweep, or completeness critic. Provides copy-paste patterns with explanations of when each applies.
version: 1.0.0
---

## The core insight

Processing a large task sequentially is inefficient. Parallel execution across multiple agents compresses hours into minutes. But naive parallelism is not enough — the patterns below add *quality* on top of speed: adversarial checking, diverse perspectives, and exhaustive coverage.

## Pattern 1 — Adversarial Verify

**When to use:** Any finding that would be acted on (a bug, a security issue, a migration candidate). Prevents plausible-but-wrong results from surviving.

**How it works:** Spawn N independent skeptics, each prompted to *refute* the claim. A finding only survives if a majority cannot refute it.

```js
async function adversarialVerify(finding, label) {
  const VERDICT = {
    type: 'object',
    properties: { refuted: { type: 'boolean' }, reason: { type: 'string' } },
    required: ['refuted']
  }
  const votes = await parallel(Array.from({length: 3}, (_, i) => () =>
    agent(`Try to refute this finding. Default to refuted=true if uncertain.\n\nFinding: ${finding}`, {
      label: `${label}:refute:${i}`,
      phase: 'Verify',
      schema: VERDICT
    })
  ))
  return votes.filter(Boolean).filter(v => !v.refuted).length >= 2
}
```

## Pattern 2 — Loop-Until-Dry

**When to use:** Discovery tasks where you don't know how many items exist — bugs, issues, edge cases. Keeps running until K consecutive rounds find nothing new.

**Key rule:** Dedup against `seen` (all ever-found items), NOT against `confirmed` (items that passed verification). If you dedup against confirmed, rejected items reappear every round and the loop never converges.

```js
const seen = new Set()
const results = []
let dry = 0

while (dry < 2) {
  const batch = await agent(
    `Find items we haven't seen yet. Already found: ${[...seen].join(', ') || 'none'}`,
    { schema: ITEMS_SCHEMA }
  )
  const fresh = batch.items.filter(i => !seen.has(i.id))
  if (!fresh.length) { dry++; continue }
  dry = 0
  fresh.forEach(i => seen.add(i.id))
  results.push(...fresh)
  log(`${results.length} found so far, dry streak reset`)
}
```

## Pattern 3 — Judge Panel

**When to use:** Design decisions, plan drafts, or architecture proposals where the solution space is wide. Better than iterating one attempt.

**How it works:** Generate N independent proposals from different angles, score with parallel judges, synthesize from the winner while grafting best ideas from runners-up.

```js
const ANGLES = ['risk-first', 'mvp-first', 'user-first', 'performance-first']
const PROPOSAL_SCHEMA = { type: 'object', properties: { approach: {type:'string'}, tradeoffs: {type:'string'}, score: {type:'number'} }, required: ['approach','tradeoffs','score'] }

const proposals = await parallel(ANGLES.map((angle, i) => () =>
  agent(`Draft a solution for: ${task}\nAngle: ${angle}. Rate your own proposal 1-10.`, {
    label: `propose:${angle}`, phase: 'Draft', schema: PROPOSAL_SCHEMA
  })
))
const ranked = proposals.filter(Boolean).sort((a,b) => b.score - a.score)
const synthesis = await agent(
  `Synthesize the best solution. Winner: ${ranked[0].approach}\nRunner-ups: ${ranked.slice(1).map(p=>p.approach).join('\n')}`,
  { phase: 'Synthesize' }
)
```

## Pattern 4 — Multi-Modal Sweep

**When to use:** When one search angle won't find everything. Each agent is blind to what others surface — diversity catches what redundancy can't.

```js
const FINDERS = [
  { key: 'by-directory', prompt: 'Search by directory structure for auth-related files' },
  { key: 'by-import',    prompt: 'Search by import statements for files importing auth modules' },
  { key: 'by-keyword',   prompt: 'Grep for auth, token, session, jwt, cookie keywords' },
  { key: 'by-route',     prompt: 'List all HTTP routes and identify which lack middleware' },
]

const sweepResults = await parallel(FINDERS.map(f => () =>
  agent(f.prompt, { label: `sweep:${f.key}`, phase: 'Sweep', schema: FILES_SCHEMA })
))

const allFiles = [...new Set(sweepResults.filter(Boolean).flatMap(r => r.files))]
```

## Pattern 5 — Completeness Critic

**When to use:** As a final pass after other patterns. Asks "what's missing?" and turns the answer into another round of work.

```js
const critic = await agent(
  `Review this set of findings and identify gaps: what search modality wasn't run, what claim is unverified, what source wasn't read?\n\nFindings: ${JSON.stringify(results)}`,
  { phase: 'Critique', schema: GAPS_SCHEMA }
)

if (critic.gaps.length > 0) {
  log(`Completeness critic found ${critic.gaps.length} gaps — running additional agents`)
  const followUp = await parallel(critic.gaps.map(gap => () =>
    agent(`Address this gap: ${gap.description}`, { phase: 'Gap Fill', schema: RESULT_SCHEMA })
  ))
  results.push(...followUp.filter(Boolean))
}
```

## Composing patterns

The patterns compose. A full exhaustive review looks like:

```
Multi-modal sweep → deduplicate → adversarial verify → completeness critic → loop-until-dry on gaps
```

Scale to what was asked for:
- "find any bugs" → a few finders, single-vote verify
- "thoroughly audit this" → full finder pool, 3-vote adversarial pass, synthesis stage, completeness critic
