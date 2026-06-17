# rllm-org skills

Public, installable agent skills for [Hivespace](https://hivespace.app) and other
rllm-org tools. The skills are plain `SKILL.md` instructions — no secrets — so this
repo is public; any actual data access is gated by your own team token.

## Install

```bash
npx skills add rllm-org/skills --skill team-memory
```

Works with any agent that supports the `SKILL.md` format (Claude Code, Codex,
Cursor, Gemini CLI, OpenCode, … 40+). Then invoke it as `/team-memory`.

## Skills

- **team-memory** — Read and contribute to a Hivespace team's shared drive /
  memory via the `hivespace` CLI. Requires the CLI (`uv tool install hivespace`)
  and a team token.
