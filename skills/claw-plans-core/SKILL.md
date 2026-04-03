---
name: claw-plans-core
description: Shared claw-plans operating rules for all agents. Use when tasks involve feature/task status, planning files, PR/task state sync, or reading/updating work items in claw-plans. Enforces clone-if-missing, pull-before-read, and HTTPS-only Git usage.
---

# claw-plans-core

Use this skill as the default operating baseline whenever claw-plans is part of the task.

## Repository baseline

- Repository URL (HTTPS): `https://github.com/jjjyrki/claw-plans.git`
- Local path expectation: `~/claw-plans`
- In container mode with `HOME=/workspace`, this resolves to `/workspace/claw-plans`.

## Mandatory workflow

1. Ensure local checkout exists.
   - If missing, clone using HTTPS:
     - `git clone https://github.com/jjjyrki/claw-plans.git ~/claw-plans`
2. Enter repo and pull latest before reading task state.
3. Treat task files as source of truth for scope and status.

## Source-of-truth paths

- Feature tasks live under:
  - `claw-plans/<project>/features/<feature>/tasks/`
- When status changes are required, update the relevant task file first.

## Git transport policy (mandatory)

- Always use HTTPS remotes/URLs.
- Never use SSH Git URLs.
- If a remote is SSH, normalize to HTTPS before continuing.

## GitHub/PR hygiene

- Use `gh` for PR/issue checks when available.
- If `gh` is missing or unauthenticated, report blocked state with exact error.
- Keep claw-plans task status aligned with PR reality (open/in review/merged) as instructed by the task flow.

## Coordination expectations

- Report concise evidence only (IDs, links, key status lines).
- Surface drift early (task status mismatch vs PR/issue state).

## Quick commands (pure shell)

Canonical task statuses used by this skill:
- `Backlog`
- `In progress`
- `In review`
- `Change requested`
- `Done`

Set repo root first:

```bash
cd ~/claw-plans
```

### 1) Find next available task (Backlog)

Holistic (all projects/features):

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Backlog$' ./*/features/*/tasks/*.md | sort | head -n 1
```

Per project (`$PROJECT`):

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Backlog$' "./$PROJECT/features"/*/tasks/*.md | sort | head -n 1
```

Per feature (`$PROJECT`, `$FEATURE`):

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Backlog$' "./$PROJECT/features/$FEATURE/tasks"/*.md | sort | head -n 1
```

### 2) Find tasks requiring review (In review)

Holistic:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* In review$' ./*/features/*/tasks/*.md | sort
```

Per project:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* In review$' "./$PROJECT/features"/*/tasks/*.md | sort
```

Per feature:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* In review$' "./$PROJECT/features/$FEATURE/tasks"/*.md | sort
```

### 3) Find tasks requiring changes (Change requested)

Holistic:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Change requested$' ./*/features/*/tasks/*.md | sort
```

Per project:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Change requested$' "./$PROJECT/features"/*/tasks/*.md | sort
```

Per feature:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Change requested$' "./$PROJECT/features/$FEATURE/tasks"/*.md | sort
```

### 4) Count completed tasks (Done)

Holistic count:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Done$' ./*/features/*/tasks/*.md | wc -l
```

Per project count:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Done$' "./$PROJECT/features"/*/tasks/*.md | wc -l
```

Per feature count:

```bash
grep -RIl --include='*.md' '^- \*\*Status:\*\* Done$' "./$PROJECT/features/$FEATURE/tasks"/*.md | wc -l
```

### Optional: quick status distribution

Holistic:

```bash
for s in 'Backlog' 'In progress' 'In review' 'Change requested' 'Done'; do
  printf '%-16s %s\n' "$s" "$(grep -RIl --include='*.md' "^- \*\*Status:\*\* $s$" ./*/features/*/tasks/*.md | wc -l)"
done
```

Per project:

```bash
for s in 'Backlog' 'In progress' 'In review' 'Change requested' 'Done'; do
  printf '%-16s %s\n' "$s" "$(grep -RIl --include='*.md' "^- \*\*Status:\*\* $s$" "./$PROJECT/features"/*/tasks/*.md | wc -l)"
done
```

Per feature:

```bash
for s in 'Backlog' 'In progress' 'In review' 'Change requested' 'Done'; do
  printf '%-16s %s\n' "$s" "$(grep -RIl --include='*.md' "^- \*\*Status:\*\* $s$" "./$PROJECT/features/$FEATURE/tasks"/*.md | wc -l)"
done
```
