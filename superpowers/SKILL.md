---
name: superpowers
description: Software development methodology for coding agents — bundles 14 composable skills (TDD, brainstorming, systematic debugging, writing plans, subagent-driven development, worktrees, code review, verification-before-completion, etc.) that auto-trigger so the agent specs, plans, and executes disciplined work instead of jumping straight to code. Use when you want your agent to plan, test, and ship rigorously on real engineering tasks. Authored by Jesse Vincent (obra). Upstream: https://github.com/obra/superpowers
metadata:
  author: obra (Jesse Vincent)
  upstream: https://github.com/obra/superpowers
  vendored_by: AnalystTom/skillupgrade
  version: "vendored-2026-05-01"
---

# Superpowers

Superpowers is a complete software development methodology for coding agents, built on a set of composable sub-skills plus initial instructions that make the agent actually use them.

The core loop:

1. Tease a spec out of the conversation before writing code
2. Show the spec back in digestible chunks for sign-off
3. Produce an implementation plan emphasising real red/green TDD, YAGNI, and DRY
4. Execute via subagent-driven development with review and verification gates

This SKILL.md is a vendored pointer. The framework lives upstream at `obra/superpowers` and is best installed as a full plugin (it ships sub-skills, agents, commands, and hooks — not just a single SKILL).

## Sub-skills bundled in the framework

- `using-superpowers` — entry-point that enforces "check for skills before any response"
- `brainstorming` — drives spec elicitation before implementation
- `writing-plans` — produces plans clear enough for a junior to follow
- `executing-plans` — disciplined plan execution loop
- `test-driven-development` — strict red/green TDD
- `systematic-debugging` — root-cause debugging instead of guess-and-check
- `verification-before-completion` — gates "done" behind real verification
- `subagent-driven-development` — long-running multi-agent execution
- `dispatching-parallel-agents` — parallel agent fan-out
- `using-git-worktrees` — isolated branches via worktrees
- `requesting-code-review` / `receiving-code-review` — structured review loop
- `finishing-a-development-branch` — clean shipping flow
- `writing-skills` — author new skills

## Install (recommended — full plugin)

Pick the one that matches your agent host. These install the whole framework, including auto-trigger hooks:

**Claude Code (official marketplace):**
```bash
/plugin install superpowers@claude-plugins-official
```

**Claude Code (Superpowers marketplace):**
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

**OpenAI Codex CLI:** run `/plugins`, search `superpowers`, install.

**Cursor:** in Agent chat, `/add-plugin superpowers`.

**OpenCode:** tell OpenCode `Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md`.

**GitHub Copilot CLI:**
```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

**Gemini CLI:**
```bash
gemini extensions install https://github.com/obra/superpowers
```

## Manual fallback (Claude Code, no plugin marketplace)

If you cannot use a plugin marketplace and only want the skill files dropped into `~/.claude/skills/`:

```bash
git clone --depth 1 https://github.com/obra/superpowers /tmp/superpowers
mkdir -p ~/.claude/skills
cp -R /tmp/superpowers/skills/* ~/.claude/skills/
```

This installs the SKILL.md files but **not** the agents, commands, or hooks. Auto-triggering depends on those — without them, you'll have to invoke skills manually via the `Skill` tool. Restart your agent after copying.

## How to use it once installed

1. Start a normal task ("build X", "fix Y", "refactor Z")
2. The `using-superpowers` skill fires first and forces a skill check before any other response
3. Process skills (brainstorming, debugging) run before implementation skills
4. The agent announces which skill it's using and follows it exactly

If the agent skips the check, that's a violation of the framework — re-prompt it to invoke `using-superpowers` first.

## License & credit

Superpowers is authored by Jesse Vincent (`obra`). License and full docs live at https://github.com/obra/superpowers. This SKILL.md is a vendored pointer published via AnalystTom/skillupgrade so it can be discovered from clawbench.com/skills. For the canonical, up-to-date framework, always go to upstream.
