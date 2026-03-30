# TOOLS.md

## Connectivity

- Use `host.docker.internal` to replace `localhost` or `172.17.0.1`

## Sub-agents

- Spawn a sub-agent only for a clearly bounded task with explicit inputs, constraints, and deliverables.
- Prefer sub-agents for context-heavy, multi-step, or independent work; avoid them for trivial tasks.
- Require the sub-agent to return a concise summary and artifacts, not its full intermediate process.

## Workspace

- Store cloned repositories and project files under `$AGENT_HOME/projects/`

## Git

- Use HTTPS for all git operations (`https://github.com/...`)
- Use `gh` for GitHub-specific actions (PRs, etc.)
- Sync the working branch with the latest base branch state before handing work to QA
- Resolve merge conflicts before requesting QA review, or explicitly report the blocker if resolution is not possible
- Use Conventional Commits with optional scope for every commit message

Commit format:
- `type: summary`
- `type(scope): summary`

Examples:
- `feat(auth): add token refresh handling`
- `fix(ui): handle empty state correctly`
- `test(api): cover retry behavior`

## Cursor CLI

- Cursor is the only allowed code-writing path for repository development work.
- Do not implement repository code changes through direct OpenClaw file-writing tools.
- Under no circumstances can you implement code with any other method than Cursor CLI
- Read the code and ticket context before prompting Cursor.
- Review Cursor output carefully before accepting it.

See `/workspace/skills/cursor/SKILL.md` for full Cursor usage, context rules, and skill integration.

## Allowed

- Read source files directly
- Implement assigned repository code changes via Cursor only
- Run tests, lint, type checks
- Inspect git diff
- Write memory notes
- Open and update PRs

## Prohibited

- Implementing repository code changes via direct OpenClaw file-writing tools (`write`, `edit`, `apply_patch`)
- Bug triage, exploratory bug investigation, or unscoped bug-fix ownership
- Reviewing or approving PRs, code, or architecture
- Carrying work through review iterations to final merge ownership unless explicitly reassigned
- Changing product requirements or architecture
- Deploying or modifying production systems
- Destructive commands
