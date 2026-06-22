---
name: team-memory
version: "0.5"
description: Read and write a Hivespace team's shared drive through the hivespace CLI (`hivespace fs …`) — the team's shared context as a filesystem, organized into memory/, sources/, capabilities/, and deliverables/. Load `memory/` (shared team memory) at session start, keep coding-agent knowledge in `memory/coding/`, upload your sessions to `sources/`, and contribute durable knowledge back for the team's other agents. Triggers on "/team-memory", "load team memory", "team context", "team files", "team drive", "remember this for the team", "hivespace fs", "upload my sessions", "seed coding sessions", "contribute my coding sessions".
---

# Team Memory

A Hivespace team's shared context is a **filesystem** — a team-wide drive reached
through `hivespace fs …` (which talks to the Hivespace API as the logged-in user).
`TEAM.md` at the drive root is the map. It's organized into four areas:

- **`memory/` — the team's brain.** Distilled, durable knowledge *every* agent
  loads each session: purpose, people and ownership, conventions, recurring
  gotchas, current focus. Small and curated; `memory/MEMORY.md` is its index (a
  Summary plus one line per file). Hivespace's own cloud agents and humans read it
  too, so keep the top level cross-cutting. **`memory/coding/`** is the same,
  scoped to knowledge mainly useful to *other coding agents* (build/test quirks,
  repo gotchas, local tooling, codegen conventions); `memory/coding/CODING.md` is
  its index. Both are seeded for every team — read and extend them.
- **`sources/` — raw captured material.** Uploaded coding-agent sessions
  (`sources/sessions/`), meeting notes, chat attachments. Read for context; the
  team's cloud agents distill it into `memory/`.
- **`capabilities/` — skills the team has built.** Loadable agent skills.
- **`deliverables/` — what agents produce for humans.** Artifacts
  (`deliverables/artifacts/`), reports, results.

This skill is about **`memory/`**: lead with it and leave it better than you found
it — load `memory/MEMORY.md` first, and write back what you learn (step 3) so the
next agent inherits it.

## 1. Preflight

```bash
hivespace auth whoami
```

- **Handle + team** → you're in; note whether the scope is **read/write** (writing
  needs it). Continue.
- **`command not found`** → install with `uv tool install hivespace` (or pipx /
  pip) and stop.
- **Not authenticated / 401** → ask the user to run `hivespace auth login` (for a
  non-default server, `export HIVESPACE_SERVER=<url>` first). **Don't run login
  yourself** — the user pastes their own token.

On more than one team, `fs` may report it's ambiguous — pass `--team <slug>` (from
`whoami`) on every command.

## 2. Load — memory first

```bash
hivespace fs cat memory/MEMORY.md   # index: Summary + one line per file
hivespace fs ls  memory             # list memory files
hivespace fs cat memory/<file>      # read what's relevant (incl. memory/coding/)
```

Read `MEMORY.md` and the topic files whose index lines fit the task (all of them
if it's broad). Then act on it: follow its conventions, refer to the people and
projects it names, respect the current focus. If a request contradicts the memory,
surface that rather than silently overriding it; don't echo memory back unless
asked. For anything bigger — a spec, a transcript, an artifact — follow memory's
pointers or browse the drive.

## 3. Contribute back — this is the point

Loading memory is the easy half; it only compounds if agents **write to it** —
what one learns, every future agent inherits. So a session of real work should
leave memory better than it found it: **watch for anything durable** and
**proactively offer to persist it** — don't wait to be asked.

When you write: a **read-only PAT can't** — say so and stop; otherwise **extend
the right file rather than duplicate it**, keep it high-level, and update its line
in the `MEMORY.md` / `CODING.md` index.

## 4. Seed coding sessions

Local coding-agent transcripts can be uploaded to `sources/sessions/` on the
drive, where the team's cloud agents distill them into `memory/coding/` so every
future agent inherits the learnings. Run `hivespace fs upload-sessions` (part of
the `hivespace` CLI). It needs a **write-scoped** token and is a graceful no-op
otherwise.

When the user asks to **upload / contribute / seed** their coding sessions:

1. **List what's available** — distinct working directories (repos) and how many
   sessions each has:
   ```bash
   hivespace fs upload-sessions --list          # prints: <count>\t<working-dir>
   ```
2. **Ask the user which directories** to upload (show them the list).
3. **Upload their picks** — comma-separated; uploads all sessions for each:
   ```bash
   hivespace fs upload-sessions --cwd <dir1>,<dir2>
   ```

Target **completed** sessions — the current live session is incomplete. Uploads
are idempotent (re-running refreshes). A human can also run the command directly
in a terminal with no args for an interactive picker.

## Command reference

`hivespace fs` operates on the whole drive; the same auth and read/write rules
apply everywhere.

```bash
hivespace fs ls   <path>                       # browse a directory
hivespace fs cat  <path>                        # read a file
hivespace fs write <path>                       # create/overwrite (stdin or --from <local file>)
hivespace fs edit  <path> --old "…" --new "…"   # in-place find/replace
hivespace fs mkdir|mv|rm|cp …                   # the usual filesystem ops
```
