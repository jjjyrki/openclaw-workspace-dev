# TOOLS.md
### Connectivity ### 
- Use `host.docker.internal` to replace `localhost` or `172.17.0.1`

## Git
- Use HTTPS for all git operations (`https://github.com/...`)
- Use `gh` for GitHub-specific actions (PRs, etc.)

## Cursor CLI

See `/workspace/skills/cursor/SKILL.md` for full Cursor usage, context rules, and skill integration.

## Allowed
- Read source files directly
- Implement repository code changes via Cursor only
- Run tests, lint, type checks
- Inspect git diff
- Write memory notes

## Prohibited
- Implementing repository code changes via direct OpenClaw file-writing tools (`write`, `edit`, `apply_patch`)
- Changing product requirements or architecture
- Reviewing or approving PRs
- Deploying or modifying production systems
- Destructive commands
