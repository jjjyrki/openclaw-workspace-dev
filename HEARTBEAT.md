# HEARTBEAT.md -- Dev Autonomous Work Loop

Run this workflow on every heartbeat.

## Goal

Pick up implementation tickets assigned to dev, complete the implementation workflow, and move ready work to `in review`.

## 1. Refresh local instructions

1. Pull the latest agent repo state first.
2. Read the latest local `HEARTBEAT.md`, `SOUL.md`, `TOOLS.md`, and `AGENTS.md` before continuing.

## 2. Establish Paperclip context

- Call `GET /api/agents/me` first.
- If a claimed Paperclip token is needed, load it from `paperclip-claimed-api-key.json` in the workspace.
- In containerized runs, use `host.docker.internal` for host-local services instead of `localhost`.

## 3. Find work

Prioritize in this order:
1. the wake-targeted ticket if `PAPERCLIP_TASK_ID` is set
2. tickets assigned to dev that are ready for implementation
3. tickets assigned to dev in `todo` or `in progress` that are implementation work

Do not pick up tickets that are already in `in review` or `done`.
Do not self-assign unrelated work.

## 4. Execute

For the selected ticket:
1. checkout/claim it if required by the workflow
2. read the issue and comments
3. execute the latest actionable non-agent instruction
4. implement via Cursor only
5. run relevant verification
6. open or update the PR
7. leave the required completion comment with evidence
8. set ticket status to `in review` when implementation and handoff are ready

## 5. Blockers

If blocked:
- keep the ticket in `in progress`
- post a concrete blocker comment with evidence
- report blocker, risk, and next need

## 6. Exit

If no eligible implementation ticket exists, exit cleanly.
If work was completed, summarize what moved to `in review`.
