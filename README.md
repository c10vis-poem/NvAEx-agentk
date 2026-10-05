# NvÆx agentk

An open **agent toolkit**: skills, hooks and protocols that keep coding agents honest, trackable and on-task. Built for Claude Code; the skills are plain Markdown, so other harnesses can read them too.

## Install (Claude Code)

```
/plugin marketplace add c10vis-poem/NvAEx-agentk
/plugin install nvaex@nvaex
```

Skills answer to their bare names, for example `/subagent-protocol`. If another skill already uses that name, the long form `/nvaex:subagent-protocol` always works.

## Skills

| Skill | What it does |
|---|---|
| [subagent-protocol](skills/subagent-protocol/SKILL.md) | When to spawn subagents, which model size to pick, how to brief them, and live progress files so you can always see where each one is, without waiting for it to finish. |

## Coming next

- The H1–H7 enforcement hooks (session-start gates, Stop gates, secret guard, ship-on-wrap-up) as an installable set.

## License

MIT
