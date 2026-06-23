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
npx skills add rllm-org/skills --skill hivespace
```

Works with any agent that supports the `SKILL.md` format (Claude Code, Codex,
Cursor, Gemini CLI, OpenCode, … 40+). Then invoke it as `/hivespace`. The skill
is **advisory** — the agent loads the team's context when it judges the skill relevant.

### Claude Code (skill + auto-load, recommended)

This repo also ships as a Claude Code **plugin marketplace**, so the skill comes
bundled with a `SessionStart` hook that **auto-loads `TEAM.md` (the drive map),
`memory/MEMORY.md`, and `memory/coding/CODING.md` into context at the start of
every session** — no need to rely on the agent invoking the skill:

```bash
claude plugin marketplace add rllm-org/skills
claude plugin install hivespace@hivespace
```

Update with `claude plugin marketplace update hivespace`; remove with
`claude plugin uninstall hivespace@hivespace`.

### Seed team memory from your sessions (optional)

A second, user-invoked plugin that mines your local coding sessions into the team's
shared memory — you confirm each memory before it's written. Good for bootstrapping a
new team or onboarding a new member:

```bash
claude plugin install hivespace-seed-memory@hivespace
```

### Seed your coding sessions (optional, one-time)

To bootstrap the team's shared coding memory, upload your recent local
coding-agent sessions to the drive — cloud agents distill them into
`memory/coding/` for every future agent. Use `hivespace fs upload-sessions`
(part of the `hivespace` CLI) for an interactive picker (choose which repos to
upload):

```bash
hivespace fs upload-sessions
```

Or just ask your agent: *"upload my recent coding sessions to the team."*
Needs a read/write team token.

## Skills

- **hivespace** — Work with your Hivespace team from a local coding session via the
  `hivespace` CLI: orient in team memory, read the drive and connected integrations,
  and contribute learnings back. Requires the CLI (`uv tool install hivespace`) and a
  team token.
