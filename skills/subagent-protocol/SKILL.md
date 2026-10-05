---
name: subagent-protocol
description: How to run subagents well — when to spawn them (unasked, for independent parts), which model size to pick, how to brief them, and live progress files so their state is always visible. Use before ANY Agent/subagent dispatch, and whenever the user asks where a subagent is, whether it is stuck, or to stop one.
---

# Subagent protocol

Four rules. The point: parallel work without idling, and no subagent is ever a black box.

## 1. When to spawn

- The request has **independent parts** → spawn one subagent per part, in parallel, without being asked. Do the remaining parts yourself meanwhile. Never sit idle waiting.
- One small, sequential task → do it inline. A spawn costs a cold start.
- If you finish first, pick up the subagent's pending steps (read its progress file to see which).

## 2. Size the model to the task

| Task | Model class |
|---|---|
| Mundane: lookup, grep, listing, copying, formatting | smallest (e.g. Haiku) |
| Normal: research, audits, routine code, summaries | mid (e.g. Sonnet) |
| Hard: architecture, debugging, ambiguous reasoning, review of risky changes | largest (e.g. Opus) |

Pass the model explicitly on the dispatch; don't let everything inherit the biggest model.

## 3. Brief every subagent with the template

A subagent starts cold. Fill in [templates/brief.md](templates/brief.md) for every dispatch. It always includes the progress-file instruction below; a brief without it is incomplete.

## 4. Live progress file (mandatory)

Every subagent writes `<progress-dir>/<YYYY-MM-DD>/agent-<topic>.md` **before doing any work**, then updates it after **every** step. Format: [templates/progress.md](templates/progress.md):

- `## Plan`: every step as a checkbox, `[x]` done, `[ ]` pending
- `## Last action`: timestamp + what it just did / is doing now
- `## Findings`: results so far, so nothing is lost if it is stopped
- `## Blocked`: anything it's stuck on

`<progress-dir>` defaults to `.agent-progress/` in the project root (add it to `.gitignore`). Override it in your project instructions (CLAUDE.md / AGENTS.md) or with the `AGENT_PROGRESS_DIR` environment variable.

## When the user asks "where is it?" / "is it stuck?"

1. Read its progress file. Answer from it immediately: done steps, current step, pending steps, findings. Never wait for the subagent to finish first.
2. `Last action` older than ~5 minutes on a step that should be quick, or no file at all → say it is likely hung, and offer to stop it.
3. If stopped: its findings survive in the file. Resume the pending steps yourself or with a fresh subagent pointed at the same file.

## Picking the mechanism

Most harnesses offer some of: a background subagent (fire, collect one report), a persistent agent you can keep messaging, and an isolated working copy (worktree or remote). Use background for bounded batches, persistent for work you'll correct iteratively, isolated when its file writes must not collide with yours. The protocol above applies to all of them.
