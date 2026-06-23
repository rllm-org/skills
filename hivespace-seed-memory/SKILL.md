---
name: hivespace-seed-memory
version: "0.1.0"
description: Seed a Hivespace team's shared memory from your local coding sessions — read past sessions, note knowledge other agents will benefit from, confirm each memory with the user, and write it to the team drive. Resolves the cold-start problem for a new team or teammate. Triggers on "/onboard", "seed team memory", "bootstrap team memory", "cold start", "learn from my sessions".
---

# Onboard — seed team memory from your sessions

First read `memory/MEMORY.md`, `memory/coding/CODING.md`, and relevant files so you don't duplicate what's
already there.

1. **Go through the user's local sessions** — ask which sessions (by working directory)
   to include. Don't assume all; personal or unrelated repos shouldn't seed team memory.

2. **Note knowledge another user's agent would benefit from** — durable decisions,
   conventions, gotchas, and how-things-work. Skip the ephemeral and anything already in
   memory.

3. **Confirm each memory with the user** — propose them to the user in high level; write nothing until
   approved.

4. **Write the approved memories** — general knowledge in `memory/`, dev-related in
   `memory/coding/`, and update the matching `MEMORY.md` / `CODING.md` index line.
