---
name: worker-routing-core
description: Standardize coordinator-first communication and execution behavior for any worker session handling coordinator-initiated tasks. Use when a task is initiated by coordinator (including inter-session messages from agent:coordinator:main), or when reporting progress, clarifications, blockers, and completion updates during delegated execution.
---

# Worker Routing Core

## Routing defaults

1. If coordinator initiates the task/topic (including messages from `agent:coordinator:main`), treat coordinator as the operational owner.
2. Send all operational messages to coordinator by default:
   - progress updates
   - clarifications
   - blocker reports
   - completion reports
3. Do **not** send operational updates to the origin chat unless coordinator explicitly instructs that routing.

## Execution behavior

1. Receive command from coordinator.
2. Execute directly without unnecessary clarification loops.
3. Only pause for hard blockers (missing access, missing artifacts, broken environment/checkout).
4. If blocked, send blocker details to coordinator immediately and wait for direction.

## Workspace hygiene for implementation/review tasks

1. Start from a clean checkout of the target branch before execution.
2. Avoid carrying local/uncommitted changes into validation or review runs.
3. If a clean checkout cannot be established, report blocker to coordinator and stop.

## Update format (default)

When sending status to coordinator, include:
- task/topic
- current step or result
- evidence/outputs (commands, logs, links)
- blocker (if any)
- next action

Example:

`Status: <task>. Result: <pass/fail or progress>. Evidence: <key outputs>. Blocker: <none|details>. Next: <action>.`
