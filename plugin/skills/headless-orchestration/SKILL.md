---
name: Headless Shell Orchestration
description: Use this skill when the user wants to run parallel Claude sessions from the shell using `claude -p` without the full workflow runtime — for scripting, CI/CD pipelines, or cases where a lightweight fan-out is enough.
version: 1.0.0
---

## Two approaches to parallel agents

Claude Code offers two overlapping strategies for parallel multi-agent work:

| | Dynamic Workflows | Headless `claude -p` |
|---|---|---|
| Orchestration | JavaScript script in the runtime | Shell scripting (`xargs`, `parallel`, `for`) |
| Resume | Yes — completed agents return cached results | No — each invocation is independent |
| Progress view | `/workflows` with drill-down | Shell stdout/stderr |
| Structured output | `schema:` with retry-on-mismatch | `--output-format json` + manual parsing |
| Verification patterns | Built-in (adversarial, loop-until-dry) | Manual shell logic |
| Best for | Large, resumable, cross-verified runs | CI pipelines, simple fan-outs, scripting |

## The headless architecture

```
Shell orchestrator (bash/python script)
  ├── Decomposes task into N work items
  ├── Spawns N parallel `claude -p` processes
  ├── Each is a fresh, isolated Claude Code session
  └── Collects output via stdout/files/JSONL
         ↓
Each `claude -p` session
  ├── Isolated context window
  ├── Distinct tool access (controlled by --allowedTools)
  ├── Separate token budget
  └── No shared state with sibling sessions
```

> Each sub-agent is a fresh Claude Code session. It does not share memory or state with other sub-agents unless explicitly passed in the task prompt.

## Basic fan-out with xargs

```bash
# Run 8 parallel Claude sessions over TypeScript files
find src/ -name "*.ts" | xargs -P 8 -I{} \
  claude -p "Review {} for missing error handling. Output JSON only: {\"file\":\"{}\",\"issues\":[]}" \
  --output-format json 2>/dev/null >> results.jsonl

# Aggregate
jq -s '[.[] | select(.issues | length > 0)]' results.jsonl
```

`-P 8` sets max parallelism. Tune to your machine's core count and Claude's rate limits.

## Structured output with --output-format

```bash
claude -p "Analyse src/auth.ts for JWT vulnerabilities. 
Return JSON: {\"file\": string, \"vulns\": [{\"line\": number, \"severity\": string, \"description\": string}]}" \
  --output-format json \
  --allowedTools "Read,Grep"
```

`--output-format json` wraps the response in a JSON envelope. The model's text output is in `.result`. Combine with `jq` to extract.

## Passing context into sub-agents

Everything a sub-agent needs must be in its prompt — it has no shared state:

```bash
# Pass file contents inline
FILE_CONTENT=$(cat src/auth.ts)
claude -p "Review this code for security issues:\n\n${FILE_CONTENT}\n\nReturn JSON: {\"issues\":[]}"

# Pass structured context as JSON
CONTEXT='{"endpoints": ["/api/users", "/api/admin"], "authType": "JWT"}'
claude -p "Given this API context: ${CONTEXT}\nFind endpoints missing auth middleware."
```

## Aggregation patterns

### JSONL accumulation
```bash
# Each agent appends one JSON object per line
for file in $(find src/ -name "*.ts"); do
  claude -p "Review ${file} for bugs. JSON: {\"file\":\"${file}\",\"bugs\":[]}" \
    --output-format json >> results.jsonl &
done
wait   # wait for all background jobs

# Flatten and filter
jq -s '.[] | .result | fromjson | select(.bugs | length > 0)' results.jsonl
```

### GNU parallel for rate-limiting
```bash
# Install: brew install parallel
find src/ -name "*.ts" | parallel -j 6 \
  'claude -p "Audit {}: JSON {\"file\":\"{}\",\"issues\":[]}" --output-format json' \
  >> audit.jsonl
```

### Python orchestrator
```python
import subprocess, json, concurrent.futures

files = ["src/auth.ts", "src/routes.ts", "src/db.ts"]

def audit(f):
    result = subprocess.run(
        ["claude", "-p", f"Audit {f} for security issues. JSON: {{\"file\":\"{f}\",\"issues\":[]}}",
         "--output-format", "json"],
        capture_output=True, text=True
    )
    return json.loads(result.stdout)

with concurrent.futures.ThreadPoolExecutor(max_workers=6) as ex:
    results = list(ex.map(audit, files))

critical = [r for r in results if any(i['severity'] == 'critical' for i in r.get('issues', []))]
```

## When to use headless vs workflow runtime

Use **headless `claude -p`** when:
- You're in a CI/CD pipeline that shells out to tools
- The task is simple fan-out with no cross-verification needed
- You want full control over the orchestration in bash/Python
- You need to integrate with existing shell pipelines

Use the **workflow runtime** when:
- You need resume (long-running jobs that might be interrupted)
- You want adversarial verification built into the orchestration
- You want a `/workflows` progress view with drill-down
- The orchestration itself should be saved and rerun
