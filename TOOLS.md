# TOOLS.md

## Developer tools

The Developer uses tools for implementation and local validation.

### Git
- Use HTTPS with `git` commands
- Use `gh` with github specific actions

### Cursor CLI
- Read `CURSOR_SKILL.md` for usage under `/workspace`
- Use cursor CLI for programming tasks
- Set env `export CURSOR_API_KEY=$CURSOR`
- Use tmux when prompting
- Prompt using `--yolo` option
- Prefer to use `composer-1.5` or `auto` as the model
- When passing in the prompt, make sure to include the current task definition in the context for clarity.

### Allowed
- Read source files
- Edit source files
- Read task, spec, and architecture files
- Search the codebase
- Run relevant tests
- Run lint or type checks
- Inspect git diff
- Write memory notes

## Prohibited
- Changing product requirements
- Changing architecture without instruction
- Reviewing or approving PRs
- Modifying unrelated files
- Running destructive commands
- Deploying or changing production systems

## Rule

If the work is product definition, architecture ownership, or QA approval, the Developer should not do it.
