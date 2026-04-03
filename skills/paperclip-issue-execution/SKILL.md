---
name: paperclip-issue-execution
description: Enforce real task execution for Paperclip-assigned issues. Use when an agent is woken by Paperclip or assigned work through a Paperclip issue, especially when the issue/comment must be read, the actionable instruction extracted, the real task executed, and evidence produced before setting the correct workflow status.
---

# Paperclip Issue Execution

Use this skill when work arrives through a Paperclip issue.

## Core rule

Reading the issue and its comments is not completion.

The agent must:
1. identify the actual requested task
2. execute that task in the real environment
3. provide evidence of what was done
4. only then set the correct workflow status for its role

## Default status routing

Unless the task explicitly says otherwise:

- `dev` -> set ready implementation work to `in review`
- `qa` -> set reviewed work to `done` on approval, or back to `in progress` when changes are required
- `fixer` -> set repaired work back to `in review`
- other agents -> do not guess; follow the explicit workflow for that agent or task

## Stale wake-template override

If a generic wake/run template tells the agent to patch the issue with `status: done`, treat that as a stale default, not as authoritative workflow routing.

Do **not** apply generic `status: done` blindly.
Instead, use the role-specific workflow status above unless the task contains a truly explicit, role-aware exception.
Only QA may set `done`, and only when approving reviewed work.

## Required flow

1. Check out or claim the issue if the workflow requires it.
2. If a claimed Paperclip token is needed, look for it inside the workspace at `paperclip-claimed-api-key.json`.
3. Read the issue body.
4. Read all relevant comments.
5. Extract the concrete instruction or requested outcome.
6. Perform the real work.
7. Gather evidence:
   - changed files
   - command output
   - test results
   - links, identifiers, or artifacts
8. Determine the correct next status for the current agent role and workflow.
9. Post a completion comment that states:
   - what was requested
   - what was done
   - evidence
   - any limitations or follow-up
   - what status the issue is being moved to and why
10. Set the issue to the correct workflow status only after the work and evidence are complete.

## Hard gates

Do not set a terminal or handoff status if any of these are still missing:

- the actionable request is still unclear
- the requested work was not actually executed
- evidence is missing
- only issue metadata/comments were fetched
- the completion note cannot explain what changed
- the chosen status does not match the current agent role and workflow

## Examples of non-completion

These do **not** count as completing the task by themselves:

- calling `GET /issue`
- calling `GET /comments`
- summarizing the request without doing it
- posting a placeholder comment
- setting `done` or any other workflow status after a checkout/read-only workflow
- setting `done` when the current role should hand off to `in review` or `in progress`

## Completion standard

A Paperclip task is complete only when a reviewer can verify from the issue record what was asked, what was executed, what evidence proves it, and why the chosen next status matches the current workflow.
