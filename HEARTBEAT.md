# HEARTBEAT.md

Run this workflow on every heartbeat.

## Goal

Check development tickets in `todo`, implement the ready ones through Cursor, and hand finished work to QA.

## Workflow

1. Check any dev tickets in `todo`.
2. Find the ones that are ready for implementation.
3. Pull latest from git.
4. Analyze the ticket and code.
5. Implement using Cursor.
6. Push the branch and open or update the PR.
7. Assign the ticket to `QA` with status `in review`.
8. Add a clear implementation summary describing what was done and how it aligns with the ticket specs.

## Rules

- Favor the current dev workflow in `AGENTS.md` if older notes or memory conflict with it.
- Keep cloned repositories and project files under `workspace/projects`.
- Use Cursor as the only code-writing path for repository code changes.
- If instructions are unclear or blocked by missing information, assign the ticket back to CTO and set status to `todo`.
- Do not move work to QA until implementation is actually ready for review.
- If no eligible dev ticket is ready, exit cleanly.
