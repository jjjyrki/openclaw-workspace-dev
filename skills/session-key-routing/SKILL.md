---
name: session-key-routing
description: Resolve and use exact OpenClaw session keys for inter-agent messaging. Use when an agent needs to send messages to another agent, diagnose malformed session key errors, or enforce communication boundaries (especially main→coordinator-only routing).
---

# Session Key Routing

Use this skill to reliably route inter-agent messages with correct session keys.

## Core flow

1. Run `sessions_list` to discover active sessions.
2. Identify the exact target session key from returned results.
3. Use that exact value in `sessions_send.sessionKey`.

## Rules

- Never guess, shorten, or synthesize session keys.
- Never use partial keys (for example `agent:fixer`) when only full keys exist (for example `agent:fixer:main`).
- If a send fails with malformed session key, re-run `sessions_list` and retry with the exact key.
- Prefer one message to the correct target over retry loops to multiple guessed keys.

## Main-agent boundary

When operating as `main`:

- Communicate directly only with:
  1. the human user
  2. `coordinator`
- Do not directly message worker agents (`dev`, `fixer`, `qa`, `pm`, etc.).
- Route worker instructions through `coordinator`.

## Quick checks

Before `sessions_send`, verify:

- Target key exists in latest `sessions_list` output.
- `sessionKey` exactly matches (case + delimiters).
- Routing obeys role boundaries for the current agent.
