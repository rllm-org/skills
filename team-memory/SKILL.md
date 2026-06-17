---
name: team-memory
version: "0.3"
description: Access a Hivespace team's shared drive through the hivespace CLI — read and write any file on the team filesystem, with the team's shared memory (.memory/) as the primary thing to load and keep current. Use to load team memory/context at session start, browse or edit team files, or persist durable knowledge for the team's other agents. Triggers on "/team-memory", "load team memory", "team context", "team files", "team drive", "remember this for the team", "hivespace fs".
---

# Team Memory

Hivespace teams have a **shared drive** — a team-wide filesystem reachable through
the `hivespace` CLI (`hivespace fs …`), which talks to the Hivespace API as the
logged-in user. You can read and write any file on it.

The most important part of that drive is the team **memory** under `.memory/` — a
`MEMORY.md` index plus one markdown file per topic (people, conventions, current
focus, integrations, …), written and maintained by the team's agents and humans.
**Lead with memory:** load it at the start of a session so you act with the team's
accumulated knowledge, and keep it current as you learn. The rest of the drive
(docs, artifacts, scratch files) is available the same way.

## 1. Preflight — CLI present and authenticated

```bash
hivespace auth whoami
```

- **Prints a handle + team** → you're in. Note the team(s) and whether the scope
  is **read/write** (writing needs a read/write token). Go to step 2.
- **`command not found`** → install the CLI with `uv tool install hivespace` (or
  `pipx install hivespace` / `pip install hivespace`; see this team's "Connect
  local agents" instructions) and stop.
- **Not authenticated / 401** → tell the user to run `hivespace auth login` first
  (for a non-default server, `export HIVESPACE_SERVER=<url>` beforehand). **Do not
  run `hivespace auth login` yourself** — the user pastes their own token.

(If the user belongs to more than one team, `fs` commands may report the team is
ambiguous; pass `--team <slug>` from `whoami` on every command below.)

## 2. Load the team memory (do this first)

```bash
hivespace fs cat .memory/MEMORY.md       # the index: Summary + one line per file
hivespace fs ls .memory                  # list all memory files
hivespace fs cat .memory/<file>          # read the relevant topic files
```

`MEMORY.md` has a **Summary** (high-level overview) and a **Memory** index listing
one line per file in `.memory/`. Read the files whose descriptions match the task;
at session start, read all of them.

## 3. Use it

Treat the loaded memory as the team's shared context: follow the conventions it
documents, refer to the people and projects it names, respect the current focus.
If a request contradicts the memory, surface the discrepancy rather than silently
overriding it. Don't dump the full memory back to the user unless asked.

## 4. Contribute durable knowledge back

When you learn something that will help the team's **other agents** in future
sessions, persist it to `.memory/`. That is the point of a shared memory: what one
agent learns, every agent inherits.

**What belongs (durable + general):** the team's purpose, who's on it and what they
own, conventions and workflows, architectural decisions and *why*, recurring
gotchas, current goals.
**What does NOT belong:** day-to-day chatter, one-off task detail, transient state,
secrets/tokens, or anything specific to only this session.

**How to contribute:**

1. **Check first** — read the existing `.memory/` files; if one already covers the
   topic, **extend or refine it** rather than creating a duplicate.
2. **Confirm with the user** what you're about to persist and where (it's shared
   team state every agent reads). Keep the note concise and high-level.
3. **Write it** — extend a file
   (`hivespace fs edit .memory/<file> --old "<anchor>" --new "<anchor>\n<addition>"`)
   or create a topic file (`hivespace fs write .memory/<topic>.md`, content via
   stdin or `--from <local file>`).
4. **Keep the index in sync** — add/update the one-line entry under the `## Memory`
   section of `MEMORY.md` so other agents discover it.

Writing requires a **read/write** PAT (step 1 shows your scope). If your token is
read-only, tell the user and stop.

## 5. Beyond memory — the rest of the drive

`hivespace fs` works on the **whole team drive**, not just `.memory/`. Same auth and
the same read/write rules apply:

```bash
hivespace fs ls <path>           # browse a directory (root = the drive root)
hivespace fs cat <path>          # read any file
hivespace fs write <path>        # create/overwrite (content via stdin / --from)
hivespace fs edit <path> --old "…" --new "…"   # in-place find/replace
hivespace fs mkdir|mv|rm|cp …    # the usual filesystem ops
```

Use this to read team docs, inspect artifacts, or drop files the team should see —
but **memory is the emphasis**: load it first, keep it current.
