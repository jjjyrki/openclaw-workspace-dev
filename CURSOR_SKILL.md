This file explains how to use cursor agent for various software engineering tasks.

### Setup api token
Before starting, set up the api token so that cursor cli will detect it.

```bash
export CURSOR_API_KEY=$CURSOR
```

### Update
Keep your CLI up to date:

```bash
agent update
```

### Session Management

Manage your agent sessions:

- **List sessions:** `agent ls`
- **Resume most recent:** `agent resume`
- **Resume specific session:** `agent --resume="[chat-id]"`

### Context Selection

Include specific files or folders in the conversation:

```
@filename.ts
@src/components/
```
### Non-interactive / CI Mode

Run the agent in a non-interactive mode, suitable for CI/CD pipelines:

```bash
agent -p 'Run tests and report coverage'
# or
agent --print 'Refactor this file to use async/await'
```

**Output formats:**

```bash
# Plain text (default)
agent -p 'Analyze code' --output-format text

# Structured JSON
agent -p 'Find bugs' --output-format json

# Real-time streaming JSON
agent -p 'Run tests' --output-format stream-json --stream-partial-output
```

**Force mode (auto-apply changes without confirmation):**

```bash
agent -p 'Fix all linting errors' --force
```

**Media support:**

```bash
agent -p 'Analyze this screenshot: screenshot.png'
```

### ⚠️ Using with AI Agents / Automation (tmux required)

**CRITICAL:** When running Cursor CLI from automated environments (AI agents, scripts, subprocess calls), the CLI requires a real TTY. Direct execution will hang indefinitely.

**The Solution: Use tmux**

```bash
# 1. Install tmux if not available
sudo apt install tmux  # Ubuntu/Debian
brew install tmux      # macOS

# 2. Create a tmux session
tmux kill-session -t cursor 2>/dev/null || true
tmux new-session -d -s cursor

# 3. Navigate to project
tmux send-keys -t cursor "cd /path/to/project" Enter
sleep 1

# 4. Run Cursor agent
tmux send-keys -t cursor "agent 'Your task here'" Enter

# 5. Handle workspace trust prompt (first run)
sleep 3
tmux send-keys -t cursor "a"  # Trust workspace

# 6. Wait for completion
sleep 60  # Adjust based on task complexity

# 7. Capture output
tmux capture-pane -t cursor -p -S -100

# 8. Verify results
ls -la /path/to/project/
```

**Why this works:**
- tmux provides a persistent pseudo-terminal (PTY)
- Cursor's TUI requires interactive terminal capabilities
- Direct `agent` calls from subprocess/exec hang without TTY

**What does NOT work:**
```bash
# ❌ These will hang indefinitely:
agent "task"                    # No TTY
agent -p "task"                 # No TTY  
subprocess.run(["agent", ...])  # No TTY
script -c "agent ..." /dev/null # May crash Cursor
```

## Rules & Configuration

The agent automatically loads rules from:
- `.cursor/rules`
- `AGENTS.md`
- `CLAUDE.md`

Use `/rules` command to create and edit rules directly from the CLI.

## Workflows

### Code Review

Perform a code review on the current changes or a specific branch:

```bash
agent -p 'Review the changes in the current branch against main. Focus on security and performance.'
```

### Refactoring

Refactor code for better readability or performance:

```bash
agent -p 'Refactor src/utils.ts to reduce complexity and improve type safety.'
```

### Debugging

Analyze logs or error messages to find the root cause:

```bash
agent -p 'Analyze the following error log and suggest a fix: [paste log here]'
```



