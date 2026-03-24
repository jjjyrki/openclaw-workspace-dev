
---
name: cursor
descriptino: Detailed reference for Cursor CLI (`agent`).
---

## Setup
```bash
export CURSOR_API_KEY=$CURSOR
```

## Flags
- `--yolo` — auto-apply all changes (required for automation)
- `--model <name>` — prefer `composer-1.5` or `auto`
- `--trust` — trust workspace on first run
- `-p` / `--print` — non-interactive output (no TTY, but also no file writes — avoid for implementation)

## tmux pattern (required for automation)
```bash
tmux kill-session -t cursor 2>/dev/null || true
tmux new-session -d -s cursor
tmux send-keys -t cursor "cd /path/to/repo" Enter
sleep 1
tmux send-keys -t cursor "export CURSOR_API_KEY=$CURSOR" Enter
tmux send-keys -t cursor "agent --yolo --model composer-1.5 'PROMPT'" Enter
sleep 3
tmux send-keys -t cursor "a" Enter   # trust workspace if prompted (first run)
sleep 60                              # adjust for task complexity
tmux capture-pane -t cursor -p -S -200
```

## Context selection

Include files in the prompt with `@`. More context = better output. Always include the task file minimum.

```
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

OpenClaw mirrors skills into your sandbox at `/workspace/skills/`.

Read a skill first with the `read` tool, then pass it to Cursor:

```
@/workspace/skills/<skill-name>/SKILL.md
@/workspace/claw-plans/<project>/features/<feature>/tasks/Txxx.md
@/workspace/<repo>/src/relevant-file.ts

<your instruction here>
```

## Rules loaded automatically
Cursor reads `.cursor/rules`, `AGENTS.md`, and `CLAUDE.md` from the working directory.
