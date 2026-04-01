---
name: cursor
description: Detailed reference for Cursor CLI (`agent`). Use when delegating implementation work to Cursor, preparing Cursor automation, or passing task files, skills, and source/test context into Cursor prompts.
---

# Cursor

## Setup

```bash
export CURSOR_API_KEY=$CURSOR
```

## CLI command
```bash
agent
```

## Flags

- `--yolo` — auto-apply all changes; required for automation.
- `--model <name>` — prefer `composer-2` or `auto`.
- `--trust` — trust workspace on first run.
- `-p` / `--print` — non-interactive output (no TTY, but also no file writes); avoid for implementation tasks.

## tmux pattern

Use this automation pattern when running Cursor non-interactively:

```bash
tmux kill-session -t cursor 2>/dev/null || true
tmux new-session -d -s cursor
tmux send-keys -t cursor "cd /path/to/repo" Enter
sleep 1
tmux send-keys -t cursor "export CURSOR_API_KEY=$CURSOR" Enter
tmux send-keys -t cursor "agent --yolo --model composer-2 'PROMPT'" Enter
sleep 3
tmux send-keys -t cursor "a" Enter # trust workspace if prompted on first run
sleep 60 # adjust for task complexity
tmux capture-pane -t cursor -p -S -200
```

## Context selection

Include files in the prompt with `@`.
More relevant context usually produces better output.
Always include the task file at minimum.

```text
@/workspace/claw-plans/<project>/features/<feature>/tasks/Txxx.md
@/workspace/<repo>/src/relevant-file.ts
@/workspace/<repo>/tests/relevant-test.ts
```

| Situation | Include in context |
|---|---|
| Implementing a task | Task file + relevant source files + test files |
| Adding tests | Task file + source file under test + existing test files |
| Fixing a bug | Task file + failing test + buggy source file |
| Following a skill | SKILL.md + task file + relevant source |

## Skills

OpenClaw mirrors skills into the sandbox at `/workspace/skills/`.

Read a skill first with the `read` tool, then pass it to Cursor:

```text
@/workspace/skills/<skill-name>/SKILL.md
@/workspace/claw-plans/<project>/features/<feature>/tasks/Txxx.md
@/workspace/<repo>/src/relevant-file.ts

<your instruction here>
```

## Automatically loaded rules

Cursor reads these automatically from the working directory when present:

- `.cursor/rules`
- `AGENTS.md`
- `CLAUDE.md`
