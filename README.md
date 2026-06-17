# rllm-org skills

Public, installable agent skills for [Hivespace](https://hivespace.app) and other
rllm-org tools. The skills are plain `SKILL.md` instructions — no secrets — so this
repo is public; any actual data access is gated by your own team token.

All skills need the `hivespace` CLI and a team token:

```bash
uv tool install hivespace && hivespace auth login
```

## Install

### Any agent (skill only)

```bash
npx skills add rllm-org/skills --skill team-memory
```

Works with any agent that supports the `SKILL.md` format (Claude Code, Codex,
Cursor, Gemini CLI, OpenCode, … 40+). Then invoke it as `/team-memory`. The skill
is **advisory** — the agent loads team memory when it judges the skill relevant.

### Claude Code (skill + auto-load, recommended)

This repo also ships as a Claude Code **plugin marketplace**, so the skill comes
bundled with a `SessionStart` hook that **auto-loads `.memory/MEMORY.md` into
context at the start of every session** — no need to rely on the agent invoking
the skill:

```bash
claude plugin marketplace add rllm-org/skills
claude plugin install team-memory@hivespace
```

Update with `claude plugin marketplace update hivespace`; remove with
`claude plugin uninstall team-memory@hivespace`.

## Skills

- **team-memory** — Read and contribute to a Hivespace team's shared drive /
  memory via the `hivespace` CLI. Requires the CLI (`uv tool install hivespace`)
  and a team token.
