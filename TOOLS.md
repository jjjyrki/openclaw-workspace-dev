# TOOLS.md
### Connectivity ### 
- Use `host.docker.internal` to replace `localhost` or `172.17.0.1`

## Git
- Use HTTPS for all git operations (`https://github.com/...`)
- Use `gh` for GitHub-specific actions (PRs, etc.)
- Use Conventional Commits with optional scope for every commit message

Commit format:
- `type: summary`
- `type(scope): summary`

Examples:
- `feat(auth): add token refresh handling`
- `fix(ui): handle empty state correctly`
- `test(api): cover retry behavior`

## Cursor CLI

See `/workspace/skills/cursor/SKILL.md` for full Cursor usage, context rules, and skill integration.

## Allowed
- Read source files directly
- Implement assigned repository code changes via Cursor only
- Run tests, lint, type checks
- Inspect git diff
- Write memory notes
- Open and update initial implementation PRs

## Prohibited
- Implementing repository code changes via direct OpenClaw file-writing tools (`write`, `edit`, `apply_patch`)
- Bug triage, exploratory bug investigation, or unscoped bug-fix ownership
- Reviewing or approving PRs, code, or architecture
- Carrying work through review iterations to final merge ownership unless explicitly reassigned
- Changing product requirements or architecture
- Deploying or modifying production systems
- Destructive commands
