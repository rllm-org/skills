---
name: hivespace
version: "0.6.1"
description: Work with your Hivespace team from a local coding session using the `hivespace` CLI — orient in the team's shared memory, read its drive and connected integrations (Slack, GitHub, Linear, Gmail, …), and contribute durable learnings back so the next agent inherits them. Triggers on "/hivespace", "hivespace cli", "team context", "team memory", "team files", "team drive", "hivespace fs", "hivespace connectors", "team integrations", "remember this for the team", "upload my sessions".
---

# Hivespace

Hivespace gives a team one shared workspace — a drive (its long-term memory),
connected integrations, a task board, and agents (Hivey + workflow runners) that
all work the same context. You're a **local coding agent** working for someone on a
team, and the `hivespace` CLI is your link into that shared context. Three things to
do with it every session: **orient** in what the team knows, **draw on** its drive
and integrations as you work, and **give back** what you learn so the team compounds.

## Setup

```bash
hivespace auth whoami     # handle, team, and scope (read vs read/write)
```

- `command not found` → `uv tool install hivespace` (or pipx / pip) and stop.
- 401 / not authenticated → ask the user to run `hivespace auth login` (for a
  non-default server they `export HIVESPACE_SERVER=<url>` first). **Don't run login
  yourself** — the user pastes their own token.
- Reaching more than one team → add `--team <slug>` to every command.

## Orient — what the team knows

The Claude Code plugin's SessionStart hook preloads the team's map and memory
indexes: `TEAM.md`, `memory/MEMORY.md`, and `memory/coding/CODING.md`. **If those are
already in your context, you're oriented.** Otherwise load them:

```bash
hivespace fs cat TEAM.md                  # the drive map — what lives where
hivespace fs cat memory/MEMORY.md         # the team's memory index
hivespace fs cat memory/coding/CODING.md  # the coding-agent slice (build/test, repo gotchas)
```

`MEMORY.md` and `CODING.md` are **indexes** — one line per file. **Before you start
coding work, open the `memory/coding/` files whose index lines fit the task** (repo
layout, build/test quirks, release flow, the trap you're about to hit) so you don't
relearn what the team already worked out:

```bash
hivespace fs cat memory/coding/<file>.md   # the topic files CODING.md points at
```

Then act on what you load: follow the team's conventions, respect its current focus,
use the people and projects it names. If a request contradicts memory, surface that
rather than silently overriding it; don't echo memory back unless asked.

## Draw on — the drive and the integrations

**The drive** (`hivespace fs`) is the team's shared context as a filesystem — memory
plus `sources/` (raw material), `capabilities/` (skills), and `deliverables/`
(outputs). Browse and read it for anything beyond the loaded memory:

```bash
hivespace fs ls  <path>
hivespace fs cat <path>
```

**The integrations** (`hivespace connectors`) are the team's connected apps — native
(Slack, GitHub, Granola) and Pipedream apps (Linear, Gmail, …). Read them live when
the drive doesn't have what you need (writes are blocked here — use the web UI or a
cloud agent for those):

```bash
hivespace connectors list [--connected]                   # what's connected
hivespace connectors tools <slug>                         # a connector's tools + arg shapes
hivespace connectors call <slug> <tool> --args '<json>'   # call a READ-only tool
```

Discover before you call — `connectors tools <slug>` shows the real tool names and
argument shapes; then pass `--args` as a JSON object.

## Give back — leave the team smarter

The point: memory only compounds if agents **write to it**. A read-only token can't
— say so and stop. Otherwise, when you learn something durable:

- **Memory** — persist it with `hivespace fs write` / `fs edit`. Team-wide facts go
  in `memory/`; build/test/repo knowledge for other coding agents goes in
  `memory/coding/`. Extend the right file (don't duplicate), keep it high-level, and
  update its line in the `MEMORY.md` / `CODING.md` index.
- **Sessions** — when the user asks to upload/seed their work, push local coding
  transcripts to the drive so cloud agents distill them into `memory/coding/`:

  ```bash
  hivespace fs upload-sessions --list              # <count>\t<working-dir>
  hivespace fs upload-sessions --cwd <dir1>,<dir2> # after the user picks which repos
  ```

  Target completed sessions (the live one is incomplete); uploads are idempotent.

## Commands

A local session works through two command groups (run either with `--help` for its
subcommands and flags):

| Group | What it's for |
|-------|---------------|
| `hivespace fs` | The shared team drive: `ls cat write edit mkdir mv rm cp`, plus `upload-sessions`. |
| `hivespace connectors` | Read-only access to the team's integrations: `list`, `tools`, `call`. |

(`hivespace auth whoami` checks your identity and scope — see Setup.) Other command
groups exist but are the cloud agents' surface, not local sessions.
