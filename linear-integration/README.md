# linear-integration

Find Linear issues, fix them autonomously in isolated worktrees, and request Codex CLI reviews.

## Commands

| Command | Description |
|---------|-------------|
| `/show-linear-issues` | Display all Linear issues in "Todo" status sorted by priority |
| `/fix-linear-issue` | Find a Linear issue and fix it in an isolated worktree |
| `/fix-and-create-pr-linear-issue` | Fix a Linear issue in a worktree, create tests, simplify, commit, and open a GitHub PR |
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
| `issue-pr-creator` | Autonomous agent that fixes issues, creates tests, runs simplify, and opens GitHub PRs |

## Usage Examples

```
/show-linear-issues                  # List all Todo issues by priority
/fix-linear-issue EO-42              # Fix issue EO-42 in a worktree
/fix-linear-issue login timeout      # Search for an issue by text
/fix-and-create-pr-linear-issue EO-42  # Fix issue, create tests, simplify, and open a PR
/cancel-linear-issue EO-42 EO-43     # Cancel multiple issues
/codex-review EO-42                  # Get Codex CLI to review an issue
/linear-plugin-version               # Show plugin version
```

### Fix and Create PR — Detailed Examples

**Fix a specific issue by ID:**

```
/fix-and-create-pr-linear-issue EO-42
```

This finds issue EO-42, immediately sets it to "In Progress", confirms with you, then autonomously: creates a
`fix/eo-42-fix-login-timeout` branch in a worktree, implements the fix, creates tests, runs `/simplify`, commits,
pushes, opens a GitHub PR, posts a summary and PR link to the Linear issue, and sets the issue to "Done".

**Search for an issue by text:**

```
/fix-and-create-pr-linear-issue login timeout bug
```

This searches for matching issues, lets you pick one, then runs the same autonomous workflow.

**Batch-process issues with `/loop`:**

First, mark the issues you want processed as "Ready for PR" in Linear. Then set up a loop:

```
/loop 5m Run /show-linear-issues and find any issues in "Ready for PR" status. For each one, run /fix-and-create-pr-linear-issue on it.
```

Issues in "Ready for PR" are immediately moved to "In Progress" (preventing duplicate processing on the next loop
tick), skip the confirmation prompt, and run fully autonomously — fixing each issue, creating PRs, and marking them
"Done" without manual intervention.

## Requirements

- Linear MCP server configured in Claude Code
- For `/codex-review`: Codex CLI installed (`npm install -g @openai/codex`)
- For `/fix-linear-issue` and `/fix-and-create-pr-linear-issue`: Git (uses worktrees for isolation)
- For `/fix-and-create-pr-linear-issue`: GitHub CLI (`gh`) for PR creation

> **Warning:** To change Linear Workspaces, you must re-authenticate with the Linear MCP server and select
> the desired Linear Workspace during the authorization process.
