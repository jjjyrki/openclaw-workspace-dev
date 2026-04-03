---
name: cursor
description: Use this skill when delegating implementation work to Cursor. Prefer the shared Python helper instead of constructing raw Cursor CLI commands manually.
---

# Cursor

Use this skill when you want Cursor to implement, update, refactor, or test code in a repository.

## Rule

Do not construct raw `agent` CLI commands yourself unless there is a very specific reason to bypass the shared helper.

Prefer the Python helper:

```python
from skills.tools.cursor import run_cursor
```

## Default usage

Most tasks should use this pattern:

```python
from skills.tools.cursor import run_cursor

result = run_cursor(
    prompt="Fix failing auth tests",
    repo_path="/workspace/my-repo",
    context_files=[
        "/workspace/my-repo/src/auth.ts",
        "/workspace/my-repo/tests/auth.test.ts",
    ],
)
```

## What the helper does

The helper handles:

- running Cursor via `agent`
- non-interactive execution
- model selection
- workspace trust
- auto-approval
- prompt formatting
- `@file` context formatting
- repo path validation
- context file validation

This means you should focus on:
- writing a clear prompt
- selecting the right repo
- passing the most relevant files as context

## Common use case

Use `run_cursor()` when you need Cursor to work on code with some relevant files included as context.

Example:

```python
from skills.tools.cursor import run_cursor

result = run_cursor(
    prompt="Implement token refresh and update tests.",
    repo_path="/workspace/my-repo",
    context_files=[
        "/workspace/my-repo/src/auth.ts",
        "/workspace/my-repo/tests/auth.test.ts",
    ],
)
```

## Choosing context files

Include only the files that are most relevant.

### Implementing a change
Include:
- the main source file
- related tests
- important interface or type files if needed

### Fixing a bug
Include:
- the buggy source file
- the failing test
- nearby related code if necessary

### Adding tests
Include:
- the source file under test
- existing relevant test files

Do not include large numbers of unrelated files.

## Prompt guidance

Write prompts that are:
- specific
- task-oriented
- scoped to the requested change

Good examples:
- `Fix failing auth tests`
- `Implement token refresh and update tests`
- `Refactor the payment service to reduce duplication without changing behavior`
- `Add tests for the invoice parser edge cases`

Better examples:
- `Fix the failing auth tests in the provided files. Preserve existing behavior unless a bug requires changing it. Update tests as needed.`
- `Implement token refresh in the auth flow and update the related tests. Keep the change minimal and avoid unrelated edits.`

## Public API

Use:

```python
run_cursor(
    *,
    prompt: str,
    repo_path: str,
    context_files: list[str] | None = None,
    model: str = "auto",
    trust_workspace: bool = True,
    auto_approve: bool = True,
    timeout_seconds: int = 1800,
    extra_instructions: str | None = None,
)
```

In most cases, only these are needed:
- `prompt`
- `repo_path`
- `context_files`

## Recommended pattern

Prefer this:

```python
from skills.tools.cursor import run_cursor

result = run_cursor(
    prompt="Implement the fix.",
    repo_path="/workspace/my-repo",
    context_files=[
        "/workspace/my-repo/src/file.ts",
        "/workspace/my-repo/tests/file.test.ts",
    ],
)
```

## Avoid

Avoid:
- calling `agent` directly
- manually building Cursor flags
- manually formatting `@file` prompt blocks
- passing unrelated files as context
- giving vague prompts like `fix this`

## Environment assumptions

This skill assumes:
- `CURSOR_API_KEY` is set
- Cursor CLI `agent` is available in `PATH`
- `repo_path` is an absolute path
- context files are absolute paths

## Result handling

`run_cursor()` returns a structured result.

Typical handling:

```python
from skills.tools.cursor import run_cursor

result = run_cursor(
    prompt="Fix failing auth tests",
    repo_path="/workspace/my-repo",
    context_files=[
        "/workspace/my-repo/src/auth.ts",
        "/workspace/my-repo/tests/auth.test.ts",
    ],
)

if not result.success:
    print(result.stderr)
else:
    print(result.stdout)
```

## Summary

When using Cursor:
1. Import `run_cursor` from `skills.tools.cursor`
2. Pass a clear prompt
3. Pass the repo path
4. Pass only the most relevant files as context
5. Let the helper handle the command details

