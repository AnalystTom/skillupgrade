---
name: benchmark-wiki
description: ClawBench benchmark knowledge wiki. Use when the agent should build and consult a structured knowledge base of benchmark-specific strategies, output contracts, scoring quirks, and solved examples.
metadata:
  author: AnalystTom
  version: "1.0.0"
---

# Benchmark Wiki

Build and maintain a local knowledge base of benchmark strategies. Consult it before every run.

## Wiki Location

Store at `~/.clawbench/wiki/`. One markdown file per benchmark:
```
~/.clawbench/wiki/
├── bm_70cc7f3191a344ae94030715.md   # ClawBench Entry Test
├── bm_8ca0e33b67f3463b9d8e5ba7.md   # Prompt Injection (High)
└── bm_pac1_public.md                # PAC1
```

## Wiki File Structure

Each file follows this template:

```markdown
# {Benchmark Title}

## Output Contract
Exact stdout format required for scoring. Copy from submission-guide.

## Scoring Rules
How points are awarded. What causes zero scores.

## Common Failures
- Failure mode 1 — how to avoid it
- Failure mode 2 — how to avoid it

## Verified Strategies
- Strategy that produced score X on date Y
- Strategy that produced score X on date Y

## Solved Examples
Concrete input→output pairs that scored correctly.
```

## Workflow

**Before a run:**
1. Check if `~/.clawbench/wiki/{benchmark_id}.md` exists
2. If yes, read it and apply the verified strategies
3. If no, fetch the submission guide and create the file:
```bash
curl https://clawbench-api-bz7c634c6q-ew.a.run.app/api/v1/benchmarks/{benchmark_id}/submission-guide
```

**After a run:**
1. Read run output: `GET /api/v1/runs/{run_id}`
2. Update the wiki file with what worked, what failed, and the score achieved
3. Add any new solved examples

Wiki entries compound over time. An agent's second run on any benchmark should always outperform its first.
