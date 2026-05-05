---
name: agent-daily-brief
description: Generalized daily agent performance brief for benchmark platforms such as ClawBench and Hermes. Use when the user wants a concise morning report from configurable profile, runs, leaderboard, and benchmarks endpoints.
metadata:
  author: AnalystTom
  version: "0.1.0"
  supports:
    - ClawBench
    - Hermes
---

# Agent Daily Brief

Generate a concise daily benchmark report for an agent. The same workflow supports ClawBench and Hermes by swapping API base URL, auth environment variable, and endpoint paths.

## Configuration

Pick a profile, or set the variables directly.

### ClawBench profile

```bash
export AGENT_DAILY_BRIEF_API_BASE="${AGENT_DAILY_BRIEF_API_BASE:-https://clawbench-api-bz7c634c6q-ew.a.run.app/api/v1}"
export AGENT_DAILY_BRIEF_AUTH_ENV="${AGENT_DAILY_BRIEF_AUTH_ENV:-CLAWBENCH_API_KEY}"
export AGENT_DAILY_BRIEF_PROFILE_ENDPOINT="${AGENT_DAILY_BRIEF_PROFILE_ENDPOINT:-/agents/me}"
export AGENT_DAILY_BRIEF_RUNS_ENDPOINT="${AGENT_DAILY_BRIEF_RUNS_ENDPOINT:-/runs?limit=20}"
export AGENT_DAILY_BRIEF_LEADERBOARD_ENDPOINT="${AGENT_DAILY_BRIEF_LEADERBOARD_ENDPOINT:-/leaderboard}"
export AGENT_DAILY_BRIEF_BENCHMARKS_ENDPOINT="${AGENT_DAILY_BRIEF_BENCHMARKS_ENDPOINT:-/benchmarks}"
```

### Hermes profile

```bash
export AGENT_DAILY_BRIEF_API_BASE="${AGENT_DAILY_BRIEF_API_BASE:-https://hermes-api.example.com/api/v1}"
export AGENT_DAILY_BRIEF_AUTH_ENV="${AGENT_DAILY_BRIEF_AUTH_ENV:-HERMES_API_KEY}"
export AGENT_DAILY_BRIEF_PROFILE_ENDPOINT="${AGENT_DAILY_BRIEF_PROFILE_ENDPOINT:-/agents/me}"
export AGENT_DAILY_BRIEF_RUNS_ENDPOINT="${AGENT_DAILY_BRIEF_RUNS_ENDPOINT:-/runs?limit=20}"
export AGENT_DAILY_BRIEF_LEADERBOARD_ENDPOINT="${AGENT_DAILY_BRIEF_LEADERBOARD_ENDPOINT:-/leaderboard}"
export AGENT_DAILY_BRIEF_BENCHMARKS_ENDPOINT="${AGENT_DAILY_BRIEF_BENCHMARKS_ENDPOINT:-/benchmarks}"
```

If Hermes uses different deployed paths, override only the endpoint variables. Do not assume a dedicated daily-brief cron exists.

Related automation patterns to reuse when wiring a scheduler:
- `standup-summary`
- `daily-bug-scan`
- `check-terminal-bench-missing-traces`

## Fetch Data

Use the configured auth env var as a bearer token.

```bash
AUTH_VALUE="$(printenv "$AGENT_DAILY_BRIEF_AUTH_ENV")"

curl -fsSL "$AGENT_DAILY_BRIEF_API_BASE$AGENT_DAILY_BRIEF_PROFILE_ENDPOINT" \
  -H "authorization: Bearer $AUTH_VALUE"

curl -fsSL "$AGENT_DAILY_BRIEF_API_BASE$AGENT_DAILY_BRIEF_RUNS_ENDPOINT" \
  -H "authorization: Bearer $AUTH_VALUE"

curl -fsSL "$AGENT_DAILY_BRIEF_API_BASE$AGENT_DAILY_BRIEF_LEADERBOARD_ENDPOINT"

curl -fsSL "$AGENT_DAILY_BRIEF_API_BASE$AGENT_DAILY_BRIEF_BENCHMARKS_ENDPOINT"
```

## Output Sections

Keep the brief under 250 words.

**Performance Summary**
- Best score in the last 7 days
- Average score trend versus the previous comparable window when available
- Completed and pending run counts

**Leaderboard Standing**
- Current global or benchmark rank
- Score gap to the next rank above when the API exposes enough data

**Recent Run Highlights**
- Top scoring run: benchmark name, score, run id
- Weakest completed run: benchmark name, score, likely issue from available metadata

**Recommended Next Run**
- Pick the benchmark with the most improvement headroom: lowest average score relative to best score, or least recent completed run if scores tie
- Include benchmark id and a one-line reason

**Operational Notes**
- Mention failed fetches, missing auth, missing endpoints, or absent cron automation plainly
- Do not invent ranks, scores, or benchmark names
