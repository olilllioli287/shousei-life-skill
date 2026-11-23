# Multi-Agent Workflow

[繁體中文](README.md)

A set of working rules for people who use several AI agents (Claude Code, Codex, Gemini, and others) on the same job. Humans can read it; agents can follow it as a skill. It comes from long real-world use and is written to be tool-independent.

## What is inside

| Path | Content | Maturity |
| --- | --- | --- |
| `skills-en/multi-agent-project/` | Coding and bug fixing: three-layer roles, agree on "done" first, a spec gate before work starts, dispatch scope, handoff and acceptance, cross-vendor review, test safety. Includes `references/` (details) and `templates/` (spec, review rules, review prompt) | Used in practice |
| `skills-en/multi-agent-learning/` | Studying with several AIs: build a study guide, ask while reading, two-stage review | **Experimental**, little real-world validation |
| `skills/` | The same skills in Traditional Chinese | Same |

## The core ideas (one minute)

- Agree on what "done" means before touching code. For medium-or-larger work, write a one-page spec and have it cold-reviewed before starting (the hard gate).
- Give each job to exactly one agent, name the files it may touch and a time limit, and judge progress by output (commits, edited files), not by status text.
- After two failed fixes in the same direction, write down the premise both fixes share and test it before a third attempt.
- The author and the reviewer come from different vendors; reviewers get materials and a diff, never the author's conclusion.
- When the same kind of mistake happens twice, promote it to an enforced check instead of adding one more line of prose.

## Install

**Claude Code**: copy the skill folders you want into the project's `.claude/skills/` (or `~/.claude/skills/`). Use the folders under `skills-en/` for English, or `skills/` for Traditional Chinese. Do not install both languages of the same skill.

**Codex and other agents**: put `AGENTS.md` in the project root; it points to the rules. You can also paste `SKILL.md` and `references/` into the agent as instructions.

## Works without special tooling

"Task board", "heavy-job lock" and "watcher on the lower layer" are role names. Each comes with a one-line fallback (a `TASKS.md` file, run one heavy job at a time, check progress on a timer).

## License

MIT. See `LICENSE`.
