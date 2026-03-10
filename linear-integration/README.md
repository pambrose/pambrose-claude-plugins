# linear-integration

Discover codebase issues, triage by priority, review with Codex CLI, fix autonomously in isolated worktrees, create PRs, and cancel issues — all integrated with Linear.

## Discover Issues

### Skill: `create-linear-bug-issues`

Review the codebase for functional bugs and file each finding as a Linear issue.

```
Find bugs in this codebase and create Linear issues for them
Review for race conditions and resource leaks
Audit error handling in the API layer
```

Looks for: logic errors, race conditions, resource leaks, null safety issues, error handling gaps, incorrect
state management.

### Skill: `create-linear-design-issues`

Review the codebase for design and architecture problems and file each finding as a Linear issue.

```
Review the design of this codebase and create Linear issues
Find tight coupling between modules
Audit separation of concerns
```

Looks for: poor separation of concerns, tight coupling, missing abstractions, god classes, inappropriate
responsibility assignment, poor error propagation, concurrency model issues.

### Skill: `create-linear-idiom-issues`

Review the codebase for non-idiomatic language usage and file each finding as a Linear issue. Automatically detects
the project language.

```
Check for non-idiomatic code and create Linear issues
Find patterns that should use language-specific features
Review for verbose code that could be more concise
```

Looks for: code written in the style of a different language, missing use of language-specific features,
imperative code that could be functional/declarative, verbose patterns where concise alternatives exist, underuse of
the type system, mutable state where immutable is idiomatic.

**How the review skills work:**

All three skills follow the same workflow:

1. **Gather Linear context** — fetch teams, projects, and users; ask which to use
2. **Ensure the label exists** — creates `bug`, `design`, or `non-idiomatic` if missing
3. **Dispatch a code explorer agent** for deep codebase analysis
4. **Present findings** in a summary table for review
5. **File approved findings** as Linear issues with structured descriptions, code snippets, and suggested fixes

## Triage Issues

### Command: `/show-linear-issues`

Display all Linear issues in "Todo" status for a selected project, sorted by priority.

```
/show-linear-issues
```

## Review Issues

### Command: `/codex-review`

Ask Codex CLI to independently review a Linear issue and post its feedback as a comment.

```
/codex-review EO-42              # Review a specific issue by ID
/codex-review login timeout bug  # Search for an issue by text
```

## Fix Issues

### Command: `/fix-linear-issue`

Find a Linear issue and fix it in an isolated worktree. After the agent finishes, presents options to merge, create
a PR, or clean up.

```
/fix-linear-issue EO-42              # Fix a specific issue by ID
/fix-linear-issue login timeout      # Search for an issue by text
```

### Command: `/fix-and-create-pr-linear-issue`

Fix a Linear issue in a worktree, create tests, run `/simplify`, commit, push, and open a GitHub PR — fully
autonomous end-to-end.

```
/fix-and-create-pr-linear-issue EO-42              # Fix a specific issue and create a PR
/fix-and-create-pr-linear-issue login timeout bug  # Search for an issue by text
```

**What it does:**

1. Finds the issue by ID or text search
2. Immediately sets the issue to "In Progress"
3. Confirms with you, then spawns the `issue-pr-creator` agent in an isolated worktree
4. The agent creates a `fix/<id>-<title>` branch, implements the fix, creates tests, runs `/simplify`, commits, pushes,
   and opens a GitHub PR
5. Posts a detailed summary and PR link to the Linear issue
6. Sets the issue to "Done" and cleans up the worktree

**Batch processing with `/loop`:**

Mark issues as "Ready for PR" in Linear, then set up a loop to process them autonomously:

```
/loop 5m Run /show-linear-issues and find any issues in "Ready for PR" status. For each one, run /fix-and-create-pr-linear-issue on it.
```

Issues in "Ready for PR" are immediately moved to "In Progress" (preventing duplicate processing on the next loop
tick), skip the confirmation prompt, and run fully autonomously — fixing each issue, creating PRs, and marking them
"Done" without manual intervention.

## Cancel Issues

### Command: `/cancel-linear-issue`

Cancel one or more Linear issues by setting their status to Cancelled.

```
/cancel-linear-issue EO-42                # Cancel a single issue
/cancel-linear-issue EO-42 EO-43 EO-44   # Cancel multiple issues at once
```

## Utility

### Command: `/linear-plugin-version`

Show the installed version of the plugin.

```
/linear-plugin-version
```

## Agents

| Agent | Used by | Description |
|-------|---------|-------------|
| `issue-fixer` | `/fix-linear-issue` | Fixes issues in isolated worktrees, runs tests, commits |
| `issue-pr-creator` | `/fix-and-create-pr-linear-issue` | Fixes issues, creates tests, runs simplify, opens GitHub PRs |

## Requirements

- Linear MCP server configured in Claude Code
- For `/codex-review`: Codex CLI installed (`npm install -g @openai/codex`)
- For `/fix-linear-issue` and `/fix-and-create-pr-linear-issue`: Git (uses worktrees for isolation)
- For `/fix-and-create-pr-linear-issue`: GitHub CLI (`gh`) for PR creation

> **Warning:** To change Linear Workspaces, you must re-authenticate with the Linear MCP server and select
> the desired Linear Workspace during the authorization process.
