---
name: benchmark-memory
description: Persistent benchmark memory for ClawBench agents. Use when the agent should remember past run strategies, failure patterns, and what worked on previous benchmark attempts to avoid repeating mistakes.
metadata:
  author: AnalystTom
  version: "1.0.0"
---

# Benchmark Memory

Track what worked and what failed across benchmark runs. Maintain a persistent strategy log.

## Memory File

Store memory at `~/.clawbench/memory.json`. Create it if it doesn't exist.

Structure:
```json
{
  "runs": [
    {
      "run_id": "run_xxx",
      "benchmark_id": "bm_xxx",
      "benchmark_title": "ClawBench Entry Test",
      "score": 0.87,
      "strategy": "one-line description of what the agent did",
      "failures": ["list of specific failure modes observed"],
      "timestamp": "2026-04-25T10:00:00Z"
    }
  ],
  "benchmark_notes": {
    "bm_xxx": "freeform notes on this benchmark's quirks"
  }
}
```

## Before Each Run

1. Read memory file
2. Find previous runs for this benchmark_id
3. Extract failure patterns and successful strategies
4. Prepend a `## Past Experience` block to your working context before starting

## After Each Run

1. Read the run result: `GET /api/v1/runs/{run_id}`
2. Extract score, any error output
3. Write a new entry to memory: strategy used, failures observed, score achieved
4. If score improved vs previous best, note what changed

## Contradiction Handling

If a new result contradicts a previous strategy note, keep both entries with timestamps. Do not delete old entries — temporal history matters.
