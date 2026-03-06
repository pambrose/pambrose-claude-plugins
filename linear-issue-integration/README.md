# linear-issue-integration

Find Linear issues, fix them autonomously in isolated worktrees, and request Codex CLI reviews.

## Commands

| Command | Description |
|---------|-------------|
| `/show-linear-issues` | Display all Linear issues in "Todo" status sorted by priority |
| `/fix-linear-issue` | Find a Linear issue and fix it in an isolated worktree |
| `/cancel-linear-issue` | Cancel one or more Linear issues by setting their status to Cancelled |
| `/codex-review` | Ask Codex CLI to independently review a Linear issue and post its feedback as a comment |
| `/linear-plugin-version` | Show the installed plugin version |

## Skills

| Skill | Description |
|-------|-------------|
| `create-linear-bug-issues` | Review codebase for functional bugs and file findings as Linear issues |
| `create-linear-design-issues` | Review codebase for design and architecture problems and file as Linear issues |
| `create-linear-idiom-issues` | Review codebase for non-idiomatic language usage and file as Linear issues |

## Agents

| Agent | Description |
|-------|-------------|
| `issue-fixer` | Autonomous agent that fixes Linear issues in isolated worktrees |

## Usage Examples

```
/show-linear-issues                  # List all Todo issues by priority
/fix-linear-issue EO-42              # Fix issue EO-42 in a worktree
/fix-linear-issue login timeout      # Search for an issue by text
/cancel-linear-issue EO-42 EO-43     # Cancel multiple issues
/codex-review EO-42                  # Get Codex CLI to review an issue
/linear-plugin-version               # Show plugin version
```

## Requirements

- Linear MCP server configured in Claude Code
- For `/codex-review`: Codex CLI installed (`npm install -g @openai/codex`)
- For `/fix-linear-issue`: Git (uses worktrees for isolation)
