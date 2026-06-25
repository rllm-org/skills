---
name: hivespace-seed-memory
version: "0.1.2"
description: Seed a Hivespace team's shared memory from your local coding sessions — mine past sessions for durable knowledge the codebase can't teach (conventions, decisions, traps), confirm each with the user, and write it to the team drive. Resolves the cold-start problem for a new team or teammate. Triggers on "/onboard", "seed team memory", "bootstrap team memory", "cold start", "learn from my sessions".
---

# Onboard — seed team memory from your sessions

First read `memory/MEMORY.md`, `memory/coding/CODING.md`, and the relevant topic
files, so you extend what's there instead of duplicating it.

## The one test

Memory competes with two things that are already better: **the codebase** (the truth
of *what is*) and **the agent's own reasoning** (*what to do now*). It earns its place
only by holding what neither can. Judge every candidate by one question:

> **Would a competent agent, reading the current code fresh, still get this wrong or
> waste real time without this note?**

If a fresh code-read would teach it just as well, **don't write it** — a note that
re-describes current architecture isn't knowledge, it's a liability that rots on the
next refactor and gets trusted over the code. Bias to **why over what**.

**Keep:** conventions/rules (the "always/never," *with the why*) · decisions +
rationale · traps & heuristics ("we tried X, do Y" — invisible in code, the
highest-value notes) · pointers, not copies · people / process / ownership.

**Reject:** architecture, schemas, file-by-file behavior, PR-by-PR changelogs,
anything a `grep` answers. A PR number or file path is *supporting evidence* for a
rule or trap, never the substance of a note.

**Form:** durable guidance + a pointer, phrased so that if it goes stale it **fails
loud** (name the pointer and the why). One idea per note; link with `[[name]]`.

## Steps

1. **Pick sessions.** Ask which working directories to include — not personal or
   unrelated repos. **Favor recent sessions** — they reflect how the team works
   *now*. Lead with the most recent and weight them heavily; older sessions can fill
   gaps, but when they conflict, the recent one wins (a convention may have changed —
   that's a stale-note flag, not a keeper).
2. **Mine for keepers.** Run candidates through the test above; most will fail, and
   that's expected. While reading, also flag existing notes that have gone stale.
3. **Confirm one at a time.** Present each as a concrete, specific claim about
   *their* team ("Your team always promotes staging→main with a merge commit, because
   squashing re-diverges the branches"), not a vague category. A precise claim said
   back is the wow moment — the "oh, Claude actually gets our team" reaction — and
   specificity is what earns it. Present each in the same considered, mannered way —
   the claim, a one-line why-it's-durable with its pointer, and whether it extends an
   existing file or starts a new one — then wait for a yes / edit / skip. Write
   nothing until approved.
4. **Prune as you go.** For each stale note from step 2, confirm with the user, then
   delete it (or fold the durable part into a current note). Seeding that only adds
   rots; leaving the team smarter means cutting too.
5. **Write the approved notes** — team-wide in `memory/`, coding in `memory/coding/`.
   Extend the right file, keep to the form above, and update the `MEMORY.md` /
   `CODING.md` index line.
