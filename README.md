# take-it-easy

A working style for AI coding agents: calm, transparent, no surprises. The agent explains each step in plain language,
says what comes next and whether it needs your confirmation, warns you before risky actions on shared or real
environments, and hands you exact commands when it cannot run something itself.

It is an [Agent Skill](https://agentskills.io) (a single `SKILL.md`), so it works in Claude Code, Cursor and any other
agent that loads skills.

## How it works

When active, the agent:

- **Explains each meaningful step in plain language** (a few lines, no jargon) and always says what the **next step**
  is and whether it **needs your confirmation**.
- **Keeps the big picture visible**: where the task stands overall and why the current step is being done.
- **Warns before risky actions** (migrations, deploys, data changes, publishes, pushes): whether similar changes were
  made before, what could fail, and how to revert, before asking for approval. An approval never carries over.
- **Hands you exact commands** when it lacks permission, with a plain explanation of what they do and why.
- **Never builds a feature without your explicit approval** of what it will build, deferring to a spec-driven skill
  such as [`sdd-feature`](https://github.com/illescasDaniel/sdd-feature-skill) when it is installed.
- **Commits or pushes only when you ask.**

Project rules (`CLAUDE.md` / `AGENTS.md`) always take precedence over the skill. The skill replies in the language you
write in; to force one, say so in your `CLAUDE.md` / `AGENTS.md` (for example "Always reply in English").

## Install

### Any agent: the `skills` CLI

```bash
npx skills add illescasDaniel/take-it-easy-skill
```

It asks which agents to install for and whether to install into the current project or globally. Non-interactive:
`npx skills add illescasDaniel/take-it-easy-skill -a claude-code -y`, plus `-g` for global.

### By hand

Copy the `skills/take-it-easy` folder into your agent's skills directory.

| | Project (shared with your team) | All your projects |
|---|---|---|
| Claude Code | `.claude/skills/take-it-easy/` | `~/.claude/skills/take-it-easy/` |
| Cursor | `.cursor/skills/take-it-easy/` | `~/.cursor/skills/take-it-easy/` |

Invoke it with `/take-it-easy`, or ask the agent to "take it easy" or work step by step.

Skills load only when the agent judges them relevant. To have this style apply from the first message of every
session, add a line to your `CLAUDE.md` / `AGENTS.md`: `Use the take-it-easy skill for all work in this project.`

## Recommended companions

This skill is only about *how the agent communicates and avoids surprises*. Other behavior lives elsewhere so each
piece has one source of truth.

**Skill [`sdd-feature`](https://github.com/illescasDaniel/sdd-feature-skill)**: for anything that adds or changes a feature, pair this with a Spec-Driven Development skill
that stops for your approval of the design, the spec and the architecture before writing tests and code, then has the
result graded independently. take-it-easy does not duplicate that workflow; it only makes each gate easier to follow.

**Lines for your `CLAUDE.md` / `AGENTS.md`**: two behaviors are project or environment settings rather than a working
style, so they are not in the skill. Paste them in if you want them:

```markdown
## Subagents
- You are the orchestrator: you talk to the user and own every approval gate and every memory write.
- Delegate big or parallelizable work (design, implementation chunks, grading, mechanical tasks) to subagents; do
  small, local changes and questions yourself. Delegation has a cost.
- Briefs must be self-contained: goal, context (file paths, decisions already made), scope, and "done when".
  Subagents have no conversation history. Say what to achieve, not how to search or edit.
- Verify what a subagent reports (read the diff) before telling the user it is done. Summarize; do not relay reports.

## Memory
- If the project keeps a `memory/` folder (or similar), read its current-state files at session start.
- Update it only when: a task is completed or discovered; focus changes, a blocker appears or work concludes; a
  significant technical decision is made (append-only, newest first).
- Before ending a turn with unfinished work, make sure the memory's record of progress is accurate and the exact next step is
  written down. If the project has no memory, do not create one unless asked.
```

## Contributing

Issues and pull requests are welcome. The skill is deliberately short: if a change makes `SKILL.md` longer, it should
earn it.

## License

MIT, see [LICENSE](LICENSE).
